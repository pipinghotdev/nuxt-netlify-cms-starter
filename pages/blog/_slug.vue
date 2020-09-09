<template>
  <section class="post">
    <Container class="meta-section">
      <h1>{{ title }}</h1>
      <p v-if="published === updated" class="post-meta">
        Posted on {{ published }} by
        <a v-if="authorlink" :href="authorlink">{{ author }}</a>
      </p>
      <p v-else class="post-meta">
        Updated on {{ updated }} by
        <a v-if="authorlink" :href="authorlink">{{ author }}</a>
      </p>
    </Container>
    <Container narrow>
      <img v-lazy="thumbnail" class="thumbnail" :alt="title" />
      <div id="shareable" class="post-content" v-html="html"></div>
    </Container>
  </section>
</template>

<script>
import Prism from 'prismjs'
import shareThis from 'share-this'
import * as twitterSharer from 'share-this/dist/sharers/twitter'
import * as facebookSharer from 'share-this/dist/sharers/facebook'
import * as redditSharer from 'share-this/dist/sharers/reddit'

import Container from '~/components/Container'

export default {
  components: {
    Container,
  },
  head() {
    return {
      title: `${this.title} | <Blog Name>`,
      meta: [
        {
          hid: 'article:published_time',
          property: 'article:published_time',
          content: this.date,
        },
        {
          hid: 'article:modified_time',
          property: 'article:modified_time',
          content: this.update,
        },
        {
          hid: 'og:updated_time',
          property: 'og:updated_time',
          content: this.update,
        },
        {
          hid: 'og:url',
          property: 'og:url',
          content: `https://<domain>/blog/${this.slug}`,
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: `${this.title} | Code Tribe`,
        },
        {
          hid: 'description',
          name: 'description',
          content: this.summary,
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.summary,
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: `https://<domain>${this.thumbnail}`,
        },
        {
          hid: 'og:image:alt',
          property: 'og:image:alt',
          content: this.title,
        },
        {
          hid: 'og:type',
          property: 'og:type',
          content: 'article',
        },
        {
          hid: 'og:article:author',
          property: 'og:article:author',
          content: this.authorlink,
        },
        {
          hid: 'twitter:card',
          name: 'twitter:card',
          content: 'summary_large_image',
        },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: this.title,
        },
        {
          hid: 'twitter:site',
          name: 'twitter:site',
          content: '@tribe_code',
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.summary,
        },
        {
          hid: 'twitter:image',
          name: 'twitter:image',
          content: `https://code-tribe.com${this.thumbnail}`,
        },
      ],
    }
  },
  async asyncData({ params }) {
    const post = await import(`~/content/blog/${params.slug}.md`)
    const attr = post.attributes
    const slug = params.slug

    const {
      author,
      authorlink,
      date,
      summary,
      thumbnail,
      title,
      type,
      update,
    } = attr

    const dateOptions = {
      weekday: 'long',
      year: 'numeric',
      month: 'short',
      day: 'numeric',
    }

    const publishedDate = new Date(date)
    const updatedDate = new Date(update)
    const published = publishedDate.toLocaleDateString('en-GB', dateOptions)
    const updated = updatedDate.toLocaleDateString('en-GB', dateOptions)

    return {
      title,
      author,
      authorlink,
      date,
      update,
      published,
      updated,
      type,
      thumbnail,
      summary,
      slug,
      html: post.html,
    }
  },
  mounted() {
    Prism.highlightAll()

    const selectionShare = shareThis({
      selector: '#shareable',
      sharers: [twitterSharer, facebookSharer, redditSharer],
    })

    selectionShare.init()
  },
}
</script>

<style lang="scss">
.share-this-popover {
  animation: share-this 360ms forwards linear;

  &::before {
    border-color: #252525 transparent;
    border-style: solid;
    border-width: 0.4em 0.4em 0;
    bottom: 100%;
    content: '';
    height: 0;
    left: 50%;
    transform: translateX(-50%);
    margin: 0;
    position: absolute;
    width: 0;
  }

  ul {
    background-color: #252525;
    border-radius: 0.5rem;
    transform: translate(-50%, -110%);
    background: linear-gradient(to bottom, rgba(49, 49, 47, 0.99), #252525);
    padding: 0;
    margin: 0;
    display: inline-block;
    color: #fff;
    left: 50%;
    display: flex;
    list-style: none;
    position: absolute;
    white-space: nowrap;
    padding: 0.5rem 0.7rem;

    li {
      display: inline-block;
      height: 1.2rem;
      line-height: 1.2rem;
      text-align: center;
      margin: 0;
      padding: 0 0.4rem;
      width: 2rem;

      &:first-child {
        padding-left: 0;
        width: 1.6rem;
      }
      &:last-child {
        padding-right: 0;
        width: 1.6rem;
      }

      a {
        color: #fff;
        box-shadow: none;
        display: block;
      }
    }
  }
}

.post {
  .meta-section,
  .thumbnail {
    margin-bottom: 2.4rem;
  }

  .thumbnail {
    width: 100%;
    height: auto;
  }

  .meta-section {
    text-align: center;
    display: block;

    .post-meta {
      margin: 0;
      color: #535353;
      font-weight: 700;
    }
  }
  h1 {
    margin-top: 0;
    margin-bottom: 2rem;
    display: block;
    width: 100%;
  }
}

@keyframes share-this {
  0% {
    opacity: 0;
    -webkit-transform: matrix(0.97, 0, 0, 1, 0, 12);
    transform: matrix(0.97, 0, 0, 1, 0, 12);
  }
  20% {
    opacity: 0.7;
    -webkit-transform: matrix(0.99, 0, 0, 1, 0, 2);
    transform: matrix(0.99, 0, 0, 1, 0, 2);
  }
  40% {
    opacity: 1;
    -webkit-transform: matrix(1, 0, 0, 1, 0, -1);
    transform: matrix(1, 0, 0, 1, 0, -1);
  }
  70% {
    opacity: 1;
    -webkit-transform: matrix(1, 0, 0, 1, 0, 0);
    transform: matrix(1, 0, 0, 1, 0, 0);
  }
  100% {
    opacity: 1;
    -webkit-transform: matrix(1, 0, 0, 1, 0, 0);
    transform: matrix(1, 0, 0, 1, 0, 0);
  }
}
</style>

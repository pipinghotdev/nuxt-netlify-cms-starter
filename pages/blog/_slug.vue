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
      <div class="post-content" v-html="html"></div>
    </Container>
  </section>
</template>

<script>
import Container from '~/components/Container'

import Prism from 'prismjs'

export default {
  components: {
    Container
  },
  head() {
    return {
      title: `${this.title} | <Blog Name>`,
      meta: [
        {
          hid: 'article:published_time',
          property: 'article:published_time',
          content: this.date
        },
        {
          hid: 'article:modified_time',
          property: 'article:modified_time',
          content: this.update
        },
        {
          hid: 'og:updated_time',
          property: 'og:updated_time',
          content: this.update
        },
        {
          hid: 'og:url',
          property: 'og:url',
          content: `https://<domain>/blog/${this.slug}`
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: `${this.title} | Code Tribe`
        },
        {
          hid: 'description',
          name: 'description',
          content: this.summary
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.summary
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: `https://<domain>${this.thumbnail}`
        },
        {
          hid: 'og:image:alt',
          property: 'og:image:alt',
          content: this.title
        },
        {
          hid: 'og:type',
          property: 'og:type',
          content: 'article'
        },
        {
          hid: 'og:article:author',
          property: 'og:article:author',
          content: this.authorlink
        },
        {
          hid: 'twitter:card',
          name: 'twitter:card',
          content: 'summary_large_image'
        },
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: this.title
        },
        {
          hid: 'twitter:site',
          name: 'twitter:site',
          content: '@tribe_code'
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.summary
        },
        {
          hid: 'twitter:image',
          name: 'twitter:image',
          content: `https://code-tribe.com${this.thumbnail}`
        }
      ]
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
      update
    } = attr

    const dateOptions = {
      weekday: 'long',
      year: 'numeric',
      month: 'short',
      day: 'numeric'
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
      html: post.html
    }
  },
  mounted() {
    Prism.highlightAll()
  }
}
</script>

<style lang="scss" scoped>
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
</style>

<template>
  <section class="post">
    <Container class="meta-section">
      <h1>{{ title }}</h1>
      <p v-if="date === update" class="post-meta">
        Posted on {{ date }}, by
        <a v-if="authorlink" :href="authorlink">{{ author }}</a>
      </p>
      <p v-else class="post-meta">
        Updated on {{ update }}, by
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
          hid: 'og:url',
          name: 'og:url',
          content: `https://nuxt-netlify-cms-starter.com/blog/${this.slug}`
        },
        {
          hid: 'og:title',
          property: 'og:title',
          content: `${this.title} | <Blog Name>`
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
          content: `https://nuxt-netlify-cms-starter.com${this.thumbnail}`
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
          content: `https://nuxt-netlify-cms-starter.com${this.thumbnail}`
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

    return {
      title,
      author,
      authorlink,
      date,
      update,
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

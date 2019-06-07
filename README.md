# NuxtJS Netlify CMS Starter

> Netlify CMS starter based on the [Code Tribe](https://code-tribe.com) blog.

## Build Setup

``` bash
# install dependencies
$ yarn install

# serve with hot reload at localhost:3000
$ yarn run dev

# generate blog
$ yarn run generate
```

## Quick Start

1. Clone or fork this repository.
2. Set up your [Netlify](https://netlify.com) account.
3. Sync Netlify with your own repository to automatically deploy when you write a new post.
4. `yarn dev` or `npm run dev` for local development (available at `http://localhost:3000`).
5. Deploy, write some posts and profit!

Access your Netlify CMS admin dashboard at `localhost:3000/admin`. By default, the project assumes you want to use [Netlify Identity](https://www.netlify.com/docs/identity/). If you don't wish to use this, modify the `static/admin/index.html` file.

## Configuration

Netlify CMS configuration is stored in `static/admin/config.yml`. See the [Netlify CMS docs](https://www.netlifycms.org/docs/intro/) for more info.

Blog posts will be stored in the `content/blog` folder.

## Features

Features include:

- Full SEO support for blog posts.
- Lazy-load images.
- robots.txt to disallow admin routes.
- Automatic sitemap generation of your homepage and blog posts.
- Font Awesome imports (see nuxt.config.js to add more).
- Google font loading.
- PrismJS syntax highlighting.
- Medium-like social sharing.
- Page transitions.
- And of course, full page HTML rendering for deployment on Netlify!

## NuxtJS

For detailed explanation on how things work, checkout [Nuxt.js docs](https://nuxtjs.org).

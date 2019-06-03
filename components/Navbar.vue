<template>
  <nav class="navbar" role="navigation">
    <div class="navbar-inner">
      <div class="brand">
        <nuxt-link to="/" class="nav-link">Blog Name</nuxt-link>
      </div>
      <ul class="social-icons">
        <li class="icon">
          <a href="#" target="_blank" class="nav-link">
            <font-awesome-icon :icon="['fab', 'twitter']" />
          </a>
        </li>
      </ul>
    </div>
  </nav>
</template>

<style lang="scss" scoped>
.navbar {
  width: 100%;
  font-family: 'Rubik', sans-serif;

  .navbar-inner {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    max-width: 1200px;
    margin: 0 auto;
    align-items: center;

    .brand {
      display: block;
      font-weight: 700;
      text-align: left;

      img {
        height: 24px;
        display: block;
      }

      a {
        padding: 1rem;
        display: block;
        box-shadow: none;
      }
    }

    .social-icons {
      padding: 0;
      margin: 0;
      list-style-type: none;
      line-height: 1;

      li {
        display: inline;

        a {
          display: inline-block;
          padding: 1.1rem 1rem 1rem 1rem;
          box-shadow: none;
        }
      }
    }
  }
}
</style>

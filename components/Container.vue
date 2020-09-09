<template>
  <div class="container" :class="{ narrow, flex }">
    <slot />
  </div>
</template>

<script>
export default {
  props: {
    narrow: {
      type: Boolean,
      default: false,
    },
    flex: {
      type: Boolean,
      default: false,
    },
  },
}
</script>

<style lang="scss">
.container {
  display: block;
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 1rem;

  &.flex {
    display: flex;
    flex-direction: row;
    justify-content: space-between;
    flex-wrap: wrap;

    @media (max-width: 567px) {
      display: block;
    }
  }

  &.narrow {
    max-width: 760px;
  }
}
</style>

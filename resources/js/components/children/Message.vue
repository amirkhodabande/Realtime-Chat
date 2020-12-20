<template>
      <div>
            <li
                  class="list-group-item "
                  :class="className"
            >
                  <slot name="message"></slot>
            </li>
            <small
                  class="badge float-right"
                  :class="badgeClassName"
            >
                  <slot name="user"></slot>
            </small>
      </div>
</template>

<script>
export default {
      data() {
            return {
                  colors: this.$slots,
            };
      },

      computed: {
            className() {
                  return "list-group-item-" + this.colors.color[0].text;
            },
            badgeClassName() {
                  return "badge-" + this.colors.color[0].text;
            },
      },
};
</script>

<style>
</style>

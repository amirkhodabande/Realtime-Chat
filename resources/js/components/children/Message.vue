<template>
      <div>
            <li
                  class="list-group-item "
                  :class="className"
            >
                  <slot></slot>
            </li>
            <small
                  class="badge float-right"
                  :class="badgeClassName"
            >You</small>
      </div>
</template>

<script>
export default {
      props: ["message", "color"],
      computed: {
            className() {
                  return "list-group-item-" + this.color;
            },
            badgeClassName() {
                  return "badge-" + this.color;
            },
      },
};
</script>

<style>
</style>

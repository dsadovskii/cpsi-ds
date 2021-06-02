<template>
  <section class="pt-tabs">
    <a-tabs v-model="currentTab" :default-active-key="defaultActiveKey">
      <slot />
    </a-tabs>
  </section>
</template>

<script>
export default {
  name: 'PtTabs',
  props: {
    defaultActiveKey: {
      type: [String, Number],
      default: '1',
      required: true,
    },
    value: {
      type: [String, Number],
    },
  },
  computed: {
    currentTab: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      },
    },
  },
}
</script>

<style lang="scss">
.pt-tabs {
  $block-name: &;
  .ant-tabs {
    &-nav {
    }
    &-tab {
      font-size: $fs-14;
      line-height: $lh-14;
      color: $color-black;
      &:hover {
        color: $color-blue;
      }
      &-active {
        font-weight: $fw-semi-bold;
        color: $color-blue;
      }
    }
    &-ink-bar-animated {
      background-color: $color-blue;
    }
    &-top-bar {
      border-color: $color-gray-3;
    }
  }
}
</style>

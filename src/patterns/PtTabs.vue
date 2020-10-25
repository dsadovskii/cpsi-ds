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
      type: String,
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
      font-size: $fs-16;
      line-height: $lh-13;
      color: $color-blue;
      &:hover {
        color: $color-blue;
      }
      &-active {
        font-weight: $fw-semi-bold;
      }
    }
    &-ink-bar-animated {
      background-color: $color-blue;
    }
    &-top-bar {
      border-color: $color-light-blue;
    }
  }
}
</style>

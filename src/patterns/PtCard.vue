<script>
export default {
  name: 'PtCard',
  functional: true,
  props: {
    to: {
      type: String,
      default: null,
    },
    nuxt: {
      type: Boolean,
      default: true,
    },
    noShadow: {
      type: Boolean,
      default: false,
    },
    noPadding: {
      type: Boolean,
      default: false,
    },
    noBorder: {
      type: Boolean,
      default: false,
    },
    noBg: {
      type: Boolean,
      default: false,
    },
  },
  render(h, { slots, props, data }) {
    const attrs = props.to ? { to: props.to } : {}
    return h(
      props.to ? (props.nuxt ? 'nuxt-link' : 'router-link') : 'article',
      {
        class: {
          'pt-card': true,
          'pt-card--no-shadow': props.noShadow,
          'pt-card--no-border': props.noBorder,
          'pt-card--no-padding': props.noPadding,
          'pt-card--no-bg': props.noBg,
          [`${data.staticClass}`]: !!data.staticClass,
          ...data.class,
        },
        attrs: attrs,
      },
      [
        !(slots()['card-header'] || slots()['card-header-left'] || slots()['card-header-right']) ||
          h('section', { class: 'pt-card__header' }, [
            !slots()['card-header-left'] || h('div', { class: 'pt-card__header-left' }, slots()['card-header-left']),
            h('div', { class: 'pt-card__header-center' }, slots()['card-header']),
            !slots()['card-header-right'] || h('div', { class: 'pt-card__header-right' }, slots()['card-header-right']),
          ]),
        !slots()['default'] || h('section', { class: 'pt-card__content' }, slots()['default']),
        !(slots()['card-footer'] || slots()['card-footer-left'] || slots()['card-footer-right']) ||
          h('section', { class: 'pt-card__footer' }, [
            !slots()['card-footer-left'] || h('div', { class: 'pt-card__footer-left' }, slots()['card-footer-left']),
            h('div', { class: 'pt-card__footer-center' }, slots()['card-footer']),
            !slots()['card-footer-right'] || h('div', { class: 'pt-card__footer-right' }, slots()['card-footer-right']),
          ]),
      ],
    )
  },
}
</script>

<style lang="scss">
.pt-card {
  $block-name: &;
  width: 100%;
  height: 100%;
  display: flex;
  flex-direction: column;
  align-items: flex-start;
  box-shadow: $shadow-card;
  border-radius: $radius-3;
  background-color: $color-white;
  transition: box-shadow 0.5s ease;
  &__header,
  &__content,
  &__footer {
    padding: $space-24;
  }
  &__header,
  &__footer {
    display: flex;
    align-items: center;
    justify-content: space-between;
    width: 100%;
    border-bottom: 1px solid $color-light-blue;
  }
  &__content {
    height: 100%;
    width: 100%;
  }
  &__footer {
    margin-top: auto;
  }
  &--no {
    &-shadow {
      box-shadow: none;
    }
    &-border {
      #{$block-name}__header {
        border-bottom: none;
      }
    }
    &-padding {
      #{$block-name}__header,
      #{$block-name}__content {
        padding: $space-zero;
      }
    }
    &-bg {
      background-color: $color-transparent;
    }
  }
}
</style>

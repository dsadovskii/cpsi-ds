<script>
export default {
  name: 'ElBadge',
  functional: true,
  props: {
    variant: {
      type: String,
      default: 'blue',
    },
    color: {
      type: String,
      default: 'white',
    },
    text: {
      type: String,
      default: null,
    },
    uppercase: {
      type: Boolean,
      default: false,
    },
  },
  render(h, { slots, props, data }) {
    return h(
      'div',
      {
        class: {
          'el-badge': true,
          'el-badge--uppercase': props.uppercase,
          [`el-badge--${props.variant}`]: !!props.variant,
          [`el-badge__color--${props.color}`]: !!props.color,
          [`${data.staticClass}`]: !!data.staticClass,
          [data.class]: !!data.class,
        },
      },
      [h('span', { class: 'el-badge__content' }, [props.text || slots()['default']])],
    )
  },
}
</script>

<style lang="scss">
.el-badge {
  $block-name: &;
  display: inline-flex;
  padding: $space-8 $space-16;
  border-radius: $radius-4;
  &__content {
    font-size: $fs-12;
    line-height: $lh-14;
  }
  &--uppercase {
    #{$block-name}__content {
      text-transform: uppercase;
    }
  }
  @each $name, $color in $backgrounds {
    &--#{$name} {
      background-color: #{$color};
    }
  }
  @each $name, $color in $colors {
    &__color--#{$name} {
      #{$block-name}__content {
        color: #{$color};
      }
    }
  }
}
</style>

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
    closable: {
      type: Boolean,
      default: false,
    },
  },
  render(h, { slots, props, data, listeners }) {
    return h(
      'div',
      {
        class: {
          'el-badge': true,
          'el-badge--uppercase': props.uppercase,
          [`el-badge--${props.variant}`]: !!props.variant,
          [`el-badge__color--${props.color}`]: !!props.color,
          'el-badge__closable': props.closable,
          [`${data.staticClass}`]: !!data.staticClass,
          [data.class]: !!data.class,
        },
      },
      [
        h('span', { class: 'el-badge__content' }, [
          props.text || slots()['default'],
          !props.closable ||
            h(
              'span',
              {
                class: 'el-badge__close',
                on: {
                  click(e) {
                    e.preventDefault()
                    e.stopPropagation()
                    if (!Object.keys(listeners).length || props.disabled) return
                    listeners['click']()
                  },
                },
              },
              '+',
            ),
        ]),
      ],
    )
  },
}
</script>

<style lang="scss">
.el-badge {
  $block-name: &;
  display: inline-flex;
  position: relative;
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
  &__closable {
    padding-right: $space-28;
  }
  &__close {
    display: inline-block;
    cursor: pointer;
    transform-origin: center;
    transform: rotate(45deg) translate(7px, 3px);
    position: absolute;
    right: 10px;
    font-size: 20px;
    line-height: 0;
    @media #{$desktop} {
      &:hover {
        color: $color-red;
      }
    }
    @media #{$mobile} {
      &:active {
        color: $color-red;
      }
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

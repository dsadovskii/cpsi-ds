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
      default: null,
    },
    countVariant: {
      type: String,
      default: null,
    },
    countColor: {
      type: String,
      default: null,
    },
    count: {
      type: [String, Number],
      default: null,
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
    clickable: {
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
          [`el-badge--color-${props.color}`]: !!props.color,
          'el-badge__closable': props.closable,
          'el-badge__clickable': props.clickable,
          [`${data.staticClass}`]: !!data.staticClass,
          [data.class]: !!data.class,
        },
        on: {
          click(e) {
            e.preventDefault()
            e.stopPropagation()
            if (!Object.keys(listeners).length) return
            listeners['click']()
          },
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
                    if (!Object.keys(listeners).length || !listeners['close']) return
                    listeners['close']()
                  },
                },
              },
              '+',
            ),
        ]),
        !props.count ||
          h(
            'span',
            {
              class: {
                'el-badge__count': true,
                [`el-badge__count--${props.countVariant}`]: !!props.countVariant,
                [`el-badge__count--color-${props.countColor}`]: !!props.countColor,
              },
            },
            props.count,
          ),
      ],
    )
  },
}
</script>

<style lang="scss">
.el-badge {
  $block-name: &;
  display: inline-flex;
  align-items: center;
  position: relative;
  padding: $space-8 $space-16;
  border-radius: $radius-4;
  height: 40px;
  &__content {
    font-size: $fs-14;
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
  &__clickable {
    cursor: pointer;
  }
  &__clickable {
    cursor: pointer;
  }
  &__count {
    font-size: $fs-14;
    line-height: $lh-14;
    padding: 1px $space-14;
    border-radius: $radius-4;
    margin-left: $space-16;
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
  @each $name, $color in $colors {
    &--#{$name} {
      background-color: #{$color};
      color: $color-blue;
      &-outline {
        border: 1px solid #{$color};
        background-color: transparent;
        color: #{$color};
      }
    }
    &--color-#{$name} {
      color: #{$color} !important;
    }
    &__count--#{$name} {
      background-color: #{$color};
      &-outline {
        border: 1px solid #{$color};
        background-color: transparent;
        color: #{$color};
      }
    }
    &__count--color-#{$name} {
      color: #{$color};
    }
  }
}
</style>

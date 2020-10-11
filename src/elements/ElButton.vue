<script>
export default {
  name: 'ElButton',
  functional: true,
  props: {
    variant: {
      type: String,
      default: 'blue',
    },
    loading: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    fullWidth: {
      type: Boolean,
      default: false,
    },
    size: {
      type: String,
      default: 's',
    },
    secondaryColor: {
      type: String,
      default: '',
    },
    minWidth: {
      type: String,
      default: null,
    },
    bgFlat: {
      type: Boolean,
      default: false,
    },
  },
  render(h, { props, slots, listeners, data }) {
    let prepend = slots()['icon-prepend']
    let append = slots()['icon-append']
    let content = [
      !prepend || h('span', { class: 'el-button__icon-prepend' }, prepend),
      !slots()['default'] || h('span', { class: 'el-button__content' }, slots()['default']),
      !append || h('span', { class: 'el-button__icon-append' }, append),
    ]
    if (props.loading) {
      content = [
        ...content,
        h('div', { class: 'fancy-spinner' }, [
          ['ring', 'ring', 'dot'].map(el => {
            return h('div', { class: el })
          }),
        ]),
      ]
    }
    return h(
      'button',
      {
        class: {
          'el-button': true,
          [`el-button--${props.variant}`]: !!props.variant,
          [`el-button--secondary-color-${props.secondaryColor}`]: !!props.secondaryColor,
          [`el-button--size-${props.size}`]: !!props.size,
          'el-button--default': !props.variant,
          'el-button--disabled': props.disabled,
          'el-button--loading': props.loading,
          'el-button--full-width': props.fullWidth,
          [`${data.staticClass}`]: !!data.staticClass,
          ...data.class,
        },
        attrs: {
          disabled: props.disabled,
        },
        style: {
          [!props.minWidth || 'min-width']: props.minWidth,
        },
        on: {
          click(e) {
            if (!Object.keys(listeners).length || props.disabled) return
            listeners['click'](e)
          },
        },
        [props.variant && props.variant.match('link') ? '' : 'directives']: [
          {
            name: 'ripple',
          },
        ],
        ref: 'el-button',
      },
      content,
    )
  },
}
</script>

<style lang="scss">
.el-button {
  $block-name: &;
  @include reset();
  position: relative;
  display: inline-flex;
  justify-content: center;
  align-items: center;
  flex-wrap: nowrap;
  max-width: 100%;
  font-weight: $fw-semi-bold;
  border-radius: $radius-4;
  white-space: nowrap;
  user-select: none;
  text-decoration: none;
  text-transform: none;
  background-color: transparent;
  background-image: none;
  cursor: pointer;
  outline: 0;
  border: 1px solid transparent;
  transition: all 0.3s;
  color: $color-black;
  -webkit-appearance: none;
  -webkit-text-fill-color: currentColor;
  &__content {
    font-size: inherit;
    line-height: inherit;
    margin: 0 $space-10;
  }
  /*TODO*/
  &--disabled {
    background-color: #928fa350;
    border-color: #928fa350;
    cursor: not-allowed;
    &:hover,
    &:active {
      background-color: #928fa350;
      border-color: #928fa350;
      transform: none;
    }
  }
  &::after {
    animation: none;
    box-shadow: none;
  }
  @each $name, $color in $colors {
    &--#{$name} {
      background-color: #{$color};
      color: #ffffff;
      @media #{$desktop} {
        &:hover {
          filter: brightness(1.3);
        }
      }
      @media #{$mobile} {
        &:active {
          filter: brightness(1.3);
        }
      }
      &.active {
        filter: brightness(1.3);
      }
    }
    &--#{$name}-link {
      @include reset-btn();
      color: #{$color};
      height: auto !important;
      @media #{$desktop} {
        &:hover {
          background-color: transparent;
          filter: brightness(1.3);
        }
      }
      @media #{$mobile} {
        &:active {
          background-color: transparent;
          filter: brightness(1.3);
        }
      }
      &.active {
        background-color: transparent;
        filter: brightness(1.3);
      }
    }
    &--secondary-color-#{$name} {
      @media #{$desktop} {
        &:hover {
          color: #{$color};
        }
      }
      @media #{$mobile} {
        &:active {
          color: #{$color};
        }
      }
      &.active {
        color: #{$color};
      }
    }
  }
  &--loading {
    #{$block-name} {
      &__content,
      &__icon-prepend,
      &__icon-append {
        opacity: 0;
      }
    }
  }
  &--full-width {
    width: 100%;
  }
  &__icon-append,
  &__icon-prepend {
    display: inline-flex;
    align-items: center;
    user-select: none;
  }
  &--size {
    &-xs {
      height: $size-32;
      padding: $space-8 $space-14;
      letter-spacing: normal;
      font-size: $fs-12;
      line-height: $lh-13;
      &-compact {
        @include size($size-32);
        padding: 6px;
        .mc-svg-icon {
          @include size($size-20);
        }
      }
      .mc-svg-icon {
        @include size($size-20);
      }
      #{$block-name} {
        &__prepend {
          margin-right: $space-4;
        }
        &__append {
          margin-left: $space-4;
        }
      }
    }
    &-s {
      height: $size-40;
      padding: $space-14 $space-16;
      letter-spacing: normal;
      font-size: $fs-12;
      line-height: $lh-13;
      &-compact {
        @include size($size-40);
        padding: $space-14;
        .mc-svg-icon {
          @include size($size-20);
        }
      }
      .mc-svg-icon {
        @include size($size-20);
      }
      #{$block-name} {
        &__prepend {
          margin-right: $space-4;
        }
        &__append {
          margin-left: $space-4;
        }
      }
    }
    &-m {
      height: $size-44;
      padding: $space-8 $space-16;
      font-size: $fs-16;
      line-height: $lh-13;
      &-compact {
        @include size($size-44);
        padding: $space-8;
        .mc-svg-icon {
          @include size($size-32);
        }
      }
      .mc-svg-icon {
        @include size($size-24);
      }
      #{$block-name} {
        &__prepend {
          margin-right: $space-4;
        }
        &__append {
          margin-left: $space-4;
        }
      }
    }
    &-l {
      height: $size-48;
      padding: $space-14 $space-24;
      font-size: $fs-24;
      line-height: $lh-13;
      &-compact {
        @include size($size-48);
        padding: $space-14;
        .mc-svg-icon {
          @include size($size-24);
        }
      }
      .mc-svg-icon {
        @include size($size-24);
      }
      #{$block-name} {
        &__prepend {
          margin-right: $space-8;
        }
        &__append {
          margin-left: $space-8;
        }
      }
    }
    &-s,
    &-m,
    &-l {
      &-compact {
        #{$block-name} {
          &__prepend,
          &__append {
            margin: 0;
          }
        }
      }
    }
  }
  .fancy-spinner {
    position: absolute;
    left: 50%;
    top: 50%;
    transform: translate(-50%, -50%);
    display: flex;
    justify-content: center;
    align-items: center;
    @include size($size: $size-16);
    div {
      position: absolute;
      width: inherit;
      height: inherit;
      border-radius: 50%;
      &.ring {
        border-width: 3px;
        border-style: solid;
        border-color: transparent;
        animation: 1s fancy infinite alternate;
        &:nth-child(1) {
          border-left-color: $color-light-blue;
          border-right-color: $color-light-blue;
        }
        &:nth-child(2) {
          border-top-color: $color-light-blue;
          border-bottom-color: $color-light-blue;
          animation-delay: 500ms;
        }
      }
      &.dot {
        width: 0.5rem;
        height: 0.5rem;
        background: $color-light-blue;
      }
    }
  }
  @keyframes fancy {
    to {
      transform: rotate(360deg) scale(0.5);
    }
  }
}
</style>

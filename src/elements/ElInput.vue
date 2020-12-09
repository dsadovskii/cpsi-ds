<script>
export default {
  name: 'ElInput',
  functional: true,
  props: {
    value: {
      type: String,
    },
    name: {
      type: String,
      required: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    label: {
      type: String,
      default: null,
    },
    placeholder: {
      type: String,
      default: '',
    },
    animated: {
      type: Boolean,
      default: false,
    },
    required: {
      type: Boolean,
      default: false,
    },
    rule: {
      type: String,
      default: '.*',
    },
    error: {
      type: [String, Array],
      default: null,
    },
    textarea: {
      type: Boolean,
      default: false,
    },
    rows: {
      type: Number,
      default: 5,
    },
    size: {
      type: String,
      default: 'm',
    },
    variant: {
      type: String,
      default: 'default',
    },
  },
  render: function(h, { data, props, parent, slots }) {
    let inputTag = props.textarea ? 'textarea' : 'input'
    let errorMessage = props.error && props.error.constructor === Array ? props.error[0] : props.error
    return h(
      'section',
      {
        class: {
          'el-input': true,
          [`el-input--variant-${props.variant}`]: true,
          [`el-input--size-${props.size}`]: true,
          'el-input--disabled': props.disabled,
          'el-input--error': props.error,
          [`${data.staticClass}`]: !!data.staticClass,
          ...data.class,
        },
        on: {
          click: () => {
            parent.$refs[`el-input_${props.name}`].focus()
          },
        },
      },
      [
        !props.animated || h('output', { class: 'el-input__output', attrs: { tabIndex: -1 } }, props.value),
        h(inputTag, {
          attrs: {
            name: props.name,
            id: props.name,
            required: props.required,
            pattern: props.rule,
            placeholder: props.animated ? '' : props.placeholder,
            disabled: props.disabled,
            rows: props.rows,
          },
          class: [`el-input__${inputTag}`, { 'el-input__input-disabled': props.disabled }],
          ref: `el-input_${props.name}`,
          domProps: { value: data.model.value },
          on: {
            input: event => {
              props.disabled || data.model.callback(event.target.value)
            },
          },
        }),
        !props.label ||
          h(
            'label',
            {
              class: ['el-input__label', { 'el-input__label-animated': props.animated }],
              attrs: { for: props.name },
            },
            props.label,
          ),
        !props.error || h('small', { class: 'el-input--error-msg' }, errorMessage),
        !slots()['append-btn'] || h('div', { class: { 'el-input__slot-append': true } }, slots()['append-btn']),
      ],
    )
  },
}
</script>

<style lang="scss">
.el-input {
  $block-name: &;
  width: 100%;
  border-radius: $radius-3;
  position: relative;
  cursor: text;
  &--disabled {
    cursor: not-allowed;
    background-color: $color-gray;
  }
  &--size-m {
    padding: $space-14 $space-24;
    #{$block-name}__label {
      padding-bottom: 1px;
      padding-top: 1px;
      padding-right: $space-24;
      font-size: $fs-16;
      line-height: $lh-14;
      &-animated {
        left: 0;
        top: $space-14;
        padding-left: $space-24;
      }
    }
    #{$block-name}__slot-append {
      .el-dropdown {
        min-width: 90px;
        height: 52px;
      }
    }
    #{$block-name}__output {
      &:not(:empty) ~ #{$block-name}__input,
      &:not(:empty) ~ #{$block-name}__textarea,
      ~ #{$block-name}__input:focus,
      ~ #{$block-name}__textarea:focus {
        ~ #{$block-name}__label {
          font-size: $fs-12;
        }
      }
    }
  }
  &--size-s {
		padding: $space-8 $space-16;
		height: $size-40;
		#{$block-name}__label {
			padding-bottom: 1px;
			padding-top: 1px;
			padding-right: $space-24;
			font-size: $fs-14;
			line-height: $lh-14;
			&-animated {
				left: 0;
				top: 9px;
				padding-left: $space-16;
			}
		}
    #{$block-name}__slot-append {
      .el-dropdown {
        min-width: 90px;
        height: 40px;
      }
    }
    #{$block-name}__output {
      &:not(:empty) ~ #{$block-name}__input,
      &:not(:empty) ~ #{$block-name}__textarea,
      ~ #{$block-name}__input:focus,
      ~ #{$block-name}__textarea:focus {
        ~ #{$block-name}__label {
          font-size: $fs-10;
        }
      }
    }
    #{$block-name}__input,
    #{$block-name}__input::placeholder {
      font-size: $fs-14;
    }
  }
  &__output {
    position: absolute;
    visibility: hidden;
    width: $space-zero;
    height: $space-zero;
    z-index: -9;
    &:not(:empty) ~ #{$block-name}__input,
    &:not(:empty) ~ #{$block-name}__textarea,
    ~ #{$block-name}__input:focus,
    ~ #{$block-name}__textarea:focus {
      transform: translateY($space-8);
      ~ #{$block-name}__label {
        font-size: $fs-12;
        line-height: $lh-1;
        transform: translateY(-$space-8);
        text-overflow: ellipsis;
        white-space: nowrap;
        overflow: hidden;
        width: 100%;
      }
    }
    ~ #{$block-name}__input::placeholder {
      visibility: hidden;
    }
  }
  &__input,
  &__textarea {
    width: 100%;
    font-size: $fs-16;
    line-height: $lh-14;
    border: none;
    background: none;
    box-shadow: none;
    outline: none;
    font-family: 'Inter', sans-serif;
    &-disabled {
      cursor: inherit;
      resize: none;
      ~ #{$block-name}__label {
        cursor: inherit;
        user-select: none;
      }
    }
    &::placeholder {
      font-family: inherit;
      color: $color-gray;
    }
  }
  &__textarea {
    min-height: $space-28;
    max-height: $space-160;
    &::-webkit-scrollbar {
      width: $space-8;
      background-color: $color-gray;
    }
    &::-webkit-scrollbar-thumb {
      border-radius: $radius-3;
      background-color: $color-blue;
    }
  }
  &__label {
    position: absolute;
    left: 0;
    bottom: calc(100% + 5px);
    color: $color-gray;
    transition: all 0.2s ease;
    cursor: text;
    height: max-content;
    border-radius: $radius-3;
    &-animated {
      display: block;
      align-items: center;
      user-select: none;
      pointer-events: none;
    }
  }
  &:hover,
  &:focus,
  &:focus-within {
    border-color: $color-blue;
  }
  &--error {
    border-color: #{$color-red} !important;
    background-color: #{$color-red}20;
    color: $color-red;
    #{$block-name} {
      &__input,
      &__input::placeholder,
      &__label,
      &__error-msg {
        color: inherit;
      }
      &__label {
        background-color: transparent;
      }
    }
    &-msg {
      position: absolute;
      left: 0;
      display: block;
      max-width: 100%;
      animation: slideDown 0.3s forwards;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
      font-size: $fs-14;
    }
    @keyframes slideDown {
      from {
        top: 100%;
        opacity: 0;
      }
      to {
        top: calc(100% + 5px);
        opacity: 1;
      }
    }
  }
  &__slot-append {
    position: absolute;
    top: 50%;
    right: $space-24;
    transform: translateY(-50%);
    transform-origin: center;
    display: flex;
    align-items: center;
    .el-dropdown {
      background: #1e3685;
      border-radius: 0 3px 3px 0;
      transform: translateX($space-24);
      .ant-select {
        height: inherit;
        display: inline-flex;
        align-items: center;
        color: #ffffff;
        &-selection {
          min-width: 100%;
        }
        .ant-select-arrow-icon {
          svg {
            fill: $color-gray;
          }
        }
      }
    }
  }
  &--variant-default {
    background-color: $color-white;
  }
  &--variant-gray {
    background-color: $bg-lighter-blue;
    border: 1px solid $color-light-blue;
    #{$block-name}__input::placeholder {
      color: $color-steel-gray;
    }
    #{$block-name}__label {
      color: $color-steel-gray;
      bottom: calc(100% + 20px);
    }
    #{$block-name}__slot-append {
      .el-button {
        border-radius: 0 $radius-3 $radius-3 0;
      }
    }
  }
}
</style>

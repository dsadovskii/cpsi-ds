<script>
export default {
  name: 'ElInput',
  functional: true,
  props: {
    value: {
      type: [String, Number],
    },
    name: {
      type: String,
      required: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    type: {
      type: String,
      default: 'text',
    },
    label: {
      type: String,
      default: null,
    },
    title: {
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
      default: 's',
    },
    variant: {
      type: String,
      default: 'lightblue',
    },
    mask: {
      type: String,
      default: '',
    },
  },
  render: function(h, { data, props, parent, slots, listeners }) {
    let inputTag = props.textarea ? 'textarea' : 'input'
    let errorMessage = props.error && props.error.constructor === Array ? props.error[0] : props.error
    return h('section', [
      !!props.title &&
        h(
          'span',
          {
            class: 'el-input__title',
          },
          props.title,
        ),
      h(
        'section',
        {
          class: {
            'el-input': true,
            [`el-input--variant-${props.variant}`]: true,
            [`el-input--size-${props.size}`]: true,
            'el-input--textarea': props.textarea,
            'el-input--disabled': props.disabled,
            'el-input--error': props.error,
            'el-input--with-title': !!props.title,
            [`${data.staticClass}`]: !!data.staticClass,
            ...data.class,
          },
          on: {
            click: () => {
              parent.$refs[`el-input_${props.name}`] && parent.$refs[`el-input_${props.name}`].focus()
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
              type: props.type,
              placeholder: props.animated ? '' : props.placeholder,
              disabled: props.disabled,
              rows: props.rows,
            },
            class: [`el-input__${inputTag}`, { 'el-input__input-disabled': props.disabled }],
            ref: `el-input_${props.name}`,
            domProps: { value: data?.model?.value || props.value || null },
            on: {
              input: event => {
                if (!props.disabled) {
                  if (data.model) data.model.callback(event.target.value)
                  if (data && data.on && data.on.input) {
                    if (data.on.input[1] && data.on.input[1].constructor === Function)
                      data.on.input[1](event.target.value)
                    if (data.on.input.constructor === Function) data.on.input(event.target.value)
                  }
                  if (data && data.nativeOn && data.nativeOn.input) {
                    if (data.nativeOn.input[1] && data.nativeOn.input[1].constructor === Function)
                      data.nativeOn.input[1](event, event.target.value)
                    if (data.nativeOn.input.constructor === Function) data.nativeOn.input(event, event.target.value)
                  }
                }
              },
              keydown: event => {
                if (event.keyCode === 13) {
                  listeners && listeners.enter && listeners.enter()
                }
              },
            },
            [!props.mask ? '' : 'directives']: [
              {
                name: 'mask',
                value: props.mask,
              },
            ],
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
      ),
    ])
  },
}
</script>

<style lang="scss">
.el-input {
  $block-name: &;
  width: 100%;
  position: relative;
  cursor: text;
  border-radius: 3px;
  &--disabled {
    cursor: not-allowed;
    background-color: $color-gray;
  }
  &--with-title {
    margin-top: $space-10;
  }
  &__title {
    font-size: $fs-14;
    line-height: $lh-14;
    color: $color-dark-gray;
    max-width: 100%;
    white-space: pre-line;
    word-break: break-word;
  }
  &--size-m {
    padding: $space-14 $space-20;
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
    padding: $space-8 $space-14;
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
  &--textarea {
    height: auto;
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
    color: $color-black;
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
      font-size: $fs-12;
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
    top: 2px;
    right: 2px;
    height: calc(100% - 4px);
    transform-origin: center;
    display: flex;
    align-items: center;
    justify-content: center;
    overflow: hidden;
    border-radius: 3px;
  }
  &--variant {
    #{$block-name}__input::placeholder {
      color: $color-gray;
    }
    #{$block-name}__label {
      color: $color-gray;
      bottom: calc(100% + 20px);
    }
    &-default {
      background-color: $color-white;
      box-shadow: 0px 5px 15px rgba(0, 0, 0, 0.1);
    }
    &-lightblue {
      background-color: $bg-lighter-blue;
      border: 1px solid $color-gray;
    }
  }
}
</style>

<script>
import { MediaGenerator } from '../helpers/mediaGenerator'

let params = ['size', 'lh']
let sizeProps = new MediaGenerator(params).generateProps(String)

export default {
  name: 'ElText',
  functional: true,
  props: {
    ...sizeProps,
    heading: {
      type: Boolean,
      default: false,
    },
    level: {
      type: Number,
      default: 2,
    },
    tag: {
      type: String,
      default: 'p',
    },
    size: {
      type: String,
      default: '16',
    },
    lh: {
      type: String,
      default: '24',
    },
    align: {
      type: String,
      default: 'left',
    },
    block: {
      type: Boolean,
      default: false,
    },
    textBold: {
      type: Boolean,
      default: false,
    },
    color: {
      type: String,
      default: 'black',
    },
    /**
     * Font-weights: regular, semi-bold, bold, black
     **/
    fontWeight: {
      type: String,
      default: 'regular',
    },
    noWrap: {
      type: Boolean,
      default: false,
    },
    ellipsis: {
      type: Boolean,
      default: false,
    },
    preLine: {
      type: Boolean,
      default: false,
    },
    maxWidth: {
      type: String,
      default: 'auto',
    },
  },
  render(h, { props, slots, data }) {
    let tag
    tag = props.heading ? `h${props.level}` : props.tag
    let classes = {
      'el-text': true,
      // [`el-text--size-${props.size}`]: props.size,
      [`el-text--size-xs-${props.size}`]: props.size,
      [`el-text--size-s-${props.sizeS}`]: props.sizeS,
      [`el-text--size-m-${props.sizeM}`]: props.sizeM,
      [`el-text--size-l-${props.sizeL}`]: props.sizeL,
      [`el-text--size-xl-${props.sizeXl}`]: props.sizeXl,
      // [`el-text--line-height-${props.lh}`]: props.lh,
      [`el-text--line-height-xs-${props.lh}`]: props.lh,
      [`el-text--line-height-s-${props.lhS}`]: props.lhS,
      [`el-text--line-height-m-${props.lhM}`]: props.lhM,
      [`el-text--line-height-l-${props.lhL}`]: props.lhL,
      [`el-text--line-height-xl-${props.lhXl}`]: props.lhXl,
      [`el-text--font-weight-${props.fontWeight}`]: props.fontWeight,
      [`el-text--align-${props.align}`]: props.align,
      [`el-text--${props.color}`]: props.color,
      'el-text--bold': props.textBold,
      'el-text--block': props.block,
      'el-text--nowrap': props.noWrap,
      'el-text--ellipsis': props.ellipsis,
      'el-text--pre-line': props.preLine,
      [`${data.staticClass}`]: !!data.staticClass,
      ...data.class,
    }
    const htmlContent = {}
    if (data.domProps && data.domProps.innerHTML) {
      htmlContent.domProps = {
        innerHTML: data.domProps.innerHTML,
      }
    }
    return h(
      tag,
      {
        class: classes,
        style: {
          maxWidth: props.maxWidth,
        },
        ...htmlContent,
      },
      slots()['default'],
    )
  },
}
</script>
<style lang="scss">
.el-text {
  $block-name: &;
  @each $media, $value in $medias {
    @media (min-width: $value) {
      @each $size, $option in $font-sizes {
        &--size-#{$media}-#{$size} {
          font-size: $option;
        }
      }
      @each $height, $option in $line-heights {
        &--line-height-#{$media}-#{$height} {
          line-height: $option;
        }
      }
    }
  }

  margin-bottom: 0;
  &--bold {
    font-weight: bold;
  }
  &--block {
    display: inline-flex;
    width: 100%;
  }
  &--align-left {
    text-align: left;
    justify-content: flex-start;
  }
  &--align-center {
    text-align: center;
    justify-content: center;
  }
  &--align-right {
    text-align: right;
    justify-content: flex-end;
  }
  &--nowrap {
    white-space: nowrap;
    flex-wrap: nowrap;
  }
  &--ellipsis {
    max-width: 100%;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    word-wrap: normal;
  }
  &--pre-line {
    white-space: pre-line;
  }
  @each $name, $color in $colors {
    &--#{$name} {
      color: #{$color};
    }
  }
  //@each $size, $param in $font-sizes {
  //  &--size-#{$size} {
  //    font-size: $param;
  //  }
  //}
  //@each $height, $param in $line-heights {
  //  &--line-height-#{$height} {
  //    line-height: $param;
  //  }
  //}
  @each $weight, $param in $font-weights {
    &--font-weight-#{$weight} {
      font-weight: $param;
    }
  }
}
</style>

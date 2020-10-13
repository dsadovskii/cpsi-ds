<template>
  <a v-if="href" :href="href" :class="computedClasses" :target="blank ? '_blank' : '_self'" :download="download">
    <span v-if="iconPrepend" class="el-link__icon-prepend">
      <slot name="icon-prepend" />
    </span>
    <span class="el-link__content">
      <slot />
    </span>
    <span v-if="iconAppend" class="el-link__icon-append">
      <slot name="icon-append" />
    </span>
  </a>
  <component :is="linkTag" v-else :class="computedClasses" :to="to">
    <span v-if="iconPrepend" class="el-link__icon-prepend">
      <slot name="icon-prepend" />
    </span>
    <span class="el-link__content">
      <slot />
    </span>
    <span v-if="iconAppend" class="el-link__icon-append">
      <slot name="icon-append" />
    </span>
  </component>
</template>

<script>
export default {
  name: 'ElLink',
  props: {
    to: {
      type: [String, Object],
      default: () => ({}),
    },
    href: {
      type: String,
      default: null,
    },
    blank: {
      type: Boolean,
      default: false,
    },
    nuxt: {
      type: Boolean,
      default: true,
    },
    variant: {
      type: String,
      default: 'blue',
    },
    size: {
      type: String,
      default: '16',
    },
    lh: {
      type: String,
      default: '14',
    },
    /**
     * Font-weights: regular, semi-bold, bold, black
     **/
    fontWeight: {
      type: String,
      default: 'bold',
    },
    fullWidth: {
      type: Boolean,
      default: false,
    },
    download: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    computedClasses() {
      return {
        'el-link': true,
        [`el-link--size-${this.size}`]: !!this.size,
        [`el-link--line-height-${this.lh}`]: !!this.lh,
        [`el-link--font-weight-${this.fontWeight}`]: !!this.fontWeight,
        [`el-link--variant-${this.variant}`]: !!this.variant,
        'el-link--full-width': this.fullWidth,
      }
    },
    iconPrepend() {
      return this.$slots['icon-prepend']
    },
    iconAppend() {
      return this.$slots['icon-append']
    },
    linkTag() {
      return this.nuxt ? 'nuxt-link' : 'router-link'
    },
  },
}
</script>

<style lang="scss">
.el-link {
  $block-name: &;
  display: inline-flex;
  align-items: center;
  flex-wrap: wrap;
  position: relative;
  text-decoration: none;
  @each $size, $param in $font-sizes {
    &--size-#{$size} {
      font-size: $param;
    }
  }
  @each $color, $param in $colors {
    &--variant-#{$color} {
      color: $param;
      @media #{$desktop} {
        &:hover {
          color: rgba($param, 0.5);
        }
      }
      @media #{$mobile} {
        &:active {
          color: rgba($param, 0.5);
        }
      }
    }
  }
  @each $height, $param in $line-heights {
    &--line-height-#{$height} {
      line-height: $param;
    }
  }

  @each $weight, $param in $font-weights {
    &--font-weight-#{$weight} {
      font-weight: $param;
    }
  }
  &--full-width {
    width: 100%;
  }
  &__icon-prepend {
    & + #{$block-name}__content {
      margin-left: 10px;
    }
  }
  &__content {
    font-size: inherit;
    font-weight: inherit;
    line-height: inherit;
    & + #{$block-name}__icon-append {
      margin-left: 10px;
    }
  }
  &__icon-prepend,
  &__icon-append {
    display: inline-flex;
    align-items: center;
  }
}
</style>

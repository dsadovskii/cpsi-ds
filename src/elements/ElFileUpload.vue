<template>
  <article class="el-file-upload">
    <section class="el-file-upload__file">
      <label class="el-file-upload__label" :for="name">
        <span class="el-file-upload__icon-prepend">
          <slot v-if="$slots['icon-prepend']" name="icon-prepend" />
        </span>
        <span class="el-file-upload__content">
          {{ text }}
        </span>
        <span class="el-file-upload__icon-append">
          <el-button :disabled="disabled" full-width @click.stop.prevent="setInputFocus">
            <el-svg-icon name="pin" size="20" />
          </el-button>
        </span>
      </label>
      <input
        :id="name"
        :ref="name"
        :name="name"
        :disabled="disabled"
        type="file"
        class="el-file-upload__input"
        multiple
        @change="setFile"
      />
    </section>
    <template v-if="hasFiles">
      <el-badge
        v-for="(file, index) in files"
        @click="() => removeFile(index, file)"
        variant="light-blue"
        color="blue"
        :closable="!disabled"
        :key="`file${file}`"
        class="mt-24 mr-16"
      >
        {{ fileName(file) }}
      </el-badge>
    </template>
  </article>
</template>

<script>
import ElButton from './ElButton.vue'
import ElBadge from './ElBadge.vue'
import ElSvgIcon from './ElSvgIcon.vue'
export default {
  name: 'ElFileUpload',
  components: {
    ElButton,
    ElBadge,
    ElSvgIcon,
  },
  props: {
    value: {},
    name: {
      type: String,
      default: null,
      required: true,
    },
    title: {
      type: String,
      default: null,
      required: true,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    files: {
      get() {
        return this.value
      },
      set(value) {
        this.$emit('input', value)
      },
    },
    hasFiles() {
      return this.value && this.value.length
    },
    text() {
      if (this.disabled) {
        return 'Files uploaded'
      } else if (this.files && this.files.length) {
        return `Selected ${this.files.length} files`
      }
      return this.title
    },
  },
  methods: {
    fileName(file) {
      if (file) {
        return file.name || file
      }
    },
    setFile(event) {
      this.$emit('input', [...event.target.files])
    },
    clearFile() {
      this.$emit('input', null)
    },
    setInputFocus() {
      this.$refs[this.name].click()
    },
    // eslint-disable-next-line no-unused-vars
    removeFile(index, file) {
      let files = [...this.files]
      files.splice(index, 1)
      this.files = files
    },
  },
}
</script>

<style lang="scss">
.el-file-upload {
  $block-name: &;
  &__input {
    display: none;
  }
  &__file,
  &__label,
  &__icon-append,
  &__icon-prepend,
  &__result {
    display: inline-flex;
    align-items: center;
  }
  &__icon-append {
    position: absolute;
    right: 0;
    top: 0;
    height: 52px;
    width: 90px;
    .el-button {
      height: inherit;
      border-radius: 0 $radius-3 $radius-3 0;
      pointer-events: none;
    }
  }
  &__file {
    display: flex;
    justify-content: space-between;
    align-items: center;
    width: 100%;
    background-color: transparent;
    border: 1px solid $color-gray;
    border-radius: $radius-3;
    padding: $space-14 $space-24;
    height: 52px;
  }
  &__label {
    font-size: $fs-16;
    line-height: $lh-14;
    transition: all 0.2s ease;
    display: flex;
    justify-content: space-between;
    width: 100%;
    #{$block-name}__content {
      flex-grow: 2;
    }
    @media #{$desktop} {
      &:hover {
        cursor: pointer;
        color: $color-blue;
        #{$block-name}__icon-append,
        #{$block-name}__icon-prepend {
          .el-icon {
            transition: all 0.2s ease;
            stroke: $color-red;
          }
        }
      }
    }
    @media #{$mobile} {
      &:active {
        color: $color-red;
        #{$block-name}__icon-append,
        #{$block-name}__icon-prepend {
          .el-icon {
            transition: all 0.2s ease;
            stroke: $color-red;
          }
        }
      }
    }
  }
  &__result {
    position: relative;
    padding-right: 20px;
  }
  &__restore {
    position: absolute;
    right: 0;
    top: 0;
    transform: translate(30%, -30%);
    @media #{$desktop} {
      &:hover {
        cursor: pointer;
        transition: all 0.2s ease;
        stroke: $color-red;
      }
    }
    @media #{$mobile} {
      &:active {
        transition: all 0.2s ease;
        stroke: $color-red;
      }
    }
  }
}
</style>

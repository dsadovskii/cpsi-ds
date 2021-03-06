<template>
  <div class="el-dropdown" @click.stop.prevent>
    <a-select
      :mode="mode"
      class="no-select-styles"
      :dropdown-match-select-width="matchWidth"
      :default-value="computedValue"
      :value="computedValue"
      :class="classes"
      :style="styles"
      :placeholder="computedPlaceholder"
      :show-search="filterable || searchable"
      :disabled="disabled"
      :required="required"
      :filter-option="!searchable"
      :not-found-content="computedNotFoundContent"
      @change="handleChange"
      @search="handleSearch"
    >
      <div slot="suffixIcon" class="custom-select-arrow">
        <el-svg-icon name="chevron_down" size="12" color="white" />
      </div>
      <!--eslint-disable-next-line-->
      <a-select-option v-for="(item, i) in options" :key="new Date().getTime() + i" :value="getValue(item)">
        {{ getTitle(item) }}
      </a-select-option>
    </a-select>
    <div v-if="mode === 'multiple' || searchable" class="searchable-icon">
      <el-svg-icon name="search" size="12" color="white" />
    </div>
  </div>
</template>

<script>
import _has from 'lodash/has'
import _get from 'lodash/get'
import ElSvgIcon from './ElSvgIcon.vue'
export default {
  name: 'ElDropdown',
  components: {
    ElSvgIcon,
  },
  props: {
    value: {
      type: [String, Number, Object, Array],
      required: false,
    },
    options: {
      type: Array,
      default: () => [],
    },
    valueField: {
      type: String,
      default: 'value',
    },
    titleField: {
      type: String,
      default: 'title',
    },
    noBg: {
      type: Boolean,
      default: false,
    },
    filterable: {
      type: Boolean,
      default: false,
    },
    noArrowBg: {
      type: Boolean,
      default: false,
    },
    matchWidth: {
      type: Boolean,
      default: false,
    },
    minWidth: {
      type: String,
      default: '1px',
    },
    /**
     * multiple, default
     * */
    mode: {
      type: String,
      default: 'default',
    },
    searchable: {
      type: Boolean,
      default: false,
    },
    placeholder: {
      type: String,
      default: null,
    },
    returnObject: {
      type: Boolean,
      default: false,
    },
    invertedColors: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    required: {
      type: Boolean,
      default: false,
    },
    size: {
      type: String,
      default: 's',
    },
    notFoundContent: {
      type: String,
      default: 'Ничего не найдено',
    },
  },
  computed: {
    computedNotFoundContent() {
      return this.searchable ? this.notFoundContent : null
    },
    computedValue: {
      get() {
        return this.returnObject
          ? this.mode === 'multiple'
            ? [...this.value].map(val => val[this.valueField])
            : (this.value && this.value[this.valueField]) || ''
          : this.value || undefined
      },
      set(value) {
        this.$emit('input', value)
      },
    },
    classes() {
      return {
        'el-dropdown--no-bg': this.noBg,
        'el-dropdown--no-arrow-bg': this.noArrowBg,
        'el-dropdown--inverted-colors': this.invertedColors,
        [`el-dropdown--size-${this.size}`]: true,
      }
    },
    styles() {
      return {
        'min-width': this.minWidth,
      }
    },
    computedPlaceholder() {
      if (!this.placeholder) {
        return this.searchable ? 'Начните вводить' : 'Выберите'
      }
      return this.placeholder
    },
  },
  methods: {
    getValue(item) {
      return _has(item, this.valueField) ? this.getValueByProp(item, this.valueField) : null
    },
    getTitle(item) {
      return _has(item, this.titleField) ? this.getValueByProp(item, this.titleField) : null
    },
    getValueByProp(item, prop) {
      return _get(item, prop)
    },
    handleChange(value) {
      if (this.returnObject) {
        if (this.mode === 'multiple') {
          value = this.options.filter(o => {
            return value.includes(o[this.valueField])
          })
        } else {
          const object = this.options.find(o => o[this.valueField] === value)
          value = {
            [this.valueField]: object[this.valueField],
            [this.titleField]: object[this.titleField],
          }
        }
      }
      this.computedValue = value
      this.$emit('change', value)
    },
    handleSearch(value) {
      this.$emit('search', value)
    },
  },
}
</script>

<style lang="scss">
.ant-select-dropdown-menu-item {
  color: $color-black !important;
}
.el-dropdown {
  $block-name: &;
  position: relative;
  min-width: 1px;
  width: 100%;
  .ant-select-selection {
    &-selected-value {
      color: black;
    }
    &__choice {
      color: $color-white !important;
      border-radius: 3px !important;
      background-color: $bg-blue !important;
      &__remove {
        color: $color-white !important;
      }
    }
  }
  &--no-bg {
    .ant-select-selection {
      background: none !important;
      box-shadow: none !important;
      border: none !important;
      height: auto;
      &__rendered {
        margin-right: 30px !important;
      }
      &-selected-value {
        color: $color-blue !important;
      }
    }
    min-width: auto !important;
  }
  &--no-arrow-bg {
    .ant-select-arrow {
      background-color: transparent !important;
    }
  }
  &--inverted-colors {
    color: $color-white !important;
    ˜ .ant-select {
      &-selection {
        background-color: $color-gray;
        border-color: $color-gray;
        color: $color-white;
        &__placeholder {
          color: $color-white;
        }
        &-selected-value {
          color: $color-white !important;
        }
      }
      &-arrow {
        color: $color-white;
      }
    }
  }
  &--size-m {
    .ant-select-selection {
      border-color: $color-gray !important;
      border-radius: 3px !important;
      padding: 0;
      &--multiple,
      &--single {
        min-height: 52px;
        font-size: $space-16;
        line-height: $lh-14;
        margin: 0 !important;
        padding: $space-10 $space-24;
      }
      &__placeholder {
        margin-left: 0 !important;
        margin-right: 0;
      }
    }
  }
  &--size-s {
    .ant-select-selection {
      border-color: $color-gray !important;
      background-color: $bg-lighter-blue;
      border-radius: 3px !important;
      padding: 0;
      box-shadow: none !important;
      &--multiple,
      &--single {
        min-height: 40px;
        font-size: $space-14;
        line-height: $lh-14;
        margin: 0 !important;
        padding: $space-4 41px $space-4 $space-20;
      }
      &__rendered {
        margin: 0 !important;
      }
      &__placeholder {
        margin-left: 0 !important;
        margin-right: 0;
      }
    }
    .ant-select-arrow {
      margin: 0 !important;
      width: 35px;
      background-color: $bg-blue;
      display: flex;
      align-items: center;
      justify-content: center;
      top: 2px;
      right: 2px;
      height: calc(100% - 4px);
      border-radius: 3px;
    }
  }
  .ant-select {
    width: 100%;
    &-arrow,
    .anticon {
      display: inline-flex;
      align-items: center;
    }
    &-arrow {
      margin-top: -5px;
    }
  }
  .searchable-icon {
    position: absolute;
    top: 3px;
    right: 3px;
    width: 35px;
    background-color: $bg-blue;
    display: flex;
    align-items: center;
    justify-content: center;
    height: calc(100% - 6px);
    border-radius: 3px;
    pointer-events: none;
  }
}
.ant-select-dropdown-menu-item {
  &-disabled {
    color: $color-gray !important;
  }
}
</style>

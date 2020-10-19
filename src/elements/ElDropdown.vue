<template>
  <div class="el-dropdown">
    <a-select
      class="no-select-styles"
      :dropdown-match-select-width="false"
      :default-value="computedValue"
      :value="computedValue"
      :class="classes"
      :style="styles"
      @change="handleChange"
    >
      <a-select-option v-for="item in options" :key="item.id || item.key" :value="getValue(item)">
        {{ getTitle(item) }}
      </a-select-option>
    </a-select>
  </div>
</template>

<script>
import _has from 'lodash/has'
import _get from 'lodash/get'
export default {
  name: 'ElDropdown',
  props: {
    value: {
      type: [String, Number, Object],
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
    minWidth: {
      type: String,
      default: '230px',
    },
    returnObject: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    computedValue: {
      get() {
        return this.returnObject ? (this.value && this.value[this.valueField]) || '' : this.value
      },
      set(value) {
        this.$emit('input', value)
      },
    },
    classes() {
      return {
        'el-dropdown--no-bg': this.noBg,
      }
    },
    styles() {
      return {
        'min-width': this.minWidth,
      }
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
        const object = this.options.find(o => o[this.valueField] === value)
        value = {
          [this.valueField]: object[this.valueField],
          [this.titleField]: object[this.titleField],
        }
      }
      this.computedValue = value
      this.$emit('change', value)
    },
  },
}
</script>

<style lang="scss">
.el-dropdown {
  $block-name: &;
  &--no-bg {
    .ant-select-selection {
      background: none;
      box-shadow: none !important;
      border: none !important;
    }
  }
  .ant-select-selection {
    &-selected-value {
      color: $color-gray;
    }
  }
}
</style>

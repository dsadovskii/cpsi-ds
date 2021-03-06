<template>
  <section class="el-datepicker" :class="computedClasses">
    <date-picker
      :id="name"
      :ref="name"
      v-model="date"
      :valueType="valueType"
      :type="type"
      :name="name"
      :required="required"
      :disabled="disabled"
      :placeholder="placeholder"
      :format="format"
      :append-to-body="appendToBody"
      @change="emit('change', $event)"
      @open="emit('open', $event)"
      @close="emit('close', $event)"
      @clear="emit('clear', $event)"
      @pick="emit('pick', $event)"
      @focus="emit('focus', $event)"
    />
    <small class="el-datepicker--error-msg">{{ error }}</small>
  </section>
</template>

<script>
import DatePicker from 'vue2-datepicker'
import 'vue2-datepicker/index.css'
import 'vue2-datepicker/locale/ru'
import moment from 'moment'
export default {
  name: 'ElDatepicker',
  components: {
    DatePicker,
  },
  props: {
    value: {
      type: [String, Date],
      default: null,
    },
    type: {
      type: String,
      default: 'date',
    },
    size: {
      type: String,
      default: 's',
    },
    name: {
      type: String,
      default: null,
    },
    required: {
      type: Boolean,
      default: false,
    },
    disabled: {
      type: Boolean,
      default: false,
    },
    valueType: {
      type: String,
      default: 'format',
    },
    placeholder: {
      type: String,
      default: null,
    },
    format: {
      type: String,
      default: 'DD.MM.YYYY',
    },
    toFormat: {
      type: String,
      default: 'YYYY-MM-DD HH:mm',
    },
    error: {
      type: [String, Array],
      default: null,
    },
    appendToBody: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      lang: {
        formatLocale: {
          firstDayOfWeek: 1,
        },
        monthBeforeYear: false,
      },
    }
  },
  computed: {
    computedClasses() {
      return [{ 'el-datepicker--error': this.error }, { [`el-datepicker--size-${this.size}`]: true }]
    },
    date: {
      get() {
        return moment(this.value, this.toFormat).format(this.format)
      },
      set(value) {
        this.$emit('input', value ? moment(value, this.format).format(this.toFormat) : value)
      },
    },
  },
  methods: {
    emit(event, value) {
      this.$emit(event, value)
    },
  },
}
</script>

<style lang="scss">
.el-datepicker {
  $block-name: &;
  position: relative;
  width: 100%;
  &--size-m {
    .mx-datepicker {
      width: 100%;
      .mx-input {
        padding: $space-14 $space-24;
        font-size: $fs-16;
        line-height: $lh-14;
        background-color: transparent;
        border: 1px solid $color-gray;
        box-shadow: none;
        outline: none;
        color: $color-black;
        height: 52px;
        border-radius: 3px;
        &::placeholder {
          color: $color-gray;
          font-size: $fs-16;
        }
      }
      .mx-icon-calendar,
      .mx-icon-clear {
        right: 24px;
        color: $color-black;
      }
      &-main {
        width: 100%;
        max-width: 300px;
        .mx-calendar {
          width: 100%;
          &-content {
            height: auto;
            .mx-table {
              &-date {
                td,
                th {
                  font-size: $fs-16;
                  line-height: $lh-2;
                }
              }
            }
          }
        }
      }
      .mx-icon-calendar {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 90px;
        height: 52px;
        right: 0;
        color: $color-white;
        background: $color-blue;
        pointer-events: none;
        border-radius: 3px;
      }
    }
  }
  &--size-s {
    .mx-datepicker {
      width: 100%;
      background-color: $bg-lighter-blue;
      .mx-input {
        padding: $space-8 $space-20;
        font-size: $fs-14;
        line-height: $lh-14;
        background-color: transparent;
        border: 1px solid $color-gray;
        box-shadow: none;
        outline: none;
        color: $color-black;
        height: 40px;
        border-radius: 3px;
        &::placeholder {
          color: $color-gray;
          font-size: $fs-14;
        }
      }
      .mx-icon-calendar,
      .mx-icon-clear {
        right: 24px;
        color: $color-black;
      }
      &-main {
        width: 100%;
        max-width: 300px;
        .mx-calendar {
          width: 100%;
          &-content {
            height: auto;
            .mx-table {
              &-date {
                td,
                th {
                  font-size: $fs-16;
                  line-height: $lh-2;
                }
              }
            }
          }
        }
      }
      .mx-icon-calendar {
        display: flex;
        align-items: center;
        justify-content: center;
        width: 35px;
        height: calc(100% - 6px);
        right: 3px;
        color: $color-white;
        background: $color-blue;
        pointer-events: none;
        border-radius: 3px;
      }
      &-popup {
        min-width: 350px;
      }
    }
  }
  &--error {
    color: $color-red;
    .mx-input {
      border-color: #{$color-red} !important;
      background-color: #{$color-red}20;
      color: inherit;
    }
    .mx-input::placeholder {
      color: $color-red !important;
    }
    &-msg {
      font-size: $fs-14;
      position: absolute;
      left: 0;
      animation: slideDown 0.3s forwards;
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
}
</style>

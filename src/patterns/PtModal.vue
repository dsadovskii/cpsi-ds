<template>
  <modal
    :class="classes"
    :name="name"
    :max-width="maxWidth"
    :click-to-close="clickToClose"
    class="pt-modal"
    scrollable
    adaptive
    height="auto"
    width="100%"
    @before-open="handleBeforeOpen"
    @before-close="handleBeforeClose"
    @closed="handleClosed"
    @opened="handleOpened"
  >
    <div class="pt-modal__inner">
      <div v-if="$slots.title" class="pt-modal__header">
        <div class="pt-modal__title">
          <!-- @slot Слот заголовка -->
          <slot name="title" />
        </div>
      </div>
      <div class="pt-modal__body">
        <!-- @slot Слот контента -->
        <slot />
      </div>
      <div v-if="$slots.footer" class="pt-modal__control">
        <!-- @slot Слот футера -->
        <slot name="footer" />
      </div>
      <button v-if="closeVisible" type="button" class="pt-modal__btn-close" @click.prevent="close">
        <el-svg-icon class="pt-modal__icon-close" size="12" name="close" />
      </button>
    </div>
  </modal>
</template>

<script>
import ElSvgIcon from '../elements/ElSvgIcon'
export default {
  name: 'McModal',
  components: { ElSvgIcon },
  status: 'ready',
  release: '1.0.0',
  props: {
    name: {
      type: String,
    },
    maxWidth: {
      type: Number,
      default: 710,
    },
    closeVisible: {
      type: Boolean,
      default: true,
    },
    /**
     *  Нужно ли закрывать попап
     *  кликом вне окна
     */
    clickToClose: {
      type: Boolean,
      default: true,
    },
    /**
     *  Стрелка в хедере
     */
    arrowVisible: {
      type: Boolean,
      default: false,
    },
    /**
     *  Кастомное модальное окно
     */
    secondaryModal: {
      type: Boolean,
      default: false,
    },
  },
  computed: {
    classes() {
      return {
        'pt-modal--arrow-visible': this.arrowVisible,
        'pt-modal--secondary': this.secondaryModal,
      }
    },
  },
  methods: {
    handleBeforeOpen(event) {
      /**
       * Событие перед открытием
       * @property {Object}
       */
      this.$emit('before-open', event)
    },
    handleBeforeClose(event) {
      /**
       * Событие перед закрытием
       * @property {Object}
       */
      this.$emit('before-close', event)
    },
    handleOpened(event) {
      /**
       * Событие после открытия
       * @property {Object}
       */
      this.$emit('opened', event)
    },
    handleClosed(event) {
      /**
       * Событие после закрытия
       * @property {Object}
       */
      this.$emit('closed', event)
    },
    close() {
      this.$modal.hide(this.name)
    },
  },
}
</script>

<!-- For use required property name & next you call $modal.show(name) -->

<style lang="scss">
.v--modal-block-scroll {
  width: 100%;
}
.pt-modal {
  $block-name: &;
  $border-color: #dee1e9;
  $box-shadow-color: #20008c28;
  padding: 12px 0;
  .vm--modal {
    border-radius: $radius-16;
  }
  &__btn-close {
    background: none;
    text-decoration: none;
    user-select: none;
    border: none;
    box-shadow: none;
    line-height: 0;
    position: absolute;
    top: $space-20;
    right: $space-20;
    padding: 5px;
    cursor: pointer;
    z-index: 1;
  }
  &__icon-close {
    @include size($size-24);
    transition: color 0.3s;
  }
  &.overlay-fade-enter-active,
  &.overlay-fade-leave-active {
    .pt-modal__inner {
      transition: all 0.3s;
      transform: translate3d(0, 0, 0);
    }
  }
  &.overlay-fade-enter,
  &.overlay-fade-leave-active {
    .pt-modal__inner {
      transform: translate3d(0, -20px, 0);
    }
  }
  &.pt-modal--arrow-visible {
    .pt-modal {
      &__header {
        padding-left: 35px;
      }
    }
  }
  &.pt-modal--secondary {
    .pt-modal {
      &__inner {
        border-radius: 32px;
        box-shadow: 0px 15px 30px $box-shadow-color;
        padding: 32px;
      }
      &__btn-close,
      &__btn-back {
        top: 27px;
      }
      &__btn-close {
        right: 32px;
      }
      &__btn-back {
        left: 32px;
      }
      &__header {
        padding-bottom: 9px;
        border-bottom: 2px solid $border-color;
        margin-bottom: 32px;
      }
      &__control {
        display: flex;
        justify-content: space-between;
        align-items: center;
        .pt-button {
          border-radius: 12px;
        }
      }
    }
  }
  &.v--modal-overlay {
    z-index: 10002;
    background-color: fade-out($color-black, 0.5);
    .v--modal-background-click {
      padding-bottom: 0;
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .v--modal-box {
      overflow: visible;
      top: auto !important;
      left: auto !important;
    }
    &.scrollable {
      .v--modal-box {
        margin-bottom: 0;
      }
    }
  }
  .v--modal,
  .vm--modal {
    background-color: transparent;
    border-radius: 0;
    box-shadow: none;
    padding: 0;
  }
  &__header {
    padding-bottom: $space-16;
    position: relative;
    /*padding-right: 35px;*/
  }
  &__title {
    margin-top: 0;
    margin-bottom: 0;
    color: hsl(0, 0%, 13%);
  }
  &__inner {
    position: relative;
    box-shadow: 0 6px 12px rgba(110, 110, 110, 0.61);
    padding: $space-40 $space-28 $space-30 $space-24;
    background-color: $color-white;
    margin: 0 12px 0 12px;
    border-radius: $radius-8;
  }
  &__control {
    display: flex;
    padding-top: $space-30;
    margin-left: -3px;
    margin-right: -3px;
    &:empty {
      display: none;
    }
    .el-button,
    button {
      margin-left: 3px;
      margin-right: 3px;
    }
  }
}
</style>

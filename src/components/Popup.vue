<template>
  <div v-if="isOpen" class="overlay" @click.self="close">
    <div class="popup">
      <header>
        <h1>Warning!</h1>
      </header>
      <hr />

      <slot></slot>

      <hr />
      <footer>
        <slot name="actions" :confirm="confirm">
          <button @click="close">Cancel</button>
          <button @click="confirm">Ok</button>
        </slot>
      </footer>
    </div>
  </div>
</template>

<script>
export default {
  data: () => ({
    isOpen: false,
  }),
  mounted() {
    document.addEventListener('keydown', this.onKeyDown)
  },
  beforeDestroy() {
    document.removeEventListener('keydown', this.onKeyDown)
  },
  currentPopupController: null,
  methods: {
    open() {
      const popupPromise = new Promise((resolve, reject) => {
        this.$options.currentPopupController = { resolve, reject }
      })
      this.isOpen = true
      return popupPromise
    },
    close() {
      this.$options.currentPopupController.resolve(false)
      this.isOpen = false
    },
    confirm() {
      this.$options.currentPopupController.resolve(true)
      this.isOpen = false
    },
    onKeyDown(e) {
      if (this.isOpen && e.key === 'Escape') {
        this.close()
      }
    },
  },
}
</script>

<style>
.overlay {
  position: fixed;
  top: 0;
  bottom: 0;
  right: 0;
  left: 0;
  width: 100%;
  min-height: 100vh;
  background-color: rgba(0, 0, 0, 0.3);
  z-index: 100;
}

.popup {
  background-color: #ffffff;
  padding: 20px;
  z-index: 101;
  max-width: 400px;
  margin: 20vh auto;
  border-radius: 20px;
}
</style>

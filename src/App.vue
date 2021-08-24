<template>
  <div id="app">
    <button @click="openPopup">Open modal</button>
    <Popup ref="confirmationPopup">
      Do you really want to do this?

      <template #actions="{ confirm }">
        Please type <b>{{ $options.CONFIRMATION_TEXT }}</b>
        <input type="text" v-model="confirmation" @keydown.enter="confirm" />
        <button @click="confirm" :disabled="!isConfirmationCorrect">OK</button>
      </template>
    </Popup>
  </div>
</template>

<script>
import Popup from './components/Popup.vue'

export default {
  name: 'App',

  data: () => ({
    confirmation: '',
  }),

  components: {
    Popup,
  },

  CONFIRMATION_TEXT: 'i understand',

  computed: {
    isConfirmationCorrect() {
      return this.confirmation.toLowerCase() === this.$options.CONFIRMATION_TEXT
    },
  },

  methods: {
    async openPopup() {
      this.confirmation = ''

      const popupResult = await this.$refs.confirmationPopup.open()
      if (popupResult) {
        alert('Confirmed!')
      }
    },
  },
}
</script>

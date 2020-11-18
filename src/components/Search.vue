<template>
  <div class="uk-section uk-section-default">
    <div class="uk-container uk-container-large">

      <form
        class="uk-position-center"
        @submit.prevent="submitHandler"
      >
        <fieldset class="uk-fieldset">

          <legend class="uk-legend">Search</legend>

          <div class="uk-margin">
            <div class="uk-inline">
              <span
                class="uk-form-icon"
                uk-icon="icon: search"
              ></span>
              <input
                class="uk-input"
                type="text"
                v-model="inputValue"
              >
              <a
                v-if="inputValue.length"
                @click="clearInput"
                class="uk-form-icon uk-form-icon-flip"
                href="#"
                uk-icon="icon: close"
              ></a>
            </div>
          </div>

        </fieldset>
      </form>

    </div>
  </div>
</template>

<script>
export default {
  name: 'search',
  data: () => ({
    inputValue: '',
    isCurrentValue: false,
    alertOpen: false
  }),
  methods: {
    clearInput() {
      this.inputValue = ''
      this.isCurrentValue = false
    },
    submitHandler() {
      const value = this.inputValue
      switch(this.isCurrentValue) {
        case !this.validateEmail(value):
          alert('Вы искали Email')
          this.clearInput()
          break
        case !this.validatePhone(value):
          alert('Вы искали Телефон')
          this.clearInput()
          break
        case !this.validateNickname(value):
          alert('Вы искали Никнейм')
          this.clearInput()
          break
        case !this.validateNameFam(value):
          alert('Вы искали Имя и Фамилию')
          this.clearInput()
          break
        default:
          alert('Мы ничего не нашли :(')
          this.clearInput()
          break
      }
    },
    validateEmail(value) {
      const re = /^[\w]{1}[\w-\.]*@[\w-]+\.[a-z]{2,4}$/i
      return this.isCurrentValue = re.test(value)
    },
    validatePhone(value) {
      const re = /^((8|\+[1-9])[\- ]?)?(\(?\d{3}\)?[\- ]?)?[\d\- ]{7,10}$/
      return this.isCurrentValue = re.test(value)
    },
    validateNickname(value) {
      const re = /^(?!.*\.\.)(?!\.)(?!.*\.$)(?!\d+$)[a-zA-Z0-9.]{5,50}$/
      return this.isCurrentValue = re.test(value)
    },
    validateNameFam(value) {
      const re = /^([a-zA-Z]{2,}\s[a-zA-Z]{1,}'?-?[a-zA-Z]{2,}\s?([a-zA-Z]{1,})?)/
      return this.isCurrentValue = re.test(value)
    },
  }
}
</script>

<style lang="scss">
  .uk-section {
    min-height: 100vh;

    & .uk-input {
      width: 400px;
    }
  }
</style>

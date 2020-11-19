<template>
  <div class="uk-section uk-section-default">
    <div class="uk-container uk-container-large">

      <form
        @submit.prevent="submitHandler"
        class="uk-position-center"
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
                @input="debounceInput"
              >
              <a
                v-if="inputValue.length"
                @click="clearInput"
                class="uk-form-icon uk-form-icon-flip"
                href="#"
                uk-icon="icon: close"
              ></a>
            </div>
            <div
              class="uk-alert"
              :class="{'visible': hasMessage}"
            >
              {{message || ''}}
            </div>

          </div>

        </fieldset>
      </form>

    </div>
  </div>
</template>

<script>
  const debounce = require('lodash.debounce')

  export default {
    name: 'search',
    data: () => ({
      inputValue: '',
      isCurrentValue: false,
      message: ''
    }),
    methods: {
      clearInput() {
        this.inputValue = ''
        this.isCurrentValue = false
        this.message = ''
      },
      submitHandler() {
        this.isCurrentValue = false
        const value = this.inputValue
        switch (this.isCurrentValue) {
          case !this.validateEmail(value):
            this.message = 'Вы искали Email'
            break
          case !this.validatePhone(value):
            this.message = 'Вы искали Телефон'
            break
          case !this.validateNickname(value):
            this.message = 'Вы искали Никнейм'
            break
          case !this.validateNameFam(value):
            this.message = 'Вы искали Имя и Фамилию'
            break
          default:
            this.message = 'Мы ничего не нашли :('
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
      }
    },
    computed: {
      debounceInput: function () {
        return debounce(this.submitHandler, 1000)
      },
      hasMessage() {
        return (this.inputValue && this.message)
      }
    }
  }
</script>

<style lang="scss">
  .uk-section {
    min-height: 100vh;

    & .uk-input,
    .uk-alert {
      box-sizing: border-box;
      width: 400px;
    }

    & .uk-alert {
      min-height: 54px;
      opacity: 0;

      &.visible {
        opacity: 1;
      }
    }
  }
</style>

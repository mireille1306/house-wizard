<template>
  <div class="wizard">
    <StepIndicator :valid-steps="validSteps" :current-step="stepIndex" :total="3" />
    <form v-if="!finished" @submit.prevent>
      <div class="step-grid" v-show="stepIndex === 0" :class="{'slide-in' : stepIndex === 0}">
        <Card title="Ik wil verkopen" animationName="sale" @click.native="setUserInputType('sale')" />
        <Card title="Ik zoek een woning" animationName="search" @click.native="setUserInputType('search')" />
      </div>
      <div class="step-bg" v-show="stepIndex === 1" :class="{'slide-in' : stepIndex === 1}">
        <fieldset>
          <div>
            <input type="text" placeholder="Beoogde vraagprijs" @keypress="isNumber($event)" :class="{'invalid': showValidation && !validateTextInput(userInput.price)}" v-if="userInput.type === 'sale'" v-model="userInput.price" />
            <input type="text" placeholder="Zoekbudget" @keypress="isNumber($event)" :class="{'invalid': showValidation && !validateTextInput(userInput.price)}" v-if="userInput.type === 'search'" v-model="userInput.price" />
            <span class="text-invalid" :class="{'active' : showValidation && !validateTextInput(userInput.price)}">Vul een grotere waarde in</span>
          </div>
          <div>
            <button @click="setNextStep(2)">Next</button>
          </div>
        </fieldset>
      </div>
      <div class="step-bg" v-show="stepIndex === 2" :class="{'slide-in' : stepIndex === 2}">
        <fieldset class="user-input-grid">
          <div>
            <input type="text" placeholder="Voornaam" :class="{'invalid': showValidation && !validateTextInput(userInput.user.firstname)}" v-model="userInput.user.firstname">
            <span class="text-invalid" :class="{'active' : showValidation && !validateTextInput(userInput.user.firstname)}">Je voornaam lijkt niet te kloppen</span>
          </div>
          <div>
            <input type="text" placeholder="Achternaam" :class="{'invalid': showValidation && !validateTextInput(userInput.user.lastname)}" v-model="userInput.user.lastname">
            <span class="text-invalid" :class="{'active' : showValidation && !validateTextInput(userInput.user.lastname)}">Je achternaam lijkt niet te kloppen</span>
          </div>
          <div>
            <input type="text" placeholder="E-mailadres" :class="{'invalid': showValidation && !validateEmailInput(userInput.user.email)}" v-model="userInput.user.email">
            <span class="text-invalid" :class="{'active' : showValidation && !validateEmailInput(userInput.user.email)}">Vul een geldig e-mailadres in</span>
          </div>
          <div>
            <input type="text" placeholder="Telefoonnummer" :class="{'invalid': showValidation && !validateTextInput(userInput.user.phone)}" v-model="userInput.user.phone">
            <span class="text-invalid" :class="{'active' : showValidation && !validateTextInput(userInput.user.phone)}">Je telefoonnummer lijkt niet te kloppen</span>
          </div>
          <div>
            <button @click="sendForm(3)">Verzenden</button>
          </div>
        </fieldset>
      </div>
    </form>
    <div class="step-bg thanks" :class="{'slide-in' : finished}" v-if="finished">
      <h2>Bedankt <strong>{{ userInput.user.firstname}}</strong> voor het invullen van je gegevens!</h2>
      <p>We hebben een mail gestuurd naar <strong>{{ userInput.user.email }}</strong>. Check je mailbox voor de vervolgstappen.</p>
    </div>
  </div>
</template>

<script>
  import Card from './Card';
  import StepIndicator from './StepIndicator';

  export default {
    components: {
      Card,
      StepIndicator
    },
    data () {
      return {
        stepIndex: 0,
        userInput: {
          type: null,
          price: null,
          user: {
            firstname: '',
            lastname: '',
            email: '',
            phone: ''
          }
        },
        showValidation: false,
        finished: false
      }
    },
    computed: {
      // The steps become valid when all fields valid
      validSteps () {
        return {
          0: this.userInput.type !== null,
          1: this.validateTextInput(this.userInput.price),
          2: Object.keys(this.userInput.user).every(key => this.validateField(key, this.userInput.user[key]))
        }
      }
    },
    methods: {
      /**
       * Set the type of wizard in the first step
       * @param value {string}: the type
       */
      setUserInputType (value) {
        this.userInput['type'] = value;
        this.setNextStep(1);
      },

      /**
       * Go to the next step
       * @param stepIndex {number}: the current step index
       */
      setNextStep(stepIndex) {
        if (this.validSteps[stepIndex - 1] ) {
          this.stepIndex = this.stepIndex + 1;
          this.showValidation = false;
        } else {
          this.showValidation = true;
        }
      },

      /**
       * Validate a single field
       * Used when user clicks on the button
       * @param key {string}: key of field
       * @parm field {string}: the field value
       */
      validateField (key, field) {
        return (key === 'email') ? this.validateEmailInput(field) : this.validateTextInput(field);
      },

      /**
       * Validate a text input field
       * @param field {string}: the value of the field
       */
      validateTextInput (field) {
        return field && field.length > 0 ? true : false;
      },

        /**
       * Validate an email input field
       * @param field {string}: the value of the field
       */
      validateEmailInput (field) {
        return /^[^\s@]+@([^\s@.,]+\.)+[^\s@.,]{2,}$/g.test(field);
      },

      /**
       * Send the form. Currently this will not actually send anything
       */
      sendForm () {
        if (Object.keys(this.validSteps).every(key => this.validSteps[key])) {
          this.finished = true;
        } else {
          this.showValidation = true;
        }
      },

      /**
       * Check if a users key input is a number
       * @param event {event}: key binding event
       */
      isNumber (event) {
        event = (event) ? event : window.event;
        const charCode = (event.which) ? event.which : event.keyCode;
        if ((charCode > 31 && (charCode < 48 || charCode > 57)) && charCode !== 46) {
          event.preventDefault();
        } else {
          return true;
        }
      }
    }
  }
</script>

<style lang="scss" scoped>

  .step-grid {
    display: grid;
    gap: 1rem;
    grid-template-columns: repeat(2, 1fr);
  }

  .step-bg {
    box-shadow: 0 0 4px rgba(0,0,0,0.25);
    background: var(--white);
    padding: 2rem;
    border-radius: 10px;
  }

  .thanks {

    h2 {
      font-size: 2rem;
    }

    p {
      font-size: 1.25rem;
      margin-bottom: 0;
    }
  }

  fieldset {
    display: grid;
    justify-content: flex-start;
    gap: 1.75rem;
    padding: 0;
    border: 0;
  }

  .user-input-grid {
    grid-template-columns: repeat(2, 1fr);
  }

  .slide-in {
    animation: slideIn .5s forwards;
  }

  @keyframes slideIn {
    0% {
      opacity: 0;
      transform: translateY(-2rem);
    }
    100% {
      opacity: 1;
      transform: translateY(0);
    }
  }
</style>
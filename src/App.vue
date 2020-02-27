<template>
  <div id="app">
    <div id="background">
      <div id ="jumbotron">
        <div id="intern-content" v-if="!show">
          <b-jumbotron header="Survey Form" lead="Complete our survey form and have a chance to win prizes!">
            <p>Click on the button below to start the form!</p>  
            <b-button variant="primary" href="#" v-on:click="show = !show">Start!</b-button>
          </b-jumbotron>
        </div>
      <transition name="fade">
        <div v-if="show">
        <h2> Survey Form </h2>
        <b-jumbotron>
          <b-form @submit="onSubmit" @reset="onReset" >
              <b-form-group
                id="fieldset-1"
                description="Let us know your name."
                label="Enter your name"
                label-for="input-1"
                :invalid-feedback="invalidFeedback"
                :valid-feedback="validFeedback"
                :state="state"
                >
                  <b-form-input id="input-1" v-model="form.name" :state="state" trim></b-form-input>
                </b-form-group>
                <b-form-group
                id="fieldset-2"
                description="Let us know your email. We'll never share it with anyone else."
                label="Enter your email"
                label-for="input-2"
                :invalid-feedback="invalidFeedback"
                :valid-feedback="validFeedback"
                :state="state"
                >
                  <b-form-input id="input-2" type="email" v-model="form.email" :state="state" trim></b-form-input>
                </b-form-group>
                <b-form-group
                id="fieldset-3"
                description="Let us know your password."
                label="Enter your password"
                label-for="password"
                :invalid-feedback="invalidFeedback"
                :valid-feedback="validFeedback"
                :state="validation"
                >
                  <b-input v-model="form.password" type="password" :state="validation" id="input-3"></b-input>
                  <p id="warning-long">
                    Your password must be 5-12 characters long.
                  </p>


                </b-form-group>
                <b-form-group
                id="fieldset-4"
                description="Let us know your password again."
                label="Enter your password again"
                label-for="input-1"
                :invalid-feedback="invalidFeedback"
                :valid-feedback="validFeedback"
                :state="validation"
                >
                  <b-form-input id="input-4" type="password" v-model="form.passwordRep" :state="validation" trim></b-form-input>
                  <p id="warning">
                    Your passwords don't match.
                  </p>
                </b-form-group>

              <b-form-group id="input-group-5" label="You are a:" label-for="input-3">
                <b-form-select
                  id="input-5"
                  v-model="form.role"
                  :options="roles"
                  required
                ></b-form-select>
              </b-form-group>

              <b-form-group id="input-group-6">
                <b-form-checkbox-group v-model="form.checked" id="checkboxes-4">
                  <b-form-checkbox value="me">I want to receive emails with offers.</b-form-checkbox>
                </b-form-checkbox-group>
              </b-form-group>


              <b-button class="buttons-form" type="submit" variant="primary">Submit</b-button>
              <b-button class="buttons-form" type="reset" variant="danger">Reset</b-button>
            </b-form>

          </b-jumbotron>
        </div>
      </transition>
    </div>
  </div>
</div>
</template>

<script>
export default {
  name: 'app',
  props:['show'],
  data () {
    return {
      userId: '',
      msg: 'Welcome to Your Vue.js App',
      form: {
          email: '',
          name: '',
          role: null,
          checked: []
        },
        roles: [{ text: 'Select One', value: null }, 'Student', 'Teacher', 'Employee', 'Intern'],
        show: true
      }
    },
    methods: {
      onSubmit(evt) {
        evt.preventDefault()
        if(this.form.password.length > 4 && this.form.password.length < 13){
          document.getElementById("warning-long").style.display = 'none'
          if(this.form.password == this.form.passwordRep){
            document.getElementById("warning").style.display = 'none'
            alert(JSON.stringify(this.form))
          }
          else {
            document.getElementById("warning").style.display = 'block'
          }
        }
        else {
          document.getElementById("warning-long").style.display = 'block'
        }

      },
      onReset(evt) {
        evt.preventDefault()
        // Reset our form values
        this.form.email = ''
        this.form.name = ''
        this.form.password = ''
        this.form.passwordRep = ''
        this.form.role = null
        this.form.checked = []
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      }
  }
}
</script>

<style>

#warning{
  display: none;
  color: red;
}

#warning-long {
  display: none;
  color: red;
}

.buttons-form {
  margin-right: 10px;
  margin-left: 10px;
}

.fade-enter-active, .fade-leave-active {
  transition: opacity .5  s;
}
.fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
  opacity: 0;
}

#jumbotron {
  width: 50%;
  margin: 30px auto 50px auto;
}

body {
    background-color: #a9d7d1 !important;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  background-color: #a9d7d1;
}
</style>

<template>
  <modal
    title="Register" 
    @close="closeModal">
    <div slot="body">
       <form @submit.prevent="onSubmit">

          <div class="form-item" :class="{errorInput: $v.name.$error}">
            <label>Name:</label>
            <input 
              v-model="name"
              :class="{error: $v.name.$error}"
              @change="$v.name.$touch()">
            <p class="errorText" v-if="!$v.name.required && $v.name.$error">Field name is required</p>
            <p class="errorText" v-if="!$v.name.minLength && $v.name.$error">Min length name is {{$v.name.$params.minLength.min}}</p>
          </div>

          <div class="form-item" :class="{errorInput: $v.email.$error}">
            <label>Email:</label>
            <input 
              v-model="email"
              :class="{error: $v.email.$error}"
              @change="$v.email.$touch()">
            <p class="errorText" v-if="!$v.email.required && $v.email.$error">Field email is required</p>
            <p class="errorText" v-if="!$v.email.email && $v.email.$error">Incorrect email</p>
          </div>

          <div class="form-item" :class="{errorInput: $v.email.$error}">
            <label>Password:</label>
            <input
              type="password"
              v-model="password"
              :class="{error: $v.password.$error}"
              @change="$v.password.$touch()">
            <p class="errorText" v-if="!$v.password.required && $v.password.$error">Field password is required</p>
            <p class="errorText" v-if="!$v.password.minLength && $v.password.$error">
              Password must be at least {{ $v.password.$params.minLength.min }} characters
            </p>
          </div>

          <div class="form-item" :class="{errorInput: $v.repeatPassword.$error}">
            <label>Repeat Password:</label>
            <input
              type="password"  
              v-model="repeatPassword"
              :class="{error: $v.repeatPassword.$error}"
              @change="$v.repeatPassword.$touch()">
            <p class="errorText" v-if="!$v.repeatPassword.$sameAs && $v.repeatPassword.$error">Passwords must match</p>
          </div>

          <button class="btn btnPrimary">Submit</button>
        </form>
    </div>
    <div class="footer" slot="footer">
      <p class="footer-link" @click="$emit('switchModal')">I already have account</p>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email, sameAs } from 'vuelidate/lib/validators'
import modal from '@/components/UI/Modal.vue'

export default {
  components: {
    modal
  },
  data() {
    return {
     name: '',
     email: '',
     password: '',
     repeatPassword: ''
    }
  },
  methods: {
    onSubmit(){
      this.$v.$touch();

      if(!this.$v.$invalid){
        const data = {
          name: this.name,
          email: this.email,
          password: this.password
        }
        console.log(data);
        
        //close
        this.closeModal();
      }
    },
    closeModal(){
      this.$v.$reset();
      this.name = '';
      this.email = '';
      this.password = '';
      this.repeatPassword = '';
      this.$emit('close');
    }
  },
  validations: {
    name: {
      required,
      minLength: minLength(4)
    },
    email: {
      required,
      email
    },
    password: {
      required,
      minLength: minLength(5),
    },
    repeatPassword: {
      sameAs: sameAs('password')
    }
  }
}
</script>

<style lang="scss">
.form-item {
  .errorText {
    color: #de4040;
    margin-bottom: 15px;
    font-size: 13.4px;
  }

  input.error {
    border-color: #de4040;
  }
}
</style>

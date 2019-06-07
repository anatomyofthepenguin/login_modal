<template>
  <modal
    title="LogIn" 
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
              type="password" 
              v-model="email"
              :class="{error: $v.email.$error}"
              @change="$v.email.$touch()">
            <p class="errorText" v-if="!$v.email.required && $v.email.$error">Field email is required</p>
            <p class="errorText" v-if="!$v.email.email && $v.email.$error">Incorrect email</p>
          </div>
          <button class="btn btnPrimary">Submit</button>
        </form>
    </div>
    <div class="footer" slot="footer">
      <p class="footer-link" @click="$emit('switchModal')">I need an account</p>
    </div>
  </modal>
</template>

<script>
import { required, minLength, email } from 'vuelidate/lib/validators'
import modal from '@/components/UI/Modal.vue'

export default {
  components: {
    modal
  },
  data() {
    return {
     name: '',
     email: ''
    }
  },
  methods: {
    onSubmit(){
      this.$v.$touch();

      if(!this.$v.$invalid){
        const data = {
          name: this.name,
          email: this.email
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

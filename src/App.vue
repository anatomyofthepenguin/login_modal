<template>
<div class="wrapper">
  <div class="wrapper-content">
    <section>
      <div class="container">
        <button class="btn btnPrimary" @click="loginModal=true">LogIn</button>
        <button class="btn btnPrimary" @click="regModal=true">Register</button>
        <loginModal 
          @close="loginModal=false"
          @switchModal = "switchModal('loginModal', 'regModal')" 
          v-show="loginModal"/>
        <regModal 
          @close="regModal=false" 
          v-show="regModal"
          @switchModal = "switchModal('regModal','loginModal')"/>
      </div>
    </section>
  </div>
</div>
</template>

<script>
import loginModal from '@/components/LoginModal'
import regModal from '@/components/RegModal'

export default {
  components: {
    loginModal,
    regModal
  },
  data() {
    return {
      loginModal: false,
      regModal: false
    }
  },
  methods:{
    switchModal(closeModal, showModal){
      this[closeModal] = false;
      this[showModal] = true;
    }
  }
}
</script>
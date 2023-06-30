<template lang="pug">
h1 Whatsapp Business
div
    PhoneNumberForm(@messages-sent="handleMessagesSent")
    PhoneNumberList(:phone-number-list="phoneNumberList")
</template>

<script>
import PhoneNumberForm from '../components/PhoneNumberForm.vue'
import PhoneNumberList from '../components/PhoneNumberList.vue'

export default {
  name: 'App',
  components: {
    PhoneNumberForm,
    PhoneNumberList 
  },
  data() {
    return {
      phoneNumberList: []
    }
  },
  methods: {
    handleMessagesSent(phoneNumberList) {
      this.phoneNumberList = phoneNumberList
    }
  }
}
</script>

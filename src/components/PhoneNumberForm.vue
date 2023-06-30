<template lang="pug">
div.container
  div.row
    div.twelve.columns.mb-1
      label Nombre de la Plantilla:
      input(type="text" v-model="templateName")
    div.twelve.columns.mb-1
      label.label-file(for="file-upload") Cargar números de teléfono
      input#file-upload(type="file" ref="fileInput" accept=".json" @change="handleFileChange")
    div.twelve.columns.mb-1
      button.button-primary(@click="sendMessage") Enviar Mensajes
</template>

<script>
import { ref, onUnmounted, onMounted } from 'vue'
import axios from 'axios'

export default {
  emits: ['messages-sent'], // Declarar los eventos personalizados que puede emitir el componente hijo
  setup(_, { emit }) {
    const phoneNumberList = ref([])
    const newPhoneNumberList = ref([])
    const accessToken = process.env.VUE_APP_ACCESS_TOKEN
    const apiUrl = process.env.VUE_APP_API_URL

    const templateName = ref('')

    onMounted(() => {
      document.addEventListener('keydown', handleKeyDown);
    })

    const handleFileChange = (event) => {
      const file = event.target.files[0]
      const reader = new FileReader()

      reader.onload = () => {
        try {
          const jsonData = JSON.parse(reader.result)
          phoneNumberList.value = jsonData.numeros_telefono
          if (phoneNumberList.value.length) {
            phoneNumberList.value.forEach((item) => {
              newPhoneNumberList.value.push({ phoneNumber: item, status: 'Cargado', message: null })
            })
          }
          emit('messages-sent', newPhoneNumberList.value) // Emitir evento al componente padre
        } catch (error) {
          alert('Error al leer el archivo JSON:', error)
        }
      }

      file && reader.readAsText(file)
    }

    const changeStatusPhone = (number, newStatus, message) => {
      const phone = newPhoneNumberList.value.find((objeto) => objeto.phoneNumber === number)

      if (phone) {
        phone.status = newStatus
        phone.message = message
      }

      emit('messages-sent', newPhoneNumberList.value) // Emitir evento al componente padre
    }

     function handleKeyDown(event) {
      if (event.key === 'Enter') {
        event.preventDefault()
        sendMessage()
      }
    }

    const sendMessage = () => {
      if (phoneNumberList.value.length === 0) {
        alert('No se han cargado números de teléfono.')
        return
      }

      if (!templateName.value) {
        alert('Debe proporcionar el nombre de la plantilla.')
        return
      }

      const headers = {
        'Authorization': `Bearer ${accessToken}`,
        'Content-Type': 'application/json'
      }

      for (const phoneNumber of phoneNumberList.value) {
        const data = {
          messaging_product: 'whatsapp',
          to: '+503' + phoneNumber.toString(),
          type: 'template',
          template: {
            name: templateName.value, // Nombre de la plantilla proporcionado por el usuario
            language: { code: 'en_US' }
          }
        }

        axios.post(apiUrl, data, { headers })
          .then(() => {
            changeStatusPhone(phoneNumber, 'Enviado', 'Enviado')
          })
          .catch((error) => {
            changeStatusPhone(phoneNumber, 'Error', error.response.data?.error?.message || error.response.data)
          })
      }
    }

    // Limpiar los datos al desmontar el componente
    onUnmounted(() => {
      phoneNumberList.value = []
      newPhoneNumberList.value = []
      document.removeEventListener('keydown', handleKeyDown)
    })

    return {
      handleFileChange,
      sendMessage,
      templateName
    }
  }
}
</script>

<style scoped>
.mb-1 {
  margin-bottom: 10px;
}
/* Ocultar el input original */
input[type="file"] {
  display: none;
}

/* Estilo personalizado para el botón */
.label-file {
  display: inline-block;
  padding: 5px 10px;
  background-color: #f1f1f1;
  color: #333;
  border: 1px solid #ccc;
  border-radius: 4px;
  cursor: pointer;
}

/* Estilo para resaltar cuando el cursor está sobre el botón */
.label-file:hover {
  background-color: #ddd;
}

/* Estilo para resaltar cuando el botón está activo */
.label-file:active {
  background-color: #bfbfbf;
}
</style>

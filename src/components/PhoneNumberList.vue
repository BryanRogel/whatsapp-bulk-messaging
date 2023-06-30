<template lang="pug">
div.container.table-container
  table.table.u-full-width(v-show="phoneNumberList.length")
    thead
      tr
        th Número
        th Estado
    tbody
      tr(v-for="(item, index) in phoneNumberList" :key="index" style="list-style: none")
        td(:class="getStatusClass(item.status)") {{ item.phoneNumber }}
        td(:class="getStatusClass(item.status)") {{ item.message }}
</template>

<script>

export default {
  props: {
    phoneNumberList: {
      type: Array,
      required: true
    }
  },
  setup() {
    const getStatusClass = (status) => {
      return {
        'green': status === 'Enviado',
        'red': status === 'Error'
      }
    }

    return {
      getStatusClass
    }
  }
}
</script>

<style scoped>
div {
  height: 80px;
  max-height: 20px;
  min-height: 50px;
}
td, th {
  text-align: center;
}
.green {
  background: #50b144;
  color: white;
}
.red {
  background: #db3434;
  color: white;
}
.table-container {
  min-height: 200px;
  overflow-y: auto;
}
.table {
  width: 100%;
}
</style>

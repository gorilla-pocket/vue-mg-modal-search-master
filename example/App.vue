<template>
  <section class="container mt-2">
    <modal-search-master v-model="master" @search="onSearch"/>
    id: {{master.id}}
    code: {{master.code}}
    name: {{master.name}}
  </section>
</template>

<script>
import ModalSearchMaster from '../src/vue-mg-modal-search-master'
import 'bootstrap/dist/css/bootstrap.min.css'
export default {
  data() {
    return {
      master: {
        id: 1,
        code: '001',
        name: 'aaaa',
      },
      items: [],

      isLoading: false,
    }
  },
  methods: {
    onSearch: function (code, name, callback) {
      let data = [
        {id: 1, code: '10', name: 'aaaa'},
        {id: 2, code: '20', name: 'bbbb'},
      ]
      callback(data)
    },
  },
  components: {
    ModalSearchMaster,
  },  
}
</script>
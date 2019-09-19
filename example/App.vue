<template>
  <section class="container mt-2">
    <modal-search-master v-model="master" :label="label" @search="onSearch" @find="onFind"/>
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
      label: '',

      isLoading: false,
    }
  },
  methods: {
    onSearch: function (code, name, callback) {
      const data = []
      for (let i = 0; i < 50; i++) {
        data.push({
          id: i+1, code: '10', name: 'aaaa'
        })
      }
      callback(data)
    },
    onFind: function (code, callback) {
      const list = [
        {id: 1, code: '10', name: 'aaaa'},
        {id: 2, code: '20', name: 'bbbb'},
        {id: 3, code: '30', name: 'cccc'},
      ]
      const data = list.find(function (value) {
        return value.code == code
      })
      callback(data)
    },
  },
  components: {
    ModalSearchMaster,
  },  
}
</script>
<template>
  <div>
    <section class="container mt-2">
      <modal-search-master v-model="master" :label="label" @search="onSearch" @find="onFind">
        <template v-slot:search>
          <div class="mt-2">
            <div class="row">
                <div class="col-sm-2 align-self-center">追加の検索条件</div>
                  <div class="col-sm-10">
                      <input type="text" class="form-control">
                  </div>
              </div>
          </div>
        </template>
      </modal-search-master>
      id: {{master.id}}
      code: {{master.code}}
      name: {{master.name}}
    </section>
    <section class="container mt-2">
      <modal-search-master v-model="master2" :label="label" @search="onSearch" @find="onFind">
        <template v-slot:search>
          <div class="mt-2">
            <div class="row">
                <div class="col-sm-2 align-self-center">追加の検索条件</div>
                  <div class="col-sm-10">
                      <input type="text" class="form-control">
                  </div>
              </div>
          </div>
        </template>
      </modal-search-master>
      id: {{master2.id}}
      code: {{master2.code}}
      name: {{master2.name}}
      other: {{master2.other}}
    </section>
    <input type="button" @click="click">
  </div>
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
      master2: {
        id: '',
        code: '',
        name: '',
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
          id: i+1, code: '10', name: 'aaaa', other: 'bbbb',
        })
      }
      callback(data)
    },
    onFind: function (code, callback) {
      const list = [
        {id: 1, code: '10', name: 'aaaa', other: 'other1' },
        {id: 2, code: '20', name: 'bbbb', other: 'other2'},
        {id: 3, code: '30', name: 'cccc'},
      ]
      const data = list.find(function (value) {
        return value.code == code
      })
      callback(data)
    },
    click: function () {
      this.master2 =  {
        id: '3',
        code: '233',
        name: 'あああ',
      }
    }
  },
  components: {
    ModalSearchMaster,
  },  
}
</script>
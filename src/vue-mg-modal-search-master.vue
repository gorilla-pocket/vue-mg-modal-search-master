<template>
<div>
    <div class="d-flex">
        <div class="align-self-center mr-3" v-if="label">
            {{label}}
        </div>
        <div>
            <div class="input-group">
                <input type="input" class="form-control" style="width:8rem;" v-model="code" @blur="onFind">
                <div class="input-group-append">
                    <button class="btn btn-primary" type="button" id="button-addon2" @click="show"><i class="fas fa-search"></i></button>
                </div>
            </div>
        </div>
        <div class="align-self-center ml-2">
            {{selected_name}}
        </div>
    </div>
    <modal-dialog :show="showModal" @close="showModal=false">
        <template v-slot:header>
            <div class="h5 mb-0">{{title}}</div>
        </template>
        <template v-slot:body>
            <slot name="body">
                <div class="">
                    <div class="row">
                        <div class="col-sm-2 align-self-center">コード</div>
                        <div class="col-sm-10">
                            <input type="text" class="form-control" v-model="search_code">
                        </div>
                    </div>
                </div>
                <div class="mt-2">
                    <div class="row">
                        <div class="col-sm-2 align-self-center">名前</div>
                        <div class="col-sm-10">
                            <input type="text" class="form-control" v-model="search_name">
                        </div>
                    </div>
                </div>
                <div class="mt-2">
                    <div class="d-flex justify-content-end">
                        <button class="btn btn-primary" type="button" :disabled="!search_code&&!search_name" @click="onSearch">検索</button>
                    </div>
                </div>
                <div class="mt-3">
                    <table class="table table-sm">
                        <thead>
                            <th class="text-center">コード</th>
                            <th class="text-center">名前</th>
                        </thead>
                        <tbody>
                            <tr v-for="(item, index) in items" :key="index" @click="onClick(index)" :class="{ 'selected-row': item.selected }">
                                <td>{{item.code}}</td>
                                <td>{{item.name}}</td>
                            </tr>
                        </tbody>
                    </table>
                </div>
            </slot>
        </template>
        <template v-slot:footer>
            <button class="btn btn-primary" :disabled="!is_selected" @click="ok">ＯＫ</button>
            <button class="btn btn-secondary" @click="showModal=false">キャンセル</button>
        </template>
    </modal-dialog>
</div>
</template>

<script>
import ModalDialog from 'vue-mg-modal-dialog'
export default {
    props: {
        title: {
            default: '検索',
        },
        label: {
            default: '',
        },
        value: {
            default: {
                id: null,
                code: '',
                name: '',
            }
        }
    },
    data () {
        return {
            items: [],
            showModal: false,
            code: '',
            search_code: '',
            search_name: '',
            selected: {
                id: null,
                code: '',
                name: '',
            },
            selected_name: 'test',
            is_selected: false,
        }
    },
    mounted: function () {
        this.items = []
        this.code = this.value.code
        this.selected_name = this.value.name
    },
    watch: {
        //
    },
    methods: {
        show: function () {
            this.init()
            this.showModal = true
        },
        init: function () {
            this.items = []
            this.selected.id = null
            this.selected.code = this.code
            this.selected.name = ''
            this.search_code = ''
            this.search_name = ''
            this.is_selected = false
        },
        onSearch: function () {
            // this.isLoading = true
            this.$emit('search', this.search_code, this.search_name, this.callback)
        },
        callback: function (data) {
            this.is_selected = false
            this.items = data
        },
        onFind: function () {
            this.$emit('find', this.code, this.callbackFind)
        },
        callbackFind: function (data) {
            let master = {
                id: null,
                code: '',
                name: '',
            }
            if (data) {
                master = {
                    id: data.id,
                    code: data.code,
                    name: data.name
                }
            }
            this.selected_name = master.name
            this.$emit('input', master)
        },
        ok: function () {
            this.showModal = false
            let master = {
                id: this.selected.id,
                code: this.selected.code,
                name: this.selected.name
            }
            this.code = this.selected.code
            this.selected_name = this.selected.name
            this.$emit('input', master)
        },
        onClick: function (index) {
            this.is_selected = true
            let item = this.items[index]

            this.selected.id = item.id
            this.selected.code = item.code
            this.selected.name = item.name

            this.items.forEach(function (value) {
                value.selected = false
            })
            item.selected = true
            this.items.splice(index, 1, item)
        },
    },
    components: {
        ModalDialog,
    }
}
</script>

<style scope>
.selected-row {
    background-color: #ffecb3 !important;
}
</style>
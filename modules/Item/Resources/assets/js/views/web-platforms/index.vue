<template>
    <div class="card card-collapsed">
        <div class="card-header bg-info">
            <h3 class="my-0">Plataformas 
            </h3>
            <div class="card-actions white-text">
                <a href="#" class="card-action card-action-toggle text-white" data-card-toggle=""></a>
                <a href="#" class="card-action card-action-dismiss text-white" data-card-dismiss=""></a>
            </div>
        </div>
        <div class="card-body">
            <div class="row">
                <div class="col">
                    <button type="button" class="btn btn-custom btn-sm  mt-2 mr-2" @click.prevent="clickCreate()"><i class="fa fa-plus-circle"></i> Nuevo</button>
                </div>
            </div>
            <div class="table-responsive">
                <table class="table">
                    <thead>
                    <tr>
                        <th>#</th>
                        <th>Nombre</th>
                        <th class="text-right">Acciones</th>
                    </tr>
                    </thead>
                    <tbody>
                    <tr v-for="(row, index) in records" :key="index">
                        <td>{{ index + 1 }}</td>
                        <td>{{ row.name }}</td>
                        <td class="text-right">
                        
                            <button type="button" class="btn waves-effect waves-light btn-xs btn-info" @click.prevent="clickCreate(row.id)">Editar</button>
                            <template v-if="typeUser === 'admin'">
                                <button type="button" class="btn waves-effect waves-light btn-xs btn-danger"  @click.prevent="clickDelete(row.id)">Eliminar</button>
                            </template>

                        </td>
                    </tr>
                    </tbody>
                </table>
            </div>
            <!-- <div class="row">
                <div class="col">
                    <button type="button" class="btn btn-custom btn-sm  mt-2 mr-2" @click.prevent="clickCreate()"><i class="fa fa-plus-circle"></i> Nuevo</button>
                </div>
            </div> -->
        </div>
        <web-platforms-form :showDialog.sync="showDialog"
                         :recordId="recordId"></web-platforms-form>
    </div>
</template>

<script>

    import WebPlatformsForm from './form.vue'
    import {deletable} from '@mixins/deletable'

    export default {
        mixins: [deletable],
        props: ['typeUser'],
        components: {WebPlatformsForm},
        data() {
            return {
                showDialog: false,
                resource: 'web-platforms',
                recordId: null,
                records: [],
            }
        },
        created() {
            this.$eventHub.$on('reloadData', () => {
                this.getData()
            })
            this.getData()
        },
        methods: {
            getData() {
                this.$http.get(`/${this.resource}/records`)
                    .then(response => {
                        this.records = response.data
                    })
            },
            clickCreate(recordId = null) {
                this.recordId = recordId
                this.showDialog = true
            },
            clickDelete(id) {
                this.destroy(`/${this.resource}/${id}`).then(() =>
                    this.$eventHub.$emit('reloadData')
                )
            }
        }
    }
</script>

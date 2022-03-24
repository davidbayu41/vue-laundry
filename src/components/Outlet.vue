<template>
    <div>
        <main>
            <div class="container-fluid px-4">
                <h1 class="mt-4">Data Outlet</h1>
                <div class="card mb-4">
                    <div class="card-body">
                        <a class="btn btn-success" v-b-modal.modal_outlet @click="Add()">
                            <span class="icon text-white-50">
                                <i class="fas fa-plus"></i>
                            </span>
                            <span class="text">Tambah</span>
                        </a>
                        <table class="table">
                            <tr>
                                <td>ID</td>
                                <td>NAMA OUTLET</td>                                
                                <td>ACTION</td>
                            </tr>
                            <!-- <tr v-for="tlet in outlet" :key="tlet">
                                <td>{{ tlet.id_outlet }}</td>
                                <td>{{ tlet.nama_outlet }}</td>
                                <td>
                                    <a v-b-modal.modal_outlet href="#" class="btn btn-info" @click="Edit(tlet)">Ubah</a>
                                    <a href="#" class="btn btn-danger" @click="Delete(tlet.id_outlet)">Hapus</a>
                                </td>
                            </tr> -->
                            <tr v-for="(tlet, index) in outlet" :key="index">
                                <td>{{ index + 1 }}</td>                        
                                <td>{{ tlet.nama_outlet }}</td>
                                <!-- <td class="text-white">
                                    <span v-if="mem.jenis_kelamin === 'l'" class="badge bg-gradient-primary">Laki-Laki</span>
                                    <span v-if="mem.jenis_kelamin === 'p'" class="badge bg-danger">Perempuan</span>
                                </td> -->                                
                                <td>
                                    <a v-b-modal.modal_outlet href="#" class="btn btn-info btn-icon-split text-light mr-2" @click="Edit(tlet)">
                                        <span class="icon text-white-50">
                                            <i class="fas fa-edit"></i>
                                        </span>
                                        <!-- <span class="text">Ubah Data</span> -->
                                    </a>
                                    <a href="#" class="btn btn-danger" @click="Delete(tlet.id_outlet)"><i class="fas fa-fw fa-trash"></i></a>
                                </td>
                            </tr>
                        </table>
                    </div>
                </div>
            </div>
        </main>

        <b-modal id="modal_outlet" ref="modal" title="Form Outlet" size="md" @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="nama_outlet" placeholder="Enter your first name" v-modal="nama_outlet" id="inputOutlet" class="form-control" type="text"/>
                    <label for="inputOutlet">Nama Outlet</label>
                </div>               
            </form>
        </b-modal>
    </div>
</template>
<script>
module.exports =  {
    data: function(){
        return {
            id_outlet: "",
            nama_outlet: "",
            outlet: [],
            action:""
        }
    },
    methods: {
        getData: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

          axios.get(base_url + '/outlet', config)
          .then( response => {
              console.log(response);
            if(response.data.success == true){
                this.outlet = response.data.data.outlet;
            }
          })

        },
        Add: function(){
            this.action = "insert";
            this.id_outlet = "";
            this.nama_outlet = "";
        },
        Edit: function(item){
            this.action = "update";
            this.id_outlet = item.id_outlet;
            this.nama_outlet = item.nama_outlet;
        },
        Save: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

            let form = {
                "id_outlet": this.id_outlet,
                "nama_outlet": this.nama_outlet,
            }

            //logika method post/get (insert /update)
            if(this.action == "insert"){
                axios.post(base_url + '/outlet', form, config)
                .then( response => {
                    alert(response.data.message);
                })
            } else { //update
                axios.put(base_url + '/outlet/' + this.id_outlet, form, config)
                .then( response => {
                    alert(response.data.message);
                })
            }
            
            this.getData();
            
        },
        Delete: function(id_outlet){
           if(confirm("Apakah anda yakin menghapus data outlet ini?")){

                let config = {
                    headers : {
                    "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                    }
                }

                axios.delete(base_url + '/outlet/' + id_outlet, config)
                .then( response => {
                    alert(response.data.message);
                })

                this.getData();
           }

            // Swal.fire({
            //     title: 'Error!',
            //     text: 'Do you want to continue',
            //     icon: 'error',
            //     confirmButtonText: 'Cool'
            // })
        }

    },
    mounted() {
        this.getData();
    },
}
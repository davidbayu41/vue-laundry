<template>
    <div>
        <main>
            <div class="container-fluid px-4">
                <h1 class="mt-4">Data Paket</h1>
                <div class="card mb-4">
                    <div class="card-body">
                        <a class="btn btn-success" v-b-modal.modal_paket @click="Add()">
                            <span class="icon text-white-50">
                                <i class="fas fa-plus"></i>
                            </span>
                            <span class="text">Tambah</span>
                        </a>
                        <table class="table">
                            <tr>
                                <td>ID</td>
                                <td>JENIS PAKET</td>
                                <td>HARGA</td>
                                <td>ACTION</td>
                            </tr>
                            <tr v-for="ket in paket" :key="ket">
                                <td>{{ ket.id_paket }}</td>
                                <td>{{ ket.jenis_paket }}</td>
                                <td>{{ ket.harga }}</td>
                                <td>
                                    <a v-b-modal.modal_paket href="#" class="btn btn-info" @click="Edit(ket)">Ubah</a>
                                    <a href="#" class="btn btn-danger" @click="Delete(ket.id_paket)">Hapus</a>
                                </td>
                            </tr>
                        </table>
                    </div>
                </div>
            </div>
        </main>

        <b-modal id="modal_paket" ref="modal" title="Form Paket" size="md" @ok="Save">
            <form>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="jenis_paket" placeholder="Masukkan Jenis Paket" v-modal="jenis_paket" id="inputPaket" class="form-control" type="text"/>
                    <label for="inputPaket">Jenis Paket</label>
                </div>
                <div class="form-floating mb-3 mb-md-0">
                    <input v-model="harga" placeholder="Masukkan Harga" v-modal="harga" id="inputHarga" class="form-control" type="int"/>
                    <label for="inputHarga">Harga</label>
                </div>
            </form>
        </b-modal>
    </div>
</template>
<script>
module.exports =  {
    data: function(){
        return {
            id_paket: "",
            jenis_paket: "",
            harga:"",
            paket: [],
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

          axios.get(base_url + '/paket', config)
          .then( response => {
              console.log(response);
            if(response.data.success == true){
                this.paket = response.data.data.paket;
            }
          })

        },
        Add: function(){
            this.action = "insert";
            this.id_paket = "";
            this.jenis_paket = "";
            this.harga = "";
        },
        Edit: function(item){
            this.action = "update";
            this.id_paket = item.id_paket;
            this.jenis_paket = item.jenis_paket;
            this.harga = item.harga;
        },
        Save: function(){
            let config = {
                headers : {
                "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                }
            }

            let form = {
                "id_paket": this.id_paket,
                "jenis_paket": this.jenis_paket,
                "harga": this.harga,
            }

            //logika method post/get (insert /update)
            if(this.action == "insert"){
                axios.post(base_url + '/paket', form, config)
                .then( response => {
                    alert(response.data.message);
                })
            } else { //update
                axios.put(base_url + '/paket/' + this.id_paket, form, config)
                .then( response => {
                    alert(response.data.message);
                })
            }
            
            this.getData();
            
        },
        Delete: function(id_paket){
           if(confirm("Apakah anda yakin menghapus data paket ini?")){

                let config = {
                    headers : {
                    "Authorization" : "Bearer " + this.$cookies.get('Authorization')
                    }
                }

                axios.delete(base_url + '/paket/' + id_paket, config)
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
</script>
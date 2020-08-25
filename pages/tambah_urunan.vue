<template>
    <div class="container">
        <div class="row align-items-center justify-content center">
            <div class="col-12 col-lg-10 col-md-10">
                <div class="card mt-5">
                    <div class="card-header">
                        <h5 class="pm">Tambah Urunan</h5>
                    </div>
                    <div class="card-body">
                        <form @submit.prevent="submit()">
                            <div class="form-group">
                                <label for="id_user">User</label>
                                <select name="id_user" id="id_user" class="form-control" required autofocus v-model="form.id_user">
                                    <option value="" disabled selected>Orang yang mau urunan</option>
                                    <option :value="user.id" v-for="user in user" :key="user.id">{{ user.nama }}</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="nominal">Nominal Urunan</label>
                                <select name="id_user" id="id_user" class="form-control" required v-model="form.nominal">
                                    <option value="" disabled selected>Nominal</option>
                                    <option value="500">Rp 500</option>
                                    <option value="1000">Rp 1000</option>
                                    <option value="2000">Rp 2000</option>
                                    <option value="3000">Rp 3000</option>
                                    <option value="4000">Rp 4000</option>
                                    <option value="5000">Rp 5000</option>
                                    <option value="6000">Rp 6000</option>
                                    <option value="7000">Rp 7000</option>
                                    <option value="8000">Rp 8000</option>
                                    <option value="9000">Rp 9000</option>
                                    <option value="10000">Rp 10000</option>
                                </select>
                            </div>
                            <div class="form-group">
                                <label for="kode">Kode Form</label>
                                <input type="password" class="form-control" id="kode" required v-model="form.kode"  placeholder="kode form">
                            </div>
                            <div class="form-group">
                                <button class="btn btn-primary" type="submit" :class="{disabled: issubmit}">Simpan</button>
                            </div>
                        </form>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    data(){
        return {
            form: {
                id_user: '',
                nominal: '',
                kode: ''
            },
            user: [],
            issubmit: false,
        }
    },
    methods: {
        getUser: function(){
            this.$axios.get('pengguna/semua')
            .then( res => {
                this.user = res.data.hasil
            })
        },
        submit: function(){
            if(!this.issubmit){
                if(this.form.kode == 'VOLIKUY'){
                    this.issubmit = true
                    this.$axios.post('urunan/tambah', {
                        id_user: this.form.id_user,
                        nominal: this.form.nominal
                    })
                    .then(res => {
                        if(res.status == 201){
                            alert('data berhasil disimpan')
                            this.issubmit = false
                            this.form= {
                                id_user: '',
                                nominal: ''
                            }
                        } else {
                            this.issubmit = false
                            alert('data gagal disimpan')
                        }
                    })
                } else {
                    alert('kode salah')
                }
            }
        }
    },
    created(){
        this.getUser()
    }
}
</script>

<style>

</style>
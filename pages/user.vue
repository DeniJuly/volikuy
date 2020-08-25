<template>
  <div class="page">
    <!-- navbar -->
    <Navbar />
    <div class="container">
      <div class="row align-items-center justify-content-center">
        <!-- list -->
        <div class="col-12 col-lg-10 col-md-10 list-pembayaran">
          <h5 class="pm mt-3">List Pembayaran</h5>
          
          <card-user :data="data" v-for="data in user" :key="data.id" />

        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '../components/Navbar';
import CardUser from '../components/cardUser';

export default {
  data(){
    return{
      user: []
    }
  },
  methods: {
    getUserAll: function(){
      this.$axios.get(`pengguna/semua`)
      .then(res => {
        this.user = res.data.hasil
      })
    },
  },
  created(){
    this.getUserAll()
  },
  components: {
    Navbar,
    CardUser
  }
}
</script>

<style scoped>
  /* list pembayaran */
  .list-pembayaran h5 {
    font-size: 15px;
    margin-bottom: 40px;
  }
</style>

<template>
  <div class="page">
    <!-- navbar -->
    <Navbar />
    <!-- banner -->
    <div class="banner">
      <div class="container">
        <div class="row justify-content-center align-items-center">
          <div class="col-12 col-lg-10 col-md-10 header mt-4 d-flex align-items-center">
            <img src="/icons/volleyball.svg" alt="volleyball">
            <div class="countdown">
              <h5 class="m-0 mb-1 pm">{{ 31 - sekarang.substr(sekarang.length - 2) }} Hari</h5>
              <p class="m-0">menuju tanggal 31 Agustus 2020</p>
            </div>
          </div>
          <div class="col-12 col-lg-10 col-md-10 terkumpul mb-3">
            <div class="card">
              <div class="card-body d-flex align-items-center">
                <div class="item">
                  <p class="title m-0 pm">Sudah Terkumpul</p>
                  <p class="nominal m-0">{{ formatRupiah((180000 - terkumpul), 'Rp') }}</p>
                </div>
                <div class="item">
                  <p class="title m-0 pm">Menuju Terkumpul</p>
                  <p class="nominal m-0">{{ formatRupiah(terkumpul, 'Rp') }}</p>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
    <div class="container">
      <div class="row align-items-center justify-content-center">
        <!-- tanggal -->
        <div class="col-12 col-lg-10 col-md-10 mt-3 tanggal mb-3">
          <p class="hari pm m-0">{{formatHari(hari_ini.substr(hari_ini.length - 2))}}</p>
          <p class="tanggal m-0">{{ formatTanggal(hari_ini) }}</p>
        </div>
        <div class="col-12 col-lg-8 col-md-8 list-tanggal mb-3">
          <div class="list d-flex align-items-center">
            <div class="item text-white text-center" @click="changeTanggal(item)" v-for="(item, index) in tanggal" :key="index" :class="{active: hari_ini.substr(hari_ini.length - 2) == item.tanggal}">
              <p class="tgl pm m-0">{{ item.tanggal }}</p>
              <p class="hari m-0">{{ item.hari }}</p>
            </div>
          </div>
        </div>
        <!-- list -->
        <div class="col-12 col-lg-10 col-md-10 list-pembayaran">
          <h5 class="pm m-0 mb-3">List Pembayaran</h5>
          <div class="list" v-if="urunan_tgl.length > 0">
            <card-urunan :data={data} v-for="data in urunan_tgl" :key="data.id"/>
          </div>
          <p class="m-0 text-center" v-else>Data Tidak Ada</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Navbar from '../components/Navbar';
import CardUrunan from '../components/CardUrunan';

export default {
  data(){
    return{
      tanggal : [
        { tanggal: 23, hari: 'min' },
        { tanggal: 24, hari: 'sen' },
        { tanggal: 25, hari: 'sel' },
        { tanggal: 26, hari: 'rab' },
        { tanggal: 27, hari: 'kam' },
        { tanggal: 28, hari: 'jum' },
        { tanggal: 29, hari: 'sab' },
        { tanggal: 30, hari: 'min' },
        { tanggal: 31, hari: 'sen' },
      ],
      hari_ini: 0,
      sekarang: 0,
      urunan_tgl: [],
      terkumpul: []
    }
  },
  methods: {
    getTerkumpul: function(){
      this.$axios.get('urunan/terkumpul')
      .then(res => {
        this.terkumpul = res.data.hasil
      })
    },
    getUrunanByTgl: function(){
      this.$axios.get(`urunan/${this.hari_ini}/all`)
      .then(res => {
        this.urunan_tgl = res.data.hasil
      })
    },
    changeTanggal: function(tgl){
      var tanggal = this.hari_ini.split('-');
      tanggal[2]  = tgl.tanggal;
      this.hari_ini = tanggal[0] + '-' + tanggal[1] + '-' + tanggal[2];
      this.getUrunanByTgl()
    },
    formatRupiah: function(angka, prefix){
			var number_string = angka.toString().replace(/[^,\d]/g, ''),
			split   		= number_string.split(','),
			sisa     		= split[0].length % 3,
			rupiah     		= split[0].substr(0, sisa),
			ribuan     		= split[0].substr(sisa).match(/\d{3}/gi);
 
			// tambahkan titik jika yang di input sudah menjadi angka ribuan
			if(ribuan){
				var separator = sisa ? '.' : '';
				rupiah += separator + ribuan.join('.');
			}
 
			rupiah = split[1] != undefined ? rupiah + ',' + split[1] : rupiah;
			return prefix == undefined ? rupiah : (rupiah ? 'Rp ' + rupiah : '');
    },
    formatHari: function(hari){
        switch(hari) {
            case '23': hari = "Minggu"; break;
            case '24': hari = "Senin"; break;
            case '25': hari = "Selasa"; break;
            case '26': hari = "Rabu"; break;
            case '27': hari = "Kamis"; break;
            case '28': hari = "Jum'at"; break;
            case '29': hari = "Sabtu"; break;
            case '30': hari = "Minggu"; break;
            case '31': hari = "Senin"; break;
        }
        return hari;
    },
    formatTanggal: function(tanggal){
        var tgl = tanggal.split('-');
        var bulan = parseInt(tgl[1].substr(1,1) - 1);
        switch(bulan) {
            case 0: bulan = "Januari"; break;
            case 1: bulan = "Februari"; break;
            case 2: bulan = "Maret"; break;
            case 3: bulan = "April"; break;
            case 4: bulan = "Mei"; break;
            case 5: bulan = "Juni"; break;
            case 6: bulan = "Juli"; break;
            case 7: bulan = "Agustus"; break;
            case 8: bulan = "September"; break;
            case 9: bulan = "Oktober"; break;
            case 10: bulan = "November"; break;
            case 11: bulan = "Desember"; break;
        }
        return tgl[2] + ' ' + bulan + ' ' + tgl[0]
    }
  },
  created(){
    const date = new Date();
    this.hari_ini = date.getFullYear() + '-' + ('0' + (date.getMonth() + 1)) + '-' + date.getDate();
    this.sekarang = date.getFullYear() + '-' + ('0' + (date.getMonth() + 1)) + '-' + date.getDate();
    this.getTerkumpul();
    this.getUrunanByTgl();
  },
  components: {
    Navbar,
    CardUrunan
  }
}
</script>

<style scoped>
  .banner {
    background-image: url(/background.svg);
    width: 100%;
    height: 267px;
    background-size: cover;
    background-repeat: no-repeat;
  }
  .banner .container,
  .banner .row {
    height: 100%;
  }
  .banner .header {
    justify-content: space-between;
  }
  .banner .header .countdown {
    color: white;
  }
  .banner .header .countdown h5 {
    font-size: 35px;
  }
  .banner .header .countdown p {
    font-size: 12px;
  }
  .banner .terkumpul .card {
    border: none;
    border-radius: 3px;
  }
  .banner .terkumpul .card-body{
    justify-content: space-between;
  }
  .banner .terkumpul .item .title {
    font-size: 10px;
  }
  .banner .terkumpul .item .nominal {
    font-size: 17px;
  }
  /* tanggal */
  .tanggal .hari {
    font-size: 15px;
  }
  .tanggal .tanggal {
    font-size: 12px;
  }
  /* list tanggal */
  .list-tanggal .list {
    overflow-x: scroll;
    justify-content: space-between;
  }
  .list-tanggal .list .item {
    background: rgba(252, 191, 30, 0.46);
    border: 1px solid #FCBF1E;
    box-sizing: border-box;
    border-radius: 3px;
    padding: 5px 8px;
    margin-right: 5px;
    cursor: pointer;
  }
  .list-tanggal .list .item.active {
    background: #FCBF1E;
  }
  .list-tanggal .list .item .tgl {
    font-size: 18px;
  }
  .list-tanggal .list .item .hari {
    font-size: 10px;
  }
  /* list pembayaran */
  .list-pembayaran h5 {
    font-size: 15px;
  }
</style>

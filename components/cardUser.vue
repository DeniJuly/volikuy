<template>
    <b-card no-body class="mb-5">
        <b-card-body class="card-body">
            <div class="info d-flex align-items-center">
            <div class="user">
                <img :src="`${$axios.defaults.baseURL}user/${data.foto}`" alt="" class="img-fit">
            </div>
            <div class="detail">
                <p class="nama pm m-0">{{ data.nama }}</p>
                <p class="nominal m-0">{{ formatRupiah(total, 'Rp') }}</p>
            </div>
            </div>
            <b-collapse :id="`detail-${data.id}`" accordion="my-accordion" role="tabpanel">
                <h5 class="pm mt-2 title">Detail Nyicil</h5>
                <div class="item d-flex align-items-center" v-for="(urunan, index) in data.urunan" :key="urunan.id">
                    <div class="no text-center">
                        <p class="m-0">{{ index + 1 }}</p>
                    </div>
                    <div class="content">
                        <p class="nominal m-0 pm">{{formatRupiah(urunan.nominal, 'Rp')}}</p>
                        <p class="tanggal m-0">{{ formatTanggal(urunan.waktu.substr(0, 10)) }}</p>
                    </div>
                </div>
            </b-collapse>
        </b-card-body>
        <b-card-footer class="card-footer text-center">
            <b-button block v-b-toggle="`detail-${data.id}`">
            <img src="/icons/arrow-down.svg" alt="arrow down">
            </b-button>
        </b-card-footer>
        </b-card>
</template>

<script>
export default {
    name: 'card-user',
    props: {
        data: Object
    },
    data(){
        return{
            total: 0
        }
    },
    created(){
        this.getTotalUrunan(this.data.id);
    },
    methods: {
        getTotalUrunan: function(id){
            this.$axios.get(`urunan/${id}/total`)
            .then(res => {
                this.total = res.data.hasil
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
    }
}
</script>

<style scoped> 
  .card {
    background: #FFFFFF;
    box-shadow: 0px 2px 8px rgba(0, 0, 0, 0.08);
    border-radius: 5px;
    border: 0;
  }
  .card .card-body {
    padding: 10px 15px;
  }
  .card .user {
    width: 70px;
    height: 70px;
    overflow: hidden;
    border: 3px solid #0779E4;
    box-sizing: border-box;
    border-radius: 40px;
    margin-top: -45px;
  }
  .card .detail {
    margin-left: 10px;
    font-size: 12px;
  }
  .card .card-footer {
    padding: 0px;
    border: 0;
    background: #ECECEC;
    cursor: pointer;
  }
  .card .card-footer button {
    background: transparent!important;
    border: 0;
  }
  .card .card-footer button:focus {
    box-shadow: none;
  }
  .title {
    font-size: 15px;
  }
  .item {
    color: #222831;
    border-bottom: 1px solid #ECECEC;
    padding: 8px;
    font-size: 12px;
  }
  .no.text-center {
    width: 28px;
  }
</style>
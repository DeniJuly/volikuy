<template>
    <div class="card mb-3">
        <div class="card-body d-flex align-items-center">
            <div class="user">
                <img :src="`${$axios.defaults.baseURL}user/${data.data.user.foto}`" alt="" class="img-fit">
            </div>
            <div class="detail">
            <p class="nama pm m-0">{{ data.data.user.nama }}</p>
            <p class="nominal m-0">{{ formatRupiah(data.data.nominal, 'Rp') }}</p>
            <p class="waktu m-0">{{ formatTanggal(data.data.waktu.substr(0, 10)) }}</p>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    name: 'card-urunan',
    props: {
        data: Object
    },
    methods: {
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
    .card-body .user {
        width: 50px;
        height: 50px;
        overflow: hidden;
        border-radius: 100%;
    }
    .card .detail {
        margin-left: 10px;
        font-size: 10px;
    }
    .card .detail .nama {
        font-size: 12px;
    }
</style>
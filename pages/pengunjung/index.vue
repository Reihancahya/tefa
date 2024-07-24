<template>
    <div class="container-fluid">
        <div class="row">
            <div class="col-lg-12">
                <h2 class="text-center my-4">RIWAYAT KUNJUNGAN</h2>
                <div class="my-3">
                    <input type="search" class="form-control  rounded-5" placeholder="filter">
                </div>
                <div class="my-3 text-muted">Menampilkan 1 dari 1</div>
                <table class="table">

                    <head>
                        <tr>
                            <td>#</td>
                            <td>NAMA</td>
                            <td>KEANGGOTAAN</td>
                            <td>WAKTU</td>
                            <td>KEPERLUAN</td>
                        </tr>
                    </head>
                    <tbody>
                        <tr v-for="(visitor, i) in visitors" :key="i">
                            <td>{{ i + 1 }}</td>
                            <td>{{ visitor.nama }}</td>
                            <td>{{ visitor.keanggotaan.nama }}</td>
                            <td>{{ visitor.tanggal }},{{ visitor.waktu }}</td>
                            <td>{{ visitor.keperluan.nama }}</td>
                        </tr>
                    </tbody>

                </table>
            </div>
        </div>
    </div>
    <Nuxt-Link to="/">
        <button type="submit" class="btn btn-secondary btm-lg rounded-5" style="margin-left: 82%;">KEMBALI</button>
    </Nuxt-Link>
</template>

<script setup>
const supabase = useSupabaseClient()

const visitors = ref([])
const getPengunjung = async () => {
    const { data, error } = await supabase.from('pengunjung').select('*,keanggotaan(*), keperluan(*)')
    if (data) visitors.value = data
}
const totalPengunjung = async () => {
    const { data, count } = await supabase.from('pengunjung')
    .select("*", {count: 'exact'})
    if (data) jumlah.value = count
}
onMounted(() => {
    getPengunjung()
    totalPengunjung()
})</script>
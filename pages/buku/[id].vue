<template>
 <div class="container-fluid">
     <h2 class="text-start my-4">{{ buku.judul }}</h2>
     <div class="row">
         <div class="col">
            <img :src="buku.cover" class="cover" alt="cover buku">
         </div>
         <div class=" col-md-9 p-5">
        <ul class="list-group list-group-flush">
          <li class="list-group-item">Judul: {{ buku.judul }}</li>
          <li class="list-group-item">Penulis: {{ buku.penulis }}</li>
          <li class="list-group-item">Penerbit: {{ buku.penerbit }}</li>
          <li class="list-group-item">Tahun Terbit: {{ buku.tahun_terbit }}</li>
          <li class="list-group-item">
            <span v-if="buku.kategori">
              Kategori : {{ buku.kategori.nama }}
            </span>
            <span v-else> </span>
          </li>
          <li class="list-group-item">Deskripsi: {{ buku.deskripsi }}</li>
        </ul>
      </div>
     </div>
     <Nuxt-Link to="/">
        <button type="submit" class="btn btn-secondary btm-lg rounded-5" style="margin-left: 82%;">KEMBALI</button>
    </Nuxt-Link>
    </div>
   
</template>
<script setup>
const supabase = useSupabaseClient()
const route = useRoute()
const buku = ref({})


const getBookById = async () => {
    const { data, error } = await supabase.from('buku').select(`*, kategori_buku(*)`)
        .eq('id', route.params.id)
    if (data) buku.value = data[0]
}

onMounted(() => {
    getBookById()
})
</script>
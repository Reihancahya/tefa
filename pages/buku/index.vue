<template>
  <div class="container-fluid">
    <div v-if="loading">Loading bentar broooooo</div>
    <div v-else="anjay">
      <form @submit.prevent="getBooks">
            <div class="my-3">
                    <input v-model="keyword" type="search" class="form-control  rounded-5" placeholder="mau baca apa hari ini...">
            </div>
      </form>
      <div class="row mt-5">

        <div class="my-3 text-muted">menampilkan {{ books.length }} buku dari {{ books.length }}</div>
        <div class="row justify-content-evenly">
          <div v-for="(book, i) in books" :key="i" class="col-lg-2 col-md-4 col-sm-6">
            <nuxt-link :to="`/buku/${book.id}`">
              <div class="card mb-3">
                <div class="card-body">
                  <img :src="book.cover"  class="cover" alt="cover" />
                </div>
              </div>
            </nuxt-link>
          </div>
        </div>
      </div>
    </div>
    
      <Nuxt-Link to="/">
        <button type="submit" class="btn btn-secondary btm-lg rounded-5" style="margin-left: 82%;">KEMBALI</button>
    </Nuxt-Link>
    <div class="text-center" v-if="!loading && books.length === 0">
      <div class="d-flex justify-content-center align-items-center" style="height: 100vh;">
        <div>
          <h1> Maaf, judul "{{ keyword }}" tidak ditemukan</h1>
         
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.btn-light {
  background-color: #5fd8fe !important;
  box-shadow: 1px 1px 10px #5fd8fe !important;
}

.cover {
  width: 100%;
  height: auto;
  object-fit: cover;
  object-position: 0;
}

.card-body {
  box-shadow: 1px 1px 5px black;
  
}

.bg-warning {
  box-shadow: 1px 1px 10px #E4C72D;
}
</style>

<script setup>
import { ref, onMounted } from 'vue';
const supabase = useSupabaseClient();

const books = ref([]);
const loading = ref(true);
const keyword = ref('');

const getBooks = async () => {  
  loading.value = true;
  const { data, error } = await supabase.from('buku')
  .select(`*`).ilike("judul", `%${keyword.value}%`);
  if (data) books.value = data
  if(error) console.log(error)
  loading.value = false;
};
// const countBook = async () => {
//   const { data, count } = await supabase
//     .from("buku")
//     .select("*", { count: "exact" });
//   if (data) book.value = count;
// };

onMounted(() => {
  getBooks();
  // countBook()
});
</script>
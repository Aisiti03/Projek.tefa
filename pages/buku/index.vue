<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">BUKU</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="form-control rounded-5" placeholder="Mau baca apa hari ini?">
          </form>

        </div>
        <div class="my-3 text-muted">menampilkan 3 dari 3</div>
      </div>
        <div v-for="(book, i) in books" :key="i" class="col-lg-2">
        <nuxt-Link :to="`/buku/${book.id}`">
          <div class="card mb-3 shadow-lg">
            <div class="card-body">
              <img :src=book.cover class="cover" alt="cover1">
            </div>
          </div>
        </nuxt-Link>
        </div>
      </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()
const books= ref([])
const keyword = ref('')

const getBooks = async () => {
  const {data, error} = await supabase.from('buku').select(`*,kategori(*)`)
  .order('id')
  .ilike("judul",`%${keyword.value}%`)
  if (error) throw error
  if(data) books.value = data
}

onMounted(()=> {
  getBooks()
})
</script>

<style scoped>
.card-body {
  width: 100%;
  height: 20em;
  padding: 0;
}
.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}
</style>
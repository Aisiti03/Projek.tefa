<template>
  <div class="content">
  <div class="container-fluid">
        <nuxt-link to="/buku">
            <button type="button" class="btn btn-outline-dark btn-lg mt-4">KEMBALI</button></nuxt-link>
                <h2 class="text-center my-4 fw-bold">RINCIAN BUKU</h2>
            <div class="row d-flex justify-content-center flex-md-wrap" style="padding-top: 190px;">
              <div class="col-3">
                  <img :src="buku?.cover"  class="cover row img-fluid" alt="cover buku"   style="width: 250px;">
              </div>
              <div class="col-8">
                  <div class="row">
                      <h2 class="text start text-center my-4 fst-italic fw-bold">{{ buku?.judul }}</h2>
                  </div>
                  <div class="col-12">
                      <h4 class="text-dark">judul : {{ buku?.judul }}</h4>
                      <h4 class="text-dark">penulis: {{ buku?.penulis }}</h4>
                      <h4  class="text-dark">tahun_terbit: {{ buku?.tahun_terbit }}</h4>
                      <h4  class="text-dark">penerbit: {{ buku?.penerbit }}</h4>
                      <h4 class="text-dark">kategori: {{ buku?.kategori_buku?.nama }}</h4>
                      <h4 class="text-dark">rak: {{buku?.rak}}</h4>
                      <h4 class="text-dark">deskripsi: {{ buku?.deskripsi }}</h4>                    
                  </div>
              </div>
          </div>
      </div>
  </div>
</template>
<script setup>
import { onMounted } from 'vue';

const supabase = useSupabaseClient()
const route = useRoute()
const buku = ref()
const kategories = ref ([])

const getBookByid = async () => {
  const { data, error } = await supabase
  .from('buku')
  .select(`*, kategori_buku(*)`)
  .eq('id', route.params.id)
  .maybeSingle()
  if (error) throw error
  if(data) buku.value = data
}

const getKategori = async () => {
  const { data, error } = await supabase
  .from('kategori_buku')
  .select('*')
  if(data) kategories.value = data
}

onMounted(() => {
  getBookByid()
  getKategori()
})

</script>
<style scoped>
.content {
background-image:linear-gradient(#0050EB,#ffffff);
background-size: cover;
width: 100%;
height: 100vh;
font-family: '';
color: black;
}
</style>
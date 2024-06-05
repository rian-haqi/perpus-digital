<template>
  <Header>
    <div class="row p-3 d-flex  navbar">
      <div class="col-lg-2 ps-5">
        <img src="@/assets/img/LOGO-SMK4.png" style="width: 100px;" alt="logo SMKN 4">
      </div>
      <div class="col-lg-6 header ">
        <h2>Perpus Digital</h2>
        <address>
          SMKN 4 Tasikmalaya
          Jl.Depok,Sukamenak,
          Purbaratu
        </address>
      </div>
      <div class="col-lg-2">
        <nuxt-link to="/pengunjung/" class="row float-center me-7">
          <button type="button" class="btn btn-primary btn-sm rounded-5" >Kembali<P>Melihat Riwayat kunjungan</P></button>
        </nuxt-link>
      </div>
    </div>
  </Header>
  <div class="container-fluid">
    <div class="row">
      <div class="row d-flex justify-content-center">
        <div class="col-lg-4">
          <form @submit.prevent="getBooks">
          <h2 class="text-center text-white my-4">BUKU</h2>
          <div class="my-3">
            <input v-model="keyword" type="Search" class="form form-control rounded-5 text-white" placeholder="🔍 Mau baca apa hari ini? . . . . .">
          </div>
        </form>
      </div>
        <div class="row">
          <div class="col-lg-2 " v-for="(buku, i) in books " :key="i">
            <div class="card mb-3 mt-4">
              <div class="card-body">
                <nuxt-link :to="`/buku/${buku.id}`">
                  <img :src="buku.cover" class="cover" :alt="buku.judul" />
                </nuxt-link>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>

const supabase = useSupabaseClient()


const books = ref([])
const keyword = ref('')

const getBooks = async () => {
  const { data, error } = await supabase.from('buku').select(`*, kategori(*)`)
    .ilike('judul', `%${keyword.value}%`)
  if (data) books.value = data
}

onMounted(() => {
  getBooks()
})
</script>



<style scoped>
.card-body {
  width: 100%;
  height: 400px;
  padding: 0%;
}

.navbar{
  background-color: #DED2D2;
}

.cover {
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: 0 30;
}

.container-fluid{
  background-image: url("assets/img/ado.avif");
}
.btn{
  font-size: 17px;
}

.header{
  text-align: center;
}
.form{
background-color: #0d6efd;
}

::placeholder{
  color: white;
}
</style>
<template>
  <div class="container-fluid ">
  <header>
    <div class="row d-flex justify-center">
      <div class="col-lg-5">
        <h1>Perpus Digital</h1>
        <address>
          SMKN 4 Tasikmalaya
          Jl.Depok,Sukamenak, Purbaratu
        </address>
      </div>
      <div class="col-lg-2">
        <img src="@/assets/img/LOGO-SMK4.png" style="width: 100px;" alt="logo SMKN 4">
      </div>
      <div class="col-lg-5">
        <nuxt-link to="/buku" class="row float-end me-5">
          <button type="button" class="btn btn-primary btn-sm rounded-5">pencarian buku</button>
        </nuxt-link>
        <h2 class="row text-center my-4">RIWAYAT KUNJUNGAN</h2>
      </div>
    </div>
  </header>
    <div class="row my-3 d-flex justify-content-center">
      <input v-model="keyword" class="col-5 form-control form-control-lg rounded-5 border-dark" placeholder="🔍 Search..." style="width: 30rem;" @input="getPengunjung" />
    </div>
  <div class="table-responsive">
    <table class="table">
      <thead>
        <tr>
          <td>No</td>
          <td>Nama</td>
          <td>Kenggotaan</td>
          <td>Waktu</td>
          <td>Keperluan</td>
          <td>Kelas</td>
        </tr>
      </thead>
      <tbody>
        <tr v-for="(visitor,i) in visitors" :key="i">
          <td>{{ i+1 }}.</td>
          <td>{{ visitor.nama }}</td>
          <td>{{ visitor.keanggotaan.nama }}</td>
          <td>{{ visitor.tanggal }}, {{visitor.jam }}</td>
          <td>{{ visitor.keperluan.nama }}</td>
          <td>{{ visitor.tingkat }}-{{ visitor.jurusan }}-{{  visitor.kelas }}</td>
        </tr>
      </tbody>
    </table>
  </div>
    <nuxt-link to="/pengunjung/tambah">
      <button type="button" class="btn btn-primary btn-lg mt-4">KEMBALI</button>
    </nuxt-link>
</div>
</template>


<script setup>

const supabase = useSupabaseClient()

const keyword = ref('')


const visitors = ref([])

const getPengunjung = async () => {
  const { data, error } = await supabase.from('pengunjung').select('*, keanggotaan(*), keperluan(*)')
  .ilike('nama', `%${keyword.value}%`)
  .order('id', { ascending: false })
  if(data) visitors.value = data
}
onMounted(() => {
  getPengunjung()
})
</script>
<style scoped>
address{
  padding-top: 5px;
}

.container-fluid{
background-color: aqua;


}
</style>


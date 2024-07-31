<template>
  <div class="content">
    <div class="container-fluid  ">
      <header>
        <div class="row">
          <div class="col-lg-4">
            <img src="@/assets/img/LOGO-SMK4.png" alt="" class="float-end" style="width: 100px;">
          </div>
          <div class="col-lg-8 ">
            <h1>Perpustakaan Digital</h1>
            <address>
              SMKN 4 Tasikmalaya<br>
              Jl.Depok, Sukamenak, Purbaratu
            </address>
          </div>
        </div>
      </header>
      <form @submit.prevent="KirimData">
        <div class="row justify-content-center">
          <div class="col-lg-10">
            <div class="mb-3">
              <input v-model="form.nama" type="text" placeholder="NAMA..."
                class="form-control form-control-lg rounded-5 border-dark">
            </div>
            <div class="mb-3">
              <select v-model="form.keanggotaan" class="form-control form-control-lg rounded-5 border-dark">
                <option disabled value="">KEANGGOTAAN...</option>
                <option v-for="(member, i) in members" :key="i" :value="member.id">{{ member.nama }}</option>
              </select>
            </div>
            <div v-if="form.keanggotaan == 1" class="mb-4">
              <div class="row">
                <div class="col-md-4">
                  <select v-model="form.tingkat"
                    class="form-control form-control-lg form-select rounded-5 mb-2 border-dark">
                    <option disabled value="">TINGKAT</option>
                    <option value="X">X</option>
                    <option value="XI">XI</option>
                    <option value="XII">XII</option>
                  </select>
                </div>
                <div class="col-md-4">
                  <select v-model="form.jurusan"
                    class="form-control form-control-lg form-select rounded-5 mb-2 border-dark">
                    <option disabled value="">JURUSAN</option>
                    <option value="PPLG">PPLG</option>
                    <option value="TBSM">TBSM</option>
                    <option value="TOI">TOI</option>
                    <option value="TKJT">TKJT</option>
                    <option value="DKV">DKV</option>
                  </select>
                </div>
                <div class="col-md-4">
                  <select v-model="form.kelas"
                    class="form-control form-control-lg form-select rounded-5 mb-2 border-dark">
                    <option disabled value="">KELAS</option>
                    <option value="1">1</option>
                    <option value="2">2</option>
                    <option value="3">3</option>
                    <option value="4">4</option>
                  </select>
                </div>
              </div>
            </div>
            <div class="mb-4">
              <select v-model="form.keperluan" class="form-control form-control-lg form-select rounded-5 border-dark">
                <option disabled value="">KEPERLUAN...</option>
                <option v-for="(item, i) in objectives" :key="i" :value="item.id">{{ item.nama }}</option>
                <option :value="4">Lainnya</option>
              </select>
            </div>
            <div v-if="form.keperluan == 4" class="mb-2">
              <div class="mb-3">
                <input v-model="keperluanLain" type="text" class="form-control form-control-lg rounded-5 border-dark" placeholder="Tulis keperluan kamu...">
              </div>
            </div>
          </div>
        </div>
        <div class="text-center border-dark">
          <button class="btn btn-primary mt-3 border-dark">Kirim</button>
        </div>

      </form>
      <button class="back card bg-pengunjung rounded-5 justify-content-center">
        <div class="card-body text-center ">
          <nuxt-link to="/" class="btn text-decoration-none">
            <h3>Back To Home</h3>
          </nuxt-link>
        </div>
      </button>
    </div>
  </div>
</template>

<script setup>
const supabase = useSupabaseClient()

const members = ref([])
const objectives = ref([])


const form = ref({
  nama: "",
  keanggotaan: "",
  tingkat: "",
  jurusan: "",
  kelas: "",
  keperluan: "",
})
const keperluanLain = ref('')

const KirimData = async () => {
  const { error } = await supabase.from('pengunjung').insert({
    nama: form.value.nama,
    keanggotaan: form.value.keanggotaan,
    tingkat: form.value.tingkat,
    jurusan: form.value.jurusan,
    kelas: form.value.kelas,
    keperluan: (keperluanLain.value) ? null : form.value.keperluan,
    keperluan_lain: keperluanLain.value
  })
  if (error) throw error
  else navigateTo('/pengunjung')
}


const getKeanggotaan = async () => {
  const { data, error } = await supabase.from('keanggotaan').select('*')
  if (data) members.value = data
}


const getKeperluan = async () => {
  const { data, error } = await supabase.from('keperluan').select('*')
  if (data) objectives.value = data
}


onMounted(() => {
  getKeanggotaan()
  getKeperluan()
});
</script>


<style scoped>
.card {
  width: 10rem;
  height: 6rem;
  background-color: rgb(255, 234, 214);
  padding: 9 px;
  color: blue;
}

.back {
  position: relative;
  margin-left: 45%;
  font-size: 20px;
  margin-top: 55px;
}

.back h3 {
  font-size: 20px;
}

.container-fluid {
  background-color: cadetblue;
  height: 100vh;
}
</style>

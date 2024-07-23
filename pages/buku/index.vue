<template>
  <div class="container-fluid">
    <div class="row">
      <div class="col-lg-12">
        <h2 class="text-center my-4">BUKU</h2>
        <div class="my-3">
          <form @submit.prevent="getBooks">
            <input v-model="keyword" type="search" class="from-control rounded-5"
              placeholder="Mau baca apa hari ini?" />
          </form>
        </div>
        <div class="my-3 text-muted fs-6">menampilkan {{ books.length }} dari {{ jumlah }}</div>
        <div class="row">
          <div v-for="(book, i) in books" :key="i" class="col-lg-3 ob-5">
            <nuxtLink :to="`/buku/${book.id}`">
              <div class="card mb-3 dcdc " style="width: 200px;">
                <div class="card-header d-flex justify-content-center" style="height: 250px;">
                  <img :src="book.cover" style="width: 100%; height: 100%">
                </div>
                <div class="card-body d-flex justify-content-center align-items-center" style="height: 50px;">
                  <h6> {{ book.judul }}</h6>
                </div>
              </div>
            </nuxtLink>
          </div>
        </div>
      </div>
    </div>

    <NuxtLink to="/">
      <button type="sumbit" class="btn btn-warning btn-lg rounded-3 px-3">kembali</button>
    </NuxtLink>

  </div>
</template>

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

<script setup>

const supabase = useSupabaseClient()

const books = ref([])
const jumlah = ref(0)
const keyword = ref('')

const getBooks = async () => {
  const { data, error } = await supabase.from('buku').select(`*, kategori(*)`)
    .ilike('judul', `%${keyword.value}%`)
  if (data) books.value = data
}

const totalBuku = async () => {
  const { data, count } = await supabase.from('buku').select("*", { count: 'exact' })
  if (data) jumlah.value = count
}

onMounted(() => {
  getBooks()
  totalBuku()
})
</script>

<template>
    <q-page padding>
    <q-card flat class="q-gutter-md">
      <q-card-section>
          <div class="text-h5">Input Pasien</div>
          <q-form
               @submit="onSubmit"
               @reset="onReset"
               class="q-mt-lg"
          >
          <div class="q-gutter-md">
              <q-input label="Nama Pasien" v-model="namaLengkap" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Nama Pasien']"></q-input>
              <q-input label="Tempat/Tgl/Lahir" v-model="ttl" filled :rules="[ val => val && val.length > 0 || 'Tolong isi ttl']"></q-input>
              <q-input label="Jenis Kelamin" v-model="jeniskelamin" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Jenis Kelamin']"></q-input>
              <q-input label="Alamat" v-model="alamat" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Alamat Anda']"></q-input>
              <q-input label="Usia Pasien" v-model="usia" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Usia Pasien']"></q-input>
              <q-input label="Diagnosa Penyakit" v-model="diagnosa" filled :rules="[ val => val && val.length > 0 || 'Tolong isi Diagnosa Penyakit']"></q-input>
              <q-btn type="submit" label="Update" color="teal" unelevated></q-btn>
              <q-btn type="reset" label="Reset" color="teal" flat unelevated></q-btn>
          </div>
          </q-form>
      </q-card-section>
    </q-card>
    </q-page>
</template>
<script>
export default {
  data () {
    return {
      namaLengkap: null,
      ttl: null,
      jeniskelamin: null,
      alamat: null,
      usia: null,
      diagnosa: null
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('pasien/tampilsingle/' + this.$route.params.id)
        .then(res => {
          const data = res.data
          this.namaLengkap = data.namaLengkap
          this.ttl = data.ttl
          this.jeniskelamin = data.jeniskelamin
          this.alamat = data.alamat
          this.usia = data.usia
          this.diagnosa = data.diagnosa
        })
    },
    onReset () {
      this.namaLengkap = null
      this.ttl = null
      this.jeniskelamin = null
      this.alamat = null
      this.usia = null
      this.diagnosa = null
    },
    onSubmit () {
      this.$axios.put('pasien/edit/' + this.$route.params.id, {
        namaLengkap: this.namaLengkap,
        ttl: this.ttl,
        jeniskelamin: this.jeniskelamin,
        alamat: this.alamat,
        usia: this.usia,
        diagnosa: this.diagnosa
      }).then(res => {
        if (res.data.sukses) {
          this.$q.notify({
            type: 'positive',
            message: res.data.pesan
          })
          this.$router.push({ name: 'dashboard' })
        } else {
          this.$q.notify({
            type: 'negative',
            message: res.data.pesan
          })
        }
      })
    }
  }
}
</script>

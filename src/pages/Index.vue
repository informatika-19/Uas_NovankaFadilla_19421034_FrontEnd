<template>
  <q-page padding>
<q-table
      title="Nama Pasien"
      :rows="data"
      :columns="columns"
      row-key="name"
    >
      <template v-slot:body="props">
        <q-tr :props="props">
          <q-td key="namaLengkap" :props="props">
            {{ props.row.namaLengkap }}
          </q-td>
          <q-td key="ttl" :props="props">
            {{ props.row.ttl }}
          </q-td>
          <q-td key="jeniskelamin" :props="props">
            {{ props.row.jeniskelamin }}
          </q-td>
           <q-td key="alamat" :props="props">
            {{ props.row.alamat }}
          </q-td>
           <q-td key="usia" :props="props">
            {{ props.row.usia }}
          </q-td>
          <q-td key="diagnosa" :props="props">
            {{ props.row.diagnosa }}
          </q-td>
          <q-td key="aksi" :props="props">
            <div class="q-pa-md q-gutter-sm">
              <div class="col">
                <q-btn  label="Edit" :to="{ name: 'formEditPasien', params: { id: props.row._id}}" color="warning" icon="edit" unelevated />
               </div>
              <div class="col">
                <q-btn label="Hapus" @click="confirm(props.row._id)" color="negative" icon="delete" unelevated/>
               </div>
              </div>
          </q-td>
        </q-tr>
      </template>
    </q-table>
  </q-page>
</template>

<script>
export default {
  name: 'PageIndex',
  data () {
    return {
      columns: [
        { name: 'namaLengkap', align: 'center', label: 'Nama Pasien', field: 'namaLengkap', sortable: true },
        { name: 'ttl', label: 'Tempat/Tgl/Lahir', align: 'center', field: 'ttl', sortable: true },
        { name: 'jeniskelamin', label: 'Jenis Kelamin', align: 'center', field: 'jeniskelamin', sortable: true },
        { name: 'alamat', label: 'Alamat', align: 'center', field: 'alamat', sortable: true },
        { name: 'usia', label: 'Usia Pasien', align: 'center', field: 'usia', sortable: true },
        { name: 'diagnosa', label: 'Diagnosa Penyakit', align: 'center', field: 'diagnosa', sortable: true },
        { name: 'aksi', label: 'Aksi', field: 'aksi', align: 'center' }
      ],
      data: []
    }
  },
  created () {
    this.getData()
  },
  methods: {
    getData () {
      this.$axios.get('pasien/tampil')
        .then(res => {
          if (res.data.sukses) {
            this.data = res.data.data
          } else {
            this.$q.notify({
              type: 'negative',
              message: res.data.pesan
            })
          }
        })
    },
    confirm (id) {
      this.$q.dialog({
        title: 'Konfirmasi',
        message: 'Apakah Anda Yakin Menghapus Data Pasien Ini ?',
        cancel: true,
        persistent: true
      }).onOk(() => {
        this.$axios.delete('pasien/delete/' + id)
          .then((res) => {
            if (res.data.sukses) {
              this.$q.notify({
                type: 'positive',
                message: res.data.pesan
              })
              this.getData()
            } else {
              this.$q.notify({
                type: 'negative',
                message: res.data.pesan
              })
            }
          })
      })
    }
  }
}
</script>

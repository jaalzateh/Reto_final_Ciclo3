<template>
  <v-data-table
    :headers="headers"
    :items="articulos"
    sort-by="id"
    items-per-page="5"
    class="elevation-1"
    :loading="cargando"
    loading-text="Cargando... Por favor, espere"
  >
    <template v-slot:no-data>
      <v-btn
        color="primary"
        @click="initialize"
      >
        Restaurar
      </v-btn>
    </template>
  </v-data-table>
</template>

<script>
  export default {
    data: () => ({

      dialog: false,
      dialogDelete: false,
      cargando : true,
      headers: [
        { text: 'ID', value: 'id' },
        { text: 'Nombre', value: 'nombre' },
        { text: 'Descripcion', value: 'descripcion' },
        { text: 'Categoria', value: 'categoria.nombre' },
      ],
      articulos: [],
      categorias: [],
      editedIndex: -1,
      editedItem: {
        nombre: '',
        id: 0,
        codigo: 1000,
        nombre: "",
        estado: 0,
        categoria: {
          id: 0,
          nombre: '',
          estado: 0
        }
      },
      defaultItem: {
        id: 0,
        codigo: 1000,
        nombre: "",
        categoria: "",
        descripcion: "",
        estado: 0,
        categoria: {
          id: 0,
          nombre: '',
          estado: 0
        }
      },
    }),
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Nuevo Artículo' : 'Editar Artículo'
      },
    },
    watch: {
      dialog (val) {
        val || this.close()
      },
      dialogDelete (val) {
        val || this.closeDelete()
      },
    },
    created () {
      this.initialize()
    },
    methods: {
      initialize () {
        this.list()
        this.listCat()
      },
      list(){
        this.$http.get('/api/articulo/list')
          .then( (response) => {
            this.articulos = response.data
            this.cargando = false
          })
          .catch(error => {
            return error
          })
      },
      listCat(){
        this.$http.get('/api/categoria/list')
          .then( (response) => {
            this.categorias = response.data
            this.cargando = false
          })
          .catch(error => {
            return error
          })
          
      },
      editItem (item) {
        this.editedIndex = this.articulos.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
      deleteItem (item) {
        this.editedIndex = this.articulos.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },
      async deleteItemConfirm () {
        if (this.editedItem.estado === 1) {
          await this.$http.put('/api/articulo/deactivate', {id: this.editedItem.id})
        } else {
          await this.$http.put('/api/articulo/activate', {id: this.editedItem.id})
        }
        this.list()
        this.closeDelete()
      },
      close () {
        this.dialog = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
      closeDelete () {
        this.dialogDelete = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
      async save () {
        if (this.editedIndex > -1) {
          Object.assign(this.articulos[this.editedIndex], this.editedItem)

          let objetoBusqueda = {
            nombre : this.editedItem.nombre,
            descripcion: this.editedItem.descripcion,
            id : this.editedItem.id,
            categoriaId: this.editedItem.categoria.id
          }
          await this.$http.put('/api/articulo/update', objetoBusqueda)

        } else {
          let objetoBusqueda = {
            nombre : this.editedItem.nombre,
            descripcion: this.editedItem.descripcion,
            id : this.editedItem.id,
            categoriaId: this.editedItem.categoria.id,
            estado: 1
          }
          await this.$http.post('/api/articulo/add', objetoBusqueda)
          this.articulos.push(this.editedItem)
        }
        this.list()
        this.close()
      },
    },
  }
</script>
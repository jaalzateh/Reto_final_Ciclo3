<template>
  <v-data-table
    :headers="headers"
    :items="articulos"
    sort-by="id"
    class="elevation-1"
    :loading="cargando"
    loading-text="Cargando... Por favor, espere"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>ARTÍCULOS</v-toolbar-title>
        <v-divider
          class="mx-4"
          inset
          vertical
        ></v-divider>
        <v-spacer></v-spacer>
        <v-dialog
          v-model="dialog"
          max-width="500px"
        >
          <template v-slot:activator="{ on, attrs }">
            <v-btn
              color="primary"
              dark
              class="mb-2"
              v-bind="attrs"
              v-on="on"
            >
              Agregar Artículo
            </v-btn>
          </template>
          <v-card>
            <v-card-title>
              <span class="headline">{{ formTitle }}</span>
            </v-card-title>

            <v-card-text>
              <v-container>
                <v-row>
                    <v-text-field
                      v-model="editedItem.nombre"
                      label="Nombre artículo"
                    ></v-text-field>
                    <v-text-field
                      v-model="editedItem.codigo"
                      label="Código"
                    ></v-text-field>
                </v-row>
                <v-row>
                    <v-select
                      v-model="editedItem.categoria"
                      :items="categorias"
                      item-text="nombre"
                      item-value="id"
                      label="Id de la Categoría"
                      return-object
                    ></v-select>
                </v-row>
                <v-row>
                  <v-textarea
                  v-model="editedItem.descripcion"
                  label="Descripción">
                  </v-textarea>
                </v-row>
                
              </v-container>
            </v-card-text>

            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn
                color="blue darken-1"
                text
                @click="close"
              >
                Cancelar
              </v-btn>
              <v-btn
                color="blue darken-1"
                text
                @click="save"
              >
                Guardar
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogDelete" max-width="500px">
          <v-card>
            <v-card-title class="headline">¿Está seguro que desea eliminar?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeDelete">Cancelar</v-btn>
              <v-btn color="blue darken-1" text @click="deleteItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
        <v-dialog v-model="dialogChange" max-width="500px">
          <v-card>
            <v-card-title class="headline">¿Está seguro que desea cambiar de estado?</v-card-title>
            <v-card-actions>
              <v-spacer></v-spacer>
              <v-btn color="blue darken-1" text @click="closeChange">Cancelar</v-btn>
              <v-btn color="blue darken-1" text @click="changeItemConfirm">OK</v-btn>
              <v-spacer></v-spacer>
            </v-card-actions>
          </v-card>
        </v-dialog>
      </v-toolbar>
    </template>
    <template v-slot:[`item.actions`]="{ item }">
      <v-icon
        small
        class="mr-2"
        @click="editItem(item)"
      >
        mdi-pencil
      </v-icon>
      <v-icon
        class="mr-2"
        small
        @click="changeItem(item)"
      >
        mdi-clipboard-list-outline
      </v-icon>
      <v-icon
        class="mr-2"
        small
        @click="deleteItem(item)"
      >
        mdi-delete
      </v-icon>
    </template>
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
      dialogChange: false,
      cargando : true,
      headers: [
        { text: 'ID', value: 'id' },
        { text: 'Código', value: 'codigo' },
        { text: 'Nombre', value: 'nombre' },
        { text: 'Estado', value: 'estado' },
        { text: 'Categoria', value: 'categoria.nombre' },
        { text: 'Acciones', value: 'actions', sortable: false },
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
      changeItem (item) {
        this.editedIndex = this.articulos.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogChange = true
      },
      deleteItem (item) {
        this.editedIndex = this.articulos.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },
      async changeItemConfirm () {
        if (this.editedItem.estado === 1) {
          await this.$http.put('/api/articulo/deactivate', {id: this.editedItem.id})
        } else {
          await this.$http.put('/api/articulo/activate', {id: this.editedItem.id})
        }
        this.list()
        this.closeChange()
      },
      async deleteItemConfirm () {
        await this.$http.put('/api/articulo/eliminate', {id: this.editedItem.id})
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
      closeChange () {
        this.dialogChange = false
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
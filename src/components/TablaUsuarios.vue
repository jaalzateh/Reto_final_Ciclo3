<template>
  <v-data-table
    :headers="headers"
    :items="usuarios"
    sort-by="id"
    class="elevation-1"
    :loading="cargando"
    loading-text="Cargando... Por favor, espere"
  >
    <template v-slot:top>
      <v-toolbar
        flat
      >
        <v-toolbar-title>USUARIOS</v-toolbar-title>
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
              Agregar Usuario
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
                      label="Nombre usuario"
                    ></v-text-field>
                    <v-text-field
                      v-model="editedItem.password"
                      label="Contraseña"
                    ></v-text-field>
                </v-row>
                <v-row>
                    <v-text-field
                      v-model="editedItem.email"
                      label="Correo"
                    ></v-text-field>
                    <v-text-field
                      v-model="editedItem.rol"
                      label="Rol"
                    ></v-text-field>
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
                Cancel
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
        { text: 'Rol', value: 'rol' },
        { text: 'Nombre', value: 'nombre' },
        { text: 'Correo Electrónico', value: 'email' },
        { text: 'Estado', value: 'estado' },
        { text: 'Contraseña', value: 'password' },
        { text: 'Acciones', value: 'actions', sortable: false },
      ],
      usuarios: [],
      editedIndex: -1,
      editedItem: {
        nombre: '',
        id: 0,
        rol: "",
        nombre: "",
        email: "",
        password: "",
        estado: 0,
      },
      defaultItem: {
        nombre: '',
        id: 0,
        rol: "",
        nombre: "",
        email: "",
        password: "",
        estado: 0,
      },
    }),
    computed: {
      formTitle () {
        return this.editedIndex === -1 ? 'Nuevo Usuario' : 'Editar Usuario'
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

      },
      list(){
        this.$http.get('/api/usuario/list')
          .then( (response) => {
            this.usuarios = response.data
            this.cargando = false
          })
          .catch(error => {
            return error
          })
          
      },
      editItem (item) {
        this.editedIndex = this.usuarios.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialog = true
      },
      changeItem (item) {
        this.editedIndex = this.usuarios.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogChange = true
      },
      deleteItem (item) {
        this.editedIndex = this.usuarios.indexOf(item)
        this.editedItem = Object.assign({}, item)
        this.dialogDelete = true
      },
      async changeItemConfirm () {
        if (this.editedItem.estado === 1) {
          await this.$http.put('/api/usuario/deactivate', {id: this.editedItem.id})
        } else {
          await this.$http.put('/api/usuario/activate', {id: this.editedItem.id})
        }
        this.list()
        this.closeChange()
      },
      async deleteItemConfirm () {
        await this.$http.put('/api/usuario/eliminate', {id: this.editedItem.id})
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
      closeChange () {
        this.dialogChange = false
        this.$nextTick(() => {
          this.editedItem = Object.assign({}, this.defaultItem)
          this.editedIndex = -1
        })
      },
      async save () {
        if (this.editedIndex > -1) {
          Object.assign(this.usuarios[this.editedIndex], this.editedItem)

          let objetoBusqueda = {
            nombre : this.editedItem.nombre,
            email : this.editedItem.email,
            rol: this.editedItem.rol,
            id : this.editedItem.id
          }
          await this.$http.put('/api/usuario/update', objetoBusqueda)

        } else {
          let objetoBusqueda = {
            nombre : this.editedItem.nombre,
            email : this.editedItem.email,
            password: this.editedItem.password,
            rol: this.editedItem.rol,
            estado: 1
          }
          await this.$http.post('/api/usuario/add', objetoBusqueda)
          this.usuarios.push(this.editedItem)
        }
        this.list()
        this.close()
      },
    },
  }
</script>
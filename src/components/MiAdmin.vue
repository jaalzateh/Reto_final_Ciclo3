<template>
    <div>
    <v-navigation-drawer
      v-model="drawer"
      app
    >
      <!--  -->
      <v-card
    class="mx-auto"
    width="300"
  >
    <v-list>
      <v-list-item
      to="home">
        <v-list-item-icon>
          <v-icon>mdi-home</v-icon>
        </v-list-item-icon>

        <v-list-item-title>Home</v-list-item-title>
      </v-list-item>

      <v-list-item
      to="login">
        <v-list-item-icon>
          <v-icon>mdi-login</v-icon>
        </v-list-item-icon>

        <v-list-item-title>Log In</v-list-item-title>
      </v-list-item>

      <v-list-item
      @click="salirSesion()">
        <v-list-item-icon>
          <v-icon>mdi-logout</v-icon>
        </v-list-item-icon>

        <v-list-item-title>Log out</v-list-item-title>
      </v-list-item>

      <v-list-group
        :value="true"
        prepend-icon="mdi-account-circle"
      >
        <template v-slot:activator>
          <v-list-item-title>Users</v-list-item-title>
        </template>

        <v-list-group
          :value="true"
          no-action
          sub-group
        >
          <template v-slot:activator>
            <v-list-item-content>
              <v-list-item-title>Admin</v-list-item-title>
            </v-list-item-content>
          </template>

          <v-list-item
            v-for="([title, icon, ruta], i) in admins"
            :to="ruta"
            :key="i"
            link
          >
            <v-list-item-title v-text="title"></v-list-item-title>

            <v-list-item-icon>
              <v-icon v-text="icon"></v-icon>
            </v-list-item-icon>
          </v-list-item>
        </v-list-group>

        <v-list-group
          no-action
          sub-group
        >
          <template v-slot:activator>
            <v-list-item-content>
              <v-list-item-title>Actions</v-list-item-title>
            </v-list-item-content>
          </template>

          <v-list-item
            v-for="([title, icon, ruta], i) in cruds"
            :key="i"
            :to="ruta"
            link
          >
            <v-list-item-title v-text="title"></v-list-item-title>

            <v-list-item-icon>
              <v-icon v-text="icon"></v-icon>
            </v-list-item-icon>
          </v-list-item>
        </v-list-group>
      </v-list-group>
    </v-list>
  </v-card>
    </v-navigation-drawer>

    <v-app-bar app>
      <v-app-bar-nav-icon @click="drawer = !drawer"></v-app-bar-nav-icon>

      <v-toolbar-title>Mi Pagina de Control</v-toolbar-title>
    </v-app-bar>

    <v-main>
      <!--  -->
      <v-container fluid>

      <!-- If using vue-router -->
      <router-view></router-view>
    </v-container>
    </v-main>
    </div>
</template>

<script>
export default {
  name: 'App',

  components: {
  },

  data: () => ({
    drawer: null ,
    salirSesion(){
      localStorage.removeItem('token');
      this.$router.push('/')
    },
    admins: [
        ['Usuarios', 'mdi-account-multiple-outline', 'usuarios'],
        ['Settings', 'mdi-cog-outline'],
      ],
      cruds: [
        ['Articulos', 'mdi-cart', 'articulos'],
        ['Categorias', 'mdi-view-list', 'categorias'],
      ],
  }),
};

</script>
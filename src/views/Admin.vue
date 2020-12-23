<template>
  <mi-admin></mi-admin>
</template>

<script>
import MiAdmin from '../components/MiAdmin.vue';

export default {
  name: 'Admin',

  components: {
    MiAdmin
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

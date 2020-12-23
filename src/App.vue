<template>
  <v-app id="inspire">
    <v-app-bar
      app
      color="white"
      flat
    >
      <v-container class="py-0 fill-height">
        <v-avatar
          class="mr-10"
          color="grey darken-1"
          size="50"
        >
        <a href="/home"><img src="https://cdn.shopify.com/shopifycloud/hatchful-web/assets/6fcc76cfd1c59f44d43a485167fb3139.png" alt="logo"></a>
        </v-avatar>

        <v-btn
          v-for="([link, route]) in links"
          :key="link"
          :to="route"
          text
        >
          {{ link }}
        </v-btn>

        <v-spacer></v-spacer>

        <v-avatar
          class="mr-10"
          color="grey darken-1"
          size="30"
        >
        </v-avatar>
      </v-container>
    </v-app-bar>

    <v-main class="grey lighten-3">
      <v-container>
        <v-row>
          <v-col>
            <v-sheet
              min-height="70vh"
              rounded="lg"
            >
              <!--  -->
              <router-view></router-view>
            </v-sheet>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    name: 'App',
    components: {
      
    },
    data: () => ({
      links: [
        ['Servicios', '/servicios'],
        ['Caso de Exito', '/casos'],
        ['Contacto', '/contacto'],
        ['Sobre Nosotros', '/acerca']
      ],
    }),
  }
</script>
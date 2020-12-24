<template>


    <v-card
      class="mx-auto"
      max-width="344"
    >
      <v-img
        src="https://image.freepik.com/foto-gratis/pizarra-sacacorchos-corchos-cerca-vino_23-2147764882.jpg"
        height="200px"
      ></v-img>
  
      <v-card-title>
        Etiquetado térmico  
      </v-card-title>
  
      <v-card-subtitle>
        Solución para disfrutar un vino en la mejor temperatura
      </v-card-subtitle>
  
      <v-card-actions>
        <v-btn
          color="orange lighten-2"
          text
        >
          Mostrar más
        </v-btn>
  
        <v-spacer></v-spacer>
  
        <v-btn
          icon
          @click="show = !show"
        >
          <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
        </v-btn>
      </v-card-actions>
  
      <v-expand-transition>
        <div v-show="show">
          <v-divider></v-divider>
  
          <v-card-text>
            Los viñedos se encuentran catalogados como los principales consumidores de nuestros productos. El efiiente etiquetado térmico que ofrecemos, permite que los clientes disfruten de un vino a una temperatura excelente para la cata. Además, son de fácil personalización de acuerdo a las necesidades de marketing específicas.
          </v-card-text>
        </div>
      </v-expand-transition>
    </v-card>

</template>


<script>
export default {
    name: 'CasoExito',
    data () {
        return {
            show: false,
        }

    },
}
</script>



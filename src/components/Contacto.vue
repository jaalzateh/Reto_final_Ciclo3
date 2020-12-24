<template>


    <v-card
      class="mx-auto"
      max-width="344"
    >
      <v-img
        src="https://image.freepik.com/foto-gratis/retrato-mujer-trabajadora-servicio-al-cliente_144627-37943.jpg"
        height="200px"
      ></v-img>
  
      <v-card-title>
        ¡Contáctanos para saber más de nuestros productos!  
      </v-card-title>
  
      <v-card-subtitle>
        Puedes comunicarte con nosotros y hacer parte de una experiencia increíble
      </v-card-subtitle>
  
      <v-card-actions>
        <v-btn
          color="orange lighten-2"
          text
        >
          CANALES DE ATENCIÓN 
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
            Correo electrónico: servicioalcliente@sdppsas.com            
          </v-card-text>
          <v-card-text>
            Teléfono: (+57 4) 322-55-66 - Bogotá 01-8000-930-5466 Resto del país 
          </v-card-text>
          <v-card-text>
            WhatsApp: +57-322-222-22-22
          </v-card-text>
          <v-card-text>
            Instagram: @Soluciones_dpp_sas
          </v-card-text>
        </div>
      </v-expand-transition>
    </v-card>

</template>


<script>
export default {
    name: 'Contacto',
    data () {
        return {
            show: false,
        }

    },
}
</script>



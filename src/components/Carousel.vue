<template>
  <v-carousel>
    <v-carousel-item
      v-for="(item, i) in items"
      :key="i"
      :src="item.src"
      reverse-transition="fade-transition"
      transition="fade-transition"
    ></v-carousel-item>
  </v-carousel>
</template>

<script>
export default {
  name: "Carousel",
  data () {
    return {
      items: [
        {
          src: 'https://image.freepik.com/psd-gratis/batidos-alimentacion-saludable-concepto-desintoxicacion-etiquetas_23-2148310927.jpg',          
        },
        {
          src: 'https://image.freepik.com/psd-gratis/fondo-productos-belleza-sobre-fondo-madera_23-2147979400.jpg',
        },
        {
          src: 'https://image.freepik.com/vector-gratis/coleccion-etiquetas-rebajas-navidenas-diseno-plano_52683-52910.jpg',
        },
        {
          src: 'https://image.freepik.com/psd-gratis/maqueta-logo-embos-etiqueta-bolso_214084-67.jpg',
        },
      ],
    }
  },
};
</script>
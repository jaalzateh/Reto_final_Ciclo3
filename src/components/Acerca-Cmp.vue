<template>
  <div class="about">
    <v-card>
      <v-footer v-bind="localAttrs" :padless="padless">
        <v-card flat tile width="100%" class="text-center">
          <v-card-text>
            <v-btn
              v-for="[icon, target] in icons"
              :key="icon"
              :href="target"
              class="mx-4"
              icon
            >
              <v-icon size="24px">
                {{ icon }}
              </v-icon>
            </v-btn>
          </v-card-text>

          <v-divider></v-divider>

          <v-card-text class="green--text">
            {{ new Date().getFullYear() }} — Madrid / Bogotá / Medellín
            <strong
              >
              Contacto: +34 672 9121
              </strong
            >
          </v-card-text>
        </v-card>
      </v-footer>
    </v-card>
  </div>
</template>
<script>
export default {
  name: "Acerca",
  data: () => ({
    icons: [
      ["mdi-email", "mailto:c-arias@outlook.com"],
      ["mdi-github", "https://github.com/jaalzateh/Reto_final_Ciclo3"]
    ],
    items: ["default", "absolute", "fixed"],
    padless: false,
    variant: "default",
  }),
  computed: {
    localAttrs() {
      const attrs = {};

      if (this.variant === "default") {
        attrs.absolute = false;
        attrs.fixed = false;
      } else {
        attrs[this.variant] = true;
      }
      return attrs;
    },
  },
};
</script>
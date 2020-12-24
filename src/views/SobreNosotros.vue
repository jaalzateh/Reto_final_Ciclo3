<template>
    <div class="container-fluid" >
      <div>

        <h1 style= "text-align:center" >SOLUCIONES EN DISEÑO Y PERSONALIZACIÓN DE PRODUCTOS S.A.S.</h1>
        <p>Somos una empresa dedicada a añadir valor agregado a todos tus productos a través de una serie de herramientas creativas de personalización y diseño, usando como insumo principal el papel. Nos hemos posicionado en el mercado como una marca práctica, que brinda soluciones eficientes y satisface las necesidades del cliente, dejando muy a gusto sus segmentos de mercado.</p>
        <h2 style= "text-align:center" font="arial" >FUNDADORES</h2>

      </div>
      <div class="row justify-content-center mb-5">
        <div class="col mt-5" v-for="(miembro, index) of Team" :key="index">
          <sobre-nosotros :member="miembro"></sobre-nosotros>
        </div>
      </div>
    </div>


</template> 

<script>

import SobreNosotros from '../components/SobreNosotros.vue';

export default {
  components: {

    SobreNosotros,

  },
  data() {
    return {
      //Archivo JSON con nuestra información
      Team: [
        {
          codigo: 1,
          nombre: "Andrés Felipe Pinzón Harker",
          descripcion:
            "Estudiante de 5to semestre en física de la Universidad Nacional de Colombia con técnico en sistemas y aficionado a la informática",
          rol: "Arquitecto",
          image: "https://i.imgur.com/3TBQvso.jpg",
          page: "https://github.com/PinzonHarker/",
        },
        {
          codigo: 2,
          nombre: "Carlos Andres Arias Urrego",
          descripcion:
            "Ingeniero de Sistemas especializado en Gerencia de Proyecto de la universidad EAFIT. Preventa de Middleware. Actualizando conocimientos.",
          rol: "Desarrollador fullstack",
          image: "https://media-exp1.licdn.com/dms/image/C4E03AQFld_Dw1Z2_BQ/profile-displayphoto-shrink_400_400/0/1566832676656?e=1613606400&v=beta&t=_cpjscEHgMhDqm77RUsrTPx-lFwjas1QJnFIZFiMM30",
          page: "https://www.linkedin.com/in/carlosa-ariasu/",
        },
        {
          codigo: 4,
          nombre: 'Jorge Andrés Alzate Hoyos',
          descripcion: 'Ingeniero Ambiental de la Universidad Nacional, estudiante de Maestría en Medio Ambiente y Desarrollo, apasionado por la programación.',
          rol: 'Desarrollador backend',
          image: 'https://media-exp1.licdn.com/dms/image/C5103AQHdpDeyFKJqYA/profile-displayphoto-shrink_800_800/0/1516889594207?e=1613606400&v=beta&t=F3NVOAhWeKM0wYvblyho_7KSSnYTDI_G7xvLJIoxoYU',
          page:"https://github.com/jaalzateh"
        },
      ],
    };
  },
};
</script>
<template>
  <div class="container">
    <div class="mb-3">
      <label for="exampleFormControlInput1" class="form-label"
        >Correo Electrónico</label
      >
      <input
        v-model="login.email"
        type="email"
        class="form-control"
        id="exampleFormControlInput1"
        placeholder="name@example.com"
      />
    </div>
    <div class="mb-3">
      <label for="exampleFormControlInput2" class="form-label">Contraseña</label>
      <input
      v-model="login.password"
        type="password"
        class="form-control"
        id="exampleFormControlInput2"
        placeholder="password"
      />
    </div>
    <v-btn
      color="primary"
      elevation="5"
      rounded
      @click="iniciarSesion()"
    >Ingresar</v-btn>
  </div>
</template>

<script>
import swal from 'sweetalert';

export default {
  name: "TheLogin",
  data() {
    return {
      login: {
        email: "",
        password: "",
      },
    }
  },
  methods:{
      async iniciarSesion(){
          // Función de iniciar sesión
          
              await this.$http.post('/api/usuario/login', this.login)
                .then((response) => {
                let token = response.data.tokenReturn
                let usuario = response.data.user
                if (token) {
                    localStorage.setItem('token', token)
                    localStorage.setItem('usuario', JSON.stringify(usuario))
                    swal("Bienvenido de nuevo!", "Ha ingresado correctamente", "success");
                    this.$router.push('/admin/home')
                }
                }).catch((error) => {
                    swal ( "Oops" ,  "Something went wrong!" ,  "error" )
                })
              
          
      }
  }
};
</script>

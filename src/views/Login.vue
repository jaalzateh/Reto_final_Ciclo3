<template>
    <div
    class="container"
    >
        <div
        class="d-flex justify-content-center pt-5 mt-5">
        <div class="form-group text-center"><the-login></the-login></div>
            
        </div>
    </div>
</template>

<script>
import TheLogin from '../components/TheLogin.vue'

export default {
  components: { TheLogin },
    name: "Login"
}
</script>
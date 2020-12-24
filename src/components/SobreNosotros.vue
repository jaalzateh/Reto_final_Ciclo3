<template>
    <div id="members">
    <div class="card text-white bg-secondary">
        <div class="d-sm-flex justify-content-center p-2">
        <img :src="member.image" alt="" width="200" class="rounded-circle">
        </div>
        <div class="card-body">
        <h5 class="card-title">{{ member.nombre}}</h5>
        <p class="card-text">
            {{ member.descripcion }}
        </p>
        </div>
    </div>
    </div>
</template>

<script>
    export default {
        name: "SobreNosotros",
        props: ['member']
    }
</script>


<template>
    <nav>
        <img src="../../assets/img/Logo.svg" alt="" class="logo">
    </nav>

    <div class="main">
        <h1>Seu Resultado</h1>
        <component :is="ResultComponent" />
        <button @click="$router.push('/questions')">REFAZER TESTE</button>
    </div>
</template>

<script setup>
    import { useRoute } from 'vue-router'
    import Colerico from '@/components/about-temperaments/colerico.vue'
    import Melancolico from '@/components/about-temperaments/melancolico.vue'
    import Sanguineo from '@/components/about-temperaments/sanguineo.vue'
    import Fleumatico from '@/components/about-temperaments/fleumatico.vue'

    const route = useRoute()
    const result = route.query.result

    const components = {
    Fleumático: Fleumatico,
    Sanguíneo: Sanguineo,
    Colérico: Colerico,
    Melancólico: Melancolico
    }

    const ResultComponent = components[result]
</script>

<style scoped lang="scss">

@use "/src/styles/variables.scss" as *;
    nav {
        margin-top: 20px;
        display: flex;
        justify-content: center;

        .logo {
            width: 200px;
        }
    }

    .main {
        display: flex;
        flex-direction: column;
        align-items: center;

        h1 {
            margin-top: 20px;
            font-size: 28px;
            color: $main-color;
            font-weight: 700;
        }

        button {
            background-color: $main-color;
            color: #FFF;
            font-weight: 700;
            padding: 7px 20px 7px 20px;
            border: none;
            border-radius: 15px;
        }
    }

    
</style>

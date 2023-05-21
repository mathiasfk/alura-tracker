<template>
    <header>
        <h1>
            <img src="../assets/logo.png" alt="Alura Tracker">
        </h1>
        <button class="button" @click="alterarTema">
            {{ textoBotao }}
        </button>
        <nav class="panel mt-5">
            <ul>
                <li>
                    <RouterLink to="/" class="link">
                        <i class="fas fa-tasks"></i>
                        Tarefas
                    </RouterLink>
                </li>
                <li>
                    <RouterLink to="/projetos" class="link">
                        <i class="fas fa-project-diagram"></i>
                        Projetos
                    </RouterLink>
                </li>
            </ul>
        </nav>
    </header>
</template>

<script lang="ts">
import { defineComponent } from "vue";

export default defineComponent({
    name: "BarraLateral",
    emits: ['aoTemaAlterado'],
    data() {
        return {
            modoEscuro: false,
        }
    },
    computed: {
        textoBotao() {
            return this.modoEscuro ? 'Modo claro' : 'Modo escuro';
        }
    },
    methods: {
        alterarTema() {
            this.modoEscuro = !this.modoEscuro;
            this.$emit('aoTemaAlterado', this.modoEscuro);
        }
    }
})
</script>
<style>
header {
    padding: 1rem;
    background: darkslateblue;
    width: 100%;
    height: 100vh;
    text-align: center;
}

@media only screen and (max-width: 768px) {
    header {
        padding: 2.5rem;
        height: auto;
    }
}

.panel li {
    margin: 8px 0;
}
.link {
    color: white;
}
.link:hover {
    color: #faf0ca
}
.link.router-link-active {
    color: #faf0ca;
}
</style>
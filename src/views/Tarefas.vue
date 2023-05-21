<template>
    <Formulario @ao-salvar-tarefa="salvarTarefa" />
    <div class="lista">
        <Box v-if="listaVazia">
            Você ainda não criou nenhuma tarefa hoje
        </Box>
        <Tarefa v-for="(tarefa, index) in tarefas" :key="index" :tarefa="tarefa" />
    </div>
</template>
  
<script lang="ts">
import { defineComponent } from 'vue';
import Formulario from '@/components/Formulario.vue';
import Tarefa from '@/components/Tarefa.vue';
import ITarefa from '@/interfaces/ITarefa';
import Box from '@/components/Box.vue';


export default defineComponent({
    name: 'App',
    components: {
        Formulario,
        Tarefa,
        Box
    },
    data() {
        return {
            tarefas: [] as ITarefa[],
            modoEscuro: false,
        }
    },
    computed: {
        listaVazia(): boolean {
            return this.tarefas.length == 0;
        }
    },
    methods: {
        salvarTarefa(tarefa: ITarefa) {
            this.tarefas.push(tarefa);
        },
        trocarTema(modoEscuro: boolean) {
            this.modoEscuro = modoEscuro;
        }
    }
});
</script>
  
<style>
.lista {
    padding: 1.25rem
}

main {
    --bg-primario: white;
    --texto-primario: black;
}

main.modo-escuro {
    --bg-primario: rgb(7, 7, 40);
    --texto-primario: #ddd;
}

.conteudo {
    background-color: var(--bg-primario);
}
</style>
  
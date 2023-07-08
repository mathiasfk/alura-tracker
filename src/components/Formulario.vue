<template>
    <div class="box formulario">
        <div class="columns">
            <div class="column is-5" role="form" aria-label="Formulario para criaçao de uma nova tarefa">
                <input type="text" class="input" placeholder="Qual tarefa voce deseja iniciar?" v-model="descricao">
            </div>
            <div class="column is-3">
                <div class="select">
                    <select v-model="idProjeto">
                        <option value="">Selecione o projeto</option>
                        <option :value="projeto.id" v-for="projeto in projetos" :key="projeto.id">
                            {{ projeto.nome }}
                        </option>
                    </select>
                </div>
            </div>
            <div class="column">
                <Temporizador @ao-temporizador-finalizado="finalizarTarefa"/>
            </div>
        </div>
    </div>
</template>

<script lang="ts">
import { defineComponent } from 'vue';
import Temporizador from './Temporizador.vue';
import ITarefa from '@/interfaces/ITarefa';
import { useStore } from 'vuex';
import { key } from '@/store';
import { computed } from '@vue/reactivity';
import { TipoNotificacao } from '@/interfaces/INotificacao';
import { NOTIFICAR } from '@/store/tipo-mutacoes';

export default defineComponent({
    name: "Formulario",
    emits: ['aoSalvarTarefa'],
    components: {
        Temporizador
    },
    data() {
        return {
            descricao: '',
            idProjeto: ''
        }
    },
    methods: {
        finalizarTarefa(tempoDecorrido: number) : void {
            const projeto = this.projetos.find(p => p.id == this.idProjeto);

            if(!projeto){
                this.commit(NOTIFICAR, {
                    titulo: 'Tarefa sem projeto',
                    texto: 'A tarefa precisa de um projeto associado para ser salva.',
                    tipo: TipoNotificacao.ATENCAO,
                });
                return;
            }
            this.$emit('aoSalvarTarefa',{
                duracaoEmSegundos: tempoDecorrido,
                descricao: this.descricao,
                projeto: this.projetos.find(p => p.id == this.idProjeto)
            } as ITarefa)
            this.descricao = '';
        }
    },
    setup() {
        const store = useStore(key);
        return {
            commit: store.commit,
            projetos: computed(() => store.state.projetos)
        }
    }
})
</script>

<style>
.formulario {
    color: var(--texto-primario);
    background-color: var(--bg-primario);
}
</style>
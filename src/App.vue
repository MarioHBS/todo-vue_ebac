<script setup>
import { reactive } from 'vue';
import Cabecalho from './components/Cabecalho.vue'
import Formulario from './components/Formulario.vue'
import ListaDeTarefas from './components/ListaDeTarefas.vue'

const estado = reactive({
  filtro: 'todas',
  tarefaTemp: '',
  tarefas: [
    {
      titulo: 'Estudar ES6',
      finalizada: false,
    },
    {
      titulo: 'Estudar SAAS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,
    },
  ]
})

const tarefasPendentes = () => {
  return estado.tarefas.filter(tarefa => !tarefa.finalizada) // se for falsa
}
const tarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada === true) // se for verdadeira
}

const tarefasFiltradas = () => {
  const { filtro } = estado;
  switch (filtro) {
    case 'pendentes': return tarefasPendentes();
    case 'finalizadas': return tarefasFinalizadas();
    default: return estado.tarefas
  }
}

const cadastrarTarefa = () => {
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  estado.tarefaTemp = ''
  estado.tarefas.push(tarefaNova);
}

</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="tarefasPendentes().length" />
    <Formulario
      :tarefa-temp="estado.tarefaTemp"
      :edita-tarefa-temp="event => estado.tarefaTemp = event.target.value"
      :cadastra-tarefa="cadastrarTarefa"
      :trocarFiltro="event => estado.filtro = event.target.value"
    />
    <ListaDeTarefas :tarefas="tarefasFiltradas()" />
  </div>

</template>
<script setup>
import { reactive } from 'vue';

const estado = reactive ({
  filtro: 'todas',
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

</script> 

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>
        Você possui {{ tarefasPendentes().length }} tarefas pendentes
      </p>
    </header>

    <form>
      <div class="row">
        <div class="col">
          <input type="text" placeholder="Digite aqui a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-1">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-2">
          <select @change="event => estado.filtro = event.target.value" class="form-control" id="">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>

    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in tarefasFiltradas()">
        <input @change="event => tarefa.finalizada = event.target.checked" :checked="tarefa.finalizada" :id="tarefa.titulo" type="checkbox">
        <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="tarefa.titulo">
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
  
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>

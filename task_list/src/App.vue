<script setup>
import { reactive } from 'vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas:[
      {
        titulo: 'Estudar Vue',
        finalizada: false
      },
      {
        titulo: 'Estudar ES6',
        finalizada: true
      },
      {
        titulo: 'Estudar React',
        finalizada: false
      }
    ]
  })

  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }
  
  const getTarefasFiltradas = () => {
    const { filtro } = estado

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      
      case 'finalizadas':
        return getTarefasFinalizadas();

      default: 
        return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTemp = '';
  }
</script>

<template>
  <div class="container">
      <header class="p-5 mb-4 mt-4 bg-light rounded-3">
        <h1>Minhas tarefas</h1>
        <p>
          Você possui {{ getTarefasPendentes().length }} tarefas pendentes
        </p>
      </header>
      <form @submit.prevent="cadastraTarefa">
        <div class="row">
          <div class="col">
            <input required :value="estado.tarefaTemp" @change="evento => estado.tarefaTemp = evento.target.value" class="form-control" type="text" placeholder="Nome da tarefa">
          </div>
          <div class="col-md-2">
            <button class="btn btn-primary" type="submit">Cadastrar</button>
          </div>
          <div class="col-md-2">
            <select @change="evento => estado.filtro = evento.target.value" class="form-control">
              <option value="todas">Todas</option>
              <option value="pendentes">Pendentes</option>
              <option value="finalizadas">Finalizadas</option>
            </select>
          </div>
        </div>
      </form>
      <ul class="list-group mt-4">
        <li class="list-group-item" v-for="tarefas in getTarefasFiltradas()">
            <input @change="evento => tarefas.finalizada = evento.target.checked" type="checkbox" :checked="tarefas.finalizada" :id="tarefas.titulo">
            <label :class="{ done: tarefas.finalizada }" class="ms-3" :for="tarefas.titulo">
                {{ tarefas.titulo }}
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
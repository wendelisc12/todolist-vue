<script setup>
import { reactive } from 'vue';

  const estado = reactive({
    filtro: 'todas',
    novaTarefa: '',
    tarefas: [
      {
        titulo: "estuda ES6",
        finalizada: false
      },
      {
        titulo: "estudar vue",
        finalizada: false
      },
      {
        titulo: "estudar react",
        finalizada: true
      }
    ]
  })

  const getTarefasPendentes = () =>{
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }

  const filtrarTarefas = () =>{
    const {filtro} = estado

    if(filtro === "pendentes"){
      return getTarefasPendentes()
    }else if(filtro === "finalizadas"){
      return estado.tarefas.filter(tarefa => tarefa.finalizada)
    }else{
      return estado.tarefas
    }
  }

  const cadastrarTarefa = ()=>{
    estado.tarefas.push({
      titulo: estado.novaTarefa,
      finalizada: false
    })

    estado.novaTarefa = ''
  }

</script>

<template>
  <div class="container">
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes</p>
    </header>
    <form action="" @submit.prevent="cadastrarTarefa">
      <div class="row">
        <div class="col">
          <input required type="text" v-model="estado.novaTarefa" placeholder="Digite a descrição da tarefa" class="form-control">
        </div>
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>
        <div class="col-md-3 ">
          <select class="form-control" v-model="estado.filtro">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>
    
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in filtrarTarefas()">
        <input type="checkbox" v-model="tarefa.finalizada" :id="tarefa.titulo" :checked="tarefa.finalizada">
        <label :for="tarefa.titulo" :class="{done:tarefa.finalizada}" class="ms-3">
          {{tarefa.titulo}}
        </label>
      </li>
    </ul>
  </div>

</template>
 
<style scoped>
  .done{
    text-decoration: line-through;
  }
</style>

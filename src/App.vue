<script setup>
import { reactive } from 'vue';

  const estado = reactive( {
    filtro: 'todas',
    tarefaTemporaria: '',     //introduzindo um novo objeto que sera a tarefa temporaria que o usuario esta cadastrando 
    
    tarefas: [
    { titulo: 'Estudar ES6', finalizada: false },
    { titulo: 'Estudar SASS', finalizada: false },
    { titulo: 'ir para a academia', finalizada: true },
    ]    
  })

  // retorna uma nova lista que contém apenas as tarefas que ainda não foram finalizadas
  const getTarefasPendentes = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)  // ou tarefa.finalizada === false
  }
  // é uma função de callback que é passada para o filter. Para cada item da lista de tarefas, essa função verifica se finalizada é false. 
  // Se for, a tarefa é incluída no novo array que filter retorna.

  const getTarefasFinalizadas = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)  // ou tarefa.finalizada === false
  }

  const getTarefasFiltradas = () => { 
    const {filtro} = estado;

    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();

      case 'finalizadas':
        return getTarefasFinalizadas();

        default:
          return estado.tarefas
    }
  }

  const cadastraTarefa = () => {
    const novaTarefa = { 
      titulo: estado.tarefaTemporaria,
      finalizada: false,
    }

    estado.tarefas.push(novaTarefa);    // introduzindo (push) um novo item a array tarefas! 
    estado.tarefaTemporaria = ' ';  //limpando a caixa q introduzimos a tarefa temporaria

  }
</script>



<template>
  <div class="container">
<!-- cabecalho -->
      <header class="p-5 mb-4 mt-4 bg-light rounded-3">
<!-- bg-light: Define o fundo do elemento como branco ou cinza claro. rounded-3: Aplica um arredondamento de borda. -->
        <h1> Minhas tarefas </h1>
        <p> Voce possui {{ getTarefasPendentes().length }} tarefas pendentes </p>
      </header>
    
      <!-- formulario  -->
    <form @submit.prevent="cadastraTarefa">
      <!-- prevent para corrigir o problema da atualizacao de formulario. -->
      <div class="row">
        <div class="col"> 
          <input type="text" class="form-control" required placeholder="Digite aqui a descrição da tarefa" @change="evento => estado.tarefaTemporaria = evento.target.value" :value="estado.tarefaTemporaria" >
        </div>
        <!-- :value="estado.tarefaTemporaria add para podermos limpar o campo dps q for add uma nova tarefa  -->
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary"> Cadastrar </button>
        </div>        

        <div class="col-md-2">      
          <!-- vamos criar uma caixa de selecionar os filtros a exibir: todas, pendentes ou finalizadas      -->
          <select class="form-control" @change="evento => estado.filtro = evento.target.value">
            <option value="todas as tarefas"> Todas as tarefas </option>
            <option value="pendentes"> Pendentes </option>
            <option value="finalizadas"> Finalizadas </option>
          </select> 
        </div>
      </div>
    </form>

    <!-- //lista de tarefas  -->
    <ul class="list-group mt-4">
      <li class="list-group-item" v-for="tarefa in getTarefasFiltradas()"> 
        <!-- listando a array com a diretiva v-for  -->
        
        <input type="checkbox" :id="tarefa.titulo" :checked="tarefa.finalizada" @change=" evento => tarefa.finalizada = evento.target.checked">  
    <!-- no input type checkbox nao usamos o VALUE e sim o CHECKED.
    Com esse evento, podemos selecionar no site as tarefas q já foram finalizadas, e elas ja ficarão salvas!  -->

        <label class="ms-3" :for="tarefa.titulo" :class="{ done: tarefa.finalizada === true }" > 
        <!-- o comando Done so ira acontecer se a condicao de tarefa.finalizada for true. -->
          {{tarefa.titulo}} 
          <!-- tarefa.titulo = estudar ES6 ou estudar Sass ou ir p academia  -->
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

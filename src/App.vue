<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue';
  import Formulario from './components/Formulario.vue';
  import ListaTarefa from './components/ListaTarefa.vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTmp: '',
    tarefas: [
      {
        titulo: 'Estudar ES6',
        finalizada: false,
      },
      {
        titulo: 'Estudar SASS',
        finalizada: false,
      },
      {
        titulo: 'Ir para academia',
        finalizada: true,
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
    const {filtro} = estado;
    switch (filtro) {
      case 'pendentes':
        return getTarefasPendentes();
      case 'finalizadas':
        return getTarefasFinalizadas();
      default: 
      return estado.tarefas;
    }
  }

  const cadastrarTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTmp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova);
    estado.tarefaTmp = '';
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length" />
    <Formulario :trocar-filtro="evento => estado.filtro =evento.target.value" :tarefa-temp="estado.tarefaTmp" :edit-tarefa-temp="evento => estado.tarefaTmp = evento.target.value" :cadastrar-tarefa="cadastrarTarefa"/>
    <ListaTarefa :tarefas="getTarefasFiltradas()" :filtro="estado.filtro"/>
  </div>
</template>
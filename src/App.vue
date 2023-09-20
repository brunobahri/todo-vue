<script setup>
  import { reactive } from 'vue';
  import Cabecalho from './components/Cabecalho.vue'
  import Formulario from './components/Formulario.vue';
  import ListaDeTarefas from './components/ListaDeTarefas.vue';

  const estado = reactive({
    filtro: 'todas',
    tarefaTemp: '',
    tarefas: [
      {
        titulo: 'Estudar1',
        finalizada: false,
      },
      {
        titulo: 'Estudar2',
        finalizada: false,
      },
      {
        titulo: 'Ir a academia',
        finalizada: true,
      },
    ]
  })

  const getTarefasPend = () => {
    return estado.tarefas.filter(tarefa => !tarefa.finalizada)
  }
  const getTarefasFin = () => {
    return estado.tarefas.filter(tarefa => tarefa.finalizada)
  }

  const getTarefasFil = () => {
    const {filtro} = estado

    switch (filtro) {
      case 'pendentes':
        return getTarefasPend();
      case 'finalizadas':
        return getTarefasFin();
      default: 
      return estado.tarefas;
    }
  }

  const cadastraTarefa = () => {
    const tarefaNova = {
      titulo: estado.tarefaTemp,
      finalizada: false,
    }
    estado.tarefas.push(tarefaNova)
    estado.tarefaTemp = ''
  }
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPend().length" />
    <Formulario :trocar-filtro="evento => estado.filtro = evento.target.value" :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa"/>
    <ListaDeTarefas :tarefas="getTarefasFil()"/>
  </div>
</template>

<style scoped>
.done {
  text-decoration: line-through;
}
</style>

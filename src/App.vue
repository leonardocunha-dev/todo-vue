<script setup>
// Importa a função `reactive` do Vue 3, que permite criar objetos reativos.
// Reatividade é essencial para que a UI seja atualizada automaticamente quando os dados mudam.
import { reactive } from 'vue';

import Cabecalho from './components/Cabecalho.vue';

import Formulario from './components/Formulario.vue';

import ListaDeTarefas from './components/ListaDeTarefas.vue';


// Criação de um estado reativo centralizado usando `reactive`.
// Esse estado será utilizado para controlar a lista de tarefas, filtros e o campo temporário do input.
const estado = reactive({
  // Define o filtro atual aplicado às tarefas ('todas', 'pendentes', 'finalizadas')
  filtro: 'todas',
  // Armazena temporariamente o valor do input de nova tarefa
  tarefaTemp: '',
  // Lista de tarefas da aplicação
  tarefas: [
    {
      titulo: 'Estudar ES6',       // Descrição da tarefa
      finalizada: false,           // Estado inicial: não finalizada
    },
    {
      titulo: 'Estudar SASS',
      finalizada: false,
    },
    {
      titulo: 'Ir para a academia',
      finalizada: true,           // Esta tarefa está marcada como finalizada
    }
  ]
})

// Função que retorna apenas as tarefas que ainda não foram finalizadas
const getTarefasPendentes = () => {
  // Usa `filter` para retornar um novo array com tarefas onde `finalizada` é `false`
  return estado.tarefas.filter(tarefa => !tarefa.finalizada)
}

// Função que retorna apenas as tarefas já concluídas
const getTarefasFinalizadas = () => {
  return estado.tarefas.filter(tarefa => tarefa.finalizada)
}

// Função responsável por retornar as tarefas de acordo com o filtro selecionado pelo usuário
const getTarefasFiltradas = () => {
  const { filtro } = estado;

  // O switch case avalia o valor atual do filtro e retorna a lista adequada
  switch (filtro) {
    case 'pendentes':
      return getTarefasPendentes();  // Apenas tarefas pendentes
    case 'finalizadas':
      return getTarefasFinalizadas(); // Apenas tarefas finalizadas
    default:
      return estado.tarefas;          // Todas as tarefas (valor padrão)
  }
}

// Função para cadastrar uma nova tarefa na lista
const cadastraTarefa = () => {
  // Cria um novo objeto tarefa com título e status inicial
  const tarefaNova = {
    titulo: estado.tarefaTemp,
    finalizada: false,
  }
  // Adiciona a nova tarefa no array de tarefas
  estado.tarefas.push(tarefaNova);
  // Limpa o campo de input após adicionar
  estado.tarefaTemp = '';
}
</script>

<template>
  <div class="container">
    <Cabecalho :tarefas-pendentes="getTarefasPendentes().length"/>
    <Formulario :tarefa-temp="estado.tarefaTemp" :edita-tarefa-temp="evento => estado.tarefaTemp = evento.target.value" :cadastra-tarefa="cadastraTarefa" :trocar-filtro="evento => estado.filtro =evento.target.value"/>
    <ListaDeTarefas :tarefas="getTarefasFiltradas()"/>
  </div>
</template>


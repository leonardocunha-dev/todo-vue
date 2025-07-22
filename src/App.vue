<script setup>
// Importa a função `reactive` do Vue 3, que permite criar objetos reativos.
// Reatividade é essencial para que a UI seja atualizada automaticamente quando os dados mudam.
import { reactive } from 'vue'

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
    <!-- Cabeçalho da aplicação -->
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <!-- Exibe quantidade de tarefas pendentes em tempo real -->
      <p>Você possui {{ getTarefasPendentes().length }} tarefas pendentes.</p>
    </header>

    <!-- FORMULÁRIO DE CADASTRO DE TAREFAS -->
    <!-- O modificador @submit.prevent impede o comportamento padrão do form (recarregar a página) -->
    <form @submit.prevent="cadastraTarefa">
      <div class="row">
        <div class="col">
          <!-- Campo de input vinculado ao estado reativo `estado.tarefaTemp` -->
          <!-- O evento @change atualiza o valor digitado no estado -->
          <input
            :value="estado.tarefaTemp"
            @change="evento => estado.tarefaTemp = evento.target.value"
            required
            class="form-control"
            type="text"
            placeholder="Digite a descrição da tarefa"
          />
        </div>

        <!-- Botão de cadastro de nova tarefa -->
        <div class="col-md-2">
          <button type="submit" class="btn btn-primary">Cadastrar</button>
        </div>

        <!-- Select para aplicar filtro nas tarefas -->
        <div class="col-md-2">
          <!-- Ao alterar o valor do select, o estado `estado.filtro` é atualizado -->
          <select @change="evento => estado.filtro = evento.target.value" class="form-control">
            <option value="todas">Todas tarefas</option>
            <option value="pendentes">Pendentes</option>
            <option value="finalizadas">Finalizadas</option>
          </select>
        </div>
      </div>
    </form>

    <!-- LISTAGEM DAS TAREFAS -->
    <!-- O `v-for` percorre a lista de tarefas filtradas dinamicamente -->
    <ul class="list-group mt-4">
      <li
        class="list-group-item"
        v-for="tarefa in getTarefasFiltradas()"
        :key="tarefa.titulo" <!-- `key` melhora performance e controle de atualização -->
      >
        <!-- Checkbox para marcar tarefa como finalizada ou não -->
        <!-- Atualiza `tarefa.finalizada` conforme o status do checkbox -->
        <input
          @change="evento => tarefa.finalizada = evento.target.checked"
          :checked="tarefa.finalizada"
          :id="tarefa.titulo"
          type="checkbox"
        />

        <!-- Label da tarefa com estilo condicional -->
        <!-- A classe `.done` será aplicada se `tarefa.finalizada` for `true` -->
        <label
          :class="{ done: tarefa.finalizada }"
          class="ms-3"
          :for="tarefa.titulo"
        >
          {{ tarefa.titulo }}
        </label>
      </li>
    </ul>
  </div>
</template>

<style scoped>
/* Estilo aplicado apenas a este componente (por causa do `scoped`) */
/* Estilo visual que risca o texto da tarefa finalizada */
.done {
  text-decoration: line-through;
}
</style>

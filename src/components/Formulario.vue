<script setup>
  const props = defineProps(['cadastraTarefa', 'tarefaTemp', 'trocarFiltro', 'editaTarefaTemp'])
</script>

<template>
  <!-- FORMULÁRIO DE CADASTRO DE TAREFAS -->
  <!-- O modificador @submit.prevent impede o comportamento padrão do form (recarregar a página) -->
  <form @submit.prevent="props.cadastraTarefa">
    <div class="row">
      <div class="col">
        <!-- Campo de input vinculado ao estado reativo `estado.tarefaTemp` -->
        <!-- O evento @change atualiza o valor digitado no estado -->
        <input
          :value="props.tarefaTemp"
          @change="props.editaTarefaTemp"
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
        <select @change="props.trocarFiltro" class="form-control">
          <option value="todas">Todas tarefas</option>
          <option value="pendentes">Pendentes</option>
          <option value="finalizadas">Finalizadas</option>
        </select>
      </div>
    </div>
  </form>
</template>

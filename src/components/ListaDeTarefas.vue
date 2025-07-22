<script setup>
  const props = defineProps(['tarefas']);
</script>

<template>
  <!-- LISTAGEM DAS TAREFAS -->
  <!-- `v-for` percorre a lista de tarefas filtradas -->
  <ul class="list-group mt-4">
    <li class="list-group-item" v-for="tarefa in props.tarefas" :key="tarefa.id">
      <!-- Checkbox de status da tarefa -->
      <input
        @change="(evento) => (tarefa.finalizada = evento.target.checked)"
        :checked="tarefa.finalizada"
        :id="'tarefa-' + tarefa.id"
        type="checkbox"
      />

      <!-- Label com estilo condicional para tarefa finalizada -->
      <label :class="{ done: tarefa.finalizada }" class="ms-3" :for="'tarefa-' + tarefa.id">
        {{ tarefa.titulo }}
      </label>
    </li>
  </ul>
</template>

<style scoped>
  /* Estilo aplicado apenas a este componente (por causa do `scoped`) */
  /* Estilo visual que risca o texto da tarefa finalizada */
  .done {
    text-decoration: line-through;
  }
</style>

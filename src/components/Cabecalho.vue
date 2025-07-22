<script setup>
  const props = defineProps(['tarefasPendentes'])
</script>
<template>
      <!-- Cabeçalho da aplicação -->
    <header class="p-5 mb-4 mt-4 bg-light rounded-3">
      <h1>Minhas tarefas</h1>
      <!-- Exibe quantidade de tarefas pendentes em tempo real -->
      <p>
        Você possui {{ props.tarefasPendentes }} tarefas pendentes.
      </p>
    </header>
</template>

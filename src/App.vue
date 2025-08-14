<script setup>
import { computed, ref } from 'vue';
import ColunaKanban from './assets/Components/ColunaKanban.vue';
const statusAfazer = 'a-fazer';
const statusEmAndamento = 'em-andamento';
const statusConcluido = 'concluido';
const tarefas = ref([]);
const novaTarefaRetornada = (textoDoCard, statusTarefa) => {
  const novaTarefa = {
    id: Date.now(),
    titulo: textoDoCard,
    status: statusTarefa,
  };
  tarefas.value.push(novaTarefa);
};

function salvarTarefaEditada(dados) {
  const tarefaParaAtualizar = tarefas.value.find(
    (tarefa) => tarefa.id === dados.id
  );
  tarefaParaAtualizar.titulo = dados.novoTitulo;
}

function apagarTarefaDefinito(idParaApagar) {
  tarefas.value = tarefas.value.filter((tarefa) => tarefa.id !== idParaApagar);
}

const tarefasAFazer = computed(() => {
  return tarefas.value.filter((tarefa) => tarefa.status === statusAfazer);
});

const tarefasConcluidas = computed(() => {
  return tarefas.value.filter((tarefa) => tarefa.status === statusConcluido);
});
const tarefasEmAndamento = computed(() => {
  return tarefas.value.filter((tarefa) => tarefa.status === statusEmAndamento);
});

function moverTarefa(pacote) {
  const tarefaParaMover = tarefas.value.find(
    (tarefa) => tarefa.id === pacote.id
  );
  // Segurança: Se por algum motivo não encontrar a tarefa, a gente para aqui.
  if (!tarefaParaMover) return;

  console.log(`aaa`, tarefaParaMover);

  const statusAtual = tarefaParaMover.status;
  const direcao = pacote.direcao;
  let novoStatus;

  if (direcao === 'direita') {
    if (statusAtual === 'a-fazer') {
      novoStatus = 'em-andamento';
    } else if (statusAtual === 'em-andamento') {
      novoStatus = 'concluido';
    }
  } else if (direcao === 'esquerda') {
    if (statusAtual === 'em-andamento') {
      novoStatus = 'a-fazer';
    } else if (statusAtual === 'concluido') {
      novoStatus = 'em-andamento';
    }
  }

  if (novoStatus) {
    tarefaParaMover.status = novoStatus;
  }
}
</script>

<template>
  <div class="max-w-7xl m-auto p-2">
    <h1 class="titulo text-white font-serif mb-10">Quadro Kanban</h1>
    <div class="quadro-kanban">
      <ColunaKanban
        titulo="A Fazer"
        :tarefas="tarefasAFazer"
        @adicionar-tarefa="
          (textoDoCard) => novaTarefaRetornada(textoDoCard, statusAfazer)
        "
        @apagar-tarefa="apagarTarefaDefinito($event)"
        @salvar-tarefa-editada="salvarTarefaEditada($event)"
        @mover-tarefa="moverTarefa($event)"
      />
      <ColunaKanban
        titulo="Em Andamento"
        :tarefas="tarefasEmAndamento"
        @adicionar-tarefa="
          (textoDoCard) => novaTarefaRetornada(textoDoCard, statusEmAndamento)
        "
        @apagar-tarefa="apagarTarefaDefinito($event)"
        @salvar-tarefa-editada="salvarTarefaEditada($event)"
        @mover-tarefa="moverTarefa($event)"
      />
      <ColunaKanban
        titulo="Concluído"
        :tarefas="tarefasConcluidas"
        @adicionar-tarefa="
          (textoDoCard) => novaTarefaRetornada(textoDoCard, statusConcluido)
        "
        @apagar-tarefa="apagarTarefaDefinito($event)"
        @salvar-tarefa-editada="salvarTarefaEditada($event)"
        @mover-tarefa="moverTarefa($event)"
      />
    </div>
  </div>
</template>

<style>
.titulo {
  font-size: 40px;
}

.quadro-kanban {
  gap: 20px;
  height: 100vh;
  display: grid;
  grid-template-columns: 1fr 1fr 1fr;
  padding: 0px 10px;
}
body {
  background-color: #1e1d23;
}
</style>

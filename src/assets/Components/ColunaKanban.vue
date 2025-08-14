<script setup>
import { ref } from 'vue';
import { X } from 'lucide-vue-next';
import { Plus } from 'lucide-vue-next';
import CardTarefa from './CardTarefa.vue';
import AddCartaoGrey from './AddCartaoGrey.vue';
const adicionarTarefa = ref(false);
const novoCardTexto = ref(''); // Texto do input
const emit = defineEmits([
  'adicionar-tarefa',
  'apagar-tarefa',
  'salvar-tarefa-editada',
  'move-to-right',
]); //Definindo a emissão

const input = ref('');
defineProps({
  titulo: String,
  tarefas: Array,
});

function moveToRightColuna(tarefa) {
  console.log('Dado que chegou em Coluna Kanban', tarefa.id);
}

function enviarNovaTarefa() {
  console.log(novoCardTexto.value);
  console.log(`enviarNovaTarefa`, novoCardTexto.value.length);
  if (novoCardTexto.value.length === 0) {
    console.log('Impossivel adicionar');
  } else {
    emit('adicionar-tarefa', novoCardTexto.value);
    novoCardTexto.value = '';
  }
}
function apagarTarefaColuna(id) {
  console.log(`id de quem vai ser excluido`, id);
  emit('apagar-tarefa', id);
}

function mudarStatusBotao() {
  adicionarTarefa.value = !adicionarTarefa.value;
}

function salvarEditadoColuna(valores) {
  emit('salvar-tarefa-editada', valores);
}
</script>

<template>
  <div>
    <div class="flex justify-between">
      <h1 class="font-sans text-gray-50 mb-2">
        {{ titulo }}
      </h1>
    </div>
    <div class="bg-[#212026] rounded-xl p-5 h-screen shadow">
      <div>
        <ul class="flex flex-col gap-2">
          <CardTarefa
            v-for="tarefa in tarefas"
            :key="tarefa.id"
            :tarefa="tarefa"
            @apagar-tarefa="apagarTarefaColuna($event)"
            @salvar-editado="salvarEditadoColuna($event)"
            @move-to-right="moveToRightColuna($event)"
          />
        </ul>
      </div>
      <div
        v-if="!adicionarTarefa"
        class="cursor-pointer flex p-2.5 gap-1.5 transition-colors duration-300 hover:bg-[#272F27] rounded-xl mt-1"
        @click="mudarStatusBotao"
      >
        <Plus color="#96959E" />
        <AddCartaoGrey />
      </div>
      <input
        v-if="adicionarTarefa"
        class="text-gray-400 bg-[#2B2A30] p-3 rounded-xl shadow w-full mt-2"
        type="text"
        placeholder="Insira uma tarefa."
        name=""
        id=""
        v-model="novoCardTexto"
        @keyup.enter="enviarNovaTarefa"
      />

      <div v-if="adicionarTarefa" class="flex mt-2 items-center gap-2">
        <button
          class="text-blue-950 cursor-pointer p-2.5 bg-blue-600 hover:bg-blue-500 transition rounded-xl"
          @click="enviarNovaTarefa"
        >
          Adicionar uma cartão
        </button>
        <X @click="mudarStatusBotao" color="#6a7282" class="cursor-pointer" />
      </div>
    </div>
  </div>
</template>

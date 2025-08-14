<script setup>
import { ref, nextTick } from 'vue';
const options = ref(false);
const handleOptions = ref(true);
const isEditing = ref(false);
const tituloEditado = ref('');
const props = defineProps({ tarefa: Object });
const inputDeEdicaoRef = ref(null);

import {
  Ellipsis,
  Trash,
  PencilLine,
  X,
  ArrowRight,
  ArrowLeft,
} from 'lucide-vue-next';

const emit = defineEmits(['apagar-tarefa', 'salvar-editado', 'move-to-right']);

function salvarEditado() {
  const dados = {
    id: props.tarefa.id,
    novoTitulo: tituloEditado.value,
  };

  emit('salvar-editado', dados);
  isEditing.value = false;
  handleOptions.value = true;
}

function apagarTarefaCard(tarefa) {
  console.log(tarefa);
  emit('apagar-tarefa', tarefa.id);
}

function moveRight(tarefa) {
  emit('move-to-right', tarefa);
}

function cancelarEdicao() {
  isEditing.value = false;
  handleOptions.value = true;
}

function ativarEdicao() {
  isEditing.value = true;
  tituloEditado.value = props.tarefa.titulo;
  nextTick(() => {
    inputDeEdicaoRef.value.focus();
  });
}

function handleOptionsValue() {
  handleOptions.value = !handleOptions.value;
  options.value = false;
}

function optionsCard() {
  options.value = !options.value;
}
</script>

<template>
  <input
    v-if="isEditing"
    ref="inputDeEdicaoRef"
    class="text-gray-400 bg-[#2B2A30] p-3 rounded-xl shadow w-full"
    type="text"
    placeholder="Insira uma tarefa."
    name=""
    id=""
    v-model="tituloEditado"
    @keyup.enter="salvarEditado"
  />

  <div v-if="isEditing" class="flex items-center gap-2">
    <button
      class="text-blue-950 cursor-pointer p-2.5 bg-blue-600 hover:bg-blue-500 transition rounded-xl"
      @click="salvarEditado"
    >
      Salvar
    </button>
    <X @click="cancelarEdicao" color="#6a7282" class="cursor-pointer" />
  </div>

  <li
    v-else
    class="flex justify-between text-gray-400 bg-[#2B2A30] p-3 rounded-xl shadow"
  >
    {{ tarefa.titulo }}
    <div
      :class="['relative', 'p-0.5', 'rounded-md', { 'bg-[#434249]': options }]"
    >
      <Ellipsis
        v-if="handleOptions"
        @click="optionsCard"
        class="cursor-pointer"
      />
      <ul
        v-if="options"
        class="absolute flex flex-col gap-1 top-[33px] right-[-81px] z-10 w-[110px] bg-[#212026] rounded-xl border border-[#2d2c33]"
      >
        <li
          v-if="tarefa.status !== 'concluido'"
          class="flex items-center cursor-pointer gap-1.5 p-2 hover:bg-[#434249] transition duration-500 rounded-xl"
          @click="moveRight(tarefa)"
        >
          <ArrowRight class="w-5 h-4" />Mover >
        </li>
        <li
          v-if="tarefa.status !== 'a-fazer'"
          class="flex items-center cursor-pointer gap-1.5 p-2 hover:bg-[#434249] transition duration-500 rounded-xl"
        >
          <ArrowLeft class="w-5 h-4" /> Mover
        </li>
        <li
          class="flex items-center cursor-pointer gap-1.5 p-2 hover:bg-[#434249] transition rounded-xl"
          @click="apagarTarefaCard(tarefa)"
        >
          <Trash class="w-5 h-4" />Apagar
        </li>
        <li
          class="flex items-center cursor-pointer gap-1.5 p-2 hover:bg-[#434249] transition duration-500 rounded-xl"
          @click="
            handleOptionsValue();
            ativarEdicao();
          "
        >
          <PencilLine class="w-5 h-4" />Editar
        </li>
      </ul>
    </div>
  </li>
</template>

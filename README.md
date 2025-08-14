

# Quadro Kanban com Vue.js 3

## 📖 Sobre o Projeto

Este projeto é um quadro de gestão de tarefas no estilo Kanban, desenvolvido como uma forma prática de aprofundar e solidificar meus conhecimentos em **Vue.js 3** e na **Composition API**.

O objetivo não era apenas criar uma interface, mas construir uma aplicação robusta, focada na arquitetura de componentes, no fluxo de dados reativo e nas melhores práticas do ecossistema Vue. 
-----

## `Funcionalidades Implementadas (V1)

  * **Gestão de Tarefas (CRUD):**
      * ✅ **Criar:** Adicionar novas tarefas em qualquer coluna.
      * ✅ **Ler:** Listar e visualizar todas as tarefas de forma organizada.
      * ✅ **Atualizar:** Editar o título de uma tarefa existente.
      * ✅ **Apagar:** Remover tarefas do quadro.
  * **Mover Tarefas:** Lógica implementada para mover os cards entre as colunas "A Fazer", "Em Andamento" e "Concluído".
  * **Interface Reativa:** A UI responde instantaneamente a todas as alterações de dados, graças à reatividade do Vue.

-----

## 🛠️ Tecnologias e Conceitos Aplicados

Este projeto foi uma oportunidade para praticar o "coração" do Vue.js. As principais ferramentas e conceitos utilizados foram:

  * **Vue.js 3:** Utilizando a **Composition API** e a sintaxe `<script setup>`.
  * **Vite:** Como ferramenta de build, garantindo um ambiente de desenvolvimento rápido e moderno.
  * **Arquitetura de Componentes:** A aplicação foi modularizada em três níveis (`QuadroKanban` \> `ColunaKanban` \> `CardTarefa`) para promover a reutilização e a organização do código.
  * **Fluxo de Dados (Props & Emits):** Domínio da comunicação entre componentes:
      * **Props:** Para o fluxo de dados unidirecional de "pai para filho".
      * **Emits:** Para a comunicação desacoplada de "filho para pai" (e até "neto para avô"), garantindo que o estado seja gerenciado de forma centralizada e previsível.
  * **Reatividade Avançada:**
      * `ref`: Para o estado reativo local e principal.
      * `computed`: Para criar listas derivadas (as colunas filtradas) de forma performática e declarativa.
  * **Diretivas Essenciais:** Uso prático de `v-for` (com `:key`), `v-if`/`v-else`, `v-model` e `v-bind` para criar uma UI totalmente dinâmica.
  * **Melhoria de UX:** Implementação de auto-foco em inputs com `Template Refs` e `nextTick` para uma experiência de usuário mais fluida.

-----

## 🚀 Como Executar o Projeto Localmente

```bash
# 1. Clone o repositório
git clone [SEU_LINK_DO_REPOSITÓRIO_AQUI]

# 2. Navegue até a pasta do projeto
cd nome-do-projeto

# 3. Instale as dependências
npm install

# 4. Inicie o servidor de desenvolvimento
npm run dev
```

-----

## Próximos Passos e Melhorias Futuras

  * [ ] **Drag and Drop:** Substituir os botões de mover por uma interface de arrastar e soltar, aprimorando a experiência do usuário.
  * [ ] **Persistência de Dados:** Salvar o estado do quadro no `localStorage` do navegador para que as tarefas não se percam ao recarregar a página.
  * [ ] **Validação de Formulários:** Adicionar validações para não permitir a criação de tarefas vazias.

-----

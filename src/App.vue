<template>
  <v-app>
    <v-app-bar app color="primary">
      <v-app-bar-title>
        Pet Shop
      </v-app-bar-title>
      <v-spacer></v-spacer>
      <v-btn @click="toggleCadastro" text>Cadastro</v-btn>
      <v-btn @click="toggleAgendamento" text>Agendamento</v-btn>
      <v-btn @click="toggleRelatorios" text>Relatórios</v-btn>
    </v-app-bar>

    <v-main class="main-background">
      <v-container class="main-container">
        <!-- Conteúdo principal -->
        <v-app-bar v-if="cadastroAberto" color="secondary" dark>
          <v-spacer></v-spacer>
          <v-btn @click="openTutorCadastro" text>Cadastrar Tutor</v-btn>
          <v-btn @click="openPetCadastro" text>Cadastrar Pet</v-btn>
          <v-btn @click="openProfissionalCadastro" text>Cadastrar Profissional</v-btn>
          <v-btn @click="openServicosCadastro" text>Cadastrar Serviço</v-btn>
        </v-app-bar>


        <v-app-bar v-if="relatoriosAberto" color="secondary" dark>
          <v-spacer></v-spacer>
          <v-btn @click="openRelatorioConsumo" text>Consulta</v-btn>
        </v-app-bar>

        <TutorCadastro v-if="cadastroTutorAberto" />
        <PetCadastro v-if="cadastroPetAberto" />
        <ProfissionalCadastro v-if="cadastroProfissionalAberto" />
        <ServicosCadastro v-if="cadastroServicosAberto" />
        <Agendamento v-if="agendamentoAberto" />
        <RelatorioConsumo v-if="relatorioConsumoAberto" />
      </v-container>
    </v-main>

    <AppFooter />
  </v-app>
</template>

<script setup>
import { ref } from 'vue';
import TutorCadastro from './components/TutorCadastro.vue';
import PetCadastro from './components/PetCadastro.vue';
import ProfissionalCadastro from './components/ProfissionalCadastro.vue';
import ServicosCadastro from './components/ServicosCadastro.vue';
import Agendamento from './components/Agendamento.vue';
import RelatorioConsumo from './components/RelatorioConsumo.vue';
import RelatorioServico from './components/RelatorioServico.vue';
import RelatorioFaturamento from './components/RelatorioFaturamento.vue';

const cadastroAberto = ref(false);
const agendamentoAberto = ref(false);
const relatoriosAberto = ref(false);
const cadastroTutorAberto = ref(false);
const cadastroPetAberto = ref(false);
const cadastroProfissionalAberto = ref(false);
const cadastroServicosAberto = ref(false);
const relatorioConsumoAberto = ref(false);
const relatorioServicoAberto = ref(false);
const relatorioFaturamentoAberto = ref(false);

function toggleCadastro() {
  cadastroAberto.value = !cadastroAberto.value;
  agendamentoAberto.value = false;
  relatoriosAberto.value = false;
  closeAllSubMenus();
}

function toggleAgendamento() {
  agendamentoAberto.value = !agendamentoAberto.value;
  cadastroAberto.value = false;
  relatoriosAberto.value = false;
  closeAllSubMenus();
}

function toggleRelatorios() {
  relatoriosAberto.value = !relatoriosAberto.value;
  cadastroAberto.value = false;
  agendamentoAberto.value = false;
  closeAllSubMenus();
}

function closeAllSubMenus() {
  cadastroTutorAberto.value = false;
  cadastroPetAberto.value = false;
  cadastroProfissionalAberto.value = false;
  cadastroServicosAberto.value = false;
  relatorioConsumoAberto.value = false;
  relatorioServicoAberto.value = false;
  relatorioFaturamentoAberto.value = false;
}

function openTutorCadastro() {
  closeAllSubMenus();
  cadastroTutorAberto.value = true;
  cadastroAberto.value = true;
}

function openPetCadastro() {
  closeAllSubMenus();
  cadastroPetAberto.value = true;
  cadastroAberto.value = true;
}

function openProfissionalCadastro() {
  closeAllSubMenus();
  cadastroProfissionalAberto.value = true;
  cadastroAberto.value = true;
}

function openServicosCadastro() {
  closeAllSubMenus();
  cadastroServicosAberto.value = true;
  cadastroAberto.value = true;
}

function openRelatorioConsumo() {
  closeAllSubMenus();
  relatorioConsumoAberto.value = true;
  relatoriosAberto.value = true;
}

function openRelatorioServico() {
  closeAllSubMenus();
  relatorioServicoAberto.value = true;
  relatoriosAberto.value = true;
}

function openRelatorioFaturamento() {
  closeAllSubMenus();
  relatorioFaturamentoAberto.value = true;
  relatoriosAberto.value = true;
}
</script>

<style scoped>
.main-background {
  background: url('@/assets/background.jpg') no-repeat center center fixed;
  background-size: cover;
  min-height: 100vh;
}

.main-container {
  position: relative;
  padding-bottom: 100px; /* Ajuste para garantir que o conteúdo não sobreponha a imagem */
}

.image-container {
  position: absolute;
  bottom: 0;
  right: 0;
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: flex-end;
  align-items: flex-end;
  overflow: hidden;
}

.right-image {
  width: 100%; /* Faz com que a imagem ocupe toda a largura disponível */
  height: auto; /* Mantém a proporção da imagem */
  max-width: 1000px; /* Ajuste conforme necessário para o tamanho da imagem */
}
</style>

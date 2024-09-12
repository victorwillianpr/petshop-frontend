<template>
  <v-container>
    <v-row>
      <!-- Coluna para o Formulário de Agendamento -->
      <v-col cols="12" md="4">
        <v-card>
          <v-card-title>Agendar Serviço</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="agendar">
              <v-text-field
                v-model="data"
                label="Data"
                variant="outlined"
                clearable
                dense
                required
                type="date"
              ></v-text-field>
              <v-text-field
                v-model="hora"
                label="Hora"
                variant="outlined"
                clearable
                dense
                required
                type="time"
              ></v-text-field>

              <!-- Serviço -->
              <v-autocomplete
                v-model="selectedServico"
                :items="servicos"
                item-value="tipo"
                item-text="tipo"
                label="Serviço"
                variant="outlined"
                clearable
                dense
                required
              ></v-autocomplete>
              <!-- Profissional -->
              <v-autocomplete
                v-model="selectedProfissional"
                :items="profissionais"
                item-value="nome"
                item-text="nome"
                label="Profissional"
                variant="outlined"
                clearable
                dense
                required
              ></v-autocomplete>

              <!-- Pet -->
              <v-autocomplete
                v-model="selectedPet"
                :items="pets"
                item-value="nome"
                item-text="nome"
                label="Pet"
                variant="outlined"
                clearable
                dense
                required
              ></v-autocomplete>

              <!-- Tutor -->
              <v-autocomplete
                v-model="selectedTutor"
                :items="tutores"
                item-value="nome"
                item-text="nome"
                label="Tutor"
                variant="outlined"
                clearable
                dense
                required
              ></v-autocomplete>

              <v-btn type="submit" color="primary" block>Agendar</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Coluna para as Tabelas de Agendamentos -->
      <v-col cols="12" md="8">
        <v-row>
          <!-- Tabela de Agendamentos Realizados -->
          <v-col cols="12">
            <v-card>
              <v-card-title>Agendamentos Realizados</v-card-title>
              <v-card-text>
                <v-data-table
                  :headers="headersRealizados"
                  :items="agendamentosRealizados"
                  item-key="id"
                >
                  <template v-slot:item.data="{ item }">
                    {{ formatDate(item.data) }}
                  </template>
                  <template v-slot:item.hora="{ item }">
                    {{ item.hora }}
                  </template>
                  <template v-slot:item.servico="{ item }">
                    {{ item.servico }}
                  </template>
                  <template v-slot:item.profissional="{ item }">
                    {{ item.profissional }}
                  </template>
                  <template v-slot:item.pet="{ item }">
                    {{ item.pet }}
                  </template>
                  <template v-slot:item.tutor="{ item }">
                    {{ item.tutor }}
                  </template>
                </v-data-table>
              </v-card-text>
            </v-card>
          </v-col>

          <!-- Tabela de Agendamentos Futuros -->
          <v-col cols="12" class="mt-4">
            <v-card>
              <v-card-title>Agendamentos Futuros</v-card-title>
              <v-card-text>
                <v-data-table
                  :headers="headersFuturos"
                  :items="agendamentosFuturos"
                  item-key="id"
                >
                  <template v-slot:item.data="{ item }">
                    {{ formatDate(item.data) }}
                  </template>
                  <template v-slot:item.hora="{ item }">
                    {{ item.hora }}
                  </template>
                  <template v-slot:item.servico="{ item }">
                    {{ item.servico }}
                  </template>
                  <template v-slot:item.profissional="{ item }">
                    {{ item.profissional }}
                  </template>
                  <template v-slot:item.pet="{ item }">
                    {{ item.pet }}
                  </template>
                  <template v-slot:item.tutor="{ item }">
                    {{ item.tutor }}
                  </template>
                </v-data-table>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      data: '',
      hora: '',
      selectedServico: null,
      selectedProfissional: null,
      selectedPet: null,
      selectedTutor: null,
      servicos: [],
      profissionais: [],
      pets: [],
      tutores: [],
      agendamentosRealizados: [],
      agendamentosFuturos: [],
      headersRealizados: [
        { text: 'Data', value: 'data' },
        { text: 'Hora', value: 'hora' },
        { text: 'Serviço', value: 'servico' },
        { text: 'Profissional', value: 'profissional' },
        { text: 'Pet', value: 'pet' },
        { text: 'Tutor', value: 'tutor' },
      ],
      headersFuturos: [
        { text: 'Data', value: 'data' },
        { text: 'Hora', value: 'hora' },
        { text: 'Serviço', value: 'servico' },
        { text: 'Profissional', value: 'profissional' },
        { text: 'Pet', value: 'pet' },
        { text: 'Tutor', value: 'tutor' },
      ],
    };
  },
  methods: {
    async fetchDados() {
      try {
        const [servicosRes, profissionaisRes, petsRes, tutoresRes, realizadosRes, futurosRes] = await Promise.all([
          axios.get('http://127.0.0.1:5000/api/servicos'),
          axios.get('http://127.0.0.1:5000/api/profissionais'),
          axios.get('http://127.0.0.1:5000/api/pets'),
          axios.get('http://127.0.0.1:5000/api/tutores'),
          axios.get('http://127.0.0.1:5000/api/agendamentos/realizados'),
          axios.get('http://127.0.0.1:5000/api/agendamentos/futuros'),
        ]);

        this.servicos = servicosRes.data.map(servico => servico.tipo);
        this.profissionais = profissionaisRes.data.map(profissional => profissional.nome);
        this.pets = petsRes.data.map(pet => pet.nome);
        this.tutores = tutoresRes.data.map(tutor => tutor.nome);
        this.agendamentosRealizados = realizadosRes.data;
        this.agendamentosFuturos = futurosRes.data;
      } catch (error) {
        console.error('Erro ao buscar dados:', error);
      }
    },
    async agendar() {
      const agendamento = {
        data: this.data,
        hora: this.hora,
        servico: this.selectedServico,
        profissional: this.selectedProfissional,
        pet: this.selectedPet,
        tutor: this.selectedTutor,
      };

      try {
        await axios.post('http://127.0.0.1:5000/api/agendamentos', agendamento);
        alert('Agendamento realizado com sucesso!');
        this.resetForm();
        this.fetchDados(); // Atualiza as tabelas após o agendamento
      } catch (error) {
        console.error('Erro ao agendar:', error);
      }
    },
    resetForm() {
      this.data = '';
      this.hora = '';
      this.selectedServico = null;
      this.selectedProfissional = null;
      this.selectedPet = null;
      this.selectedTutor = null;
    },
    formatDate(dateString) {
      const date = new Date(dateString);
      return date.toLocaleDateString();
    },
  },
  created() {
    this.fetchDados();
  },
};
</script>

<style scoped>
/* Estilo adicional para melhorar o layout */
.v-col {
  padding: 16px;
}
</style>

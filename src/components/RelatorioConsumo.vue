<template>
  <v-container>
    <v-row>
      <!-- Coluna para o Formulário de Consulta -->
      <v-col cols="12" md="4">
        <v-card>
          <v-card-title>Adicionar Consulta</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="adicionarConsulta">
              <!-- Tipo de Agendamento Futuro -->
              <v-select
                v-model="selectedPetNome"
                :items="agendamentosFuturos.map(agendamento => agendamento.pet)"
                item-text="petNome"
                item-value="petNome"
                label="Agendamentos Futuros"
                variant="outlined"
                clearable
                dense
                required
              ></v-select>
              <v-text-field
                v-model="diagnostico"
                label="Diagnóstico"
                variant="outlined"
                clearable
                dense
                required
              ></v-text-field>
              <v-text-field
                v-model="sintomas"
                label="Sintomas"
                variant="outlined"
                clearable
                dense
                required
              ></v-text-field>
              <v-text-field
                v-model="tratamento"
                label="Tratamento"
                variant="outlined"
                clearable
                dense
                required
              ></v-text-field>
              <v-text-field
                v-model="dataRetorno"
                label="Data de Retorno"
                variant="outlined"
                clearable
                dense
                required
                type="date"
              ></v-text-field>
              <v-btn type="submit" color="primary" block>Adicionar Consulta</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Coluna para as Tabelas de Consultas -->
      <v-col cols="8">
        <v-card>
          <v-card-title>Consultas</v-card-title>
          <v-card-text>
            <v-data-table
              :headers="headersConsultas"
              :items="consultas"
              item-key="id"
            >
              <template v-slot:item.data="{ item }">
                {{ formatDate(item.data) }}
              </template>
              <template v-slot:item.retorno="{ item }">
                {{ formatDate(item.retorno) }}
              </template>
              <template v-slot:item.sintomas="{ item }">
                {{ item.sintomas }}
              </template>
              <template v-slot:item.diagnostico="{ item }">
                {{ item.diagnostico }}
              </template>
              <template v-slot:item.tratamento="{ item }">
                {{ item.tratamento }}
              </template>
              <template v-slot:item.petNome="{ item }">
                {{ item.petNome }}
              </template>
            </v-data-table>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      selectedPetNome: '',  // Armazena o nome do pet selecionado
      diagnostico: '',
      sintomas: '',
      tratamento: '',
      dataRetorno: '',
      agendamentosFuturos: [],
      consultas: [],
      headersConsultas: [
        { text: 'Data', value: 'data' },
        { text: 'Data de Retorno', value: 'retorno' },
        { text: 'Sintomas', value: 'sintomas' },
        { text: 'Diagnóstico', value: 'diagnostico' },
        { text: 'Tratamento', value: 'tratamento' },
        { text: 'Nome do Pet', value: 'petNome' },  // Adicionado ao cabeçalho da tabela
      ],
    };
  },
  methods: {
    async fetchDados() {
      try {
        const [futurosRes, consultasRes] = await Promise.all([
          axios.get('http://127.0.0.1:5000/api/agendamentos/futuros'),
          axios.get('http://127.0.0.1:5000/api/consultas'),
        ]);

        this.agendamentosFuturos = futurosRes.data;
        this.consultas = consultasRes.data;
      } catch (error) {
        console.error('Erro ao buscar dados:', error);
      }
    },
    async adicionarConsulta() {
      const consulta = {
        data: new Date().toISOString(), // Data atual ou de outro campo se disponível
        retorno: this.dataRetorno,
        sintomas: this.sintomas,
        diagnostico: this.diagnostico,
        tratamento: this.tratamento,
        petNome: this.selectedPetNome // Nome do pet diretamente do v-model
      };

      try {
        await axios.post('http://127.0.0.1:5000/api/consultas', consulta);
        alert('Consulta adicionada com sucesso!');
        this.resetFormConsulta();
        this.fetchDados(); // Atualiza as tabelas após adicionar a consulta
      } catch (error) {
        console.error('Erro ao adicionar consulta:', error);
      }
    },
    resetFormConsulta() {
      this.selectedPetNome = ''; // Ajustado para limpar o nome do pet
      this.diagnostico = '';
      this.sintomas = '';
      this.tratamento = '';
      this.dataRetorno = '';
    },
    formatDate(date) {
      const options = { year: 'numeric', month: '2-digit', day: '2-digit' };
      return new Date(date).toLocaleDateString('pt-BR', options);
    }
  },
  mounted() {
    this.fetchDados();
  }
};
</script>

<style>
/* Seu estilo aqui */
</style>

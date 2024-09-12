<template>
  <v-container>
    <v-row>
      <!-- Coluna para o Formulário -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Cadastro de Tutor</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="cadastrarTutor">
              <v-text-field v-model="nome" label="Nome" variant="outlined" clearable dense required></v-text-field>
              <v-text-field v-model="cpf" label="CPF" variant="outlined" clearable dense required></v-text-field>
              <v-text-field v-model="email" label="Email" variant="outlined" clearable dense required></v-text-field>
              <v-text-field v-model="telefone" label="Telefone" variant="outlined" clearable dense></v-text-field>
              <v-btn color="primary" type="submit">Cadastrar</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Coluna para a Tabela -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Tabela de Tutores</v-card-title>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="tutores"
              class="elevation-1"
            >
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
      nome: '',
      cpf: '',
      email: '',
      telefone: '',
      headers: [
        { text: 'Nome', value: 'nome' },
        { text: 'CPF', value: 'cpf' },
        { text: 'Email', value: 'email' },
        { text: 'Telefone', value: 'telefone' },
      ],
      tutores: [],
    };
  },
  methods: {
    async cadastrarTutor() {
      if (!this.nome || !this.cpf || !this.email) {
        alert('Nome, CPF e Email são obrigatórios!');
        return;
      }

      const novoTutor = {
        nome: this.nome,
        cpf: this.cpf,
        email: this.email,
        telefone: this.telefone,
      };

      try {
        const response = await axios.post('http://127.0.0.1:5000/api/tutores', novoTutor);
        console.log('Tutor cadastrado com sucesso', response.data);
        // Atualize a lista de tutores
        this.fetchTutores();
        // Limpe os campos do formulário
        this.nome = '';
        this.cpf = '';
        this.email = '';
        this.telefone = '';
      } catch (error) {
        console.error('Erro ao cadastrar tutor', error);
      }
    },
    async fetchTutores() {
      try {
        const response = await axios.get('http://127.0.0.1:5000/api/tutores');
        this.tutores = response.data;
      } catch (error) {
        console.error('Erro ao buscar tutores', error);
      }
    }
  },
  created() {
    // Busca os tutores ao carregar o componente
    this.fetchTutores();
  },
};
</script>

<style scoped>
/* Ajuste a largura do formulário e tabela conforme necessário */
</style>

<template>
  <v-container>
    <v-row>
      <!-- Coluna para o Formulário -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Cadastro de Profissional</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="cadastrarProfissional">
              <v-text-field v-model="nome" label="Nome" variant="outlined" clearable dense required></v-text-field>
              <v-text-field v-model="email" label="Email" variant="outlined" clearable dense required></v-text-field>
              <v-text-field v-model="cpf" label="CPF" variant="outlined" clearable dense required></v-text-field>
              <v-btn color="primary" type="submit">Cadastrar</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Coluna para a Tabela -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Tabela de Profissionais</v-card-title>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="profissionais"
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
import axios from 'axios'

export default {
  data() {
    return {
      nome: '',
      email: '',
      cpf: '',
      headers: [
        { text: 'Nome', value: 'nome' },
        { text: 'Email', value: 'email' },
        { text: 'CPF', value: 'cpf' },
      ],
      profissionais: [],
    };
  },
  methods: {
    async cadastrarProfissional() {
      if (!this.nome || !this.email || !this.cpf) {
        alert('Todos os campos são obrigatórios!');
        return;
      }

      const profissional = {
        nome: this.nome,
        email: this.email,
        cpf: this.cpf
      };

      try {
        const response = await axios.post('http://127.0.0.1:5000/api/profissionais', profissional);
        console.log('Profissional cadastrado com sucesso', response.data);
        this.profissionais.push({ ...profissional });
        this.nome = '';
        this.email = '';
        this.cpf = '';
      } catch (error) {
        console.error('Erro ao cadastrar profissional', error);
      }
    },
    async fetchProfissionais() {
      try {
        const response = await axios.get('http://127.0.0.1:5000/api/profissionais');
        this.profissionais = response.data;
      } catch (error) {
        console.error('Erro ao buscar profissionais', error);
      }
    }
  },
  created() {
    this.fetchProfissionais();
  }
};
</script>

<style scoped>
/* Ajuste a largura do formulário e tabela conforme necessário */
</style>

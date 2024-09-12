<template>
  <v-container>
    <v-row>
      <!-- Coluna para o Formulário -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Cadastro de Serviço</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="cadastrarServico">
              <v-text-field v-model="tipo" label="Tipo" variant="outlined" clearable dense required></v-text-field>
              <v-text-field v-model="preco" label="Preço" variant="outlined" clearable dense required></v-text-field>
              <v-btn color="primary" type="submit">Cadastrar</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Coluna para a Tabela -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Tabela de Serviços</v-card-title>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="servicos"
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
      tipo: '',
      preco: '',
      headers: [
        { text: 'Tipo', value: 'tipo' },
        { text: 'Preço', value: 'preco' },
      ],
      servicos: [],
    };
  },
  methods: {
    async cadastrarServico() {
      if (!this.tipo || !this.preco) {
        alert('Todos os campos são obrigatórios!');
        return;
      }

      const servico = {
        tipo: this.tipo,
        preco: this.preco
      };

      try {
        const response = await axios.post('http://127.0.0.1:5000/api/servicos', servico);
        console.log('Serviço cadastrado com sucesso', response.data);
        this.servicos.push({ ...servico });
        this.tipo = '';
        this.preco = '';
      } catch (error) {
        console.error('Erro ao cadastrar serviço', error);
      }
    },
    async fetchServicos() {
      try {
        const response = await axios.get('http://127.0.0.1:5000/api/servicos');
        this.servicos = response.data;
      } catch (error) {
        console.error('Erro ao buscar serviços', error);
      }
    }
  },
  created() {
    this.fetchServicos();
  }
};
</script>

<style scoped>
/* Ajuste a largura do formulário e tabela conforme necessário */
</style>

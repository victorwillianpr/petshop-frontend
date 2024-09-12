<template>
  <v-container>
    <v-row>
      <!-- Coluna para o Formulário -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Cadastro de Pet</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="cadastrarPet">
              <v-text-field
                v-model="nome"
                label="Nome"
                variant="outlined"
                clearable
                dense
                required
              ></v-text-field>
              <v-text-field
                v-model="raca"
                label="Raça"
                variant="outlined"
                clearable
                dense
                required
              ></v-text-field>
              <v-autocomplete
                v-model="selectedTutor"
                :items="tutores"
                item-text="nome"
                item-value="nome"
                label="Tutor"
                variant="outlined"
                clearable
                dense
                required
              ></v-autocomplete>
              <v-btn color="primary" type="submit">Cadastrar</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Coluna para a Tabela -->
      <v-col cols="12" md="6">
        <v-card>
          <v-card-title>Tabela de Pets</v-card-title>
          <v-card-text>
            <v-data-table
              :headers="headers"
              :items="pets"
              class="elevation-1"
            >
              <template v-slot:item.tutor="{ item }">
                <span>{{ item.tutor }}</span>
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
      nome: '',
      raca: '',
      selectedTutor: '',
      tutores: [],
      headers: [
        { text: 'Nome', value: 'nome' },
        { text: 'Raça', value: 'raca' },
        { text: 'Tutor', value: 'tutor' },
      ],
      pets: [],
    };
  },
  methods: {
    async cadastrarPet() {
      if (!this.nome || !this.raca || !this.selectedTutor) {
        alert('Todos os campos são obrigatórios!');
        return;
      }

      const pet = {
        nome: this.nome,
        raca: this.raca,
        tutor: this.selectedTutor
      };

      try {
        const response = await axios.post('http://127.0.0.1:5000/api/pets', pet);
        console.log('Pet cadastrado com sucesso', response.data);
        this.fetchPets();
        this.nome = '';
        this.raca = '';
        this.selectedTutor = '';
      } catch (error) {
        console.error('Erro ao cadastrar pet', error);
      }
    },
    async fetchTutores() {
      try {
        const response = await axios.get('http://127.0.0.1:5000/api/tutores');
        this.tutores = response.data.map(tutor => tutor.nome);
      } catch (error) {
        console.error('Erro ao buscar tutores', error);
      }
    },
    async fetchPets() {
      try {
        const response = await axios.get('http://127.0.0.1:5000/api/pets');
        console.log('Pets recebidos:', response.data);
        this.pets = response.data;
      } catch (error) {
        console.error('Erro ao buscar pets', error);
      }
    }
  },
  created() {
    this.fetchTutores();
    this.fetchPets();
  }
};
</script>

<style scoped>
/* Ajuste a largura do formulário e tabela conforme necessário */
</style>

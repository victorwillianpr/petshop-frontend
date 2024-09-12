<template>
  <v-container>
    <v-card>
      <v-card-title>Serviço Mais Consumido</v-card-title>
      <v-card-text>
        <v-data-table
          :headers="headers"
          :items="items"
          class="elevation-1"
        ></v-data-table>
      </v-card-text>
    </v-card>
  </v-container>
</template>

<script>
export default {
  name: 'RelatorioServico',
  data() {
    return {
      headers: [
        { text: 'Serviço', value: 'servico' },
        { text: 'Quantidade', value: 'quantidade' }
      ],
      items: [
        { servico: 'Banho', quantidade: 30 },
        { servico: 'Tosa', quantidade: 25 },
        { servico: 'Consulta', quantidade: 20 }
      ]
    };
  }
};
</script>

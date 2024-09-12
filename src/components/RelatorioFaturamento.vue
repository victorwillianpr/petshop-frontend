<template>
  <v-container>
    <v-card>
      <v-card-title>Faturamento</v-card-title>
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
  name: 'RelatorioFaturamento',
  data() {
    return {
      headers: [
        { text: 'Mês', value: 'mes' },
        { text: 'Faturamento', value: 'faturamento' }
      ],
      items: [
        { mes: 'Janeiro', faturamento: 'R$ 5000' },
        { mes: 'Fevereiro', faturamento: 'R$ 7000' },
        { mes: 'Março', faturamento: 'R$ 6000' }
      ]
    };
  }
};
</script>

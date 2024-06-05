<template>
    <div>
      <v-card title="Gêneros" flat>
        <template v-slot:text>
        </template>
        
        <v-data-table
          :headers="headers"
          :items="generos"
          class="elevation-1"
        >
          <template v-slot:item.actions="{ item }">
            <v-btn icon @click="editarGenero(item)">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-card>
      <div>
        <v-btn variant="tonal" href="/genero/criarGenero">
          Adicionar
        </v-btn>
      </div>
    </div>
  </template>
  
  <script>
  import axios from 'axios';
  
  export default {
    data() {
      return {
        search: '',
        headers: [
          {
            text: 'Nome',
            align: 'start',
            value: 'nome',
            sortable: false,
          },
          {
            text: 'Ações',
            value: 'actions',
            sortable: false,
          },
        ],
        generos: [],
      };
    },
    created() {
      this.fetchGeneros();
    },
    methods: {
      async fetchGeneros() {
        try {
          const response = await axios.get('https://localhost:7262/api/genero');
          this.generos = response.data;
        } catch (error) {
          console.error('Erro ao buscar gêneros:', error);
        }
      },
      editarGenero(genero) {
        this.$router.push(`/genero/atualizarGenero/${genero.id}`);
      },
    },
  };
  </script>
  
  <style>
  </style>
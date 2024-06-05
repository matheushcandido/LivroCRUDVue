<template>
    <div>
      <v-card class="mx-auto" max-width="320" title="Editar Gênero">
        <v-card-text>
          <v-text-field v-model="genero.nome" label="Nome"></v-text-field>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="salvarGenero">Salvar</v-btn>
          <v-btn color="secondary" @click="cancelarEdicao">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        genero: {
          id: '',
          nome: '',
        },
      };
    },
    methods: {
      async buscarGenero(id) {
        try {
          const { default: axios } = await import('axios');
          const response = await axios.get(`https://localhost:7262/api/genero/${id}`);
          this.genero = response.data;
        } catch (error) {
          console.error('Erro ao buscar gênero:', error);
        }
      },
      async salvarGenero() {
        try {
          const { default: axios } = await import('axios');
          await axios.put(`https://localhost:7262/api/genero/${this.genero.id}`, this.genero);
          this.$router.push(`/genero/generos`);
        } catch (error) {
          console.error('Erro ao salvar gênero:', error);
        }
      },
      cancelarEdicao() {
        this.$router.push(`/genero/generos`);
      },
    },
    created() {
      const generoId = this.$route.params.id;
      this.buscarGenero(generoId);
    },
  };
  </script>
  
  <style>
  </style>
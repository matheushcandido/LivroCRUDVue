<template>
    <div>
      <v-card class="mx-auto" max-width="320" title="Editar Livro">
        <v-card-text>
          <v-text-field v-model="livro.nome" label="Nome"></v-text-field>
          <v-text-field v-model="livro.autor" label="Autor"></v-text-field>
          <v-text-field v-model="livro.anoPublicacao" label="Ano de Publicação"></v-text-field>
          <v-select
            v-model="livro.generoId"
            :items="generos"
            item-value="id"
            item-text="nome"
            label="Gênero"
          ></v-select>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" @click="salvarLivro">Salvar</v-btn>
          <v-btn color="secondary" @click="cancelarEdicao">Cancelar</v-btn>
        </v-card-actions>
      </v-card>
    </div>
  </template> 
  
  <script>
export default {
  data() {
    return {
      livro: {
        id: '',
        nome: '',
        autor: '',
        anoPublicacao: '',
        generoId: '',
      },
      generos: [],
    };
  },
  methods: {
    async buscarLivro(id) {
      try {
        const { default: axios } = await import('axios');
        const response = await axios.get(`https://localhost:7262/api/livro/${id}`);
        this.livro = response.data;
      } catch (error) {
        console.error('Erro ao buscar livro:', error);
      }
    },
    async buscarGeneros() {
      try {
        const { default: axios } = await import('axios');
        const response = await axios.get('https://localhost:7262/api/genero');
        this.generos = response.data;
      } catch (error) {
        console.error('Erro ao buscar gêneros:', error);
      }
    },
    async salvarLivro() {
      try {
        const { default: axios } = await import('axios');
        await axios.put(`https://localhost:7262/api/livro/${this.livro.id}`, this.livro);
        this.$router.push(`/livro/livros`);
      } catch (error) {
        console.error('Erro ao salvar livro:', error);
      }
    },
    cancelarEdicao() {
      this.$router.push(`/livro/livros`);
    },
  },
  created() {
    const livroId = this.$route.params.id;
    this.buscarLivro(livroId);
    this.buscarGeneros();
  },
};
  </script>
  
  <style>
  </style>
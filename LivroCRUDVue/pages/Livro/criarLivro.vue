<template>
  <div>
    <div>
      <v-sheet class="mx-auto" width="300">
        <v-form @submit.prevent="salvarLivro">
          <v-text-field
            v-model="livro.titulo"
            :rules="rules"
            label="Título"
          ></v-text-field>
          <v-text-field
            v-model="livro.autor"
            label="Autor"
          ></v-text-field>
          <v-text-field
            v-model="livro.anoPublicacao"
            label="Ano de Publicação"
            type="date"
          ></v-text-field>
          <v-select
            v-model="livro.generoId"
            :items="generos"
            label="Gênero"
            item-text="nome"
            item-value="id"
          ></v-select>
          <v-btn class="mt-2" type="submit" block>Salvar</v-btn>
        </v-form>
      </v-sheet>
    </div>
  </div>
</template>

<script>
import Livro from '@/models/Livro';
import https from 'https';

export default {
  data() {
    return {
      livro: new Livro('', '', '', null),
      generos: [],
      rules: [
        value => !!value || 'Valor não pode ser nulo.',
      ],
    };
  },
  created() {
    this.carregarGeneros();
  },
  methods: {
    async carregarGeneros() {
      try {
        const { default: axios } = await import('axios');
        const response = await axios.get('https://localhost:7262/api/genero', {
          httpsAgent: new https.Agent({
            rejectUnauthorized: false
          })
        });
        this.generos = response.data;
      } catch (error) {
        console.error('Erro ao buscar gêneros:', error);
      }
    },
    async salvarLivro() {
      if (this.validarFormulario()) {
        try {
          const { default: axios } = await import('axios');
          await axios.post('https://localhost:7262/api/livro', this.livro);
          this.$router.push(`/livro/livros`);
        } catch (error) {
          console.error('Erro ao criar livro:', error);
        }
      }
    },
    validarFormulario() {
      if (!this.livro.titulo) {
        console.error('Nome do livro não pode estar vazio.');
        return false;
      }
      if (!this.livro.autor) {
        console.error('Nome do autor não pode estar vazio.');
        return false;
      }
      if (!this.livro.anoPublicacao) {
        console.error('Data de publicação não pode estar vazia.');
        return false;
      }
      if (!this.livro.generoId) {
        console.error('Gênero do livro não pode estar vazio.');
        return false;
      }
      return true;
    },
  },
};
</script>

<style scoped>
</style>
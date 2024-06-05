<template>
    <div>
      <v-card title="Livros" flat>
        <template v-slot:text>
        </template>
        
        <v-data-table
          :headers="headers"
          :items="livros"
          class="elevation-1"
        >
          <template v-slot:item.actions="{ item }">
            <v-btn icon @click="editarLivro(item)">
              <v-icon>mdi-pencil</v-icon>
            </v-btn>
          </template>
        </v-data-table>
      </v-card>
      <div>
        <v-btn variant="tonal" href="/livro/criarLivro">
          Adicionar
        </v-btn>
      </div>
    </div>
  </template>
  
  <script>
  
  export default {
    data() {
      return {
        search: '',
        headers: [
          {
            text: 'Título',
            align: 'start',
            value: 'titulo',
            sortable: false,
          },
          {
            text: 'Autor',
            align: 'start',
            value: 'autor',
            sortable: false,
          },
          {
            text: 'Ano da publicação',
            align: 'start',
            value: 'anoPublicacao',
            sortable: false,
          },
          {
            text: 'Gênero',
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
        livros: [],
      };
    },
    created() {
      this.fetchLivros();
    },
    methods: {
      async fetchGeneros() {
        try {
        const { default: axios } = await import('axios');
          const response = await axios.get('https://localhost:7262/api/genero');
          return response.data;
        } catch (error) {
          console.error('Erro ao buscar gêneros:', error);
          return [];
        }
      },
      async fetchLivros() {
        try {
            const { default: axios } = await import('axios');
          const [livrosResponse, generos] = await Promise.all([
            axios.get('https://localhost:7262/api/livro'),
            this.fetchGeneros()
          ]);
  
          const generoMap = {};
          generos.forEach(genero => {
            generoMap[genero.id] = genero.nome;
          });
  
          this.livros = livrosResponse.data.map(livro => {
            return {
              ...livro,
              nome: generoMap[livro.generoId] || 'Desconhecido'
            };
          });
        } catch (error) {
          console.error('Erro ao buscar livros:', error);
        }
      },
      editarLivro(livro) {
        this.$router.push(`/livro/atualizarLivro/${livro.id}`);
      },
    },
  };
  </script>
  
  <style>
  </style>
  
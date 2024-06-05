<template>
  <div>
    <div>
      minha pagina de generos
    </div>
    <div>
      <v-sheet class="mx-auto" width="300">
        <v-form @submit.prevent="salvarGenero">
          <v-text-field
            v-model="nome"
            :rules="rules"
            label="Nome"
          ></v-text-field>
          <v-btn class="mt-2" type="submit" block>Salvar</v-btn>
        </v-form>
      </v-sheet>
    </div>
  </div>
</template>

<script>

export default {
  data() {
    return {
      nome: '',
      rules: [
        value => !!value || 'Você precisa preencher o nome.',
      ],
    };
  },
  methods: {
  async salvarGenero() {
    if (this.nome) {
      try {
        const { default: axios } = await import('axios');
        const novoGenero = { nome: this.nome };
        const response = await axios.post('https://localhost:7262/api/genero', novoGenero);
        this.$router.push(`/genero/generos`);
      } catch (error) {
        console.error('Erro ao criar gênero:', error);
      }
    } else {
      console.error('Nome não pode ser vazio.');
    }
  },
},

};

</script>

<style scoped>
</style>
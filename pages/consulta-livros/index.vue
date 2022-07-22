<template>
  <v-container>
    <v-container style="text-align: center">
      <h1>Consulte O Livro Pelo Emprestimo</h1>
      <hr>
    </v-container>
    <v-container>
      <v-row>
          <v-col>
            <v-autocomplete
              v-model="idLivro"
              :items="livros"
              outlined
              label="Livros"
              item-text="titulo"
              item-value="id"
            ></v-autocomplete>
          </v-col>
        </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      idLivro: null,
      livros: []
    }
  },

  created () {
    this.getLivros();
  },
 
  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get(`http://localhost:3333/livros`);
    }
  }

}
</script>


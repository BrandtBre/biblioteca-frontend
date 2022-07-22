<template>
  <v-container>
    <h1 style="">Cadastro de Livros</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"           
          >
            <v-text-field
              v-model="livro.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.titulo"
              placeholder="Titulo"
              label="Titulo"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="livro.sinopse"
              placeholder="Sinopse"
              label="Sinopse"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete
              v-model="livro.idAutor"
              :items="autores"
              outlined
              label="Autor"
              item-text="nome"
              item-value="id"
              :rules="rule"
              required
            ></v-autocomplete>
          </v-col>
        </v-row>
        <v-row>
          <v-col>
             <v-autocomplete
              v-model="livro.idCategoria"
              :items="categorias"
              outlined
              label="Categoria"
              item-text="nome"
              item-value="id"
              :rules="rule"
              required
            ></v-autocomplete>
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/livros"
      color="red"
    >
      Cancelar
    </v-btn>
    <v-btn
      outlined
      @click="cadastrar"
      color="green"
    >
      Cadastrar
    </v-btn>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      valid: false,
      livro: {
        id: null,
        titulo: null,
        sinopse: null,
        idCategoria: null,
        idAutor: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ],
      categorias: [],
      autores: []
    }
  },

  created () {
    this.getAutores();
    this.getCategorias();
    if (this.$route?.params?.id) {
      this.getById(this.$route.params.id)
    }
  },

  methods: {
    async cadastrar () {
      try {
      if (!this.valid) {
        return this.$toast.warning('O formulário de cadastro não é válido!')
      }
      let livro = {
        titulo: this.livro.titulo,
        sinopse: this.livro.sinopse,
        idCategoria: this.livro.idCategoria,
        idAutor: this.livro.idAutor
      };

      if (!this.livro.id) {
        await this.$axios.$post('http://localhost:3333/livros', livro);
        this.$toast.success('Cadastro realizado com sucesso!');
        return this.$router.push('/livros');
      }

      await this.$axios.$post(`http://localhost:3333/livros/${this.livro.id}`, livro);
        this.$toast.success('Cadastro atualizado com sucesso!');
        return this.$router.push('/livros');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

    async getById (id) {
      this.livro = await this.$axios.$get(`http://localhost:3333/livros/${id}`);
    },    
    async getAutores () {
      this.autores = await this.$axios.$get('http://localhost:3333/autores');
    },
    async getCategorias () {
      this.categorias = await this.$axios.$get('http://localhost:3333/categorias');
    }
  }
}
</script>

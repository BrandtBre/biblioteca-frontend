<template>
  <v-container>
    <h1 style="">Cadastro de Categorias</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"           
          >
            <v-text-field
              v-model="categoria.id"
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
              v-model="categoria.nome"
              placeholder="Nome"
              label="Nome"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/categorias"
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
  name: 'CadastroCategoriasPage',
  data () {
    return {
      valid: false,
      categoria: {
        id: null,
        nome: null
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
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
      let categoria = {
        nome: this.categoria.nome
      };
      
      if (!this.categoria.id) {
        await this.$axios.$post('http://localhost:3333/categorias', categoria);
        this.$toast.success('Cadastro realizado com sucesso!');
        return this.$router.push('/categorias');
      }
      
      
      await this.$axios.$post(`http://localhost:3333/categorias/${this.categoria.id}`, categoria);
      this.$toast.success('Cadastro atualizado com sucesso!');
      this.$router.push('/categorias');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    },

    async getById (id) {
      this.categoria = await this.$axios.$get(`http://localhost:3333/categorias/${id}`);
    }
  }
}
</script>
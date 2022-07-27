<template>
  <v-container>
    <h1 style="">Cadastro de Emprestimos</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"           
          >
            <v-text-field
              v-model="emprestimo.id"
              placeholder="Código"
              label="Código"
              disabled
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-date-picker
              v-model="emprestimo.prazo"
              required
              outlined
              :disabled="desabilitar"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="emprestimo.devolucao"
              placeholder="Devolução"
              label="Devolução"
              required
              outlined
              disabled
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete
              v-model="emprestimo.idUsuario"
              :items="usuarios"
              outlined
              :disabled="desabilitar"
              label="Usuarios"
              item-text="nome"
              item-value="id"
              color="blueviolet"
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-autocomplete
              multiple
              v-model="emprestimo.livros"
              :items="livros"
              outlined
              :disabled="desabilitar"
              label="Livros"
              item-text="titulo"
              item-value="id"
              :rules="rule"
              color="blueviolet"
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/emprestimos"
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
  name: 'CadastroEmprestimoPage',
  data () {
    return {
      desabilitar: false,
      valid: false,
      idLivro: false,
      idUsuarios: false,
      livros: [],
      usuarios: [],
      emprestimo: {
        id: null,
        prazo: null,
        devolucao: null,
        idUsuario: null,
        livros: []
      },
      rule: [
        v => !!v || 'Esse campo é obrigatório'
      ]
    }
  },

  created () {
    this.getUsuarios();
    if (this.$route?.params?.id) {
      this.getLivros();
      this.getById(this.$route.params.id)
      this.desabilitar = true;
      return;
    };
    this.getLivrosDisponiveis();
  },

  methods: {
    async cadastrar () {
      try {
        if (!this.valid) {
          return this.$toast.warning('O formulário de cadastro não é válido!')
        }
        let emprestimo = {
          prazo: this.emprestimo.prazo,
          devolucao: this.emprestimo.devolucao,
          idUsuario: this.emprestimo.idUsuario,
          livros: this.emprestimo.livros
        };

        if (!this.emprestimo.id) {
          await this.$axios.$post('http://localhost:3333/emprestimos', emprestimo);
          this.$toast.success('Emprestimo realizado com sucesso!');
          return this.$router.push('/emprestimos');
        };

        await this.$axios.$post(`http://localhost:3333/emprestimos/${this.emprestimo.id}`, emprestimo);
        this.$toast.success('Emprestimo atualizado com sucesso!');
        return this.$router.push('/emprestimos');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o emprestimo!');
      }
    },

    async getLivros () {
      this.livros = await this.$axios.$get(`http://localhost:3333/livros`);
    },

    async getLivrosDisponiveis () {
      this.livros = await this.$axios.$get(`http://localhost:3333/livros-disponiveis`);
    },

    async getUsuarios () {
      this.usuarios = await this.$axios.$get(`http://localhost:3333/usuarios`);
    },

    async getById (id) {
      this.emprestimo = await this.$axios.$get(`http://localhost:3333/emprestimos/${id}`);
    }
  }
}
</script>
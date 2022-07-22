<template>
  <v-container>
    <h1 style="">Cadastro de Usuarios</h1>
    <hr>
    <v-form v-model="valid">
      <v-container>
        <v-row>
          <v-col
            cols="2"           
          >
            <v-text-field
              v-model="usuario.id"
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
              v-model="usuario.nome"
              placeholder="Nome"
              label="Nome"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.cpfcnpj"
              placeholder="CPF ou CNPJ"
              label="CPF ou CNPJ"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.email"
              placeholder="Email"
              label="Email"
              :rules="rule"
              required
              outlined
            />
          </v-col>
        </v-row>
        <v-row>
          <v-col>
            <v-text-field
              v-model="usuario.telefone"
              placeholder="Telefone"
              label="Telefone"
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
      to="/usuarios"
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
  name: 'CadastroUsuariosPage',
  data () {
    return {
      valid: false,
      usuario: {
        nome: null,
        cpfcnpj: null,
        email: null,
        telefone: null
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
      let usuario = {
        nome: this.usuario.nome,
        cpfcnpj: this.usuario.cpfcnpj,
        email: this.usuario.email,
        telefone: this.usuario.telefone
      };
      if (!this.usuario.id) {
        await this.$axios.$post('http://localhost:3333/usuarios', usuario);
        this.$toast.success('Cadastro realizado com sucesso!');
        return this.$router.push('/usuarios');
      }

      await this.$axios.$post(`http://localhost:3333/usuarios/${this.usuario.id}`, usuario);
      this.$toast.success('Cadastro atualizado com sucesso!');
      return this.$router.push('/usuarios');

      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }

    },
    async getById (id) {
      this.usuario = await this.$axios.$get(`http://localhost:3333/usuarios/${id}`);
    }
  }
}
</script>
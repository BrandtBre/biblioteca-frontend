<template>
  <v-container>
    <h1 style="">Cadastro de Usuarios</h1>
    <hr>
    <v-form>
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
      usuario: {
        nome: null,
        cpfcnpj: null,
        email: null,
        telefone: null
      }
    }
  },
  methods: {
    async cadastrar () {
      let usuario = {
        nome: this.usuario.nome,
        cpfcnpj: this.usuario.cpfcnpj,
        email: this.usuario.email,
        telefone: this.usuario.telefone
      };
      console.log(usuario.cpfcnpj);
      let response = await this.$axios.$post('http://localhost:3333/usuarios', usuario);
      console.log(response);
    }
  }
}
</script>
<template>
  <v-container>
    <h1 style="">Cadastro de Autores</h1>
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
              v-model="usuario.email"
              placeholder="Email"
              label="Email"
              outlined
            />
          </v-col>
        </v-row>
      </v-container>
    </v-form>
    <v-btn
      outlined
      to="/autores"
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
  name: 'CadastroAutoresPage',
  data () {
    return {
      usuario: {
        nome: null,
        email: null,
      }
    }
  },
  methods: {
    async cadastrar () {
      try {
      let usuario = {
        nome: this.usuario.nome,
        email: this.usuario.email
      };
      await this.$axios.$post('http://localhost:3333/autores', usuario);
      this.$toast.success('Cadastro realizado com sucesso!');
      this.$router.push('/autores');
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar o cadastro!');
      }
    }
  }
}
</script>
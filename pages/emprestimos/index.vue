<template>
  <v-container>
    <h1>Consulta de Emprestimos</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getEmprestimos"
          >
            Pesquisar
            <v-icon style="margin-left: 5%">
              mdi-magnify
            </v-icon>
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -80%"
            outlined
            to="/emprestimos/cadastro"
          >
            Cadastrar
            <v-icon style="margin-left: 5%">
              mdi-plus-circle-outline
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="autores"
        :items-per-page="10"
        class="elevation-1"
      >
        <template v-slot:item.actions="{ item }">
              <v-icon
                small
                class="mr-2"
                @click="editItem(item)"
              >
                mdi-pencil
              </v-icon>
              <v-icon
                small
                @click="deletar(item)"
              >
                mdi-delete
              </v-icon>
        </template>
      </v-data-table>
    </v-container>
  </v-container>
</template>

<script>
export default {
  name: 'ConsultaAutoresPage',
  data () {
    return {
      headers: [
        {
          text: 'Código', //nome da coluna
          align: 'center', //alinhamento -center, end, start
          sortable: false, //se permite ordenação dos dados por essa coluna
          value: 'id', //é o dado que essa coluna vai receber
        },
        {
          text: 'Id do Usuário',
          align: 'center',
          sortable: false,
          value: 'idUsuario',
        },
        {
          text: 'Prazo',
          align: 'center',
          sortable: false,
          value: 'prazo'
        },
        {
          text: 'Devolução',
          align: 'center',
          sortable: false,
          value: 'devolucao'
        },
        {text: "", value: "actions"}
      ],
      emprestimos: []
    }
  },

  created () {
    this.getEmprestimos()
  },
  
  methods: {
    async getEmprestimos () {
      this.emprestimos = await this.$axios.$get('http://localhost:3333/emprestimos');
    },

    async deletar (emprestimo) {
      try {
        if (confirm(`Deseja deletar o emprestimo id ${emprestimo.id} ?`)) {
          let response = await this.$axios.$post('http://localhost:3333/emprestimos/deletar', { id: emprestimo.id });
          this.$toast(response.message)
          this.getEmprestimos();
        }
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao deletar o registro');
      }

    },

    async editItem (categoria) {
      this.$router.push({
        name: 'autores-cadastro',
        params: { id: categoria.id }
      });
    }
  }
}
</script>
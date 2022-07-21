<template>
  <v-container>
    <h1>Consulta de Livros</h1>
    <hr>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            outlined
            @click="getLivros"
            color="blue"
            
          >
            Pesquisar
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            style="margin-left: -80%"
            outlined            
            to="/livros/cadastro"
            color="green"
            
          >
            Cadastrar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <v-container>
      <v-data-table
        :headers="headers"
        :items="livros"
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
  name: 'ConsultaLivrosPage',
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
          text: 'Titulo',
          align: 'center',
          sortable: false,
          value: 'titulo',
        },
        {
          text: 'Sinopse',
          align: 'center',
          sortable: false,
          value: 'sinopse',
        },
        {
          text: 'Nome da Categoria',
          align: 'center',
          sortable: false,
          value: 'categoria.nome'
        },
        {
          text: 'Autor',    
          align: 'center',
          sortable: false,
          value: 'autor.nome'    
        },
        {text: "", value: "actions"}
      ],

      livros: []
    }
  },

  created () {
    this.getLivros()
    
  },
  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get('http://localhost:3333/livros');
    },
    async deletar (autor) {
      if (confirm(`Deseja deletar o livro id ${autor.id} - ${autor.titulo}?`)) {
        let response = await this.$axios.$post('http://localhost:3333/livros/deletar', { id: autor.id });
        this.$toast(response.message)
        this.getAutores();
      }
    }
  }
}
</script>
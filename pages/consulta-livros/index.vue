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
              color="blueviolet"
            ></v-autocomplete>
          </v-col>
        </v-row>
    </v-container>
    <v-container>
      <v-row>
        <v-col>
          <v-btn
            @click="pesquisarLivro"
          >
            Pesquisar
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
    <template v-if="this.idLivro && this.emprestimos.emprestado == true">
      <v-card
        class="mx-auto"
        max-width="344"
      >

        <v-card-title>
          Emprestado 
          <v-icon>
            mdi-emoticon-sad-outline
          </v-icon>
        </v-card-title>

        <v-card-subtitle>
          Este livro ja esta emprestado !
        </v-card-subtitle>

        <v-card-actions>
          <v-btn
            color="#c91818"
            text
            @click="fecharCard"
          >
            Fechar
          </v-btn>

          <v-spacer></v-spacer>

          <v-btn
            icon
            @click="show = !show"
          >
            <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
          </v-btn>
        </v-card-actions>

        <v-expand-transition>
          <div v-show="show">
            <v-divider></v-divider>

            <v-card-text>
              O livro {{ livroSelecionado.titulo }} ja está sobre emprestimo, a data prazo de devolução é {{ emprestimos.emprestimos[0].prazo }}
            </v-card-text>
          </div>
        </v-expand-transition>
      </v-card>
  </template>
  <template v-if="this.idLivro && this.emprestimos.emprestado == false">
      <v-card
        class="mx-auto"
        max-width="344"
      >

        <v-card-title>
          Livro para emprestimo
          <v-icon>
            mdi-emoticon-happy-outline
          </v-icon>
        </v-card-title>

        <v-card-subtitle>
          Este livro não esta emprestado !
        </v-card-subtitle>

        <v-card-actions>
          <v-btn
            color="#c91818"
            text
            @click="fecharCard"
          >
            Fechar
          </v-btn>

          <v-spacer></v-spacer>

          <v-btn
            icon
            @click="show = !show"
          >
            <v-icon>{{ show ? 'mdi-chevron-up' : 'mdi-chevron-down' }}</v-icon>
          </v-btn>
        </v-card-actions>

        <v-expand-transition>
          <div v-show="show">
            <v-divider></v-divider>

            <v-card-text>
              O livro {{ livroSelecionado.titulo }} não está sobre emprestimo, sua sinopse é {{ livroSelecionado.sinopse}}
            </v-card-text>
          </div>
        </v-expand-transition>
      </v-card>
  </template>
  </v-container>
</template>

<script>
export default {
  name: 'CadastroLivrosPage',
  data () {
    return {
      show: false,
      idLivro: false,
      livros: [],
      emprestimos: [],
      livroSelecionado: {}

    }
  },

  created () {
    this.getLivros();
  },
 
  methods: {
    async getLivros () {
      this.livros = await this.$axios.$get(`http://localhost:3333/livros`);
    },

    async pesquisarLivro () {
      try {
        if (!this.idLivro) {
          return this.$toast.error('Selecione um livro antes de realizar a pesquisa!');
        }
        this.livros.forEach(livro => {
          if (livro.id === this.idLivro) {
            this.livroSelecionado = livro;
            return;
          }          
        });
        this.emprestimos = await this.$axios.$post(`http://localhost:3333/emprestimos/encontrar-emprestimo`, {idLivro: this.idLivro})
      } catch (error) {
        this.$toast.error('Ocorreu um erro ao realizar a pesquisa!');
      }

    },

    async fecharCard () {
      this.idLivro = false;
    }
  }

}
</script>


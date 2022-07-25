<template>
  <v-container>
    <v-container style=" padding: 10%;  "> 
      <v-row>
        <v-col>
          <v-text-field
            v-model="visor"
            outlined
            disabled
            color="#8a2be2"
            
          >
          </v-text-field>
        </v-col>
      </v-row>
      <v-row>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('7')" 
          >
            7
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('8')" 
          >
            8
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('9')" 
          >
            9
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
            @click=" divisao()" 
          >
            <v-icon>
              mdi-division
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>
      
      <v-row>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('4')" 
          >
            4
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('5')" 
          >
            5
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('6')" 
          >
            6
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
            @click=" multiplicacao()" 
          >
            <v-icon>
              mdi-close
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>
      
      <v-row>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('1')" 
          >
            1
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('2')" 
          >
            2
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('3')" 
          >
            3
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
            @click=" substracao()" 
          >
            <v-icon>
              mdi-minus
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>

      <v-row>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
           @click=" numeroClicado ('0')" 
          >
          0
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"

          >
          .
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
            @click="resultado()"
          >
          <v-icon>
            mdi-equal
          </v-icon>
          </v-btn>
        </v-col>
        <v-col>
          <v-btn
            outlined
            color="#8a2be2"
            @click=" adicao()"
          >
            <v-icon>
              mdi-plus
            </v-icon>
          </v-btn>
        </v-col>
      </v-row>
    </v-container>
  </v-container>
</template>

<script>
export default {
  data () {
    return {
      numeros: [],
      visor: 0,
      aux: 0,
      res: [],
      valor: 0
    }

  },

  methods: {
    numeroClicado (numero) {
      this.numeros.push(numero);
      this.visor = this.numeros.join("");
      this.aux = this.numeros.join("");
      
    },

    adicao () {
      this.res.push(Number(this.aux));
      this.numeros = []
      this.res.push('+')


    },

    substracao () {
      this.res.push(Number(this.aux));
      this.numeros = []
      this.res.push('-')
    },

    multiplicacao () {
      this.res.push(Number(this.aux));
      this.numeros = []
      this.res.push('*')
    },
    
    divisao () {
      this.res.push(Number(this.aux));
      this.numeros = []
      this.res.push('/')
    },

    resultado () {
      this.res.push(Number(this.aux))
      this.carculaFinalmente()
    },

    carculaFinalmente () {
      for (let index = 0; index < this.res.length; index++) {
        if (index === 0) {
          this.valor = this.res[index]
        } else if (this.res[index] % 2 !== 0) {
          switch (this.res[index]) { 
            case '+':
                this.valor += this.res[index + 1]
              break;
            case '-':
                this.valor -= this.res[index + 1]
              break;
            case '/':
                this.valor /= this.res[index + 1]
              break;
            case '*':
                this.valor *= this.res[index + 1]
              break;
            default:
              break;
          }
          this.visor = this.valor;
        }
        
      }
      
    }    
    
  },
  
}
</script>


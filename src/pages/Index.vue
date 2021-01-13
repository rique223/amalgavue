<template id="app">
  <div id="app" class="q-px-md">

    <div class="row q-col-gutter-xs">
      <div class="col-md-5">
        <q-input square @submit.prevent outlined class="q-my-md" label="Titulo" @keyup.enter="novoCep()" v-model="titulo"/>
      </div>
      
      <div class="col-md-4">
        <q-form>
          <q-input square @submit.prevent mask="##.###-###" outlined class="q-my-md" @keyup.enter="novoCep()" label="CEP" v-model="mensagem"/>
        </q-form>
      </div>

      <div class="col-md-2">
        <q-btn unelevated class="full-width q-mt-md" color="info" icon-right="send" label="ENVIAR" size="21.7px" @click="novoCep()"/>
      </div>

      <div class="col-md-1">
        <q-btn unelevated class="full-width q-mt-md" color="negative" size="21.7px" @click="deleteGeral()">
          <q-icon color="white" :name="fasTimesCircle"/>
          <q-tooltip>
            Delete todos os cards
          </q-tooltip>
        </q-btn>
      </div>
    </div>

    <transition-group name="fade" tag="div" class="row items-end q-col-gutter-md">
      <div class="col-md-3" v-for="cep in ceps" :key="cep.id">
          <q-card align="middle">
            <q-card-section :class="classes">
              <div class="absolute-top-left q-pt-sm q-pl-md text-light-blue-10">
                <span>{{ cep.id }}</span>
              </div>

              <div style="z-index: 1" class="absolute-top-right q-pt-xs q-pr-sm cursor-pointer">
                <q-icon color="white" :name="fasTimes" @click="fechar(cep)"/>
              </div>

              <div class="absolute-center full-width q-pa-lg">
                <article class="q-mb-lg">
                  <h2 style="margin: 0; cursor: default" class="text-h3 text-light-blue-1">{{ cep.titulo }}</h2>
                </article>

                <article class="q-mb-md">
                  <p style="cursor: default" class="text-h5 text-bold q-ma-none text-left text-light-blue-10">{{ cep.cep }}</p>
                  <h2 style="cursor: default" class="text-h5 text-left q-ma-none text-weight-light text-light-blue-1">CEP</h2>
                </article>
                
                <article class="q-mb-md">
                  <p style="cursor: default" class="text-h5 text-bold q-ma-none text-left text-light-blue-10">{{ cep.rua }}  {{ cep.bairro }} {{ cep.cidade }}-{{ cep.estado }}</p>
                  <h2 style="cursor: default" class="text-h5 text-left q-ma-none text-weight-light text-light-blue-1">Endereço</h2>
                </article>
              </div>
            </q-card-section>
          </q-card>
      </div>
    </transition-group>
  </div>
</template>

<script>
  import { fasTimes } from '@quasar/extras/fontawesome-v5';
  import { fasTimesCircle } from '@quasar/extras/fontawesome-v5';
  import { axios } from '../boot/axios';

  export default {
    name: 'PageIndex',
    data() {
      return {
        classes: 'teste text-h3 text-weight-bold text-uppercase bg-info vertical-middle',
        id: 1,
        mensagem: '',
        titulo: '',
        ceps: [],
        fasTimes: '',
        fasTimesCircle: ''
      }
    },
    created() {
      this.fasTimes = fasTimes;
      this.fasTimesCircle = fasTimesCircle;
      if(localStorage.getItem('CEPS')){
        this.ceps = JSON.parse(localStorage.getItem('CEPS'));
      }
    },
    watch: {
      ceps: function (val) {
        localStorage.setItem('CEPS' ,JSON.stringify(val));
      }
    },
    methods: {
      fechar(cep) {
        const cepIndex = this.ceps.indexOf(cep);
        this.ceps.splice(cepIndex, 1);
        
        if(this.id === 1) {
          this.id = 1;
        } else {
          this.id--;
        }
      },
      novoCep() {
        let novoCep;

        if(this.titulo !== '' && this.mensagem !== ''){
          this.$axios.get(`https://brasilapi.com.br/api/cep/v1/${this.mensagem}`)
          .then(
            (response) => {
              novoCep = {
                id: this.id, 
                cep: this.formataCep(response.data.cep), 
                estado: response.data.state, 
                cidade: response.data.city, 
                bairro: response.data.neighborhood, 
                rua: response.data.street, 
                titulo: this.titulo
              };

              if(this.ceps.filter(provavelCEP => provavelCEP.cep === novoCep.cep).length === 0) {
                this.ceps.push(novoCep);
                this.id++;
              } else {
                this.$q.notify ({
                  color: 'positive',
                  position: 'bottom',
                  message: 'Esse cep já foi armazenado',
                  icon: 'done'
                })
              }
            }
          )
          .catch(() => {
            this.$q.notify({
              color: 'negative',
              position: 'top',
              message: 'Este cep não existe',
              icon: 'report_problem'
            })
          })
        }
      },
      formataCep(str) {
        let part1 = str.slice(0, 2);
        let part2 = str.slice(2, 5);
        let part3 = str.slice(5, 8);
        
        str = part1 + '.' + part2 + '-' + part3;

        return str;
      },
      deleteGeral() {
        this.ceps = [];
        localStorage.setItem('CEPS', []);
        this.id = 0;
      }
    }
  }
</script>

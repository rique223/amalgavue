<template id="app">
  <div id="app" class="q-px-md">

    <div class="row q-col-gutter-xs">
      <div class="col-md-5">
        <q-form>
          <q-input square outlined class="q-my-md" label="CEP" v-model="mensagem"/>
        </q-form>
      </div>

      <div class="col-md-5">
        <q-input square outlined class="q-my-md" label="Titulo" v-model="titulo"/>
      </div>

      <div class="col-md-2">
        <q-btn unelevated class="full-width q-mt-md" color="info" icon-right="send" label="ENVIAR" size="21.7px" @click="novoCep()"/>
      </div>
    </div>


    <transition-group name="fade" tag="div">
      <transition-group name="fade" tag="div" class="row q-col-gutter-md items-end" v-for="i in Math.ceil(ceps.length / 4)" :key="i">
        <div class="col-md-3 q-my-sm" v-for="cep in ceps.slice((i-1) * 4, i * 4)" :key="cep">
            <q-card align="middle">
              <q-card-section :class="classes">
                <div class="absolute-top-right q-pt-xs q-pr-sm" @click="fechar(cep)">
                  <q-icon color="red" :name="fasTimes" />
                </div>

                <div class="absolute-center">
                  <p>{{ cep.title }}</p>
                  <p>{{ cep.rua }}</p>
                </div>
              </q-card-section>
            </q-card>
        </div>
      </transition-group>
    </transition-group>

  </div>
</template>

<script>
  import { fasTimes } from '@quasar/extras/fontawesome-v5';

  export default {
    name: 'PageIndex',
    data() {
      return {
        msg: 'Test',
        classes: 'teste text-h3 text-weight-bold text-uppercase bg-info vertical-middle',
        id: 0,
        mensagem: '',
        titulo: '',
        ceps: []
      }
    },
    created() {
      this.fasTimes = fasTimes;
    },
    methods: {
      fechar(cep) {
        const cepIndex = this.ceps.indexOf(cep);
        this.ceps.splice(cepIndex, 1);
      },
      novoCep() {
        const novoEndereco = {id: this.id, title: this.titulo, rua: this.mensagem};

        this.id++;
        
        if(this.titulo !== '' && this.mensagem !== ''){
          this.ceps.push(novoEndereco);
        }
      }
    }
  }
</script>

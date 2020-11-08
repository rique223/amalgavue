<template id="app">
  <div id="app" class="q-px-md">
    <div class="row items-end q-col-gutter-md">

      <div class="col-3 q-my-md">
        <q-card align="middle">
          <q-card-section :class="classes">
            <div class="absolute-top-right q-pt-xs q-pr-sm" @click="fechar($event)">
              <q-icon color="red" :name="fasTimes" />
            </div>

            <div class="absolute-center">
              <p>{{ msg }}</p> 
            </div>
          </q-card-section>
        </q-card>
      </div>

      <div class="col-3 q-my-md">
        <q-card align="middle">
          <q-card-section :class="classes">
            <div class="absolute-top-right q-pt-xs q-pr-sm" @click="fechar($event)">
              <q-icon color="red" :name="fasTimes" />
            </div>

            <div class="absolute-center">
              <p>{{ msg }}</p> 
            </div>
          </q-card-section>
        </q-card>
      </div>

      <div class="col-3 q-my-md">
        <q-card align="middle">
          <q-card-section :class="classes">
            <div class="absolute-top-right q-pt-xs q-pr-sm" @click="fechar($event)">
              <q-icon color="red" :name="fasTimes" />
            </div>

            <div class="absolute-center">
              <p>{{ msg }}</p> 
            </div>
          </q-card-section>
        </q-card>
      </div>

      <div class="col-3 q-my-md">
        <q-card align="middle">
          <q-card-section :class="classes">
            <div class="absolute-top-right q-pt-xs q-pr-sm" @click="fechar($event)">
              <q-icon color="red" :name="fasTimes" />
            </div>

            <div class="absolute-center">
              <p>{{ msg }}</p> 
            </div>
          </q-card-section>
        </q-card>
      </div>

    </div>
  </div>
</template>

<script>
  import { fasTimes } from '@quasar/extras/fontawesome-v5';

  export default {
    name: 'PageIndex',
    data() {
      return {
        msg: 'Test',
        classes: 'teste text-h3 text-weight-bold text-uppercase bg-info vertical-middle'
      }
    },
    created() {
      this.fasTimes = fasTimes
    },
    methods: {
      fechar(event) {
        event.currentTarget.parentElement.parentElement.classList.add("hidden");
      }
    }
  }
</script>

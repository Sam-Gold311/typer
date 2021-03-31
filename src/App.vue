<template>
  <v-app>
    <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="text-h4 d-flex align-self-center">
        Typer
      </div>

    </v-app-bar>

    <v-main>
      <div class='typer d-flex flex-column align-center'>
        <div class="typer__title text-h1 animate__animated animate__fadeInLeftBig">
          Welcome to the Typer
        </div>
        <div class="typer__subtitle text-h2 animate__animated animate__fadeInRightBig animate__delay-1s">
          Show your speed!
        </div>
        
        <div class="typer__keyboard animate__animated animate__fadeIn animate__delay-2s animate__slow">
          <Key ref='keys' v-for='(key, i) in enVersion.keys' :key='i' :somekey='key'/>
        </div>
      </div>
    </v-main>
  </v-app>
</template>

<script>
import Key from '@/components/Key.vue'

export default {
  name: 'App',

  components: {
    Key
  },
  data: () => ({
    enVersion: {
      keys: [
        { keyText: 'q', code: 'KeyQ' },
        { keyText: 'w', code: 'KeyW' },
      ]
    }
  }),
  mounted() {
    document.addEventListener('keydown', this.keyPressed);
  },
  methods: {
    keyPressed(e) {
      for (let i = 0; i < this.enVersion.keys.length; i++) {
        if(this.$refs.keys[i].somekey.code === e.code) {
          this.$refs.keys[i].keyPressed();
          return;
        }
      }
    }
  },
};
</script>


<style media="screen" lang='scss'>

  .typer {
    &__title {
      margin: 25px 0;
    }
    &__subtitle {
      margin-bottom: 50px;
    }
    &__area {
      width: 60%;
    }
    &__keyboard {
      display: flex;
      padding: 10px;
      border-radius: 10px;
      border: 2px solid #000;
      width: 40%;
      height: 300px;
      font-weight: bold;
      font-size: 20px;
    }
  }
</style>

<template>
  <v-app>
    <!-- <v-app-bar
      app
      color="primary"
      dark
    >
      <div class="text-h4 d-flex align-self-center">
        Typer
      </div>

    </v-app-bar> -->

    <v-main>
      <div class='typer d-flex flex-column align-center'>
        <div class="typer__title text-h1 animate__animated animate__fadeInLeftBig">
          Welcome to the Typer
        </div>
        <div class="typer__subtitle text-h2 animate__animated animate__fadeInRightBig animate__delay-1s">
          Show your speed!
        </div>
        <div class="typer__text">
          {{en.text}}
        </div>
        <hr>
        <div class="typer__keyboard animate__animated animate__fadeIn animate__delay-2s animate__slow">
          <div class="">
            <Key ref='keys' @change-text='changeText' :nextLetter='nextLetter' v-for='(key, i) in firstLineKeys' :key='i' :somekey='key'/>
          </div>
          <div class="">
            <Key ref='keys' @change-text='changeText' :nextLetter='nextLetter' v-for='(key, i) in secondLineKeys' :key='i' :somekey='key'/>
          </div>
          <div class="">
            <Key ref='keys' @change-text='changeText' :nextLetter='nextLetter' v-for='(key, i) in thirdLineKeys' :key='i' :somekey='key'/>
          </div>
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
    en: {
      text: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      keys: [
        { keyText: 'q', code: 'KeyQ' },
        { keyText: 'w', code: 'KeyW' },
        { keyText: 'e', code: 'KeyE' },
        { keyText: 'r', code: 'KeyR' },
        { keyText: 't', code: 'KeyT' },
        { keyText: 'y', code: 'KeyY' },
        { keyText: 'u', code: 'KeyU' },
        { keyText: 'i', code: 'KeyI' },
        { keyText: 'o', code: 'KeyO' },
        { keyText: 'p', code: 'KeyP' },
        { keyText: '[', code: 'BracketLeft' },
        { keyText: ']', code: 'BracketRight' },
        { keyText: 'a', code: 'KeyA' },
        { keyText: 's', code: 'KeyS' },
        { keyText: 'd', code: 'KeyD' },
        { keyText: 'f', code: 'KeyF' },
        { keyText: 'g', code: 'KeyG' },
        { keyText: 'h', code: 'KeyH' },
        { keyText: 'j', code: 'KeyJ' },
        { keyText: 'k', code: 'KeyK' },
        { keyText: 'l', code: 'KeyL' },
        { keyText: ';', code: 'Semicolon' },
        { keyText: "'", code: 'Quote' },
        { keyText: 'z', code: 'KeyA' },
        { keyText: 'x', code: 'KeyS' },
        { keyText: 'c', code: 'KeyD' },
        { keyText: 'v', code: 'KeyF' },
        { keyText: 'b', code: 'KeyG' },
        { keyText: 'n', code: 'KeyH' },
        { keyText: 'm', code: 'KeyJ' },
        { keyText: ',', code: 'KeyK' },
        { keyText: '.', code: 'KeyL' },
        { keyText: '/', code: 'Semicolon' },
      ]
    }
  }),
  mounted() {
    document.addEventListener('keydown', this.keyPressed);
  },
  methods: {
    keyPressed(e) {
      for (let i = 0; i < this.en.keys.length; i++) {
        if(this.$refs.keys[i].somekey.keyText === e.key) {
          this.$refs.keys[i].keyPressed(e);
          return;
        }
      }
    },
    changeText() {
      console.log('used');
      this.en.text = this.en.text.slice(1);
    }
  },
  computed: {
    firstLineKeys() {
      return this.en.keys.slice(0, 12);
    },
    secondLineKeys() {
      return this.en.keys.slice(12, 23);
    },
    thirdLineKeys() {
      return this.en.keys.slice(23, 33);
    },
    nextLetter() {
      return this.en.text.slice(0, 1);
    }
  }
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
    &__text {
      font-size: 25px;
      width: 60%;
      &:first-letter {
        background: #C5CAE9;
      }
    }
    & hr {
      margin: 50px 0;
      width: 100%;
    }
    &__keyboard {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: center;
      padding: 15px;
      border-radius: 10px;
      border: 3px solid #000;
      width: 750px;
      height: 300px;
      font-weight: bold;
      font-size: 23px;
      & > div {
        display: flex;
        & > div {
          &:not(:last-child) {
            margin-right: 5px;
          }
        }
      }
    }
  }
</style>

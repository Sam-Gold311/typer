<template>
  <v-app>
    <v-main>
      <div class='typer d-flex flex-column align-center'>
        <div class="typer__title text-h1 animate__animated animate__fadeInLeftBig">
          Welcome to the Typer
        </div>
        <div class="typer__subtitle text-h2 animate__animated animate__fadeInRightBig animate__delay-1s">
          Show your speed!
        </div>
        <div class="typer__text animate__animated animate__fadeIn animate__delay-2s animate__slow">
          <span>{{nextLetter}}</span>{{restText}}
          <div v-if='progress === 100' class="text-h3">
            <span>Mistakes: {{this.trainInfo.mistakes}}</span>
            <span>Speed: {{}}</span>
          </div>
        </div>
        <div class='typer__progress'>
          <v-progress-linear
            v-model='progress'
            color="blue-grey"
            height="25"
          >
            <!-- <strong>{{ progress }}%</strong> -->
          </v-progress-linear>

        </div>
        <hr>
        <div class="typer__controls">
          <div class="typer__keyboard animate__animated animate__fadeIn animate__delay-2s animate__slow">
            <div class="">
              <Key ref='keys' :uppercase='uppercase' v-for='(key, i) in firstLineKeys' :key='i' :somekey='key'/>
            </div>
            <div class="">
              <Key ref='keys' :uppercase='uppercase' v-for='(key, i) in secondLineKeys' :key='12 + i' :somekey='key'/>
            </div>
            <div class="">
              <Key ref='keys' :uppercase='uppercase' v-for='(key, i) in thirdLineKeys' :key='23 + i' :somekey='key'/>
            </div>
          </div>
          <div class="typer__buttons">
            <v-btn
              elevation="2"
              x-large
              :disabled='!this.buttons.start'
            >
              Start
            </v-btn>
            <v-btn
              elevation="2"
              x-large
              :disabled='!this.buttons.restart'
            >
              Restart
            </v-btn>
            <v-btn
              elevation="2"
              x-large
              color='error'
              :disabled='!this.buttons.stop'
            >
              Stop
            </v-btn>
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
    uppercase: false,
    keyboard: false,
    buttons: {
      start: true,
      restart: false,
      stop: false
    },
    trainInfo: {
      mistakes: 0
    },
    en: {
      stableText: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      text: 'Lorem ipsum dolor sit amet, consectetur adipisicing elit, sed do eiusmod tempor incididunt ut labore et dolore magna aliqua. Ut enim ad minim veniam, quis nostrud exercitation ullamco laboris nisi ut aliquip ex ea commodo consequat. Duis aute irure dolor in reprehenderit in voluptate velit esse cillum dolore eu fugiat nulla pariatur. Excepteur sint occaecat cupidatat non proident, sunt in culpa qui officia deserunt mollit anim id est laborum.',
      keys: [
        { lowText: 'q', upText: 'Q'},
        { lowText: 'w', upText: 'W'},
        { lowText: 'e', upText: 'E'},
        { lowText: 'r', upText: 'R'},
        { lowText: 't', upText: 'T'},
        { lowText: 'y', upText: 'Y'},
        { lowText: 'u', upText: 'U'},
        { lowText: 'i', upText: 'I'},
        { lowText: 'o', upText: 'O'},
        { lowText: 'p', upText: 'P'},
        { lowText: '[', upText: '{'},
        { lowText: ']', upText: '}'},
        { lowText: 'a', upText: 'A'},
        { lowText: 's', upText: 'S'},
        { lowText: 'd', upText: 'D'},
        { lowText: 'f', upText: 'F'},
        { lowText: 'g', upText: 'G'},
        { lowText: 'h', upText: 'H'},
        { lowText: 'j', upText: 'J'},
        { lowText: 'k', upText: 'K'},
        { lowText: 'l', upText: 'L'},
        { lowText: ';', upText: ':'},
        { lowText: "'", upText: '"'},
        { lowText: 'z', upText: 'Z'},
        { lowText: 'x', upText: 'X'},
        { lowText: 'c', upText: 'C'},
        { lowText: 'v', upText: 'V'},
        { lowText: 'b', upText: 'B'},
        { lowText: 'n', upText: 'N'},
        { lowText: 'm', upText: 'M'},
        { lowText: ',', upText: '<'},
        { lowText: '.', upText: '>'},
        { lowText: '/', upText: '?'},
        { lowText: ' ', upText: ' ', space: true}
      ]
    }
  }),
  methods: {
    startTrain() {
      document.addEventListener('keydown', this.pressedKey);
    },
    endTrain() {
      document.removeEventListener('keydown', this.pressedKey);
    },
    pressedKey(e) {
      if (e.key === 'Shift' && e.repeat === false) {
        this.uppercase = !this.uppercase;
        document.addEventListener('keyup', this.changeCase);
      }
      if (e.key === 'CapsLock') {
        this.uppercase = !this.uppercase;
      }
      for (let i = 0; i < this.en.keys.length; i++) {
        if(this.$refs.keys[i].somekey.lowText === e.key.toLowerCase()) {
          if(e.key === this.nextLetter) {
            this.$refs.keys[i].$el.classList.add('key_correct');
            setTimeout(() => this.$refs.keys[i].$el.classList.remove('key_correct'), 100);
            this.changeText();
          } else {
            this.$refs.keys[i].$el.classList.add('key_wrong');
            setTimeout(() => this.$refs.keys[i].$el.classList.remove('key_wrong'), 100);
            this.trainInfo.mistakes++;
          }
          return;
        }
      }
    },
    changeText() {
      this.en.text = this.en.text.slice(1);
    },
    changeCase(e) {
      if (e.key === 'Shift') {
        this.uppercase = !this.uppercase;
        document.removeEventListener('keyup', this.changeCase);
      }
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
      return this.en.keys.slice(23, 34);
    },
    nextLetter() {
      return this.en.text.slice(0, 1);
    },
    restText() {
      return this.en.text.slice(1);
    },
    progress() {
      const PERCENT = this.en.stableText.length / 100;
      if(this.en.text.length) {
        return Math.floor(((this.en.stableText.length - this.en.text.length) / PERCENT));
      } else {
        return 100;
      }
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
      font-size: 27px;
      width: 75%;
      min-height: 300px;
      white-space: pre-wrap;
      position: relative;
      & span {
        color: #fff;
        background: grey;
      }
    }
    &__progress {
      width: 75%;
    }
    & hr {
      margin: 40px 0;
      width: 100%;
    }
    &__controls {
      width: 75%;
      justify-content: center;
      display: flex;
    }
    &__buttons {
      display: flex;
      flex-direction: column;
      justify-content: space-around;
    }
    &__keyboard {
      margin-right: 40px;
      width: 700px;
      padding: 15px;
      border-radius: 10px;
      background: #E8EAF6;
      border: 3px solid #000;
      font-weight: bold;
      font-size: 23px;
      & > div {
        display: flex;
        flex-wrap: wrap;
        justify-content: center;
        &:not(:last-child) {
          margin-bottom: 5px;
        }
        & > div {
          &:not(:last-child) {
            margin-right: 5px;
          }
        }
      }
    }
  }
</style>

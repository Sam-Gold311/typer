<template>
  <v-app>
    <v-main>
      <div class='typer d-flex flex-column align-center'>
        <div class="typer__version">
          <v-btn-toggle
          mandatory
          v-model='version'
          >
            <v-btn
             :disabled='version === 0'
             @click='stopTrain'
             x-large
             >EN</v-btn>
            <v-btn
            :disabled='version === 1'
            @click='stopTrain'
            x-large
            >RU</v-btn>
          </v-btn-toggle>
        </div>
        <div class="typer__title text-h1 animate__animated animate__fadeInLeftBig">
          {{ this.content.title }}
        </div>
        <div class="typer__subtitle text-h2 animate__animated animate__fadeInRightBig animate__delay-1s">
          {{ this.content.subtitle }}
        </div>
        <div class="typer__text animate__animated animate__fadeIn animate__delay-2s animate__slow">
          <span>{{nextLetter}}</span>{{restText}}
          <div v-if='progress === 100' class="text-h3 d-flex flex-column">
            <span>Mistakes: {{this.trainInfo.mistakes}}</span>
            <span>Speed: {{this.speed}} CPM</span>
          </div>
        </div>
        <div class='typer__progress'>
          <v-progress-linear
            class='animate__animated animate__fadeIn animate__delay-2s animate__slow'
            :value='progress'
            color="blue-grey"
            height="25"
          >
            <!-- <strong>{{ progress }}%</strong> -->
          </v-progress-linear>

        </div>
        <hr>
        <div class="typer__controls">
          <div class="typer__keyboard animate__animated animate__faster">
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
          <div class="d-flex flex-column justify-space-between animate__animated animate__fadeIn animate__delay-2s animate__slow">
            <div class="typer__buttons">
              <v-btn
                @click='startTrain'
                large
                elevation="2"
                :disabled='!this.buttons.start'
              >
                Start
              </v-btn>
              <v-btn
                @click='restartTrain'
                large
                elevation="2"
                :disabled='!this.buttons.restart'
              >
                Restart
              </v-btn>
              <v-btn
                @click='stopTrain'
                large
                elevation="2"
                color='error'
                :disabled='!this.buttons.stop'
              >
                Stop
              </v-btn>
            </div>
            <div class="typer__timer">
              {{this.content.timer}}:<br>{{timer}}
            </div>
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
    version: 0,
    uppercase: false,
    keyboard: false,
    buttons: {
      start: true,
      restart: false,
      stop: false
    },
    timer: 0,
    timerInterval: null,
    trainInfo: {
      mistakes: 0
    },
    ru: {
      title: 'Добро пожаловать в Typer',
      subtitle: 'Покажи свою скорость!',
      stableText: 'Разнообразный и богатый опыт сложившаяся структура организации требуют от нас анализа систем массового участия. Значимость этих проблем настолько очевидна, что начало повседневной работы по формированию позиции требуют определения и уточнения новых предложений. Задача организации, в особенности же новая модель организационной деятельности влечет за собой процесс внедрения и модернизации дальнейших направлений развития.',
      text: 'Разнообразный и богатый опыт сложившаяся структура организации требуют от нас анализа систем массового участия. Значимость этих проблем настолько очевидна, что начало повседневной работы по формированию позиции требуют определения и уточнения новых предложений. Задача организации, в особенности же новая модель организационной деятельности влечет за собой процесс внедрения и модернизации дальнейших направлений развития.',
      keys: [
        { lowText: 'й', upText: 'Й' },
        { lowText: 'ц', upText: 'Ц' },
        { lowText: 'у', upText: 'У' },
        { lowText: 'к', upText: 'К' },
        { lowText: 'е', upText: 'Е' },
        { lowText: 'н', upText: 'Н' },
        { lowText: 'г', upText: 'Г' },
        { lowText: 'ш', upText: 'Ш' },
        { lowText: 'щ', upText: 'Щ' },
        { lowText: 'з', upText: 'З' },
        { lowText: 'х', upText: 'Х' },
        { lowText: 'ъ', upText: 'Ъ' },
        { lowText: 'ф', upText: 'Ф' },
        { lowText: 'ы', upText: 'Ы' },
        { lowText: 'в', upText: 'В' },
        { lowText: 'а', upText: 'А' },
        { lowText: 'п', upText: 'П' },
        { lowText: 'р', upText: 'Р' },
        { lowText: 'о', upText: 'О' },
        { lowText: 'л', upText: 'Л' },
        { lowText: 'д', upText: 'Д' },
        { lowText: 'ж', upText: 'Ж' },
        { lowText: 'э', upText: 'Э' },
        { lowText: 'я', upText: 'Я' },
        { lowText: 'ч', upText: 'Ч' },
        { lowText: 'с', upText: 'С' },
        { lowText: 'м', upText: 'М' },
        { lowText: 'и', upText: 'И' },
        { lowText: 'т', upText: 'Т' },
        { lowText: 'ь', upText: 'Ь' },
        { lowText: 'б', upText: 'Б' },
        { lowText: 'ю', upText: 'Ю' },
        { lowText: '.', upText: ',' },
        { lowText: ' ', upText: ' ', space: true}
      ],
      timer: 'Время'
    },
    en: {
      title: 'Welcome to the Typer',
      subtitle: 'Show your speed!',
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
      ],
      timer: 'Time'
    }
  }),
  methods: {
    startTrain() {
      document.addEventListener('keydown', this.pressedKey);
      this.buttons.start = false;
      this.buttons.restart = true;
      this.buttons.stop = true;
      this.$el.getElementsByClassName('typer__keyboard')[0].classList.remove('animate__fadeOutLeft');
      this.$el.getElementsByClassName('typer__keyboard')[0].classList.add('animate__fadeInLeft');
      this.timer = 0;
      this.timerInterval = setInterval(() => this.timer++, 1000);
      this.content.text = this.content.stableText;
    },
    restartTrain() {
      clearInterval(this.timerInterval);
      this.timer = 0;
      this.timerInterval = setInterval(() => this.timer++, 1000);
      this.content.text = this.content.stableText;
    },
    stopTrain() {
      document.removeEventListener('keydown', this.pressedKey);
      this.buttons.start = true;
      this.buttons.restart = false;
      this.buttons.stop = false;
      if(this.$el.getElementsByClassName('typer__keyboard')[0].classList.contains('animate__fadeInLeft')) {
        this.$el.getElementsByClassName('typer__keyboard')[0].classList.remove('animate__fadeInLeft');
        this.$el.getElementsByClassName('typer__keyboard')[0].classList.add('animate__fadeOutLeft');
      }
      clearInterval(this.timerInterval);
    },
    pressedKey(e) {
      if (e.key === 'Shift' && e.repeat === false) {
        this.uppercase = !this.uppercase;
        document.addEventListener('keyup', this.changeCase);
      }
      if (e.key === 'CapsLock') {
        this.uppercase = !this.uppercase;
      }
      for (let i = 0; i < this.content.keys.length; i++) {
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
      this.content.text = this.content.text.slice(1);
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
      return this.content.keys.slice(0, 12);
    },
    secondLineKeys() {
      return this.content.keys.slice(12, 23);
    },
    thirdLineKeys() {
      return this.content.keys.slice(23, 34);
    },
    nextLetter() {
      return this.content.text.slice(0, 1);
    },
    restText() {
      return this.content.text.slice(1);
    },
    speed() {
      const CHAR_PER_SECOND = (this.content.stableText.length - this.content.text.length) / this.timer;
      return Math.floor(CHAR_PER_SECOND * 60);
    },
    progress() {
      const PERCENT = this.content.stableText.length / 100;
      if(this.content.text.length) {
        return Math.floor(((this.content.stableText.length - this.content.text.length) / PERCENT));
      } else {
        this.stopTrain();
        return 100;
      }
    },
    content() {
      if(this.version === 0) {
        return this.en;
      } else {
        return this.ru;
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
      width: 80%;
      min-height: 300px;
      white-space: pre-wrap;
      position: relative;
      & > span {
        color: #fff;
        background: grey;
      }
    }
    &__progress {
      width: 80%;
    }
    & hr {
      margin: 40px 0;
      width: 100%;
    }
    &__controls {
      width: 80%;
      flex-wrap: wrap;
      justify-content: space-around;
      display: flex;
    }
    &__buttons {
      display: flex;
      & button:not(:last-child) {
        margin-right: 20px;
      }
    }
    &__version {
      position: absolute;
      top: 15px;
      right: 15px;
    }
    &__keyboard {
      opacity: 0;
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
    &__timer {
      align-self: center;
      text-align: center;
      font-size: 45px;
    }
  }
</style>

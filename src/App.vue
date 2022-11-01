/* eslint-disable no-unused-vars */
/* eslint-disable no-unused-vars */
<template>
  <div id="app">
    <img class="img" src="./assets/cronometro.png" />
    <a class="timer">{{ numero }}</a>

    <div class="btn-area">
      <button @click="goTimer" class="btn">{{ btn }}</button>
      <button @click="clearTimer" class="btn">CLEAR</button>
    </div>

    <div class="message" v-show="historico.length > 0">
      <ul>
        <li v-for="registro in historico" :key="registro.id">
          {{ registro.message }}
        </li>
      </ul>
      <button @click="historico = []" class="btnDelete" :id="idButton">
        Limpar historico
      </button>
    </div>
  </div>
</template>

<script>
export default {
  name: "App",
  data() {
    return {
      numero: 0,
      seg: 0,
      min: 0,
      hrs: 0,
      btn: "GO",
      timer: null,
      registro: "",
      historico: [],
      idButton: null,
    };
  },
  methods: {
    goTimer() {
      if (this.timer !== null) {
        //Algo rodando no timer
        clearInterval(this.timer);
        this.timer = null;
        this.btn = "GO";
        this.registro = {
          id: this.historico.length + 1,
          message: `Você pausou em: ${this.numero}`,
          tempo: this.numero,
        };
        if (this.seg !== 0) {
          this.historico.push(this.registro);
        }
        this.idButton = this.registro.id;
      } else {
        //o timer está zerado ou parado
        this.btn = "PAUSE";
        this.timer = setInterval(() => {
          this.playTimer();
        }, 1);
      }
    },
    clearTimer() {
      if (this.timer !== null) {
        clearInterval(this.timer);
        this.timer = null;
      }
      this.btn = "GO";
      this.numero = 0;
      this.hrs = 0;
      this.min = 0;
      this.seg = 0;
      this.historico = [];
    },
    playTimer() {
      this.seg++;
      if (this.seg == 59) {
        this.min++;
        this.seg = 0;
      }

      if (this.min == 59) {
        this.hrs++;
        this.min = 0;
      }

      let format =
        (this.hrs < 10 ? "0" + this.hrs : this.hrs) +
        ":" +
        (this.min < 10 ? "0" + this.min : this.min) +
        ":" +
        (this.seg < 10 ? "0" + this.seg : this.seg);

      return (this.numero = format);
    },
  },
};
</script>

<style>
#app {
  display: flex;
  width: 100%;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}

.img {
  width: 420px;
  height: 480px;
  padding-top: 20px;
}

.timer {
  color: #fff;
  font-size: 70px;
  margin-top: -250px;
}

.btn-area {
  margin-top: 200px;
  display: flex; /*todos os itens filhos recebem display flex*/
}

.btn {
  /*-webkit-user-select: none; --> Não permite selecionar com o cursor do mouse o texto do botão*/
  width: 150px;
  background-color: #1d1f2b;
  color: white;
  font-size: 20px;
  border: 1px solid #fff;
  border-radius: 20px;
  text-align: center;
  margin: 0 15px 0 15px;
  padding: 5px;
  cursor: pointer;
}

.btn:hover {
  background-color: #fff;
  color: #1d1f2b;
  transition: all 0.3s;
}

.message {
  color: #fff;
  font-style: oblique;
  width: 50%;
  text-align: center;
}

p {
  background-color: #fff;
  color: #1d1f2b;
  font-weight: bold;
  opacity: 0.5;
  padding: 10px;
}

ul {
  text-align: center;
  padding: 0px;
  list-style: none;
}

ul li {
  margin-top: 4px;
  padding: 15px;
  background-color: hsl(229, 19%, 22%);
  font-size: 18px;
  border: 1px solid #fff;
  border-radius: 30px;
}

.btnDelete {
  border: 1px solid #fff;
  padding: 5px;
  color: white;
  border-radius: 10px;
  background: #1d1f2b;
  margin-left: 3px;
}

.btnDelete:hover {
  color: #1d1f2b;
  background: #fff;
  transition: all 0.3s;
}
</style>

<template>
  <v-container class="my-5" style="background-color:rgb(53 73 94); padding:2%; border-radius: 6px;">
    <v-row>
      <v-card elevation="3" min-width="100%" min-height="50px" color="rgb(65 184 131)" style="display:flex; align-items: center;">
        <h4 class="mx-5">Timer: {{ tempo }}</h4>
        <v-spacer/>
        <v-btn color="rgb(53 73 94)" class="mx-5" @click="resetGame()"><span style="color:white;">Reiniciar</span></v-btn>
      </v-card>
    </v-row>
    <v-row class="text-center">
      <v-col v-for="(card, index) in cards" :key="index" class="my-2" cols="6" lg="2" md="3" sm="4">
        <v-card class="memory-card" :class="{ flipped: card.flipped, found: card.found }"
          @click="flipCard(card.image.id, index)">

          <v-img class="memory-card-front" :src="card.image.link"></v-img>
          <v-img class="memory-card-back" src="../assets/logo.png"></v-img>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
    
<script>
export default {
  name: 'HelloWorld',
  data() {
    return {
      cards: [],
      flippedCards: [],
      allowTouch: false,
      gameOver: false,
      remainingTime: 300, // 5 minutos em segundos
      tempo:'5:00',
      timerId: null,
    };
  },
  methods: {

    startGame() {
      this.cards.forEach((card) => {
        card.flipped = false;
      });
      setTimeout(() => {
        this.cards.forEach((card) => {
          card.flipped = true;
        });
        this.allowTouch = true;
      }, 6000);

      // Início do contador regressivo
      let timer = setInterval(() => {
        this.countdown();
        if (this.remainingTime === 0) {
          clearInterval(timer);
          this.gameOver = true;
          alert("Tempo esgotado. Tente novamente!");
          this.resetGame();
        }
      }, 1000);
    },

    countdown() {
      const minutes = Math.floor(this.remainingTime / 60);
      const seconds = this.remainingTime % 60;
      this.tempo = `${minutes < 10 ? '0' + minutes : minutes}:${seconds < 10 ? '0' + seconds : seconds}`;
      this.remainingTime--;
    },

    finishGame() {
  this.gameOver = true;
  this.cards.forEach((card) => {
    card.flipped = true;
  });
  alert("Parabéns! Você venceu o jogo!");

  // Código adicionado para exibir o tempo decorrido
  clearInterval(this.timerId);
  this.tempo = "00:00";
},

    flipCard(id, index) {
      if (this.allowTouch && !this.cards[index].found && !this.cards[index].clicked) {
        this.cards[index].flipped = false;
        this.cards[index].clicked = true;

        this.flippedCards.push(index);
        if (this.flippedCards.length === 2) {

          const firstCard = this.cards[this.flippedCards[0]];
          const secondCard = this.cards[this.flippedCards[1]];
          
          this.allowTouch = false;
          if (firstCard.image.id === secondCard.image.id) {
            this.allowTouch = true;
            this.cards[this.flippedCards[0]].flipped = false;
            this.cards[this.flippedCards[1]].flipped = false;
            this.cards[this.flippedCards[0]].clicked = false;
            this.cards[this.flippedCards[1]].clicked = false;

            firstCard.found = true;
            secondCard.found = true;
            this.flippedCards = [];
            this.checkForGameOver();
          } else {
            setTimeout(() => {
              this.cards[this.flippedCards[0]].flipped = true;
              this.cards[this.flippedCards[1]].flipped = true;
              this.cards[this.flippedCards[0]].clicked = false;
              this.cards[this.flippedCards[1]].clicked = false;

              this.flippedCards = [];
              this.allowTouch = true;
            }, 1000);
          }

          // this.flippedCards = [];
        }
      }
    },


    checkForGameOver() {
      if (this.cards.every((card) => card.found)) {
        setTimeout(() => {
          this.cards.forEach((card) => {
            card.flipped = true;
          });
          this.allowTouch = false;
          this.finishGame();
        }, 2000);
      }
    },

    shuffleCards() {
      for (let i = this.cards.length - 1; i > 0; i--) {
        const j = Math.floor(Math.random() * (i + 1));
        [this.cards[i], this.cards[j]] = [this.cards[j], this.cards[i]];
      }
    },

    resetGame() {
      location.reload()
    },
  },

  created() {
    const images = [
      { id: "1", link: "https://picsum.photos/200/200?random=1" },
      { id: "2", link: "https://picsum.photos/200/200?random=2" },
      { id: "3", link: "https://picsum.photos/200/200?random=3" },
      { id: "4", link: "https://picsum.photos/200/200?random=4" },
      { id: "5", link: "https://picsum.photos/200/200?random=5" },
      { id: "6", link: "https://picsum.photos/200/200?random=6" },
      { id: "7", link: "https://picsum.photos/200/200?random=7" },
      { id: "8", link: "https://picsum.photos/200/200?random=8" },
      { id: "9", link: "https://picsum.photos/200/200?random=9" },
      { id: "10", link: "https://picsum.photos/200/200?random=10" },
      { id: "11", link: "https://picsum.photos/200/200?random=11" },
      { id: "12", link: "https://picsum.photos/200/200?random=12" },
    ];

    this.cards = images.concat(images).map((image) => ({
      image,
      flipped: false,
      found: false,
    }));

    this.shuffleCards();
    this.startGame();
  },
};
</script>
  
<style scoped>
.memory-card {
  position: relative;
  width: 200px;
  height: 300px;
  border-radius: 10px;
  perspective: 1000px;
  cursor: pointer;
}

.memory-card .v-image {
  border-radius: 10px;
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transition: transform 0.6s ease;
  transform-style: preserve-3d;
  backface-visibility: hidden;
}

.memory-card .memory-card-back {
  transform: rotateY(180deg);
}

.memory-card.flipped .memory-card-front {
  transform: rotateY(-180deg);
}

.memory-card.flipped .memory-card-back {
  transform: rotateY(0deg);
}

.memory-card.found {
  cursor: default;
}

.text-center {
  text-align: center;
}

.my-5 {
  margin-top: 3rem;
  margin-bottom: 3rem;
}
</style>
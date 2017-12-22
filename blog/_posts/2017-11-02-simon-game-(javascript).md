---
layout: post
---

![alt text](https://images-na.ssl-images-amazon.com/images/I/91TacoMOvIL._SX355_.jpg "Simon")

I recently made my very own version of the popular game "Simon" in Javascript. It was the final project of freeCodeCamp's front-end development certification and it taught me a lot about object-oriented programming.

You can find a demo [here](https://codepen.io/jduran/pen/Yrbvvg).

This was my approach for the logic of the game:

I started by defining the constants LIGHT_DELAY, LIGHT_GAP, and MAX_ROUNDS, which set characteristics of the UI and the game itself, respectively.

{% highlight javascript %}
const LIGHT_DELAY = 600;
const LIGHT_GAP = LIGHT_DELAY / 3;
const MAX_ROUNDS = 20;
{% endhighlight %}

Then the class "Round" was defined, along with methods to: keep track of which button was clicked, starting the game, generating random moves, cleaning moves, showing the generated pattern, changing the color intensity of the buttons, playing sounds, and finally checking if the moves generated and played are the same:

{% highlight javascript %}
class Round {
  constructor(cpuMoves, updateSeq){
    this.cpuMoves = cpuMoves;
    this.humanMoves = [];
    this.updateSeq = updateSeq;
  };

  captureClick(band) { 
    console.log('captured ' + band)
    this.humanMoves.push(band);
    this.displayBand(band);
    this.checkMoves();
  }
  
  start(cb) {
    this.cb = cb;
    $('#round').text(this.cpuMoves.length + ((this.updateSeq) ? 1 : 0));
    this.generateMove();
    this.showPattern();
  }
  
  generateMove(){
    if(this.updateSeq) {
      this.cpuMoves.push(Math.floor((Math.random() * 4) + 1));
    }
  }
  
  showPattern(){
    let counter = 0;
    const handle = setInterval(()=>{
      if(counter < this.cpuMoves.length){
        this.displayBand(this.cpuMoves[counter++]);
      } else {
        clearInterval(handle);
        this.humanMoves = [];
      }
    }, LIGHT_DELAY + LIGHT_GAP)
  }
  
  cleanMoves(){
    this.cpuMoves = [];
    this.humanMoves = [];
  }

  displayBand(band){
    this.playSound(band);
    const $band = $('div[data-id="'+band+'"]');
    $band.addClass('highlight');
    setTimeout(() => {
        $band.removeClass('highlight');
    }, LIGHT_DELAY)
  }

  checkMoves(){
    for(var i=0;i<this.humanMoves.length;i++){
        if(this.cpuMoves[i] !== this.humanMoves[i]){
          this.cb(false);
          $('#round').text("WRONG!");
          this.humanMoves = [];
          return;
        }
    }
    if(this.cpuMoves.length == MAX_ROUNDS 
      && this.cpuMoves.length == this.humanMoves.length){
      $('#round').text("YOU WON!");
      game.round.cleanMoves();
    }
    if(this.humanMoves.length == this.cpuMoves.length){
     this.cb(true); 
    }
  }
  
  playSound(sound){
    let audio = new Audio('https://s3.amazonaws.com/freecodecamp/simonSound'+sound+'.mp3');
      audio.loop = false;
      audio.play();
  }
}
{% endhighlight %}
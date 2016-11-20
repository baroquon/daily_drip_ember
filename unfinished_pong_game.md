ember new air-hockey-game
ember g component game-paddle
ember g component game-ball
ember g service game-state
ember g component score-board
ember g component board-container

Paddle will have an ID for leftPaddle and one for rightPaddle

add a few classes so we can style our items:

gamePaddle: classNames: game-paddle
game-ball: classNames: game-ball
score-board: classNames: scoreBoard

add your two paddles and a ball in the application template:

{{#board-container}}
  {{game-paddle id="left-paddle"}}
  {{game-paddle id="right-paddle"}}
  {{game-ball}}
{{/board-container}}

Now, let's set up our game state on our service.

```JavaScript
export default Ember.Service.extend({
  // Properties
  leftScore: 0,
  rightScore: 0,
  boardWidth: null,
  boardHeight: null,
  leftPaddle: null,
  rightPaddle: null,
  ballPosition: [0, 0],

  initializeGame(boardWidth, boardHeight){
    let paddleHeight = boardHeight/2-30;
    this.setPaddlePosition('leftPaddle', paddleHeight);
    this.setPaddlePosition('rightPaddle', paddleHeight);
    this.setScore('rightScore', 0);
    this.setScore('leftScore', 0);

    Ember.set(this, 'boardWidth', boardWidth);
    Ember.set(this, 'boardHeight', boardHeight);

  },
  setBallPosition(xPos, yPos){
    if(Ember.isPresent(...arguments)){
      return Ember.set(this, 'ballPosition', [xPos, yPos]);
    } else {
      return Ember.get(this, 'ballPosition');
    }
  },
  setScore(side, score){
    if(Ember.isPresent(...arguments)){
      Ember.set(this, side, score);
    }
    return { rightScore: Ember.get(this, rightScore), leftScore: Ember.get(this, leftScore) };
  },
  setPaddlePosition(paddle, position){
    if(Ember.isPresent(position)){
      return Ember.set(this, paddle, position);
    } else {
      return Ember.get(this, paddle);
    }
  }
});

```

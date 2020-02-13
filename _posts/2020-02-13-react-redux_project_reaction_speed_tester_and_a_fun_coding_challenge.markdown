---
layout: post
title:      "React-Redux Project: REACTion Speed Tester and a fun coding challenge "
date:       2020-02-13 15:12:40 -0500
permalink:  react-redux_project_reaction_speed_tester_and_a_fun_coding_challenge
---


Git Hub repo: [https://github.com/dkintop/redux-reflex-game](http://)


For my React-Redux Project I decided to design a simple game where users could test and improve their reaction times by clicking a tile as quickly as possible after it appears on the screen. I did just that and am super proud that I managed to start with an idea and then figure it out step by step as I went. One challenge that I came across was that if the active game tile appeared in the same place on the screen every time it would decrease the challenge to the point of basically being pointless, so I was determined to figure out how to render the active game tile in a random location each time. 

The first step was telling the application where it was allowed to render the active tile, and I quickly figured out that the best way to do that was going to be to render some place holder divs and I knew that these tiles needed no other functionality other than to be there so I made a list of empty divs like so:

```
 matrix = [
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>,
    <div className="game-tile" ></div>
  ];
```

and then, beacuse react is awesome, I could render each of them like this:

```
render() {
    return <div className="game-board">{this.matrix}</div>;
  }
```

With a bit of css magic I was able to organize these divs into a visually appealing grid. So far so good! But now I needed to make one of those tiles function as a stop button for the timer that I wanted to start as soon as it was rendered. As opposed to trying to figure out how to make one of those tiles go from inactive to active, I decided that I could create a new component (ActiveTile)  to take care of dealing with all of the functionality that this tile would need to have.   After fiddling with intervals and lifecycle methods for awhile, I was able to come up with a component that had exactly the functionality I needed to accomplish the task of starting the time, stopping timer, and recording the results in my redux store to then later be sent to my database to maintain a list of the high scores. I'll include the code for that just because I am proud of it: 

```
import React, { Component } from "react";
import { recordScore } from "../actions/index.js";
import { addHighScore } from "../actions/index.js";
import { showForm } from "../actions/index.js";
import { connect } from "react-redux";
import { NewScoreForm } from "./NewScoreForm.js";
export class ActiveGameTile extends Component {
  state = {
    score: 0.0,
    showForm: false
  };

  incrementTime = () => {
    this.setState(prevState => ({
      ...prevState,
      score: prevState.score + 10
    }));
  };

  componentDidMount() {
    this.interval = setInterval(this.incrementTime, 10);
  }

  formatTime() {
    return this.state.score / 1000;
  }

  componentWillUnmount() {
    clearInterval(this.interval);
  }

  toggleForm = () => {
    this.setState(prevState => ({
      ...prevState,
      showForm: true
    }));
  };

  handleToggleForm = () => {
    if (this.props.showForm) {
      return <NewScoreForm />;
    }
  };
  //this NewScoreform Component does not have access to the props that are mapped in NewScoreForm.js for some ungodly reason. that is why it errors out on submit
  handleClick = () => {
    clearInterval(this.interval);
    this.props.recordScore(this.state.score);
    this.toggleForm();

    this.props.showForm();
  };

  render() {
    return (
      <div className="game-tile-active" key="90" onClick={this.handleClick}>
        <div>CLICK ME!</div>

        <div id="time-display">{this.formatTime()}</div>
        {/* {this.handleToggleForm()} */}
        {/* <NewScoreForm /> */}
      </div>
    );
  }
}

const mapStateToProps = state => {
  return {
    score: state.gameReducer.score,
    showForm: state.gameReducer.score
  };
};

let mapDispatchToProps = dispatch => {
  return {
    recordScore: score => dispatch(recordScore(score / 1000)),
    addHighScore: score => dispatch(addHighScore(score)),
    showForm: () => dispatch(showForm())
  };
};

export default connect(mapStateToProps, mapDispatchToProps)(ActiveGameTile);
```


Now that that beast was tamed, it was time to get back to figuring out how to render that component in a new location each time. I then quickly realized that if react was rendering an array of elements, then it had to have been doing it one at a time starting from index 0. Now all I needed to do was replace one of the divs in the matrix variable (I am using the term matrix loosely here, forgive me) with my active game tile and one of the tiles would now have all the properties and functionality of the activeGameTile component, and then figure out a way to change the position of that component in the matrix array each time it was rendered. 

I remembered reading somewhere something along the lines of "no one is going to be impressed if you reinvent the wheel," meaning that you will never get bonus points for writing code yourself that you could have gotten from somewehere else. Therefore I opted to find a sorting algorithm somewhere and imported that into my ActiveGameTile component file and used that to then shuffle my matrix array, which would put the ActiveGameTile component in a new position each time it was called, and then use the return value of that function to render the game tiles, like this! 


import { shuffle } from "../algorithms/shuffle.js";
import ActiveGameTile from "./ActiveGameTile.js";


export class Game extends Component {
  
	
	matrix = [
    <ActiveGameTile />,
    <div className="game-tile" key="10"></div>,
    <div className="game-tile" key="20"></div>,
    <div className="game-tile" key="30"></div>,
    <div className="game-tile" key="40"></div>,
    <div className="game-tile" key="50"></div>,
    <div className="game-tile" key="60"></div>,
    <div className="game-tile" key="70"></div>,
    <div className="game-tile" key="80"></div>
  ];

  shuffledMatrix = () => {
    return shuffle(this.matrix);
  };

  render() {
    return <div className="game-board">{this.shuffledMatrix()}</div>;
  }
	
	Boom! Now each time the Game component mounts the active tile will be rendered in a new position on the grid! This project as a whole was really fun and I really developed a love for react, and even redux (which I hated until I was about midway through the project.). Before this project I was certain I wanted nothing to do with front end UI development, now I can't stop playing with it! 


### Step-by-Step Breakdown of the Dice Game

---

#### HTML Setup:

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link rel="stylesheet" href="style.css" />
    <title>Pig Game</title>
  </head>
</html>
```

- **DOCTYPE Declaration**: The document type is defined as HTML.
- **Meta Charset**: Specifies the character encoding (UTF-8) to handle special characters.
- **Viewport**: Ensures the game is responsive on different devices.
- **X-UA-Compatible**: Helps with Internet Explorer rendering compatibility.
- **Stylesheet**: Links to the external CSS file for styling.
- **Title**: Sets the title of the webpage as "Pig Game."

---

#### Body and Game UI:

```html
  <body>
    <main>
      <section class="player player--0 player--active">
        <h2 class="name" id="name--0">Player 1</h2>
        <p class="score" id="score--0">43</p>
        <div class="current">
          <p class="current-label">Current</p>
          <p class="current-score" id="current--0">0</p>
        </div>
      </section>

      <section class="player player--1">
        <h2 class="name" id="name--1">Player 2</h2>
        <p class="score" id="score--1">24</p>
        <div class="current">
          <p class="current-label">Current</p>
          <p class="current-score" id="current--1">0</p>
        </div>
      </section>

      <img src="dice-5.png" alt="Playing dice" class="dice" />
      <button class="btn btn--new">🔄 New game</button>
      <button class="btn btn--roll">🎲 Roll dice</button>
      <button class="btn btn--hold">📥 Hold</button>
    </main>
    <script src="script.js"></script>
  </body>
</html>
```

1. **Main Content**: The game layout is built using sections for two players.
2. **Player Sections**:
   - Each player has their name, score, and current score displayed.
   - `player--0` is marked as the active player using `player--active` to give it special styling.
   - IDs like `score--0`, `current--0`, etc., are used for DOM manipulation to dynamically update the scores.
3. **Buttons**:
   - Buttons for starting a new game, rolling the dice, and holding the score are provided.
   - Each button has a unique class (e.g., `btn--new`, `btn--roll`, `btn--hold`).
4. **Dice Image**: Initially shows a dice image, and its `src` will dynamically change based on the roll.
5. **Script**: A link to the JavaScript (`script.js`) that contains the game logic.

---

#### CSS Styling:

```css
/* General Reset */
* {
  margin: 0;
  padding: 0;
  box-sizing: inherit;
}

html {
  font-size: 62.5%;
  box-sizing: border-box;
}

body {
  font-family: 'Nunito', sans-serif;
  font-weight: 400;
  height: 100vh;
  color: #333;
  background-image: linear-gradient(to top left, #753682 0%, #bf2e34 100%);
  display: flex;
  align-items: center;
  justify-content: center;
}
```

- **CSS Reset**: Resets default browser margins and paddings.
- **Font and Size**: Sets the base font to `Nunito` and the base font size to 62.5%, which is 10px (useful for easy scaling with rem units).
- **Flexbox Layout**: The `body` uses flexbox to center the main game area in the viewport, vertically and horizontally.

---

#### Layout and Player Sections:

```css
main {
  position: relative;
  width: 100rem;
  height: 60rem;
  background-color: rgba(255, 255, 255, 0.35);
  backdrop-filter: blur(200px);
  box-shadow: 0 3rem 5rem rgba(0, 0, 0, 0.25);
  border-radius: 9px;
  display: flex;
}

.player {
  flex: 50%;
  padding: 9rem;
  display: flex;
  flex-direction: column;
  align-items: center;
  transition: all 0.75s;
}
```

- **Main Layout**: The `main` container is centered and has a glassy, blurred background.
- **Player Section**: Each player occupies 50% of the game width, with padding and vertical alignment of content.

---

#### Player Elements:

```css
.name {
  font-size: 4rem;
  text-transform: uppercase;
  letter-spacing: 1px;
  font-weight: 300;
  margin-bottom: 1rem;
}

.score {
  font-size: 8rem;
  font-weight: 300;
  color: #c7365f;
}

.player--active {
  background-color: rgba(255, 255, 255, 0.4);
}

.current {
  background-color: #c7365f;
  border-radius: 9px;
  color: #fff;
  width: 65%;
  padding: 2rem;
  text-align: center;
}
```

- **Player Styling**: Names are large and uppercase for emphasis. The active player has a special background.
- **Score Display**: The scores are displayed prominently with large font sizes.
- **Current Score Box**: The player's current score is highlighted in red with a centered text.

---

#### Buttons and Dice:

```css
.btn {
  position: absolute;
  left: 50%;
  transform: translateX(-50%);
  color: #444;
  background-color: rgba(255, 255, 255, 0.6);
  padding: 0.7rem 2.5rem;
  border-radius: 50rem;
  cursor: pointer;
}

.dice {
  position: absolute;
  left: 50%;
  top: 16.5rem;
  transform: translateX(-50%);
  height: 10rem;
}
```

- **Buttons**: Centered horizontally, with a translucent background and rounded edges. They are styled for interaction.
- **Dice Image**: Positioned in the center of the game area, the image will dynamically change based on the dice roll.

---

### Game Logic Breakdown (JavaScript)

#### Resetting Scores:

```javascript
const score0El = document.getElementById('score--0');
const score1El = document.getElementById('score--1');
score0El.textContent = 0;
score1El.textContent = 0;
```

- **Goal**: To reset player scores to zero when the game starts or resets.
- **Why `getElementById`?**: It's slightly faster than `querySelector` because it directly selects by the ID.

#### Hiding the Dice:

```css
.hidden {
  display: none;
}
```

```javascript
const diceEl = document.querySelector('.dice');
diceEl.classList.add('hidden');
```

- **CSS**: Creates a hidden class to hide elements (in this case, the dice).
- **JavaScript**: Adds the `hidden` class to the dice, hiding it initially.

#### Rolling Dice Logic:

```javascript
btnRoll.addEventListener('click', () => {
  if (playing) {
    const dice = Math.trunc(Math.random() * 6) + 1;
    diceEl.classList.remove('hidden');
    diceEl.src = `dice-${dice}.png`;

    if (dice !== 1) {
      currentScore += dice;
      document.getElementById(`current--${activePlayer}`).textContent =
        currentScore;
    } else {
      switchPlayer();
    }
  }
});
```

- **Random Dice Roll**: Generates a number between 1 and 6, and the corresponding dice image is displayed by dynamically updating the `src` attribute.
- **Score Update**: If the dice roll isn’t 1, it adds the score to the current score. If it is 1, the turn switches.

#### Switch Player Logic:

```javascript
const switchPlayer = () => {
  document.getElementById(`current--${activePlayer}`).textContent = 0;
  currentScore = 0;
  activePlayer = activePlayer === 0 ? 1 : 0;
  player0El.classList.toggle('player--active');
  player1El.classList.toggle('player--active');
};
```

- **Purpose**: Resets the current score and switches the active player.
- **`toggle` Method**: Adds or removes the `player--active` class based on the active player.

---

### Summary:

- The project creates a **turn-based dice game** with a simple interface where two players can roll, hold, and compete for 100 points.
- It demonstrates the use of **event listeners**, **DOM manipulation**, and **state management** using JavaScript and basic CSS for styling.

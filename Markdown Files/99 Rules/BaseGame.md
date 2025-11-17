# AI Coding Prompt: Modular JavaScript Card Game Framework for **99**

This document defines a complete specification for generating clean, modular, extensible JavaScript code for the card game **99**. The goal is to build a highly structured codebase where **game rules, card logic, math rules, turn rules, and abilities** can be swapped or extended without changing the core architecture.

---

## 🎯 **Overall Goal**

Create a modular JavaScript card‑game engine for the game **99** using OOP principles. Each card should have its own file/class and derive from a shared abstract `Card` class. The game logic should allow rules to be replaced by passing in rule modules.

The base gameplay:

* Players take turns playing a card.
* Each card adds its numeric value to the running pile total.
* If the total exceeds **99**, the game ends.

---

## 📁 **Recommended File Structure**

```
src/
  core/
    Card.js (abstract)
    Player.js
    Hand.js
    DrawPile.js
    PlayPile.js
    Game.js

  cards/
    Ace.js
    Two.js
    Three.js
    ... etc

  rules/
    mathRules.js
    turnRules.js
    abilityRules.js

  config/
    ruleSelector.js
```

---

## 🧱 **1. Abstract Class: Card**

Every card file should extend this.

### Required Properties

* `name`
* `rank`
* `suit`
* `abilities` (array or object)
* `mathRule` (function reference)
* `turnRule` (function reference)

### Required Methods

* `applyMath(total, gameState)` → returns new total
* `applyTurn(gameState)` → affects whose turn it is
* `applyAbilities(gameState)` → optional abilities

### Required Behavior

The `Card` class should NOT determine the math, turn, or abilities.
Instead, the **rules are injected from ruleSelector.js**.

---

## 📝 **2. Example Card Class (Ace.js)**

Each card file should:

* Import `Card`
* Import rule selectors
* Provide rank, suit, and rule assignments

Example behaviors for Ace in base game:

* Math: +1
* Turn: normal (next player)
* Ability: none

---

## 🧠 **3. Rules System**

In the `/rules` folder:

### **mathRules.js**

Exports functions for numeric effects, e.g.:

* `addRankValue`
* `setTo99`
* `reverseTotal`

### **turnRules.js**

Exports functions for turn control behaviors, e.g.:

* `nextPlayer`
* `skipNext`
* `reverseOrder`

### **abilityRules.js**

Exports optional extras, e.g.:

* `noAbility`
* `drawExtraCard`
* `forceOpponentDiscard`

---

## 🎛️ **4. Rule Selector File**

### `/config/ruleSelector.js`

This selects the rule modules assigned to each specific card.

Example object:

```js
export const ruleSelector = {
  Ace: {
    math: mathRules.addRankValue,
    turn: turnRules.nextPlayer,
    ability: abilityRules.none,
  },
  // etc for all cards
};
```

Each card class simply queries this object.

---

## 🧩 **5. Core Game Classes**

### **Player**

* id
* name
* hand
* drawCard()
* playCard(card)

### **Hand**

* cards[]
* add(card)
* remove(card)

### **DrawPile**

* cards[] (shuffled)
* draw()

### **PlayPile**

* total
* addCard(card)

### **Game**

* players[]
* drawPile
* playPile
* currentPlayerIndex
* isGameOver
* initializeDeck()
* startGame()
* applyCardEffects(card)
* checkEndCondition()

---

## 🎮 **6. Required Game Flow**

1. Initialize deck using all card classes.
2. Assign rule modules automatically.
3. Shuffle and distribute hands.
4. Players alternate turns.
5. Played card updates play pile total.
6. If total > 99 → game ends.

---

## 🧪 **7. Testing Requirements**

The AI must generate code that supports:

* Injecting alternate rule sets without modifying core files.
* Creating a variant like:

  * Aces add 15 instead of 1
  * Kings set total to 99
  * Jacks reverse the turn order

---

## 🎉 **End Result**

The AI should output a complete, well‑organized codebase that:

* Uses modern ES6 classes
* Is modular and rule‑driven
* Allows easy extension of card types
* Allows alternate game rules by editing only ruleSelector.js

This markdown prompt should guide the AI to generate the most optimal, extensible, and cleanly structured code possible.
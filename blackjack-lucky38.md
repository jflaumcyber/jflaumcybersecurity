# 🃏 Blackjack — Lucky 38 Casino

A fully functional GUI Blackjack game built with Python and Tkinter, developed as the final project for **Script Programming (ITD 2313)** at OSU-IT.

**Course:** ITD 2313 – Script Programming | **Instructor:** M. Schnell | **Date:** July 25, 2024

---

## Project Overview

Lucky 38 Casino Blackjack is a desktop card game that implements standard Blackjack rules with a graphical card display, real-time scoring, and session statistics tracking. The game is themed after the Lucky 38 Casino from *Fallout: New Vegas*.

---

## Features

- 🃏 Full visual card deck with PNG card images (4 suits × 13 cards)
- 🤖 Dealer AI that follows standard Blackjack rules (hits until 17+)
- 📊 Live score tracking for both player and dealer
- 🔄 New Game button to reset between rounds
- 📈 Session statistics tracked across all games:
  - Games played
  - Games won
  - Games lost
  - Number of 21s scored
- 🏆 Stats summary popup on exit (Game Over button)
- 🪟 Window title dynamically updates with current session stats

---

## How to Play

| Button | Action |
|---|---|
| **Hit** | Draw another card |
| **Stand** | End your turn; dealer plays out |
| **New Game** | Reset the table and deal a fresh hand |
| **Game Over** | Display session statistics and exit |

**Rules:**
- Aces count as 11, reduced to 1 if the hand would bust
- Face cards (Jack, Queen, King) are worth 10
- Dealer must hit until reaching 17 or higher
- Bust over 21 = loss

---

## Technical Details

**Language:** Python 3  
**GUI Framework:** Tkinter (built-in)  
**Deck:** Triple deck (3× 52 cards), shuffled with `random.shuffle()`

### Project Structure

```
blackjack/
├── blackjack.py       # Main game logic and GUI
└── cards/             # Card image assets (PNG/PPM)
    ├── 1_heart.png
    ├── jack_spade.png
    └── ... (52 cards × 4 suits)
```

### Key Functions

| Function | Description |
|---|---|
| `load_images()` | Loads all card images from the `cards/` directory |
| `_deal_card(frame)` | Pops a card off the deck, renders it in the UI, returns the card |
| `score_hand(hand)` | Calculates total hand value with Ace handling |
| `deal_player()` | Deals one card to the player and updates score |
| `deal_dealer()` | Runs dealer AI and determines the winner |
| `new_game()` | Resets frames, hands, and deals a fresh opening hand |
| `shuffle()` | Displays session stats in a popup, then exits |
| `update_window_title()` | Refreshes the title bar with live stats |

---

## Source Code

```python
def score_hand(hand):
    """Calculate hand total with Ace (1 or 11) handling."""
    score = 0
    ace = False
    for next_card in hand:
        card_value = next_card[0]
        if card_value == 1 and not ace:
            ace = True
            card_value = 11
        score += card_value
        if score > 21 and ace:
            score -= 10
            ace = False
    return score


def deal_dealer():
    """Run dealer AI: hit until 17+, then determine winner."""
    dealer_score = score_hand(dealer_hand)
    while 0 < dealer_score < 17:
        dealer_hand.append(_deal_card(dealer_card_frame))
        dealer_score = score_hand(dealer_hand)
        dealer_score_label.set(dealer_score)

    player_score = score_hand(player_hand)
    if player_score > 21:
        result_text.set("Dealer wins!")
    elif dealer_score > 21 or dealer_score < player_score:
        result_text.set("Player wins!")
    elif dealer_score > player_score:
        result_text.set("Dealer wins!")
    else:
        result_text.set("Draw!")
```

---

## Running the Game

**Requirements:** Python 3.x (Tkinter is included in the standard library)

```bash
# Clone or download the project
git clone https://github.com/YOUR_USERNAME/blackjack-lucky38.git
cd blackjack-lucky38

# Run the game
python blackjack.py
```

> Make sure the `cards/` folder with all card images is in the same directory as `blackjack.py`.

---

## Screenshots

| State | Description |
|---|---|
| Initial Game | Fresh deal showing player and dealer opening cards |
| Dealer Wins | Dealer hand displayed after standing |
| Player Wins | Player beats dealer or dealer busts |
| 21 | Blackjack hand achieved |
| Score Window | Session statistics popup on exit |

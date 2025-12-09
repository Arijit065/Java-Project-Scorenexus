# 🏏 ScoreNexus: Digital Cricket Scoreboard

**ScoreNexus** is a robust, Java-based desktop application designed to manage and track cricket matches in real-time. It features a modern Swing GUI, persistent database storage using SQLite, and data export capabilities.

![Java](https://img.shields.io/badge/Java-ED8B00?style=for-the-badge&logo=openjdk&logoColor=white)
![SQLite](https://img.shields.io/badge/SQLite-07405E?style=for-the-badge&logo=sqlite&logoColor=white)
![Swing](https://img.shields.io/badge/Swing-GUI-blue?style=for-the-badge)

## 📖 Project Overview

This application serves as a digital scorer for cricket matches (T20, ODI, Test). Unlike simple counters, ScoreNexus maintains detailed statistics for every player (strike rates, economy, boundaries) and logs ball-by-ball highlights. It utilizes a **Stack data structure** to implement a robust "Undo" feature and **JDBC** for data persistence.

## ✨ Key Features

* **Match Setup:** Configure teams, player names, match type (overs), and toss details.
* **Live Scoring Dashboard:**
    * Track runs, wickets, and extras (Wides, No-Balls, Leg Byes, Byes).
    * Auto-calculation of Run Rate, Target, and Overs.
    * Visual indication of current strikers and bowlers.
* **Deep Statistics:**
    * **Batting:** Runs, Balls Faced, 4s, 6s, and Live Strike Rate.
    * **Bowling:** Overs, Maidens, Runs Conceded, Wickets, and Economy.
* **Data Persistence:** Automatically saves match data, player stats, and highlights to a local SQLite database (`scorenexus_gamedata.db`).
* **Undo Functionality:** Implemented using a **Stack** to revert accidental scoring inputs, restoring the exact previous game state.
* **Export Data:** Generate CSV files of the match highlights with a single click.
* **Modern UI:** Built with Java Swing using the Nimbus Look and Feel and a custom color palette.

---

## 🏏 Inside the Cricket Module

The core functionality of ScoreNexus lies in its advanced cricket scoring engine (`CricketPanel.java`). It goes beyond simple counting by implementing real-world cricket rules and logic.

### 1. Game Logic & Rules Implemented
* **Automatic Over Counting:** The system tracks valid balls. Wides (WD) and No-Balls (NB) add runs but do not count as valid deliveries in the over count, keeping the math accurate.
* **Batsman Rotation:**
    * **Run-based Rotation:** Automatically swaps the striker and non-striker on odd runs (1, 3) and keeps them the same on even runs (0, 2, 4, 6).
    * **Over-End Rotation:** Automatically swaps batsmen at the end of every 6-ball over.
* **Innings Management:**
    * **Target Calculation:** When the first innings ends (either by All-out or Overs completion), the system automatically calculates the target (Score + 1) and initializes the second innings.
    * **Match Result:** Detects win conditions (Runs chased or All-out) and declares the winner (e.g., *"Team A won by 4 wickets"* or *"Team B won by 20 runs"*).
* **Extras Handling:**
    * **Wides & No-Balls:** Adds 1 run to the total and excludes the ball from the over count.
    * **Byes & Leg Byes:** Adds runs to the team total but **not** to the batsman's individual score, ensuring statistical accuracy.

### 2. Live Statistical Tracking
The application calculates complex statistics in real-time, updating the database with every ball:
* **Batting Stats:** Tracks Runs, Balls Faced, Fours, Sixes, and **Live Strike Rate (SR)** for both batsmen at the crease.
* **Bowling Stats:** Tracks Overs, Runs Conceded, Wickets, and Economy Rate.
* **Match Stats:** Tracks Current Run Rate (CRR) and Required Runs (in the chase).

---

## 🛠️ Technical Implementation

### Tech Stack
* **Language:** Java (JDK 17+) - Utilizes modern features like `Records` and Text Blocks.
* **GUI:** Java Swing (JFrame, JPanel, CardLayout).
* **Database:** SQLite (via JDBC).
* **Build:** Standard Java Project structure.

### Key Concepts & Data Structures
* **OOP Principles:** Encapsulation using `MatchData` (Records) and `PlayerStats` models.
* **Stack (LIFO):** Used in `CricketPanel.java` to manage the `historyStack` for the Undo feature.
* **HashMap:** Used to map player names to their respective `PlayerStats` objects for O(1) retrieval during scoring.
* **JDBC:** Uses `PreparedStatement` and `ResultSet` to handle SQL injection-safe database transactions.

### Code Structure
| Class | Responsibility |
| :--- | :--- |
| **`CricketSetupPanel`** | Collects match config (Teams, Toss, Overs) and validates inputs. |
| **`CricketPanel`** | The "Brain" of the app. Handles state (score/wickets), UI updates, and business logic. |
| **`MatchData`** | A Java Record that acts as an immutable data carrier for match settings. |
| **`PlayerStats`** | A model class storing individual stats (runs, balls, wickets, etc.). |
| **`MatchState`** | A Record used specifically for the **Stack-based Undo feature** to snapshot the game state. |

## 📂 Database Schema

The application automatically generates the following tables on the first run:
1.  **`matches`**: Stores match meta-data (Teams, Venue, Result).
2.  **`players`**: Global registry of unique player names.
3.  **`match_stats`**: Relational table linking players to matches with specific performance data.
4.  **`highlights`**: Stores the ball-by-ball commentary events.

## 🚀 How to Run

### Prerequisites
1.  Java Development Kit (JDK) installed (version 17 or higher recommended).
2.  The SQLite JDBC Driver (`sqlite-jdbc-*.jar`) placed in the `lib` folder.

### Installation
1.  Clone the repository:
    ```bash
    git clone [https://github.com/yourusername/ScoreNexus.git](https://github.com/yourusername/ScoreNexus.git)
    ```
2.  Navigate to the project directory.
3.  Compile the source code:
    ```bash
    javac -d bin -cp "lib/*:src" src/com/scorenexus/main/ScoreboardManager.java
    ```
4.  Run the application:
    ```bash
    java -cp "bin:lib/*" com.scorenexus.main.ScoreboardManager
    ```
    
## 👤 Author

**Arijit Mandal**

---
*This project was developed to demonstrate proficiency in Java Application Development and Database Management.*

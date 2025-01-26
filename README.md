<b>Hey you</b>,

I’m a Software Developer who loves tackling challenges head-on. I’m all about breaking down complex problems into bite-sized data and UI components. Creating Lo-Fi prototypes and turning them into flawless applications? That’s my jam. 

<br>
<b>What I've been up to</b>
<br></br>
⚙️ Currently maintaining older applications, as well as building up <i>Mastermind</i> - a word puzzle app. 
<br><br>
🎓 '23 Grad @Columbia University in the City of New York 
<br>
<br>
<b>More about me</b>
<br></br>

🌱 I’m currently working on my <b>Angular</b> skills, and studying to take my AWS Developer certification.

💬 Ask me anything about: UI Design, career development, software engineering, Lo-Fi Prototyping. 

📫 How to reach me: - LinkedIn: [Mariana Maher](https://linkedin.com/in/marianamaher/), - Mail: [Send me an e-mail!](mailto:mariana.maherr@gmail.com)

<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Connections Game</title>
    <style>
      body {
        font-family: "Arial", sans-serif;
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        height: 100vh;
        margin: 0;
      }
      .board {
        display: grid;
        grid-template-columns: repeat(4, 150px);
        gap: 9px;
        margin-bottom: 20px;
      }
      .cell {
        width: 150px;
        height: 50px;
        font-size: 1em;
        text-align: center;
        line-height: 50px;
        vertical-align: middle;
        border: 2px solid #c5c5c5;
        border-radius: 5px;
        cursor: pointer;
        background-color: #fff;
        transition: background-color 0.3s;
        color: rgb(114, 114, 114);
      }
      .cell:hover{
        background: linear-gradient(132deg, rgb(255, 206, 236) 0.00%, rgba(161, 159, 245, 0.897) 100.00%);
        border: none;
        box-shadow: 0px 3px 5px 0px rgba(156, 155, 155, 0.75);
        color: rgb(6, 6, 58);
      }
      #foundConnections {
        margin-top: 10px;
        background: linear-gradient(132deg, rgba(255, 206, 236, 0.719) 0.00%, rgba(160, 159, 245, 0.664) 100.00%);
        border-radius: 5px;
        padding-bottom: 20px;
        text-align: center;
        display: flex;
        flex-direction: column;
        animation: slideDown 5s ease-in-out;
        
      }
      #foundConnections p, #foundConnections span {
        display: inline-block;
        text-align: center;
        margin-right: 5px; 
        margin-bottom: -2px;
        color: rgb(6, 6, 58);
        padding-left: 7px;
        
      }

      h2 {
        color: rgb(7, 7, 68);
      }

      h3{
        font-size: 14px;
        color: grey;
        font-weight: 100;
      }
      .remainingMistakes{
        color: rgb(15, 15, 95);
        text-align: center;
      }
      @keyframes slideDown {
        0% {
          transform: translateY(-100%);
        }
        100% {
          transform: translateY(0);
        }
      }
      .incorrectContainer{
        margin-bottom: 10px;
        color: rgb(197, 80, 191);
       
      }
      #startGameButton{
        margin-top: 10px;
        padding: 10px;
        border-radius: 5px;
        color: rgb(6, 6, 58);
      }
      #startGameButton:hover{
        background-color: rgba(6, 6, 71, 0.582);
        color: white;
        cursor: pointer;
      }
    </style>
  </head>
  <body>
    <h2>Create four groups of four that share a common thread about me.</h2>
    <div class="board" id="board"></div>
    <div class="incorrectContainer"></div>
    <div class="remainingMistakes">Remaining mistakes: <span id="trials">4</span></div>
    <div id="foundConnections"></div>
    <script>
      let remainingConnections = [
        ["dccc", "uneal", "columbia", "ucla"],
        ["taylor swift", "evermore", "lover", "reputation"],
        ["al", "ny", "pa", "nj"],
        ["snake", "coffee", "shield", "triangle"],
      ];

      let foundConnections = [];
      let revealed = false;
      let trials = 4;

      // Hardcoded shuffled board
      let shuffledValues = [
        "dccc",
        "ny",
        "coffee",
        "taylor swift",
        "shield",
        "uneal",
        "snake",
        "lover",
        "pa",
        "triangle",
        "reputation",
        "nj",
        "columbia",
        "evermore",
        "al",
        "ucla",
      ];

      let categoryNames = {
        dccc: "Universities I took classes at:",
        uneal: "Universities took classes at:",
        columbia: "Universities I took classes at:",
        ucla: "Universities I took classes at:",
        "taylor swift": "My favorite TS albums:",
        evermore: "My favorite TS albums:",
        lover: "My favorite TS albums:",
        reputation: "My favorite TS albums:",
        al: "States I’ve lived in:",
        ny: "States I’ve lived in:",
        pa: "States I’ve lived in:",
        nj: "States I’ve lived in:",
        snake: "Symbols of my tech stack:",
        coffee: "Symbols of my tech stack:",
        shield: "Symbols of my tech stack:",
        triangle: "Symbols of my tech stack:",
      };

      function renderBoard(values) {
        const boardElement = document.getElementById("board");
        boardElement.innerHTML = "";
        const resultsContainer = document.getElementById("foundConnections");

        for (let i = 0; i < values.length; i++) {
          const cell = document.createElement("div");
          cell.classList.add("cell");
          cell.onclick = () => cellClick(i);
          cell.innerText = values[i];
          boardElement.appendChild(cell);
        }
      }

      function startGame() {
        revealed = false;
        document.getElementById("trials").innerText = trials;
        document.getElementById("foundConnections").style.backfaceVisibility =
          "hiddenn";

        if (remainingConnections.length === 0 || trials <= 0) {
          // Game completed or no more trials
          renderBoard([]);
          document.querySelector(".remainingMistakes").remove();
          alert("Game Over. Thank you for playing!");
          const startGameButton = document.createElement("button");
          startGameButton.innerText = "Play Again";
          startGameButton.id = "startGameButton";
          startGameButton.onclick = () => {
            location.reload();
          };
          return document.body.appendChild(startGameButton);
        }

        renderBoard(shuffledValues);
      }

      function cellClick(index) {
        const clickedCell = document.getElementById("board").children[index];

        if (!revealed && foundConnections.length < 4) {
          if (foundConnections.includes(clickedCell.innerText)) {
            // Deselect cell
            clickedCell.style.backgroundColor = "#fff";
            foundConnections = foundConnections.filter(
              (cell) => cell !== clickedCell.innerText
            );
          } else {
            // Select cell
            clickedCell.style.backgroundColor = "#dddddd"; // Light grey when selected
            clickedCell.style.borderColor = "#afafaf";
          
            foundConnections.push(clickedCell.innerText);

            if (foundConnections.length === 4) {
              checkConnection();
            }
          }
        }
      }

      function checkConnection() {
        revealed = true;
        const boardElement = document.getElementById("board");
        const foundConnectionsElement =
          document.getElementById("foundConnections");

        if (foundConnections.every((cell) => isCorrectConnection(cell))) {
          // Correct connection
          const categoryName = getCategoryName(foundConnections[0]);
          if (categoryName) {
            // Add category name when a connection is found
            const categoryHeading = document.createElement("p");
            categoryHeading.innerText = categoryName;
            categoryHeading.style.fontWeight = "bold";
            categoryHeading.style.marginBottom = "-10px";
            foundConnectionsElement.appendChild(categoryHeading);
          }

          // Remove found connections from the shuffled board
          shuffledValues = shuffledValues.filter(
            (value) => !foundConnections.includes(value)
          );

          // Add to found connections list
          updateFoundConnectionsList(foundConnections);

          // Remove found connections from remaining connections
          remainingConnections = remainingConnections.filter((group) => {
            return !foundConnections.some((cell) => group.includes(cell));
          });

          setTimeout(() => {
            foundConnections = [];
            startGame();
          }, 1000);
        } else {
          // Incorrect connection
          trials--;
          document.getElementById("trials").innerText = trials;

          const message = document.createElement("p");
          message.innerText = "Incorrect connection.";
          message.style.textAlign = "center";
          message.style.fontWeight = "bold";
          const incorrectContainer = document.querySelector(".incorrectContainer");
          incorrectContainer.appendChild(message);

          setTimeout(() => {
            // Reset cell backgrounds
            document.querySelectorAll(".cell").forEach((cell) => {
              cell.style.backgroundColor = "#fff";
            });

            revealed = false;
            foundConnections = [];
            incorrectContainer.removeChild(message);
            startGame();
          }, 2000);
        }
      }

      function isCorrectConnection(cell) {
        return foundConnections.every((c) =>
          remainingConnections.some(
            (group) => group.includes(cell) && group.includes(c)
          )
        );
      }

      function getCategoryName(cell) {
        return categoryNames[cell] || null;
      }

      function updateFoundConnectionsList(connections) {
        const foundConnectionsElement =
          document.getElementById("foundConnections");

        const list = document.createElement("span");
        connections.forEach((connection) => {
          const listItem = document.createElement("p");
          listItem.innerText = connection + " ◦ ";
          list.appendChild(listItem);
        });
        foundConnectionsElement.appendChild(list);
      }

      // Initial game start
      startGame();
    </script>
  </body>
</html>


<!---
marianamaher/marianamaher is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->




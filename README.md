# sommerss.github.io

<!DOCTYPE html>
<html>




  <body>

    <div id="score">0</div>
    <canvas id="snakeboard" width="500" height="500"></canvas>

    <style>
      #snakeboard {
        position: absolute;
        top: 50%;
        left: 50%;
        transform: translate(-50%, -50%);
      }
      #score {
        text-align: center;
        font-size: 75px;
      }
    </style>
  </body>

  <script>
    const board_border = 'black';
    const board_background = "white";
    const snake_col = 'lightblue';
    const snake_border = 'darkblue';
    
    let snake = [
      {x: 200, y: 200},
      {x: 190, y: 200},
      {x: 180, y: 200},
      {x: 170, y: 200},
      {x: 160, y: 200}
    ]

    let score = 0;
    // True if changing direction
    let changing_direction = false;
    // Horizontal velocity
    let food_x;
    let food_y;
    let dx = 10;
    // Vertical velocity
    let dy = 0;
    
    
    // Get the canvas element
    const snakeboard = document.getElementById("snakeboard");
    // Return a two dimensional drawing context
    const snakeboard_ctx = snakeboard.getContext("2d");
    // Start game
    main();

    gen_food();

    document.addEventListener("keydown", change_direction);
    
    // main function called repeatedly to keep the game running
    function main() {

        if (has_game_ended()) return;

        changing_direction = false;
        setTimeout(function onTick() {
        clear_board();
        drawFood();
        move_snake();
        drawSnake();
        // Repeat
        main();
      }, 100)
    }
    
    // draw a border around the canvas
    function clear_board() {
      //  Select the colour to fill the drawing
      snakeboard_ctx.fillStyle = board_background;
      //  Select the colour for the border of the canvas
      snakeboard_ctx.strokestyle = board_border;
      // Draw a "filled" rectangle to cover the entire canvas
      snakeboard_ctx.fillRect(0, 0, snakeboard.width, snakeboard.height);
      // Draw a "border" around the entire canvas
      snakeboard_ctx.strokeRect(0, 0, snakeboard.width, snakeboard.height);
    }
    
    // Draw the snake on the canvas
    function drawSnake() {
      // Draw each part
      snake.forEach(drawSnakePart)
    }

    function drawFood() {
      snakeboard_ctx.fillStyle = 'lightgreen';
      snakeboard_ctx.strokestyle = 'darkgreen';
      snakeboard_ctx.fillRect(food_x, food_y, 10, 10);
      snakeboard_ctx.strokeRect(food_x, food_y, 10, 10);
    }
    
    // Draw one snake part
    function drawSnakePart(snakePart) {

      // Set the colour of the snake part
      snakeboard_ctx.fillStyle = snake_col;
      // Set the border colour of the snake part
      snakeboard_ctx.strokestyle = snake_border;
      // Draw a "filled" rectangle to represent the snake part at the coordinates
      // the part is located
      snakeboard_ctx.fillRect(snakePart.x, snakePart.y, 10, 10);
      // Draw a border around the snake part
      snakeboard_ctx.strokeRect(snakePart.x, snakePart.y, 10, 10);
    }

    function has_game_ended() {
      for (let i = 4; i < snake.length; i++) {
        if (snake[i].x === snake[0].x && snake[i].y === snake[0].y) return true
      }
      const hitLeftWall = snake[0].x < 0;
      const hitRightWall = snake[0].x > snakeboard.width - 10;
      const hitToptWall = snake[0].y < 0;
      const hitBottomWall = snake[0].y > snakeboard.height - 10;
      return hitLeftWall || hitRightWall || hitToptWall || hitBottomWall
      if (hitLeftWall || hitRightWall || hitTopWall || hitBottomWall) {
    console.log("game over!");
    return true;
  }

  return false;
}
    
    
    

    function random_food(min, max) {
      return Math.round((Math.random() * (max-min) + min) / 10) * 10;
    }

    function gen_food() {
      // Generate a random number the food x-coordinate
      food_x = random_food(0, snakeboard.width - 10);
      // Generate a random number for the food y-coordinate
      food_y = random_food(0, snakeboard.height - 10);
      // if the new food location is where the snake currently is, generate a new food location
      snake.forEach(function has_snake_eaten_food(part) {
        const has_eaten = part.x == food_x && part.y == food_y;
        if (has_eaten) gen_food();
      });
    }

    function change_direction(event) {
  const LEFT_KEY = 37;
  const RIGHT_KEY = 39;
  const UP_KEY = 38;
  const DOWN_KEY = 40;
  const A_KEY = 56;

  // Prevent the snake from reversing
  if (changing_direction) return;
  changing_direction = true;

  const keyPressed = event.keyCode;

  if (keyPressed === LEFT_KEY && dx !== 10) {
    dx = -10;
    dy = 0;
  } else if (keyPressed === UP_KEY && dy !== 10) {
    dx = 0;
    dy = -10;
  } else if (keyPressed === RIGHT_KEY && dx !== -10) {
    dx = 10;
    dy = 0;
  } else if (keyPressed === DOWN_KEY && dy !== -10) {
    dx = 0;
    dy = 10;
  } else if (keyPressed === A_KEY) {
    clear_board();
  }
}


    function move_snake() {
      // Create the new Snake's head
      const head = {x: snake[0].x + dx, y: snake[0].y + dy};
      // Add the new head to the beginning of snake body
      snake.unshift(head);
      const has_eaten_food = snake[0].x === food_x && snake[0].y === food_y;
      if (has_eaten_food) {
        // Increase score
        score += 1;
        // Display score on screen
        document.getElementById('score').innerHTML = score;
        document.getElementById('snakescore').value = score; // up
        // Generate new food location
        gen_food();
      } else {
        // Remove the last part of snake body
        snake.pop();
      }}

    
    </script>
<form action="javascript:login_user()">
    <p><label>
        name:
        <input type="text" name="name" id="name" required>
    </label></p>
    <p><label> 
        uid:
        <input type="text" name="uid" id="uid" required>
    </label></p>
    <p><label>
        snake score:
        <input type="number" name="snake score" id="snakescore" required>
    </label></p>
    <p><button>log</button></p>
    <p id="message"></p>
</form>

<script>
    // URL for deployment
   
    var url = "https://nashcsp.duckdns.org"; 
// Comment out next line for local testing

// Authenticate endpoint
const login_url = url + '/api/snake/create';

function login_user(){
   // Set body to include login data
   const body = {
       name: document.getElementById("name").value,
       uid: document.getElementById("uid").value,
       snakescore: parseInt(document.getElementById("snakescore").value) // assuming snake score is a number
   };
   // Set Headers to support cross origin
   const requestOptions = {
       method: 'POST',
       mode: 'cors', // no-cors, *cors, same-origin
       cache: 'no-cache', // *default, no-cache, reload, force-cache, only-if-cached
       // credentials: 'include', // include, *same-origin, omit
       body: JSON.stringify(body),
       headers: {
           "content-type": "application/json",
       },
   };

   // Fetch all users to check for duplicates
   fetch(url + '/api/snake/users')
   .then(response => {
       // trap error response from Web API
       if (response.status !== 200) {
           const message = 'Error fetching user data: ' + response.status + " " + response.statusText;
           document.getElementById("message").innerHTML = message;
           return;
       }
       // Valid response will contain json data
       response.json().then(data => {
           if (data.error) {
               const message = data.error;
               document.getElementById("message").innerHTML = message;
               return;
           }

           // Check for duplicate uid
           const userExists = data.find(user => user.uid === body.uid);
           if (userExists) {
               const message = 'User with the same id already exists.';
               document.getElementById("message").innerHTML = message;
               return;
           }

           // Fetch JWT if no duplicates found
           fetch(login_url, requestOptions)
           .then(response => {
               // trap error response from Web API
               if (response.status !== 200) {
                   const message = 'Login error: ' + response.status + " " + response.statusText;
                   document.getElementById("message").innerHTML = message;
                   localStorage.removeItem("uid");
                   localStorage.removeItem("visitor");
                   return;
               }
               // Valid response will contain json data
               response.json().then(data => {
                       if (data.error) {
                           const message = data.error;
                           document.getElementById("message").innerHTML = message;
                           localStorage.removeItem("uid");
                           localStorage.removeItem("visitor");
                           return;
                       }
                       const message = 'Login success: ' + data.name;
                       document.getElementById("message").innerHTML = message;
                       localStorage.setItem("uid", data.uid);
                       localStorage.setItem("visitor", data.name);
               })
           });

       });
   });
}


</script>

 
 
 
  
</body>


</html>

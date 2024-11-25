# Overview

This code implements a simple Snake game on an Arduino using an OLED display and an analog joystick (referred to as "STICKBOX" in the code) for input. The snake moves around the screen, eats food, grows, and the game ends if the snake crashes into the walls or itself. This code is not mine; I am simply documenting [this pre-existing project](https://github.com/Elecrow-RD/short_eletronics/blob/main/snake/snake.ino) from [this website](https://www.elecrow.com/blog/creating-the-snake-game-using-arduino.html).

Here is a detailed breakdown of the code:

### Libraries:
- `Wire.h`: This is for I2C communication, which is required for communicating with the OLED display.
- `Adafruit_GFX.h`: A graphics library that provides common drawing functions like `drawRect`, `drawBitmap`, and `print` for OLED displays.
- `Adafruit_SSD1306.h`: A library for controlling the SSD1306-based OLED displays.

### OLED Object:
```cpp
Adafruit_SSD1306 oled(128, 64, &Wire, -1);
```
This initializes the OLED display with a resolution of 128x64 pixels, using I2C communication (`&Wire`), and no reset pin (`-1`).

### Direction Constants:
```cpp
#define RIGHT 0
#define LEFT  1
#define UP    2
#define DOWN  3
```
These constants represent the four directions the snake can move: right, left, up, and down.

### Joystick Pin Definitions:
```cpp
#define pinX  A0
#define pinY  A1
```
The joystick's X and Y axes are connected to analog pins A0 and A1. These will be read to determine the direction of the snake.

### Game Variables:
- `valueX`, `valueY`: Variables to hold the joystick's X and Y axis values.
- `keyValue`: A variable for storing the joystick input state (though it seems unused in the code).
  
Snake game variables:
- `snake_head_x`, `snake_head_y`: Coordinates of the snake's head.
- `x[]`, `y[]`: Arrays to hold the coordinates of each part of the snake's body.
- `snake_len`: Length of the snake.
- `snake_dir`: The current direction the snake is moving in.
- `food_x`, `food_y`: Coordinates for the food.
- `food_eaten`: A boolean indicating whether the food has been eaten and needs to be regenerated.
- `game_over`: A boolean to track whether the game has ended.
- `score`, `level`, `snake_speed`: Variables for the score, current game level, and speed of the snake.

### Functions:

1. **keyScan()**:
   - Reads the joystick's X and Y values.
   - Determines the direction of the snake based on joystick movement:
     - If X is small, it moves the snake down.
     - If X is large, it moves the snake up.
     - If Y is small, it moves the snake left.
     - If Y is large, it moves the snake right.
   - The code uses a debounce-like approach (`keyUp` flag) to prevent multiple direction changes within a short time.

2. **draw_snake()**:
   - Draws a 4x4 pixel block representing a part of the snake at the given `x` and `y` coordinates using `drawBitmap()`.

3. **show_score()**:
   - Displays the current score or level at the specified `x` and `y` positions on the screen.

4. **screen()**:
   - Clears the display and redraws the game screen:
     - Draws a border rectangle around the screen.
     - Displays the level and score.
     - Draws the snake and the food.

5. **draw_food()**:
   - Generates a random position for the food, ensuring it doesn't overlap with the snake's body.
   - Once the food is eaten, it marks `food_eaten` as `true` to allow food regeneration.

6. **snake_move()**:
   - Moves the snake in the current direction (`RIGHT`, `LEFT`, `UP`, `DOWN`).
   - If the snake eats food (i.e., its head reaches the food's position), it grows by increasing `snake_len` and the score increases. The game level increases every 5 points, and the snake's speed decreases with each level.
   - Moves each part of the snake’s body (from tail to head) to simulate the movement of the snake.
   - Calls `check_snake_die()` to check if the snake collides with the wall or itself.

7. **draw_game_over()**:
   - Clears the screen and displays "GAME OVER" with the final level and score.

8. **check_snake_die()**:
   - Checks if the snake collides with the wall:
     - The snake dies if its head goes out of bounds (left, right, top, or bottom).
   - Checks if the snake collides with itself:
     - The snake dies if its head's coordinates match any of the coordinates of its body.

### Setup and Loop:

- **setup()**:
  - Initializes the OLED display.
  - Seeds the random number generator using an analog input (pin 3), which is used to generate random food positions.

- **loop()**:
  - Main game loop:
    - If the game is over, it displays the game over screen.
    - Otherwise, it reads the joystick input, moves the snake, generates food if needed, and updates the screen.
    - The `delay(snake_speed)` controls the game speed.

### Key Concepts:
- **Snake Movement**: The snake’s movement is controlled by reading the joystick and updating the snake's position.
- **Collision Detection**: The game ends if the snake collides with the screen border or itself.
- **Food Mechanics**: The snake grows when it eats food, and the food regenerates at random positions on the screen.
- **Difficulty Scaling**: As the score increases, the game gets harder by increasing the speed (`snake_speed`) and level.

### Summary:
This code implements a functional Snake game on an Arduino using an OLED display. The player controls the snake with an analog joystick, the snake grows when it eats food, and the game ends if the snake crashes into the walls or itself. The game also tracks and displays the player's score and level, with the speed increasing as the player progresses.
# FiniteStateMachine

PROJ REQ and yeah

# CIRCUIT SCHEMATIC

I2C PULLUP RESISTORS ON SDA, SCL (as bits are active low, pulling the line down). THESE ARE INTERNALLY CONNECTED.

# STATE MACHINE DESIGN
<img src="https://i.ibb.co/5ncZTbp/statemachine.jpg" alt="statemachine" border="0">

This is a Moore state machine design, as the state machine action depends only on the active state.   
Typically, this is just displaying the current state's screen / animation.  
* The __START__ and __LOAD__ states are progressed by pressing the start button.  
* The __RUNNING__ state toggles with the __PAUSED__ state by pressing the pause button.  
* The game engine runs in one state (see above right), and the pause state is a "pause" text overlay over the current game frame / loop. 
* The __RUNNING__ state transitions to either the __SUCCESS__ or __GAMEOVER__ state, depending on the outcome of the game.  
* Pressing start in either of these states transitions back to the __START__ state.  

The corresponding state actions are as follows:
* `displayStartScreen()` - the screen the player is first met with. Introduces the game.  
* `displayLoadScreen()` - prompts the player to calibrate / reset the potentiometer to reset the starting position of the car. This is required as the knob is a physical mechanism (superceeding any software reset done).  
* `runGame()` - run the game engine, until the player wins or loses.  
* `displayPaused()` - hold the game's current frame, until the player re-presses pause.  
* `displayWinScreen()` - reward the player with a victory animation.  
* `displayLoseScreen()` - consolidate the player with a defeat screen.  

Transitions between states depend on these 'events':
* `Start_Pressed` - set when the 'start' button press triggers an external interrupt (negative edge detected), and in the interrupt service routine called.  
* `Pause_Pressed` - similarly as above.  
* `Collision_Detected` - set when the player's car collides with the road edges or with any oncoming car in the game.  
* `Win_Condition_Met` - set when the player successfully passes the required number of cars (in this specific instance).  

Note: As the start and pause button 'pressed flag' latches on, these need to be reset when transitioning to a state checking for these button presses (to further transition). This prevents improper state transitioning - for example, if the start button was pressed during the game running state, though it would seem to have no immediate effect, the result (if not reset) would be immediately restarting the game when it was won or lost. The gameover or game won screen would effectively be skipped.

# PHYSICAL CIRCUIT
<img src="https://i.ibb.co/nPXnSB2/breadboardcircuit.jpg" alt="breadboardcircuit" border="0">


# SOURCE CODE

* EXPLAIN RUNGAME
* EXPLAIN STATEMACHINE

# IMAGES AND REFERENCES
<img src="https://i.ibb.co/YXXHpGh/mazda-323f-1993-2.jpg" alt="mazda-323f-1993-2" border="0"><img src="https://i.ibb.co/k1RwF41/mazda-323f-1993-tiny.jpg" alt="mazda-323f-1993-tiny" border="0">  
https://getoutlines.com/blueprints/3143/1998-mazda-323f-lantis-f-hatchback-blueprints

<img src="https://i.ibb.co/chX98p3/render-97.png" alt="render-97" border="0"><img src="https://i.ibb.co/y45htKp/dxracer.png" alt="dxracer" border="0"><img src="https://i.ibb.co/bv4p8rv/dxracer-RIP.png" alt="dxracer-RIP" border="0">  
https://gamebanana.com/sprays/21984

<img src="https://i.ibb.co/7nwqyK3/tropy-award.jpg" alt="tropy-award" border="0"><img src="https://i.ibb.co/BKYy9FG/tropy-award-pixel.jpg" alt="tropy-award-pixel" border="0">
https://bornzilla.eventsmart.com/events/meet-greet-soulzepi/tropy-award/ 

<img src="https://i.ibb.co/V2H6Txn/lcdassistant.jpg" alt="lcdassistant" border="0">
LCD Assistant: http://en.radzio.dxp.pl/bitmap_converter/

SSD1306 Sample Code.

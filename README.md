# project3
public class Character {
	
	EZImage spriteSheet;
	
	
	int x = 0;				// Position of Sprite
	int y = 0;
	EZImage[] Wall;       //wall array
	int spriteWidth;		// Width of each sprite
	int spriteHeight;		// Height of each sprite
	int direction = 0;		// Direction character is walking in
	int walkSequence = 0;	// Walk sequence counter
	int cycleSteps;			// Number of steps before cycling to next animation step
	int counter = 0;	        // Cycle counter
	int wallCount = 0;    //# of walls in main program
	

	Character(String imgFile, int startX, int startY, int width, int height, int steps, EZImage walls[], int numwalls) {
		x = startX;					// position of the sprite character on the screen
		y = startY;
		Wall = walls;
		wallCount= numwalls;     //saves # of walls main program
		spriteWidth = width;		// Width of the sprite character
		spriteHeight = height;		// Height of the sprite character
		cycleSteps = steps;			// How many pixel movement steps to move before changing the sprite graphic
		spriteSheet = EZ.addImage(imgFile, x, y); //adds the sprite image sheet
		setImagePosition();
	}
	
	boolean checkCollision(int xstep, int ystep) {   //checks to see if the next step will make blink touch the wall
		for(int i=0; i<wallCount; i++) {
			if(Wall[i].isPointInElement(x+xstep, y+ystep)) {
				return true;
			}
		}
		return false;
	}

	private void setImagePosition() {		// Move the entire sprite sheet
		spriteSheet.translateTo(x, y);
		// Show only a portion of the sprite sheet.
		// Portion is determined by setFocus which takes 4 parameters:
		// The 1st two numbers is the top left hand corner of the focus region.
		// The 2nd two numbers is the bottom right hand corner of the focus region.
		spriteSheet.setFocus(walkSequence * spriteWidth, direction,
				walkSequence * spriteWidth + spriteWidth, direction + spriteHeight);
	}

	public void moveDown(int stepSize) { //move down function
		y = y + stepSize;

		direction = 0;

		if ((counter % cycleSteps) == 0) { //cycles through 3 different pictures of the sprite sheet that reflect the character walking down
			walkSequence++;
			if (walkSequence > 3)
				walkSequence = 0;
		}
		counter++;
		setImagePosition();
	}
	
	public void moveLeft(int stepSize) { //same as above
		x = x - stepSize;
		direction = spriteHeight;

		if ((counter % cycleSteps) == 0) {
			walkSequence--;
			if (walkSequence < 0)
				walkSequence = 3;
		}
		counter++;
		setImagePosition();
	}

	public void moveRight(int stepSize) {
		x = x + stepSize;
		direction = spriteHeight * 2;
		
		if ((counter % cycleSteps) == 0) {
			walkSequence++;
			if (walkSequence > 3)
				walkSequence = 0;
		}
		counter++;

		setImagePosition();
	}

	public void moveUp(int stepSize) {
		y = y - stepSize;
		direction = spriteHeight * 3;

		if ((counter % cycleSteps) == 0) {
			walkSequence--;
			if (walkSequence < 0)
				walkSequence = 3;
		}
		setImagePosition();

		counter++;
	}

	 void go() {                            //controls for character
	if (EZInteraction.isKeyDown('w')) {
		if(!checkCollision(0,-8))        //checks for collision, if not the character can move
		moveUp(8);
		if(checkCollision(0,-8))  //if there will be a collision, the character cannot move
			moveUp(0);
	} else if (EZInteraction.isKeyDown('a')) {
		if(!checkCollision(-8,0))
		moveLeft(8);
		if(checkCollision(-8,0))
			moveLeft(0);
	} else if (EZInteraction.isKeyDown('s')) {
		if(!checkCollision(0,8)) 
		moveDown(8);
		if(checkCollision(0,8))
			moveDown(0);
	} else if (EZInteraction.isKeyDown('d')) {
		if(!checkCollision(8,0)) 
		moveRight(8);
		if(checkCollision(8,0))
		moveRight(0);

	}

}

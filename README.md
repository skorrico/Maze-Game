# Maze-Game
// Squirrel.java
/* this class was made to make the squirrel in the game to run away from
*/other classes are needed to test actual game
// Author: Sarah Orrico
// Date: May 5, 2015

package myPackage;

import myPackage.Interface.eMove;

public class Squirrel implements Interface {
	
	//class interface varaibles 

	// Current location of squirrel
	private int currentRow;
	private int currentCol;
	private char board[][];

	// Information about closest terrier
	private int closestRow;
	private int closestCol;
	private double closestDistance = 0.0;

	// Create a squirrel
	public Squirrel(int row, int Col, char board[][]) {
		this.currentRow = row;
		this.currentCol = Col;
		this.board = board;
	}

	// Getters for squirrel
	public int getCurrentRow() { return currentRow; }
	public int getCurrentCol() { return currentCol; }
	public int getClosestRow() { return closestRow; }
	public int getClosestCol() { return closestCol; }
	public double getClosestDist() { return closestDistance; }

	// Setters for squirrel
	public void setCurrentRow(int row) { this.currentRow = row; }
	public void setCurrentCol(int col) { this.currentCol = col; }
	
	// Move the squirrel
	public eMove move() {

		// Call method to find closest terrier
		findClosest1();
		// Call method to figure out move in opposite direction
		eMove e = run();
		// --- student code here ---

		// Call method to avoid going off the board
		if ((e == eMove.DOWN_LEFT || e == eMove.DOWN || e == eMove.DOWN_RIGHT)
				&& currentRow == board.length - 1)
			e = eMove.LEFT;
		if ((e == eMove.UP_LEFT || e == eMove.LEFT || e == eMove.DOWN_LEFT)
				&& currentCol == 0)
			e = eMove.UP;
		if ((e == eMove.UP_LEFT || e == eMove.UP || e == eMove.UP_RIGHT)
				&& currentRow == 0)
			e = eMove.RIGHT;
		if ((e == eMove.UP_RIGHT || e == eMove.RIGHT || e == eMove.DOWN_RIGHT)
				&& currentCol == board[0].length - 1)
			e = eMove.DOWN;
		// Return move
		return e;
		
		}

	// Method to find closest terrier
		private double findClosest1(){
			closestDistance = 88888;
			double minimum = Double.MAX_VALUE;
				closestRow = -1;
				closestCol = -1;

				for (int row = 0; row < board.length; row++) {
					for (int col = 0; col < board[0].length; col++) {
						if (board[row][col] == 'T') {

							// Found a squirrel
							double distance = computeDistance(row, currentRow, col, currentCol);
							if (distance < minimum) {

								// Store the closest
								minimum = distance;
								closestRow = row;
								closestCol = col;
								closestDistance = minimum;
							}
						}
					}
				}
				return closestDistance;
			}
	// Method to figure out move in opposite direction
		private eMove run(){
			eMove move;

			if (closestRow < currentRow) {
				if (closestCol < currentCol)
					move = eMove.DOWN_RIGHT;
				else if (closestCol == currentCol)
					move = eMove.DOWN;
				else
					move = eMove.DOWN_LEFT;
			} else if (closestRow == currentRow) {
				if (closestCol < currentCol)
					move = eMove.RIGHT;
				else
					move = eMove.LEFT;
			} else { // if (closestRow > currentRow
				if (closestCol < currentCol)
					move = eMove.UP_RIGHT;
				else if (closestCol == currentCol)
					move = eMove.UP;
				else
					move = eMove.UP_LEFT;
			}
			return move;
		}
    // Method to avoid going off the board
		

	// Compute euclidean distance
	private double computeDistance(int row0, int row1, int col0, int col1) {
		double rowDiff = Math.pow(row1 - row0, 2);
		double colDiff = Math.pow(col1 - col0, 2);
		return Math.sqrt(rowDiff + colDiff);
	}
}

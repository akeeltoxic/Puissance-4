Puissance 4

Ce projet implémente le jeu classique de Puissance 4 en Java, jouable sur le terminal. Vous pouvez jouer en mode joueur contre joueur ou joueur contre machine.

## Table des Matières
- [Nom et prénom](#auteur)
- [À propos](#à-propos)
- [Installation](#installation)
- [Utilisation](#utilisation)
- [Contribuer](#contribuer)
- [Licence](#licence)
- [Auteurs](#auteurs)
- [Remerciements](#remerciements)
## Nom et Prénom élève
Nom : SOUVALTZIS
Prénom: Goulam Farez Achil
## À propos

Le projet Puissance 4 est un jeu de stratégie classique où deux joueurs s'affrontent pour aligner quatre jetons de la même couleur horizontalement, verticalement ou en diagonale. Le jeu propose deux modes : joueur contre joueur et joueur contre machine.

## Installation

Pour installer et exécuter ce projet, suivez les étapes ci-dessous :

1. Clonez le dépôt :
    ```bash
    git clone https://github.com/votre-utilisateur/votre-projet.git
    ```

2. Naviguez dans le répertoire du projet :
    ```bash
    cd votre-projet
    ```

3. Compilez le projet :
    ```bash
    javac -d bin src/fr/achil/puissance4/*.java
    ```

4. Exécutez le projet :
    ```bash
    java -cp bin fr.achil.puissance4.Puissance4
    ```

## Utilisation

Après avoir lancé le jeu, choisissez le mode de jeu :
- Tapez `1` pour le mode Joueur contre Joueur.
- Tapez `2` pour le mode Joueur contre Machine.

Suivez les instructions affichées pour jouer votre tour en entrant le numéro de colonne souhaité.

## Code Source

```java
package fr.achil.puissance4;

import java.util.Scanner;
import java.util.Random;

public class Puissance4 {
	private static final int COLS= 7;
	private static final int ROWS = 6;
	private static final char EMPTY_SLOT ='.';
	private static final char PLAYER_ONE= 'X';
	private static final char PLAYER_TWO = 'O';
	private static char[][] board = new char[ROWS][COLS];
	private static Scanner scanner = new Scanner(System.in);
	private static Random random = new Random();
	
	public static void main(String[]args) {
		initializeBoard();
		printBoard();
		System.out.println("Choisissez le mode de jeu");
		System.out.println("1. Joueur contre joueur");
		System.out.println("2. Joueur contre machine");
		int mode = scanner.nextInt();
		if (mode == 1) {
			PlayervsPlayer();
		} else if (mode == 2) {
			playerVSMachine();
		}
	}
	
	private static void initializeBoard() {
		for (int i = 0; i < ROWS; i++) {
			for (int j = 0; j < COLS; j++) {
				board[i][j] = EMPTY_SLOT;
			}
		}
	}
	
	private static void printBoard() {
		for (int i = 0; i < ROWS; i++) {
			for (int j = 0; j < COLS; j++) {
				System.out.print(board[i][j] + "|");
			}
			System.out.println();
		}
	}
	
	private static void PlayervsPlayer() {
		boolean playerOneTurn = true;
		while (true) {
			if (playerOneTurn) {
				System.out.println("Joueur 1 (X) - choisissez une colonne (1-7): ");
			} else {
				System.out.println("Joueur 2 (O) - choisissez une colonne (1-7): ");
			}
			int col = scanner.nextInt() - 1;
			if (col < 0 || col >= COLS) {
				System.out.println("Colonne invalide. Réessayez.");
				continue;
			}
			if (!makeMove(col, playerOneTurn ? PLAYER_ONE : PLAYER_TWO)) {
				System.out.println("Colonne pleine. Réessayez.");
				continue;
			}
			printBoard();
			if (checkWin(playerOneTurn ? PLAYER_ONE : PLAYER_TWO)) {
				if (playerOneTurn) {
					System.out.println("Joueur 1 (X) gagne!");
				} else {
					System.out.println("Joueur 2 (O) gagne!");
				}
				break;
			}
			if (isBoardFull()) {
				System.out.println("Match nul!");
				break;
			}
			playerOneTurn = !playerOneTurn;
		}
	}
	
	private static void playerVSMachine() {
		boolean playerTurn = true;
		while (true) {
			if (playerTurn) {
				System.out.println("Joueur (X) - Choisissez une colonne (1-7): ");
				int col = scanner.nextInt() - 1;
				if (col < 0 || col >= COLS) {
					System.out.println("Colonne invalide. Réessayez.");
					continue;
				}
				if (!makeMove(col, PLAYER_ONE)) {
					System.out.println("Colonne pleine. Réessayez.");
					continue;
				}
			} else {
				int col = random.nextInt(COLS);
				while (!makeMove(col, PLAYER_TWO)) {
					col = random.nextInt(COLS);
				}
				System.out.println("Machine (O) a joué en colonne: " + (col + 1));
			}
			printBoard();
			if (checkWin(playerTurn ? PLAYER_ONE : PLAYER_TWO)) {
				if (playerTurn) {
					System.out.println("Joueur (X) gagne!");
				} else {
					System.out.println("Machine (O) gagne!");
				}
				break;
			}
			if (isBoardFull()) {
				System.out.println("Match nul!");
				break;
			}
			playerTurn = !playerTurn;
		}
	}
	
	private static boolean makeMove(int col, char player) {
		for (int i = ROWS - 1; i >= 0; i--) {
			if (board[i][col] == EMPTY_SLOT) {
				board[i][col] = player;
				return true;
			}
		}
		return false;
	}
	
	private static boolean checkWin(char player) {
		// check horizontal
		for (int i = 0; i < ROWS; i++) {
			for (int j = 0; j < COLS - 3; j++) {
				if (board[i][j] == player && board[i][j + 1] == player && board[i][j + 2] == player && board[i][j + 3] == player) {
					return true;
				}
			}
		}
		// check vertical
		for (int i = 0; i < ROWS - 3; i++) {
			for (int j = 0; j < COLS; j++) {
				if (board[i][j] == player && board[i + 1][j] == player && board[i + 2][j] == player && board[i + 3][j] == player) {
					return true;
				}
			}
		}
		// check diagonal de droite à gauche
		for (int i = 3; i < ROWS; i++) {
			for (int j = 0; j < COLS - 3; j++) {
				if (board[i][j] == player && board[i - 1][j + 1] == player && board[i - 2][j + 2] == player && board[i - 3][j + 3] == player) {
					return true;
				}
			}
		}
		// check diagonal de gauche à droite
		for (int i = 0; i < ROWS - 3; i++) {
			for (int j = 0; j < COLS - 3; j++) {
				if (board[i][j] == player && board[i + 1][j + 1] == player && board[i + 2][j + 2] == player && board[i + 3][j + 3] == player) {
					return true;
				}
			}
		}
		return false;
	}
	
	private static boolean isBoardFull() {
		for (int j = 0; j < COLS; j++) {
			if (board[0][j] == EMPTY_SLOT) {
				return false;
			}
		}
		return true;
	}
}

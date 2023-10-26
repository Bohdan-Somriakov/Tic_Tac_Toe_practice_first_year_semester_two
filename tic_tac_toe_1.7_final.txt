//#define _CRT_SECURE_NO_WARNINGS
#include <iostream>
#include <iomanip>
#include <string>
#include <regex>
//#include <windows.h>
using namespace std;
regex botXReg("\\s*bo?t?\\s*x\\s*"); //bot x b x
regex botOReg("\\s*bo?t?\\s*o\\s*"); //bot o b o
regex playerReg("\\s*pl?a?y?e?r?\\s*\\s*"); // player p
regex botXBotOReg("\\s*bo?t?\\s*x\\s*|\\s*bo?t?\\s*o\\s*"); //both bot x b x AND bot o b o 
regex botXBotORegPlayerReg("\\s*bo?t?\\s*x\\s*|\\s*bo?t?\\s*o\\s*|\\s*pl?a?y?e?r?\\s*\\s*");  // bot x b x AND bot o b o AND player p
regex botXBotORegPlayerRegStop("\\s*bo?t?\\s*x\\s*|\\s*bo?t?\\s*o\\s*|\\s*pl?a?y?e?r?\\s*\\s*|stop");  // bot x b x AND bot o b o AND player p


void displayArrViaPixels(char arr[][3], int size) {
	const int SIZE = 3;
	constexpr int CROSS_AND_CIRCLE_SIZE = 7;
	const string cross[CROSS_AND_CIRCLE_SIZE] = { "       ■"," x   x ■","  x x  ■","   x   ■","  x x  ■"," x   x ■","       ■" };
	const string circle[CROSS_AND_CIRCLE_SIZE] = { "       ■", "  ooo  ■"," o   o ■"," o   o ■"," o   o ■","  ooo  ■", "       ■" };
	const string space = "       ■";
	string crossCircle[CROSS_AND_CIRCLE_SIZE][SIZE];
	// cross[0] circle[0] space
	// cross[1] circle[1] space
	// cross[2] circle[2] space
	// cross[3] circle[3] space
	// cross[4] circle[4] space
	// cross[5] circle[5] space
	// cross[6] circle[6] space
	cout << "\n■■■■■■■■■■■■■■■■■■■■■■■■\n";
	for (int k = 0; k < size; k++) {
		for (int i = 0; i < size; i++) {
			if (arr[k][i] == 'x') {
				for (int j = 0; j < CROSS_AND_CIRCLE_SIZE; j++) {
					crossCircle[j][i] = cross[j];
				}
			}
			if (arr[k][i] == 'o') {
				for (int j = 0; j < CROSS_AND_CIRCLE_SIZE; j++) {
					crossCircle[j][i] = circle[j];
				}
			}
			if (arr[k][i] == 'n') {
				for (int j = 0; j < CROSS_AND_CIRCLE_SIZE; j++) {
					crossCircle[j][i] = space;
				}
			}
		}
		for (int i = 0; i < CROSS_AND_CIRCLE_SIZE; i++) {
			for (int j = 0; j < size; j++) {
				cout << crossCircle[i][j];
			}
			cout << endl;
		}
		cout << "■■■■■■■■■■■■■■■■■■■■■■■■\n";
	}
	cout << '\n';
}
void fillCheckArr(bool(&checkArr)[9], int input) {
	checkArr[input - 1] = true;
}
void checkIfElementExists(bool checkArr[9], int& input) {
	while (checkArr[input - 1] == true) {
		cout << "Wrong input. Please enter new input\n";
		cin >> input;
	}
}
//fill n
void fillArrWithN(char(&arr)[3][3], int size) {
	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			arr[i][j] = 'n';
		}
	}
}
// Display array
void displayArr(char arr[][3], int size) {
	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			cout << arr[i][j] << setw(3);
		}
		cout << '\n';
	}
}
int checkWin(char arr[][3], int turn, int size) {
	// 0 - game is still on
	// 1 - x wins
	// 2 - o wins
	// 3 - draw
	// turn 1 means turn o
	// turn 0 means turn x
	int countForOHorizontal = 0;
	int countForOVertical = 0;
	if (turn == 1) {
		for (int i = 0; i < size; i++) {
			for (int j = 0; j < size; j++) {
				if (arr[j][i] == 'o') {
					countForOVertical++;
				}
			}
			if (countForOVertical == 3) {
				return 2;
			}
			countForOVertical = 0;
		}
		for (int i = 0; i < size; i++) {
			for (int j = 0; j < size; j++) {
				if (arr[i][j] == 'o') {
					countForOHorizontal++;
				}
			}
			if (countForOHorizontal == 3) {
				return 2;
			}
			countForOHorizontal = 0;
		}
		if (arr[1][1] == 'o') {
			if (arr[0][0] == 'o' && arr[2][2] == 'o') {
				return 2;
			}
			if (arr[2][0] == 'o' && arr[0][2] == 'o') {
				return 2;
			}
		}
	}
	int countForXHorizontal = 0;
	int countForXVertical = 0;
	if (turn == 0) {
		for (int i = 0; i < size; i++) {
			for (int j = 0; j < size; j++) {
				if (arr[j][i] == 'x') {
					countForXVertical++;
				}
			}
			if (countForXVertical == 3) {
				return 1;
			}
			countForXVertical = 0;
		}
		for (int i = 0; i < size; i++) {
			for (int j = 0; j < size; j++) {
				if (arr[i][j] == 'x') {
					countForXHorizontal++;
				}
			}
			if (countForXHorizontal == 3) {
				return 1;
			}
			countForXHorizontal = 0;
		}
		if (arr[1][1] == 'x') {
			if (arr[0][0] == 'x' && arr[2][2] == 'x') {
				return 1;
			}
			if (arr[2][0] == 'x' && arr[0][2] == 'x') {
				return 1;
			}
		}
	}
	bool flag = true; //assume that the array is fully filled
	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			if ((arr[i][j] != 'x') && (arr[i][j] != 'o')) {
				flag = false;
			}
		}
	}
	if (flag == true) {
		return 3; //draw
	}
	return 0;
}
void alertResult(char arr[][3], int turn, int size) {
	if (checkWin(arr, turn, size) == 1) {
		cout << "X WON!\n";
	}
	if (checkWin(arr, turn, size) == 2) {
		cout << "O WON!\n";
	}
	if (checkWin(arr, turn, size) == 3) {
		cout << "DRAW!\n";
	}
}
//void findTwoCrossesInARow(int arr[3][3], int size) {
//
//}
void botEasy(int& input, bool(&checkArr)[9]) {
	cout << "easy\n";
	srand(time(nullptr));
	input = rand() % 9 + 1;
	while (checkArr[input - 1] == true) {
		input = rand() % 9 + 1;
	}
	// fillCheckArr(checkArr, input);
}
void CreateRowColumnPatternsBasedOnPriority(char(&patternArr)[3][3], int size, char priority) {
	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			if (patternArr[i][j] != 'n') {
				patternArr[i][j] = priority;
			}
		}
	}
}
int inputBasedOnRowColumnPattern(int pattern, int size, char arr[3][3]) noexcept {
	char target = 't';
	for (int i = 0; i < size; i++) {
		if (arr[pattern][i] == 'n') {
			arr[pattern][i] = target;
		}
	}
	int count = 0;
	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			count++;
			if (arr[i][j] == target) {
				return count; //input
			}
		}
	}
	cout << "SOMETHING WENT WRONG";
	return -1;
}
int findTwoSymbolsInARowBasedOnTurn(char arr[3][3], int size, char priority) {
	const int SIZE = 3;
	char rowPatterns[SIZE][SIZE]{
		{'n','?','?'},
		{'?','n','?'},
		{'?','?','n'},
	};
	CreateRowColumnPatternsBasedOnPriority(rowPatterns, size, priority);
	int count = 0;
	int findPattern = -1;
	for (int k = 0; k < size; k++) {
		for (int i = 0; i < SIZE; i++) {
			for (int j = 0; j < SIZE; j++) {
				//cout << arr[i][j] << ' ' << rowPatterns[k][j] << '\n';
				if (arr[i][j] == rowPatterns[k][j]) {
					count++;
				}
			}
			findPattern++;
			if (count == 3) {
				return inputBasedOnRowColumnPattern(findPattern, size, arr);
			}
			count = 0;
		}
		findPattern = -1;
	}
	return -1; // not found
}
int inputBasedOnColumnParttern(int pattern, int size, char arr[3][3]) noexcept {
	char target = 't';
	for (int i = 0; i < size; i++) {
		if (arr[i][pattern] == 'n') {
			arr[i][pattern] = target;
		}
	}
	int count = 0;
	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			count++;
			if (arr[i][j] == target) {
				return count; //input
			}
		}
	}
	cout << "SOMETHING WENT WRONG";
	return -1;
}
int findTwoSymbolsInAColumnBasedOnTurn(char arr[3][3], int size, char priority) {
	const int SIZE = 3;
	char columnPatterns[SIZE][SIZE]{
		{'n','?','?'},
		{'?','n','?'},
		{'?','?','n'},
	};
	CreateRowColumnPatternsBasedOnPriority(columnPatterns, size, priority);
	int count = 0;
	int findPattern = -1;
	for (int k = 0; k < size; k++) {
		for (int i = 0; i < SIZE; i++) {
			for (int j = 0; j < SIZE; j++) {
				if (arr[j][i] == columnPatterns[k][j]) {
					count++;
				}
			}
			findPattern++;
			if (count == 3) {
				return inputBasedOnColumnParttern(findPattern, size, arr);
			}
			count = 0;
		}
		findPattern = -1;
	}
	return -1; // not found
}
int findTwoSymbolsInADiagonalBasedOnTurn(char arr[3][3], char priority) {
	char searchFor = priority;
	if (arr[0][0] == searchFor && arr[1][1] == searchFor && arr[2][2] == 'n') {
		return 9;
	}
	else if (arr[0][0] == searchFor && arr[1][1] == 'n' && arr[2][2] == searchFor) {
		return 5;
	}
	else if (arr[0][0] == 'n' && arr[1][1] == searchFor && arr[2][2] == searchFor) {
		return 1;
	}
	else if (arr[0][2] == searchFor && arr[1][1] == searchFor && arr[2][0] == 'n') {
		return 7;
	}
	else if (arr[0][2] == searchFor && arr[1][1] == 'n' && arr[2][0] == searchFor) {
		return 5;
	}
	else if (arr[0][2] == 'n' && arr[1][1] == searchFor && arr[2][0] == searchFor) {
		return 3;
	}
	else {
		return -1; //not found
	}
}
void FindTwoCrosses(char arr[3][3], int size, int& input, string action) {
	char priority = '?';
	if (regex_match(action, botOReg)) {
		priority = 'o';
	}
	else if (regex_match(action, botXReg)) {
		priority = 'x';
	}
	int tempInput1 = findTwoSymbolsInARowBasedOnTurn(arr, size, priority);
	int tempInput2 = findTwoSymbolsInAColumnBasedOnTurn(arr, size, priority);
	int tempInput3 = findTwoSymbolsInADiagonalBasedOnTurn(arr, priority);
	if (tempInput1 != -1) {
		input = tempInput1;
		return;
	}
	else if (tempInput2 != -1) {
		input = tempInput2;
		return;
	}
	else if (tempInput3 != -1) {
		input = tempInput3;
		return;
	}
	if (regex_match(action, botOReg)) {
		priority = 'x';
	}
	else if (regex_match(action, botXReg)) {
		priority = 'o';
	}
	int tempInput4 = findTwoSymbolsInARowBasedOnTurn(arr, size, priority);
	int tempInput5 = findTwoSymbolsInAColumnBasedOnTurn(arr, size, priority);
	int tempInput6 = findTwoSymbolsInADiagonalBasedOnTurn(arr, priority);
	if (tempInput4 != -1) {
		input = tempInput4;
		return;
	}
	else if (tempInput5 != -1) {
		input = tempInput5;
		return;
	}
	else if (tempInput6 != -1) {
		input = tempInput6;
		return;
	}
	else {
		input = -1; // call botEasy or smth
	}
}
void botMedium(int& input, bool(&checkArr)[9], char arr[3][3], int size, string action) {
	FindTwoCrosses(arr, size, input, action);
	if (input == -1) { //if it did not find two symbols
		botEasy(input, checkArr);
	}
}
void botHardFirstMove(char arr[3][3], string action, int& input) {
	if (regex_match(action, botXReg)) {
		input = 7; //best first moves are the corner moves. let's choose 7
	}
	else if (regex_match(action, botOReg)) {
		if (arr[1][1] == 'n') {
			input = 5;
		}
		else {
			input = 7; //why not
		}
	}
}
void botHardSecondMove(char arr[3][3], string action, int& input) {
	if (regex_match(action, botXReg)) {
		if (arr[1][1] == 'o') {
			input = 3; //why not
		}
		if (arr[0][0] == 'o' || arr[0][1] == 'o' || arr[1][0] == 'o') {
			input = 9;
		}
		if (arr[2][1] == 'o' || arr[2][2] == 'o' || arr[1][2] == 'o') {
			input = 1;
		}
		if (arr[0][2] == 'o') {
			input = 9;
		}
	}
	if (regex_match(action, botOReg)) {
		//general solutions
		if (arr[0][0] == 'n') {
			input = 1;
		}
		if (arr[0][1] == 'n') {
			input = 1;
		}
		if (arr[0][2] == 'n') {
			input = 1;
		}
		//////////////////////
		if (arr[0][2] == 'x') {
			input = 6;
		}
		if (arr[1][2] == 'x') {
			input = 9;
		}
		if (arr[0][0] == 'x' && arr[2][2] == 'x' && arr[1][1] == 'o') {
			input = 6;
		}
		if (arr[2][0] == 'x' && arr[0][2] == 'x' && arr[1][1] == 'o') {
			input = 6;
		}
	}
}
void botHardThirdMove(char arr[3][3], string action, int& input) {
	if (action == "bot x" || action == "b x") {
		if (arr[1][1] == 'o') {
			//do nothing (mediumBot will handle the situation)
		}
		if (arr[0][0] == 'x' && arr[2][0] == 'x') {
			input = 3;
		}
		if (arr[0][2] == 'x' && arr[2][1]) {
			//do nothing (mediumBot will handle the situation) since 
			// o n x
			// n o x
			// x n n
		}
		if (arr[0][2] == 'x' && arr[2][2] == 'x' && arr[0][1] == 'n') {
			input = 3;
		}
		if (arr[0][2] == 'x' && arr[2][2] == 'x' && arr[0][1] == 'o') {
			input = 1;
		}
		if (arr[2][0] == 'x' && arr[2][1] == 'o' && arr[2][2] == 'x') {
			input = 1;
		}
		if (arr[0][0] == 'o' && arr[2][0] == 'x' && arr[2][1] == 'o' && arr[2][2] == 'x') {
			input = 3;
		}
		if (arr[1][0] == 'o' && arr[2][0] == 'x' && arr[2][1] == 'o' && arr[2][2] == 'x') {
			input = 3;
		}
	}
}
void copyArr(char arr[3][3], char(&tempArr)[3][3], int size) {
	for (int i = 0; i < size; i++) {
		for (int j = 0; j < size; j++) {
			tempArr[i][j] = arr[i][j];
		}
	}
}
void inputO(int input, char(&arr)[3][3]) {
	if (input == 1) {
		arr[0][0] = 'o';
	}
	if (input == 2) {
		arr[0][1] = 'o';
	}
	if (input == 3) {
		arr[0][2] = 'o';
	}
	if (input == 4) {
		arr[1][0] = 'o';
	}
	if (input == 5) {
		arr[1][1] = 'o';
	}
	if (input == 6) {
		arr[1][2] = 'o';
	}
	if (input == 7) {
		arr[2][0] = 'o';
	}
	if (input == 8) {
		arr[2][1] = 'o';
	}
	if (input == 9) {
		arr[2][2] = 'o';
	}
}
void inputX(int input, char(&arr)[3][3]) {
	if (input == 1) {
		arr[0][0] = 'x';
	}
	if (input == 2) {
		arr[0][1] = 'x';
	}
	if (input == 3) {
		arr[0][2] = 'x';
	}
	if (input == 4) {
		arr[1][0] = 'x';
	}
	if (input == 5) {
		arr[1][1] = 'x';
	}
	if (input == 6) {
		arr[1][2] = 'x';
	}
	if (input == 7) {
		arr[2][0] = 'x';
	}
	if (input == 8) {
		arr[2][1] = 'x';
	}
	if (input == 9) {
		arr[2][2] = 'x';
	}
}
int checkCheckArr(bool checkArr[9]) {
	int count = 0;
	for (int i = 0; i < 9; i++) { //9 is the size of checkArr
		if (checkArr[i] == true) {
			count++;
		}
	}
	return count;
}
void botHard(int& input, bool(&checkArr)[9], char arr[3][3], int size, string action, int turn) {
	FindTwoCrosses(arr, size, input, action);
	static int didFirstMoveHappen = 0;
	static int didSecondMoveHappen = 0;
	static int didThirdMoveHappen = 0;
	if (checkCheckArr(checkArr) == 0) {
		didFirstMoveHappen = 0;
		didSecondMoveHappen = 0;
		didThirdMoveHappen = 0;
	}
	if (regex_match(action, botOReg)) {
		if (checkCheckArr(checkArr) == 1) {
			didFirstMoveHappen = 0;
			didSecondMoveHappen = 0;
			didThirdMoveHappen = 0;
		}
	}
	if (input == -1) {
		if (didFirstMoveHappen == 0) {
			botHardFirstMove(arr, action, input);
			didFirstMoveHappen++;
		}


	}
	if (input == -1) {
		if (didSecondMoveHappen == 0) {
			botHardSecondMove(arr, action, input);
			didSecondMoveHappen++;
		}
	}
	if (input == -1) {
		if (didThirdMoveHappen == 0) {
			botHardThirdMove(arr, action, input);
		}
	}
	//cout << "game: " << checkWin(arr, turn, size);
	if (input == -1) {
		botEasy(input, checkArr); //call the expert
	}
}
void inputTurn(int& turn, int& input, char(&arr)[3][3], int size, bool(&checkArr)[9], string action, string difficulty) {
	//перевірка x і o
	if (turn == 1 && checkWin(arr, turn, size) == 0) {
		checkIfElementExists(checkArr, input);
		fillCheckArr(checkArr, input);
		inputX(input, arr);
		turn = 0;
		//displayArr(arr, size);
		displayArrViaPixels(arr, size);
		//cout << checkWin(arr, turn, size);
		if (checkWin(arr, turn, size) == 0) {
			if (regex_match(action, botOReg)) {
				if (difficulty == "easy") {
					botEasy(input, checkArr);
				}
				else if (difficulty == "medium") {
					botMedium(input, checkArr, arr, size, action);
				}
				else if (difficulty == "hard") {
					botHard(input, checkArr, arr, size, action, turn);
				}
			}

			else {
				cout << "Turn o: ";
				cin >> input;
				while (input < 1 || input > 9) {
					cout << "Wrong. Enter input:\n";
					cin >> input;
				}
			}
		}
	}
	if (turn == 0 && checkWin(arr, turn, size) == 0) {
		checkIfElementExists(checkArr, input);
		fillCheckArr(checkArr, input);
		inputO(input, arr);
		turn = 1;
		//displayArr(arr, size);
		displayArrViaPixels(arr, size);
		//cout << checkWin(arr, turn, size) << '\n';
		if (checkWin(arr, turn, size) == 0) {
			if (regex_match(action, botXReg)) {
				if (difficulty == "easy") {
					botEasy(input, checkArr);
				}
				else if (difficulty == "medium") {
					botMedium(input, checkArr, arr, size, action);
				}
				else if (difficulty == "hard") {
					botHard(input, checkArr, arr, size, action, turn);
				}
			}
			else {
				cout << "Turn x: ";
				cin >> input;
				while (input < 1 || input > 9) {
					cout << "Wrong. Enter input:\n";
					cin >> input;
				}
			}
		}

	}
}
void playerMode(string action) {
	int turn = 1; // 0 - o 1 - x
	const int SIZE = 3;
	char arr[SIZE][SIZE]{};
	bool checkArr[] = { false,false,false,false,false,false,false,false,false };
	fillArrWithN(arr, SIZE);
	//displayArr(arr, size);
	displayArrViaPixels(arr, SIZE);
	int input;
	cout << "Enter input:\n";
	cout << "Turn x: ";
	cin >> input;
	while (input < 1 || input > 9) {
		cout << "Wrong. Enter input:\n";
		cin >> input;
	}
	string difficulty = "";
	while (checkWin(arr, turn, SIZE) == 0) {
		inputTurn(turn, input, arr, SIZE, checkArr, action, difficulty);
		alertResult(arr, turn, SIZE);
	}
}
void botMode(string action) {
	int turn = 1; // 0 - o 1 - x
	const int SIZE = 3;
	char arr[SIZE][SIZE]{};
	bool checkArr[] = { false,false,false,false,false,false,false,false,false };
	fillArrWithN(arr, SIZE);
	//displayArr(arr, size);
	string difficulty;
	cout << "Enter the difficulty. Available choices: easy, medium, hard\n";
	cin >> difficulty;
	while (difficulty != "easy" && difficulty != "medium" && difficulty != "hard") {
		cout << "Wrong difficulty. Enter the difficulty. Available choices: easy, medium, hard\n";
		cin >> difficulty;
	}
	displayArrViaPixels(arr, SIZE);
	int input = -1;
	if (regex_match(action, botOReg)) {
		cout << "Enter input:\n";
		cout << "Turn x: ";
		cin >> input;
	}
	while ((input < 1 || input > 9)) {
		if (regex_match(action, botXReg)) {
			break;
		}
		cout << "Wrong. Enter input:\n";
		cin >> input;
	}
	if (regex_match(action, botOReg)) {
		checkIfElementExists(checkArr, input);
		fillCheckArr(checkArr, input);
		inputX(input, arr);
		turn = 0;
	}
	bool flag = 0;
	if (flag == 0) {
		flag = 1;
		if (difficulty == "easy") {
			botEasy(input, checkArr);
		}
		else if (difficulty == "medium") {
			botMedium(input, checkArr, arr, SIZE, action);
		}
		else if (difficulty == "hard") {
			botHard(input, checkArr, arr, SIZE, action, turn);
		}
		if (regex_match(action, botXReg)) {
			inputO(input, arr);
			//turn = 0;
		}
	}
	while (checkWin(arr, turn, SIZE) == 0) {
		inputTurn(turn, input, arr, SIZE, checkArr, action, difficulty);
		alertResult(arr, turn, SIZE);
	}

}

void inputAction() {
	string action = "something";
	while (action != "stop") {
		cout << "Would you like to play with a bot or with a player? If you want to play with a bot, type in 'bot' or 'b' with a corresponding symbol 'x' or 'o' e.g. 'bot x', if you want to play with a player, type in 'player' or 'p'. If you want to stop playing the game, type in 'stop'. \n";
		getline(cin, action);
		//cout <<"action: "<< action << '\n';
		while (!regex_match(action, botXBotORegPlayerRegStop)) {
			cout << "Wrong action. If you want to play with a bot, type in 'bot' or 'b' with a corresponding symbol 'x' or 'o' e.g. 'bot x'. if you want to play with a player, type in 'player' or 'p'. If you want to stop playing the game, type in 'stop'. \n";
			getline(cin, action);
		}
		if (regex_match(action, playerReg)) {
			playerMode(action);
		}
		if (regex_match(action, botXBotOReg)) {
			botMode(action);
		}
		if (action == "stop") {
			return;
		}
		cin.ignore();
	}
}
int main()
{
	inputAction();
}

# basic pacman code
my first pacman game in c++



#include <iostream>
#include <iomanip>
#include <windows.h>



using namespace std;

bool time = true;

int velocidade = 10;
int x, y = 0;
bool gameover = false;
char pacman = '^';

int contador;

int opcao;
int pacmanx = 20;
int pacmany = 12;
int ghostx = 1;
int ghosty = 1;
int score = 0;

bool ghost = true;
bool game1 = true;
bool game2 = false;
bool movimento = false;
bool powerup = false;
int powertime = 0;


//fantasmas


void startup()
{

	contador = 0;

	SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 9);

	while(contador != 3)
	{

		for (x = 0; x < 3; x++)
		{
			if (contador < 1)
			{


				for(y = 0; y < 10; y++)
				{
					cout << "processing"[y];
					Sleep(50);
				}
			}
			else
			{
				cout << "processing";
			}

			Sleep(500);

			cout << ".";

			Sleep(500);

			cout << ".";

			Sleep(500);

			cout << ".";

			system("cls");

			contador = contador + 1;

		}

	}

	contador = 0;
//loading maps
	while(contador != 2)
	{

		for (x = 0; x < 2; x++)
		{
			if (contador < 1)
			{


				for(y = 0; y < 11; y++)
				{
					cout << "loading map"[y];
					Sleep(50);
				}
			}
			else
			{
				cout << "loading map";
			}

			Sleep(500);

			cout << ".";

			Sleep(500);

			cout << ".";

			Sleep(500);

			cout << ".";

			system("cls");

			contador = contador + 1;
		}


	}
	system ("cls");
}

//0 parede
//1 ponto
//2 ghost
//3 pacman
//4 espaço

int mapa[22][25] =
{

	0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
	0, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,
	0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,
	0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0,
	0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 4, 0, 4, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0,
	4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4,
	0, 0, 0, 0, 0, 1, 0, 4, 0, 0, 0, 0, 4, 0, 0, 0, 0, 4, 0, 1, 0, 0, 0, 0, 0,
	0, 1, 1, 1, 1, 1, 4, 4, 0, 4, 4, 4, 4, 4, 4, 4, 0, 4, 4, 1, 1, 1, 1, 1, 0,
	0, 0, 0, 0, 0, 1, 0, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 4, 0, 1, 0, 0, 0, 0, 0,
	4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4,
	0, 0, 0, 0, 0, 1, 0, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 4, 0, 1, 0, 0, 0, 0, 0,
	0, 5, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 5, 0,
	0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0,
	0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0,
	0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0,
	0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,
	0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
};

int mapa2[22][25] =
{

	0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0,
	0, 2, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,
	0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,
	0, 1, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0,
	0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0, 4, 0, 4, 0, 0, 0, 0, 0, 1, 0, 0, 0, 0, 0,
	4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4,
	0, 0, 0, 0, 0, 1, 0, 4, 0, 0, 0, 0, 4, 0, 0, 0, 0, 4, 0, 1, 0, 0, 0, 0, 0,
	0, 1, 1, 1, 1, 1, 4, 4, 0, 4, 4, 4, 4, 4, 4, 4, 0, 4, 4, 1, 1, 1, 1, 1, 0,
	0, 0, 0, 0, 0, 1, 0, 4, 0, 0, 0, 0, x, 0, 0, 0, 0, 4, 0, 1, 0, 0, 0, 0, 0,
	4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 4, 0, 1, 0, 4, 4, 4, 4,
	0, 0, 0, 0, 0, 1, 0, 4, 0, 0, 0, 0, 0, 0, 0, 0, 0, 4, 0, 1, 0, 0, 0, 0, 0,
	0, 5, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 5, 0,
	0, 1, 0, 0, 0, 1, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 1, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0, 1, 1, 1, 0,
	0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 1, 0, 0, 0,
	0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0, 1, 1, 1, 1, 1, 0,
	0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0, 1, 0, 0, 0, 0, 0, 0, 0, 0, 0, 1, 0,
	0, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 3, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 1, 0,
	0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0
};

void clearscreen()
{
	HANDLE hOut;
	COORD Position;

	hOut = GetStdHandle(STD_OUTPUT_HANDLE);

	Position.X = 0;
	Position.Y = 0;
	SetConsoleCursorPosition(hOut, Position);
}

void mapareset()
{

	for(x = 0; x < 22; x++)
	{
		for(y = 0; y < 25; y++)
		{
			mapa[x][y] = mapa2[x][y];

		}
	}

	pacmanx = 20;
	pacmany = 12;
	ghostx = 1;
	ghosty = 1;
	score = 0;
	ghost = true;

}

void criarmapaintro()
{

	for(x = 0; x < 22; x++)
	{
		for(y = 0; y < 25; y++)
		{
			if ((mapa[x][y]) == 1)
			{

				cout << " ";
				Sleep(2);
			}

			if ((mapa[x][y]) == 2)
			{

				cout << "#";
				Sleep(2);
			}
			if ((mapa[x][y]) == 0)
			{
				cout << "º";
				Sleep(2);
			}
		}
		cout << endl;
	}

}

void mantermapa()
{
	clearscreen();

	for(x = 0; x < 22; x++)
	{
		for(y = 0; y < 25; y++)
		{

			if ((mapa[x][y]) == 0)
			{

				SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 9);
				cout << "º";

			}

			if ((mapa[x][y]) == 1)
			{

				SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 15);
				cout << ".";

			}

			if ((mapa[x][y]) == 2)
			{
				SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 4);
				cout << "#";

			}

			if ((mapa[x][y]) == 3)
			{
				if (powerup == false)
				{

					SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 10);
					cout << '^';

				}

				else
				{

					SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 5);
					cout << '^';
				}

			}

			if ((mapa[x][y]) == 4)
			{

				cout << " ";

			}
			if ((mapa[x][y]) == 5)
			{
				SetConsoleTextAttribute(GetStdHandle(STD_OUTPUT_HANDLE), 9);
				cout << "o";

			}

		}
		cout << endl;

	}
	cout << setw(5) << "score: " << score;
	

	if (powertime >= 0)
	{
		cout << setw(10) << powertime;
	}

}


void ghost1()
{
	//descer

	movimento = false;

	if ((powerup == false) && (ghost == true))
	{

		if ((ghostx < pacmanx) && (movimento == false))
		{

			if(mapa[ghostx + 1][ghosty] != 0)
			{
				if(mapa[ghostx + 1][ghosty] == 4)
				{
					mapa[ghostx][ghosty] = 4;
					mapa[ghostx + 1][ghosty] = 2;
					ghostx = ghostx + 1;
					Sleep(velocidade);
					movimento = true;

				}
				else
				{

					mapa[ghostx][ghosty] = 1;
					mapa[ghostx + 1][ghosty] = 2;
					ghostx = ghostx + 1;
					Sleep(velocidade);
					movimento = true;
				}

			}
		}



		//direita

		if ((ghosty < pacmany) && (movimento == false))
		{
			if(mapa[ghostx ][ghosty + 1] != 0)
			{

				if(mapa[ghostx ][ghosty + 1] == 4)
				{
					mapa[ghostx][ghosty] = 4;
					mapa[ghostx][ghosty + 1] = 2;
					ghosty = ghosty + 1;
					Sleep(velocidade);
					movimento = true;

				}
				else
				{
					mapa[ghostx][ghosty] = 1;
					mapa[ghostx][ghosty + 1] = 2;
					ghosty = ghosty + 1;
					Sleep(velocidade);
					movimento = true;

				}


			}
		}

		//subir

		if ((ghostx > pacmanx) && (movimento == false))
		{

			if(mapa[ghostx - 1][ghosty] != 0)
			{

				if(mapa[ghostx - 1][ghosty] == 4)
				{

					mapa[ghostx][ghosty] = 4;
					mapa[ghostx - 1][ghosty] = 2;
					ghostx = ghostx - 1;
					Sleep(velocidade);
					movimento = true;
				}

				else
				{

					mapa[ghostx][ghosty] = 1;
					mapa[ghostx - 1][ghosty] = 2;
					ghostx = ghostx - 1;
					Sleep(velocidade);
					movimento = true;
				}

			}
		}



		//esquerda

		if ((ghosty > pacmany) && (movimento == false))
		{

			if(mapa[ghostx][ghosty - 1] != 0)
			{


				if(mapa[ghostx][ghosty - 1] == 4)
				{

					mapa[ghostx][ghosty] = 4;
					mapa[ghostx][ghosty - 1] = 2;
					ghosty = ghosty - 1;
					Sleep(velocidade);
					movimento = true;
				}
				else
				{
					mapa[ghostx][ghosty] = 1;
					mapa[ghostx][ghosty - 1] = 2;
					ghosty = ghosty - 1;
					Sleep(velocidade);
					movimento = true;
				}


			}
		}
		//if powerup false

	}


}


int main()
{


	while(gameover == false)
	{

		system("cls");

			if (time == true)
			{
				startup();
				time = false;
			}

		mapareset();
		criarmapaintro();
		clearscreen();
		mantermapa();

		while (game1 == true)
		{


			//movimento
			//0=parede
			//1=ponto
			//2=ghost
			//3=pacman
			//4=espaço

			if (powertime <= 0)
			{
				powerup = false;


			}


			if (GetAsyncKeyState(VK_UP))
			{
				//caso parede
				if(	(mapa[pacmanx - 1][pacmany]) == 0)
				{
					mapa[pacmanx - 1][pacmany] = 0;
					mapa[pacmanx][pacmany] = 3;


				}

				//caso ponto
				if(	(mapa[pacmanx - 1][pacmany]) == 1)
				{
					mapa[pacmanx - 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmanx = pacmanx - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powertime--;
				}

				//caso espaço
				if((mapa[pacmanx - 1][pacmany]) == 4)
				{


					mapa[pacmanx - 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmanx = pacmanx - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					powertime--;

				}

				// caso powerup

				if(	(mapa[pacmanx - 1][pacmany]) == 5)
				{
					mapa[pacmanx - 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmanx = pacmanx - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powerup = true;
					powertime = 10;

				}
				//caso ghost

				if(((mapa[pacmanx - 1][pacmany]) == 2))
				{
					if (powerup == true)
					{
						ghost = false;
						mapa[pacmanx - 1][pacmany] = 3;
						mapa[pacmanx][pacmany] = 4;
						pacmanx = pacmanx - 1;
						ghost1();
						mantermapa();
						Sleep(velocidade);
						powertime--;
					}
					else
					{
						mapa[pacmanx - 1][pacmany] = 2;
						mapa[pacmanx][pacmany] = 4;
						mantermapa();
						game1 = false;

					}

				}


			}

			if (GetAsyncKeyState(VK_DOWN))
			{

				//caso parede
				if(	(mapa[pacmanx + 1][pacmany]) == 0)
				{
					mapa[pacmanx + 1][pacmany] = 0;
					mapa[pacmanx][pacmany] = 3;


				}

				//caso ponto
				if(	(mapa[pacmanx + 1][pacmany]) == 1)
				{


					mapa[pacmanx + 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmanx = pacmanx + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powertime--;
				}

				//caso espaço
				if ((mapa[pacmanx + 1][pacmany]) == 4)
				{

					mapa[pacmanx + 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmanx = pacmanx + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					powertime--;

				}

				//caso powerup

				if(	(mapa[pacmanx + 1][pacmany]) == 5)
				{


					mapa[pacmanx + 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmanx = pacmanx + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powerup = true;
					powertime = 10;
				}

				//caso ghost

				if(((mapa[pacmanx + 1][pacmany]) == 2))
				{
					if (powerup == true)
					{


						ghost = false;
						mapa[pacmanx + 1][pacmany] = 3;
						mapa[pacmanx][pacmany] = 4;
						pacmanx = pacmanx + 1;
						ghost1();
						mantermapa();
						Sleep(velocidade);
						powertime--;
					}
					else
					{
						mapa[pacmanx + 1][pacmany] = 2;
						mapa[pacmanx][pacmany] = 4;
						mantermapa();
						game1 = false;

					}

				}




			}

			if (GetAsyncKeyState(VK_LEFT))
			{
				//caso parede
				if(	(mapa[pacmanx][pacmany - 1]) == 0)
				{
					//tunel
					if ((pacmanx == 10) && (pacmany == 1))

					{
						mapa[pacmanx][pacmany] = 4;

						pacmany = 23;

						mapa[pacmanx][pacmany] = 3;
						mantermapa();
						powertime--;

					}
					else
						mapa[pacmanx][pacmany - 1] = 0;
					mapa[pacmanx][pacmany] = 3;


				}


				//casoponto
				if(	(mapa[pacmanx][pacmany - 1]) == 1)
				{


					mapa[pacmanx][pacmany - 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmany = pacmany - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powertime--;
				}

				//caso espaço
				if ((mapa[pacmanx][pacmany - 1]) == 4)
				{
					mapa[pacmanx][pacmany - 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmany = pacmany - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					powertime--;

				}

				//caso powerup

				if(	(mapa[pacmanx][pacmany - 1]) == 5)
				{


					mapa[pacmanx][pacmany - 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmany = pacmany - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powerup = true;
					powertime = 10;
				}

				//caso ghost

				if(((mapa[pacmanx][pacmany - 1]) == 2))
				{
					if (powerup == true)
					{


						ghost = false;
						mapa[pacmanx ][pacmany - 1] = 3;
						mapa[pacmanx][pacmany] = 4;
						pacmany = pacmany - 1;
						ghost1();
						mantermapa();
						Sleep(velocidade);
						powertime--;
					}
					else
					{
						mapa[pacmanx][pacmany - 1] = 2;
						mapa[pacmanx][pacmany] = 4;
						mantermapa();
						game1 = false;

					}

				}


			}

			if (GetAsyncKeyState(VK_RIGHT))
			{


				//caso parede
				if(	(mapa[pacmanx][pacmany + 1]) == 0)
				{
					//tunel
					if ((pacmanx == 10) && (pacmany == 23))

					{
						mapa[pacmanx][pacmany] = 4;

						pacmany = 1;

						mapa[pacmanx][pacmany] = 3;
						mantermapa();
						powertime--;

					}
					else
					{
						mapa[pacmanx][pacmany + 1] = 0;
						mapa[pacmanx][pacmany] = 3;
					}

				}

				//caso ponto
				if(	(mapa[pacmanx][pacmany + 1]) == 1)
				{


					mapa[pacmanx][pacmany + 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmany = pacmany + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powertime--;
				}



				//caso espaço
				if ((mapa[pacmanx ][pacmany + 1]) == 4)
				{

					mapa[pacmanx][pacmany + 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmany = pacmany + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					powertime--;

				}

				//caso powerup

				if(	(mapa[pacmanx][pacmany + 1]) == 5)
				{


					mapa[pacmanx][pacmany + 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					pacmany = pacmany + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);
					score = score + 10;
					powerup = true;
					powertime = 10;
				}

				//caso ghost

				if(((mapa[pacmanx][pacmany + 1]) == 2))
				{
					if(powerup == true)
					{


						ghost = false;
						mapa[pacmanx ][pacmany + 1] = 3;
						mapa[pacmanx][pacmany] = 4;
						pacmany = pacmany + 1;
						ghost1();
						mantermapa();
						Sleep(velocidade);
						powertime--;
					}
					else
					{
						mapa[pacmanx][pacmany+1] = 2;
						mapa[pacmanx][pacmany] = 4;
						mantermapa();
						game1 = false;

					}

				}

			}

			//caso ganhe proximo nivel e mapa reset
			if (score == 2040)
			{

				game1 = false;
				game2 = true;
				cout << endl;

				for(y = 0; y < 18; y++)

				{
					cout << "congratz lvl 2 now"[y];
					Sleep(80);

				}

				system("cls");
				mapareset();
				mapa[14][23] = 1;
				mapa[14][1] = 1;
				mapa[11][12] = 4;
				criarmapaintro();
				clearscreen();
				mantermapa();


			}

			//caso morra para fantasma
			if ((pacmanx == ghostx) && (pacmany == ghosty) && (powerup == false))
			{

				if (ghost == true)
				{
					mantermapa();
					game1 = false;
				}


			}


		}

		//nivel2

		//	mapa[14][23] = 1;


		while(game2 == true)
		{
			ghost = true;

			//movimento
			//0=parede
			//1=ponto
			//2=spawn mobs
			//3=pacman
			//4=espaço

			if (GetAsyncKeyState(VK_UP))
			{
				//caso parede
				if(	(mapa[pacmanx - 1][pacmany]) == 0)
				{
					mapa[pacmanx - 1][pacmany] = 0;
					mapa[pacmanx][pacmany] = 3;


				}

				//caso ponto
				if(	(mapa[pacmanx - 1][pacmany]) == 1)
				{
					mapa[pacmanx - 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmanx = pacmanx - 1;
					ghost1();
					score = score + 10;
					mantermapa();
					Sleep(velocidade);

				}

				//caso espaço
				if((mapa[pacmanx - 1][pacmany]) == 4)
				{


					mapa[pacmanx - 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmanx = pacmanx - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);

				}

			}


			if (GetAsyncKeyState(VK_DOWN))
			{

				//caso parede
				if(	(mapa[pacmanx + 1][pacmany]) == 0)
				{
					mapa[pacmanx + 1][pacmany] = 0;
					mapa[pacmanx][pacmany] = 3;


				}

				//caso ponto
				if(	(mapa[pacmanx + 1][pacmany]) == 1)
				{


					mapa[pacmanx + 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmanx = pacmanx + 1;
					ghost1();
					score = score + 10;
					mantermapa();
					Sleep(velocidade);

				}

				//caso espaço
				if ((mapa[pacmanx + 1][pacmany]) == 4)
				{

					mapa[pacmanx + 1][pacmany] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmanx = pacmanx + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);

				}


			}

			if (GetAsyncKeyState(VK_LEFT))
			{
				//caso parede
				if(	(mapa[pacmanx][pacmany - 1]) == 0)
				{

					mapa[pacmanx][pacmany - 1] = 0;
					mapa[pacmanx][pacmany] = 3;

				}


				//casoponto
				if(	(mapa[pacmanx][pacmany - 1]) == 1)
				{


					mapa[pacmanx][pacmany - 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmany = pacmany - 1;
					ghost1();
					score = score + 10;
					mantermapa();
					Sleep(velocidade);

				}

				//caso espaço
				if ((mapa[pacmanx][pacmany - 1]) == 4)
				{
					mapa[pacmanx][pacmany - 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmany = pacmany - 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);

				}

			}


			if (GetAsyncKeyState(VK_RIGHT))
			{


				//caso parede
				if(	(mapa[pacmanx][pacmany + 1]) == 0)
				{
					//tunel

					mapa[pacmanx][pacmany + 1] = 0;
					mapa[pacmanx][pacmany] = 3;

				}

				//caso ponto
				if(	(mapa[pacmanx][pacmany + 1]) == 1)
				{


					mapa[pacmanx][pacmany + 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmany = pacmany + 1;
					ghost1();
					score = score + 10;
					mantermapa();
					Sleep(velocidade);

				}



				//caso espaço
				if ((mapa[pacmanx ][pacmany + 1]) == 4)
				{

					mapa[pacmanx][pacmany + 1] = 3;
					mapa[pacmanx][pacmany] = 4;
					clearscreen();
					pacmany = pacmany + 1;
					ghost1();
					mantermapa();
					Sleep(velocidade);

				}

			}

			if (score == 2040)
			{

				game1 = false;
				game2 = false;

				cout << endl;

				for(y = 0; y < 24; y++)

				{
					cout << "congratz u beat the game"[y];
					Sleep(80);

				}
				gameover = true;

			}

			//caso morra para fantasma
			if ((pacmanx == ghostx) && (pacmany == ghosty))
			{

				game2 = false;


			}


		}

		//caso perca

		if ((game1 == false) && (game2 == false) && (gameover == false))
		{
			cout << endl;
			cout << "1- try again" << endl;
			cout << "2- exit" << endl;

			cin >> opcao;

			switch(opcao)

			{

			case 1:


				game1 = true;
				for(y = 0; y < 17; y++)

				{
					cout << "good luck big man"[y];
					Sleep(80);

				}


				break;

			case 2:
				gameover = true;
				cout << "THX FOR PLAYING";



				//	temporizador();
			}

		}


	}

}




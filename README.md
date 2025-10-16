#include <iostream>
#include <cstdlib>
#include <ctime>
#include<vector>
using namespace std;
 	int intentos = 6;
    string palabra;
    char letra;
    vector<char> usadas;
    string oculta;

void escoger (){

    srand(time(0));
    string alzar[10] = {
        "GATO", "PERRO", "CASA", "SOL", "LUNA",
        "CIELO", "MAR", "ANIMALES", "FLOR", "NUBE"
    };
 
    int indice = rand() % 10;  
    palabra = alzar[indice];  
    
}


void intro(){
		cout << "\n===============================" << endl;
        cout << "        NUEVA PARTIDA" << endl;
        cout << "===============================\n";
        cout<<"	HANGMAN				"<<endl;
        cout<<"ADIVINA LA PALABRA CON UN MAXIMO DE 6 INTENTOS, SI NO ¡MORIRA ALGUIEN!\n";
        
}

 void mostrar() {
    cout << " " << endl;
    cout << " |    |" << endl;

    if (intentos <= 5) cout << " O " << endl;
    else cout << endl;

    if (intentos <= 4) {
        if (intentos <= 3) {
            if (intentos <= 2)
                cout << "/|\\";
            else
                cout << "/|";
        } else cout << " |";
        cout << endl;
    } else cout << endl;

    if (intentos <= 1) cout << "/ \\" << endl;
    else if (intentos <= 2) cout << " /" << endl;
    else cout << endl;

    cout << "======" << endl;
}

void reiniciar() {
    intentos = 6;
    usadas.clear();
    
	oculta = string(palabra.size(), '_');
}
void juego(){

	string entrada;
    while (intentos > 0 && oculta != palabra){
        cout << "\nPalabra: " << oculta << endl;
        cout << "Intentos restantes: " << intentos << endl;
        mostrar();
        cout << "Ingresa una letra: ";
        cin >> entrada;
        
         if (entrada.size() != 1) {
            cout << "Solo puedes ingresar una letra a la vez.\n";
			continue;
        }
        letra = entrada[0]; 
        
        if(letra>96 ){	
       		 letra = letra -32;
        }
     	while (!((letra >= 'A' && letra <= 'Z') || (letra >= 'a' && letra <= 'z'))) {
				cout << "Caracter invalido. Digita de nuevo la letra: \n";
     			   cin >> letra;
    	}
   
    		
        bool repetida = false;
        for (int i = 0; i < usadas.size(); i++) {
            if (usadas[i] == letra) {
                repetida = true;
                break;
            }
        }
        if (repetida) {
            cout << "Ya usaste esa letra. Intenta con otra.\n";
            continue;
        }
        usadas.push_back(letra);
      	cout << "Letras usadas: ";
		for (int i = 0; i < usadas.size(); i++) {
    		cout << usadas[i] << " ";
			}
         bool acierto = false;
        
        for (int i = 0; i < palabra.size(); i++) {
            if (palabra[i] == letra) {
                oculta[i] = letra;  
                acierto = true;
            }
		}
        if (!acierto) {
            intentos--;
            cout << "\nLetra incorrecta!" << endl;
        } else {
            cout << "\n Bien hecho!" << endl;
        }
        
    }
    
}

void fin(){
	mostrar();
    if (oculta == palabra) {
        cout << "\n¡VICTORIA! Adivinaste la palabra: " << palabra << endl;
    } else {
        cout << "\n GAME OVER La palabra era: " << palabra << endl;
    }
}

bool repetirJuego() {
    char respuesta;
    cout << "\n ¿Deseas jugar otra vez? (S/N): ";
    cin >> respuesta;
    return (respuesta == 'S' || respuesta == 's');
}
   
int main() {
  do {
        escoger();
      	reiniciar();
      	intro();
        juego();
        fin();
    } while (repetirJuego());  
    cout << "\nGracias por jugar. ¡Hasta la proxima!\n";
    return 0;
}



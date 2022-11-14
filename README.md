#include <stdio.h>
#include <stdlib.h>
#include <time.h>
#include<windows.h>

//DANIELE VITAGLIANO 4IA® 
struct informazioni
	{
		char nome[40];
		int numero_giocate;
		int eta;
	//	float credito;
	};
void  carica_dati(struct informazioni dato)
	{	
		printf("Inserisci il tuo nome:\n");
			gets(dato.nome);
		printf("Inserisci il numero di giocate che hai fatto:\n");
			scanf("%d",&dato.numero_giocate);
		printf("Inserisci la tua eta:\n");
			scanf("%d",&dato.eta);	
	}
void stampa_dati(struct informazioni dato)
	{
		printf("Ecco il nome del giocatore:\t%s\n",dato.nome);
		printf("Inserisci la tua eta':\t%d\n",dato.numero_giocate);
		printf("Ecco la sua eta':\t%d\n",dato.eta);
	}
void SetColor(unsigned short color)
{
	HANDLE hCon=
	GetStdHandle(STD_OUTPUT_HANDLE);
	SetConsoleTextAttribute(hCon,color);
}		
//creo le carte randomiche
int mischia(int carte[])
{
    
    int t;
    int i;
    int tavolo[52];
    for (i=0;i<52;i++)
        tavolo[i] = (i/13+3)*100 + i%13 + 1;

    srand(time(NULL));
    for (i = 0; i < 52; i++)
    {
        do
        {
        t = rand() % 52;
        } while (tavolo[t] == 0);
        carte[i] = tavolo[t];
        tavolo[t] = 0;
        
    }
    
    return 0;
}
// converto il Re , la reggina e il Jack
int convert_jkq(int a)
{
    if ((a%100==11) ||(a%100==12) ||(a%100==13)) return (a/100)*100+10;
    else return a;
}
//faccio uscire le carte a video
void pic(int num)
{
    char fl;
    int po_num;
    
    fl = num / 100;
    po_num = num % 100;
    switch (po_num)
    {
        case 1:
        {
            printf(" _______ \n");
            printf("|       |\n");
            printf("|  %c    |\n", fl);
            printf("|  ASSO |\n");
            printf("|       |\n");
            printf("|_______|\n");
            break;
        }
        case 2:
        case 3:
        case 4: 
        case 5:
        case 6:
        case 7:
        case 8:
        case 9:
        case 10:
        {
            printf(" _______ \n");
            printf("|       |\n");
            printf("|  %c    |\n", fl);
            printf("|   %2d  |\n",po_num);
            printf("|       |\n");
            printf("|_______|\n");
            break;
        }
        case 11:
        {
            printf(" _______ \n");
            printf("|       |\n");
            printf("|  %c    |\n", fl);
            printf("|  JACK |\n");
            printf("|       |\n");
            printf("|_______|\n");
            break;
        }
        case 12:
        {
            printf(" _______ \n");
            printf("|       |\n");
            printf("|  %c    |\n", fl);
            printf("| QUEEN |\n");
            printf("|       |\n");
            printf("|_______|\n");
            break;
        }
        case 13:
        {
            printf(" _______ \n");
            printf("|       |\n");
            printf("|  %c    |\n", fl);
            printf("|  RE   |\n");
            printf("|       |\n");
            printf("|_______|\n");
            break;
        }
    }
}
int gioca(void)
{
    int i;
    int psomma=0;
    int bsomma=0;
    int pcarte[5]={0};
    int bcarte[5]={0};
    int carte[52];
    char vai_su;
    char d;
//benvenuto  
	printf("Benvenuto nel mio Black Jack!\n"
	"Se non vuoi giocare premi Ctrl+C per uscire, ma mi farai diventare triste :'(.\n"
	"Se vuoi giocare premi invio per continuare ; )......\n");
   
	
	do{
    vai_su = getchar();
    } while (vai_su != '\n');
    printf("\n");
    
    //mischia le carte
   mischia(carte);

    //do le carte all'utente e al banco
    pcarte[0]=carte[0];
    pcarte[1]=carte[1];
    bcarte[0]=carte[2];
    bcarte[1]=carte[3];
    
    //la seconda carta ricevuta dall'utente
    printf("Prima carta del banco:\n");
    pic(bcarte[0]);
    printf("\n");
    printf("Carte del giocatore:\n");
    pic(pcarte[0]);
    printf("\n");
    pic(pcarte[1]);
    printf("\n");
//l'utente sceglie il valore che vuole attribuire all'asso    
    i=0;
    for (i=0; i<2; i++)
    {
        if (pcarte[i]%100 == 1)
        {
            printf("Scegli il valore dell'asso %d, inserisci 's' per farlo valere 11 o 'n' per farlo valere 1 (RICORDA CHE IL CARATTERE DEVE ESSERE SCRITTO IN MINUSCOLO):\n", i+1);
            do{
                d = getchar();
            } while (d!='s' && d!='n');
          	
            if (d == 's')
            {
                printf("Hai scelto il valore 11 per l'asso.\n");
                psomma = psomma + 11;
            }
            else if(d == 'n')
            {
                printf("Hai scelto il valore 1 per l'asso.\n");
                psomma = psomma +1;
            }
        }
        else if (convert_jkq(pcarte[i]) %100 ==10) psomma = psomma + 10;
        else psomma = psomma + pcarte[i]%100;
        
        if (psomma > 21)
        {
        	SetColor(4);
          printf("Somma delle carte del giocatore momentanee:%d\n\n",psomma);
          printf("Il banco vince!\n");
          printf("******         \n");
          printf("******         \n");
        	printf("**             \n");
        	printf("******        \n" );
       		printf("******          \n");
        	printf("**              \n");
        	printf("**               \n");
        	SetColor(4);
            return 0;
        }
        else if (psomma == 21)
        {
        	SetColor(14);
          printf("Somma delle carte del banco momentanee:%d\n\n",psomma);
          printf("Il giocatore vince'!\n");
          printf("**      **       ***         **      **          **     **     ****     **  *********      *******          \n");
        	printf("**      **      **  **       **       **        **      **     ****     **  *********     **     **         \n");
        	printf("**      **     **    **      **        **      **       **     **  **   **      **        **     **         \n" );
        	printf("**********     ********      **         **    **        **     **   **  **      **        **     **         \n");
       		printf("**********     **    **      **          **  **         **     **    ** **      **        **     **         \n");
        	printf("**      **     **    **      **           ****          **     **     ****      **        **     **         \n");
        	printf("**      **     **    **      **            **           **     **      ***      **         *******          \n");
        	SetColor(14);
            return 0;
        }
    }
    printf("Somma delle carte del giocatore momentanee:%d\n\n",psomma);
//chiedo all'tente se vuole un'ulteriore carta    
    i=0;
    for (i=0; i<3; i++)
    {
        char j = 'n';
        
        printf("Vuoi un'altra carta? Premi 's' o 'n'(RICORDA CHE IL CARATTERE DEVE ESSERE SCRITTO IN MINUSCOLO):\n");
        do{
            j = getchar();
        } while (j!='s' &&j!='n');
        
        if (j=='s')
        {
            printf("Hai ottenuto un'altra carta ora.\n");
            pcarte[i+2]=carte[i+4];
            printf("La tua carta ricevuta e':\n", i+3);
            pic(pcarte[i+2]);
            
            if (pcarte[i+2]%100 == 1)
            {
                printf("Scegli il valore che vuoi attribuire all'asso , premi 's' per farlo valere 11 o 'n' per farlo valere 1(RICORDA CHE IL CARATTERE DEVE ESSERE SCRITTO IN MINUSCOLO):\n", i+3);
                do{
                    d = getchar();
                } while (d!='s' && d!='n');
                if (d == 's')
                {
                    printf("Hai scelto di far valere l'asso 11.\n");
                    psomma = psomma + 11;
                }
                else if(d == 'n')
                {
                    printf("Hai scelto di far valere l'asso 1.\n");
                    psomma = psomma +1;
                }
            }
            else if (convert_jkq(pcarte[i+2]) %100 ==10) psomma = psomma + 10;
            else psomma = psomma + pcarte[i+2]%100;
            
            if (psomma > 21)
            {
            SetColor(4);
            printf("Somma delle carte del giocatore:%d\n\n",psomma);
            printf("Il banco vince!\n");
            printf("******         \n");
            printf("******         \n");
        		printf("**             \n");
        		printf("******        \n" );
       		 	printf("******          \n");
        		printf("**              \n");
        		printf("**               \n");
        		SetColor(4);
        	return 0;
                return 0;
            }
            else if (psomma == 21)
            {
            printf("Somma delle carte del banco ora:%d\n\n",psomma);
            printf("Il giocatore vince!\n");
            SetColor(14);
            printf("**      **       ***         **      **          **     **     ****     **  *********      *******          \n");
        		printf("**      **      **  **       **       **        **      **     ****     **  *********     **     **         \n");
        		printf("**      **     **    **      **        **      **       **     **  **   **      **        **     **         \n" );
        		printf("**********     ********      **         **    **        **     **   **  **      **        **     **         \n");
       			printf("**********     **    **      **          **  **         **     **    ** **      **        **     **         \n");
        		printf("**      **     **    **      **           ****          **     **     ****      **        **     **         \n");
        		printf("**      **     **    **      **            **           **     **      ***      **         *******          \n");
        		SetColor(14);
                return 0;
            }
            else printf("Somma delle carte del giocatore momentanee:%d\n\n",psomma);
        }
        else
        {
            printf("Somma delle carte del giocatore momentanee:%d\n\n",psomma);
            break;
        }
    }
    if (i == 3)
    {
        printf("Il giocatore vince! Perché la somma delle tue 5 carte non è maggiore di 21! Che fortunata! :) \n");
        SetColor(14);
        printf("**      **       ***         **      **          **     **     ****     **  *********      *******          \n");
        printf("**      **      **  **       **       **        **      **     ****     **  *********     **     **         \n");
        printf("**      **     **    **      **        **      **       **     **  **   **      **        **     **         \n" );
        printf("**********     ********      **         **    **        **     **   **  **      **        **     **         \n");
       	printf("**********     **    **      **          **  **         **     **    ** **      **        **     **         \n");
        printf("**      **     **    **      **           ****          **     **     ****      **        **     **         \n");
        printf("**      **     **    **      **            **           **     **      ***      **         *******          \n");
        SetColor(14);
        return 0;
    }
    
   
//sezione dedicata alle carte del banco    
    printf("Carte del banco:\n");
    pic(bcarte[0]);
    pic(bcarte[1]);

    if (bcarte[0]%100 + bcarte[1]%100 == 2)
    {
        bsomma=12; 
        printf("Somma delle carte del banco:%d\n\n", bsomma);
    }
    else if ((convert_jkq(bcarte[0]))%100 + (convert_jkq(bcarte[1]))%100 ==1)
    {
        bsomma=21;
        SetColor(4);
        printf("Somma delle carte del banco momentanee:%d\n\n", bsomma);
        printf("IL Banco vince!\n");
        printf("******         \n");
        printf("******         \n");
        printf("**             \n");
        printf("******        \n" );
       	printf("******          \n");
        printf("**              \n");
        printf("**               \n");
        SetColor(4);
        return 0;
    }
    else if (bcarte[0]%100==1 || bcarte[1]%100==1)
    {
        bsomma=(bcarte[0]+bcarte[1])%100+(rand()%2)*10;
        printf("Somma delle carte del banco ora:%d\n\n", bsomma);
    }
    else
    {
        bsomma = (convert_jkq(bcarte[0]))%100 + (convert_jkq(bcarte[1]))%100;
        printf("Somma delle carte del banco ora:%d\n\n", bsomma);
    }
    
   
    for (i=0; i<3 && bsomma<17; i++)
    {
        bcarte[i+2]=carte[i+7];
        printf("Le carte del banco sono %d :\n", i+3);
        pic(bcarte[i+2]);
        
        if (bcarte[i+2]%100 == 1)
        {
            if (bsomma+11 <= 21)
            {
                printf("Il banco ha scelto che il proprio asso vale 11\n");
                bsomma = bsomma+11;
                printf("Somma delle carte del banco ora:%d\n\n", bsomma);
            }
            else
            {
                printf("Il banco ha scelto che il proprio asso vale 1\n");
                bsomma = bsomma+1;
                printf("Somma delle carte momentanee:%d\n\n", bsomma);
            }
        }
        else
        {
            bsomma = bsomma + convert_jkq(bcarte[i+2])%100;
            printf("Somma delle carte momentanee:%d\n\n", bsomma);
        }
    }
    if (i == 3)
    {
    	SetColor(4);
        printf("Il banco vince!Perché la somma delle sue 5 carte non è maggiore di 21! GG :(\n");
        printf("******         \n");
        printf("******         \n");
        printf("**             \n");
        printf("******        \n" );
       	printf("******          \n");
        printf("**              \n");
        printf("**               \n");
        SetColor(4);
        return 0;
    }
    

    if (bsomma>21 || psomma>bsomma)
    {
    	SetColor(14);
        printf("Il giocatore vince! : )\n");
        printf("**      **       ***         **      **          **     **     ****     **  *********      *******          \n");
        printf("**      **      **  **       **       **        **      **     ****     **  *********     **     **         \n");
        printf("**      **     **    **      **        **      **       **     **  **   **      **        **     **         \n" );
        printf("**********     ********      **         **    **        **     **   **  **      **        **     **         \n");
       	printf("**********     **    **      **          **  **         **     **    ** **      **        **     **         \n");
        printf("**      **     **    **      **           ****          **     **     ****      **        **     **         \n");
        printf("**      **     **    **      **            **           **     **      ***      **         *******          \n");
        SetColor(14);
        return 0;
    }
    else if (psomma == bsomma)
    {
        printf("L'utente ed il banco hanno lo stesso valore!\n  NSOMMA : / \n"
		"Questo e' un 'push' non vince nessuno \n");
		printf("*******   **    **  ******    **    **   \n");
		printf("*******   **    **  ******    **    **   \n");
    printf("*    **   **    **  **        **    **   \n");
    printf("*******   **    **  ******    ********   \n" );
    printf("*******   **    **  ******    ********   \n");
    printf("**        **    **      **    **    **   \n");
    printf("**        ********  ******    **    **   \n");
    printf("**         ******   ******    **    **   \n");
    return 3;
    }
    else if (psomma < bsomma)
    {
        printf("Il Banco Vince! Perche' la somma delle proprie carte e' maggiore di quella dell'utente.\n");
        printf("******    ******     \n");
        printf("******    ******     \n");
        printf("**        **         \n" );
        printf("**   **** **   ****  \n");
        printf("**    **  **    **   \n");
        printf("********  *******    \n");
        printf("*******   ******     \n");
        return 1;
    }
    	   
        
    return 0;
}
//CHIEDO ALL'UTENTE SE VUOLE CONTINUARE A GIOCARE
int main(void)
{
	struct informazioni dato;
	int scelta;
    char dinuovo;
    		
     do 
    {
    	system("cls");
		SetColor(11);
  			    printf("******   **         ***      ******   **   **      *****  **     ******   **   **    **   **   **     \n");
            printf("**   **  **        ** **    **    **  **  **         *   *  *   **    **  **  **     **   **   **     \n");
            printf("**   **  **       **   **   **        ** **          *  *    *  **        ** **      **   **   **     \n" );
            printf("******   **       *******   **        * **           *  ******  **        ** **      **  ****  **     \n");
            printf("**   **  *******  **   **   **    **  **  **     *   *  *    *  **    **  **  **      *** ** ***      \n");
            printf("*******  *******  **   **    ******   **   **     ****  *    *   ******   **   **      ** ** **       \n");
            printf("                                                                                          **		  \n");
            printf("                                                                                          **		  \n");
		        printf("*******   **    **      *****    **        **    *****       \n");
            printf("**   **    **  **       **   **   **      **    **  **       \n");
            printf("**   **      **         **    **   **    **    **   **       \n" );
            printf("*******      **         **    **    **  **    ********      \n");
            printf("**    **     **         **   **      ****           **       \n");
            printf("*******      **         ******        **            **       \n");
			SetColor(14);
			      printf("           *******  ******          *******      **   **              **    **      ********   *******               \n");
			      printf("           *******  ******          *******    **  **  **            **   **  **    ********   *******               \n");
            printf("                **      **          **        **    **  **          **   **    **   **         **                    \n");
            printf("               **       **          *******  **      **  **        **   **      **  **         **                    \n");
            printf("              **        **          *******  **********   **      **    **********  **         *******               \n");
            printf("             **         **               **  **********    **    **     **********  **   ***** *******               \n");
            printf("            **          **               **  **      **     **  **      **      **  **     **  **                    \n");
			      printf("           *******    ******        *******  **      **      ****       **      **  *********  *******               \n");
			      printf("           *******    ******        *******  **      **       **        **      **  *********  *******               \n");
            SetColor(14);
			SetColor(5);
			printf("\n---Digita la tua scelta\n");
			printf("1)Inserisci i tuoi dati per giocare\n");
		    SetColor(5);
		    SetColor(2);
	        printf("2)GIOCA\n");
	        SetColor(2);
	        SetColor(4);
	        printf("3)VEDI LE REGOLE DEL GIOCO\n");
	        SetColor(4);
	        SetColor(15);
          	printf("4)ESCI DAL GIOCO\n");
          	SetColor(15);
        	scanf("%d",&scelta);	//opzioni di ricerca
        	fflush(stdin);				//pulisco il buffer
  	switch (scelta)
  	{
  		case 1:
  			{
  				printf("Compila le seguenti richieste:\n");
  				carica_dati(dato);
  				printf("Ecco i dati raccolti:\n");
  				stampa_dati(dato);
  				break;
			  }
        case 2:
	    {
	    	system("cls");
	    	gioca();
	  
	    	printf("\nVuoi giocare ancora? Premi 's' o 'n'(RICORDA CHE IL CARATTERE DEVE ESSERE SCRITTO IN MINUSCOLO):\n");
	    	do{
	       	 	dinuovo = getchar();
	    	} while (dinuovo!='s' && dinuovo!='n');
	   
	    	if (dinuovo == 's') 
	    
	    
	    	{
	        	printf("\nOK, Rigiochiamo! : )\n\n");
	        	system("cls");
	        	main();
	        
	    	}

         
	    	break;
	    	
    	}
    	case 3:
    	{
    	 printf("\nINTRUDUZIONE AL GIOCO.\n"
		"Una volta che i giocatori hanno fatto la loro puntata, il banchiere procedendo da sinistra verso destra assegna a ciascuno dei giocatori una carta scoperta in ogni postazione giocata, assegnando l'ultima al banco.\n"
		"Effettua poi un secondo giro di carte scoperte, senza pero' attribuirne una a se stesso.\nAvvenuta la distribuzione, il mazziere  legge in ordine il punteggio di ciascun giocatore invitandoli a manifestare il loro gioco: essi potranno chiedere carta o stare, a loro discrezione.\nSe un giocatore supera il 21 perde e il mazziere incassera' la puntata.\nUna volta che i giocatori hanno definito i loro punteggi il mazziere sviluppa il suo gioco seguendo la (regola del banco): egli deve tirare carta con un punteggio inferiore a 17.\n"
		"Una volta ottenuto o superato 17 si deve fermare. Se oltrepassa il 21 il banco 'sballa' e deve pagare tutte le puntate rimaste sul tavolo.\nUna volta definiti tutti i punteggi, il mazziere confronta il proprio con quello degli altri giocatori, paga le combinazioni superiori alla sua, ritira quelle inferiori e lascia quelle in parita'.\n"
		"Il pagamento delle puntate vincenti e' alla pari.\n");
		break;
    	}
        case 4:
				return 0;
      }
      
	  }while(scelta!=4);
	  	system("cls");
	  	gioca();
	  
	    	printf("\nVuoi giocare ancora? Premi 's' o 'n'(RICORDA CHE IL CARATTERE DEVE ESSERE SCRITTO IN MINUSCOLO):\n");
	    	
	    	do{
	       	 	dinuovo = getchar();
	    	} while (dinuovo!='s' && dinuovo!='n');
	   
	    	if (dinuovo == 's') 
	    	    
	    	{
	        	printf("\nOK, Rigiochiamo! : )\n\n");
	        	system("cls");
	        	main();
	        
	    	}  
    		SetColor(14);
			   
    	return 0;
}

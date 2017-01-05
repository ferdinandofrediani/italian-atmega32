# Atmega32

Benevenuto sulla guida italiana del microcontrollore **ATMEGA32**, verranno spiegate le parti salienti che permettono la programmazione dell'atmega32 con qualche codice esempio in **Assembly** e in **C** .
La funziona della guida è quella di aiutare un utente alla scoperta del microcontrollore e dunque non si sostituisce ad alcun documento tecnico rilasciato da **ATMEL** .

### Panoramica del microcontrollore

Il microcontrollore è basato su un architettura di tipo **RISC**.
E' composto da **32 registri** a 8 bit tutti collegati direttamente alla **ALU** , 32 KB di **flash programmabile** , 1024 Bytes di **EEPROM** 2 KB di **SRAM** più una miriade di componenti che servono ad interfacciarci con il mondo esterno (ad esempio : A/D converter , TimerCounter , chi più ne ha più ne metta , etcc ) che vedremo step by step successivamente .

## AVR CPU Core

### La ALU e le operazioni

I 32 registri sono collegate direttamente alla **ALU** , questo permette che gran parte delle istruzioni aritmetche siano eseguite in un unico ciclo di **clock** .
Inoltre il processore fa operazioni in little endian.
Le istruzioni aritmetiche che vengono eseguite in 2 cicli di clock sono quelle che utilizzano operandi o coppie di registri per un totale di 16 bit, ad esempio l'istruzione SBIW :

```
;questo è un commento in assembly

sbiw r25:r24,1 ; sottrae 1 dalla coppia di registri r25 e r24

;impiega 2 clicli di clock

```

### Lo Status Register

Lo status register **(SREG)** è un registro di fondamentale importanza del microcontrollore che ci permette di visualizzare il risultato dell' **ultima**  **operazione aritmetica**. Conoscere il risultato di un informazione ci permette di modificare eventualmente il **flusso di programma** . Lo status register è aggiornato dopo ogni operazione della **ALU** .
SREG è composto da 8 bit :  

```

I : bit che attiva gli interrupt

T : le istruzionni BLD e BST usano questo bit come bit di risorsa o destinazione per copiare bit da un **Register File**

H : indica un half carry in operazioni aritmetiche 

S : flag di segno

V : presenza di un overflow in complemento a due

Z : Zero flag

C : Indica un carry che avviene in un operazione aritmetica o logica

```
Sul **8 bit Avr Instruction Set** del Atmel troviamo tutte le istruzioni in assembly supportate dal microcontrollore con la descrizione , per ogni istruzione, delle eventuali modifiche del **SREG**

### Registri per uso generale
Il nostro microcontrollore ha ben **32 general purpose** registrers da **8** bit ciascuno.
Come dicono le parole “general purpose” , tutti i 32 registri sono per  operazioni di “**uso generico**” come un calcolo aritmetico o lo spostamento di eventuali dati dalla memoria alla flash etc..
Però non possiamo usare **indistintamente** tutti i 32 registri con tutte le istruzioni dell’AVR.
Facciamo un esempio:
Una delle istruzioni più usate come la LDI ovvero la Load Immediate funziona solamente dal registro r16 al registro r31.


```

;La LDI 

ldi r17, 0x05 ; carichiamo il 0x05 nel registro R17

```

Nei registri R0 e R1 vengono caricati i risultati delle moltiplicazione e in alcune istruzioni (come la **LPM** ) viene usato R0 come registro di riferimento .
I registri da **R26 a R31** sono dei registri di particolare importanza, poichè le coppie di registri 

```
X -> R26 : R27
Y -> R28 : R29
Z -> R30 : R31

```

permettono di essere utilizzati come registri a **16 bit** . Essendo ogni indirizzo di memoria a **16 bit** , utilizzando queste coppie di registri possiamo scrivere all'interno di uno dei tre puntatori (X,Y,Z) l'indirizzo di una cella di memoria di nostro interesse. Vediamo un esempio :

```
;sia all'indirizzo in ram 0x6A un dato di nostro interesse
;vogliamo dunque prelevarlo

ldi XL , low(0x6A) ;carichiamo la parte bassa dell'indirizzo in XL
ldi XH , high(0x6A) ; carichiamo la parte alta dell'indirizzo in XH
ld r17, X ; carichiamo il valore in memoria alla locazione 0x6A in r17

```
L'utilizzo di questi registri è molto utile soprattutto quando dobbiamo inserire o prelevare ad esempio in **memoria** dati in locazioni *consecutive*.


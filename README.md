# Atmega32

Benevenuto sulla guida italiana del microcontrollore **ATMEGA32**, verranno spiegate le parti salienti che permettono la programmazione dell'atmega32 con qualche codice esempio in **Assembly** e in **C** .
La funziona della guida è quella di aiutare un utente alla scoperta del microcontrollore e dunque non si sostituisce ad alcun documento tecnico rilasciato da **ATMEL** .

### Panoramica del microcontrollore

Il microcontrollore è basato su un architettura RISC.
E' composto da **32 registri** a 8 bit tutti collegati direttamente alla **ALU** , 32 KB di **flash programmabile** , 1024 Bytes di **EEPROM** 2 KB di **SRAM** più una miriade di componenti che servono ad interfacciarci con il mondo esterno (ad esempio : A/D converter , TimerCounter , chi più ne ha più ne metta , etcc ) che vedremo step by step successivamente .

## AVR CPU Core

### La ALU e le operazioni

I 32 registri sono collegate direttamente alla **ALU** , questo permette che gran parte delle istruzioni aritmetche siano eseguite in un unico ciclo di **clock** .
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





## Step 0: Ottieni il tuo bios
Si ritiene necessrio estrarre il bios con UEFITool e con ifrextract, così otterrai il file .txt del tuo bios. In questa guida chiameremo "Bios.txt" (su alcune guide tra cui quella di khronokernel viene convenzionalmente chiamato Setup.txt)

## Step 1: Cerca la voce del valore che vuoi cambiare

Segnati due valori che trovi in quella riga in cui risiede il nome del valore che vuoi cercare:

A. il valore dell'offset:

ad esempio 

`One Of: CFG Lock, VarStoreInfo (VarOffset/VarName): 0x3C`

B. il valore della "VarStore", di solito appena adiacente:

esempio:

`VarStore: 0x3`

## Step 2: Trova il nome della sezione da estrarre


Cerca sotto la sezione "Advanced" di "Bios.txt" la voce "VarStoreId: 0xqualcosa"

con al posto di "qualcosa" metti il valore esadecimale vicino a "VarStore", quindi nel nostro caso 0x3.

Ti servirà di segnarti il nome della sezione corrispondente in cui trovi "VarStoreid: 0xqualcosa", quindi nel nostro caso a Varstoreid 0x3 corrisponde il nome di una sezione.

Se stai cercando nel tuo bios dov'è memorizzato l'offset del CFG Lock, molto probabilmente dopo "VarStoreId: 0xqualcosa" troverai una sezione chiamata "CpuSetup", mentre se cerchi di trovare i valori di DVMT, la sezione dovrebbe chiamarsi "SaSetup"

Si ritiene necessario segnarsi questo "CpuSetup" insieme all'offset preso inizialmente (che non c'entra nulla con il VarStoreId, sono due valori diversi)

## Step 4:

Ora entra in ballo il .exe che viene dal pacchetto ufficiale di InsydeH2O. Aprite command Prompt come amministratore, dirigetevi nella cartella estratta e scrivete:

`.\H2OUVE-W-CONSOLEx64.exe -gv arbitrarionomesezione.txt -n CpuSetup`

Si raccomanda rispettare i caratteri case-sensitive di CpuSetup (o il nome della sezione che volete estrarre), inoltre il parametro "-gv" è possibile ricordarlo pensando a:

`-gv = get variables.`

il tool ora tira fuori il file arbitrarionomesezione.txt dalla sezione CpuSetup (il parametro -n vuole a suo seguito il nome della sezione da estrarre)

Duplicare questo file per avere un backup, rinominatelo in arbitrarionome2sezione.txt

## Step 5: entra in gioco l'offset

Vi ricordate che l'offset all'inizio era 0x3C?

Andate a vedervi la tabella delle coppie di numeri che trovate nel file .txt che avete duplicato in arbitrarionome2sezione.txt e andate alla riga "0x30" (spesso espressa come 00000030, con zeri dietro) e alla colonna "0xC" ricordandovi appunto che C in esadecimale corrisponde a 12, ma fate attenzione, serve iniziare a contare dalla colonna zero, quindi dovrete andare alla colonna tredicesima (n+1)

Se andate a rivedervi il file .txt del bios, dove avete trovato l'offset del valore, di solito ci sta anche scritto quale sia l'impostazione che il produttore ha impostato di default, nella maggior parte dei casi in cui deve essere sbloccato il cfg lock, questo valore corrisponde a 0x1 e quindi è altamente probabile che all'intersezione riga-colonna che ci da l'offset troveremo il valore 01.

Momento cuciale, cambiare quel 01 in 00, quindi state effettivamente cambiando quel valore che di default appunto era su 01, ossia abilitato. 

## Step 6:

Ora serve ricaricare quel .txt nei settaggi del bios, per farlo serve un altro comando.

`.\H2OUVE-W-CONSOLEx64.exe -sv arbitrarionome2sezione.txt -n CpuSetup`

Ricordiamo il parametro

`-sv = set variables`

quindi dopo che hai caricato il .txt modificato, al riavvio hai sbloccato il Cfg Lock. Questo ovviamente puo essere fatto anche per il DVMT ovviamente,
basta fare lo stesso identico procedimento cercando appunto quali siano i "VarStoreId" e andando a modificare la sezione in cui quel VarStoreId risiede e modificare il valore su ascissa e su ordinata in base al valore dell'offset.


Scritta da A23SS4NDRO, credits 

# 5BINF BONOMO ALESSIO ELABORATO 
Un barbiere situato in un appartamento senza reti pre-esistenti, vuole realizzare una nuova infrastruttura di rete che fornisca i seguenti servizi in maniera adeguatamente separata:
schermi dove proiettare video musicali o dimostrativi di prodotti o acconciature
tablet per i clienti per offrire la navigazione in internet, letture di giornali, consultazione cataloghi di prodotti più un servizio innovativo di simulazione dell’acconciatura sovrapposta all'immagine in tempo reale del volto;
tablet per il personale per selezionare le proposte di acconciature da proiettare nel tablet del cliente oppure da utilizzare come specchio virtuale per visualizzare il retro delle acconciature
rete VOIP per telefoni cordless per rispondere ad eventuali prenotazioni telefoniche che è possibile registrare anch’esse nei tablet del personale
una cassa con collegamento internet per la fatturazione automatica periodica presso l’agenzia delle entrate
un POS wireless per il collegamento presso un servizio di pagamento elettronico di una banca
la possibilità di permettere ad un BYOD del cliente di utilizzare internet e i servizi informatici di cui sopra

Il sistema deve permettere la memorizzazione di anagrafica e schede clienti (comprese eventuali allergie), agenda appuntamenti, operazioni di cassa, fidelity card, carte regalo, statistiche economiche e di comportamento, cronologia eventi, campagne promozionali, gestione magazzino, archivio foto, schede tecniche.

Per evitare assembramenti e code all’interno del locale si riceve solo per prenotazione. 

Realizzare il progetto dell’applicazione che gestisce le prenotazioni e le visite implementando in dettaglio un nucleo di operazioni ritenute significative.

Il candidato analizzi la realtà di riferimento e, fatte le opportune ipotesi aggiuntive, individui una soluzione che a suo motivato giudizio sia la più idonea a sviluppare i seguenti punti:
* il dimensionamento del progetto, con numero di utenti stimato e altre quantità significative
* un modello grafico che rappresenti il sistema, ne ponga in evidenza i vari componenti e le loro interconnessioni, motivando le scelte effettuate
* una descrizione, anche utilizzando uno schema grafico, con le funzionalità tecnologiche che dovranno possedere i dispositivi terminali utente e quelli aziendali dislocati *   nei punti strategici dei locali
* l’individuazione dei protocolli di comunicazione da adottare per garantire la sicurezza da attacchi informatici e la resilienza a guasti e malfunzionamenti delle             applicazioni e le relative tecnologie
* la definizione del database del sistema, sotto forma di entità relazione e schema logico 
* il progetto dell’interfaccia grafica, sotto forma di rappresentazione grafica
* l’implementazione di una parte significativa dell'interfaccia grafica dell’applicazione
* l’implementazione del database e dell’applicazione su una macchina locale, on-premises o in cloud
  una stima dei tempi per la realizzazione del progetto, evidenziando la correlazione fra le attività
* Sviluppi in linguaggio SQL delle query che consentano di ottenere le seguenti informazioni:
    * I trattamenti effettuati nell’ultimo mese dalla cliente Angelina Jolie;
    * Tutti i trattamenti più gettonati raggruppati per tipo

* un “executive summary” del progetto redatto in lingua inglese, che presenti il progetto in un’ottica di business.  
## PIANO INDIRIZZAMENTO 
![PIANO INDIRIZZAMENTO](https://raw.githubusercontent.com/alessiobonomo/5binf-elaborato/main/elaboratoIndirizzo.png)

## INFRASTRUTTURA ELABORATO 
![STRUTTURA ELABORATO ](https://raw.githubusercontent.com/alessiobonomo/5binf-elaborato/main/STRUTTURA%20ELABORATO.PNG)

## TABELLE DB
CREATE TABLE Clienti  (
codfiscale VARCHAR(16) NOT NULL PRIMARY KEY,
nome VARCHAR(10) NOT NULL,
cognome VARCHAR(10) NOT NULL,
data_nascita  INT(10) NOT NULL,
indirizzo  VARCHAR(45) NOT NULL,
genere VARCHAR(45) NOT NULL,
allergie  VARCHAR(20) NOT NULL
username varchar(15);


CREATE TABLE appuntamenti (
id_appuntamento INT(30) NOT NULL PRIMARY KEY,
ora TIME NOT NULL,
data DATE NOT NULL,
CodFiscale VARCHAR(16) NOT NULL);

CREATE TABLE operazioni_cassa (
id_operazione INT NOT NULL PRIMARY KEY,
valore_ammontare FLOAT NOT NULL,
descrizione VARCHAR(40) NOT NULL,
data  DATE NOT NULL,
ora TIME NOT NULL,
id_promozione INT NOT NULL,
CodFiscale VARCHAR(16) NOT NULL,
id_Prodotto INT);

CREATE TABLE fidelity_card (
id_Carta INT NOT NULL PRIMARY KEY,
punti INT NOT NULL,
CodFiscale VARCHAR(16) NOT NULL);

CREATE TABLE carta_regalo (
id_CartaR INT NOT NULL PRIMARY KEY,
valore  FLOAT NOT NULL,
id_Operazione INT NOT NULL);

CREATE TABLE Cronologia_eventi  (
id_Evento INT NOT NULL PRIMARY KEY,
data DATE NOT NULL,
ora TIME NOT NULL,
luogo VARCHAR(20) NOT NULL,
CodFiscale VARCHAR(16) NOT NULL);

CREATE TABLE stat_Economiche (
id_stat INT NOT NULL PRIMARY KEY,
preferenze VARCHAR(30) NOT NULL,
necessità  VARCHAR(30) NOT NULL,
id_appuntamento INT NOT NULL);

CREATE TABLE Gestione_Prodotto (
id_prodotto INT NOT NULL PRIMARY KEY,
tipo VARCHAR(20) NOT NULL,
quantità  INT NOT NULL,
descrizione  VARCHAR(45) NOT NULL,
n_scaffale INT NOT NULL);

CREATE TABLE campagne_promozionali (
id_Promozione INT NOT NULL PRIMARY KEY,
tipo VARCHAR(20) NOT NULL,
sconto INT NOT NULL,
costo  FLOAT NOT NULL,
CodFiscale VARCHAR(16) NOT NULL,
id_Prodotto INT NOT NULL);

CREATE TABLE archivio_foto (
id_Foto INT NOT NULL PRIMARY KEY,
data DATE NOT NULL,
ora TIME NOT NULL,
CodFiscale VARCHAR(16) NOT NULL);

CREATE TABLE Scheda_tecnica (
id_scheda  INT NOT NULL PRIMARY KEY,
CodFiscale VARCHAR(16) NOT NULL);

CREATE TABLE operazioni_scheda (
id_operazioneS INT NOT NULL PRIMARY KEY,
id_operazione INT NOT NULL,
id_scheda INT NOT NULL);

CREATE TABLE Utente (
username varchar(15) PRIMARY KEY,
password varchar(100));


ALTER TABLE Clienti ADD  FOREIGN KEY (username) REFERENCES Utente(username);
ALTER TABLE appuntamenti ADD  FOREIGN KEY (CodFiscale) REFERENCES Clienti (codfiscale);
ALTER TABLE operazioni_cassa ADD FOREIGN KEY (id_promozione) REFERENCES campagne_promozionali (id_Promozione);
ALTER TABLE operazioni_cassa ADD  FOREIGN KEY (CodFiscale) REFERENCES Clienti (codfiscale);
ALTER TABLE operazioni_cassa ADD FOREIGN KEY (id_Prodotto) REFERENCES Gestione_Prodotto(id_prodotto);
ALTER TABLE fidelity_card ADD  FOREIGN KEY (CodFiscale) REFERENCES Clienti (codfiscale);
ALTER TABLE carta_regalo ADD FOREIGN KEY (id_Operazione) REFERENCES  operazioni_cassa(id_operazione);
ALTER TABLE Cronologia_eventi  ADD FOREIGN KEY (CodFiscale) REFERENCES Clienti (codfiscale);
ALTER TABLE stat_Economiche ADD FOREIGN KEY (id_appuntamento) REFERENCES appuntamenti(id_appuntamento) ;
ALTER TABLE campagne_promozionali ADD FOREIGN KEY (CodFiscale) REFERENCES Clienti(codfiscale);
ALTER TABLE campagne_promozionali ADD FOREIGN KEY (id_Prodotto) REFERENCES Gestione_Prodotto(id_prodotto);
ALTER TABLE archivio_foto ADD FOREIGN KEY (CodFiscale) REFERENCES Clienti (codfiscale);
ALTER TABLE Scheda_tecnica ADD FOREIGN KEY (CodFiscale) REFERENCES Clienti (codfiscale);
ALTER TABLE operazioni_scheda ADD  FOREIGN KEY (id_operazione) REFERENCES operazioni_cassa(id_operazione);
ALTER TABLE operazioni_scheda ADD FOREIGN KEY (id_scheda) REFERENCES Scheda_tecnica(id_scheda) ;




## QUERY
"I trattamenti effettuati nell’ultimo mese dalla cliente Angelina Jolie"

SELECT C.codFiscale,C.Nome,C.cognome,O.descrizione,A.data

FROM Clienti AS C,operazioni_cassa AS O,Scheda_tecnica AS S,operazioni_scheda AS OS,appuntamenti AS A 

WHERE C.codFiscale=S.codFiscale and S.id_Scheda=OS.id_Scheda and OS.id_operazioneS=O.id_operazione and A.data=2020-04-01



"Tutti i trattamenti più gettonati raggruppati per tipo"

SELECT SE.tipo,SE.preferenze

FROM stat_Economiche SE

GROUP BY tipo,preferenze


## 3 PUNTO ELABORATO 
* una descrizione, anche utilizzando uno schema grafico, con le funzionalità tecnologiche che dovranno possedere i dispositivi terminali utente e quelli aziendali dislocati *   nei punti strategici dei locali

Dispositivi Utenti:

-ACCESS POINT permette l accesso ad internet se collegato ad uno switch;

-TABLET dove i clienti potranno vedere in internet, letture di giornali, cataloghi di prodotti;

-TV dove si proietteranno video su accunciature e prodotti;

Dispositivi dipendenti:

-ACCESS POINT permette l accesso ad internet se collegato ad uno switch;

-Tablet  per le proposte di acconciature da proiettare nel tablet del cliente oppure da utilizzare come specchio,registrare prenotazioni in alternativa al telefono;

-telefoni cordless :per rispondere ad eventuali prenotazioni telefoniche;

-PC-CASSA:FATTURAZIONE PERIODICA;





## Executive summary
A barber located in an apartment without pre-existing networks, wants to create a new network infrastructure that provides certain services, which also represent the objectives:

Screens for projecting music videos or product or hairstyling demonstrations;

-Tablet for customers to offer internet browsing, reading newspapers, consulting product catalogs;

-tablet for staff to select hairstyles proposals to be projected on the client's tablet or to be used as a virtual mirror;

- VOIP network for cordless phones to answer any telephone reservations;
 
- Registration site;

Other solutions have already been proposed in the market to answer this problem: other users have developed sites for online booking, but my solution is better than the others because in the shop we offer the possibility to consult products, newspaper readings, mirrors and more a service innovative simulation of the hairstyle superimposed on the real-time image of the face so the client will be able to see if the hairstyle is suitable for him or not, and this is a particularly significant point because if the customer sees that there is something that other barbers have not, will spread the word and bring other customers.


Guida operativa per l’operatore — Applicazione web REUSE


1 — Panoramica rapida

REUSE è una web app che gestisce il ciclo di valorizzazione degli scarti agroindustriali: dalla ricezione dei lotti fino alla trasformazione, essiccazione, confezionamento e stoccaggio. L’interfaccia è web, accessibile via browser; i dati vengono caricati tramite file Excel ottenuto dall'applicativo SAP aziendale e ogni fase scrive i risultati in file Excel nella cartella uploads/ della web app.

File e cartelle principali (struttura tipica):

flask_app/
	Files:
	app.py			//File principale è app.py, che funge da main e richiama sia lo script di pre-processing sia le pagine html relative alla web app.
	requirements.txt

	Directory:
	__pycache__/
	plots/
	static/
	templates/
	uploads/
	utilis/



2 — Requisiti minimi (per l’operatore)

- PC o tablet con browser moderno (Chrome, Firefox, Edge).

- Connessione alla rete intranet o Internet dove è ospitata l’app.

- Permessi d’accesso ai file Excel con i dati di processo (forniti dall’ufficio di produzione o qualità).

- Per la stampa di etichette: stampante PDF o stampante di rete configurata.



3 — Accesso all’app

Apri il browser e inserisci l’indirizzo dell’app: REUSE-RobertoMaiell.pythonanywhere.com (oppure http://127.0.0.1:5000 se in locale).

Verrai indirizzato alla Home / Index: qui trovi l’area di upload in cui caricare il file Excel di produzione e tre pulsanti principali per il Pre-processing, Mostra Grafici, Storico Upload e al di sotto il flusso operativo caratterizzato da: Recupero → Trasformazione → Stoccaggio.



4 — Primo utilizzo: caricare un file e pre-processing

Dalla home, clicca sul campo "Scegli file" e seleziona il file Excel in locale (estensione .xls o .xlsx).

Premi sul pulsante Pre-processing: l’app salva il file nella cartella uploads/ e avvia lo script python di pre-processing. Il risultato è un file Preprocessed_<nome>.xlsx nella cartella uploads/ con una strutturazione rigosa facilmente utilizzabile dal calcolatore.

Stato atteso: viene mostrata una pagina di preview con la tabella preprocessed.

Nota operativa: il preprocessing normalizza colonne e formati, aggiunge colonne previste dall’app e rende i dati compatibili con le pagine successive.



5 - Report

Dalla home, clicca sul campo "Mostra grafici" e seleziona uno dei presenti file di report. Questi vengono automaticamente generati in base al file caricato nella fase precedente e permettono di avere una visione chiara e immediata di alcune specifiche dei prodotti. 

Ad esempio, il grafico a torta "PIE CHART: Conteggio di INC. %CALO per CATEGORY" mostra, per ogni categoria di prodotto, la percentuale di scarti che va a generare.



6 - Storico

Dalla home, clicca sul campo "Storico Upload" per accedere al database contenente tutti i file caricati e quelli prodotti durante l'elaborazione. E' inoltre possibile scaricare in locale ogni file presente nel db per prenderne visione.



7 Recupero (Rotta: /recupero)

Scopo: registrare i lotti provenienti da SAP (file Quantità_lotti.xlsx).


Azioni:

Scegli la “Materia prima” su cui operare, seleziona uno o più lotti disponibili e automaticamente viene generata la quantità totale.

Premi la conferma per generare un file arricchito da un ID progressivo (LOT-XXX) per identificare il lotto di scarti lavorato.

Risultato: genera/aggiorna Quantità_lotti.xlsx in uploads/.



7.1 Purea (/purea), Cernito (/cernito), Pelatura (/pelatura)

Scopo: fasi di pre-trattamento che ricavano quantità di purea o scarti da rimuovere.

Azioni generali:

Seleziona la Categoria o materia prima e scegli i lotti.

Visualizza i pesi calcolati, l'eventuale quantità di purea o cernito ricavabile e conferma per salvare.

Effetto: i risultati vengono memorizzati nei file di log (es. Quantità_lotti_aggiornata.xlsx).



7.2 Conservazione (/conservazione)

Scopo: gestire quantità refrigerata e durata conservazione.

Interfaccia mostra:

Quantità lavorata,

Quantità residua,

Campo per selezionare quantità da refrigerare e la relativa data di conservazione.

Regole:

Capacità frigorifero: 5000 kg (check automatico). Se la quantità refrigerata supera il limite, il sistema rifiuta il salvataggio.

Salvataggio: clic su “Salva dati” invia i valori a /submit_conservazione che crea o aggiorna i file Excel.



8 Trasformazione (/trasformazione) 

Scopo: trasformare lo scarto in un prodotto semilavorato con una ridotta percentuale di umidità.



8.1 Mescolamento (/mescolamento), Milling (/milling)

Descrizione delle procedure di miscelazione per uniformare gli scarti da destinare all’essiccazione e successiva fase di Milling.



8.2 Essiccazione (/essiccazione)

Azioni:

Selezioni il numero di forni da utilizzare e imposta i rispettivi parametri quali tempo di utilizzo e potenza consumata; il sistema calcola automaticamente la potenza giornaliera consumata e l'energia complessiva impiegata, poi produce la Quantità essiccata (kg) che viene salvata in Quantità_essiccata.xlsx.



9 Stoccaggio (/stoccaggio)

Scopo: definire capacità confezioni (5,10,25,50 kg), calcolare numero di sacchetti e residui (rimanenze), scegliere quantità da refrigerare da rimanenze o valori predefiniti, calcolare report finale (biochar e bioattivi) e guadagno teorico.

Azioni:

Seleziona capacità confezioni dal menu a tendina.

Visualizza il numero di confezioni e il residuo aggiornato.

Clicca su “Mostra etichetta” per visualizzare ID, materia prima, lotto.

Clicca su “Stampa etichetta” per scaricare l’immagine PNG dell’etichetta contenente logo, testo e codice a barre. Il logo è caricato nella cartella uploads/.

Clicca su “Visualizza report finali” per vedere Biochar, Bioattivi, potenza e guadagno.

Premi “Salva dati” per memorizzare tutto in Quantità_stoccaggio.xlsx.



NOTE:
Quando esegui (RUN) app.py, nel terminale compare un link: http://127.0.0.1:5000. 
Lo usi come url nel browser e si apre la web app relativa a REUSE da eseguire in locale. 
Carichi il file grezzo, questo viene preprocessato e cliccando su pre-procesing si ottiene la tabella ottimizzata.
Cliccando su mostra grafici, vengono elencati alcuni grafici disponibili per analizzare alcune caratteristiche da poter monitorare.

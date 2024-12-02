 # Progetto pgAdmin - Gestione Database

Questo progetto è stato creato per esercitarsi con PostgreSQL e pgAdmin 4. Il database gestisce le seguenti entità:

## Entità e Struttura

- **Clienti**: Memorizza informazioni sui clienti.
  - **NumeroCliente** (PK)
  - **Nome**
  - **Cognome**
  - **Anno di Nascita**
  - **Regione di Residenza**

- **Prodotti**: Dettagli sui prodotti.
  - **IdProdotto** (PK)
  - **Descrizione**
  - **InProduzione**
  - **InCommercio**
  - **DataAttivazione**
  - **DataDisattivazione**

- **Fornitori**: Informazioni sui fornitori.
  - **NumeroFornitore** (PK)
  - **Denominazione**
  - **Regione di Residenza**

- **Fatture**: Dati delle fatture emesse.
  - **NumeroFattura** (PK)
  - **Tipologia**
  - **Importo**
  - **Iva**
  - **IdCliente** (FK - Clienti)
  - **DataFattura**
  - **NumeroFornitore** (FK - Fornitori)

## Obiettivi

1. Creazione delle tabelle `Clienti`, `Prodotti`, `Fornitori`, e `Fatture` nel database.
2. Scrittura e esecuzione di query SQL per estrarre informazioni dai dati.
3. Gestione delle relazioni tra tabelle e integrità referenziale.

## Come eseguire il progetto

### 1. Installazione di PostgreSQL e pgAdmin 4
   - Scarica e installa **PostgreSQL** e **pgAdmin 4**.
   - Configura PostgreSQL per l'accesso remoto (se necessario).
   
### 2. Creazione del database
   - Crea un nuovo database in pgAdmin 4.
   - Usa gli script SQL per creare le tabelle e aggiungere i dati.

### 3. Esecuzione delle query
   - Una volta che il database è configurato, puoi eseguire le query per estrarre i dati da tutte le tabelle.

## Query Esempio

### 1. Estrai tutti i clienti con nome "Mario"
```sql
SELECT * FROM Clienti WHERE Nome = 'Mario';

SELECT * FROM Prodotti 
WHERE EXTRACT(YEAR FROM DataAttivazione) = 2017 
AND (InProduzione = true OR InCommercio = true);

SELECT Fatture.NumeroFattura, Fatture.Importo, Fatture.Iva, Clienti.Nome, Clienti.Cognome
FROM Fatture
JOIN Clienti ON Fatture.IdCliente = Clienti.NumeroCliente
WHERE Fatture.Importo < 1000;

SELECT EXTRACT(YEAR FROM DataFattura) AS Anno, COUNT(*) AS NumeroFatture
FROM Fatture
WHERE Iva = 20
GROUP BY EXTRACT(YEAR FROM DataFattura);

SELECT Clienti.RegioneResidenza, SUM(Fatture.Importo) AS TotaleImporti
FROM Fatture
JOIN Clienti ON Fatture.IdCliente = Clienti.NumeroCliente
GROUP BY Clienti.RegioneResidenza;

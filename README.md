# Sanremodata Project Scraper dati
## Creazione di un dataset del festival di Sanremo

Andiamo a scaricare le prime informazioni necessarie, il lavoro svolto permettere di scaricare
3 dataframe-tabelle.

- 1. La prima è una tabella di informazioni riassuntive sui festival prese da wikipedia, informazioni quali la sede dell'evento, l'anno, il presentatore, il numero di partecipanti

- 2. La seconda è un'altra tabella sempre da wikipedia che conterrà i dati di classifica con tutti i partecipanti e le loro posizioni nonchè le canzoni in gara anno per anno

- 3. Da queste due tabelle precedenti risaliamo tramite l'informazioni 'Canzone' ad una terza ed ultima tabella con le informazioni estratte tramite api di spotify sulle canzoni che hanno partecipato a tutti i festival, nome canzone, anno ultimo rilascio, minuti, popolarità, immagine album 

## Cosa ci occorre
- 1. Python e un qualsiasi Ide, vscode, spyder, jupyter
- 2. Alcune librerie di Python
- 3. Registrazione su spotify per le credenziali Api (si trova facilmente guida online step by step)

## Prima tabella informazioni sui festival di Sanremo dal 1951 al 2023

### Python dataset preview
Tutto il codice utilizzato è presente nel file script.ipynb

```python
  # Lista per memorizzare i dati estratti da tutte le pagine
data_list = []
year_start = 1951
year_end = 2024

# Ciclo attraverso gli anni da 1951 a 2023
for year in range(year_start, year_end):
    # Costruisci l'URL per l'anno corrente
    url = f'https://it.wikipedia.org/wiki/Festival_di_Sanremo_{year}'

    # Effettua la richiesta HTTP per ottenere il contenuto della pagina
    response = requests.get(url)
    if response.status_code == 200:
        # Utilizza BeautifulSoup per analizzare l'HTML della pagina
        soup = BeautifulSoup(response.content, 'html.parser')

        # Trova tutti gli elementi <th>
        period_elements = soup.find('th', text='Periodo')
        sede_elements = soup.find('th', text='Sede')
        speaker_elements = soup.find('th', text='Presentatore')
        singers_elements = soup.find('th', text='Partecipanti')
        winner_elements = soup.find('th', text='Vincitore')
        # Estrai il titolo della sede associato a ciascun elemento <th> trovato
        for sede_element in sede_elements:
            # Ottieni il titolo della sede
            if period_elements is not None:
                #singers = singers_elements.get_text(strip=True)
                period = period_elements.find_next('td').get_text(strip=True)
                # Esegui altre operazioni con il testo estratto
            else:
                print(year," - ","Elemento non trovato.")
            sede = sede_element.find_next('a')['title']
            speaker = speaker_elements.find_next('a')['title']
            winner = winner_elements.find_next('a')['title']
            if singers_elements is not None:
                #singers = singers_elements.get_text(strip=True)
                singers = singers_elements.find_next('td').get_text(strip=True)
                # Esegui altre operazioni con il testo estratto
            else:
                print(year," - ","Elemento non trovato.")
            # Aggiungi il titolo della sede e l'anno alla lista dei dati
            data_list.append({'Anno': year,'Periodo': period, 'Sede': sede,'Presentatore': speaker,'Partecipanti': singers,'Vincitore': winner})

# Ora hai una lista di dizionari, ognuno contenente l'anno e la sede estratta da una pagina
# Puoi convertire questa lista in un DataFrame pandas se lo desideri

```

## First XLSX table preview
Anteprima di risultato del prima tabella

| Anno        | formato      | dimensione     | link fonte    | descrizione |
| ------------- |:-------------:| :-------------:|:-------------|:-------------|
|dataset_bank.csv| CSV | 10 | api.revolut.com/bankdata/| Dataset della banca sui clienti che hanno richiesto un credito e risultato del credit score |
| dataset_fiscalagency.json | JSON      |   10 | api.agenziaentrate.gov.it/portale/ | Dataset dell’agenzia delle entrate sui dieci clienti che hanno richiesto il credito |
| dataset_transactions.xml |  XML      |    10 | api.stripe.com/bank/37475859 | Dataset creato da azienda di credito terza su tutti i pagamenti online e offline effettuati |

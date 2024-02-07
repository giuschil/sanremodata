# Sanremodata Project Scraper dati
## Creazione di un dataset del festival di Sanremo

Andiamo a scaricare le prime informazioni necessarie, il lavoro svolto permettere di scaricare
3 dataframe-tabelle
che andremo a scaricare

La prima è una tabella di informazioni riassuntive sui festival prese da wikipedia, informazioni quali la sede dell'evento, l'anno, il presentatore, il numero di partecipanti
La seconda è un'altra tabella sempre da wikipedia che conterrà i dati di classifica con tutti i partecipanti e le loro posizioni nonchè le canzoni in gara anno per anno
Da queste due tabelle precedenti risaliamo tramite l'informazioni 'Canzone' ad una terza ed ultima tabella con le informazioni estratte tramite api di spotify sulle canzoni che hanno partecipato a tutti i festival, nome canzone, anno ultimo rilascio, minuti, popolarità, immagine album 


## Cosa ci occorre
- 1. Python e un qualsiasi Ide, vscode, spyder, jupyter
- 2. Alcune librerie di Python
- 3. Registrazione su spotify per le credenziali Api (si trova facilmente guida online step by step)

## Prima tabella informazioni sui festival di Sanremo dal 1951 al 2023


### Python dataset preview
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

## XLSX table preview
Anteprima di risultato del prima tabella

| Anno        | formato      | dimensione     | link fonte    | descrizione |
| ------------- |:-------------:| :-------------:|:-------------|:-------------|
|dataset_bank.csv| CSV | 10 | api.revolut.com/bankdata/| Dataset della banca sui clienti che hanno richiesto un credito e risultato del credit score |
| dataset_fiscalagency.json | JSON      |   10 | api.agenziaentrate.gov.it/portale/ | Dataset dell’agenzia delle entrate sui dieci clienti che hanno richiesto il credito |
| dataset_transactions.xml |  XML      |    10 | api.stripe.com/bank/37475859 | Dataset creato da azienda di credito terza su tutti i pagamenti online e offline effettuati |

## Seconda tabella informazioni sulle classifiche di Sanremo dal 1951 al 2023

Solo i già clienti possono richiedere un prestito o credito al consumo.
I clienti possono richiedere un credito della durata massima 30 anni, di quantità non superiore a 300.000 €, non è importante sapere il tasso a cui è stato richiesto il prestito.
I clienti possono chiedere un credito per l’acquisto di una car, house, other. I dati forniti dalla banca sono in formato CSV.

## Terza tabella informazioni sulle canzoni presentate a Sanremo dal 1951 al 2023 tramite Spotify
Per tutti Per i clienti (10), identificati da un codice numerico univoco, il codice fiscale, il codice cliente, l’età, il sesso, lo stato civile, il lavoro, se si vive in casa di proprietà o in affitto, la somma che è sul conto, l’ammontare del credito, la duruta del credito richiesto, la decisione del modello.


## CSV dataset preview

|id| fiscal_code | banck_account_number|age|sex|personal_status|job|housing|saving_account|credit_amount|duration|purpose|decision|
| ------------- |:-------------:| :-------------:|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|:-------------|
1|MRARSS90P29H501U|843721|23|M|single|Insegnante|proprietà|30000|25000|5|car|yes|
2|FHHGMS459TOVK595|539889|61|F|single|Ingegnere|affitto|10000|130000|10|house|no|
3|GHCMET56970FKB95|701740|65|F|single|Libero Professionista|affitto|50000|25000|15|car|no|

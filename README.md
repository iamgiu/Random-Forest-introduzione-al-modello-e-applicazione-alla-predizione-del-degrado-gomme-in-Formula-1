# Random Forest come sviluppo del Decision Tree
### Applicazione alla predizione del degrado gomme in Formula 1

Progetto di tesi triennale in Ingegneria Informatica, Università di Genova.
Relatore: Prof. Luca Calatroni. Autrice: Giulia Righi.

La tesi completa (in italiano) è in [`docs/tesi.pdf`](docs/tesi.pdf).

## Obiettivo

Introdurre il modello Random Forest come evoluzione del Decision Tree e applicarlo a un problema reale:
classificare gli stint di gara di Formula 1 in **alto** o **basso degrado** delle gomme (`HighDeg`).
La soglia è la mediana del training set per ogni coppia (Circuito, Compound).

Le feature sono le velocità in qualifica relative alla mediana della gara
(`SpeedST_rel`, `SpeedI1_rel`, `SpeedI2_rel`), usate come proxy del carico aerodinamico.

## Metodologia

- Modelli: Logistic Regression, kNN, Decision Tree, Random Forest (Pipeline con `SimpleImputer` mediana + `StandardScaler`)
- Validazione: `LeaveOneGroupOut` per stagione (2022–2024), `GridSearchCV` con `balanced_accuracy`
  e regola dell'errore standard (*one standard error rule*) per la scelta del modello più semplice
- Test: stagione 2025
- Controlli: baseline `DummyClassifier`, intervallo di confidenza bootstrap per gara, `permutation_test_score`

## Risultati

Nessun modello supera il caso: la balanced accuracy sul test è circa 0.46 e i p-value dei test di
permutazione sono tutti > 0.05. Il risultato è discusso in dettaglio nella tesi.

## Struttura del repository

```
.
├── notebooks/tesi.ipynb   # analisi completa
├── docs/tesi.pdf          # testo della tesi
├── data/                  # qui va f1_dataset.csv (non incluso)
├── requirements.txt
└── README.md
```

## Come eseguire

```bash
python -m venv .venv
source .venv/bin/activate        # su Windows: .venv\Scripts\activate
pip install -r requirements.txt
jupyter notebook notebooks/tesi.ipynb
```

> **Dataset.** Il notebook legge `f1_dataset.csv`, che deve essere collocato in `data/`
> (adatta il percorso nella prima cella se necessario). I dati provengono dalla libreria
> [FastF1](https://docs.fastf1.dev/). Il degrado per stint è la pendenza del tempo sul giro con correzione
> carburante (0.03 s/kg, carico iniziale 110 kg), filtrando giri oltre il 107%, giri con SC/VSC e giri di pit.

## Citazione

G. Righi, *Random Forest come sviluppo del Decision Tree: introduzione al modello e applicazione alla
predizione del degrado gomme in Formula 1*, tesi triennale, Università di Genova.

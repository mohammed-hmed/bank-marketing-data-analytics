# Bank Marketing – Wen soll die Bank zuerst anrufen?

DMI01 / DAN01 Data Analytics, Abschlussprojekt (November 2026)

**Forschungsfrage:** Wie gut lässt sich ein Festgeldabschluss mit den Merkmalen vorhersagen, die vor einem Anruf bekannt sind?

**Daten:** Moro, S., Rita, P., & Cortez, P. (2014). *Bank Marketing* [Dataset]. UCI Machine Learning Repository. https://doi.org/10.24432/C5K306 (CC BY 4.0). Verwendet wird `bank-additional-full.csv` (41.188 Kontakte, 20 Merkmale).

## Aufbau (wie in den Kursnotebooks EDA_0 und Met_1)

```
Bank_Marketing_Projekt/
├── data/
│   ├── raw/bank-additional-full.csv          unveränderte Rohdaten
│   └── processed/bank_marketing_processed.csv Ausgabe von EDA_0
├── notebooks/
│   ├── EDA_0_Bank_Marketing.ipynb             1. Daten laden … 8. Transformieren und Speichern
│   └── Met_1_Klassifikation_Bank_Marketing.ipynb  Baseline, Entscheidungsbaum, Random Forest, logistische Regression
├── images/                                    Abbildungen für Bericht und Folien
├── results/                                   Modellvergleich und Szenarien als CSV
└── requirements.txt
```

## Installation und Start

1. Repository herunterladen: `git clone <Link dieses Repositorys>` oder oben auf „Code“ und dann „Download ZIP“ klicken.
2. Pakete installieren: `pip install -r requirements.txt` (in Anaconda ist fast alles schon da).
3. In `notebooks/` zuerst `EDA_0_Bank_Marketing.ipynb`, dann `Met_1_Klassifikation_Bank_Marketing.ipynb` öffnen.
4. Jeweils Kernel > Restart Kernel and Run All Cells. Met_1 dauert etwa 2 Minuten.

Alle Pfade sind relativ. Zufallszahlen sind mit `random_state=42` festgelegt. Die Zahlen im Bericht stammen aus einem Lauf auf macOS (Anaconda, Python 3.12). Auf einem anderen Rechner können einzelne Werte von Baum und Random Forest in der dritten Nachkommastelle leicht abweichen; die Kernaussagen (Random Forest knapp vorne, Top 20 % = 610 von 928 Abschlüssen) bleiben gleich.

## Wichtigste Festlegungen

- `duration` (Gesprächsdauer) wird entfernt, weil sie erst nach dem Anruf bekannt ist (Data Leakage).
- `pdays = 999` bedeutet „nie kontaktiert“ und wird durch das Merkmal `previously_contacted` ersetzt.
- „unknown“ bleibt eine eigene Kategorie. Ausreißer werden markiert, aber nicht gelöscht.
- Train/Test-Split 80/20 mit `stratify=y` vor jeder lernenden Vorverarbeitung. Die EDA nutzt nur das Training.
- Modellwahl mit `GridSearchCV` und 5-facher Kreuzvalidierung (ROC-AUC) nur im Training.

## Ergebnis

Random Forest: ROC-AUC 0,797 in der Kreuzvalidierung und 0,812 im Test. Ruft die Bank nur die besten 20 % der Liste an, erreicht sie 65,7 % aller Abschlüsse; die Abschlussquote steigt von 11,3 % auf 37,0 % (Lift 3,3). Historische Daten 2008–2010; kein Nachweis für zusätzlichen Umsatz.

## Credits

- **Autor:** Mohammed Ahmed, Digital Business University of Applied Sciences (DBU), Modul DMI01 / DAN01 Data Analytics.
- **Daten:** Moro, S., Rita, P., & Cortez, P. (2014), UCI Machine Learning Repository, Lizenz CC BY 4.0. Die Rohdaten liegen unverändert in `data/raw`.
- **Aufbau:** Struktur der Notebooks nach den Kursbeispielen von D. Ambach (EDA_0_Hotel_booking_data, Met_1_Regressionsbaum).
- **Werkzeuge:** Python, pandas, scikit-learn, matplotlib, seaborn.

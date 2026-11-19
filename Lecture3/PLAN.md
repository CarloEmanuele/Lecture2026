# Lecture 3 — Piano: campionamento temporale, spaziale e di popolazione

Notebook: [exercises_sampling.ipynb](exercises_sampling.ipynb) ·
[solutions_sampling.ipynb](solutions_sampling.ipynb)

## Il filo del discorso

La Lecture 2 riguarda il campionamento delle **molecole**. La Lecture 3 sale di livello e si chiede *cosa*
abbiamo misurato. Ogni dataset è un campione a livelli annidati:

**time point → organoide/batch → sezione → bin/cellula → molecole**

Ogni livello porta varianza e bias propri. La domanda da ripetere è: *a quale livello replico, e a quale
livello voglio generalizzare?*

Ordine delle parti: tempo → spazio → popolazioni. Si chiude sulla pseudoreplicazione e sul disegno
sperimentale, che è il messaggio più pratico per chi farà esperimenti con gli organoidi.

## Esercizi

| # | Titolo | Dati | Messaggio |
|---|---|---|---|
| 1 | Il picco che non c'era | simulazione | Un picco transitorio di ~2 settimane: con 3 time point si vede il 10% della sua altezza; con spaziatura ≥ 20 giorni dipende dalla fortuna |
| 2 | Che età ha questo organoide? | simulazione (stile BrainSpan) | Età di calendario: errore medio ~2,6 PCW; età trascrittomica: ~0,2 PCW. Discussione sulla circolarità |
| 3 | Pseudotempo da uno snapshot | simulazione | PC1 ripiega una traiettoria curva. Se mancano gli stati intermedi, il grafo kNN si sconnette (k piccolo) o prende una scorciatoia (k grande, ρ ≈ 0,5) |
| 4 | Una fetta di organoide | simulazione 3D | 10 rosette nell'organoide: in media se ne vede ~1 per sezione e il 39% delle sezioni non ne mostra nessuna |
| 5a | Bin vicini non sono indipendenti | simulazione | Campo smussato senza differenze: un t-test sui bin dà 77–93% di falsi positivi, sui blocchi ≈ 5–10% |
| 5b | Moran's I | Visium HD (crop della Lecture 2, 16 µm) | I geni con pattern anatomico hanno I fino a ~0,75; dopo lo shuffle I ≈ 0 |
| 6 | Quante cellule servono? | analitico + simulazione | n ≥ ln(1−p)/ln(1−f). Per vedere 10 cellule di una popolazione allo 0,5% ne servono ~3.100 |
| 7a | Effetto composizionale | simulazione | Raddoppia solo il tipo A, ma le proporzioni di B, C e D risultano "significativamente" calate |
| 7b | Cellule o organoidi? | simulazione | Proporzioni: chi-quadro sulle cellule ~74% di falsi positivi, t-test sugli organoidi ≈ 6% |
| 7c | Batch confuso con la condizione | simulazione | Disegno confuso: effetto non identificabile (rango 2). Disegno bilanciato: si stima |
| 7d | Dove sono finiti i neuroni? | simulazione | Il bias di dissociazione porta i neuroni dal 50% al 29%; la correzione richiede di conoscere la cattura |

## Collegamenti

- **Lecture 2:** stesso Visium HD; la pseudoreplicazione (6c) diventa qui il tema centrale (5, 7b).
- **Nico, Lezione 2:** benchmarking time-wise e allineamento con BrainSpan (es. 1–2); raccomandazioni per
  validare i protocolli, cioè quanti organoidi e quanti batch (es. 4, 7).
- **Nico, Lezione 3:** traiettorie (es. 3); composizione dei tipi cellulari dopo il clustering (es. 6–7).

## Da decidere

- Per il 2 si potrebbero usare i dati veri di BrainSpan invece della simulazione: richiede un download.
- Il tema dei dati composizionali (7a) potrebbe diventare un esercizio intero, con CLR, propeller e scCODA.

# Lecture 2 — Piano: segnale e rumore, dal gene singolo alle firme

Notebook: [exercises_signal_noise.ipynb](exercises_signal_noise.ipynb) ·
[solutions_signal_noise.ipynb](solutions_signal_noise.ipynb)

## Collegamento con le lezioni di Nico

- **Lezione 2 (09/10), organoidi e benchmarking:** PCA, correlazione con BrainSpan e WGCNA funzionano perché
  sono dati bulk e moduli di geni, cioè aggregazione. È lo stesso principio degli esercizi 2, 4 e 5.
- **Lezione 3 (12/10), single-cell:** matrice di conteggi (es. 3), identificazione dei tipi cellulari
  (es. 5), espressione differenziale (es. 6b, 6c).

## Il filo del discorso

1. **Bulk, qPCR e ISH** mediano milioni di cellule. Ogni gene è letto con molte molecole e il rumore relativo
   (≈ 1/√λ) è trascurabile: per questo **bastava un gene marcatore**.
2. **Single-cell e Visium HD** leggono una cellula o un bin. Le molecole sono poche, λ < 1 per quasi tutti
   i geni e lo zero è la lettura più probabile anche se il gene è espresso.
   Nel Visium HD di cervello di topo usato a lezione:
   - a 8 µm la mediana è di 156 UMI per bin, su tutti i geni, e solo 5 geni hanno in media almeno 1 conta per bin;
   - a 2 µm la mediana è di 9 UMI per bin e nessun gene arriva a 1 conta in media.
3. Di conseguenza **il singolo gene non regge più**. Bisogna aggregare:
   - geni: firme e moduli, con un rapporto segnale/rumore che cresce circa come √k;
   - spazio: bin più grandi e smoothing;
   - cellule: cluster e pseudobulk.
4. **Precisazione:** in gran parte gli "zeri" non sono un dropout tecnico ma campionamento Poisson/NB
   (Svensson 2020). Il problema è che i conteggi sono pochi, non che la tecnica sia rotta.

## Esercizi

| # | Titolo | Dati | Messaggio |
|---|---|---|---|
| 1 | Quante molecole servono per vedere un gene? | simulazione | Curva di detection 1 − e^(−λ), CV ≈ 1/√λ, geni "leggibili" per profondità, dal bulk al bin da 2 µm |
| 2a | Un marker o una firma? | simulazione | AUC al variare del numero di marker k e della profondità: con poche molecole servono molti geni |
| 2b | Un progenitore ha *Pax6*? | E18 single-cell | ~30% delle cellule più "progenitore" per la firma ha *Pax6* = 0 (la firma non contiene *Pax6*) |
| 3 | Zeri: dropout o campionamento? | E18 single-cell | Zeri osservati contro attesa Poisson/NB: l'eccesso mediano è ≈ 1% |
| 4a | Quante molecole per bin? | Visium HD | Aggregazione 2 → 32 µm fatta dagli studenti; % di zeri di *Slc17a7*, *Mbp*, *Rorb* |
| 4b | Un gene o una firma, nello spazio | Visium HD | *Rorb* a 8 µm è sale e pepe; smussato o come firma diventa la banda dello strato 4/5 |
| 4c | Quanto è riproducibile un bin? | Visium HD | Count splitting: correlazione tra le due metà, gene singolo contro firma, per risoluzione |
| 5 | Le popolazioni vengono dal single-cell | Visium HD | Label transfer con firme da atlanti single-cell: confetti a 8 µm; smoothing → anatomia (ma inventa blob) |
| 6a | Profondità come confondente | E18 | Diradamento binomiale: i conteggi grezzi danno tutto "DE"; conteggi/totale ≈ 1%; il log1p reintroduce la profondità (Lun 2018) |
| 6b | Double dipping | simulazione | Cluster e test sugli stessi dati: ~25% di falsi positivi; con count splitting ≈ 5% |
| 6c | Pseudoreplicazione | simulazione | Cellule come n: ~75% di falsi positivi; pseudobulk per organoide ≈ 5% |
| 6d | Spillover | Visium HD | Profilo di *Ppp1r1b* dallo striato verso la corteccia |

## Dati (in `Datasets/`)

- `e18_mouse_brain_*`: 10x *1k Brain Cells from an E18 Mouse* (v3). 1.301 cellule, geni rilevati in almeno 10 cellule.
- `visiumhd_mouse_brain_cortex_2um.npz`: 10x *Visium HD Mouse Brain* (fresh frozen). Ritaglio di 1,7 × 1,7 mm
  a 2 µm (striato → sostanza bianca → corteccia → pia), 67 geni marcatori più gli UMI totali per bin.
- `visiumhd_mouse_brain_gene_means.csv`: media per bin di ogni gene sull'intera sezione, a 2, 8 e 16 µm.

## Scelte fatte rispetto alla prima bozza

- Al posto della deconvoluzione NNLS ci sono gli score di firma con le firme degli atlanti (Tasic 2018,
  Zeisel 2018, Yao 2021). Il motivo: non c'è un reference single-cell adulto leggero da distribuire. Strumenti
  come RCTD e cell2location vengono citati.
- Il reference single-cell E18 (cervello in sviluppo) è usato per gli esercizi sul single-cell e non per
  annotare la sezione adulta: età diverse non vanno mescolate.

## Se manca tempo

Tenere 1, 2a, 4b, 4c e 6c: portano il messaggio principale.

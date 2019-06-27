# pandoc-vex

>**Note** This pandoc filter is aimed at solving a specific problem related to the drafting of agreements subject to the Italian Law, so the remainder of this README will be in Italian language only.

Filtro per pandoc per creare automaticamente una lista delle clausole vessatorie (da approvare specificamente per iscritto ex artt.1341-2 c.c.) alla fine di un contratto redatto in linguaggio markdown e compilato con pandoc e pandoc-crossref.

## Installazione

>**Nota**: Si presuppone che [pandoc] e [pandoc-crossref] siano già stati installati e che se ne conosca l'utilizzo

1. Installare panflute: `sudo pip3 install panflute`
2. Copiare pandox-vex dentro a una cartella nella PATH e renderlo eseguibile: `sudo cp pandoc-vex /usr/local/bin/ && sudo chmod +x /usr/local/bin/pandoc-vex`

## Sintassi

Le clausole vessatorie vanno contrassegnate con un id univoco con prefisso `sec:` e con l'attributo 'vex', cui basta dare un valore qualsiasi (1, true, ecc.), e alla fine nella formula ex art.1341-2 c.c. basta utilizzare `@vex` al posto dell'elenco delle clausole vessatorie. Ad esempio:

```markdown
---
numberSections: true
sectionsDepth: 3
secHeaderDelim: ".&nbsp;"
secPrefix:
  - "art."
  - "artt."
---

# Miscellanea

## Limitazione di responsabilità {#sec:lim_resp vex=true}

La responsabilità del fornitore per tutte le proprie obbligazioni
previste dal presente contratto sarà limitata, salvo il caso di dolo o colpa
grave, a ...

## Solve et repete {#sec:solve_repete vex=true}

Il cliente dovrà corrispondere i canoni pattuiti...

...

Ai sensi e per gli effetti di cui agli artt.1341-2 c.c., si approvano
specificamente le seguenti clausole: @vex

```

Il testo che precede verrà compilato in questo modo:

>**1. Miscellanea**
>
>**1.1. Limitazione di responsabilità**
>
>La responsabilità del fornitore per tutte le proprie obbligazioni previste dal presente contratto sarà limitata, salvo il caso di dolo o colpa grave, a ….
>
>**1.2. Solve et repete**
>
>Il cliente dovrà corrispondere i canoni pattuiti…
>
>...
>
>Ai sensi e per gli effetti di cui agli artt.1341-2 c.c., si approvano specificamente le seguenti clausole: art. 1.1 (Limitazione di responsabilità), art. 1.2 (Solve et repete)

**IMPORTANTE** Il riferimento incrociato `@vex` va utilizzato esclusivamente alla fine del testo del contratto. Eventuali clausole vessatorie che per qualsiasi motivo si trovassero in un punto successivo del testo verranno ignorate.

## Utilizzo

Va usato in cascata appena prima di pandoc-crossref. Ad esempio:

```
pandoc \
  --filter=pandoc-vex \
  --filter=pandoc-crossref \
  contratto.md -o contratto.docx
```



[pandoc]: https://github.com/jgm/pandoc
[pandoc-crossref]: https://github.com/lierdakil/pandoc-crossref/

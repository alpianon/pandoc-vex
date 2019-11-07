# pandoc-vex

>**Note** This pandoc filter is aimed at solving a specific problem related to the drafting of agreements subject to the Italian Law, so the remainder of this README will be mainly in Italian language.

# English

This is a filter to create one or more lists of clauses in an agreement. Under Italian Law you are required to approve certain clauses in writing, but this could be used to create an arbitrary list of clauses, for instance, sections whose breach would cause immediata termination. It is used in a markdown agreement compiled with  pandoc and pandoc-crossref.

## Install

1. Install panflute: `sudo pip3 install panflute`
2. Copy pandox-vex to a PATH directory and make it executable: `sudo cp pandoc-vex /usr/local/bin/ && sudo chmod +x /usr/local/bin/pandoc-vex`

Syntax: Unique prefix  `sec:` e attribute `vex` *or* `ris` *or both*

Any reference within the text of literal `@vex` and/or `@ris` will be replaced by a list of clauses (number + headings) which are marked with those attributes mentioned above.

**Note**: all relevant headings must be *above* the reference, or they will be missed. So ideally those list must be at the end of the contract.

## Use

Must be in a cascade just before pandoc-crossref, such as in:

```
pandoc \
  --filter=pandoc-vex \
  --filter=pandoc-crossref \
  contratto.md -o contratto.docx
```

# Italiano

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

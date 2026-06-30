# gitmdict

JMdict in JSON format, one file per entry and language — designed for git-friendly diffing and incremental updates.

## Structure

```
metadata.json              — entity map (POS tags, misc tags, etc.)
entries/
  <shard>/
    <seq>.json             — one file per dictionary entry (kanji, kana, sense structure)
translations/
  <lang>/
    <shard>/
      <seq>.json           — one file per entry × language (glosses)
```

Shards are `seq // 10000`, keeping each directory under 1000 files.

## Entry format

```json
{
  "seq": 1358280,
  "kanji": [
    {"text": "食べる", "common": true, "tags": ["ichi1"]},
    {"text": "喰べる", "common": false, "tags": []}
  ],
  "kana": [
    {"text": "たべる", "common": true, "tags": ["ichi1"], "appliesToKanji": ["*"]}
  ],
  "senses": [
    {
      "partOfSpeech": ["v1", "vt"],
      "related": [], "antonym": [], "field": [],
      "dialect": [], "misc": [], "info": [], "languageSource": []
    }
  ]
}
```

`senses` contains structural metadata from the English senses only (POS, cross-references, etc.). Glosses for all languages are in the corresponding translation files.

## Translation format

```json
{
  "seq": 1358280,
  "lang": "eng",
  "glosses": [
    ["to eat"]
  ]
}
```

`glosses` is a list of senses, each sense being a list of gloss strings. The index matches the corresponding sense in the entry file.

## Languages

| Code | Language |
|------|----------|
| `eng` | English |
| `ger` | German |
| `dut` | Dutch |
| `fre` | French |
| `rus` | Russian |
| `hun` | Hungarian |
| `spa` | Spanish |
| `slv` | Slovenian |
| `swe` | Swedish |

## Generation

The JSON files are generated from the JMdict XML file using
[xml-to-git.py](https://github.com/HappyPeng2x/SumatoraIndex/blob/master/xml-to-git.py)
from the [SumatoraIndex](https://github.com/HappyPeng2x/SumatoraIndex) project.

To rebuild a SQLite database from these files, use
[git-to-sqlite.py](https://github.com/HappyPeng2x/SumatoraIndex/blob/master/git-to-sqlite.py).

## Licence

This repository contains data derived from the **JMdict** dictionary file.

> This package uses the JMdict/EDICT dictionary files. These files are
> the property of the Electronic Dictionary Research and Development
> Group, and are used in conformance with the Group's licence.

**Copyright:** James William BREEN and The Electronic Dictionary Research and Development Group  
**Licence:** [Creative Commons Attribution-ShareAlike 4.0 International (CC BY-SA 4.0)](https://creativecommons.org/licenses/by-sa/4.0/)  
**EDRDG licence page:** https://www.edrdg.org/edrdg/licence.html  
**JMdict project:** https://www.edrdg.org/wiki/index.php/JMdict-EDICT_Dictionary_Project

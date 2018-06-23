---
title: Experimenteren - cognitieve Azure-Services | Microsoft Docs
description: Dit artikel is een handleiding voor Azure aangepaste besluit Service experimenteren.
services: cognitive-services
author: marco-rossi29
manager: marco-rossi29
ms.service: cognitive-services
ms.topic: article
ms.date: 05/10/2018
ms.author: marossi
ms.openlocfilehash: b0ac0bc049d556423493f0c48dd9a548929bcd41
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345598"
---
# <a name="experimentation"></a>Experiment

De theorie van de volgende [contextuele bandieten (CB)](https://www.microsoft.com/en-us/research/blog/contextual-bandit-breakthrough-enables-deeper-personalization/), aangepaste besluit Service herhaaldelijk toetsenbordinvoer een context, een actie onderneemt en toetsenbordinvoer van een compensatie voor de gekozen actie. Een voorbeeld is de inhoud personalisatie: Beschrijving van de context van een gebruiker, acties candidate artikelen zijn en de prijs meet hoeveel van de gebruiker het aanbevolen artikel bevallen.

Aangepaste besluit Service produceert een beleid zoals dit uit contexten wordt toegewezen aan acties. Met een specifiek doel-beleid dat u wilt weten van de verwachte derden. Een manier om een schatting maken van de prijs is het een beleidsregel gebruiken om online en laat deze acties (bijvoorbeeld aanbevelen artikelen die voor gebruikers). Deze online evaluatie kan echter zijn kostbare van twee redenen:

* Gebruikers in een beleid niet getest, experimentele worden getoond.
* Deze niet kan worden uitgebreid naar meerdere doel beleidsregels te evalueren.

Evaluatie van het uit het beleid is een alternatieve paradigma. Als u de logboeken van een bestaand online systeem volgen van een beleid voor logboekregistratie hebt, kunt beleidsevaluatie uit schatting maken van de verwachte voordelen van de nieuwe doel-beleidsregels.

Met behulp van het logboekbestand zoekt experimenteren het beleid met de hoogste geschatte, verwacht prijs vinden. Doel beleidsregels zijn parameters gebruikt [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki) argumenten. Het script test in de standaardmodus tal van Vowpal Wabbit argumenten door toe te voegen aan de `--base_command`. Het script voert de volgende handelingen uit:

* Naamruimten van de eerste automatisch detecteert functies `--auto_lines` regels van het bestand voor invoer.
* Voert een eerste vegen uit via de hyper-parameters (`learning rate`, `L1 regularization`, en `power_t`).
* Evaluatie van het beleid wordt getest `--cb_type` (inverse investeringsneiging score (`ips`) of tweemaal robuuste (`dr`). Zie voor meer informatie [contextuele bandieten voorbeeld](https://github.com/JohnLangford/vowpal_wabbit/wiki/Contextual-Bandit-Example).
* Tests randnummers.
* Tests kwadratische interactieve functies:
   * **brute kracht fase**: test alle combinaties met `--q_bruteforce_terms` paren of minder.
   * **greedy fase**: voegt het beste paar totdat er is geen verbetering voor `--q_greedy_stop` afgerond.
* Voert een tweede vegen uit via de hyper-parameters (`learning rate`, `L1 regularization`, en `power_t`).

De parameters waarmee deze stappen omvatten een aantal argumenten Vowpal Wabbit:
- Opties voor gegevensbewerking voorbeeld:
  - gedeelde naamruimten.
  - actie-naamruimten
  - marginale naamruimten
  - kwadratische functies
- Opties voor bijwerken regel
  - leertempo
  - L1 regularisatie
  - t power-waarde

Zie voor een gedetailleerde uitleg van de bovenstaande argumenten [Vowpal Wabbit opdrachtregelargumenten](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments).

## <a name="prerequisites"></a>Vereisten
- Vowpal Wabbit: Geïnstalleerd en op uw pad.
  - Windows: [gebruik de `.msi` installer](https://github.com/eisber/vowpal_wabbit/releases).
  - Andere platforms: [ophalen van de broncode](https://github.com/JohnLangford/vowpal_wabbit/releases).
- Python 3: Geïnstalleerd en op uw pad.
- NumPy: Gebruik de package manager van uw keuze.
- De *mwt-Microsoft-ds* opslagplaats: [kloon de opslagplaats](https://github.com/Microsoft/mwt-ds).
- Besluit Service JSON-logboekbestand: omvat standaard de base opdracht `--dsjson`, waardoor de JSON van de beslissing bij het parseren van het bestand invoergegevens. [Ophalen van een voorbeeld van deze indeling](https://github.com/JohnLangford/vowpal_wabbit/blob/master/test/train-sets/decisionservice.json).

## <a name="usage"></a>Gebruik
Ga naar `mwt-ds/DataScience` en voer `Experimentation.py` met de relevante argumenten, zoals beschreven in de volgende code:

```cmd
python Experimentation.py [-h] -f FILE_PATH [-b BASE_COMMAND] [-p N_PROC]
                          [-s SHARED_NAMESPACES] [-a ACTION_NAMESPACES]
                          [-m MARGINAL_NAMESPACES] [--auto_lines AUTO_LINES]
                          [--only_hp] [-l LR_MIN_MAX_STEPS]
                          [-r REG_MIN_MAX_STEPS] [-t PT_MIN_MAX_STEPS]
                          [--q_bruteforce_terms Q_BRUTEFORCE_TERMS]
                          [--q_greedy_stop Q_GREEDY_STOP]
```

Een logboek van de resultaten wordt toegevoegd aan de *mwt-ds/DataScience/experiments.csv* bestand.

### <a name="parameters"></a>Parameters
| Invoer | Beschrijving | Normaal |
| --- | --- | --- |
| `-h`, `--help` | Help-bericht en afsluiten weergeven. | |
| `-f FILE_PATH`, `--file_path FILE_PATH` | Pad voor de gegevensbestanden (`.json` of `.json.gz` indeling - elke regel is een `dsjson`). | Vereist |  
| `-b BASE_COMMAND`, `--base_command BASE_COMMAND` | Base Vowpal Wabbit-opdracht.  | `vw --cb_adf --dsjson -c` |  
| `-p N_PROC`, `--n_proc N_PROC` | Het aantal parallelle processen te gebruiken. | Logische processors |  
| `-s SHARED_NAMESPACES, --shared_namespaces SHARED_NAMESPACES` | Gedeeld onderdeel naamruimten (bijvoorbeeld `abc` betekent naamruimten `a`, `b`, en `c`).  | Automatische detectie van gegevensbestand |  
| `-a ACTION_NAMESPACES, --action_namespaces ACTION_NAMESPACES` | Naamruimten van de functie in te grijpen. | Automatische detectie van gegevensbestand |  
| `-m MARGINAL_NAMESPACES, --marginal_namespaces MARGINAL_NAMESPACES` | Marginale functie naamruimten. | Automatische detectie van gegevensbestand |  
| `--auto_lines AUTO_LINES` | Aantal regels van de gegevens bestand om te scannen voor functies naamruimten automatisch detecteren. | `100` |  
| `--only_hp` | Sweep alleen via de hyper-parameters (`learning rate`, `L1 regularization`, en `power_t`). | `False` |  
| `-l LR_MIN_MAX_STEPS`, `--lr_min_max_steps LR_MIN_MAX_STEPS` | Het bereik van Learning als positieve waarden `min,max,steps`. | `1e-5,0.5,4` |  
| `-r REG_MIN_MAX_STEPS`, `--reg_min_max_steps REG_MIN_MAX_STEPS` | L1 regularisatie bereik als positieve waarden `min,max,steps`. | `1e-9,0.1,5` |  
| `-t PT_MIN_MAX_STEPS`, `--pt_min_max_steps PT_MIN_MAX_STEPS` | Power_t bereik als positieve waarden `min,max,step`. | `1e-9,0.5,5` |  
| `--q_bruteforce_terms Q_BRUTEFORCE_TERMS` | Het aantal kwadratische paren om te testen in brute-force-fase. | `2` |  
| `--q_greedy_stop Q_GREEDY_STOP` | Rondt zonder verbeteringen, waarna kwadratische greedy zoeken fase is stopgezet. | `3` |  

### <a name="examples"></a>Voorbeelden
De vooraf ingestelde standaardwaarden gebruiken:
```cmd
python Experimentation.py -f D:\multiworld\data.json
```

Oftewel Vowpal Wabbit ook kunt opnemen `.json.gz` bestanden:
```cmd
python Experimentation.py -f D:\multiworld\data.json.gz
```

Getrokken om alleen via de hyper-parameters (`learning rate`, `L1 regularization`, en `power_t`, stoppen na stap 2):
```cmd
python Experimentation.py -f D:\multiworld\data.json --only_hp
```

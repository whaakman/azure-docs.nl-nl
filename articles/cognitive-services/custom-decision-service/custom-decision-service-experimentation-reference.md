---
title: Experimenten - Custom Decision Service
titlesuffix: Azure Cognitive Services
description: In dit artikel is een handleiding voor experimenten met Custom Decision Service.
services: cognitive-services
author: marco-rossi29
manager: cgronlun
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: marossi
ms.openlocfilehash: 90a99d4910b0afb885b415760f6a7ef1ca2aec33
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55219818"
---
# <a name="experimentation"></a>Experiment

De theorie van de volgende [contextuele bandieten (CB)](https://www.microsoft.com/en-us/research/blog/contextual-bandit-breakthrough-enables-deeper-personalization/), Custom Decision Service herhaaldelijk observeert een context, actie onderneemt, en een beloning voor de gekozen actie toetsenbordinvoer. Een voorbeeld is personalisatie van inhoud: Beschrijving van de context van een gebruiker, acties zijn candidate verhalen en de prijs meet hoeveel van de gebruiker het aanbevolen verhaal leuk vinden.

Custom Decision Service maakt een beleid, zoals het van contexten wordt toegewezen aan acties. Met een specifiek doel-beleid, die u wilt weten van de verwachte beloning. Een manier om te schatten van de prijs is het gebruik van een beleid voor online en laat deze acties (bijvoorbeeld aanbevolen artikelen aan gebruikers). Deze online evaluatie kan echter zijn kostbare om twee redenen:

* Gebruikers aan een beleid niet getest, experimentele beschikbaar worden gemaakt.
* Het schalen niet naar meerdere doel beleidsregels te evalueren.

Evaluatie van het uit het beleid is een alternatieve paradigma. Als u logboeken van een bestaand online systeem die volgen op een beleid voor logboekregistratie hebt, kunt beleidsevaluatie uit schatting maken van de verwachte voordelen van de nieuwe doel-beleidsregels.

Met behulp van het logboekbestand, zoekt experimenten te vinden van het beleid met de hoogste geschatte, verwachte prijs. Beleid voor doel bestaan parameters door [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki) argumenten. In de standaardmodus, het script een verscheidenheid aan Vowpal Wabbit argumenten getest door toe te voegen aan de `--base_command`. Het script voert de volgende handelingen uit:

* Naamruimten van de eerste automatisch gedetecteerd functies `--auto_lines` regels van het invoerbestand.
* Een eerste opschoning van hyper-parameters uitvoert (`learning rate`, `L1 regularization`, en `power_t`).
* Evaluatie van het beleid test `--cb_type` (inverse neiging zullen zijn score (`ips`) of dubbel robuuste (`dr`). Zie voor meer informatie, [contextuele bandieten voorbeeld](https://github.com/JohnLangford/vowpal_wabbit/wiki/Contextual-Bandit-Example).
* Tests randnummers.
* Tests kwadratische interactieve functies:
   * **brute-force fase**: Test alle combinaties met `--q_bruteforce_terms` paren of minder.
   * **greedy fase**: Voegt de beste combinatie totdat er is geen verbetering voor `--q_greedy_stop` wordt afgerond.
* Een tweede opschoning van hyper-parameters uitvoert (`learning rate`, `L1 regularization`, en `power_t`).

De parameters waarmee deze stappen bevatten sommige argumenten Vowpal Wabbit:
- Opties voor gegevensbewerking voorbeeld:
  - gedeelde naamruimten
  - actie-naamruimten
  - marginale naamruimten
  - kwadratische functies
- Opties voor de regel bijwerken
  - leertempo
  - L1 regularisatie
  - t-waarde voor energiebeheer

Zie voor een gedetailleerde uitleg van de bovenstaande argumenten [Vowpal Wabbit opdrachtregelargumenten](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments).

## <a name="prerequisites"></a>Vereisten
- Vowpal Wabbit: Geïnstalleerd en op het pad.
  - Windows: [Gebruik de `.msi` installatieprogramma](https://github.com/eisber/vowpal_wabbit/releases).
  - Andere platforms: [Haal de broncode](https://github.com/JohnLangford/vowpal_wabbit/releases).
- Python 3: Geïnstalleerd en op het pad.
- NumPy: Gebruik de package manager van uw keuze.
- De *mwt-Microsoft-ds* opslagplaats: [Kloon de opslagplaats](https://github.com/Microsoft/mwt-ds).
- Besluit Service JSON-logboekbestand: De basis-opdracht bevat standaard `--dsjson`, waardoor Decision Service JSON parseren van het bestand invoergegevens. [Ophalen van een voorbeeld van deze indeling](https://github.com/JohnLangford/vowpal_wabbit/blob/master/test/train-sets/decisionservice.json).

## <a name="usage"></a>Gebruik
Ga naar `mwt-ds/DataScience` en uit te voeren `Experimentation.py` met de relevante argumenten, zoals beschreven in de volgende code:

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
| Invoer | Beschrijving | Standaard |
| --- | --- | --- |
| `-h`, `--help` | Help-bericht en afsluiten weergeven. | |
| `-f FILE_PATH`, `--file_path FILE_PATH` | Pad naar bestand (`.json` of `.json.gz` indeling - elke regel is een `dsjson`). | Vereist |  
| `-b BASE_COMMAND`, `--base_command BASE_COMMAND` | Basis Vowpal Wabbit-opdracht.  | `vw --cb_adf --dsjson -c` |  
| `-p N_PROC`, `--n_proc N_PROC` | Het aantal parallelle processen te gebruiken. | Logische processors |  
| `-s SHARED_NAMESPACES, --shared_namespaces SHARED_NAMESPACES` | Gedeelde functie naamruimten (bijvoorbeeld `abc` betekent naamruimten `a`, `b`, en `c`).  | Automatische detectie van bestand |  
| `-a ACTION_NAMESPACES, --action_namespaces ACTION_NAMESPACES` | Actie functie naamruimten. | Automatische detectie van bestand |  
| `-m MARGINAL_NAMESPACES, --marginal_namespaces MARGINAL_NAMESPACES` | Functie voor marginale naamruimten. | Automatische detectie van bestand |  
| `--auto_lines AUTO_LINES` | Het aantal gegevens bestandsregels om te scannen voor het automatisch detecteren van functies naamruimten. | `100` |  
| `--only_hp` | Zwaaihoek alleen via de hyper-parameters (`learning rate`, `L1 regularization`, en `power_t`). | `False` |  
| `-l LR_MIN_MAX_STEPS`, `--lr_min_max_steps LR_MIN_MAX_STEPS` | Leren werken met het bereik als positieve waarden `min,max,steps`. | `1e-5,0.5,4` |  
| `-r REG_MIN_MAX_STEPS`, `--reg_min_max_steps REG_MIN_MAX_STEPS` | L1 regularisatie bereik als positieve waarden `min,max,steps`. | `1e-9,0.1,5` |  
| `-t PT_MIN_MAX_STEPS`, `--pt_min_max_steps PT_MIN_MAX_STEPS` | Power_t bereik als positieve waarden `min,max,step`. | `1e-9,0.5,5` |  
| `--q_bruteforce_terms Q_BRUTEFORCE_TERMS` | Het aantal kwadratische paren om te testen in brute-force-fase. | `2` |  
| `--q_greedy_stop Q_GREEDY_STOP` | Rondt af zonder verbeteringen, waarna kwadratische greedy zoeken fase is gestopt. | `3` |  

### <a name="examples"></a>Voorbeelden
De vooraf ingestelde standaardwaarden gebruiken:
```cmd
python Experimentation.py -f D:\multiworld\data.json
```

Oftewel Vowpal Wabbit ook kan opnemen `.json.gz` bestanden:
```cmd
python Experimentation.py -f D:\multiworld\data.json.gz
```

Getrokken om alleen via de hyper-parameters (`learning rate`, `L1 regularization`, en `power_t`, stoppen na stap 2):
```cmd
python Experimentation.py -f D:\multiworld\data.json --only_hp
```

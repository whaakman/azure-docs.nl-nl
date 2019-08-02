---
title: Experimenteren-Custom Decision Service
titlesuffix: Azure Cognitive Services
description: Dit artikel is een hand leiding voor experimenten met Custom Decision Service.
services: cognitive-services
author: marco-rossi29
manager: nitinme
ms.service: cognitive-services
ms.subservice: custom-decision-service
ms.topic: conceptual
ms.date: 05/10/2018
ms.author: marossi
ROBOTS: NOINDEX
ms.openlocfilehash: e6e8e7d0d5b969464ba9183ccae9080f58f786a0
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68707287"
---
# <a name="experimentation"></a>Experiment

Na de theorie van [contextuele Bandits (CB)](https://www.microsoft.com/en-us/research/blog/contextual-bandit-breakthrough-enables-deeper-personalization/), Custom decision service herhaaldelijk een context zien, een actie ondernemen en een beloning voor de gekozen actie naleven. Een voor beeld is het personaliseren van inhoud: in de context wordt een gebruiker, acties, kandidaten verhalen beschreven en de beloning meet hoeveel de gebruiker het aanbevolen verhaal vond.

Custom Decision Service produceert een beleid, zoals het toewijst van contexten aan-acties. Met een specifiek doel beleid wilt u weten wat de verwachte beloning is. Een manier om de beloning te ramen, is door een beleid online te gebruiken en acties te laten kiezen (bijvoorbeeld verhalen aan gebruikers te adviseren). Een dergelijke online evaluatie kan echter om twee redenen kostbaar zijn:

* Het stelt gebruikers in staat om een niet-getest, experimenteel beleid te doen.
* Het kan niet worden geschaald om te evalueren van meerdere doel beleidsregels.

Niet-beleids evaluatie is een alternatief paradigma. Als u Logboeken hebt van een bestaand online systeem dat een beleid voor logboek registratie volgt, kan niet-beleids evaluatie de verwachte voor delen van nieuw doel beleid schatten.

Met behulp van het logboek bestand zoekt Experimenteer het beleid met de hoogst geschatte, verwachte beloning. Doel beleidsregels worden para meters door [Vowpal Wabbit](https://github.com/JohnLangford/vowpal_wabbit/wiki) -argumenten. In de standaard modus test het script een aantal Vowpal Wabbit-argumenten door toe te voegen aan `--base_command`de. Het script voert de volgende handelingen uit:

* Automatische detectie van de functie naam ruimten van de `--auto_lines` eerste regels van het invoer bestand.
* Voert een eerste sweep uit via Hyper-para`learning rate`meters ( `power_t`, `L1 regularization`, en).
* Test beleids evaluatie `--cb_type` (inverse neiging score`ips`() of dubbele robuuste`dr`versie (). Zie [contextuele Bandit-voor beeld](https://github.com/JohnLangford/vowpal_wabbit/wiki/Contextual-Bandit-Example)voor meer informatie.
* Marges testen.
* Tests van de kwadratische interactie functies:
   * **Grove fase**: Test alle combi Naties `--q_bruteforce_terms` met paren of minder.
   * **Greedy-fase**: Voegt het beste paar toe totdat er geen verbeteringen voor `--q_greedy_stop` de ronding zijn.
* Voert een tweede sweep uit via Hyper-para`learning rate`meters ( `power_t`, `L1 regularization`, en).

De para meters die deze stappen bepalen, zijn enkele argumenten voor Vowpal Wabbit:
- Voor beelden van opties voor manipulatie:
  - gedeelde naam ruimten
  - actie naam ruimten
  - marginale naam ruimten
  - kwadratische functies
- Opties voor regel bijwerken
  - Leer tempo
  - L1 regularisatie
  - t-waarde voor energie

Zie [Vowpal Wabbit opdracht regel argumenten](https://github.com/JohnLangford/vowpal_wabbit/wiki/Command-line-arguments)voor een gedetailleerde uitleg van de bovenstaande argumenten.

## <a name="prerequisites"></a>Vereisten
- Vowpal Wabbit: Geïnstalleerd en op uw pad.
  - Windows: [Gebruik het `.msi` installatie programma](https://github.com/eisber/vowpal_wabbit/releases).
  - Andere platforms: [Haal de bron code](https://github.com/JohnLangford/vowpal_wabbit/releases)op.
- Python 3: Geïnstalleerd en op uw pad.
- NumPy: Gebruik pakket beheer van uw keuze.
- De *micro soft/mwt-DS-* opslag plaats: [Kloon de opslag plaats](https://github.com/Microsoft/mwt-ds).
- JSON-logboek bestand van de beslissings service: Standaard bevat `--dsjson`de basis opdracht, waarmee u de JSON-parsering van het invoergegevens bestand voor de beslissings service kunt inschakelen. [Een voor beeld van deze indeling weer](https://github.com/JohnLangford/vowpal_wabbit/blob/master/test/train-sets/decisionservice.json)geven.

## <a name="usage"></a>Gebruik
Ga naar `mwt-ds/DataScience` en voer `Experimentation.py` uit met de relevante argumenten, zoals wordt beschreven in de volgende code:

```cmd
python Experimentation.py [-h] -f FILE_PATH [-b BASE_COMMAND] [-p N_PROC]
                          [-s SHARED_NAMESPACES] [-a ACTION_NAMESPACES]
                          [-m MARGINAL_NAMESPACES] [--auto_lines AUTO_LINES]
                          [--only_hp] [-l LR_MIN_MAX_STEPS]
                          [-r REG_MIN_MAX_STEPS] [-t PT_MIN_MAX_STEPS]
                          [--q_bruteforce_terms Q_BRUTEFORCE_TERMS]
                          [--q_greedy_stop Q_GREEDY_STOP]
```

Een logboek van de resultaten wordt toegevoegd aan het bestand *mwt-DS/DataScience/experimenten. CSV* .

### <a name="parameters"></a>Parameters
| Invoer | Description | Standaard |
| --- | --- | --- |
| `-h`, `--help` | Help-bericht weer geven en afsluiten. | |
| `-f FILE_PATH`, `--file_path FILE_PATH` | Pad naar gegevens bestand`.json` ( `.json.gz` of indeling: elke regel is `dsjson`a). | Vereist |  
| `-b BASE_COMMAND`, `--base_command BASE_COMMAND` | Opdracht base Vowpal Wabbit.  | `vw --cb_adf --dsjson -c` |  
| `-p N_PROC`, `--n_proc N_PROC` | Aantal parallelle processen dat moet worden gebruikt. | Logische processors |  
| `-s SHARED_NAMESPACES, --shared_namespaces SHARED_NAMESPACES` | Naam ruimten van gedeelde onderdelen ( `abc` bijvoorbeeld: `a`naam ruimten, `b`en `c`).  | Automatische detectie van gegevens bestand |  
| `-a ACTION_NAMESPACES, --action_namespaces ACTION_NAMESPACES` | Naam ruimten voor actie onderdelen. | Automatische detectie van gegevens bestand |  
| `-m MARGINAL_NAMESPACES, --marginal_namespaces MARGINAL_NAMESPACES` | Marginale functie-naam ruimten. | Automatische detectie van gegevens bestand |  
| `--auto_lines AUTO_LINES` | Aantal regels van het gegevens bestand dat moet worden gescand op automatische detectie van naam ruimten. | `100` |  
| `--only_hp` | Alleen wegvegen via Hyper-para`learning rate`meters ( `power_t`, `L1 regularization`en). | `False` |  
| `-l LR_MIN_MAX_STEPS`, `--lr_min_max_steps LR_MIN_MAX_STEPS` | Leer frequentie bereik als positieve waarden `min,max,steps`. | `1e-5,0.5,4` |  
| `-r REG_MIN_MAX_STEPS`, `--reg_min_max_steps REG_MIN_MAX_STEPS` | L1 regularisatie bereik als positieve waarden `min,max,steps`. | `1e-9,0.1,5` |  
| `-t PT_MIN_MAX_STEPS`, `--pt_min_max_steps PT_MIN_MAX_STEPS` | Power_t bereik als positieve waarden `min,max,step`. | `1e-9,0.5,5` |  
| `--q_bruteforce_terms Q_BRUTEFORCE_TERMS` | Aantal kwadratische paren dat in grove fase moet worden getest. | `2` |  
| `--q_greedy_stop Q_GREEDY_STOP` | Rondt af zonder verbeteringen, waarna de zoek fase van de kwadratische Greedy wordt gestopt. | `3` |  

### <a name="examples"></a>Voorbeelden
De vooraf ingestelde standaard waarden gebruiken:
```cmd
python Experimentation.py -f D:\multiworld\data.json
```

Vowpal Wabbit kan ook `.json.gz` bestanden opnemen:
```cmd
python Experimentation.py -f D:\multiworld\data.json.gz
```

Alleen wegvegen via Hyper-para meters `L1 regularization`(`learning rate`, `power_t`, en stoppen na stap 2):
```cmd
python Experimentation.py -f D:\multiworld\data.json --only_hp
```

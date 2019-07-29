---
title: Zin en uitlijning-aangepaste vertaler
titleSuffix: Azure Cognitive Services
description: Tijdens de uitvoering van de training worden zinnen in parallelle documenten gekoppeld of uitgelijnd. Aangepaste Translator leert vertalingen per zin, door een zin te lezen, de vertaling van deze zin. Vervolgens worden woorden en zinsdelen in deze twee zinnen op elkaar uitgelijnd.
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.date: 02/21/2019
ms.author: swmachan
ms.topic: conceptual
ms.openlocfilehash: e9bc5c876da6bd2be1b22b389b819e51330b2e50
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68595468"
---
# <a name="sentence-pairing-and-alignment-in-parallel-documents"></a>Zin en uitlijning in parallelle documenten

Tijdens de training worden de zinnen in parallelle documenten gekoppeld of uitgelijnd. De aangepaste vertaler rapporteert het aantal zinnen dat kan worden gekoppeld als de uitgelijnde zinnen in elk van de gegevens sets.

## <a name="pairing-and-alignment-process"></a>Proces voor koppelen en uitlijnen

Aangepaste vertalers leren de vertalingen van zinnen per zin. Er wordt een zin van de bron gelezen en vervolgens de vertaling van deze zin vanaf het doel. Vervolgens worden woorden en zinsdelen in deze twee zinnen op elkaar uitgelijnd. Dit proces maakt het mogelijk om een kaart van de woorden en zinsdelen in één zin te maken naar de equivalente woorden en zinsdelen in de vertaling van deze zin. Uitlijning probeert ervoor te zorgen dat de systeem treinen op zinnen met vertalingen van elkaar worden uitgevoerd.

## <a name="pre-aligned-documents"></a>Vooraf uitgelijnde documenten

Als u weet dat u parallelle documenten hebt, kunt u de opmaak van de zin onderdrukken door vooraf uitgelijnde tekst bestanden op te geven. U kunt alle zinnen uit beide documenten uitpakken in een tekst bestand, één zin per regel geordend en uploaden met een `.align` uitbrei ding. De `.align` uitbrei ding signaleert aangepaste vertalers, waarna de uitlijning van de zin moet worden overgeslagen.

Voor de beste resultaten probeert u te controleren of u één zin per regel hebt in uw bestanden. U hebt geen nieuwe regel tekens binnen een zin, omdat dit resulteert in slechte uitlijning.

## <a name="suggested-minimum-number-of-extracted-and-aligned-sentences"></a>Aanbevolen minimum aantal geëxtraheerde en uitgelijnde zinnen

Voor een succes volle training bevat de onderstaande tabel het minimale aantal geëxtraheerde zinnen en uitgelijnde zinnen dat is vereist in elke gegevensset. Het voorgestelde minimale aantal geëxtraheerde zinnen is veel hoger dan het voorgestelde minimum aantal uitgelijnde zinnen dat rekening moet worden gehouden met het feit dat de uitlijning van de zin mogelijk niet alle geëxtraheerde zinnen kan uitlijnen.

| Gegevensset   | Aanbevolen minimum aantal geëxtraheerde zinnen | Aanbevolen minimum aantal ingelijnde zinnen | Maximum aantal uitgelijnde zinnen |
|------------|--------------------------------------------|------------------------------------------|--------------------------------|
| Training   | 10.000                                     | 2,000                                    | Geen bovengrens                 |
| Optimalisatie     | 2,000                                      | 500                                      | 2,500                          |
| Testen    | 2,000                                      | 500                                      | 2,500                          |
| Woordenlijst | 0                                          | 0                                        | Geen bovengrens                 |

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het gebruik van een [woorden lijst](what-is-dictionary.md) in Custom Translator.

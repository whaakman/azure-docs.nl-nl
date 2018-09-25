---
title: Azure Data Explorer-cluster vergroten/verkleinen om te voldoen aan de veranderende vraag
description: Dit artikel wordt beschreven stappen voor het uitbreiden en schalen in een Azure Data Explorer-cluster op basis van veranderende vraag.
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 59a6a94e2906413423a4ae03a7c1c115b2ec0cc0
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47047418"
---
# <a name="manage-cluster-scaling-to-accommodate-changing-demand"></a>Beheren met het cluster schalen om te voldoen aan veranderende vraag

Formaat van een cluster op de juiste wijze is essentieel dat de prestaties van Azure Data Explorer. Maar vraag op een cluster met 100% nauwkeurigheid kan niet worden voorspeld. Een statische clustergrootte kan leiden tot te voorzichtige gebruik of te veel gebruik geen van beide is ideaal. Een betere aanpak is het *schaal* een cluster, toevoegen en capaciteit met het wijzigen van de aanvraag verwijderen. In dit artikel wordt beschreven hoe u voor het beheren van schaalmogelijkheden voor clusters.

Navigeer in uw cluster en klikt u onder **instellingen** Selecteer **uitschalen**. Onder **configureren**, selecteer **automatisch schalen inschakelen**.

![Automatisch schalen inschakelen](media/manage-cluster-scaling/enable-autoscale.png)

De volgende afbeelding ziet u de stroom van de volgende stappen. We bieden hieronder de afbeelding voor meer informatie.

![Schaalregel](media/manage-cluster-scaling/scale-rule.png)

1. Onder **naam van de instelling voor automatisch schalen**, Geef een naam op, zoals *Scale-out: gebruik in de cache*.

1. Onder **schaalmodus**, selecteer **schalen op basis van een metrische waarde**. In deze modus biedt dynamische schaling; u kunt ook selecteren **schalen naar een specifiek aantal instanties**.

1. Selecteer **toevoegen van een regel**.

1. In de **schaalregel** sectie aan de rechterkant, geef waarden op voor elke instelling.

    | Instelling | Beschrijving en waarde |
    | --- | --- | --- |
    | **Tijdverzameling** | Selecteer een aggregatie criteria, zoals **gemiddelde**. |
    | **Naam van de meetwaarde** | Selecteer de metrische gegevens die u wilt dat de schaalbewerking wordt uitgevoerd op, zoals **Cache gebruik**. |
    | **Tijdsintervalstatistieken** | Kiezen tussen **gemiddelde**, **Minimum**, **maximale**, en **som**. |
    | **Operator** | Kies de gewenste optie, zoals **groter is dan of gelijk zijn aan**. |
    | **Drempelwaarde** | Kies de gewenste waarde. 80% is bijvoorbeeld voor het gebruik van de cache, een goed uitgangspunt. |
    | **Duur** | Kies een passende hoeveelheid tijd voor het systeem om te controleren of terug bij het berekenen van metrische gegevens. Beginnen met de standaardwaarde van tien minuten. |
    | **Bewerking** | Kies de gewenste optie om te schalen of uitschalen. |
    | **Aantal exemplaren** | Kies het aantal knooppunten of exemplaren die u wilt toevoegen of verwijderen wanneer een metrische voorwaarde wordt voldaan. |
    | **Afkoelen (minuten)** | Kies een passende tijdsinterval moet worden gewacht tussen schaalbewerkingen. Beginnen met de standaardwaarde van vijf minuten. |
    |  |  |

1. Selecteer **Toevoegen**.

1. In de **Instantielimieten** sectie aan de linkerkant, geef waarden op voor elke instelling.

    | Instelling | Beschrijving en waarde |
    | --- | --- | --- |
    | *Minimum* | Dit is het aantal exemplaren dat uw cluster zal niet worden uitgebreid, ongeacht het gebruik. |
    | *Maximaal* | Dit is het aantal exemplaren dat uw cluster zal niet worden uitgebreid, ongeacht het gebruik. |
    | *Standaard* | Het standaardaantal exemplaren, als er een probleem opgetreden bij het lezen van metrische gegevens voor resources die worden gebruikt. |
    |  |  |

1. Selecteer **Opslaan**.

U hebt nu een scale-out-bewerking voor uw Azure Data Explorer cluster geconfigureerd. Een andere regel voor een bewerking schaal toevoegen. Hierdoor kunnen uw cluster schalen dynamisch op basis van metrische gegevens over Resourcegebruik die u opgeeft.

Als u hulp nodig met het cluster schalen problemen hebt, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com).
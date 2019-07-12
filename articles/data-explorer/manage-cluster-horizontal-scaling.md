---
title: Een Azure Data Explorer-cluster uitbreiden
description: Dit artikel wordt beschreven stappen voor het uitbreiden en schalen in een Azure Data Explorer-cluster op basis van veranderende vraag.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: conceptual
ms.date: 06/30/2019
ms.openlocfilehash: 29bfcc42462a667850f0b2e1bbda3d29cd1597ab
ms.sourcegitcommit: 1e347ed89854dca2a6180106228bfafadc07c6e5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67571515"
---
# <a name="manage-cluster-horizontal-scaling-to-accommodate-changing-demand"></a>Horizontaal schalen van cluster om te voldoen aan veranderende vraag beheren

Formaat van een cluster op de juiste wijze is essentieel dat de prestaties van Azure Data Explorer. Maar vraag op een cluster met absolute nauwkeurigheid kan niet worden voorspeld. Een statische clustergrootte kan leiden tot ondermaats gebruik of overutilization, geen van beide is ideaal.

Een betere aanpak is het *schaal* een cluster, toevoegen en capaciteit met het wijzigen van de aanvraag verwijderen. Er zijn twee werkstromen voor het schalen van: 
* Horizontaal schalen, ook wel genoemd uitschalen en inschalen.
* Verticaal schalen, ook wel omhoog en omlaag schalen.

In dit artikel wordt uitgelegd dat de werkstroom voor horizontaal schalen.

Horizontaal schalen, kunt u het aantal exemplaren automatisch op basis van vooraf gedefinieerde regels en schema's schalen. Geef de instellingen voor automatisch schalen voor uw cluster in Azure portal, zoals beschreven in dit artikel.

## <a name="steps-to-configure-horizontal-scaling"></a>Stappen voor het configureren van horizontaal schalen

In de Azure-portal, gaat u naar uw clusterbron Data Explorer. Onder de **instellingen** kop, selecteer **uitschalen**. 

Selecteer de gewenste functie voor automatisch schalen manier: **Handmatig schalen**, **geoptimaliseerd voor automatisch schalen** of **aangepaste voor automatisch schalen**.

### <a name="manual-scale"></a>Handmatig schalen

Handmatig schalen is de standaardinstelling bij het maken van clusters. Dit betekent dat het cluster heeft de capaciteit van een statische cluster die niet automatisch wordt gewijzigd. U kunt ervoor kiezen de statische capaciteit via de zoekbalk en wordt niet gewijzigd totdat de volgende keer dat u van het cluster scale-out instelling wijzigen.

   ![Methode handmatig schalen](media/manage-cluster-horizontal-scaling/manual-scale-method.png)

### <a name="optimized-autoscale"></a>Geoptimaliseerd voor automatisch schalen

Geoptimaliseerd voor automatisch schalen is de methode aanbevolen voor automatisch schalen. Stappen voor het configureren van geoptimaliseerd voor automatisch schalen:

1. Geoptimaliseerd voor automatisch schalen die optie hebt geselecteerd en kies een ondergrens en bovengrens voor de hoeveelheid exemplaren van het cluster, wordt de automatische schaalaanpassing uitgevoerd tussen deze limieten.
2. Klik op opslaan.

   ![Methode geoptimaliseerd voor automatisch schalen](media/manage-cluster-horizontal-scaling/optimized-autoscale-method.png)

Acties worden zichtbaar in het activiteitenlogboek van het cluster nadat te klikken op Opslaan geoptimaliseerd voor automatisch schalen mechanisme wordt gestart om te werken en het is. Deze methode voor automatisch schalen is de cluster-prestaties en kosten te optimaliseren: als het cluster wordt gestart om te gaan naar een status van ondermaats Gebruik deze zal worden geschaald in verlaten prestaties van de dezelfde en lagere kosten, en als het cluster wordt gestart om te gaan naar een status van overutilization, worden uitgebreid om te controleren of dat deze goed presteert

### <a name="custom-autoscale"></a>Aangepaste voor automatisch schalen

Methode aangepaste voor automatisch schalen kunt u het cluster dynamisch op basis van metrische gegevens die u opgeeft wordt geschaald. De volgende afbeelding ziet u de stroom en de stappen voor het configureren van aangepaste voor automatisch schalen. Meer informatie volgt u de afbeelding.

1. In de **naam van de instelling voor automatisch schalen** Geef een naam, zoals *Scale-out: gebruik in de cache*. 

   ![Schaalregel](media/manage-cluster-horizontal-scaling/custom-autoscale-method.png)

2. Voor **schaalmodus**, selecteer **schalen op basis van een metrische waarde**. In deze modus biedt dynamische schaalbaarheid. U kunt ook selecteren **schalen naar een specifiek aantal instanties**.

3. Selecteer **+ toevoegen van een regel**.

4. In de **schaalregel** sectie aan de rechterkant, geef waarden op voor elke instelling.

    **Criteria**

    | Instelling | Beschrijving en waarde |
    | --- | --- |
    | **Tijdverzameling** | Selecteer een aggregatie criteria, zoals **gemiddelde**. |
    | **Naam van de meetwaarde** | Selecteer de metrische gegevens die u wilt dat de schaalbewerking wordt uitgevoerd op, zoals **Cache gebruik**. |
    | **Tijdsintervalstatistieken** | Kiezen tussen **gemiddelde**, **Minimum**, **maximale**, en **som**. |
    | **Operator** | Kies de gewenste optie, zoals **groter is dan of gelijk zijn aan**. |
    | **Drempelwaarde** | Kies de gewenste waarde. 80 procent is bijvoorbeeld voor het gebruik van de cache, een goed uitgangspunt. |
    | **Duur (in minuten)** | Kies een passende hoeveelheid tijd voor het systeem om te controleren of terug bij het berekenen van metrische gegevens. Beginnen met de standaardwaarde van 10 minuten. |
    |  |  |

    **Actie**

    | Instelling | Beschrijving en waarde |
    | --- | --- |
    | **Bewerking** | Kies de gewenste optie om te schalen of uitschalen. |
    | **Aantal exemplaren** | Kies het aantal knooppunten of exemplaren die u wilt toevoegen of verwijderen wanneer een metrische voorwaarde wordt voldaan. |
    | **Afkoelen (minuten)** | Kies een passende tijdsinterval moet worden gewacht tussen schaalbewerkingen. Beginnen met de standaardwaarde van vijf minuten. |
    |  |  |

5. Selecteer **Toevoegen**.

6. In de **Instantielimieten** sectie aan de linkerkant, geef waarden op voor elke instelling.

    | Instelling | Beschrijving en waarde |
    | --- | --- |
    | **Minimum** | Het aantal exemplaren dat uw cluster wordt niet hieronder, ongeacht het gebruik schalen. |
    | **Maximum** | Het aantal exemplaren dat uw cluster wordt niet, ongeacht het gebruik schalen. |
    | **Standaard** | Het standaardaantal exemplaren. Deze instelling wordt gebruikt als er problemen zijn met het lezen van de metrische gegevens voor resources. |
    |  |  |

7. Selecteer **Opslaan**.

U hebt nu een scale-out-bewerking voor uw Azure Data Explorer cluster geconfigureerd. Een andere regel voor een bewerking schaal toevoegen. Als u hulp nodig hebt bij problemen met een cluster schalen [een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) in Azure portal.

## <a name="next-steps"></a>Volgende stappen

* [De prestaties, de status en het gebruik met metrische gegevens over Azure Data Explorer controleren](using-metrics.md)
* [Verticaal schalen van cluster beheren](manage-cluster-vertical-scaling.md) voor de juiste grootte van een cluster.

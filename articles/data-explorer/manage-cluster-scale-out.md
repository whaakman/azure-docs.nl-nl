---
title: Azure Data Explorer-cluster vergroten/verkleinen om te voldoen aan de veranderende vraag
description: Dit artikel wordt beschreven stappen voor het uitbreiden en schalen in een Azure Data Explorer-cluster op basis van veranderende vraag.
author: orspod
ms.author: orspodek
ms.reviewer: mblythe
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: 9b54bf182f23eceb47c392059ff52c04bf0a8aed
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/01/2019
ms.locfileid: "58755075"
---
# <a name="manage-cluster-scale-out-to-accommodate-changing-demand"></a>Cluster uitbreiden om te voldoen aan veranderende vraag beheren

Formaat van een cluster op de juiste wijze is essentieel dat de prestaties van Azure Data Explorer. Maar vraag op een cluster met absolute nauwkeurigheid kan niet worden voorspeld. Een statische clustergrootte kan leiden tot ondermaats gebruik of overutilization, geen van beide is ideaal.

Een betere aanpak is het *schaal* een cluster, toevoegen en capaciteit met het wijzigen van de aanvraag verwijderen. Er zijn twee werkstromen voor het schalen van: scale-up en scale-out. In dit artikel wordt uitgelegd dat de scale-out-werkstroom.

In dit artikel laat zien hoe voor het beheren van cluster scale-out, ook wel bekend als automatisch schalen. Automatisch schalen kunt u het aantal exemplaren automatisch op basis van vooraf gedefinieerde regels en schema's uitschalen. Geef de instellingen voor automatisch schalen voor uw cluster in Azure portal, zoals beschreven in dit artikel.

Ga naar het cluster. Onder **instellingen**, selecteer **uitschalen**. Onder **configureren**, selecteer **automatisch schalen inschakelen**.

![Automatisch schalen inschakelen](media/manage-cluster-scaling/enable-autoscale.png)

De volgende afbeelding ziet u de stroom van de volgende stappen. Meer informatie vindt u onder de afbeelding.

![Schaalregel](media/manage-cluster-scaling/scale-rule.png)

1. In de **naam van de instelling voor automatisch schalen** Geef een naam, zoals *Scale-out: gebruik in de cache*.

1. Voor **schaalmodus**, selecteer **schalen op basis van een metrische waarde**. In deze modus biedt dynamische schaalbaarheid. U kunt ook selecteren **schalen naar een specifiek aantal instanties**.

1. Selecteer **+ toevoegen van een regel**.

1. In de **schaalregel** sectie aan de rechterkant, geef waarden op voor elke instelling.

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

1. Selecteer **Toevoegen**.

1. In de **Instantielimieten** sectie aan de linkerkant, geef waarden op voor elke instelling.

    | Instelling | Beschrijving en waarde |
    | --- | --- |
    | **Minimum** | Het aantal exemplaren dat uw cluster wordt niet hieronder, ongeacht het gebruik schalen. |
    | **Maximum** | Het aantal exemplaren dat uw cluster wordt niet, ongeacht het gebruik schalen. |
    | **Standaard** | Het standaardaantal exemplaren. Deze instelling wordt gebruikt als er problemen zijn met het lezen van de metrische gegevens voor resources. |
    |  |  |

1. Selecteer **Opslaan**.

U hebt nu een scale-out-bewerking voor uw Azure Data Explorer cluster geconfigureerd. Een andere regel voor een bewerking schaal toevoegen. Deze configuratie kunt uw cluster schalen dynamisch op basis van metrische gegevens die u opgeeft.

U kunt ook [beheren cluster omhoog](manage-cluster-scale-up.md) voor de juiste grootte van een cluster.

Als u hulp nodig hebt bij problemen met een cluster schalen [een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) in Azure portal.

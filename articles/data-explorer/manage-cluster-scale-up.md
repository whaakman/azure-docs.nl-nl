---
title: Azure Data Explorer-cluster vergroten/verkleinen om te voldoen aan de veranderende vraag
description: Dit artikel wordt beschreven stappen voor het omhoog en omlaag schalen een Azure Data Explorer-cluster op basis van veranderende vraag.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 02/18/2019
ms.openlocfilehash: bc3f97c798f5e040908e8103c00d3f015f8c824d
ms.sourcegitcommit: 79038221c1d2172c0677e25a1e479e04f470c567
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/19/2019
ms.locfileid: "56415331"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Cluster omhoog schalen om te voldoen aan veranderende vraag beheren

Formaat van een cluster op de juiste wijze is essentieel dat de prestaties van Azure Data Explorer. Maar vraag op een cluster met 100% nauwkeurigheid kan niet worden voorspeld. Een statische clustergrootte kan leiden tot te voorzichtige gebruik of te veel gebruik geen van beide is ideaal. Een betere aanpak is het *schaal* een cluster, toevoegen en verwijderen van capaciteit en de CPU met het wijzigen van de aanvraag. Er zijn twee werkstromen voor het schalen, scale-up en scale-out. In dit artikel wordt beschreven hoe u voor het beheren van cluster omhoog.

1. Navigeer in uw cluster en klikt u onder **instellingen** Selecteer **omhoog schalen**.

    Krijgt u een lijst met beschikbare SKU's. Bijvoorbeeld, in de afbeelding hieronder is er slechts één beschikbare SKU's: D14_V2.

    ![Omhoog schalen](media/manage-cluster-scale-up/scale-up.png)

    D13_V2 is uitgeschakeld omdat het de huidige SKU van het cluster. N8 en L16 zijn uitgeschakeld omdat ze niet beschikbaar in de regio waar het cluster zich bevindt.

1. Als uw SKU wijzigen, selecteert u de SKU die u wilt gebruiken en druk op de **Selecteer** knop.

> [!NOTE]
> De schaal van proces kan enkele minuten duren en gedurende die tijd is uw cluster, worden opgeschort. Houd er rekening mee dat omlaag schalen schadelijk voor de clusterprestaties van uw zijn kan.

U hebt nu een bewerking omhoog of omlaag schalen voor uw Azure Data Explorer-cluster uitgevoerd. U kunt ook doen [cluster van uitbreidbare](manage-cluster-scale-out.md), ook wel aangeduid als automatisch schalen om te schalen dynamisch op basis van metrische gegevens die u opgeeft.

Als u hulp nodig met het cluster schalen problemen hebt, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).

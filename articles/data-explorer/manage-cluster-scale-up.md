---
title: Azure Data Explorer-cluster vergroten/verkleinen om te voldoen aan de veranderende vraag
description: Dit artikel wordt beschreven stappen voor het uitbreiden en schalen in een Azure Data Explorer-cluster op basis van veranderende vraag.
author: radennis
ms.author: radennis
ms.reviewer: v-orspod
ms.service: data-explorer
services: data-explorer
ms.topic: conceptual
ms.date: 01/31/2019
ms.openlocfilehash: 213a49d87d5e9f801bb17604a322c231a4e3dee2
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55735814"
---
# <a name="manage-cluster-scale-up-to-accommodate-changing-demand"></a>Cluster schalen tot beheren voldoen aan de veranderende vraag

Formaat van een cluster op de juiste wijze is essentieel dat de prestaties van Azure Data Explorer. Maar vraag op een cluster met 100% nauwkeurigheid kan niet worden voorspeld. Een statische clustergrootte kan leiden tot te voorzichtige gebruik of te veel gebruik geen van beide is ideaal. Een betere aanpak is het *schaal* een cluster, toevoegen en verwijderen van capaciteit en de CPU met het wijzigen van de aanvraag. In dit artikel leest u hoe voor het beheren van cluster scale up.

Navigeer in uw cluster en klikt u onder **instellingen** Selecteer **omhoog schalen**.

U zou vervolgens ontvangen de lijst met SKU's die beschikbaar zijn. U kunt kiezen uit de lijst met ingeschakelde kaarten. Bijvoorbeeld in de onderstaande afbeelding is slechts één SKU die u kunt uit D14_vs kiezen.

![Omhoog schalen](media/manage-cluster-scale-up/scale-up.png)

D13_v2 is uitgeschakeld omdat dit de huidige SKU van het cluster is. N8 en L16 zijn uitgeschakeld omdat ze niet beschikbaar in de regio waar het cluster is.

Als u wilt een SKU Klik de SKU wijzigen, u wilt gebruiken en klik op de **Selecteer** knop.

[!NOTE] De schaal van proces kan enkele minuten duren en gedurende die tijd is uw cluster, worden opgeschort. Houd er rekening mee dat omlaag schalen schadelijk voor de clusterprestaties van uw zijn kan.

Als u hulp nodig met het cluster schalen problemen hebt, opent u een ondersteuningsaanvraag in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
---
title: Hoge beschikbaarheid en herstel na noodgevallen - Azure Batch | Microsoft Docs
description: Informatie over het ontwerpen van uw Batch-toepassing voor een regionale onderbreking
services: batch
documentationcenter: ''
author: laurenhughes
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2019
ms.author: lahugh
ms.openlocfilehash: b863785575263fedd144b3d599962a8e1559e0a3
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55252382"
---
# <a name="design-your-application-for-high-availability"></a>Een toepassing voor hoge beschikbaarheid ontwerpen

Azure Batch is een regionale service. Batch is beschikbaar in alle Azure-regio's, maar als u een Batch-account maakt het moet worden gekoppeld aan een regio. Alle bewerkingen voor het Batch-account wordt vervolgens toepassen voor deze regio. Bijvoorbeeld, worden pools en de bijbehorende virtuele machines (VM's) gemaakt in dezelfde regio als het Batch-account.

Bij het ontwerpen van een toepassing die gebruikmaakt van Batch, moet u rekening houden met de mogelijkheid van Batch is niet beschikbaar worden gesteld in een regio. Het is mogelijk een zeldzame situatie voordoen wanneer er een probleem met de regio als geheel, de hele Batch-service in de regio of een probleem met uw specifieke Batch-account.

Als de toepassing of oplossing met behulp van Batch altijd beschikbaar moet zijn, dan moet worden ontworpen voor een failover naar een andere regio of beschikt altijd over de werkbelasting verdeeld tussen twee of meer regio's. Beide benaderingen vereist ten minste twee Batch-accounts, met elk account zich in een andere regio.

## <a name="multiple-batch-accounts-in-multiple-regions"></a>Meerdere Batch-accounts in meerdere regio 's

Met behulp van meerdere Batch-accounts in verschillende regio's biedt de mogelijkheid voor uw toepassing worden uitgevoerd als een Batch-account in een andere regio niet beschikbaar. Met behulp van meerdere accounts is vooral belangrijk als de toepassing nodig heeft maximaal beschikbaar.

In sommige gevallen kan een toepassing altijd gebruik van twee of meer regio's zijn ontworpen. Bijvoorbeeld, wanneer u een aanzienlijke hoeveelheid capaciteit nodig hebt, met behulp van meerdere regio's kan nodig zijn voor een grootschalige toepassing verwerken of geschikt zijn voor toekomstige groei.

## <a name="design-considerations-for-providing-failover"></a>Ontwerpoverwegingen voor het leveren van failover

Een belangrijk punt om te overwegen bij het opgeven van de mogelijkheid om failover naar een andere regio is dat alle onderdelen in een oplossing worden overwogen moeten; het is niet voldoende als gewoon een tweede Batch-account. Bijvoorbeeld, zijn in de meeste Batch-toepassingen, Azure storage-account is vereist, met de storage-account en de Batch-account dat u hoeft te worden in dezelfde regio voor aanvaardbare prestaties.

Houd rekening met de volgende punten bij het ontwerpen van een oplossing die u kunt een failover:

- Vooraf maken alle vereiste accounts in elke regio, zoals de Batch-account en storage-account. Er vaak is geen kosten in rekening gebracht voor accounts die zijn gemaakt, alleen als er gegevens die zijn opgeslagen of het account dat wordt gebruikt.
- Zorg ervoor dat quota worden ingesteld op de accounts tevoren, zodat u het vereiste aantal kernen met behulp van de Batch-account kunt toewijzen.
- Sjablonen en/of scripts gebruiken om de implementatie van de toepassing in een regio te automatiseren.
- Binaire waarden van toepassingen en referentiegegevens up-to-date houden in alle regio's. Up-to-date blijven zorgt ervoor dat de regio kan online worden gebracht snel zonder te wachten op het uploaden en de implementatie van bestanden. Bijvoorbeeld, als een aangepaste toepassing te installeren op de pool is opgeslagen en waarnaar wordt verwezen met behulp van Batch-toepassingspakketten, klikt u vervolgens wanneer een nieuwe versie van de toepassing wordt gemaakt, moet worden geüpload naar elk Batch-account en waarnaar wordt verwezen door de configuratie van de groep (of de nieuwe versie maken de standaardversie).
- In de toepassing het aanroepen van Batch, opslag, en andere services, eenvoudig overschakeling clients of de belasting naar de andere regio.
- Er is een best practice om te controleren of dat een failover kan tot stand worden te vaak overschakeling naar een andere regio als onderdeel van de normale werking. Bijvoorbeeld, met twee implementaties in verschillende regio's, overschakelen naar de andere regio elke maand.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het maken van Batch-accounts waarvoor de [Azure-portal](batch-account-create-portal.md), wordt de [Azure CLI](cli-samples.md), [Powershell](batch-powershell-cmdlets-get-started.md), of de [Batch-API voor beheer](batch-management-dotnet.md).
- Standaardquota zijn gekoppeld aan een Batch-account; [in dit artikel](batch-quota-limit.md) details het standaardquotum waarden en wordt beschreven hoe de quota kunnen worden verhoogd.

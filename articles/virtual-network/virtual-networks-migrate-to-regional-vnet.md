---
title: Migreren van een Azure-netwerk (klassiek) van een affiniteitsgroep naar een regio | Microsoft Docs
description: Informatie over het migreren van een virtueel netwerk (klassiek) van een affiniteitsgroep naar een regio.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: genli
ms.openlocfilehash: d3bb93d12a217e6d9066d037ff92f071b6139ab3
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "60648632"
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-to-a-region"></a>Migreren van een virtueel netwerk (klassiek) van een affiniteitsgroep naar een regio

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Resource Manager-implementatiemodel gebruiken.

Affiniteitsgroepen ervoor te zorgen dat resources die zijn gemaakt in dezelfde affiniteitsgroep fysiek op servers die zich dicht bij elkaar, zodat deze resources om te communiceren sneller worden gehost. In het verleden waren affiniteitsgroepen vereist voor het maken van virtuele netwerken (klassiek). Op dat moment kan de network manager-service die virtuele netwerken (klassiek) worden beheerd uitsluitend worden gebruikt in een set van fysieke servers of schaaleenheid. Architectuur verbeteringen zijn het bereik van beheer van het netwerk naar een regio toegenomen.

Als gevolg van deze architectuur verbeteringen zijn affiniteitsgroepen niet langer aanbevolen of vereist zijn voor virtuele netwerken (klassiek). Het gebruik van affiniteitsgroepen voor virtuele netwerken (klassiek) wordt vervangen door de regio's. Virtuele netwerken (klassiek) die gekoppeld aan regio's zijn, regionale virtuele netwerken worden genoemd.

U wordt aangeraden dat u affiniteitsgroepen in het algemeen niet gebruikt. Afgezien van de vereiste virtuele netwerk affiniteitsgroepen zijn ook belangrijk om te gebruiken om te controleren of de resources, zoals compute (klassiek) en storage (klassiek), in de buurt van elkaar zijn geplaatst. Met de architectuur van de huidige Azure-netwerk zijn niet langer deze plaatsing vereisten die nodig zijn.

> [!IMPORTANT]
> Hoewel het is nog steeds technisch mogelijk te maken van een virtueel netwerk dat is gekoppeld aan een affiniteitsgroep, is er geen dwingende redenen om dit te doen. Veel functies van virtual network, zoals netwerkbeveiligingsgroepen, zijn alleen beschikbaar wanneer u een regionaal virtueel netwerk, en zijn niet beschikbaar voor virtuele netwerken die gekoppeld aan affiniteitsgroepen zijn.
> 
> 

## <a name="edit-the-network-configuration-file"></a>Het netwerkconfiguratiebestand bewerken

1. Het netwerkconfiguratiebestand exporteert. Zie voor informatie over het exporteren van een netwerkconfiguratiebestand met PowerShell of de Azure-opdrachtregelinterface (CLI) 1.0, [configureren van een virtueel netwerk met een netwerkconfiguratiebestand](virtual-networks-using-network-configuration-file.md#export).
2. Bewerken van het netwerkconfiguratiebestand vervangen **AffinityGroup** met **locatie**. Opgeven van een Azure [regio](https://azure.microsoft.com/regions) voor **locatie**.
   
   > [!NOTE]
   > De **locatie** is de regio die u hebt opgegeven voor de affiniteitsgroep die is gekoppeld aan het virtuele netwerk (klassiek). Bijvoorbeeld, als uw virtuele netwerk (klassiek) is gekoppeld aan een affiniteitsgroep die zich in VS-West, wanneer u migreert, uw **locatie** moet verwijzen naar VS-West. 
   > 
   > 
   
    De volgende regels in het configuratiebestand van uw netwerk, vervang de waarden door uw eigen bewerken: 
   
    **Oude waarde:** \<VirtualNetworkSitename="VNetUSWest" AffinityGroup="VNetDemoAG"\> 
   
    **Nieuwe waarde:** \<VirtualNetworkSitename="VNetUSWest" Location="West US"\>
3. Sla uw wijzigingen en [importeren](virtual-networks-using-network-configuration-file.md#import) de netwerkconfiguratie naar Azure.

> [!NOTE]
> Deze migratie leidt niet tot uitvaltijd voor uw service.
> 
> 

## <a name="what-to-do-if-you-have-a-vm-classic-in-an-affinity-group"></a>Wat te doen als u een virtuele machine (klassiek) in een affiniteitsgroep hebt
(Klassiek), die momenteel zijn opgenomen in een affiniteitsgroep VM's hoeft te worden verwijderd uit de affiniteitsgroep. Wanneer een virtuele machine is geïmplementeerd, wordt deze geïmplementeerd op een enkele schaaleenheid. Affiniteitsgroepen kunt beperken de set beschikbare VM-grootten voor een nieuwe VM-implementatie, maar elke bestaande virtuele machine die wordt geïmplementeerd is al beperkt tot het instellen van VM-grootten die beschikbaar zijn in de schaaleenheid waarin de virtuele machine is geïmplementeerd. Omdat de virtuele machine al is geïmplementeerd op een schaaleenheid, heeft een virtuele machine verwijderen uit een affiniteitsgroep geen effect op de virtuele machine.

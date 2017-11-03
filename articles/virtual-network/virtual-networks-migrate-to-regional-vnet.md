---
title: Een Azure-netwerk (klassiek) migreren van een affiniteitsgroep naar een regio | Microsoft Docs
description: Informatie over het migreren van een virtueel netwerk (klassiek) van een affiniteitsgroep naar een regio.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 84febcb9-bb8b-4e79-ab91-865ad9de41cb
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.openlocfilehash: b9b3bd0f2184ac85261166d5fe2ab67e1bf319d4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-a-virtual-network-classic-from-an-affinity-group-to-a-region"></a>Een virtueel netwerk (klassiek) migreren van een affiniteitsgroep naar een regio

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en classic](../resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. Microsoft raadt aan dat de meeste nieuwe implementaties het Resource Manager-implementatiemodel gebruiken.

Affiniteitsgroepen Zorg ervoor dat resources gemaakt binnen dezelfde affiniteitsgroep fysiek op servers die zich dicht bij elkaar inschakelen van deze bronnen te laten communiceren sneller worden gehost. In het verleden waren affiniteitsgroepen een vereiste voor het maken van virtuele netwerken (klassiek). Op dat moment werken manager met de netwerkservice die virtuele netwerken (klassiek beheerde) kan alleen binnen een set van fysieke servers of schaaleenheid. Architectuur verbeteringen zijn het bereik van netwerkbeheer in een regio toegenomen.

Als gevolg van deze architectuur verbeteringen zijn affiniteitsgroepen niet langer aanbevolen of vereist zijn voor virtuele netwerken (klassiek). Het gebruik van affiniteitsgroepen voor virtuele netwerken (klassiek) wordt vervangen door de regio's. Regionale virtuele netwerken, virtuele netwerken (klassiek) die gekoppeld aan de regio's zijn worden genoemd.

Het is raadzaam dat u in het algemeen affiniteitsgroepen niet gebruikt. Naast de vereiste virtueel netwerk affiniteitsgroepen is ook belangrijk om te gebruiken om ervoor te zorgen resources, zoals berekeningen (klassiek) en opslag (klassiek), in de buurt van elkaar zijn geplaatst. Met de huidige architectuur van de Azure-netwerk zijn niet langer deze vereisten plaatsing vereist.

> [!IMPORTANT]
> Hoewel het technisch nog steeds mogelijk te maken van een virtueel netwerk dat is gekoppeld aan een affiniteitsgroep, is er geen dwingende redenen om dit te doen. Veel functies van virtueel netwerk, zoals netwerkbeveiligingsgroepen, zijn alleen beschikbaar wanneer u een regionaal virtueel netwerk en zijn niet beschikbaar voor virtuele netwerken die gekoppeld aan affiniteitsgroepen zijn.
> 
> 

## <a name="edit-the-network-configuration-file"></a>De netwerk-configuratiebestand bewerken

1. Exporteer het configuratiebestand van het netwerk. Zie voor meer informatie over het exporteren van een netwerk-configuratiebestand met PowerShell of de Azure-opdrachtregelinterface (CLI) 1.0, [configureren van een virtueel netwerk met een netwerk-configuratiebestand](virtual-networks-using-network-configuration-file.md#export).
2. Bewerk het bestand van de configuratie van netwerk vervangen **AffinityGroup** met **locatie**. Geef van een Azure [regio](https://azure.microsoft.com/regions) voor **locatie**.
   
   > [!NOTE]
   > De **locatie** de regio die u hebt opgegeven voor de affiniteitsgroep die is gekoppeld aan het virtuele netwerk (klassiek). Als het virtuele netwerk (klassiek) is gekoppeld aan een affiniteitsgroep die zich in VS-West, wanneer u migreert, bijvoorbeeld uw **locatie** moet verwijzen naar VS-West. 
   > 
   > 
   
    De volgende regels in het configuratiebestand van uw netwerk, waarbij de waarden vervangt door uw eigen bewerken: 
   
    **Oude waarde:** \<VirtualNetworkSitename = 'VNetUSWest' AffinityGroup = "VNetDemoAG"\> 
   
    **Nieuwe waarde:** \<VirtualNetworkSitename = "VNetUSWest" locatie 'VS-West' =\>
3. Uw wijzigingen hebt opgeslagen en [importeren](virtual-networks-using-network-configuration-file.md#import) de netwerkconfiguratie in Azure.

> [!NOTE]
> Deze migratie veroorzaakt geen uitvaltijd voor uw service.
> 
> 

## <a name="what-to-do-if-you-have-a-vm-classic-in-an-affinity-group"></a>Wat te doen als u een VM (klassiek) in een affiniteitsgroep hebt
VMs (klassiek) die momenteel in een affiniteitsgroep hoeft niet worden verwijderd uit de affiniteitsgroep. Zodra een virtuele machine is geïmplementeerd, wordt deze op een enkele schaaleenheid geïmplementeerd. Affiniteitsgroepen kunt Beperk het aantal beschikbare VM-grootten voor een nieuwe VM-implementatie, maar er is al een bestaande virtuele machine die wordt geïmplementeerd beperkt tot de groep van VM-formaten is beschikbaar in de schaaleenheid waarin de virtuele machine wordt geïmplementeerd. Omdat de virtuele machine is al geïmplementeerd op een schaaleenheid, heeft een virtuele machine verwijderen uit een affiniteitsgroep geen effect op de virtuele machine.

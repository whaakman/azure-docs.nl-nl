---
title: Beheren van updates in de Azure-Stack overzicht | Microsoft Docs
description: "Meer informatie over updatebeheer voor Azure-Stack geïntegreerd systemen."
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 9b0781f4-2cd5-4619-a9b1-59182b4a6e43
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/25/2017
ms.author: mabrigg
ms.openlocfilehash: 23b05909bda7785b45aeaeed0bd75a90de9ffe50
ms.sourcegitcommit: 0e1c4b925c778de4924c4985504a1791b8330c71
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/06/2018
---
# <a name="manage-updates-in-azure-stack-overview"></a>Beheren van updates in de Azure-Stack-overzicht

*Van toepassing op: Azure Stack geïntegreerd systemen*

Microsoft brengt updatepakketten voor Azure-Stack geïntegreerd systemen een reguliere uitgebracht die doorgaans op de vierde dinsdag van elke maand vallen begint bij algemene beschikbaarheid. Vraag uw OEM over hun specifieke notification-proces om ervoor te zorgen update meldingen reach voor uw organisatie of schakel dit selectievakje onder Overview\Release Notes\Integrated systemen release-opmerkingen voor meer informatie over specifieke releases.

Elke versie van Microsoft software-updates, wordt meegeleverd als één update-pakket. Als een Azure-Stack-operator, kunt u eenvoudig importeren, updatepakketten installeren en de voortgang van de installatie van deze monitor uit de beheerdersportal. 

Uw oorspronkelijke leveranciers (OEM) hardwareleverancier brengt ook updates, zoals stuurprogramma en firmware-updates. Deze updates als afzonderlijke pakketten worden geleverd door de leverancier van de OEM-hardware, en worden afzonderlijk beheerd van Microsoft updates.

Om te houden van uw systeem ondersteuning, moet u Azure Stack bijgewerkt naar een specifieke versie bewaren. Zorg ervoor dat u wordt aangeraden de [Azure Stack beleid onderhoud](azure-stack-servicing-policy.md).

> [!NOTE]
> U kunt Azure-Stack-updatepakketten niet toepassen op Azure Stack Development Kit. De updatepakketten zijn ontworpen voor geïntegreerde systemen.

## <a name="the-update-resource-provider"></a>De Update-resourceprovider

Azure Stack bevat een Update-resourceprovider die de toepassing van Microsoft software-updates ingedeeld. Deze resourceprovider zorgt ervoor dat er updates worden toegepast op alle fysieke hosts, Service Fabric-toepassingen en runtimes, en alle virtuele machines van de infrastructuur en hun bijbehorende services.

Tijdens updates worden geïnstalleerd, kunt u eenvoudig statuswaarde op hoog niveau weergeven als de update proces niet de verschillende subsystemen in Azure-Stack (bijvoorbeeld fysieke hosts en virtuele machines van infrastructuur).

## <a name="plan-for-updates"></a>Updates plannen

Wordt aangeraden dat u gebruikers van elke onderhoudsbewerkingen waarschuwen en dat u plant dat normale onderhoudsvensters tijdens buiten kantooruren zo veel mogelijk. Onderhoud kunnen zowel tenantwerkbelastingen als portal operations beïnvloeden.

## <a name="using-the-update-tile-to-manage-updates"></a>Met behulp van de tegel Update om updates te beheren
Het beheren van updates via de beheerdersportal is een eenvoudig proces. Een Azure-Stack-operator kunt navigeren naar de Update-tegel in het dashboard om:

- belangrijke informatie zoals de huidige versie weergeven.
- updates installeren en de voortgang controleren.
- Historie van updates voor eerder geïnstalleerde updates bekijken.
 
## <a name="determine-the-current-version"></a>De huidige versie niet vaststellen

De Update-tegel ziet u de huidige versie van Azure-Stack. U kunt op de tegel Update krijgen met behulp van een van de volgende methoden in de beheerdersportal:

- Bekijk op het dashboard, de huidige versie in de **Update** tegel.
 
   ![Updates op standaarddashboard tegel](./media/azure-stack-updates/image1.png)
 
- Op de **regio management** tegel, klikt u op de regionaam van de. De huidige versie in de **Update** tegel.

## <a name="next-steps"></a>Volgende stappen

- [Azure Stack onderhoud van beleid](azure-stack-servicing-policy.md) 
- [Regio management in Azure-Stack](azure-stack-region-management.md)     



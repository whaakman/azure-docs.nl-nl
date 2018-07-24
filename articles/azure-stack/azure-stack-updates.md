---
title: Updates beheren in Azure Stack-overzicht | Microsoft Docs
description: Meer informatie over updatebeheer voor geïntegreerde Azure Stack-systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 9b0781f4-2cd5-4619-a9b1-59182b4a6e43
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: mabrigg
ms.openlocfilehash: 484f04dc2ed523d3f979b0ba857c92f90d9d32ad
ms.sourcegitcommit: 248c2a76b0ab8c3b883326422e33c61bd2735c6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2018
ms.locfileid: "39215807"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Updates beheren in Azure Stack-overzicht

*Is van toepassing op: Azure Stack-geïntegreerde systemen*

Microsoft-updatepakketten vrij voor Azure Stack-systemen doorgaans geïntegreerde rond de vierde dinsdag van elke maand. Stel uw OEM over hun specifieke melding om ervoor te zorgen updatemeldingen bereiken in uw organisatie. U kunt ook controleren in deze Documentatiebibliotheek onder **overzicht** > **opmerkingen bij de Release** voor informatie over de versies die in de actieve ondersteuning zijn. 

Elke versie van Microsoft-software-updates wordt geleverd als één update-pakket. Als Azure Stack-operators, u kunt importeren, installeren en de voortgang van installatie van de volgende pakketten uit de beheerdersportal bijwerken. 

De leverancier van de oorspronkelijke leveranciers (OEM)-hardware brengt-updates, zoals stuurprogramma en firmware-updates ook. Terwijl deze updates worden geleverd als afzonderlijke pakketten door de leverancier van de OEM-hardware, ze worden geïmporteerd, geïnstalleerd en de dezelfde manier-updatepakketten beheerd van Microsoft-updatepakketten worden geïmporteerd, geïnstalleerd en beheerd.

Om te voorkomen dat uw systeem onder ondersteuning, moet u Azure Stack is bijgewerkt naar een specifieke versie. Zorg ervoor dat u de [Azure Stack servicebeleid](azure-stack-servicing-policy.md).

> [!NOTE]
> U kunt Azure Stack-updatepakketten niet toepassen op Azure Stack Development Kit. De updatepakketten zijn ontworpen voor geïntegreerde systemen. Zie voor meer informatie, [opnieuw implementeren van de ASDK](https://docs.microsoft.com/en-us/azure/azure-stack/asdk).

## <a name="the-update-resource-provider"></a>De resourceprovider voor Update

Azure Stack bevat een resourceprovider voor Update die de toepassing van software-updates van Microsoft regelt. Deze resourceprovider zorgt ervoor dat updates worden toegepast op alle fysieke hosts, Service Fabric-toepassingen en verbeteren, en alle virtuele machines van de infrastructuur en hun bijbehorende services.

Als er updates worden geïnstalleerd, kunt u statuswaarde op hoog niveau weergeven als de update proces doelen de verschillende subsystemen weer in Azure Stack (bijvoorbeeld fysieke hosts en infrastructuur-VM's).

## <a name="plan-for-updates"></a>Updates plannen

Wordt aangeraden dat u op de hoogte stellen gebruikers van elke onderhoudsbewerkingen, en dat u plant dat normale onderhoudsvensters tijdens de kantooruren indien mogelijk. Onderhoudsbewerkingen kunnen invloed hebben op zowel tenantwerkbelastingen en portal bewerkingen.

## <a name="using-the-update-tile-to-manage-updates"></a>Met behulp van de tegel Update om updates te beheren
U beheren updates vanuit de beheerdersportal. U kunt de tegel Update in het dashboard om te gebruiken als Azure Stack-operators:

- belangrijke informatie weergeven, zoals de huidige versie.
- updates installeren en de voortgang controleren.
- Historie van updates voor eerder geïnstalleerde updates bekijken.
 
## <a name="determine-the-current-version"></a>Bepalen van de huidige versie

De Update-tegel ziet u de huidige versie van Azure Stack. U kunt krijgen tot de tegel Update met behulp van een van de volgende methoden in de beheerdersportal:

- Weergeven op het dashboard, de huidige versie in de **Update** tegel.
 
   ![Updates tegel op standaarddashboard](./media/azure-stack-updates/image1.png)
 
- Op de **regiobeheer** tegel, klikt u op de regionaam van de. De huidige versie in de **Update** tegel.

## <a name="next-steps"></a>Volgende stappen

- [Azure Stack servicebeleid](azure-stack-servicing-policy.md) 
- [Regiobeheer in Azure Stack](azure-stack-region-management.md)     



---
title: Updates beheren in Azure Stack-overzicht | Microsoft Docs
description: Meer informatie over updatebeheer voor geïntegreerde Azure Stack-systemen.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2019
ms.author: mabrigg
ms.lastreviewed: 04/04/2019
ms.reviewer: justini
ms.openlocfilehash: bf797404b173d6febe133eacbb9d36310160aff6
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/08/2019
ms.locfileid: "59281570"
---
# <a name="manage-updates-in-azure-stack-overview"></a>Updates beheren in Azure Stack-overzicht

*Van toepassing op Geïntegreerde Azure Stack-systemen*

Microsoft-updatepakketten vrij voor Azure Stack-systemen doorgaans geïntegreerde rond de vierde dinsdag van elke maand. Stel uw oorspronkelijke leveranciers (OEM) over hun specifieke melding om ervoor te zorgen updatemeldingen bereiken in uw organisatie. U kunt ook controleren in deze Documentatiebibliotheek onder **overzicht** > **opmerkingen bij de Release** voor informatie over de versies die in de actieve ondersteuning zijn. 

Elke versie van Microsoft-software-updates wordt geleverd als één update-pakket. Als Azure Stack-operators, u kunt importeren, installeren en de voortgang van installatie van de volgende pakketten uit de beheerdersportal bijwerken. 

De OEM-leverancier brengt ook updates, zoals stuurprogramma en firmware-updates. Terwijl deze updates worden geleverd als afzonderlijke pakketten door de leverancier, zijn ze geïmporteerd, geïnstalleerd en op dezelfde manier als updatepakketten van Microsoft beheerd.

Om te voorkomen dat uw systeem onder ondersteuning, moet u Azure Stack is bijgewerkt naar een specifieke versie. Zorg ervoor dat u de [Azure Stack servicebeleid](azure-stack-servicing-policy.md).

> [!NOTE]
> U kunt Azure Stack-updatepakketten niet toepassen op Azure Stack Development Kit. De updatepakketten zijn ontworpen voor geïntegreerde systemen. Zie voor meer informatie, [opnieuw implementeren van de ASDK](https://docs.microsoft.com/azure/azure-stack/asdk).

## <a name="the-update-resource-provider"></a>De resourceprovider voor Update

Azure Stack bevat een provider voor Update-resource die verantwoordelijk is voor de toepassing van software-updates van Microsoft. Deze provider wordt gecontroleerd dat updates worden toegepast op alle fysieke hosts, Service Fabric-toepassingen en verbeteren, en alle virtuele machines van de infrastructuur en hun bijbehorende services.

Als er updates worden geïnstalleerd, kunt u statuswaarde op hoog niveau weergeven als de update proces doelen de verschillende subsystemen weer in Azure Stack (bijvoorbeeld fysieke hosts en infrastructuur-VM's).

## <a name="plan-for-updates"></a>Updates plannen

Wordt aangeraden dat u op de hoogte stellen gebruikers van elke onderhoudsbewerkingen, en dat u plant dat normale onderhoudsvensters tijdens de kantooruren indien mogelijk. Onderhoudsbewerkingen kunnen invloed hebben op zowel tenantwerkbelastingen en portal bewerkingen.

- Voordat u begint met de installatie van deze update, voert u [Test AzureStack](azure-stack-diagnostic-test.md) met de volgende parameters om te valideren van de status van uw Azure-Stack en los eventuele operationele problemen gevonden, met inbegrip van alle waarschuwingen en fouten. Ook actieve waarschuwingen bekijken en op te lossen die actie is vereist.  

  ```powershell
  Test-AzureStack -Group UpdateReadiness
  ``` 

## <a name="using-the-update-tile-to-manage-updates"></a>Met behulp van de tegel Update om updates te beheren

U beheren updates vanuit de beheerdersportal. U kunt de tegel Update in het dashboard om te gebruiken als Azure Stack-operators:

- belangrijke informatie weergeven, zoals de huidige versie.
- updates installeren en de voortgang controleren.
- Historie van updates voor eerder geïnstalleerde updates bekijken.
- Huidige versie van OEM-pakket van de cloud weergeven
 
## <a name="determine-the-current-version"></a>Bepalen van de huidige versie

U kunt de huidige versie van Azure Stack weergeven in de tegel bijwerken. Open de tegel als volgt:

1. Open de Azure Stack-beheerportal.
2. Selecteer **Dashboard**. In de **Update** tegel, de huidige versie wordt weergegeven. 

    ![Updates tegel op standaarddashboard](./media/azure-stack-updates/image1.png)

    Bijvoorbeeld, is de versie in het scherm 1.1903.0.35.

## <a name="install-updates-and-monitor-progress"></a>Updates installeren en de voortgang van de


1. Open de Azure Stack-beheerportal.
2. Selecteer **Dashboard**. Selecteer de tegel Update.
3. Selecteer **nu bijwerken**.

    ![Azure Stack update-uitvoering details](media/azure-stack-updates/azure-stack-update-button.png)

4.  U kunt statuswaarde op hoog niveau weergeven, zoals het updateproces verschillende subsystemen in Azure Stack doorlopen. Voorbeeld van de subsystemen zijn van fysieke hosts, Service Fabric-infrastructuur virtuele machines en services die bieden zowel de beheerder en gebruiker portals. Tijdens het updateproces rapporteert de provider voor Update resource aanvullende details over de update, zoals het aantal stappen die zijn geslaagd, evenals het aantal in uitvoering.

5. Selecteer de **volledige logboeken downloaden die u** blade met details om volledige logboeken te downloaden van de Update worden uitgevoerd.

    ![Azure Stack update-uitvoering details](media/azure-stack-updates/update-run-details.png)

6. Als voltooid, wordt de Update-resourceprovider biedt een **geslaagd** bevestiging van u te laten weten dat het updateproces is voltooid en hoe lang het heeft geduurd. Hier vindt u informatie over alle updates, beschikbare updates of geïnstalleerde updates met behulp van het filter.

    ![Azure Stack update-uitvoering details geslaagd](media/azure-stack-updates/update-success.png)

   Als de update is mislukt, de Update rapporten tegel **aandacht**. Gebruik de **volledige logboeken downloaden die u** om op te halen op hoog niveau status waarin de update kan zijn mislukt. De Azure Stack-logboekverzameling vergemakkelijkt diagnostiek en probleemoplossing.

## <a name="next-steps"></a>Volgende stappen

- [Azure Stack servicebeleid](azure-stack-servicing-policy.md) 
- [Regiobeheer in Azure Stack](azure-stack-region-management.md)

---
title: Status en waarschuwingen in Azure Stack controleren | Microsoft Docs
description: Informatie over het bewaken van de status en waarschuwingen in Azure Stack.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/18/2019
ms.author: mabrigg
ms.lastreviewed: 01/18/2019
ms.openlocfilehash: 3e37b8f45d7068586d20b9c4b3ccdabb017d0416
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55242855"
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Controleprogramma wordt de status en waarschuwingen in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack bevat infrastructuurbewaking mogelijkheden die u helpen bij weer de status en waarschuwingen voor een Azure Stack-regio. De **regiobeheer** tegel vastgemaakt in de beheerdersportal standaard voor het abonnement van de Provider standaard een lijst met alle geïmplementeerde regio's van Azure Stack. De tegel ziet u het aantal actieve waarschuwingen voor de kritieke en waarschuwingsstatussen voor elke regio. De tegel is uw ingangspunt in de status en waarschuwingen functionaliteit van Azure Stack.

![De tegel regiobeheer](media/azure-stack-monitor-health/image1.png)

## <a name="understand-health-in-azure-stack"></a>Status in Azure Stack begrijpen

De Health-resourceprovider beheert de status en waarschuwingen. Azure Stack-infrastructuuronderdelen registreren bij de resourceprovider health tijdens de Azure Stack-implementatie en configuratie. Deze registratie kunnen de weergave van de status en waarschuwingen voor elk onderdeel. De status in Azure Stack is een eenvoudig concept. Als waarschuwingen voor een geregistreerde exemplaar van een onderdeel bestaat, de status van dit onderdeel is inclusief de slechtste ernst van actieve waarschuwing: waarschuwing of kritiek.

## <a name="alert-severity-definition"></a>De definitie van de ernst van waarschuwing

Genereert waarschuwingen met slechts twee dagen per week in Azure Stack: **waarschuwing** en **kritieke**.

- **Waarschuwing**  
  Een operator kan voorzien in de waarschuwing in een geplande wijze. De waarschuwing doorgaans heeft geen invloed op werkbelastingen van de gebruiker.

- **Critical**  
  Een operator moet de kritieke waarschuwing aanpakken met urgentie. Dit zijn problemen die momenteel zijn van invloed zijn op of is binnenkort van invloed op Azure Stack-gebruikers.


## <a name="view-and-manage-component-health-state"></a>Weergeven en Onderdeelstatus beheren

U kunt de status van onderdelen weergeven in de beheerdersportal en via REST-API en PowerShell.

Als u wilt de status in de portal weergeven, klikt u op de regio die u weergeven wilt de **regiobeheer** tegel. U kunt de status van de infrastructuur en van resourceproviders bekijken.

![Lijst met rollen van de infrastructuur](media/azure-stack-monitor-health/image2.png)

U kunt klikken op een resourceprovider of de functie van de infrastructuur om meer gedetailleerde informatie weer te geven.

> [!WARNING]  
> Als u klikt u op de infrastructuurrol van een, en klik vervolgens op de rolinstantie, er zijn opties voor **Start**, **opnieuw**, of **afsluiten**. Gebruik deze acties niet wanneer u updates op een geïntegreerd systeem toepassen. Voer ook **niet** gebruik van deze opties in een omgeving met Azure Stack Development Kit. Deze opties zijn alleen ontworpen voor een omgeving met geïntegreerde systemen waarbij er meer dan één rolexemplaar per functie van de infrastructuur. Opnieuw opstarten van een rolinstantie (met name AzS-Xrp01) in de development kit zorgt ervoor dat de instabiliteit van het systeem. Plaatsen voor hulp bij probleemoplossing, uw probleem in de [Azure Stack-forum](https://aka.ms/azurestackforum).
>

## <a name="view-alerts"></a>Waarschuwingen weergeven

De lijst met actieve waarschuwingen voor elke Azure Stack-regio is rechtstreeks beschikbaar vanuit de **regiobeheer** blade. De eerste tegel in de standaardconfiguratie is de **waarschuwingen** tegel, wordt een samenvatting van de essentiële en waarschuwingen voor de regio. U kunt de tegel waarschuwingen, zoals elke andere tegel op deze blade, om het dashboard voor snelle toegang kunt vastmaken.

![Waarschuwingen van de tegel waarin een waarschuwing wordt weergegeven](media/azure-stack-monitor-health/image3.png)

Door het selecteren van het bovenste gedeelte van de **waarschuwingen** tegel, u gaat u naar de lijst met alle actieve waarschuwingen voor de regio. Als u een selecteren de **kritieke** of **waarschuwing** regelitem in de tegel, u gaat u naar een gefilterde lijst met waarschuwingen (Kritiek of waarschuwing). 

De **waarschuwingen** blade ondersteunt de mogelijkheid om te filteren, zowel op status (actief of gesloten) en de ernst (Kritiek of waarschuwing). De standaardweergave toont alle actieve waarschuwingen. Alle gesloten waarschuwingen worden verwijderd uit het systeem na zeven dagen.

![Filterdeelvenster om te filteren op kritieke of waarschuwingsstatus](media/azure-stack-monitor-health/alert-view.png)

De **weergave API** actie wordt de REST-API die is gebruikt voor het genereren van de lijstweergave. Deze actie biedt een snelle manier om vertrouwd te raken met de syntaxis van de REST-API die u voor query-waarschuwingen gebruiken kunt. U kunt deze API in automation of voor integratie met uw bestaande datacenter bewaking, rapportage en ticketing oplossingen gebruiken.

U kunt klikken op een specifieke waarschuwing om de Waarschuwingsdetails weer te geven. De Waarschuwingsdetails weergeven alle velden die zijn gekoppeld aan de waarschuwing, en snel te navigeren naar het betreffende onderdeel en de bron van de waarschuwing inschakelen. Bijvoorbeeld, de volgende waarschuwing wordt gegenereerd als een van de instanties van de infrastructuur offline gaat of niet toegankelijk is.  

![De blade Waarschuwingsdetails](media/azure-stack-monitor-health/alert-detail.png)

## <a name="repair-alerts"></a>Herstellen van waarschuwingen

U kunt selecteren **herstellen** in bepaalde waarschuwingen.

Als u selecteert, de **herstellen** actie wordt uitgevoerd voor specifieke stappen op de waarschuwing om te proberen het probleem op te lossen. Eenmaal geselecteerd de status van de **herstellen** actie is beschikbaar als een portalmelding.

![Het herstel wordt uitgevoerd](media/azure-stack-monitor-health/repair-in-progress.png)

De **herstellen** actie ondergeschikt is voltooid of mislukt de actie in de dezelfde blade in de portalmelding te voltooien.  Als een herstelactie is mislukt voor een waarschuwing, kunt u opnieuw de **herstellen** actie van de Waarschuwingsdetails. Als u de herstelactie is voltooid, **niet** opnieuw de **herstellen** actie.

![Het herstel is voltooid](media/azure-stack-monitor-health/repair-completed.png)

Nadat de rolinstantie infrastructuur weer online is, wordt deze waarschuwing automatisch wordt gesloten. Veel, maar niet elke waarschuwing, wordt automatisch gesloten wanneer het onderliggende probleem is opgelost. Waarschuwingen met een knop herstellen actie wordt automatisch gesloten als het probleem wordt opgelost door Azure Stack.  Voor alle andere waarschuwingen, selecteert u **sluiten waarschuwing** nadat u de stappen voor herstel uitvoeren. Als het probleem zich blijft voordoen, wordt een nieuwe waarschuwing gegenereerd in Azure Stack. Als u het probleem is opgelost, wordt de waarschuwing blijft afgesloten en er geen stappen meer vereist.

## <a name="next-steps"></a>Volgende stappen

[Updates beheren in Azure Stack](azure-stack-updates.md)

[Regiobeheer in Azure Stack](azure-stack-region-management.md)

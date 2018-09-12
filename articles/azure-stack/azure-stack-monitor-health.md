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
ms.date: 9/10/2018
ms.author: mabrigg
ms.openlocfilehash: 69ed08e8f6c820790c432bfa25988e038fd0efbd
ms.sourcegitcommit: 5a9be113868c29ec9e81fd3549c54a71db3cec31
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44379686"
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Controleprogramma wordt de status en waarschuwingen in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack bevat mogelijkheden waarmee u kunt bekijken van status en waarschuwingen voor een Azure Stack-regio bewaking van de infrastructuur. De **regiobeheer** tegel vastgemaakt in de beheerdersportal standaard voor het abonnement van de Provider standaard een lijst met alle geïmplementeerde regio's van Azure Stack. De tegel toont het aantal actieve waarschuwingen voor de kritieke en waarschuwingsstatussen voor elke regio en is uw ingangspunt in de status en waarschuwingen functionaliteit van Azure Stack.

 ![De tegel regiobeheer](media/azure-stack-monitor-health/image1.png)

 ## <a name="understand-health-in-azure-stack"></a>Status in Azure Stack begrijpen

 Status en waarschuwingen worden beheerd door de Health-resourceprovider. Azure Stack-infrastructuuronderdelen registreren bij de resourceprovider Health tijdens de Azure Stack-implementatie en configuratie. Deze registratie kunnen de weergave van de status en waarschuwingen voor elk onderdeel. De status in Azure Stack is een eenvoudig concept. Als waarschuwingen voor een geregistreerde exemplaar van een onderdeel bestaat, de status van dit onderdeel is inclusief de slechtste active ernst van waarschuwing; waarschuwing of kritiek.

## <a name="alert-severity-definition"></a>De definitie van de ernst van waarschuwing

In Azure Stack waarschuwingen worden gegenereerd met slechts twee dagen per week: **waarschuwing** en **kritieke**.

- **Waarschuwing**  
  Een operator kan voorzien in de waarschuwing in een geplande wijze. De waarschuwing doorgaans heeft geen invloed op werkbelastingen van de gebruiker.

- **Kritieke**  
  Een operator moet de kritieke waarschuwing aanpakken met urgentie. Dit zijn problemen die momenteel zijn van invloed zijn op of is binnenkort van invloed op Azure Stack-gebruikers. 

 
 ## <a name="view-and-manage-component-health-state"></a>Weergeven en Onderdeelstatus beheren
 
 Als Azure Stack-operators, kunt u de status van onderdelen weergeven in de beheerdersportal en via REST-API en PowerShell.
 
Als u wilt de status in de portal weergeven, klikt u op de regio die u weergeven wilt de **regiobeheer** tegel. U kunt de status van de infrastructuur en van resourceproviders bekijken.

![Lijst met rollen van de infrastructuur](media/azure-stack-monitor-health/image2.png)

U kunt klikken op een resourceprovider of de functie van de infrastructuur om meer gedetailleerde informatie weer te geven.

> [!WARNING]  
> Als u klikt u op de infrastructuurrol van een, en klik vervolgens op de rolinstantie, zijn er opties om te starten, opnieuw opstarten of afsluiten. Gebruik deze acties niet wanneer u updates op een geïntegreerd systeem toepassen. Voer ook **niet** gebruik van deze opties in een omgeving met Azure Stack Development Kit. Deze opties zijn ontworpen voor een omgeving met geïntegreerde systemen waarbij er meer dan één rolexemplaar per functie van de infrastructuur. Opnieuw opstarten van een rolinstantie (met name AzS-Xrp01) in de development kit zorgt ervoor dat de instabiliteit van het systeem. Plaatsen voor hulp bij probleemoplossing, uw probleem in de [Azure Stack-forum](https://aka.ms/azurestackforum).
>
 
## <a name="view-alerts"></a>Waarschuwingen weergeven

De lijst met actieve waarschuwingen voor elke Azure Stack-regio is rechtstreeks beschikbaar vanuit de **regiobeheer** blade. De eerste tegel in de standaardconfiguratie is de **waarschuwingen** tegel, wordt een samenvatting van de essentiële en waarschuwingen voor de regio. U kunt de tegel waarschuwingen, zoals elke andere tegel op deze blade, om het dashboard voor snelle toegang kunt vastmaken.   

![Waarschuwingen van de tegel waarin een waarschuwing wordt weergegeven](media/azure-stack-monitor-health/image3.png)

Door het selecteren van het bovenste gedeelte van de **waarschuwingen** tegel, u gaat u naar de lijst met alle actieve waarschuwingen voor de regio. Als u een selecteren de **kritieke** of **waarschuwing** regelitem in de tegel, u gaat u naar een gefilterde lijst met waarschuwingen (Kritiek of waarschuwing). 

![Gefilterde waarschuwingsmeldingen](media/azure-stack-monitor-health/image4.png)
  
De **waarschuwingen** blade ondersteunt de mogelijkheid om te filteren, zowel op status (actief of gesloten) en de ernst (Kritiek of waarschuwing). De standaardweergave toont alle actieve waarschuwingen. Alle gesloten waarschuwingen worden verwijderd uit het systeem na zeven dagen.

![Filterdeelvenster om te filteren op kritieke of waarschuwingsstatus](media/azure-stack-monitor-health/image5.png)

De **weergave API** actie wordt de REST-API die is gebruikt voor het genereren van de lijstweergave. Deze actie biedt een snelle manier om vertrouwd te raken met de syntaxis van de REST-API die u voor query-waarschuwingen gebruiken kunt. U kunt deze API in automation of voor integratie met uw bestaande datacenter bewaking, rapportage en ticketing oplossingen gebruiken. 

![De weergave API-optie die de REST-API toont](media/azure-stack-monitor-health/image6.png)

U kunt klikken op een specifieke waarschuwing om de Waarschuwingsdetails weer te geven. De Waarschuwingsdetails weergeven alle velden die zijn gekoppeld aan de waarschuwing, en snel te navigeren naar het betreffende onderdeel en de bron van de waarschuwing inschakelen. Bijvoorbeeld, de volgende waarschuwing wordt gegenereerd als een van de instanties van de infrastructuur offline gaat of niet toegankelijk is.  

![De blade Waarschuwingsdetails](media/azure-stack-monitor-health/image7.png)

Nadat de rolinstantie infrastructuur weer online is, wordt deze waarschuwing automatisch wordt gesloten. Veel, maar niet elke waarschuwing wordt automatisch gesloten wanneer het onderliggende probleem opgelost is. Het is raadzaam dat u selecteert **sluiten waarschuwing** nadat u de stappen voor herstel uitvoeren. Als het probleem zich blijft voordoen, wordt een nieuwe waarschuwing gegenereerd in Azure Stack. Als u het probleem is opgelost, wordt de waarschuwing blijft afgesloten en er is geen verdere actie vereist.

## <a name="next-steps"></a>Volgende stappen

[Updates beheren in Azure Stack](azure-stack-updates.md)

[Regiobeheer in Azure Stack](azure-stack-region-management.md)

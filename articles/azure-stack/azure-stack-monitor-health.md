---
title: Status en waarschuwingen in de Azure-Stack controleren | Microsoft Docs
description: Informatie over het bewaken van de status en waarschuwingen in de Azure-Stack.
services: azure-stack
documentationcenter: 
author: mattbriggs
manager: femila
editor: 
ms.assetid: 69901c7b-4673-4bd8-acf2-8c6bdd9d1546
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/10/2017
ms.author: mabrigg
ms.openlocfilehash: 446df7922422ccfcf3fbb92ecf153c6dec2f6197
ms.sourcegitcommit: a5f16c1e2e0573204581c072cf7d237745ff98dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="monitor-health-and-alerts-in-azure-stack"></a>Status en waarschuwingen in de Azure-Stack controleren

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

Azure Stack bevat mogelijkheden waarmee u om weer te geven van de status en waarschuwingen voor een Azure-Stack-gebied bewaking van infrastructuur. De **regio management** tegel standaard in de beheerdersportal vastgemaakt voor het abonnement van de Provider standaard een lijst met alle geïmplementeerde regio's van Azure-Stack. De tegel toont het aantal actieve kritieke en waarschuwingsstatussen waarschuwingen voor elke regio en is uw ingangspunt voor de status en waarschuwingen functionaliteit van Azure-Stack.

 ![De tegel regio Management](media/azure-stack-monitor-health/image1.png)

 ## <a name="understand-health-in-azure-stack"></a>Status in Azure-Stack begrijpen

 Status en waarschuwingen worden beheerd door de Health-resourceprovider. Azure Stack infrastructuuronderdelen registreren bij de resourceprovider Health tijdens de implementatie van de Azure-Stack en configuratie. Deze registratie kunnen de weergave van de status en waarschuwingen voor elk onderdeel. De status in Azure-Stack is een eenvoudige concept. Als waarschuwingen voor een geregistreerde exemplaar van een component aanwezig is, de status van dit onderdeel wordt doorgevoerd slechtste active ernst van de waarschuwing; waarschuwing of kritiek.
 
 ## <a name="view-and-manage-component-health-state"></a>Weergeven en beheren van de onderdeelstatus
 
 Als een Azure-Stack-operator, kunt u de status van onderdelen weergeven in de beheerdersportal en via REST-API en PowerShell.
 
De status in de portal klikt u op de regio die u weergeven wilt de **regio management** tegel. U kunt de status van de functies van de infrastructuur en resourceproviders bekijken.

![Lijst met rollen van de infrastructuur](media/azure-stack-monitor-health/image2.png)

U kunt klikken op een resourceprovider of de functie infrastructuurmaster om meer gedetailleerde informatie weer te geven.

> [!WARNING]
>Als u een infrastructuurrol op, en klik vervolgens op de rolinstantie, zijn er opties om op te starten, opnieuw opstarten of afsluiten. Deze acties geen gebruiken wanneer u updates op een geïntegreerd systeem toepassen. Kies ook **niet** gebruik van deze opties in de omgeving van een Azure-Stack Development Kit. Deze opties zijn specifiek bedoeld voor een geïntegreerde omgeving, waarin er meer dan één rolexemplaar per infrastructuurrol. Een rolexemplaar (met name AzS-Xrp01) opnieuw te starten in de development kit zorgt ervoor dat de instabiliteit van het systeem. Voor hulp bij probleemoplossing, posten uw probleem dat de [Azure Stack-forum](https://aka.ms/azurestackforum).
>
 
## <a name="view-alerts"></a>Waarschuwingen weergeven

De lijst met actieve waarschuwingen voor elke regio Azure-Stack is beschikbaar rechtstreeks vanuit de **regio management** blade. De eerste tegel in de standaardconfiguratie wordt de **waarschuwingen** tegel, geeft een samenvatting van de essentiële en waarschuwingen voor de regio. U kunt de tegel waarschuwingen, zoals elke andere tegel in deze blade naar het dashboard voor snelle toegang vastmaken.   

![Waarschuwingen van de tegel die krijgt de waarschuwingsstatus](media/azure-stack-monitor-health/image3.png)

Door het selecteren van het bovenste gedeelte van de **waarschuwingen** tegel, navigeer naar de lijst met alle actieve waarschuwingen voor de regio. Als u een selecteren de **Kritiek** of **waarschuwing** item in de tegel, navigeer naar een gefilterde lijst met waarschuwingen (Kritiek of waarschuwing). 

![Gefilterde waarschuwingen](media/azure-stack-monitor-health/image4.png)
  
De **waarschuwingen** blade ondersteunt de mogelijkheid om te filteren op status (actief of gesloten) en de ernst (Kritiek of waarschuwing). De standaardweergave bevat alle actieve waarschuwingen. Alle gesloten waarschuwingen worden verwijderd uit het systeem na zeven dagen.

![Filterdeelvenster filteren op kritieke of waarschuwingsstatus](media/azure-stack-monitor-health/image5.png)

De **weergave API** actie wordt de REST-API die is gebruikt voor het genereren van de lijstweergave. Deze actie biedt een snelle manier om vertrouwd te raken met de syntaxis van de REST-API die u waarschuwingen van de query kunt. U kunt deze API in automation en voor integratie met uw bestaande datacenter bewaking, rapportage en tickets oplossingen gebruiken. 

![De weergave-API-optie die het ziet u de REST-API](media/azure-stack-monitor-health/image6.png)

U kunt klikken op een specifieke waarschuwing om de Waarschuwingsdetails weer te geven. Details van de waarschuwing weergeven alle velden die zijn gekoppeld aan de waarschuwing en snel navigeren naar het betreffende onderdeel en de bron van de waarschuwing in te schakelen. Bijvoorbeeld, treedt de volgende waarschuwing op als een van de infrastructuur-rolexemplaren gaat het offline of niet toegankelijk is.  

![De blade Waarschuwingsdetails](media/azure-stack-monitor-health/image7.png)

Nadat de instantie van infrastructuur weer online is, wordt deze waarschuwing automatisch wordt gesloten. Veel, maar niet elke waarschuwing wordt automatisch gesloten wanneer het onderliggende probleem opgelost is. Het is raadzaam dat u selecteert **waarschuwing sluiten** nadat u herstelstappen uitgevoerd. Als het probleem zich blijft voordoen, wordt een nieuwe waarschuwing gegenereerd in Azure-Stack. Als het probleem is opgelost, wordt de waarschuwing blijft gesloten en is geen verdere actie vereist.

## <a name="next-steps"></a>Volgende stappen

[Beheren van updates in de Azure-Stack](azure-stack-updates.md)

[Regio management in Azure-Stack](azure-stack-region-management.md)

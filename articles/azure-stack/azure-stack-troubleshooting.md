---
title: Microsoft Azure-Stack probleemoplossing | Microsoft Docs
description: Azure Stack het oplossen van problemen.
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/11/2017
ms.author: helaw
ms.openlocfilehash: 0a8e871a3a44cb14503832d2f3a096712f8112a7
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Microsoft Azure-Stack probleemoplossing

*Van toepassing op: Azure stapelen Development Kit*

Dit document bevat algemene informatie over probleemoplossing voor Azure-Stack. 

Omdat de Azure-Stack technische Development Kit wordt aangeboden als een evaluatieomgeving, is er geen officiële ondersteuning van Microsoft Customer Support Services.  Als er een probleem dat wordt niet beschreven, Controleer of de [Stack MSDN-Forum Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) voor verdere ondersteuning en informatie.  

De aanbevelingen voor het oplossen van problemen die worden beschreven in deze sectie zijn afgeleid van diverse bronnen en kunnen of kunnen uw probleem niet oplossen. Codevoorbeelden worden verstrekt vorm en de verwachte resultaten kunnen niet worden gegarandeerd. Deze sectie is onderworpen aan regelmatige wijzigingen en updates zoals verbeteringen aan het product zijn geïmplementeerd.

## <a name="deployment"></a>Implementatie
### <a name="deployment-failure"></a>Fout bij implementatie
Als er een fout tijdens de installatie optreden, kunt u de optie opnieuw uitvoeren van het script voor implementatie gebruiken om opnieuw te starten van de implementatie van de mislukte stap.  


### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Aan het einde van de implementatie van de PowerShell-sessie is nog geopend en eventuele uitvoer wordt niet weergegeven
Dit gedrag is waarschijnlijk alleen het resultaat van het standaardgedrag van een PowerShell-opdrachtvenster wanneer dit item is geselecteerd. De implementatie van development kit daadwerkelijk is geslaagd, maar het script is onderbroken bij het selecteren van het venster. U kunt controleren of dat dit het geval is door te zoeken naar het woord 'selecteren' in de titelbalk van het opdrachtvenster.  Druk op ESC deze selectie opheffen en de voltooiingsbericht nadat deze moet worden weergegeven.

## <a name="virtual-machines"></a>Virtuele machines
### <a name="default-image-and-gallery-item"></a>Standaard installatiekopie en de galerie-item
Voordat u virtuele machines in Azure-Stack implementeert, moet u eerst een Windows Server-installatiekopie en de galerie-item toevoegen.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Start opnieuw op mijn Azure-Stack-host en een aantal virtuele machines niet automatisch wordt gestart.
De host opnieuw is opgestart, merkt u wellicht de Stack Azure-services zijn niet onmiddellijk beschikbaar.  Dit is omdat de Stack is Azure [infrastructuur VMs](azure-stack-architecture.md#virtual-machine-roles) en RPs duren een beetje om consistentie te controleren, maar wordt uiteindelijk automatisch gestart.

U merkt wellicht ook dat virtuele machines niet automatisch wordt gestart na opnieuw opstarten van de Azure-Stack development kit host tenant.  Dit is een bekend probleem en moet een aantal handmatige stappen voor het ze online brengt:

1.  Start op de Azure-Stack development kit host, **Failoverclusterbeheer** vanuit het Menu Start.
2.  Selecteer het cluster **S Cluster.azurestack.local**.
3.  Selecteer **rollen**.
4.  Tenant-VM's worden weergegeven in een *opgeslagen* status.  Zodra alle infrastructuur VM's worden uitgevoerd, met de rechtermuisknop op de tenant-VM's en selecteer **Start** hervatten van de virtuele machine.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ik een aantal virtuele machines hebt verwijderd, maar nog steeds de VHD-bestanden op schijf. Wordt dit gedrag verwacht?
Ja, is dit gedrag verwacht. Het is zo ontworpen, omdat:

* Wanneer u een virtuele machine verwijdert, worden virtuele harde schijven worden niet verwijderd. Schijven zijn afzonderlijke resources in de resourcegroep.
* Wanneer een opslagaccount wordt verwijderd, de verwijdering is zichtbaar onmiddellijk via Azure Resource Manager (portal, PowerShell), maar de schijven die deze mogelijk nog steeds in de opslag worden bewaard totdat garbagecollection wordt uitgevoerd.

Als u 'zwevende' VHD's ziet, is het belangrijk te weten als ze deel uitmaken van de map voor een opslagaccount dat is verwijderd. Als het opslagaccount kan niet worden verwijderd, is het normaal bent.

Meer informatie over het configureren van de retentie drempelwaarde en on-demand vrijmaken in [storage-accounts beheren](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Storage
### <a name="storage-reclamation"></a>Opslag vrijmaken
Het duurt maximaal 14 uur geregenereerde capaciteit worden weergegeven in de portal. Ruimte vrijmaken, is afhankelijk van verschillende factoren, met inbegrip van gebruikspercentage van de informatie over het van interne containerbestanden in het bloblarchief blok. Daarom afhankelijk van hoeveel gegevens worden verwijderd, is er geen garantie van de hoeveelheid ruimte die kan worden vrijgemaakt bij-garbagecollector wordt uitgevoerd.

## <a name="windows-azure-pack-connector"></a>Windows Azure Pack-Connector
* Als u het wachtwoord van het account azurestackadmin wijzigt nadat u Azure-Stack development kit hebt geïmplementeerd, kunt u niet langer meerdere cloud-modus. Daarom zijn deze niet mogelijk verbinding maken met de Windows Azure Pack doelomgeving.
* Na het instellen van meerdere cloud modus:
    * Een gebruiker ziet het dashboard nadat ze opnieuw instellen van de portalinstellingen. (Klik op het pictogram portal-instellingen (tandwielpictogram pictogram in de rechterbovenhoek) in de gebruikersportal. Onder **standaardinstellingen herstellen**, klikt u op **toepassen**.)
    * De titels van het dashboard mogelijk niet weergegeven. Als dit probleem optreedt, moet u handmatig toevoegen ze terug.
    * Bepaalde tegels mogelijk niet correct weergegeven wanneer u ze eerst aan het dashboard toevoegen. U kunt dit probleem oplossen door de browser te vernieuwen.




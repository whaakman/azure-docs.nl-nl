---
title: Het oplossen van Microsoft Azure Stack | Microsoft Docs
description: Azure Stack voor probleemoplossing.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: a20bea32-3705-45e8-9168-f198cfac51af
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: jeffgilb
ms.reviewer: unknown
ms.openlocfilehash: 53bb89daee47d5f380786246070cf5cddb69b731
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929548"
---
# <a name="microsoft-azure-stack-troubleshooting"></a>Het oplossen van Microsoft Azure Stack

Dit document bevat algemene informatie over probleemoplossing voor Azure Stack. 

> [!NOTE]
> Omdat de Azure Stack Technical Development Kit (ASDK) wordt aangeboden als een evaluatie-omgeving, is er geen officiële ondersteuning van Microsoft Customer Support Services. Als u een probleem ondervindt, controleert u of om te controleren of de [MSDN-Forum voor Azure Stack](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) voor verdere ondersteuning en informatie.  

De aanbevelingen voor het oplossen van problemen die worden beschreven in deze sectie zijn afgeleid van diverse bronnen en kunnen of kunnen uw probleem niet oplossen. Voorbeelden van code worden verstrekt, vorm en verwachte resultaten niet worden gegarandeerd. In deze sectie is onderhevig aan regelmatig wijzigingen en updates, verbeteringen aan het product zijn geïmplementeerd.

## <a name="deployment"></a>Implementatie
### <a name="deployment-failure"></a>Fout bij de implementatie
Als u een fout opgetreden tijdens de installatie ondervindt, kunt u de implementatie van de mislukte stap opnieuw opstarten met behulp van de - optie opnieuw uitvoeren van het script voor implementatie.  

### <a name="at-the-end-of-asdk-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Aan het einde van de ASDK implementatie van de PowerShell-sessie is nog steeds geopend en eventuele uitvoer wordt niet weergegeven.
Dit gedrag is waarschijnlijk alleen het resultaat van het standaardgedrag van een PowerShell-opdrachtvenster wanneer deze is geselecteerd. De development kit-implementatie is voltooid, maar het script is onderbroken bij het selecteren van het venster. U kunt controleren of de installatie is voltooid door te zoeken naar het woord 'selecteren' in de titelbalk van het opdrachtvenster.  Druk op ESC om terug te Hef de selectie van het en het voltooiingsbericht is nadat deze moet worden weergegeven.

## <a name="virtual-machines"></a>Virtuele machines
### <a name="default-image-and-gallery-item"></a>Standaard-installatiekopie en galerie-item
Een Windows Server-installatiekopie en galerie-item moet worden toegevoegd voor het implementeren van virtuele machines in Azure Stack.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Nadat de Azure Stack-host opnieuw is opgestart, enkele VM's kunnen niet automatisch wordt gestart.
Na het opnieuw opstarten van de host, merkt u wellicht Azure Stack-services zijn niet onmiddellijk beschikbaar.  Dit komt doordat Azure Stack [infrastructuur-VM's](..\azure-stack\asdk\asdk-architecture.md#virtual-machine-roles) en resourceproviders even duren om te controleren op consistentie, maar uiteindelijk automatisch wordt gestart.

U merkt wellicht ook die tenant die virtuele machines niet automatisch wordt gestart na opnieuw opstarten van de Azure Stack development kit-host. Dit is een bekend probleem en moet een aantal handmatige stappen voor het ze online brengt:

1.  Start op de host van Azure Stack development kit, **Failoverclusterbeheer** vanuit het Menu Start.
2.  Selecteer het cluster **S Cluster.azurestack.local**.
3.  Selecteer **rollen**.
4.  Tenant-VM's worden weergegeven in een *opgeslagen* staat. Zodra alle infrastructuur-VM's worden uitgevoerd, met de rechtermuisknop op de tenant-VM's en selecteer **Start** hervatten van de virtuele machine.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Kan ik een aantal virtuele machines hebt verwijderd, maar nog steeds de VHD-bestanden op schijf. Is dit gedrag verwacht?
Ja, dit is normaal. Het is zo ontworpen omdat:

* Wanneer u een virtuele machine verwijdert, worden de VHD's niet verwijderd. Schijven zijn afzonderlijke resources in de resourcegroep.
* Wanneer een storage-account wordt verwijderd, wordt de verwijdering is zichtbaar onmiddellijk via Azure Resource Manager, maar de schijven bevat mogelijk nog steeds worden bewaard in opslag totdat garbagecollection wordt uitgevoerd.

Als u VHD's 'zwevende' ziet, is het belangrijk te weten als ze deel uitmaken van de map voor een opslagaccount dat is verwijderd. Als het opslagaccount is niet verwijderd, is het normaal dat ze nog steeds aanwezig.

U kunt meer lezen over het configureren van de bewaarperiode drempelwaarde en on-demand vrijmaken in [opslagaccounts beheren](azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Storage
### <a name="storage-reclamation"></a>Vrijmaken van opslagruimte
Het duurt maximaal 14 uur geregenereerde capaciteit worden weergegeven in de portal. Vrijmaken van ruimte, is afhankelijk van diverse factoren, met inbegrip van gebruikspercentage van de interne containerbestanden in blok-blobopslag. Daarom afhankelijk van hoeveel gegevens worden verwijderd, is er geen garantie voor de hoeveelheid ruimte die kan worden vrijgemaakt wanneer garbagecollector wordt uitgevoerd.


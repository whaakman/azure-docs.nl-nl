---
title: Microsoft Azure-Stack probleemoplossing | Microsoft Docs
description: Azure Stack Development Kit (ASDK) informatie over probleemoplossing.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2018
ms.author: jeffgilb
ms.reviewer: misainat
ms.openlocfilehash: 6c715f07f75c9196b7cf2cc8659c6e541e1260da
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
# <a name="microsoft-azure-stack-development-kit-asdk-troubleshooting"></a>Het oplossen van Microsoft Azure Stack Development Kit (ASDK)
Dit document bevat algemene informatie over probleemoplossing voor de ASDK. Als er een probleem dat is niet gedocumenteerd, Controleer of de [Stack MSDN-Forum Azure](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack) voor verdere ondersteuning en informatie.  

> [!IMPORTANT]
> Omdat de ASDK een evaluatieomgeving is, is geen officiële ondersteuning wordt aangeboden via Microsoft Customer ondersteunen Services (CSS).

De aanbevelingen voor het oplossen van problemen die worden beschreven in deze sectie zijn afgeleid van diverse bronnen en kunnen of kunnen uw probleem niet oplossen. Codevoorbeelden worden geleverd 'als zodanig' en de verwachte resultaten kunnen niet worden gegarandeerd. Deze sectie is onderworpen aan regelmatige wijzigingen en updates zoals verbeteringen aan het product zijn geïmplementeerd.

## <a name="deployment"></a>Implementatie
### <a name="deployment-failure"></a>Fout bij implementatie
Als er een fout tijdens de installatie optreden, kunt u de implementatie van de mislukte stap opnieuw starten met behulp van de - optie opnieuw uitvoeren van het implementatiescript zoals in het volgende voorbeeld:

  ```powershell
  cd C:\CloudDeployment\Setup
  .\InstallAzureStackPOC.ps1 -Rerun
  ```

### <a name="at-the-end-of-the-deployment-the-powershell-session-is-still-open-and-doesnt-show-any-output"></a>Aan het einde van de implementatie van de PowerShell-sessie is nog geopend en eventuele uitvoer wordt niet weergegeven
Dit gedrag is waarschijnlijk alleen het resultaat van het standaardgedrag van een PowerShell-opdrachtvenster wanneer dit item is geselecteerd. De development kit-implementatie is geslaagd, maar het script is onderbroken bij het selecteren van het venster. U kunt controleren of de installatie is voltooid door te zoeken naar het woord 'selecteren' in de titelbalk van het opdrachtvenster. Druk op ESC deze selectie opheffen en de voltooiingsbericht nadat deze moet worden weergegeven.

## <a name="virtual-machines"></a>Virtuele machines
### <a name="default-image-and-gallery-item"></a>Standaard installatiekopie en de galerie-item
Een Windows Server-installatiekopie en de galerie-item moet worden toegevoegd voordat u virtuele machines in Azure-Stack implementeert.

### <a name="after-restarting-my-azure-stack-host-some-vms-may-not-automatically-start"></a>Start opnieuw op mijn Azure-Stack-host en een aantal virtuele machines niet automatisch wordt gestart.
De host opnieuw is opgestart, merkt u wellicht de Stack Azure-services zijn niet onmiddellijk beschikbaar. Dit is omdat de Stack is Azure [infrastructuur VMs](asdk-architecture.md#virtual-machine-roles) en los het RPs sommige tijd om consistentie te controleren, maar wordt uiteindelijk automatisch gestart.

U merkt wellicht ook dat virtuele machines niet automatisch wordt gestart na opnieuw opstarten van de Azure-Stack development kit host tenant. Dit is een bekend probleem en moet een aantal handmatige stappen voor het ze online brengt:

1.  Start op de Azure-Stack development kit host, **Failoverclusterbeheer** vanuit het Menu Start.
2.  Selecteer het cluster **S Cluster.azurestack.local**.
3.  Selecteer **rollen**.
4.  Tenant-VM's worden weergegeven in een *opgeslagen* status. Zodra alle infrastructuur VM's worden uitgevoerd, met de rechtermuisknop op de tenant-VM's en selecteer **Start** hervatten van de virtuele machine.

### <a name="i-have-deleted-some-virtual-machines-but-still-see-the-vhd-files-on-disk-is-this-behavior-expected"></a>Ik een aantal virtuele machines hebt verwijderd, maar nog steeds de VHD-bestanden op schijf. Wordt dit gedrag verwacht?
Ja, is dit gedrag verwacht. Het is zo ontworpen, omdat:

* Wanneer u een virtuele machine verwijdert, worden virtuele harde schijven worden niet verwijderd. Schijven zijn afzonderlijke resources in de resourcegroep.
* Wanneer een opslagaccount wordt verwijderd, de verwijdering is zichtbaar onmiddellijk via Azure Resource Manager, maar de schijven die deze mogelijk nog steeds in de opslag worden bewaard totdat garbagecollection wordt uitgevoerd.

Als u 'zwevende' VHD's ziet, is het belangrijk te weten als ze deel uitmaken van de map voor een opslagaccount dat is verwijderd. Als het opslagaccount kan niet worden verwijderd, is het normaal bent.

Meer informatie over het configureren van de retentie drempelwaarde en on-demand vrijmaken in [storage-accounts beheren](.\.\azure-stack-manage-storage-accounts.md).

## <a name="storage"></a>Storage
### <a name="storage-reclamation"></a>Opslag vrijmaken
Het duurt maximaal 14 uur geregenereerde capaciteit worden weergegeven in de portal. Ruimte vrijmaken, is afhankelijk van verschillende factoren, met inbegrip van gebruikspercentage van de informatie over het van interne containerbestanden in het bloblarchief blok. Daarom afhankelijk van hoeveel gegevens worden verwijderd, is er geen garantie van de hoeveelheid ruimte die kan worden vrijgemaakt bij-garbagecollector wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
[Ga naar het Azure-Stack-forum voor ondersteuning](https://social.msdn.microsoft.com/Forums/azure/home?forum=azurestack)


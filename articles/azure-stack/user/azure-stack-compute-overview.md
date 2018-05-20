---
title: Kennismaking met virtuele machines in Azure Stack
description: Meer informatie over Azure-Stack virtuele machines
services: azure-stack
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.topic: get-started-article
ms.date: 02/28/2018
ms.author: mabrigg
ms.openlocfilehash: 41e75a6806cc5ff13fad64fd415344376e0d6e88
ms.sourcegitcommit: c52123364e2ba086722bc860f2972642115316ef
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="introduction-to-azure-stack-virtual-machines"></a>Kennismaking met virtuele machines in Azure Stack

*Van toepassing op: Azure Stack geïntegreerde systemen en Azure Stack Development Kit*

## <a name="overview"></a>Overzicht
Een Azure Stack van virtuele Machine (VM) is een op aanvraag, schaalbare rekenbron die Azure-Stack biedt één type. Normaal gesproken kiest u voor een VM wanneer u meer controle nodig hebt over de computeromgeving dan andere opties bieden. In dit artikel vindt u informatie over wat u moet overwegen voordat u een VM maakt, hoe u deze maakt en hoe u deze beheert.

Een Azure-Stack van virtuele machine biedt u de flexibiliteit van virtualisatie zonder de noodzaak om clusters of afzonderlijke computers te beheren. U moet echter nog steeds het onderhouden van de virtuele machine door taken uitvoeren zoals het configureren, patchen en installeren van de software die wordt uitgevoerd op deze.

U kunt Azure-Stack virtuele machines op verschillende manieren gebruiken. Bijvoorbeeld:

* **Ontwikkeling en tests** – Stack van virtuele machines van Azure bieden een snelle en gemakkelijke manier om het maken van een computer met een specifieke configuratie vereist om de code en testen van een toepassing.

* **Toepassingen in de cloud** – omdat de aanvraag voor uw toepassing kunt fluctueren, kan het zinvol economische uit te voeren op een virtuele machine in Azure-Stack. U betaalt voor extra virtuele machines wanneer u ze nodig hebt en schakelt ze uit wanneer u ze niet meer nodig hebt.

* **Uitgebreide datacenter** – virtuele machines in een virtueel netwerk van Azure-Stack kunnen eenvoudig worden aangesloten op het netwerk van uw organisatie of naar Azure.

De virtuele machines die uw toepassing gebruikt kunnen omhoog schalen of uitschalen naar wat is vereist om te voldoen aan uw behoeften.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Waar moet ik over nadenken voordat ik een VM maak?

Er zijn altijd talrijke Ontwerpoverwegingen bij het samenstellen van de infrastructuur van een toepassing in Azure-Stack. Deze aspecten van een virtuele machine zijn belangrijk om na te denken over voordat u begint met het maken van uw infrastructuur:

* De namen van de toepassingsresources van uw.
* De grootte van de virtuele machine.
* Het maximum aantal VM's die kunnen worden gemaakt.
* Het besturingssysteem dat de virtuele machine wordt uitgevoerd.
* De configuratie van de virtuele machine nadat deze is gestart.
* De gerelateerde bronnen waarvoor u de virtuele machine.

### <a name="naming"></a>Naamgeving

Een virtuele machine heeft een naam die is toegewezen en er een computernaam die is geconfigureerd als onderdeel van het besturingssysteem. De naam van een VM mag uit maximaal 15 tekens bestaan.

Als u Azure-Stack gebruikt voor het maken van de schijf van het besturingssysteem, de computernaam en naam van de virtuele machine zijn hetzelfde. Als u uploaden en uw eigen installatiekopie waarin een eerder geconfigureerde besturingssysteem gebruikt en een virtuele machine maken, is het mogelijk dat de namen van de andere. Als u uw eigen installatiekopiebestand uploadt, Controleer de computernaam in het besturingssysteem en de virtuele machine de naam hetzelfde zijn als een aanbevolen procedure.

### <a name="vm-size"></a>VM-grootte

De grootte van de virtuele machine die u gebruikt, wordt bepaald door de werkbelasting die u wilt uitvoeren. De grootte die u vervolgens kiest, bepaalt factoren als processorsnelheid, geheugen en opslagcapaciteit. Azure-Stack biedt een groot aantal verschillende grootten voor verschillende manieren van gebruik.

### <a name="vm-limits"></a>VM-limieten

Uw abonnement heeft standaard de quotalimieten dat invloed op de implementatie van veel VM's voor uw project hebben kunnen. De huidige limiet per abonnement is 20 VM's per regio.

### <a name="operating-system-disks-and-images"></a>Schijven en installatiekopieën voor een besturingssysteem

Virtuele machines maken gebruik van virtuele harde schijven (VHD's) voor de opslag van het besturingssysteem (OS) en de gegevens. VHD's worden ook gebruikt voor de installatiekopieën waarmee u een besturingssysteem kunt installeren.
Azure Stack biedt een marketplace voor gebruik met verschillende versies en typen van besturingssystemen. Marketplace-installatiekopieën worden geïdentificeerd door de installatiekopie-uitgever, aanbieding, sku en versie (meestal versie als laatste is opgegeven).

De volgende tabel ziet u enkele manieren dat u de gegevens voor een installatiekopie kunt vinden:

|Methode|Beschrijving|
|---------|---------|
|Azure Stack-portal|De waarden worden automatisch opgegeven wanneer u een installatiekopie selecteert om te gebruiken.|
|PowerShell voor Azure Stack|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|REST-API’s     |[Uitgevers van installatiekopieën weergeven](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publishers)<br>[Aanbiedingen van installatiekopieën weergeven](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offers)<br>[Afbeelding van de lijst SKU 's](https://docs.microsoft.com/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus)|

U kunt kiezen om te uploaden en uw eigen installatiekopie gebruiken. Als u dit doet, worden de naam van de uitgever, aanbieding en sku worden niet gebruikt.

### <a name="extensions"></a>Extensies

VM-extensies bieden de aanvullende mogelijkheden van uw virtuele machine via de post-implementatieconfiguratie en geautomatiseerde taken.
Deze algemene taken kunnen worden uitgevoerd met extensies:

* De extensie voor aangepaste scripts kunt uitvoeren van aangepaste scripts – u werkbelastingen op de virtuele machine configureren door het script wordt uitgevoerd wanneer de virtuele machine is ingericht.
* Implementeren en beheren van configuraties: uitbreiding van de PowerShell Desired State Configuration (DSC) kunt u DSC op een virtuele machine instellen voor het beheren van configuraties en omgevingen.
* De Azure-extensie voor diagnostische gegevens verzamelen van diagnostische gegevens kunt u de virtuele machine voor het verzamelen van diagnostische gegevens die kunnen worden gebruikt voor het controleren van de status van uw toepassing configureren.

### <a name="related-resources"></a>Gerelateerde resources

De resources in de volgende tabel worden gebruikt door de virtuele machine en moeten bestaan of worden gemaakt wanneer de virtuele machine wordt gemaakt.


|Resource|Vereist|Beschrijving|
|---------|---------|---------|
|Resourcegroep|Ja|De VM moet zijn opgenomen in een resourcegroep.|
|Storage-account|Ja|De VM heeft het opslagaccount nodig voor het opslaan van de virtuele harde schijven.|
|Virtueel netwerk|Ja|De VM moet lid zijn van een virtueel netwerk.|
|Openbaar IP-adres|Nee|Aan de VM kan een openbaar IP-adres worden toegewezen voor externe toegang.|
|Netwerkinterface|Ja|De netwerkinterface van de VM moet in het netwerk communiceren.|
|Gegevensschijven|Nee|De VM kan gegevensschijven bevatten om opslagmogelijkheden uit te breiden.|

## <a name="how-do-i-create-my-first-vm"></a>Hoe kan ik mijn eerste VM maken?

U hebt verschillende mogelijkheden voor het maken van een virtuele machine. Uw keuze is afhankelijk van uw omgeving.
De volgende tabel bevat informatie om u te helpen beginnen met het opstellen van uw virtuele machine.


|Methode|Artikel|
|---------|---------|
|Azure Stack-portal|Een virtuele Windows-machine maken met de Stack van Azure-portal<br>[Maken van een virtuele Linux-machine met behulp van de Stack van Azure-portal](azure-stack-quick-linux-portal.md)|
|Sjablonen|Azure-Stack Quick Start-sjablonen bevinden zich op:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)|
|PowerShell|[Een virtuele Windows-machine maken met behulp van PowerShell in Azure-Stack](azure-stack-quick-create-vm-windows-powershell.md)<br>[Een virtuele Linux-machine maken met behulp van PowerShell in Azure-Stack](azure-stack-quick-create-vm-linux-powershell.md)|
|CLI|[Een virtuele Windows-machine maken met CLI in Azure-Stack](azure-stack-quick-create-vm-windows-cli.md)<br>[Een virtuele Linux-machine maken met CLI in Azure-Stack](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="how-do-i-manage-the-vm-that-i-created"></a>Hoe beheer ik de VM die ik heb gemaakt?

U kunt virtuele machines met een browser gebaseerde portal, opdrachtregelprogramma's met ondersteuning voor het uitvoeren van scripts of rechtstreeks via API's kunt beheren. Er zijn een aantal veelvoorkomende beheertaken die u kunt uitvoeren:

* Informatie ophalen over een virtuele machine
* Verbinding maken met een virtuele machine
* Beschikbaarheid van beheren
* Back-ups maken

### <a name="get-information-about-a-vm"></a>Informatie over een VM ophalen

De volgende tabel ziet u een aantal manieren waarop die u informatie over een virtuele machine ophalen kunt.


|Methode|Beschrijving|
|---------|---------|
|Azure Stack-portal|Klik op de virtuele Machines in het hubmenu en selecteer vervolgens de virtuele machine in de lijst. Op de pagina voor de virtuele machine hebt u toegang tot informatie waarden instellen en controle van metrische gegevens.|
|Azure PowerShell|Het beheren van virtuele machines lijkt in Azure en Azure-Stack. Zie de volgende Azure onderwerp voor meer informatie over het gebruik van PowerShell:<br>[Maken en beheren van Windows virtuele machines met de Azure PowerShell-module](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-manage-vm#understand-vm-sizes)|
|Client-SDK 's|Met C# voor het beheren van virtuele machines lijkt in Azure en Azure-Stack. Zie de volgende Azure onderwerp voor meer informatie:<br>[Maken en beheren van Windows-machines in Azure met C#](https://docs.microsoft.com/azure/virtual-machines/windows/csharp)|

### <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

U kunt de **Connect** knop in de Stack van Azure-portal verbinding maken met uw virtuele machine.

## <a name="next-steps"></a>Volgende stappen

* [Overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md)

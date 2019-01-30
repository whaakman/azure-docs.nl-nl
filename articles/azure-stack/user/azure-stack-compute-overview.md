---
title: Kennismaking met virtuele machines in Azure Stack
description: Meer informatie over virtuele machines van Azure Stack
services: azure-stack
author: sethmanheim
manager: femila
ms.service: azure-stack
ms.topic: get-started-article
ms.date: 01/05/2019
ms.author: sethm
ms.reviewer: kivenkat
ms.lastreviewed: 01/05/2019
ms.openlocfilehash: 13454e7dfb18e4d0e87b9b041b53dbef155153a8
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55251636"
---
# <a name="introduction-to-azure-stack-virtual-machines"></a>Kennismaking met virtuele machines in Azure Stack

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Azure Stack biedt virtuele machines (VM's) als één type van een on-demand en schaalbare computing-resource. Wanneer u meer controle over de computeromgeving nodig hebt, kunt u een virtuele machine kiezen. Dit artikel bevat informatie voordat u uw eerste virtuele machine maakt.

Een VM in Azure Stack biedt u de flexibiliteit van virtualisatie zonder de noodzaak voor het beheren van clusters of afzonderlijke computers. U moet wel aan de virtuele machine onderhouden door taken uitvoeren zoals het configureren, patchen en installeren van de software die erop wordt uitgevoerd.

U kunt virtuele machines van Azure Stack op verschillende manieren gebruiken. Bijvoorbeeld:

- **Ontwikkelen en testen**  
    Virtuele machines in Azure Stack kunt u een computer met een specifieke configuratie vereist om code te maken en testen van een toepassing.

- **Toepassingen in de cloud**  
    Omdat de vraag naar uw toepassing kan variëren, kan het zinvol economische uit te voeren op een virtuele machine in Azure Stack. U betaalt voor extra virtuele machines wanneer u ze nodig hebt en ze schakelt uit wanneer u dit niet doet.

- **Uitgebreid datacenter**  
    Virtuele machines in een Azure Stack-netwerk kunnen worden verbonden met het netwerk van uw organisatie of met Azure.

De virtuele machines die uw toepassing gebruikt kunnen omhoog of uitschalen, naar wat vereist is om te voldoen aan uw behoeften.

## <a name="what-do-i-need-to-think-about-before-creating-a-vm"></a>Wat moet ik over nadenken voordat het maken van een virtuele machine

Er zijn altijd Ontwerpoverwegingen bij het samenstellen van de infrastructuur van een toepassing in Azure Stack. Deze aspecten van een virtuele machine zijn belangrijk om na te denken over voordat u begint met het maken van uw infrastructuur:

- De namen van de toepassingsresources van uw.
- De grootte van de virtuele machine.
- Het maximum aantal virtuele machines dat kan worden gemaakt.
- Het besturingssysteem waarop de virtuele machine wordt uitgevoerd.
- De configuratie van de virtuele machine nadat deze is gestart.
- De gerelateerde resources die de virtuele machine nodig heeft.

### <a name="naming"></a>Naamgeving

Een virtuele machine heeft een naam krijgt toegewezen en er een computernaam geconfigureerd als onderdeel van het besturingssysteem. De naam van een VM mag uit maximaal 15 tekens bestaan.

Als u Azure Stack om aan te maken van de besturingssysteemschijf, zijn de computernaam en naam van de virtuele machine hetzelfde. Als u uploaden en uw eigen installatiekopie gebruikt die een eerder geconfigureerd besturingssysteem bevat en een virtuele machine maken, is het mogelijk dat de namen van de andere. Wanneer u uw eigen installatiekopiebestand uploaden als een best practice, zorg ervoor dat de computernaam in het besturingssysteem en de naam van de virtuele machine zijn hetzelfde.

### <a name="vm-size"></a>VM-grootte

De grootte van de virtuele machine die u gebruikt, wordt bepaald door de werkbelasting die u wilt uitvoeren. De grootte die u vervolgens kiest, bepaalt factoren als processorsnelheid, geheugen en opslagcapaciteit. Azure Stack biedt tal van grootten voor verschillende manieren van gebruik.

### <a name="vm-limits"></a>VM-limieten

Uw abonnement heeft standaard-quotumlimiet die gevolgen voor de implementatie van virtuele machines voor uw project hebben kan. De huidige limiet per abonnement is 20 VM's per regio.

### <a name="operating-system-disks-and-images"></a>Schijven en installatiekopieën voor een besturingssysteem

Virtuele machines maken gebruik van virtuele harde schijven (VHD's) voor de opslag van het besturingssysteem (OS) en de gegevens. VHD's worden ook gebruikt voor de installatiekopieën waarmee u een besturingssysteem kunt installeren. Azure Stack biedt een marketplace voor gebruik met verschillende versies en typen van besturingssystemen. Marketplace-installatiekopieën worden aangeduid met installatiekopie-uitgever, aanbieding, SKU en versie (de versie wordt meestal gespecificeerd als **nieuwste**.)

De volgende tabel laat zien hoe de gegevens voor een installatiekopie vinden:

|Methode|Beschrijving|
|---------|---------|
|Azure Stack-portal|De waarden worden automatisch opgegeven wanneer u een installatiekopie selecteert om te gebruiken.|
|PowerShell voor Azure Stack|`Get-AzureRMVMImagePublisher -Location "location"`<br>`Get-AzureRMVMImageOffer -Location "location" -Publisher "publisherName"`<br>`Get-AzureRMVMImageSku -Location "location" -Publisher "publisherName" -Offer "offerName"`|
|REST-API’s     |[Uitgevers van installatiekopieën weergeven](/rest/api/compute/platformimages/platformimages-list-publishers)<br>[Aanbiedingen van installatiekopieën weergeven](/rest/api/compute/platformimages/platformimages-list-publisher-offers)<br>[Afbeelding van lijst met SKU 's](/rest/api/compute/platformimages/platformimages-list-publisher-offer-skus)|

U kunt kiezen om te uploaden en uw eigen installatiekopie gebruiken. Als u dit doet, worden de naam van de uitgever, aanbieding en SKU niet gebruikt.

### <a name="extensions"></a>Extensies

VM-extensies geven uw VM meer mogelijkheden via de post-implementatieconfiguratie en geautomatiseerde taken.
Deze algemene taken kunnen worden uitgevoerd met extensies:

- **Aangepaste scripts uitvoeren**  
    De aangepaste scriptextensie helpt u bij het configureren van werkbelastingen op de virtuele machine door het uitvoeren van uw script wanneer de virtuele machine is ingericht.

- **Configuraties implementeren en beheren**  
    De extensie PowerShell Desired State Configuration (DSC) helpt u bij het instellen van DSC op een virtuele machine voor het beheren van configuraties en omgevingen.

- **Verzamelen van diagnostische gegevens**  
    De Azure Diagnostics-extensie kunt u de virtuele machine voor het verzamelen van diagnostische gegevens die kunnen worden gebruikt voor het bewaken van de status van uw toepassing configureren.

### <a name="related-resources"></a>Gerelateerde resources

De resources in de volgende tabel worden gebruikt door de virtuele machine en moeten bestaan of worden gemaakt wanneer de virtuele machine wordt gemaakt:

|Resource|Vereist|Beschrijving|
|---------|---------|---------|
|Resourcegroep|Ja|De VM moet zijn opgenomen in een resourcegroep.|
|Storage-account|Nee|De virtuele machine hoeft niet de storage-account voor het opslaan van de virtuele harde schijven als schijven beheerd. <br>De virtuele machine hoeft het opslagaccount voor het opslaan van de virtuele harde schijven als niet-beheerde schijven.|
|Virtueel netwerk|Ja|De VM moet lid zijn van een virtueel netwerk.|
|Openbaar IP-adres|Nee|Aan de VM kan een openbaar IP-adres worden toegewezen voor externe toegang.|
|Netwerkinterface|Ja|De netwerkinterface van de VM moet in het netwerk communiceren.|
|Gegevensschijven|Nee|De VM kan gegevensschijven bevatten om opslagmogelijkheden uit te breiden.|

## <a name="create-your-first-vm"></a>Uw eerste virtuele machine maken

U hebt verschillende mogelijkheden om een VM te maken. Uw keuze is afhankelijk van uw omgeving. De volgende tabel bevat informatie waarmee u aan de slag uw VM te maken:

|Methode|Artikel|
|---------|---------|
|Azure Stack-portal|Een Windows-machine maken met de Azure Stack-portal<br>[Een Linux-machine met behulp van de Azure Stack-portal maken](azure-stack-quick-linux-portal.md)|
|Sjablonen|Snelstartsjablonen voor Azure Stack bevinden zich op:<br> [https://github.com/Azure/AzureStack-QuickStart-Templates](https://github.com/Azure/AzureStack-QuickStart-Templates)|
|PowerShell|[Een Windows-machine maken met behulp van PowerShell in Azure Stack](azure-stack-quick-create-vm-windows-powershell.md)<br>[Een virtuele Linux-machine maken met behulp van PowerShell in Azure Stack](azure-stack-quick-create-vm-linux-powershell.md)|
|CLI|[Een Windows-machine maken met behulp van CLI in Azure Stack](azure-stack-quick-create-vm-windows-cli.md)<br>[Een virtuele Linux-machine maken met behulp van CLI in Azure Stack](azure-stack-quick-create-vm-linux-cli.md)|

## <a name="manage-your-vm"></a>Uw virtuele machine beheren

U kunt virtuele machines via een browser gebaseerde portal, opdrachtregelprogramma's met ondersteuning voor het uitvoeren van scripts of rechtstreeks via API's kunt beheren. Er zijn enkele veelvoorkomende beheertaken die u kunt uitvoeren:

- Informatie ophalen over een virtuele machine
- Verbinding maken met een virtuele machine
- Beschikbaarheid beheren
- Back-ups maken

### <a name="get-information-about-your-vm"></a>Informatie over uw VM ophalen

De volgende tabel ziet u enkele van de manieren waarop die u informatie over een VM kunt ophalen.

|Methode|Beschrijving|
|---------|---------|
|Azure Stack-portal|In het hubmenu op virtuele Machines en selecteer vervolgens de virtuele machine in de lijst. Op de pagina voor de virtuele machine hebt u toegang tot informatie inzien, waarden instellen en metrische gegevens controleren.|
|Azure PowerShell|Beheren van virtuele machines is in Azure en Azure Stack vergelijkbaar. Zie de volgende Azure onderwerp voor meer informatie over het gebruik van PowerShell:<br>[Maken en beheren van Windows VM's met de Azure PowerShell-module](../../virtual-machines/windows/tutorial-manage-vm.md#understand-vm-sizes)|
|Client-SDK 's|Met behulp van C# voor het beheren van virtuele machines is in Azure en Azure Stack vergelijkbaar. Zie de volgende Azure onderwerp voor meer informatie:<br>[Windows-machines in Azure met behulp van C# maken en beheren](../../virtual-machines/windows/csharp.md)|

### <a name="connect-to-your-vm"></a>Verbinding maken met uw VM

U kunt de **Connect** knop in de Azure Stack-portal verbinding maken met uw virtuele machine.

## <a name="next-steps"></a>Volgende stappen

- [Overwegingen voor virtuele Machines in Azure Stack](azure-stack-vm-considerations.md)

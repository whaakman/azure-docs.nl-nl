---
title: Meerdere IP-adressen voor virtuele machines in Azure - sjabloon | Microsoft Docs
description: Informatie over meerdere IP-adressen toewijzen aan een virtuele machine met een Azure Resource Manager-sjabloon.
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/08/2016
ms.author: jdial
ms.openlocfilehash: d4b189fb23dda1167c4f6b17b618c718d32dd98f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-an-azure-resource-manager-template"></a>Meerdere IP-adressen toewijzen aan virtuele machines met een Azure Resource Manager-sjabloon

[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]

Dit artikel wordt uitgelegd hoe u een virtuele machine (VM) maken via het Azure Resource Manager-implementatiemodel met een Resource Manager-sjabloon. Meerdere openbare en particuliere IP-adressen kunnen niet worden toegewezen aan de dezelfde NIC bij het implementeren van een virtuele machine via het klassieke implementatiemodel. Lees voor meer informatie over Azure-implementatiemodellen de [begrijpen implementatiemodellen](../resource-manager-deployment-model.md) artikel.

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name="template-description"></a>Beschrijving van sjabloon

Implementeren van een sjabloon kunt u snel en consistent Azure-resources met andere configuratiewaarden maken. Lees de [overzicht voor Resource Manager-sjabloon](../azure-resource-manager/resource-manager-template-walkthrough.md?toc=%2fazure%2fvirtual-network%2ftoc.json) artikel als u niet bekend met Azure Resource Manager-sjablonen bent. De [implementeren van een virtuele machine met meerdere IP-adressen](https://azure.microsoft.com/resources/templates/101-vm-multiple-ipconfig) sjabloon worden gebruikt in dit artikel.

<a name="resources"></a>De sjabloon implementeren, maakt de volgende bronnen:

|Resource|Naam|Beschrijving|
|---|---|---|
|Netwerkinterface|*myNic1*|De drie IP-configuraties beschreven in de sectie scenario van dit artikel zijn gemaakt en toegewezen aan deze NIC.|
|Openbare IP-adres resource|2 worden gemaakt: *myPublicIP* en *myPublicIP2*|Deze resources statische openbare IP-adressen zijn toegewezen en zijn toegewezen aan de *IPConfig 1* en *IPConfig 2* IP-configuraties beschreven in het scenario.|
|VM|*myVM1*|Een standaard DS3 VM.|
|Virtueel netwerk|*myVNet1*|Een virtueel netwerk met één subnet met de naam *mySubnet*.|
|Storage-account|Uniek is voor de implementatie|Een opslagaccount.|

<a name="parameters"></a>Wanneer u de sjabloon implementeert, moet u waarden voor de volgende parameters opgeven:

|Naam|Beschrijving|
|---|---|
|adminUsername|Gebruikersnaam van de beheerder. De gebruikersnaam moet voldoen aan [gebruikersnaam van Azure-vereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json).|
|adminPassword|Het wachtwoord moet voldoen aan beheerderswachtwoord [Azure wachtwoordvereisten](../virtual-machines/windows/faq.md?toc=%2fazure%2fvirtual-network%2ftoc.json#what-are-the-password-requirements-when-creating-a-vm).|
|dnsLabelPrefix|DNS-naam voor PublicIPAddressName1. De DNS-naam wordt omgezet in een van de openbare IP-adressen toegewezen aan de virtuele machine. De naam moet uniek zijn binnen de Azure-regio (locatie) maken van de virtuele machine in.|
|dnsLabelPrefix1|DNS-naam voor PublicIPAddressName2. De DNS-naam wordt omgezet in een van de openbare IP-adressen toegewezen aan de virtuele machine. De naam moet uniek zijn binnen de Azure-regio (locatie) maken van de virtuele machine in.|
|OSVersion|De Windows-/ Linux-versie voor de virtuele machine. Het besturingssysteem is een volledig patches afbeelding van de opgegeven Windows of Linux-versie geselecteerd.|
|imagePublisher|De uitgever van de installatiekopie van Windows of Linux voor de geselecteerde virtuele machine.|
|imageOffer|De Windows-/ Linux-afbeelding voor de geselecteerde virtuele machine.|

Elk van de resources die zijn geïmplementeerd door de sjabloon is geconfigureerd met verschillende standaardinstellingen. U kunt deze instellingen via een van de volgende manieren weergeven:

- **De sjabloon weergeven op GitHub:** als u bekend met sjablonen bent, kunt u weergeven met de instellingen in de [sjabloon](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json).
- **Bekijk de instellingen na de implementatie:** als u niet bekend met sjablonen bent, kunt u de sjabloon die met behulp van de stappen in een van de volgende secties implementeren en vervolgens de instellingen bekijken na de implementatie.

U kunt de Azure-portal, PowerShell of de Azure-opdrachtregelinterface (CLI) gebruiken om de sjabloon te implementeren. Alle methoden opleveren hetzelfde resultaat. Volg de stappen in een van de volgende secties voor het implementeren van de sjabloon:

## <a name="deploy-using-the-azure-portal"></a>Implementeren met behulp van de Azure-portal

Voor het implementeren van de sjabloon met de Azure portal, moet u de volgende stappen uitvoeren:

1. Wijzig de sjabloon, indien gewenst. De sjabloon implementeert de bronnen en instellingen die worden vermeld de [resources](#resources) sectie van dit artikel. Lees voor meer informatie over sjablonen en het schrijven van deze de [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md?toc=%2fazure%2fvirtual-network%2ftoc.json)artikel.
2. De sjabloon implementeren met een van de volgende methoden:
    - **Selecteer de sjabloon die in de portal:** Voer de stappen in de [resources met aangepaste sjabloon implementeren](../azure-resource-manager/resource-group-template-deploy-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json#deploy-resources-from-custom-template) artikel. Kies de bestaande sjabloon met de naam *101 vm-meerdere ipconfig*.
    - **Rechtstreeks:** klikt u op de knop Volgende om de sjabloon rechtstreeks in de portal:<a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-vm-multiple-ipconfig%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

Ongeacht welke methode u kiest, moet u waarden voor de [parameters](#parameters) eerder in dit artikel worden vermeld. Nadat de virtuele machine is geïmplementeerd, verbinding maken met de virtuele machine en de persoonlijke IP-adressen toevoegen aan het besturingssysteem die u hebt geïmplementeerd via de stappen in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel. Het openbare IP-adressen niet aan het besturingssysteem toevoegen.

## <a name="deploy-using-powershell"></a>Implementeren met behulp van PowerShell

Voor het implementeren van de sjabloon die met behulp van PowerShell, moet u de volgende stappen uitvoeren:

1. De sjabloon te implementeren via de stappen in de [implementeren van een sjabloon met PowerShell](../azure-resource-manager/resource-group-template-deploy-cli.md) artikel. Het artikel beschrijft meerdere opties voor het implementeren van een sjabloon. Als u wilt implementeren met behulp van de `-TemplateUri parameter`, de URI voor deze sjabloon is *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Als u wilt implementeren met behulp van de `-TemplateFile` parameter, Kopieer de inhoud van de [sjabloonbestand](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) vanuit GitHub in een nieuw bestand op uw computer. Wijzig de sjablooninhoud, indien gewenst. De sjabloon implementeert de bronnen en instellingen die worden vermeld de [resources](#resources) sectie van dit artikel. Lees voor meer informatie over sjablonen en het schrijven van deze de [Azure Resource Manager-sjablonen samenstellen ](../azure-resource-manager/resource-group-authoring-templates.md)artikel.

    Ongeacht de optie die u kiest voor het implementeren van de sjabloon met, dient u waarden voor de parameterwaarden die worden vermeld in de [parameters](#parameters) sectie van dit artikel. Als u een parameterbestand met parameters opgeven, Kopieer de inhoud van de [parameterbestand](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) vanuit GitHub in een nieuw bestand op uw computer. Wijzig de waarden in het bestand. Gebruik van het bestand dat u hebt gemaakt als de waarde voor de `-TemplateParameterFile` parameter.

    Om te bepalen van geldige waarden voor de OSVersion, ImagePublisher en imageOffer parameters, voer de stappen in de [navigeren door en selecteer Windows-VM-installatiekopieën artikel](../virtual-machines/windows/cli-ps-findimage.md) artikel.

    >[!TIP]
    >Als u niet zeker weet of een dnslabelprefix beschikbaar is, voert u de `Test-AzureRmDnsAvailability -DomainNameLabel <name-you-want-to-use> -Location <location>` opdracht om erachter te komen. Als deze beschikbaar is, de opdracht retourneert `True`.

2. Nadat de virtuele machine is geïmplementeerd, verbinding maken met de virtuele machine en de persoonlijke IP-adressen toevoegen aan het besturingssysteem die u hebt geïmplementeerd via de stappen in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel. Het openbare IP-adressen niet aan het besturingssysteem toevoegen.

## <a name="deploy-using-the-azure-cli"></a>Implementeren met behulp van de Azure CLI

Voor het implementeren van de sjabloon die de Azure CLI 1.0 gebruikt, moet u de volgende stappen uitvoeren:

1. De sjabloon te implementeren via de stappen in de [implementeren van een sjabloon met de Azure CLI](../azure-resource-manager/resource-group-template-deploy-cli.md) artikel. Het artikel beschrijft meerdere opties voor het implementeren van de sjabloon. Als u wilt implementeren met behulp van de `--template-uri` (-f), de URI voor deze sjabloon is *https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json*. Als u wilt implementeren met behulp van de `--template-file` (-f) parameter, Kopieer de inhoud van de [sjabloonbestand](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.json) vanuit GitHub in een nieuw bestand op uw computer. Wijzig de sjablooninhoud, indien gewenst. De sjabloon implementeert de bronnen en instellingen die worden vermeld de [resources](#resources) sectie van dit artikel. Lees voor meer informatie over sjablonen en het schrijven van deze de [Azure Resource Manager-sjablonen samenstellen ](../azure-resource-manager/resource-group-authoring-templates.md)artikel.

    Ongeacht de optie die u kiest voor het implementeren van de sjabloon met, dient u waarden voor de parameterwaarden die worden vermeld in de [parameters](#parameters) sectie van dit artikel. Als u een parameterbestand met parameters opgeven, Kopieer de inhoud van de [parameterbestand](https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-multiple-ipconfig/azuredeploy.parameters.json) vanuit GitHub in een nieuw bestand op uw computer. Wijzig de waarden in het bestand. Gebruik van het bestand dat u hebt gemaakt als de waarde voor de `--parameters-file` (-e) parameter.

    Om te bepalen van geldige waarden voor de OSVersion, ImagePublisher en imageOffer parameters, voer de stappen in de [navigeren door en selecteer Windows-VM-installatiekopieën artikel](../virtual-machines/windows/cli-ps-findimage.md) artikel.

2. Nadat de virtuele machine is geïmplementeerd, verbinding maken met de virtuele machine en de persoonlijke IP-adressen toevoegen aan het besturingssysteem die u hebt geïmplementeerd via de stappen in de [toevoegen IP-adressen naar een VM-besturingssysteem](#os-config) sectie van dit artikel. Het openbare IP-adressen niet aan het besturingssysteem toevoegen.

[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]

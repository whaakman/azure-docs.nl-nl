---
title: Weergeven en wijzigen van hostnamen | Microsoft Docs
description: Het weergeven en wijzigen van de hostnamen voor Azure virtual machines, web- en werkrollen voor naamomzetting
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/30/2018
ms.author: genli
ms.openlocfilehash: 4a836e0f5e47d6ef91e11fd1a0076859b54ed8e6
ms.sourcegitcommit: dbfd977100b22699823ad8bf03e0b75e9796615f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/30/2018
ms.locfileid: "50243024"
---
# <a name="viewing-and-modifying-hostnames"></a>Weergeven en wijzigen van hostnamen
Als u wilt toestaan dat uw rolinstanties worden verwezen door de hostnaam van de, moet u de waarde voor de hostnaam instellen in het configuratiebestand van de service voor elke rol. U doet dit door het toevoegen van de gewenste hostnaam in de **vmName** kenmerk van de **rol** element. De waarde van de **vmName** kenmerk wordt gebruikt als basis voor de hostnaam van elke rolinstantie. Bijvoorbeeld, als **vmName** is *webrole* en er zijn drie exemplaren van deze rol, de hostnamen van de exemplaren worden *webrole0*, *webrole1*, en *webrole2*. U hoeft niet om op te geven van de naam van een host voor virtuele machines in het configuratiebestand, omdat de hostnaam voor een virtuele machine is ingevuld op basis van de naam van de virtuele machine. Zie voor meer informatie over het configureren van een Microsoft Azure-service [configuratieschema voor Azure-Service (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Hostnamen weergeven
U kunt de hostnamen van virtuele machines en rolinstanties in een cloudservice weergeven met behulp van de onderstaande hulpprogramma's.

### <a name="service-configuration-file"></a>Serviceconfiguratiebestand
U kunt het serviceconfiguratiebestand voor een geïmplementeerde service uit downloaden de **configureren** blade van de service in Azure portal. U kunt vervolgens zoeken naar de **vmName** kenmerk voor de **rolnaam** element om de hostnaam te zien. Houd er rekening mee dat de naam van deze host wordt gebruikt als basis voor de hostnaam van de rolinstantie. Bijvoorbeeld, als **vmName** is *webrole* en er zijn drie exemplaren van deze rol, de hostnamen van de exemplaren worden *webrole0*, *webrole1*, en *webrole2*.

### <a name="remote-desktop"></a>Extern bureaublad
Nadat u extern bureaublad (Windows), Windows PowerShell voor externe toegang (Windows) of SSH (Linux en Windows)-verbindingen op uw virtuele machines of rolinstanties inschakelt, kunt u de naam van de host van een actieve verbinding met extern bureaublad op verschillende manieren weergeven:

* Hostnaam Typ achter de opdrachtprompt of de SSH-terminal.
* Typ ipconfig/alle bij de opdrachtprompt (alleen Windows).
* Naam van de computer weergeven in de instellingen van het systeem (alleen Windows).

### <a name="azure-service-management-rest-api"></a>Azure Service Management REST-API
Volg deze instructies uit een REST-client:

1. Zorg ervoor dat u hebt een clientcertificaat verbinding maken met de Azure portal. Als u wilt een certificaat hebt verkregen, volgt u de stappen die zijn gepresenteerd in [hoe: downloaden en importeren Publish Settings- en abonnementsgegevens](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Stelt u de vermelding van een koptekst met de naam x-ms-version met een waarde van 01-11-2013.
3. Een aanvraag verzenden in de volgende indeling: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<service-name\>?embed-detail=true
4. Zoek de **hostnaam** element voor elk **RoleInstance** element.

> [!WARNING]
> U kunt ook het interne domeinachtervoegsel voor je cloudservice uit het antwoord van REST-aanroep weergeven door het controleren van de **InternalDnsSuffix** -element, of door het uitvoeren van ipconfig/alle vanaf een opdrachtprompt in een extern bureaublad-sessie (Windows) of door cat /etc/resolv.conf uitgevoerd vanuit een terminal SSH (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Een hostnaam wijzigen
U kunt de hostnaam voor elke virtuele machine of rolinstantie wijzigen door het uploaden van een configuratiebestand voor de gewijzigde service of door het wijzigen van de computernaam van een extern bureaublad-sessie.

## <a name="next-steps"></a>Volgende stappen
[Naamomzetting (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Het configuratieschema van de Azure-Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network-configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093)

[DNS-instellingen met behulp van de configuratiebestanden netwerk opgeven](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)


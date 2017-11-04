---
title: Weergeven en wijzigen van hostnamen | Microsoft Docs
description: Het weergeven en wijzigen van de hostnamen voor Azure virtual machines, web- en werkrollen voor naamomzetting
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
ms.assetid: c668cd8e-4e43-4d05-acc3-db64fa78d828
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/27/2016
ms.author: jdial
ms.openlocfilehash: 9a3a1e1b58dcb828e2d2d09c18f1aab6d46051aa
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="viewing-and-modifying-hostnames"></a>Weergeven en wijzigen van hostnamen
Als u uw rolinstanties naar worden verwezen met de hostnaam, moet u de waarde voor de hostnaam instellen in het configuratiebestand van de service voor elke rol. U doet dit door het toevoegen van de gewenste hostnaam voor de **vmName** kenmerk van de **rol** element. De waarde van de **vmName** kenmerk wordt gebruikt als basis voor de hostnaam van elk rolexemplaar. Bijvoorbeeld, als **vmName** is *webrole* en er zijn drie exemplaren van deze rol, de hostnamen van de exemplaren *webrole0*, *webrole1*, en *webrole2*. U hoeft niet te geven van een hostnaam voor de virtuele machines in het configuratiebestand omdat de host-naam voor een virtuele machine is ingevuld op basis van de naam van de virtuele machine. Zie voor meer informatie over het configureren van een Microsoft Azure-service [configuratieschema voor Azure-Service (.cscfg-bestand)](https://msdn.microsoft.com/library/azure/ee758710.aspx)

## <a name="viewing-hostnames"></a>Hostnamen weergeven
U kunt de namen van de hosts van virtuele machines en rolexemplaren in een cloudservice weergeven met behulp van de onderstaande hulpprogramma's.

### <a name="azure-portal"></a>Azure Portal
U kunt de [Azure-portal](http://portal.azure.com) om de hostnamen voor virtuele machines op de overzichtsblade voor een virtuele machine weer te geven. Houd er rekening mee dat de blade ziet u een waarde voor **naam** en **hostnaam**. Hoewel ze in eerste instantie hetzelfde zijn, wordt er bij het wijzigen van de hostnaam niet de naam van de virtuele machine of rolinstantie gewijzigd.

Rolinstanties kunnen ook worden weergegeven in de Azure portal, maar wanneer u de exemplaren in een cloudservice, de naam van de host niet wordt weergegeven. Ziet u een naam voor elk exemplaar, maar deze naam staat niet voor de hostnaam.

### <a name="service-configuration-file"></a>Serviceconfiguratiebestand
U kunt het configuratiebestand van de service voor een geïmplementeerde service uit downloaden de **configureren** blade van de service in de Azure-portal. Vervolgens kunt u zoeken naar de **vmName** kenmerk voor de **rolnaam** element om de hostnaam te zien. Houd er rekening mee dat deze hostnaam wordt gebruikt als basis voor de hostnaam van elk rolexemplaar. Bijvoorbeeld, als **vmName** is *webrole* en er zijn drie exemplaren van deze rol, de hostnamen van de exemplaren *webrole0*, *webrole1*, en *webrole2*.

### <a name="remote-desktop"></a>Extern bureaublad
Nadat u extern bureaublad (Windows), Windows PowerShell voor externe toegang (Windows) of SSH (Linux- en Windows-) verbindingen met uw virtuele machines of rolinstanties inschakelt, kunt u de hostnaam van een actieve verbinding met extern bureaublad op verschillende manieren weergeven:

* Hostname Typ achter de opdrachtprompt of SSH-terminal.
* Typ ipconfig/alle bij de opdrachtprompt (alleen Windows).
* Naam van de computer weergeven in de instellingen van het systeem (alleen Windows).

### <a name="azure-service-management-rest-api"></a>Azure Service Management REST-API
Volg deze instructies in een REST-client:

1. Zorg ervoor dat u een clientcertificaat verbinding maken met de Azure-portal. Als u een clientcertificaat, volgt u de stappen die zijn gepresenteerd in [hoe: downloaden en importeren publicatie-instellingen en abonnementsgegevens](https://msdn.microsoft.com/library/dn385850.aspx). 
2. Stel een header fragment x-ms-version met een waarde van 2013-11-01.
3. Een aanvraag verzenden in de volgende indeling: https://management.core.windows.net/\<subscrition-id\>/services/hostedservices/\<servicenaam\>? insluiten detail = true
4. Zoek naar de **hostnaam** element voor elk **RoleInstance** element.

> [!WARNING]
> U kunt ook het interne domeinachtervoegsel voor uw cloudservice uit het antwoord REST-aanroep bekijken door het controleren van de **InternalDnsSuffix** element, of door het uitvoeren van ipconfig/alle vanaf een opdrachtprompt in een extern bureaublad-sessie (Windows) of door het uitvoeren van cat /etc/resolv.conf van een SSH-terminal (Linux).
> 
> 

## <a name="modifying-a-hostname"></a>Een hostnaam wijzigen
U kunt de hostnaam voor elke virtuele machine of rolinstantie wijzigen door het uploaden van een configuratiebestand gewijzigde service of door het wijzigen van de computernaam van een extern bureaublad-sessie.

## <a name="next-steps"></a>Volgende stappen
[Naamomzetting (DNS)](virtual-networks-name-resolution-for-vms-and-role-instances.md)

[Het configuratieschema Azure-Service (.cscfg)](https://msdn.microsoft.com/library/windowsazure/ee758710.aspx)

[Azure Virtual Network-configuratieschema](http://go.microsoft.com/fwlink/?LinkId=248093)

[DNS-instellingen met behulp van de configuratiebestanden netwerk opgeven](virtual-networks-specifying-a-dns-settings-in-a-virtual-network-configuration-file.md)


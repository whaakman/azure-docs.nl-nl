---
title: Virtuele machine van Azure-extensies en functies | Microsoft Docs
description: Meer informatie over welke Azure VM-extensies anre hoe u deze gebruikt met virtuele machines in Azure
services: virtual-machines-linux
documentationcenter: ''
author: danielsollondon
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 03/30/2018
ms.author: danis
ms.openlocfilehash: 01178995dbf9203082a6250ef256522bc1101e57
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Virtuele machine van Azure-extensies en functies
Azure virtuele machine (VM)-uitbreidingen zijn kleine toepassingen waarmee de configuratie en automatisering taken na de implementatie op Azure Virtual machines, kunt u bestaande installatiekopieën gebruiken en ze vervolgens aanpassen als onderdeel van uw implementaties u buiten het bedrijf van aangepaste ophalen installatiekopie bouwen.

De Azure-platform als host fungeert voor veel extensies die van het VM-configuratie, bewaking, beveiliging en hulpprogramma toepassingen variëren. Uitgevers een toepassing, klikt u vervolgens opnemen in een uitbreiding en vereenvoudigen van de installatie, dus u hoeft alleen verplichte parameters bevatten. 

 Er is een grote keuze van de eerste en uitbreidingen van derden, als de toepassing in de extensieopslagplaats niet bestaat, en vervolgens kunt u de extensie voor aangepaste scripts gebruiken en configureren van uw virtuele machine met uw eigen opdrachten en scripts.

Voorbeelden van belangrijke scenario's die uitbreidingen worden gebruikt voor:
* VM-configuratie, kunt u Powershell DSC (Desired State Configuration), Chef, Puppet en aangepaste scriptextensies VM configuratie agents installeren en configureren van uw virtuele machine. 
* AV-producten, zoals Symantec, ESET.
* VM beveiligingslek hulpprogramma zoals Qualys, Rapid7, HPE.
* Virtuele machine en App controle tooling zoals DynaTrace, Azure-netwerk-Watcher Site24x7 en Stackify.

Extensies zijn gekoppeld aan een nieuwe VM-implementatie. Ze kunnen bijvoorbeeld deel uitmaken van een grotere implementatie van toepassingen op de virtuele machine inrichten, configureren of uitvoeren op alle ondersteunde extensie gerund systemen post-implementatie.

## <a name="how-can-i-find-what-extensions-are-available"></a>Hoe vind ik welke extensies beschikbaar zijn?
U kunt beschikbare uitbreidingen bekijken in de VM-blade in de Portal onder extensies, dit is slechts een kleine hoeveelheid voor een volledige lijst kunt u de CLI-hulpprogramma's gebruiken, Zie [VM-extensies voor Linux detecteren](features-linux.md) en [ Detectie van VM-extensies voor Windows](features-windows.md).

## <a name="how-can-i-install-an-extension"></a>Hoe kan ik een uitbreiding installeren?
Azure VM-extensies kunnen worden beheerd via de Azure CLI 2.0, Azure PowerShell, Azure Resource Manager-sjablonen en de Azure-portal. Als u wilt proberen een uitbreiding, kunt u gaat u naar de Azure portal, selecteert de aangepaste Scriptextensie, doorgeven in een opdracht / script en de extensies worden uitgevoerd.

Als u dezelfde extensie u hebt toegevoegd wilt de de portal door CLI of Resource Manager-sjabloon, Zie de documentatie voor andere extensie, zoals [Windows aangepaste Scriptextensie](custom-script-windows.md) en [Linux aangepaste Scriptextensie](custom-script-linux.md).

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Hoe beheer ik extensie toepassing lifecycle?
U hoeft geen verbinding maken met een virtuele machine rechtstreeks te installeren of verwijderen van de extensie. Als de levenscyclus van de toepassing Azure-extensie is beheerd buiten de virtuele machine en geïntegreerd in de Azure-platform, ophalen u ook geïntegreerde status van de extensie.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Overige die ik over voor uitbreidingen nadenkt moet?
Extensies toepassingen installeren, zoals toepassingen er zijn een aantal vereisten voor uitbreidingen er is een lijst met ondersteunde Windows- en Linux-OSes en moet u beschikken over de Azure VM moeten worden geïnstalleerd. Sommige afzonderlijke VM-extensie-toepassingen kunnen hun eigen omgevingsvereisten, zoals toegang tot een eindpunt hebben.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de werking van de Linux-Agent en -extensies [Azure VM-extensies en functies voor Linux](features-linux.md).
* Zie voor meer informatie over de werking van de Windows-Gastagent en -extensies [Azure VM-extensies en functies voor Linux](features-windows.md).  
* Zie het installeren van de Windows-Gastagent [overzicht van Azure Windows virtuele machines Agent ](agent-windows.md).  
* Zie de Linux-Agent installeren [overzicht van Azure Linux VM-Agent ](agent-linux.md).  


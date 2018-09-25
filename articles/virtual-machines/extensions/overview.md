---
title: Virtuele machine van Azure-extensies en functies | Microsoft Docs
description: Welke Azure-VM-extensies anre leren om ze te gebruiken met Azure virtual machines
services: virtual-machines-linux
documentationcenter: ''
author: zroiy
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
ms.author: roiyz
ms.openlocfilehash: c54bcc1239c2d8b8a88d5e30b06f8856b73b47ae
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46960682"
---
# <a name="azure-virtual-machine-extensions-and-features"></a>Virtuele machine van Azure-extensies en functies
Extensies voor Azure-machines (VM) zijn kleine toepassingen die taken van configuratie- en automatiseringstaken na de implementatie op Azure Virtual machines bieden, kunt u de bestaande installatiekopieën gebruiken en ze vervolgens aanpassen als onderdeel van uw implementaties aan u buiten het bedrijf van aangepaste installatiekopie samenstellen.

Het Azure-platform als host fungeert voor veel extensies die van het VM-configuratie, bewaking, beveiliging en hulpprogramma voor toepassingen variëren. Uitgevers een toepassing, klikt u vervolgens omsluit u deze in een extensie en vereenvoudigen van de installatie, dus u hoeft alleen verplichte parameters opgeven. 

 Er is een grote keuze van de eerste en extensies van derden, als de toepassing in de extensieopslagplaats niet bestaat en u kunt de aangepaste scriptextensie gebruiken en configureren van uw virtuele machine met uw eigen scripts en opdrachten.

Voorbeelden van belangrijke scenario's die worden gebruikt voor extensies:
* VM-configuratie, kunt u Powershell DSC (Desired State Configuration), Chef, Puppet en aangepaste scriptextensies VM-configuratie van agents installeren en configureren van uw virtuele machine. 
* AV-producten, zoals Symantec, ESET.
* Virtuele machine door een beveiligingslek hulpprogramma, zoals Qualys, Rapid7, HPE.
* Virtuele machine en hulpprogramma's, zoals DynaTrace, Azure Network Watcher, Site24x7 en Stackify controle-App.

Extensies zijn gekoppeld aan een nieuwe VM-implementatie. Bijvoorbeeld, kunnen ze deel uitmaken van een grotere implementatie van toepassingen op virtuele machine inrichten, configureren of uitvoeren op alle ondersteunde extensie bediend systemen na de implementatie.

## <a name="how-can-i-find-what-extensions-are-available"></a>Hoe vind ik welke extensies beschikbaar zijn?
U kunt beschikbare uitbreidingen bekijken in de VM-blade in de Portal, onder extensies, dit is slechts een klein bedrag voor de volledige lijst, kunt u de CLI-hulpprogramma's gebruiken, Zie [VM-extensies voor Linux detecteren](features-linux.md) en [ Detectie van VM-extensies voor Windows](features-windows.md).

## <a name="how-can-i-install-an-extension"></a>Hoe kan ik een extensie installeren?
Azure VM-extensies kunnen worden beheerd met behulp van de Azure CLI, Azure PowerShell, Azure Resource Manager-sjablonen en Azure portal. Als u wilt proberen een uitbreiding, kunt u gaat u naar de Azure portal, selecteer de aangepaste Scriptextensie, vervolgens doorgeven in een opdracht / script en voert de extensies.

Als u dezelfde extensie die u hebt toegevoegd in de portal via de CLI of Resource Manager-sjabloon wilt, ziet u andere extensies documentatie, zoals [Windows Custom Script Extension](custom-script-windows.md) en [Linux Custom Script Extension](custom-script-linux.md).

## <a name="how-do-i-manage-extension-application-lifecycle"></a>Hoe kan ik extensie toepassingslevenscyclus beheren?
U hoeft geen verbinding maken met een virtuele machine rechtstreeks te installeren of verwijderen van de extensie. Als de levensduur van de extensie van Azure-toepassingen wordt beheerd buiten de virtuele machine en is geïntegreerd in het Azure-platform, ophalen u ook geïntegreerde status van de extensie.

## <a name="anything-else-i-should-be-thinking-about-for-extensions"></a>Iets anders die ik over voor uitbreidingen nadenken moet?
Extensies installeren van toepassingen, net als alle toepassingen die er zijn aan bepaalde vereisten, voor extensies er wordt een lijst met ondersteunde OSes voor Linux en Windows, en moet u beschikken over de Azure VM agents zijn geïnstalleerd. Sommige afzonderlijke VM-extensie toepassingen hebben hun eigen omgevingsvereisten beschreven, zoals toegang tot een eindpunt.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over de werking van de Linux-Agent en -extensies [Azure VM-extensies en functies voor Linux](features-linux.md).
* Zie voor meer informatie over de werking van de Windows Guest Agent en -extensies [Azure VM-extensies en functies voor Windows](features-windows.md).  
* Zie voor het installeren van de Windows-Gastagent [overzicht van Azure Windows VM Agent ](agent-windows.md).  
* Zie voor het installeren van de Linux-Agent, [overzicht van Azure Linux Virtual Machine Agent ](agent-linux.md).  


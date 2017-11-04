---
title: Het oplossen van problemen OpenShift implementatie in Azure | Microsoft Docs
description: Problemen met implementatie OpenShift in Azure oplossen
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldw
manager: najoshi
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 
ms.author: haroldw
ms.openlocfilehash: ea3664870480f6ed170972a5f52940dc4a852219
ms.sourcegitcommit: b979d446ccbe0224109f71b3948d6235eb04a967
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2017
---
# <a name="troubleshooting-openshift-deployment-in-azure"></a>Problemen met implementatie OpenShift in Azure oplossen

Als het cluster OpenShift niet met succes is geïmplementeerd, zijn er bepaalde taken voor het oplossen van problemen die kunnen worden uitgevoerd als u het probleem wilt beperken. De implementatiestatus weergeven en vergelijken met de volgende lijst met afsluitcodes.

- Afsluitcode 3: Uw Red Hat abonnement gebruikersnaam / wachtwoord of organisatie-ID-activeringscode is onjuist
- Afsluitcode 4: Uw Red Hat Pool-ID is onjuist of er zijn geen rechten beschikbaar
- Afsluitcode 5: Kan niet worden ingericht Docker Thin groep Volume
- Afsluitcode 6: OpenShift Cluster installatie is mislukt
- Afsluitcode 7: OpenShift clusterinstallatie is voltooid maar Azure Cloud Provider configuratie is mislukt - master config op knooppunt Master probleem
- Afsluitcode 8: OpenShift Cluster installatie is voltooid maar Azure Cloud Provider configuratie is mislukt - knooppunt config op knooppunt Master probleem
- Afsluitcode 9: OpenShift Cluster installatie is voltooid maar Azure Cloud Provider configuratie is mislukt - knooppunt config op probleem Infra of App-knooppunt
- Afsluitcode 10: OpenShift clusterinstallatie geslaagd maar Azure Cloud Provider configuratie is mislukt: knooppunten Master te corrigeren of er kan geen Master niet instellen als unschedulable
- Afsluitcode 11: De metrische gegevens kan niet worden geïmplementeerd
- Afsluitcode 12: Logboekregistratie kan niet worden geïmplementeerd

Het OpenShift Cluster hebt geïnstalleerd voor afsluitcodes 7-10, maar configuratie van de Azure-Cloud-Provider is mislukt. U kunt SSH naar de Master-knooppunt (oorsprong) of Bastionomgeving-knooppunt (Container Platform) en van er SSH aan elk van de knooppunten in het cluster en los de problemen.

Een veelvoorkomende oorzaak van de mislukt met afsluitcode Codes 7 - 9 is de Service-Principal heeft geen juiste machtigingen voor het abonnement of de resourcegroep. Als dit inderdaad het probleem, wijs de juiste machtigingen en het script die een alle daaropvolgende scripts niet handmatig opnieuw uitvoeren.
Zorg ervoor dat de service die is mislukt (bijvoorbeeld systemctl herstart atomic-openshift-node.service) voordat u de scripts opnieuw te starten.

Voor meer probleemoplossing SSH in uw knooppunt Master op poort 2200 (oorsprong) of een Bastionomgeving knooppunt op poort 22 (Container Platform). U moet hoofdmap (sudo su-) en navigeer vervolgens naar de volgende map: /var/lib/waagent/custom-script/download

Hier ziet u een map met de naam '0' en '1'. In elk van deze mappen ziet u twee bestanden: stderr en stdout. U kunt zoeken door deze bestanden om te bepalen waar de fout is opgetreden.

---
title: Problemen met implementatie OpenShift in Azure oplossen | Microsoft Docs
description: Problemen met de implementatie van OpenShift in Azure.
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
ms.openlocfilehash: 35e554d3a9c7e7d56546ae9723c33eb59e906472
ms.sourcegitcommit: 6a22af82b88674cd029387f6cedf0fb9f8830afd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/11/2017
---
# <a name="troubleshoot-openshift-deployment-in-azure"></a>Problemen met implementatie OpenShift in Azure oplossen

Als uw cluster OpenShift niet met succes is geïmplementeerd, kunt u deze taken om het probleem vast te stellen het oplossen van problemen. De implementatiestatus weergeven en vergelijken met de volgende lijst met afsluitcodes:

- Afsluitcode 3: uw Red Hat abonnement-gebruikersnaam / wachtwoord of organisatie-ID-activeringscode is onjuist
- Afsluitcode 4: uw Red Hat Pool-ID is onjuist of er zijn geen rechten beschikbaar
- Afsluitcode 5: kan niet worden ingericht Docker Thin groep Volume
- Afsluitcode 6: OpenShift Cluster de installatie is mislukt
- Afsluitcode 7: OpenShift Cluster-installatie is voltooid maar Azure Cloud Solution Provider configuratie is mislukt - master config op knooppunt Master probleem
- Afsluitcode 8: OpenShift Cluster-installatie is voltooid maar Azure Cloud Solution Provider configuratie is mislukt - knooppunt config op knooppunt Master probleem
- Afsluitcode 9: OpenShift Cluster-installatie is voltooid maar Azure Cloud Solution Provider configuratie is mislukt - knooppunt config op probleem Infra of App-knooppunt
- Afsluitcode 10: OpenShift clusterinstallatie geslaagd maar Azure Cloud Solution Provider-configuratie is mislukt: knooppunten Master te corrigeren of er kan geen Master niet instellen als unschedulable
- Afsluitcode 11: metrische gegevens kan niet worden geïmplementeerd
- Afsluitcode 12: logboekregistratie kan niet worden geïmplementeerd

Voor afsluitcodes 7-10, het cluster OpenShift is geïnstalleerd, maar de configuratie van Azure Cloud Solution Provider is mislukt. U kunt SSH naar het hoofdknooppunt (OpenShift oorsprong) of de bastionomgeving-knooppunt (OpenShift Container Platform) en van SSH aan elk clusterknooppunt er de fouten te herstellen.

Een veelvoorkomende oorzaak van de fouten met afsluitcodes 7-9, is de service-principal heeft geen juiste machtigingen voor het abonnement of de resourcegroep. Als dit het probleem, wijs de juiste machtigingen toe en handmatig opnieuw uitvoeren van het script dat is mislukt en alle latere scripts.

Zorg ervoor dat de service die is mislukt (bijvoorbeeld systemctl herstart atomic-openshift-node.service) voordat u de scripts opnieuw te starten.

Voor meer probleemoplossing SSH in uw basispagina knooppunt op poort 2200 (oorsprong) of het knooppunt van de bastionomgeving op poort 22 (Container Platform). U moet zich in de hoofdmap (sudo su-) en blader vervolgens naar de volgende map: /var/lib/waagent/custom-script/download.

Hier ziet u de mappen met de naam '0' en '1'. In elk van deze mappen die u ziet twee bestanden: 'stderr' en "stdout." Bekijk deze bestanden om te bepalen waar de fout is opgetreden.

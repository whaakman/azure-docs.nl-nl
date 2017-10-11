---
title: Azure Service Fabric-Containers controle en diagnostische gegevens | Microsoft Docs
description: Informatie over het bewaken en onderzoeken van containers gedirigeerd op Microsoft Azure Service Fabric met OMS van Containers oplossing.
services: service-fabric
documentationcenter: .net
author: dkkapur
manager: timlt
editor: 
ms.assetid: 
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 05/10/2017
ms.author: dekapur
ms.openlocfilehash: 874c1a5c4b399ff2254072b7282f05d83a005cc3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="monitoring-windows-server-containers-with-oms"></a>Bewaking van Windows Server-containers met OMS

## <a name="oms-containers-solution"></a>OMS Containers oplossing

Het team Operations Management Suite (OMS) heeft een oplossing Containers voor diagnostische gegevens en de bewaking voor containers gepubliceerd. Deze oplossing is samen met hun Service Fabric-oplossing een uitstekend hulpprogramma voor het bewaken van containerimplementaties gedirigeerd op Service Fabric. Hier volgt een eenvoudig voorbeeld van het dashboard in de oplossing ziet er als:

![Basic OMS-Dashboard](./media/service-fabric-diagnostics-containers-windowsserver/oms-containers-dashboard.png)

Deze verzamelt ook verschillende soorten logboeken die in de OMS-logboekanalyse-hulpprogramma kunnen worden opgevraagd en kunnen worden gebruikt voor het visualiseren van alle metrische gegevens of gebeurtenissen wordt gegenereerd. De logboek-typen die zijn verzameld zijn:

1. ContainerInventory: geeft informatie over de containerlocatie, naam en installatiekopieën
2. ContainerImageInventory: informatie over geïmplementeerde installatiekopieën, met inbegrip van id's of grootten
3. ContainerLog: specifieke foutenlogboeken, docker-Logboeken (stdout, enzovoort) en andere items
4. ContainerServiceLog: docker-daemon opdrachten die zijn uitgevoerd
5. Prestaties: prestatiemeteritems met inbegrip van container cpu, geheugen, netwerkverkeer, schijf-i/o en aangepaste metrische gegevens van de hostmachines

In dit artikel bevat informatie over de stappen die nodig zijn voor het instellen van de bewaking van de container voor uw cluster. Bekijk voor meer informatie over de OMS-Containers oplossing hun [documentatie](../log-analytics/log-analytics-containers.md).

## <a name="1-set-up-a-service-fabric-cluster"></a>1. Een Service Fabric-cluster instellen

Maak een cluster met de Azure Resource Manager-sjabloon gevonden [hier](https://github.com/dkkapur/Service-Fabric/tree/master/ARM%20Templates/SF%20OMS%20Sample). Zorg ervoor dat een unieke naam van de OMS-werkruimte toevoegen. Deze sjabloon wordt ook standaard tot het implementeren van een cluster in de preview-versie van Service Fabric (v255.255), wat betekent dat deze kan niet worden gebruikt in productie en kan niet worden bijgewerkt naar een andere Service Fabric-versie. Als u deze sjabloon gebruiken voor lange termijn of productie, wijzigt u de versie op een stabiele versie getal.

Zodra het cluster is ingesteld, moet u bevestigen dat u het juiste certificaat hebt geïnstalleerd en controleer of u verbinding kunnen bent maken met het cluster.

Bevestig dat de resourcegroep is ingesteld kop correct door aan de [Azure-portal](https://portal.azure.com/) en de implementatie te zoeken. De resourcegroep moet bevatten alle Service Fabric-resources en hebt ook een Log Analytics-oplossing, evenals een Service Fabric-oplossing.

Voor het wijzigen van een bestaand Service Fabric-cluster:
* Controleer of de diagnostische gegevens is ingeschakeld (als dit niet het geval is, het inschakelen via [bijwerken van de virtuele-machineschaalset](/rest/api/virtualmachinescalesets/create-or-update-a-set))
* Een OMS-werkruimte toevoegen door het maken van een oplossing 'Service Fabric Analytics' via Azure Marketplace
* De gegevensbronnen van de Service Fabric-oplossing om op te halen gegevens uit de juiste Azure Storage (ingesteld door af) bewerken in de resourcegroep die de cluster in gebruik is
* Toevoegen van de agent als een [-extensie voor de virtuele-machineschaalset](/powershell/module/azurerm.compute/add-azurermvmssextension) via PowerShell of via het bijwerken van de virtuele machine schaalset (dezelfde koppeling als hierboven om te wijzigen van de Resource Manager-sjabloon)

## <a name="2-deploy-a-container"></a>2. Een container implementeren

Als het cluster gereed is en u hebt gecontroleerd of u toegang hebt, implementeert u een container voor het. Als u kiest voor het gebruik van de preview-versie zoals ingesteld door de sjabloon, kunt u ook verkennen van Service Fabric nieuwe docker compose functionaliteit. Vergeet dat de eerste keer dat de installatiekopie van een container wordt geïmplementeerd naar een cluster duurt enkele minuten om de installatiekopie, afhankelijk van de grootte te downloaden.

## <a name="3-add-the-containers-solution"></a>3. De oplossing Containers toevoegen

In de Azure portal, maakt u een bron Containers (onder de bewaking en beheer categorie) via Azure Marketplace. 

![Containers oplossing toevoegen](./media/service-fabric-diagnostics-containers-windowsserver/containers-solution.png)

In de stap het maken van aanvraagt het een OMS-werkruimte. Selecteer de versie die is gemaakt met de bovenstaande implementatie. Deze stap voegt een oplossing Containers in de OMS-werkruimte en wordt automatisch gedetecteerd door de OMS-agent die is geïmplementeerd door de sjabloon. De agent wordt gestart met het verzamelen van gegevens over de containers processen in het cluster en in ongeveer 10-15 minuten, ziet u het lichte oplossing om met gegevens zoals in de installatiekopie van het bovenstaande dashboard.

## <a name="4-next-steps"></a>4. Volgende stappen

OMS biedt verschillende hulpprogramma's in de werkruimte om als nuttiger voor u. Bekijk de volgende opties voor het aanpassen van de oplossing voor uw behoeften:
- Familiarized ophalen met de [zoeken en uitvoeren van query's in logboek registreren](../log-analytics/log-analytics-log-searches.md) functies die worden aangeboden als onderdeel van logboekanalyse
- Configureer de OMS-agent om op te halen specifieke prestatiemeteritems (Ga naar de startpagina van de werkruimte > Instellingen > gegevens > Windows-prestatiemeteritems)
- Configureren van OMS om in te stellen [geautomatiseerde waarschuwingen](../log-analytics/log-analytics-alerts.md) regels om te helpen detecteren en diagnostische gegevens
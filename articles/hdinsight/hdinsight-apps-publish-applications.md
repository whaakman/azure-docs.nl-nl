---
title: Azure HDInsight-toepassingen publiceren
description: Informatie over het maken van een HDInsight-toepassing en publiceert u het in de Azure Marketplace.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 05/14/2018
ms.author: hrasheed
ms.openlocfilehash: fd6d47d8300872f5bcdf36aec492289f93858504
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55814183"
---
# <a name="publish-an-hdinsight-application-in-the-azure-marketplace"></a>Een HDInsight-toepassing publiceren in de Azure Marketplace
U kunt een Azure HDInsight-toepassing installeren op een Linux gebaseerde HDInsight-cluster. In dit artikel leert u hoe u een HDInsight-toepassing publiceren in de Azure Marketplace. Raadpleeg voor algemene informatie over het publiceren op Azure Marketplace [een aanbieding publiceren in de Azure Marketplace](../marketplace/marketplace-publishers-guide.md).

HDInsight-toepassingen gebruiken het *doen om uw eigen licentie (BYOL)* model. In een BYOL-scenario is de provider van een toepassing die verantwoordelijk is voor de toepassing aan gebruikers van app-licentieverlening. Appgebruikers betalen alleen voor de Azure-resources die ze, zoals het HDInsight-cluster, en het virtuele machines en de knooppunten van het cluster maken. Facturering voor de toepassing zelf niet op dit moment wordt uitgevoerd in Azure.

Zie voor meer informatie deze HDInsight-toepassing-gerelateerde artikelen:

* [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md). Meer informatie over het installeren van een HDInsight-toepassing op uw clusters.
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md). Informatie over het installeren en testen van aangepaste HDInsight-toepassingen.

## <a name="prerequisites"></a>Vereisten
Indienen van uw aangepaste toepassing in de Marketplace, eerst [maken en testen van uw aangepaste toepassing](hdinsight-apps-install-custom-applications.md).

Ook moet u uw ontwikkelaarsaccount registreren. Zie voor meer informatie, [een aanbieding publiceren in de Azure Marketplace](../marketplace/marketplace-publishers-guide.md) en [maken van een Microsoft Developer-account](../marketplace/marketplace-publishers-guide.md).

## <a name="define-the-application"></a>De toepassing definiëren
Twee stappen worden uitgevoerd in de publicatie van toepassingen in de Marketplace. Eerst definieert u een *createUiDef.json* bestand. Het createUiDef.json-bestand geeft aan welke clusters de toepassing compatibel is. Vervolgens publiceert u de sjabloon van de Azure-portal. Hier volgt een voorbeeld createUiDef.json-bestand:

```json
{
    "handler": "Microsoft.HDInsight",
    "version": "0.0.1-preview",
    "clusterFilters": {
        "types": ["Hadoop", "HBase", "Storm", "Spark"],
        "versions": ["3.6"]
    }
}
```

| Veld | Description | Mogelijke waarden |
| --- | --- | --- |
| typen |De clustertypen waarmee de toepassing compatibel is. |Hadoop, HBase, Storm, Spark (of een combinatie hiervan) |
| versies |De HDInsight-clustertypen waarmee de toepassing compatibel is. |3.4 |

## <a name="application-installation-script"></a>Script voor installatie van toepassing
Wanneer een toepassing wordt geïnstalleerd op een cluster (hetzij op een bestaand cluster of op een nieuwe), wordt een edge-knooppunt gemaakt. Het installatiescript van de toepassing wordt uitgevoerd op het edge-knooppunt.

  > [!IMPORTANT]  
  > De naam van het installatiescript van de toepassing moet uniek zijn voor een specifieke cluster. Naam van het script moet de volgende indeling hebben:
  > 
  > "naam": "[concat ('hue-install-v0 ','-', uniquestring('applicationName')]"
  > 
  > Naam van het script bevat drie onderdelen:
  > 
  > * Het voorvoegsel van een script, waaronder de toepassingsnaam van de of een naam die relevant zijn voor de toepassing moet.
  > * Een afbreekstreepje voor de leesbaarheid.
  > * Een unieke tekenreeksfunctie met de naam van de toepassing als de parameter.
  > 
  > In de lijst met persistente scriptacties, in het voorgaande voorbeeld wordt weergegeven als **hue-install-v0-4wkahss55hlas**. Zie een [voorbeeld-JSON-nettolading](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 

Het script voor installatie moet de volgende kenmerken:
* Het script is idempotent zijn. Meerdere aanroepen naar het script opleveren hetzelfde resultaat.
* Het script is correct is samengesteld. Gebruik een andere locatie voor het script wanneer u een upgrade of wijzigingen te testen. Dit zorgt ervoor dat klanten die de toepassing installeren niet getroffen door uw updates of testen zijn. 
* Het script heeft voldoende logboekregistratie op elk moment. Normaal gesproken zijn scriptlogboeken de enige manier om op te sporen installatieproblemen.
* Aanroepen naar externe services of resources hebben voldoende nieuwe pogingen, zodat de installatie wordt niet beïnvloed door tijdelijke netwerkproblemen.
* Als uw script start de services op de knooppunten, worden services bewaakt en geconfigureerd om automatisch te starten als er een opnieuw opstarten van het knooppunt optreedt.

## <a name="package-the-application"></a>De toepassing verpakken
Maak een ZIP-bestand met de bestanden die nodig zijn om uw HDInsight-toepassing te installeren. Het ZIP-bestand kunt u de toepassing te publiceren. Het ZIP-bestand bevat de volgende bestanden:

* createUiDefinition.json
* mainTemplate.json (Zie voor een voorbeeld [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).)
* Alle vereiste scripts

> [!NOTE]  
> U kunt de toepassingsbestanden (inclusief alle web-app-bestanden) hosten op elk willekeurig eindpunt dat openbaar toegankelijk is.

## <a name="publish-the-application"></a>De toepassing publiceren
Een HDInsight-toepassing publiceren:

1. Aanmelden bij [Azure-publicatie](https://publish.windowsazure.com/).
2. Selecteer in het menu links **oplossingssjablonen**.
3. Voer een titel in en selecteer vervolgens **maken van een nieuwe oplossingssjabloon**.
4. Als u uw organisatie nog niet hebt geregistreerd, selecteert u **ontwikkelaarscentrumaccount maken en deelnemen aan het programma Azure**.  Zie voor meer informatie, [maken van een Microsoft Developer-account](../marketplace/marketplace-publishers-guide.md).
5. Selecteer **topologieën aan de slag definiëren**. Een oplossingssjabloon is een "ouder" voor alle bijbehorende topologieën. U kunt meerdere topologieën definiëren in één aanbieding of oplossing sjabloon. Wanneer een aanbieding wordt doorgestuurd voor fasering, wordt deze doorgestuurd met alle bijbehorende topologieën. 
6. Voer een topologienaam in en selecteer vervolgens **+**.
7. Voer een nieuwe versie, en selecteer vervolgens **+**.
8. Upload het ZIP-bestand dat u hebt gemaakt toen u de toepassing.  
9. Selecteer **certificering aanvragen**. Het team van Microsoft-certificering controleert de bestanden en certificeert de topologie.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md) in uw clusters.
* Meer informatie over het [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md) en implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* Meer informatie over het [scriptactie gebruiken op Linux gebaseerde HDInsight-clusters aanpassen](hdinsight-hadoop-customize-cluster-linux.md) en meer toepassingen toevoegen. 
* Meer informatie over het [Apache Hadoop op basis van Linux-clusters in HDInsight maken met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
* Meer informatie over het [een leeg edge-knooppunt gebruiken in HDInsight](hdinsight-apps-use-edge-node.md) voor toegang tot HDInsight-clusters, HDInsight toepassingen testen en HDInsight-toepassingen hosten.


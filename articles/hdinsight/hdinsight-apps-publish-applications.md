---
title: Azure HDInsight-toepassingen publiceren | Microsoft Docs
description: Informatie over het maken van een HDInsight-toepassing en vervolgens publiceren in Azure Marketplace.
services: hdinsight
documentationcenter: 
author: mumian
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 14aef891-7a37-4cf1-8f7d-ca923565c783
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/03/2018
ms.author: jgao
ms.openlocfilehash: 597ea68f063d02541132d275de815c1673369ae0
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="publish-an-hdinsight-application-in-the-azure-marketplace"></a>Een HDInsight-toepassing publiceren in Azure Marketplace
U kunt een Azure HDInsight-toepassing installeren op een Linux gebaseerde HDInsight-cluster. Informatie over het publiceren van een HDInsight-toepassing in Azure Marketplace in dit artikel. Raadpleeg voor algemene informatie over publiceren in Azure Marketplace [een aanbieding publiceren in Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md).

HDInsight-toepassingen gebruiken het *Bring Your Own License (BYOL)* model. Een toepassingsprovider is in een scenario BYOL die verantwoordelijk is voor de toepassing voor gebruikers van de app-licentieverlening. App-gebruikers alleen voor de Azure-resources die ze, zoals het HDInsight-cluster en virtuele machines en de knooppunten van het cluster maken in rekening worden gebracht. Facturering voor de toepassing zelf niet op dit moment wordt uitgevoerd in Azure.

Zie voor meer informatie deze HDInsight-toepassing-gerelateerde artikelen:

* [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md). Informatie over het installeren van een HDInsight-toepassing op uw clusters.
* [Aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md). Informatie over het installeren en testen van aangepaste HDInsight-toepassingen.

## <a name="prerequisites"></a>Vereisten
Verzenden van uw aangepaste toepassing in de Marketplace eerst [maken en testen van uw aangepaste toepassing](hdinsight-apps-install-custom-applications.md).

Ook moet u uw ontwikkelaarsaccount registreren. Zie voor meer informatie [een aanbieding publiceren in Azure Marketplace](../marketplace-publishing/marketplace-publishing-getting-started.md) en [een Microsoft Developer-account maken](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).

## <a name="define-the-application"></a>De toepassing definiëren
Twee stappen betrokken zijn bij het publiceren van toepassingen in de Marketplace. Eerst definieert een *createUiDef.json* bestand. Het createUiDef.json-bestand geeft aan welke clusters de toepassing compatibel is. Vervolgens de sjabloon van de Azure-portal publiceren. Hier volgt een voorbeeld van een createUiDef.json-bestand:

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

| Veld | Beschrijving | Mogelijke waarden |
| --- | --- | --- |
| typen |De clustertypen waarmee de toepassing compatibel is. |Hadoop, HBase, Storm, Spark (of een combinatie hiervan) |
| versies |De HDInsight-clustertypen waarmee de toepassing compatibel is. |3.4 |

## <a name="application-installation-script"></a>Het installatiescript toepassing
Wanneer een toepassing wordt geïnstalleerd op een cluster (een op een bestaand cluster of op een nieuw), wordt een edge-knooppunt gemaakt. Het installatiescript voor de toepassing wordt uitgevoerd op de edge-knooppunt.

  > [!IMPORTANT]
  > De naam van het installatiescript van toepassing moet uniek zijn voor een specifiek cluster. Naam van het script moet de volgende indeling hebben:
  > 
  > "name": "[concat ('hue-install-v0 ','-', uniquestring('applicationName')]"
  > 
  > Naam van het script bestaat uit drie delen:
  > 
  > * Een voorvoegsel, die moet de naam van de toepassing of een naam die relevant zijn voor de toepassing bevatten.
  > * Een afbreekstreepje omwille van de leesbaarheid.
  > * Een unieke tekenreeksfunctie met de naam van de toepassing als de parameter.
  > 
  > In de lijst met persistente scriptacties, het voorgaande voorbeeld wordt weergegeven als **hue-install-v0-4wkahss55hlas**. Zie een [voorbeeld JSON-nettolading](https://raw.githubusercontent.com/hdinsight/Iaas-Applications/master/Hue/azuredeploy.json).
  > 

Het script voor installatie moet de volgende kenmerken:
* Het script is idempotent. Meerdere aanroepen naar het script levert hetzelfde resultaat.
* Het script is juist samengesteld. Gebruik een andere locatie voor het script wanneer u upgradet of wijzigingen te testen. Dit zorgt ervoor dat klanten die de toepassing installeren niet betrokken door uw software-updates of testen zijn. 
* Het script heeft voldoende logboekregistratie op elk punt. Normaal gesproken zijn script Logboeken de enige manier om problemen met de installatie van de toepassing voor foutopsporing.
* Aanroepen naar externe services of bronnen hebben voldoende nieuwe pogingen, zodat de installatie wordt niet beïnvloed door tijdelijke netwerkproblemen.
* Als uw script start de services op de knooppunten, worden services bewaakt en geconfigureerd om automatisch te starten als een herstart van het knooppunt optreedt.

## <a name="package-the-application"></a>De toepassing van het pakket
Maak een ZIP-bestand met de bestanden die nodig zijn voor uw HDInsight-toepassing installeren. Gebruik van het ZIP-bestand naar [Publiceer de toepassing](#publish-application). Het ZIP-bestand bevat de volgende bestanden:

* [createUiDefinition.json](#define-application)
* mainTemplate.json (Zie voor een voorbeeld [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md).)
* Alle vereiste scripts

> [!NOTE]
> U kunt de toepassingsbestanden (inclusief een web-app-bestanden) hosten op een openbaar toegankelijk eindpunt.
> 

## <a name="publish-the-application"></a>De toepassing publiceren
Een HDInsight-toepassing publiceren:

1. Aanmelden bij [Azure Publishing](https://publish.windowsazure.com/).
2. Selecteer in het menu links **oplossingssjablonen**.
3. Voer een titel en selecteer vervolgens **maken van een nieuwe oplossingssjabloon**.
4. Als u al uw organisatie nog niet hebt geregistreerd, selecteert u **ontwikkelaarscentrumaccount maken en deelnemen aan het Azure-programma**.  Zie voor meer informatie [een Microsoft Developer-account maken](../marketplace-publishing/marketplace-publishing-accounts-creation-registration.md).
5. Selecteer **aan de slag topologieën definiëren**. Een oplossingssjabloon is een 'ouder' alle bijbehorende topologieën. U kunt meerdere topologieën definiëren in één aanbieding of oplossing sjabloon. Wanneer een aanbieding wordt doorgegeven voor fasering, wordt deze doorgegeven met alle bijbehorende topologieën. 
6. Voer een naam voor de topologie en selecteer vervolgens  **+** .
7. Voer een nieuwe versie, en selecteer vervolgens  **+** .
8. Upload het ZIP-bestand dat u hebt gemaakt wanneer u [verpakt van de toepassing](#package-application).  
9. Selecteer **certificering aanvragen**. De Microsoft-certificeringsteam beoordeelt de bestanden en certificeert de topologie.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over hoe [HDInsight-toepassingen installeren](hdinsight-apps-install-applications.md) in clusters.
* Meer informatie over hoe [aangepaste HDInsight-toepassingen installeren](hdinsight-apps-install-custom-applications.md) en implementeren van een niet-gepubliceerde HDInsight-toepassing op HDInsight.
* Meer informatie over hoe [scriptactie gebruiken voor het aanpassen van Linux gebaseerde HDInsight-clusters](hdinsight-hadoop-customize-cluster-linux.md) en meer toepassingen toe te voegen. 
* Meer informatie over hoe [Linux gebaseerde Hadoop-clusters maken in HDInsight met behulp van Azure Resource Manager-sjablonen](hdinsight-hadoop-create-linux-clusters-arm-templates.md).
* Meer informatie over hoe [gebruik van een leeg edge-knooppunt in HDInsight](hdinsight-apps-use-edge-node.md) voor toegang tot HDInsight-clusters, HDInsight-toepassingen testen en HDInsight-toepassingen worden gehost.


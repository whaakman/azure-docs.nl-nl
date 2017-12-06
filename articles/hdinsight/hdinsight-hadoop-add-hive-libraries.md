---
title: Hive-bibliotheken toevoegen tijdens het maken van HDInsight-cluster - Azure | Microsoft Docs
description: Informatie over het toevoegen van Hive-bibliotheken (jar-bestanden), met een HDInsight-cluster tijdens het maken van het cluster.
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
ms.assetid: 2fd74b8d-c006-45c6-a9e2-72ff5d2d978a
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/05/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: bc12646840b0a28eee1ea11c40a57e59995bbf75
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a>Aangepaste Hive-bibliotheken toevoegen bij het maken van uw HDInsight-cluster

Informatie over het vooraf laden op HDInsight Hive-bibliotheken. Dit document bevat informatie over het gebruik van een scriptactie vooraf om bibliotheken te laden tijdens het maken van het cluster. Bibliotheken die zijn toegevoegd met de stappen in dit document algemeen beschikbaar zijn in de Hive - hoeft niet te gebruiken [JAR toevoegen](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) geladen.

## <a name="how-it-works"></a>Hoe werkt het?

Wanneer u een cluster maakt, kunt u een scriptactie clusterknooppunten aanpassen zoals ze zijn gemaakt. Het script in dit document accepteert één parameter, de locatie van de tapewisselaars is. Deze locatie moet zich in een Azure Storage-Account en de bibliotheken als jar-bestanden moeten worden opgeslagen.

Tijdens het maken van het cluster het script de bestanden inventariseert, kopieert deze naar de `/usr/lib/customhivelibs/` directory op de knooppunten hoofd- en werkrollen, vervolgens toevoegt aan de `hive.aux.jars.path` eigenschap in de `core-site.xml` bestand. Op Linux gebaseerde clusters werkt het ook de `hive-env.sh` bestand met de locatie van de bestanden.

> [!NOTE]
> Met de scriptacties in dit artikel worden de tapewisselaars beschikbaar zijn in de volgende scenario's:
>
> * **HDInsight op basis van Linux** : wanneer een client Hive **WebHCat**, en **HiveServer2**.
> * **HDInsight op basis van Windows** : wanneer met behulp van de client Hive en **WebHCat**.

## <a name="the-script"></a>Het script

**De locatie van script**

Voor **op basis van Linux-clusters**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Voor **Windows gebaseerde clusters**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

**Vereisten**

* De scripts moeten worden toegepast op beide de **hoofdknooppunten** en **Worker-knooppunten**.

* De potten die u wilt installeren, moeten worden opgeslagen in Azure Blob Storage in een **enkele container**.

* Het opslagaccount met de bibliotheek met jar-bestanden **moet** tijdens het maken worden gekoppeld aan het HDInsight-cluster. Dit moet het standaardopslagaccount of een account toegevoegd via __optionele configuratie__.

* Het pad WASB naar de container moet worden opgegeven als een parameter voor de actie Script. Bijvoorbeeld, als de potten worden opgeslagen in een container met de naam **bibliotheken** op een opslagaccount met de naam **mystorage**, de parameter zou worden  **wasb://libs@mystorage.blob.core.windows.net/** .

  > [!NOTE]
  > Dit document wordt ervan uitgegaan dat u hebt al een opslagaccount, kan de blob-container gemaakt en de bestanden naar deze geüpload.
  >
  > Als u geen opslagaccount hebt gemaakt, kunt u doen via zodat de [Azure-portal](https://portal.azure.com). U kunt vervolgens een hulpprogramma zoals gebruiken [Azure Opslagverkenner](http://storageexplorer.com/) een container maken in het account en bestanden uploaden naar het.

## <a name="create-a-cluster-using-the-script"></a>Een cluster met behulp van het script maken

> [!NOTE]
> De volgende stappen maakt een Linux gebaseerde HDInsight-cluster. Selecteer voor het maken van een cluster met Windows **Windows** als het cluster-OS bij het maken van het cluster en gebruik de Windows (PowerShell) in plaats van het script bash script.
>
> U kunt ook Azure PowerShell of de HDInsight .NET SDK gebruiken om een cluster met dit script te maken. Zie voor meer informatie over het gebruik van deze methoden [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

1. Start de inrichting van een cluster met behulp van de stappen in [HDInsight-clusters inrichten op Linux](hdinsight-hadoop-provision-linux-clusters.md), maar inrichting niet voltooid.

2. Op de **optionele configuratie** sectie **scriptacties**, en geef de volgende informatie:

   * **NAAM**: een beschrijvende naam voor de scriptactie.

   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh

   * **HEAD**: Schakel deze optie.

   * **WERKNEMER**: Schakel deze optie.

   * **ZOOKEEPER**: leeg laten.

   * **PARAMETERS**: Geef het adres WASB op het account-container en opslag die de potten bevat. Bijvoorbeeld:  **wasb://libs@mystorage.blob.core.windows.net/** .

3. Aan de onderkant van de **scriptacties**, gebruiken de **Selecteer** om op te slaan van de configuratie.

4. Op de **optionele configuratie** sectie **gekoppelde Storage-Accounts** en selecteer de **van een sleutel opslag** koppeling. Selecteer het opslagaccount waarin de potten. Gebruik vervolgens de **Selecteer** knoppen instellingen opslaan en retourneren de **optionele configuratie**.

5. Voor het opslaan van de optionele configuratie gebruiken de **Selecteer** knop aan de onderkant van de **optionele configuratie** sectie.

6. Doorgaan met het inrichten van het cluster, zoals beschreven in [HDInsight-clusters inrichten op Linux](hdinsight-hadoop-provision-linux-clusters.md).

Nadat het maken van clusters is voltooid, u zich kunt gebruiken de potten toegevoegd via dit script uit Hive te gebruiken zonder de `ADD JAR` instructie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het werken met Hive [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)

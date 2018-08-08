---
title: Hive-bibliotheken toevoegen tijdens het HDInsight-cluster maken - Azure
description: Informatie over het toevoegen van Hive-bibliotheken (jar-bestanden), met een HDInsight-cluster tijdens het maken van clusters.
services: hdinsight
author: jasonwhowell
editor: jasonwhowell
ms.service: hdinsight
ms.topic: conceptual
ms.date: 02/27/2018
ms.author: jasonh
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: fe7cb2c3990a5aa161665519490ede90be04e1ee
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39594584"
---
# <a name="add-custom-hive-libraries-when-creating-your-hdinsight-cluster"></a>Aangepaste Hive-bibliotheken toevoegen bij het maken van uw HDInsight-cluster

Informatie over het vooraf laden op HDInsight Hive-bibliotheken. Dit document bevat informatie over het gebruik van een scriptactie bibliotheken vooraf laden tijdens het maken van clusters. Bibliotheken die zijn toegevoegd met behulp van de stappen in dit document zijn wereldwijd beschikbaar in Hive - hoeft niet te gebruiken [toevoegen JAR](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Cli) te laden.

## <a name="how-it-works"></a>Hoe werkt het?

Bij het maken van een cluster, kunt u een scriptactie clusterknooppunten wijzigen zodra deze worden gemaakt. Het script in dit document accepteert één parameter, dit de locatie van de bibliotheken is. Deze locatie moet zich in een Azure Storage-Account en de bibliotheken moeten worden opgeslagen als een jar-bestanden.

Tijdens het maken van een cluster, het script de bestanden inventariseert, kopieert deze naar de `/usr/lib/customhivelibs/` map op de hoofd- en worker-knooppunten, klikt u vervolgens voegt deze toe aan de `hive.aux.jars.path` eigenschap in de `core-site.xml` bestand. Op basis van Linux-clusters, werkt het ook de `hive-env.sh` -bestand met de locatie van de bestanden.

> [!NOTE]
> Met behulp van de scriptacties in dit artikel worden de bibliotheken beschikbaar in de volgende scenario's:
>
> * **HDInsight op basis van Linux** : wanneer een client Hive **WebHCat**, en **HiveServer2**.
> * **HDInsight op basis van Windows** - bij het gebruik van de Hive-client en **WebHCat**.

## <a name="the-script"></a>Het script

**Locatie van het script**

Voor **Linux gebaseerde clusters**: [https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh)

Voor **Windows gebaseerde clusters**: [https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1](https://hdiconfigactions.blob.core.windows.net/setupcustomhivelibsv01/setup-customhivelibs-v01.ps1)

> [!IMPORTANT]
> Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

**Vereisten**

* De scripts moeten worden toegepast op zowel de **hoofdknooppunten** en **Worker-knooppunten**.

* De JAR-bestanden u wilt installeren, moeten worden opgeslagen in Azure Blob-opslag in een **enkele container**.

* Het opslagaccount met de bibliotheek met jar-bestanden **moet** tijdens het maken worden gekoppeld aan het HDInsight-cluster. Dit moet het standaardopslagaccount of een account toegevoegd via __optionele configuratie__.

* Het WASB-pad naar de container moet worden opgegeven als parameter voor de scriptactie. Bijvoorbeeld, als de JAR-bestanden worden opgeslagen in een container met de naam **bibliotheken** op een storage-account met de naam **mijnopslag**, zou de parameter **wasb://libs@mystorage.blob.core.windows.net/**.

  > [!NOTE]
  > Dit document wordt ervan uitgegaan dat u hebt al een storage-account, blob-container gemaakt en de bestanden geüpload naar deze.
  >
  > Als u niet een storage-account hebt gemaakt, kunt u doen dus via de [Azure-portal](https://portal.azure.com). U kunt vervolgens een hulpprogramma zoals gebruiken [Azure Storage Explorer](http://storageexplorer.com/) te maken van een container in het account en bestanden uploaden naar het.

## <a name="create-a-cluster-using-the-script"></a>Een cluster met behulp van het script maken

> [!NOTE]
> De volgende stappen maakt u een Linux gebaseerde HDInsight-cluster. Voor het maken van een cluster op basis van Windows selecteert **Windows** als het cluster-OS bij het maken van het cluster en gebruik de Windows (PowerShell) script in plaats van de bash-script.
>
> U kunt ook Azure PowerShell of de HDInsight .NET SDK gebruiken om een cluster met behulp van dit script te maken. Zie voor meer informatie over het gebruik van deze methoden [aanpassen HDInsight-clusters met scriptacties](hdinsight-hadoop-customize-cluster-linux.md).

1. Start het inrichten van een cluster met behulp van de stappen in [inrichten HDInsight-clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md), maar ze niet worden voltooid wordt ingericht.

2. Op de **optionele configuratie** sectie, selecteer **scriptacties**, en geef de volgende informatie:

   * **NAAM**: Geef een beschrijvende naam voor de scriptactie.

   * **SCRIPT-URI**: https://hdiconfigactions.blob.core.windows.net/linuxsetupcustomhivelibsv01/setup-customhivelibs-v01.sh

   * **HEAD**: Schakel deze optie.

   * **WERKNEMER**: Schakel deze optie.

   * **ZOOKEEPER**: laat dit veld leeg.

   * **PARAMETERS**: Geef de WASB-adres op van de container en de opslag-account met de JAR-bestanden. Bijvoorbeeld **wasb://libs@mystorage.blob.core.windows.net/**.

3. Aan de onderkant van de **scriptacties**, gebruikt u de **selecteren** om de configuratie op te slaan.

4. Op de **optionele configuratie** sectie, selecteer **gekoppelde Storage-Accounts** en selecteer de **een opslagsleutel toevoegen** koppeling. Selecteer het opslagaccount waarin de JAR-bestanden. Gebruik vervolgens de **Selecteer** knoppen instellingen opslaan en retourneren de **optionele configuratie**.

5. Als u wilt de optionele configuratie op te slaan, gebruikt u de **Selecteer** knop aan de onderkant van de **optionele configuratie** sectie.

6. Doorgaan met het inrichten van het cluster, zoals beschreven in [inrichten HDInsight-clusters op Linux](hdinsight-hadoop-provision-linux-clusters.md).

Als het maken van clusters is voltooid, bent u de JAR-bestanden van Hive via dit script toegevoegd zonder gebruik te gebruiken de `ADD JAR` instructie.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het werken met Hive [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)

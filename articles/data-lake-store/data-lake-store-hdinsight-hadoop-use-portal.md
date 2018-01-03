---
title: De Azure portal gebruiken om Azure HDInsight-clusters maken met Data Lake Store | Microsoft Docs
description: De Azure portal gebruiken voor het maken en gebruiken van HDInsight-clusters met Azure Data Lake Store
services: data-lake-store,hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/28/2017
ms.author: nitinme
ms.openlocfilehash: 4a27ae1402717c91029eda9d635db124f8bb6b8d
ms.sourcegitcommit: b5c6197f997aa6858f420302d375896360dd7ceb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2017
---
# <a name="create-hdinsight-clusters-with-data-lake-store-by-using-the-azure-portal"></a>HDInsight-clusters maken met Data Lake Store via de Azure-portal
> [!div class="op_single_selector"]
> * [Azure Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [Gebruik PowerShell (voor opslag van de standaard)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [Gebruik PowerShell (voor extra opslagruimte)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Gebruik Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Informatie over het gebruik van de Azure-portal voor het maken van een HDInsight-cluster met een Azure Data Lake Store-account als de standaard-opslag of een extra opslagruimte. Hoewel extra opslagruimte optioneel voor een HDInsight-cluster is, wordt het aanbevolen voor het opslaan van uw zakelijke gegevens in de extra opslagaccounts.

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, zorg ervoor dat u de volgende vereisten hebt voldaan:

* **Een Azure-abonnement**. Ga naar [gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Een Azure Data Lake Store-account**. Volg de instructies van [aan de slag met Azure Data Lake Store met behulp van de Azure-portal](data-lake-store-get-started-portal.md). U moet een hoofdmap ook maken met het account.  In deze zelfstudie een hoofdmap aangeroepen __/clusters__ wordt gebruikt.
* **Een Azure Active Directory-service-principal**. Deze zelfstudie vindt u instructies voor het maken van een service principal in Azure Active Directory (Azure AD). Voor het maken van een service-principal, moet u echter een Azure AD-beheerder zijn. Als u een beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    >[!NOTE]
    >U kunt een service principal maken alleen als u een Azure AD-beheerder. Uw Azure AD-beheerder moet maken een service principal voordat u een HDInsight-cluster met Data Lake Store maken kunt. Ook de service-principal moet worden gemaakt met een certificaat, zoals beschreven op [een service-principal maken met certificaat](../azure-resource-manager/resource-group-authenticate-service-principal.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>Een HDInsight-cluster maken

In deze sectie maakt u een HDInsight-cluster met Data Lake Store-accounts als de standaardwaarde of de extra opslagruimte. Dit artikel is alleen gericht het deel van de configuratie van Data Lake Store-accounts.  Zie voor het maken van algemene clustergegevens en -procedures, [maken Hadoop-clusters in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-store-as-default-storage"></a>Een cluster met Data Lake Store als standaard opslag maken

**Maken van een HDInsight-cluster met een Data Lake Store als het standaardopslagaccount**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga als volgt [clusters maken](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) voor algemene informatie over het maken van HDInsight-clusters.
3. Op de **opslag** blade onder **primaire opslagtype**, selecteer **Data Lake Store**, en voer de volgende informatie:

    ![Service-principal met HDInsight-cluster toevoegen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "service-principal met HDInsight-cluster toevoegen")

    - **Selecteer Data Lake Store-account**: Selecteer een bestaande Data Lake Store-account. Er is een bestaande Data Lake Store-account vereist.  Zie [vereisten](#prereuisites).
    - **Hoofdpad naar**: Geef een pad op de cluster-specifieke-bestanden die moeten worden opgeslagen. Op de schermafbeelding is __/clusters/myhdiadlcluster/__, waarin de __/clusters__ map moet bestaan en de Portal maakt *myhdicluster* map.  De *myhdicluster* is de naam van het cluster.
    - **Data Lake Store toegang**: toegang tussen de Data Lake Store-account en een HDInsight-cluster configureren. Zie voor instructies [Configure Data Lake Store toegang](#configure-data-lake-store-access).
    - **Extra opslagaccounts**: Azure Storage-Accounts toevoegen als extra opslagruimte accounts voor het cluster. Als u wilt toevoegen als u meer Lake gegevensarchieven gebeurt door de cluster-machtigingen van de gegevens in Data Lake Store-accounts meer tijdens het configureren van een Data Lake Store-account als het primaire type. Zie [Toegang tot Data Lake Store configureren](#configure-data-lake-store-access).

4. Op de **Data Lake Store toegang**, klikt u op **Selecteer**, en ga verder met maken van het cluster, zoals beschreven in [maken Hadoop-clusters in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-store-as-additional-storage"></a>Een cluster maken met Data Lake Store als extra opslagruimte

De volgende instructies maakt een HDInsight-cluster met een Azure Storage-account omdat de opslaggroep standaard en een Data Lake Store-account als extra opslag.
**Maken van een HDInsight-cluster met een Data Lake Store als het standaardopslagaccount**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga als volgt [clusters maken](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) voor algemene informatie over het maken van HDInsight-clusters.
3. Op de **opslag** blade onder **primaire opslagtype**, selecteer **Azure Storage**, en voer de volgende informatie:

    ![Service-principal met HDInsight-cluster toevoegen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "service-principal met HDInsight-cluster toevoegen")

    - **Selectiemethode**: gebruik een van de volgende opties:

        * Als u wilt opgeven van een opslagaccount die deel uitmaakt van uw Azure-abonnement **abonnementen**, en selecteer vervolgens het opslagaccount.
        * Als u wilt opgeven van een opslagaccount die zich buiten uw Azure-abonnement **toegangssleutel**, en geef vervolgens de gegevens voor het externe opslagaccount.

    - **Standaardcontainer**: Gebruik de standaardwaarde of de naam van uw eigen opgeven.

    - Extra Storage-accounts: meer Azure Storage-accounts toevoegen als de extra opslagruimte.
    - Data Lake Store toegang: toegang tussen de Data Lake Store-account en een HDInsight-cluster configureren. Zie voor instructies [Configure Data Lake Store toegang](#configure-data-lake-store-access).

## <a name="configure-data-lake-store-access"></a>Data Lake Store toegang configureren 

In deze sectie configureert u Data Lake Store-toegang van HDInsight-clusters met een Azure Active Directory-service-principal. 

### <a name="specify-a-service-principal"></a>Geef een service-principal

Vanuit de Azure-portal kunt u een bestaande service-principal gebruiken of maak een nieuwe.

**Een service-principal maken vanuit de Azure-portal**

1. Klik op **Data Lake Store toegang** uit de Store-blade.
2. Op de **Data Lake Store toegang** blade, klikt u op **nieuw**.
3. Klik op **Service-Principal**, en volg de instructies voor het maken van een service-principal.
4. Als u besluit deze in de toekomst opnieuw gebruiken, downloadt u het certificaat. Downloaden van het certificaat is handig als u de dezelfde service-principal gebruiken wilt bij het maken van aanvullende HDInsight-clusters.

    ![Service-principal met HDInsight-cluster toevoegen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "service-principal met HDInsight-cluster toevoegen")

4. Klik op **toegang** voor het configureren van de toegang tot de map.  Zie [bestandsmachtigingen configureren](#configure-file-permissions).


**Gebruik een bestaande service-principal in de Azure portal**

1. Klik op **Data Lake Store toegang**.
1. Op de **Data Lake Store toegang** blade, klikt u op **gebruik bestaande**.
2. Klik op **Service-Principal**, en selecteer vervolgens een service-principal. 
3. Upload het certificaat (.pfx-bestand) die is gekoppeld aan de geselecteerde service-principal en voer vervolgens het wachtwoord voor het certificaat.

    ![Service-principal met HDInsight-cluster toevoegen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "service-principal met HDInsight-cluster toevoegen")

4. Klik op **toegang** voor het configureren van de toegang tot de map.  Zie [bestandsmachtigingen configureren](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Bestandsmachtigingen configureren

De configureert zijn verschillend afhankelijk van of het account wordt gebruikt als de standaard-opslag of een extra storage-account:

- Gebruikt als standaard opslag

    - machtigingen op het hoogste niveau van het Data Lake Store-account
    - de machtiging op het hoofdniveau van de opslag van de HDInsight-cluster. Bijvoorbeeld, de __/clusters__ map die eerder in de zelfstudie wordt gebruikt.
- Als extra opslag gebruiken

    - Op de mappen waar moet u toegang tot bestand toestemming.

**Om toe te wijzen op het hoofdniveau van de Data Lake Store-account**

1. Op de **Data Lake Store toegang** blade, klikt u op **toegang**. De **Selecteer bestandsmachtigingen** blade wordt geopend. Hiermee geeft u de Data Lake Store-accounts in uw abonnement.
2. Beweeg de muisaanwijzer (niet op) de muis op de naam van het Data Lake Store-account, het selectievakje in om zichtbaar te maken, selecteer vervolgens het selectievakje in.

    ![Service-principal met HDInsight-cluster toevoegen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "service-principal met HDInsight-cluster toevoegen")

  Standaard __lezen__, __schrijven__, en __EXECUTE__ zijn geselecteerd.

3. Klik op **Selecteer** aan de onderkant van de pagina.
4. Klik op **uitvoeren** om toe te wijzen.
5. Klik op **Gereed**.

**Om toe te wijzen op het hoofdniveau van de HDInsight-cluster**

1. Op de **Data Lake Store toegang** blade, klikt u op **toegang**. De **Selecteer bestandsmachtigingen** blade wordt geopend. Hiermee geeft u de Data Lake Store-accounts in uw abonnement.
1. Van de **Selecteer bestandsmachtigingen** blade, klikt u op de naam van de Data Lake Store om de inhoud ervan weer te geven.
2. Selecteer de hoofdmap van HDInsight-cluster opslag door het selectievakje aan de linkerkant van de map te selecteren. Volgens de schermafbeelding eerder, de hoofdmap van de opslag cluster is __/clusters__ map die u hebt opgegeven bij het selecteren van de Data Lake Store als standaard opslag.
3. Stel de machtigingen voor de map.  Standaard lezen, schrijven en uitvoeren zijn geselecteerd.
4. Klik op **Selecteer** aan de onderkant van de pagina.
5. Klik op **Run**.
6. Klik op **Gereed**.

Als u Data Lake Store als extra opslagruimte gebruikt, moet u de machtiging alleen voor de mappen die u wilt openen vanuit het HDInsight-cluster toewijzen. Bijvoorbeeld, in de onderstaande schermafbeelding u toegang bieden tot **hdiaddonstorage** map in een Data Lake Store-account.

![Service-principal machtigingen toewijzen aan het HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "service principal machtigingen toewijzen aan het HDInsight-cluster")


## <a name="verify-cluster-set-up"></a>Controleer of cluster instellen

Nadat de installatie voltooid is, Controleer op de cluster-blade of uw resultaten als volgt een of beide van de volgende stappen uit:

* Om te bevestigen dat de bijbehorende opslagruimte voor het cluster het Data Lake Store-account dat u hebt opgegeven, klikt u op **opslagaccounts** in het linkerdeelvenster.

    ![Service-principal met HDInsight-cluster toevoegen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "service-principal met HDInsight-cluster toevoegen")

* Om te controleren of de service-principal correct gekoppeld aan het HDInsight-cluster is, klikt u op **Data Lake Store toegang** in het linkerdeelvenster.

    ![Service-principal met HDInsight-cluster toevoegen](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "service-principal met HDInsight-cluster toevoegen")


## <a name="examples"></a>Voorbeelden

Nadat u het cluster met Data Lake Store als uw opslag hebt ingesteld, kunt u verwijzen naar deze voorbeelden van het HDInsight-cluster gebruiken om de gegevens die zijn opgeslagen in Data Lake Store te analyseren.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-primary-storage"></a>Een Hive-query uitvoeren op gegevens in een Data Lake Store (als primaire opslag)

Als u wilt een Hive-query uitvoert, moet u de interface van de weergaven Hive in de portal Ambari gebruiken. Zie voor instructies over het gebruik van de Ambari Hive-weergaven [de weergave Hive gebruiken met Hadoop in HDInsight](../hdinsight/hadoop/apache-hadoop-use-hive-ambari-view.md).

Wanneer u met gegevens in een Data Lake Store werkt, zijn er enkele tekenreeksen te wijzigen.

Als u gebruikt, bijvoorbeeld het cluster dat u hebt gemaakt met Data Lake Store als primaire opslag, het pad naar de gegevens is: *adl: / / < data_lake_store_account_name > /azuredatalakestore.net/path/to/file*. Een Hive-query voor het maken van een tabel met voorbeeldgegevens die zijn opgeslagen in de Data Lake Store-account, ziet de volgende instructie:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsstorage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Beschrijvingen van:
* `adl://hdiadlstorage.azuredatalakestore.net/`is de hoofdmap van het Data Lake Store-account.
* `/clusters/myhdiadlcluster`is de hoofdmap van de clustergegevens die u hebt opgegeven tijdens het maken van het cluster.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/`de locatie van de voorbeeld-bestand dat u in de query gebruikt is.

### <a name="run-a-hive-query-against-data-in-a-data-lake-store-as-additional-storage"></a>Een Hive-query uitvoeren op gegevens in een Data Lake Store (als extra opslag)

Als het cluster dat u hebt gemaakt wordt Blob-opslag als standaard opslag gebruikt, de voorbeeldgegevens niet is opgenomen in het Azure Data Lake Store-account dat wordt gebruikt als extra opslagruimte. In dat geval eerst gegevensoverdracht van Blob-opslag naar de Data Lake Store en voer vervolgens in de query's, zoals wordt weergegeven in het voorgaande voorbeeld.

Zie de volgende artikelen voor meer informatie over de gegevens uit Blob storage kopiëren naar een Data Lake Store:

* [Distcp gebruiken om gegevens tussen Azure Storage-blobs en Data Lake Store kopiëren](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy gebruiken om gegevens te kopiëren van Azure Storage-blobs naar Data Lake Store](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-store-with-a-spark-cluster"></a>Gebruik Data Lake Store met een Spark-cluster
U kunt een Spark-cluster Spark taken uitvoeren op gegevens die zijn opgeslagen in een Data Lake Store. Zie voor meer informatie [gebruik HDInsight Spark-cluster voor het analyseren van gegevens in Data Lake Store](../hdinsight/spark/apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-store-in-a-storm-topology"></a>Gebruik Data Lake Store in een Storm-topologie
Schrijven van gegevens van een Storm-topologie kunt u de Data Lake Store. Zie voor instructies over het bereiken van dit scenario [gebruik Azure Data Lake Store met Apache Storm met HDInsight](../hdinsight/storm/apache-storm-write-data-lake-store.md).

## <a name="see-also"></a>Zie ook
* [Gebruik Data Lake Store met Azure HDInsight-clusters](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [PowerShell: Een HDInsight-cluster voor het gebruik van Data Lake Store maken](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

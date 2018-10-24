---
title: De Azure portal gebruiken voor het maken van Azure HDInsight-clusters met Azure Data Lake Storage Gen1 | Microsoft Docs
description: De Azure portal gebruiken voor het maken en gebruiken van HDInsight-clusters met Azure Data Lake Storage Gen1
services: data-lake-store,hdinsight
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: a8c45a83-a8e3-4227-8b02-1bc1e1de6767
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: b81916de0940aa56eda97df94527fa6adc208285
ms.sourcegitcommit: 5c00e98c0d825f7005cb0f07d62052aff0bc0ca8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49955827"
---
# <a name="create-hdinsight-clusters-with-azure-data-lake-storage-gen1-by-using-the-azure-portal"></a>HDInsight-clusters maken met Azure Data Lake Storage Gen1 met behulp van Azure portal
> [!div class="op_single_selector"]
> * [Azure Portal gebruiken](data-lake-store-hdinsight-hadoop-use-portal.md)
> * [PowerShell gebruiken (voor standaardopslag)](data-lake-store-hdinsight-hadoop-use-powershell-for-default-storage.md)
> * [PowerShell gebruiken (voor extra opslag)](data-lake-store-hdinsight-hadoop-use-powershell.md)
> * [Gebruik Resource Manager](data-lake-store-hdinsight-hadoop-use-resource-manager-template.md)
>
>

Informatie over het gebruik van de Azure-portal een HDInsight-cluster met een Azure Data Lake Storage Gen1-account maken als de standaardopslag of een extra opslag. Zelfs als extra opslag optioneel voor een HDInsight-cluster is, is het aanbevolen voor het opslaan van uw zakelijke gegevens in de extra opslagaccounts.

## <a name="prerequisites"></a>Vereisten
Voordat u deze zelfstudie begint, zorg ervoor dat u hebt voldaan aan de volgende vereisten:

* **Een Azure-abonnement**. Ga naar [gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **De account van een Data Lake Storage Gen1**. Volg de instructies van [aan de slag met Azure Data Lake Storage Gen1 met behulp van de Azure-portal](data-lake-store-get-started-portal.md). U moet ook een hoofdmap maken voor het account.  In deze zelfstudie een basismap met de naam __/clusters__ wordt gebruikt.
* **Een Azure Active Directory-service-principal**. In deze zelfstudie vindt u instructies voor het maken van een service principal in Azure Active Directory (Azure AD). Voor het maken van een service-principal, moet u echter een Azure AD-beheerder zijn. Als u een beheerder bent, kunt u deze vereiste overslaan en doorgaan met de zelfstudie.

    >[!NOTE]
    >U kunt een service-principal maken alleen als u een Azure AD-beheerder. Uw Azure AD-beheerder moet een service-principal maken voordat u een HDInsight-cluster met Data Lake Storage Gen1 kunt maken. Ook de service-principal moet worden gemaakt met een certificaat, zoals beschreven op [een service-principal maken met certificaat](../active-directory/develop/howto-authenticate-service-principal-powershell.md#create-service-principal-with-self-signed-certificate).
    >

## <a name="create-an-hdinsight-cluster"></a>Een HDInsight-cluster maken

In deze sectie maakt u een HDInsight-cluster met Data Lake Storage Gen1 accounts als de standaardwaarde of de extra opslagruimte. In dit artikel is alleen gericht het deel van het configureren van Gen1 van Data Lake Storage-accounts.  Zie voor het maken van algemene clustergegevens en -procedures, [Hadoop-clusters maken in HDInsight](../hdinsight/hdinsight-hadoop-provision-linux-clusters.md).

### <a name="create-a-cluster-with-data-lake-storage-gen1-as-default-storage"></a>Een cluster maken met Data Lake Storage Gen1 als standaardopslag

**Een HDInsight-cluster met een Gen1 van Data Lake Storage-account maken als het standaardaccount voor opslag**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga als volgt [clusters maken](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) voor de algemene informatie over het maken van HDInsight-clusters.
3. Op de **opslag** blade onder **primaire opslagtype**, selecteer **Azure Data Lake Storage Gen1**, en voer de volgende informatie:

    ![Service-principal toevoegen aan HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.adls.storage.png "service-principal toevoegen aan HDInsight-cluster")

    - **Selecteer Data Lake Store-account**: Selecteer een bestaand Data Lake Storage Gen1-account. Een bestaand Data Lake Storage Gen1-account is vereist.  Zie [Vereisten](#prerequisites).
    - **Het pad naar hoofdmap**: Geef een pad op de cluster-specifieke-bestanden die moeten worden opgeslagen. Op de schermafbeelding is het __/clusters/myhdiadlcluster/__, waarin de __/clusters__ map moet aanwezig zijn, en de Portal maakt *myhdicluster* map.  De *myhdicluster* is de naam van het cluster.
    - **Data Lake Store-toegang**: de toegang tussen de Gen1 van Data Lake Storage-account en het HDInsight-cluster configureren. Zie voor instructies [configureren Data Lake Storage Gen1 toegang](#configure-data-lake-store-access).
    - **Extra opslagaccounts**: Voeg Azure storage-accounts als extra opslagaccounts voor het cluster. Als u wilt toevoegen van extra Gen1 van Data Lake Storage-accounts wordt gedaan door te geven van de cluster-machtigingen op gegevens in meer Gen1 van Data Lake Storage-accounts tijdens het configureren van een Data Lake Storage Gen1-account als de primaire opslag. Zie [configureren Data Lake Storage Gen1 toegang](#configure-data-lake-store-access).

4. Op de **Data Lake Store-toegang**, klikt u op **Selecteer**, en ga verder met het maken van clusters, zoals beschreven in [Hadoop-clusters maken in HDInsight](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md).


### <a name="create-a-cluster-with-data-lake-storage-gen1-as-additional-storage"></a>Een cluster maken met Data Lake Storage Gen1 als extra opslag

De volgende instructies wordt een HDInsight-cluster maken met Azure storage-account als de standaardopslag en een Gen1 van Data Lake Storage-account als extra opslag.

**Een HDInsight-cluster met een Gen1 van Data Lake Storage-account maken als een extra opslagaccount**

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Ga als volgt [clusters maken](../hdinsight/hdinsight-hadoop-create-linux-clusters-portal.md#create-clusters) voor de algemene informatie over het maken van HDInsight-clusters.
3. Op de **opslag** blade onder **primaire opslagtype**, selecteer **Azure Storage**, en voer de volgende informatie:

    ![Service-principal toevoegen aan HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.1.png "service-principal toevoegen aan HDInsight-cluster")

    - **Selectiemethode**: gebruik een van de volgende opties:

        * Als u een opslagaccount die deel uitmaakt van uw Azure-abonnement, schakelt u **Mijn abonnementen**, en selecteer vervolgens het opslagaccount.
        * Als u een opslagaccount die zich buiten uw Azure-abonnement, schakelt u **toegangssleutel**, en geef vervolgens de informatie voor het externe opslagaccount.

    - **Standaardcontainer**: Gebruik de standaardwaarde of uw eigen naam opgeven.

    - Extra opslagaccounts: meer Azure storage-accounts toevoegen als de extra opslagruimte.
    - Data Lake Store-toegang: de toegang tussen de Gen1 van Data Lake Storage-account en het HDInsight-cluster configureren. Zie voor instructies [configureren Data Lake Storage Gen1 toegang](#configure-data-lake-storage-gen1-access).

## <a name="configure-data-lake-storage-gen1-access"></a>Toegang tot Data Lake Storage Gen1 configureren 

In deze sectie configureert u toegang tot Data Lake Storage Gen1 van HDInsight-clusters met behulp van een service-principal voor Azure Active Directory. 

### <a name="specify-a-service-principal"></a>Een service-principal opgeven

Vanuit Azure portal, kunt u een bestaande service-principal gebruiken of een nieuwe maken.

**Een service-principal maken vanuit Azure portal**

1. Klik op **Data Lake Store-toegang** op de blade opslag.
2. Op de **toegang tot Data Lake Storage Gen1** blade, klikt u op **nieuw**.
3. Klik op **Service-principal**, en volg de instructies voor het maken van een service-principal.
4. Download het certificaat als u besluit om deze te gebruiken in de toekomst opnieuw. Downloaden van het certificaat is handig als u de dezelfde service-principal gebruiken wilt bij het maken van aanvullende HDInsight-clusters.

    ![Service-principal toevoegen aan HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.2.png "service-principal toevoegen aan HDInsight-cluster")

4. Klik op **toegang** om de maptoegang te configureren.  Zie [bestandsmachtigingen configureren](#configure-file-permissions).


**Gebruik een bestaande service-principal van Azure portal**

1. Klik op **Data Lake Store-toegang**.
1. Op de **toegang tot Data Lake Storage Gen1** blade, klikt u op **gebruik bestaande**.
2. Klik op **Service-principal**, en selecteer vervolgens een service-principal. 
3. Upload het certificaat (.pfx-bestand) dat is gekoppeld aan uw geselecteerde service-principal en voer vervolgens het wachtwoord voor het certificaat.

    ![Service-principal toevoegen aan HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.5.png "service-principal toevoegen aan HDInsight-cluster")

4. Klik op **toegang** om de maptoegang te configureren.  Zie [bestandsmachtigingen configureren](#configure-file-permissions).


### <a name="configure-file-permissions"></a>Machtigingen voor bestanden configureren

De configureert verschillen, afhankelijk van of het account wordt gebruikt als de standaardopslag of een extra opslagaccount:

- Als standaardopslag gebruikt

    - machtiging op het hoogste niveau van het Data Lake Storage Gen1-account
    - machtiging op het hoogste niveau van de opslag van de HDInsight-cluster. Bijvoorbeeld, de __/clusters__ map die eerder in de zelfstudie wordt gebruikt.
- Als een extra opslag gebruiken

    - Machtiging op de mappen waar moet u toegang tot bestand.

**Om toe te wijzen op het hoofdniveau van Gen1 van Data Lake Storage-account**

1. Op de **toegang tot Data Lake Storage Gen1** blade, klikt u op **toegang**. De **bestandsmachtigingen selecteren** blade wordt geopend. Geeft alle Gen1 van Data Lake Storage-accounts in uw abonnement.
2. Beweeg de muisaanwijzer (niet op) de muis op de naam van het Data Lake Storage Gen1 dat het selectievakje in om zichtbaar te maken, selecteer vervolgens het selectievakje in.

    ![Service-principal toevoegen aan HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3.png "service-principal toevoegen aan HDInsight-cluster")

  Standaard __lezen__, __schrijven__, en __EXECUTE__ zijn geselecteerd.

3. Klik op **Selecteer** aan de onderkant van de pagina.
4. Klik op **uitvoeren** waarmee machtigingen worden toegewezen.
5. Klik op **Gereed**.

**Om toe te wijzen op het hoofdniveau van de HDInsight-cluster**

1. Op de **toegang tot Data Lake Storage Gen1** blade, klikt u op **toegang**. De **bestandsmachtigingen selecteren** blade wordt geopend. Geeft alle Gen1 van Data Lake Storage-accounts in uw abonnement.
1. Uit de **bestandsmachtigingen selecteren** blade, klikt u op de naam van het Data Lake Storage Gen1 om de inhoud ervan weer te geven.
2. Selecteer de storage-hoofdmap van het HDInsight-cluster door het selectievakje aan de linkerkant van de map te selecteren. Op basis van de schermafbeelding eerder de hoofdmap van het cluster opslag is __/clusters__ map die u hebt opgegeven bij het selecteren van Data Lake Storage Gen1 als standaardopslag.
3. Stel de machtigingen op de map.  Standaard, lezen, schrijven en uitvoeren zijn geselecteerd.
4. Klik op **Selecteer** aan de onderkant van de pagina.
5. Klik op **Run**.
6. Klik op **Gereed**.

Als u Data Lake Storage Gen1 als extra opslag gebruikt, moet u de machtiging alleen voor de mappen die u wilt openen via het HDInsight-cluster toewijzen. Bijvoorbeeld, in de onderstaande schermafbeelding u toegang bieden tot de **mynewfolder** map in een Data Lake Storage Gen1-account.

![Service-principal machtigingen toewijzen aan het HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.3-1.png "service-principal machtigingen toewijzen aan het HDInsight-cluster")


## <a name="verify-cluster-set-up"></a>Controleer of cluster instellen

Nadat de installatie voltooid is, de resultaten op de cluster-blade controleren door het uitvoeren van een of beide van de volgende stappen uit:

* Om te bevestigen dat de gekoppelde opslag voor het cluster het Gen1 van Data Lake Storage-account dat u hebt opgegeven, klikt u op **opslagaccounts** in het linkerdeelvenster.

    ![Service-principal toevoegen aan HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6-1.png "service-principal toevoegen aan HDInsight-cluster")

* Om te bevestigen dat de service-principal correct gekoppeld aan het HDInsight-cluster, klikt u op **toegang tot Data Lake Storage Gen1** in het linkerdeelvenster.

    ![Service-principal toevoegen aan HDInsight-cluster](./media/data-lake-store-hdinsight-hadoop-use-portal/hdi.adl.6.png "service-principal toevoegen aan HDInsight-cluster")


## <a name="examples"></a>Voorbeelden

Nadat u het cluster met Data Lake Storage Gen1 als uw opslag hebt ingesteld, kunt u verwijzen naar deze voorbeelden van hoe u HDInsight-cluster gebruiken om de gegevens die zijn opgeslagen in Data Lake Storage Gen1 te analyseren.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-primary-storage"></a>Een Hive-query uitvoeren op gegevens in een Data Lake Storage Gen1-account (als primaire opslag)

Als u wilt een Hive-query uitvoeren, gebruiken de interface van de Hive-weergaven in de Ambari-portal. Zie voor instructies over het gebruik van de Ambari Hive-weergaven [de weergave Hive gebruiken met Hadoop in HDInsight](../hdinsight/hadoop/apache-hadoop-use-hive-ambari-view.md).

Wanneer u met gegevens in een Data Lake Storage Gen1-account werkt, zijn er enkele tekenreeksen te wijzigen.

Als u gebruikt, bijvoorbeeld het cluster dat u hebt gemaakt met Data Lake Storage Gen1 als primaire opslag, het pad naar de gegevens is: *adl: / / < data_lake_storage_gen1_account_name > /azuredatalakestore.net/path/to/file*. Een Hive-query te maken van een tabel met voorbeeldgegevens die zijn opgeslagen in het Data Lake Storage Gen1 account lijkt op de volgende instructie:

    CREATE EXTERNAL TABLE websitelog (str string) LOCATION 'adl://hdiadlsg1storage.azuredatalakestore.net/clusters/myhdiadlcluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/'

Beschrijving:
* `adl://hdiadlsg1storage.azuredatalakestore.net/` is de hoofdmap van het Data Lake Storage Gen1-account.
* `/clusters/myhdiadlcluster` is de hoofdmap van de clustergegevens die u hebt opgegeven tijdens het maken van het cluster.
* `/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/` is de locatie van het voorbeeldbestand dat u in de query gebruikt.

### <a name="run-a-hive-query-against-data-in-a-data-lake-storage-gen1-account-as-additional-storage"></a>Een Hive-query uitvoeren op gegevens in een Data Lake Storage Gen1-account (als extra opslag)

Als het cluster die u hebt gemaakt Blob-opslag als standaardopslag gebruikt, wordt de voorbeeldgegevens niet opgenomen in de Gen1 van Data Lake Storage-account dat wordt gebruikt als extra opslag. In dat geval moet eerst de gegevens overbrengen van Blob-opslag naar het Data Lake Storage Gen1-account en voer de query's zoals weergegeven in het voorgaande voorbeeld.

Zie de volgende artikelen voor meer informatie over het kopiëren van gegevens van Blob storage naar een Gen1 van Data Lake Storage-account:

* [Distcp gebruiken om gegevens tussen Azure Storage-blobs en Data Lake Storage Gen1 te kopiëren](data-lake-store-copy-data-wasb-distcp.md)
* [AdlCopy gebruiken om gegevens te kopiëren van Azure Storage-blobs naar Data Lake Storage Gen1](data-lake-store-copy-data-azure-storage-blob.md)

### <a name="use-data-lake-storage-gen1-with-a-spark-cluster"></a>Data Lake Storage Gen1 met een Spark-cluster gebruiken
Een Spark-cluster kunt u Spark-taken uitvoeren op gegevens die zijn opgeslagen in een Data Lake Storage Gen1-account. Zie voor meer informatie, [gebruik HDInsight Spark-cluster voor het analyseren van gegevens in Data Lake Storage Gen1](../hdinsight/spark/apache-spark-use-with-data-lake-store.md).


### <a name="use-data-lake-storage-gen1-in-a-storm-topology"></a>Data Lake Storage Gen1 gebruiken in een Storm-topologie
U kunt de Gen1 van Data Lake Storage-account gebruiken om gegevens te schrijven van een Storm-topologie. Zie voor instructies over het bereiken van dit scenario [gebruik Azure Data Lake Storage Gen1 met Apache Storm met HDInsight](../hdinsight/storm/apache-storm-write-data-lake-store.md).

## <a name="see-also"></a>Zie ook
* [Data Lake Storage Gen1 met Azure HDInsight-clusters gebruiken](../hdinsight/hdinsight-hadoop-use-data-lake-store.md)
* [PowerShell: Een HDInsight-cluster voor het gebruik van Data Lake Storage Gen1 maken](data-lake-store-hdinsight-hadoop-use-powershell.md)

[makecert]: https://msdn.microsoft.com/library/windows/desktop/ff548309(v=vs.85).aspx
[pvk2pfx]: https://msdn.microsoft.com/library/windows/desktop/ff550672(v=vs.85).aspx

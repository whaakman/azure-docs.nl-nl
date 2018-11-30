---
title: Aanvullende Azure storage-accounts toevoegen aan HDInsight
description: Informatie over het toevoegen van aanvullende Azure storage-accounts aan een bestaand HDInsight-cluster.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 194f84d2ce5360aa9ad9f3d771bf24f153ac6f5c
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52496299"
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>Extra opslagaccounts toevoegen aan HDInsight

Informatie over het gebruik van scriptacties aanvullende Azure storage-accounts toevoegen aan HDInsight. De stappen in dit document wordt een storage-account toevoegen aan een bestaand HDInsight op basis van Linux-cluster.

> [!IMPORTANT]
> De informatie in dit document is over het toevoegen van extra opslag aan een cluster nadat deze is gemaakt. Zie voor meer informatie over het toevoegen van storage-accounts tijdens het maken van een cluster [clusters instellen in HDInsight met Apache Hadoop, Apache Spark en Apache Kafka](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="how-it-works"></a>Hoe werkt het?

Met dit script worden de volgende parameters:

* __Azure storage-accountnaam__: de naam van het opslagaccount om toe te voegen aan het HDInsight-cluster. Nadat het script is uitgevoerd, kan HDInsight lezen en schrijven van gegevens die zijn opgeslagen in dit storage-account.

* __Azure storage-accountsleutel__: een sleutel die toegang tot het opslagaccount verleent.

* __-p__ (optioneel): als u opgeeft, wordt de sleutel is niet versleuteld en is opgeslagen in het bestand core-site.xml als tekst zonder opmaak.

Tijdens de verwerking voert het script de volgende acties:

* Als het opslagaccount al in de core-site.xml-configuratie voor het cluster bestaat, het script wordt afgesloten en er is geen verdere acties worden uitgevoerd.

* Hiermee wordt gecontroleerd of het opslagaccount bestaat en toegankelijk via de sleutel.

* De sleutel met behulp van de cluster-referentie gecodeerd.

* Het opslagaccount dat aan het bestand core-site.xml toegevoegd.

* Stopt en start u de [Apache Oozie](https://oozie.apache.org/), [Apache Hadoop YARN](https://hadoop.apache.org/docs/current/hadoop-yarn/hadoop-yarn-site/YARN.html), [Apache Hadoop MapReduce2](https://hadoop.apache.org/docs/current/hadoop-mapreduce-client/hadoop-mapreduce-client-core/MapReduceTutorial.html), en [Apache Hadoop HDFS](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-hdfs/HdfsUserGuide.html) services. Deze services starten en stoppen, kunnen ze het nieuwe opslagaccount gebruiken.

> [!WARNING]
> Met behulp van een storage-account in een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

## <a name="the-script"></a>Het script

__Scriptlocatie__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Vereisten voor__:

* Het script moet worden toegepast op de __hoofdknooppunten__.

## <a name="to-use-the-script"></a>Het gebruik van het script

Met dit script kan worden gebruikt vanuit de Azure-portal, Azure PowerShell of de klassieke Azure-CLI. Zie voor meer informatie de [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.

> [!IMPORTANT]
> Wanneer u de stappen in het document aanpassen, gebruikt u de volgende informatie om toe te passen met dit script:
>
> * Een voorbeeld-scriptactie URI vervangen door de URI voor dit script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).
> * Van de voorbeeldparameters vervangen door het Azure storage-accountnaam en de sleutel van het opslagaccount dat moet worden toegevoegd aan het cluster. Als u de Azure portal gebruikt, moeten deze parameters worden gescheiden door een spatie.
> * U hoeft niet te markeren met dit script als __persistente__, zoals de Ambari-configuratie van het cluster rechtstreeks worden bijgewerkt.

## <a name="known-issues"></a>Bekende problemen

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Storage-accounts niet weergegeven in Azure portal of hulpprogramma 's

Wanneer u het HDInsight-cluster in Azure portal bekijkt, selecteert de __Opslagaccounts__ item onder __eigenschappen__ storage-accounts die zijn toegevoegd via deze scriptactie niet wordt weergegeven. Azure PowerShell en klassieke Azure-CLI weergeven niet het extra opslagaccount de.

De opslaginformatie wordt niet weergegeven omdat het script alleen het core-site.xml-configuratie van het cluster wijzigt. Deze informatie wordt niet gebruikt bij het ophalen van de clustergegevens met behulp van Azure management API's.

Als u wilt weergeven van gegevens over het opslagaccount toegevoegd aan het cluster met behulp van dit script, gebruikt u de Ambari REST-API. Gebruik de volgende opdrachten om op te halen van deze informatie voor uw cluster:

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> Stel `$clusterName` op de naam van het HDInsight-cluster. Stel `$storageAccountName` op de naam van het storage-account. Voer desgevraagd de cluster-aanmeldingswachtwoord (beheerder) en het wachtwoord.

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> Stel `$PASSWORD` naar de accountwachtwoord voor clusteraanmelding (beheerder). Stel `$CLUSTERNAME` op de naam van het HDInsight-cluster. Stel `$STORAGEACCOUNTNAME` op de naam van het storage-account.
>
> In dit voorbeeld wordt [curl (http://curl.haxx.se/) ](http://curl.haxx.se/) en [jq (https://stedolan.github.io/jq/) ](https://stedolan.github.io/jq/) ophalen en parseren van JSON-gegevens.

Wanneer u deze opdracht gebruikt, Vervang __CLUSTERNAME__ met de naam van het HDInsight-cluster. Vervang __wachtwoord__ met de HTTP-aanmeldingswachtwoord voor het cluster. Vervang __STORAGEACCOUNT__ met de naam van de storage-account toegevoegd met behulp van scriptacties. Gegevens die zijn geretourneerd door deze opdracht wordt weergegeven die vergelijkbaar is met de volgende tekst:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Deze tekst is een voorbeeld van een versleutelde sleutel, die wordt gebruikt voor toegang tot het opslagaccount.

### <a name="unable-to-access-storage-after-changing-key"></a>Geen toegang krijgt tot opslag na het wijzigen van sleutel

Als u de sleutel voor een storage-account wijzigt, kan HDInsight niet langer toegang tot het opslagaccount. HDInsight maakt gebruik van een in cache opgeslagen kopie van de sleutel in de core-site.xml voor het cluster. De kopie van deze cache moet worden bijgewerkt zodat deze overeenkomt met de nieuwe sleutel.

Uitvoeren van de scriptactie opnieuw heeft __niet__ bijwerken van de sleutel, zoals het script wordt gecontroleerd of er al een vermelding voor het opslagaccount bestaat. Als er al een vermelding bestaat, wordt er geen wijzigingen aanbrengen.

U kunt dit probleem omzeilen, moet u de bestaande vermelding voor de storage-account verwijderen. Gebruik de volgende stappen uit om te verwijderen van de bestaande vermelding:

1. Open de Ambari-Webinterface voor uw HDInsight-cluster in een webbrowser. De URI is https://CLUSTERNAME.azurehdinsight.net. Vervang __CLUSTERNAME__ door de naam van uw cluster.

    Wanneer u hierom wordt gevraagd, typt u de HTTP-aangemelde gebruiker en het wachtwoord voor uw cluster.

2. Selecteer in de lijst met services aan de linkerkant van de pagina __HDFS__. Selecteer vervolgens de __Peeringconfiguraties__ tabblad in het midden van de pagina.

3. In de __filteren...__  veld, voert u een waarde van __fs.azure.account__. Hiermee wordt de vermeldingen voor alle extra opslagaccounts die zijn toegevoegd aan het cluster. Er zijn twee soorten vermeldingen; __keyprovider__ en __sleutel__. Beide bevatten de naam van het opslagaccount dat als onderdeel van de naam van de sleutel.

    Hieronder vindt u voorbeelden van vermeldingen voor een opslagaccount met de naam __mijnopslag__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Nadat u hebt vastgesteld dat de sleutels voor het opslagaccount dat u wilt verwijderen, gebruikt u de rode '-' pictogram aan de rechterkant van het item te verwijderen. Gebruik vervolgens de __opslaan__ knop uw wijzigingen op te slaan.

5. Nadat wijzigingen zijn opgeslagen, moet u de scriptactie gebruiken om toe te voegen van de storage-account en de nieuwe waarde van de sleutel aan het cluster.

### <a name="poor-performance"></a>Slechte prestaties

Als het opslagaccount zich in een andere regio dan het HDInsight-cluster, kan slechte prestaties optreden. Toegang tot gegevens in een andere regio stuurt netwerkverkeer buiten de regionale Azure-datacenter en via het openbare internet, dit leiden latentie tot kan.

> [!WARNING]
> Met behulp van een storage-account in een andere regio dan het HDInsight-cluster wordt niet ondersteund.

### <a name="additional-charges"></a>Extra kosten

Als het opslagaccount zich in een andere regio dan het HDInsight-cluster, merkt u wellicht aanvullende uitgaand verkeer kosten in rekening gebracht op uw Azure-facturering. Een kosten voor uitgaand verkeer toegepast wanneer gegevens een regionaal datacenter verlaat. Deze kosten wordt toegepast, zelfs als het verkeer dat bestemd voor een andere Azure-datacenter in een andere regio is.

> [!WARNING]
> Met behulp van een storage-account in een andere regio dan het HDInsight-cluster wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u extra opslagaccounts toevoegen aan een bestaand HDInsight-cluster. Zie voor meer informatie over scriptacties [aanpassen Linux gebaseerde HDInsight-clusters met script action](hdinsight-hadoop-customize-cluster-linux.md)

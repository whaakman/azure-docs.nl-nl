---
title: Extra Azure storage-accounts toevoegen aan HDInsight | Microsoft Docs
description: Informatie over het toevoegen van extra Azure storage-accounts aan een bestaand HDInsight-cluster.
services: hdinsight
documentationCenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.service: hdinsight
ms.devlang: 
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/04/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 0853e8605e07c28867676e9c13b89263ade67c88
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="add-additional-storage-accounts-to-hdinsight"></a>Extra opslagaccounts toevoegen aan HDInsight

Informatie over het gebruik van scriptacties extra Azure storage-accounts toevoegen aan HDInsight. De stappen in dit document wordt een opslagaccount toevoegen aan een bestaand Linux gebaseerde HDInsight-cluster.

> [!IMPORTANT]
> De informatie in dit document is over het toevoegen van extra opslagruimte aan een cluster nadat deze is gemaakt. Zie voor meer informatie over het storage-accounts toevoegen tijdens het maken van het cluster [clusters in HDInsight met Hadoop, Spark en Kafka instellen](hdinsight-hadoop-provision-linux-clusters.md).

## <a name="how-it-works"></a>Hoe werkt het

Dit script bevat de volgende parameters:

* __Naam van het Azure-opslagaccount__: de naam van het storage-account toevoegen aan het HDInsight-cluster. HDInsight kan na het uitvoeren van het script, lezen en schrijven gegevens opgeslagen in dit opslagaccount.

* __Azure-toegangssleutel__: een sleutel die toegang tot het opslagaccount verleent.

* __-p__ (optioneel): als u opgeeft, wordt de sleutel is niet versleuteld en wordt opgeslagen in het bestand core site.xml als tekst zonder opmaak.

Tijdens de verwerking van het script worden de volgende acties uitgevoerd:

* Als het opslagaccount al in de configuratie van de core site.xml voor het cluster bestaat, het script wordt afgesloten en er is geen verdere acties worden uitgevoerd.

* Hiermee wordt gecontroleerd of het opslagaccount bestaat en toegankelijk is met de sleutel.

* Versleutelt de sleutel met de referenties van het cluster.

* Het storage-account aan het bestand core site.xml toegevoegd.

* Stopt en de Oozie, YARN MapReduce2 en HDFS-services opnieuw gestart. Deze services starten en stoppen, kunnen ze het nieuwe opslagaccount gebruiken.

> [!WARNING]
> Met behulp van een opslagaccount in een andere locatie dan het HDInsight-cluster wordt niet ondersteund.

## <a name="the-script"></a>Het script

__Locatie script__: [https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh](https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh)

__Vereisten__:

* Het script moet worden toegepast op de __hoofdknooppunten__.

## <a name="to-use-the-script"></a>Het script gebruiken

Dit script kan worden gebruikt vanuit de Azure-portal, Azure PowerShell of de Azure CLI 1.0. Zie voor meer informatie de [aanpassen Linux gebaseerde HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md#apply-a-script-action-to-a-running-cluster) document.

> [!IMPORTANT]
> Wanneer u de stappen in het document aanpassing, gebruik de volgende informatie om dit script te passen:
>
> * Een voorbeeld-scriptactie URI vervangen door de URI voor dit script (https://hdiconfigactions.blob.core.windows.net/linuxaddstorageaccountv01/add-storage-account-v01.sh).
> * Van de voorbeeldparameters vervangen door de naam van het Azure-opslagaccount en de sleutel van het opslagaccount moet worden toegevoegd aan het cluster. Als u de Azure portal gebruikt, moeten deze parameters worden gescheiden door een spatie.
> * U hoeft niet te markeren met dit script als __persistente__, zoals de Ambari-configuratie voor het cluster rechtstreeks worden bijgewerkt.

## <a name="known-issues"></a>Bekende problemen

### <a name="storage-accounts-not-displayed-in-azure-portal-or-tools"></a>Storage-accounts niet weergegeven in de Azure portal of hulpprogramma 's

Wanneer het HDInsight-cluster bekijkt in de Azure-portal, selecteert de __Opslagaccounts__ item onder __eigenschappen__ storage-accounts die zijn toegevoegd met deze scriptactie niet wordt weergegeven. Azure PowerShell en Azure CLI weergeven niet het extra opslagaccount ofwel.

De storage-gegevens wordt niet weergegeven omdat het script wijzigt u alleen de core site.xml-configuratie voor het cluster. Deze informatie wordt niet gebruikt bij het ophalen van de clustergegevens met behulp van Azure management API's.

Om weer te geven gegevens van de storage-account is toegevoegd aan het cluster met dit script, moet u de Ambari REST-API gebruiken. Gebruik de volgende opdrachten in deze informatie voor uw cluster op te halen:

```PowerShell
$creds = Get-Credential -UserName "admin" -Message "Enter the cluster login credentials"
$resp = Invoke-WebRequest -Uri "https://$clusterName.azurehdinsight.net/api/v1/clusters/$clusterName/configurations/service_config_versions?service_name=HDFS&service_config_version=1" `
    -Credential $creds
$respObj = ConvertFrom-Json $resp.Content
$respObj.items.configurations.properties."fs.azure.account.key.$storageAccountName.blob.core.windows.net"
```

> [!NOTE]
> Stel `$clusterName` op de naam van het HDInsight-cluster. Stel `$storageAccountName` op de naam van het opslagaccount. Wanneer u wordt gevraagd, typt u de cluster-aanmelding (admin) en het wachtwoord.

```Bash
curl -u admin:PASSWORD -G "https://CLUSTERNAME.azurehdinsight.net/api/v1/clusters/CLUSTERNAME/configurations/service_config_versions?service_name=HDFS&service_config_version=1" | jq '.items[].configurations[].properties["fs.azure.account.key.$STORAGEACCOUNTNAME.blob.core.windows.net"] | select(. != null)'
```

> [!NOTE]
> Stel `$PASSWORD` op het accountwachtwoord van de cluster-aanmelding (admin). Stel `$CLUSTERNAME` op de naam van het HDInsight-cluster. Stel `$STORAGEACCOUNTNAME` op de naam van het opslagaccount.
>
> In dit voorbeeld wordt [curl (http://curl.haxx.se/)](http://curl.haxx.se/) en [jq (https://stedolan.github.io/jq/)](https://stedolan.github.io/jq/) op te halen en JSON-gegevens parseren.

Wanneer u deze opdracht gebruikt, vervangen __CLUSTERNAME__ met de naam van het HDInsight-cluster. Vervang __wachtwoord__ met het aanmeldingswachtwoord HTTP voor het cluster. Vervang __STORAGEACCOUNT__ met de naam van het opslagaccount met behulp van de scriptactie toegevoegd. Met deze opdracht geretourneerde informatie lijkt op de volgende tekst:

    "MIIB+gYJKoZIhvcNAQcDoIIB6zCCAecCAQAxggFaMIIBVgIBADA+MCoxKDAmBgNVBAMTH2RiZW5jcnlwdGlvbi5henVyZWhkaW5zaWdodC5uZXQCEA6GDZMW1oiESKFHFOOEgjcwDQYJKoZIhvcNAQEBBQAEggEATIuO8MJ45KEQAYBQld7WaRkJOWqaCLwFub9zNpscrquA2f3o0emy9Vr6vu5cD3GTt7PmaAF0pvssbKVMf/Z8yRpHmeezSco2y7e9Qd7xJKRLYtRHm80fsjiBHSW9CYkQwxHaOqdR7DBhZyhnj+DHhODsIO2FGM8MxWk4fgBRVO6CZ5eTmZ6KVR8wYbFLi8YZXb7GkUEeSn2PsjrKGiQjtpXw1RAyanCagr5vlg8CicZg1HuhCHWf/RYFWM3EBbVz+uFZPR3BqTgbvBhWYXRJaISwssvxotppe0ikevnEgaBYrflB2P+PVrwPTZ7f36HQcn4ifY1WRJQ4qRaUxdYEfzCBgwYJKoZIhvcNAQcBMBQGCCqGSIb3DQMHBAhRdscgRV3wmYBg3j/T1aEnO3wLWCRpgZa16MWqmfQPuansKHjLwbZjTpeirqUAQpZVyXdK/w4gKlK+t1heNsNo1Wwqu+Y47bSAX1k9Ud7+Ed2oETDI7724IJ213YeGxvu4Ngcf2eHW+FRK"

Deze tekst is een voorbeeld van een versleutelde sleutel die wordt gebruikt voor toegang tot het opslagaccount.

### <a name="unable-to-access-storage-after-changing-key"></a>Kan geen toegang hebben tot opslag na het wijzigen van de sleutel

Als u de sleutel voor een opslagaccount wijzigt, HDInsight kan geen toegang meer tot het opslagaccount. HDInsight gebruikt een cachekopie van de sleutel in het core site.xml voor het cluster. Dit exemplaar in de cache moet worden bijgewerkt zodat deze overeenkomen met de nieuwe sleutel.

Het script opnieuw uitgevoerd biedt __niet__ de sleutel wordt bijgewerkt als het script wordt gecontroleerd of er al een vermelding voor het opslagaccount bestaat. Als er al een item bestaat, biedt het geen wijzigingen aanbrengen.

U kunt dit probleem omzeilen, moet u de bestaande vermelding voor de storage-account verwijderen. Gebruik de volgende stappen uit om te verwijderen van het bestaande item:

1. Open de Ambari-Webgebruikersinterface voor uw HDInsight-cluster in een webbrowser. De URI is https://CLUSTERNAME.azurehdinsight.net. Vervang __CLUSTERNAME__ door de naam van uw cluster.

    Wanneer u wordt gevraagd, typt u de HTTP-aanmeldingsgebruiker en het wachtwoord voor uw cluster.

2. Selecteer in de lijst van services aan de linkerkant van de pagina __HDFS__. Selecteer vervolgens de __Configs__ tabblad in het midden van de pagina.

3. In de __Filter...__  en voer een waarde van __fs.azure.account__. Hiermee wordt de vermeldingen voor elke extra opslagaccounts die zijn toegevoegd aan het cluster. Er zijn twee soorten posten; __keyprovider__ en __sleutel__. Zowel bevatten de naam van het opslagaccount als onderdeel van de naam van de sleutel.

    Hieronder vindt u voorbeelden van vermeldingen voor een opslagaccount met de naam __mystorage__:

        fs.azure.account.keyprovider.mystorage.blob.core.windows.net
        fs.azure.account.key.mystorage.blob.core.windows.net

4. Nadat u hebt vastgesteld dat de sleutels voor het opslagaccount dat u wilt verwijderen, gebruikt u de rode '-' pictogram aan de rechterkant van de vermelding te verwijderen. Gebruik vervolgens de __opslaan__ knop uw wijzigingen op te slaan.

5. Nadat wijzigingen zijn opgeslagen, gebruikt u de scriptactie de storage-account en de nieuwe sleutelwaarde toevoegen aan het cluster.

### <a name="poor-performance"></a>Slechte prestaties

Als het opslagaccount zich in een andere regio dan het HDInsight-cluster, kan u verminderde prestaties ervaren. Toegang tot gegevens in een andere regio verzendt netwerkverkeer buiten het regionale Azure datacenter en via het openbare internet; dit leiden latentie tot kan.

> [!WARNING]
> Met behulp van een opslagaccount in een andere regio dan het HDInsight-cluster wordt niet ondersteund.

### <a name="additional-charges"></a>Extra kosten

Als het opslagaccount zich in een andere regio dan het HDInsight-cluster, merkt u wellicht aanvullende uitgaande kosten op uw Azure-facturering. Een uitgaande kosten wordt toegepast wanneer de gegevens blijft een regionaal datacenter. Deze kosten is toegepast, zelfs als het verkeer is bestemd voor een andere Azure-datacenter in een andere regio.

> [!WARNING]
> Met behulp van een opslagaccount in een andere regio dan het HDInsight-cluster wordt niet ondersteund.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe extra opslagaccounts toevoegen aan een bestaand HDInsight-cluster. Zie voor meer informatie over scriptacties [aanpassen Linux gebaseerde HDInsight-clusters met behulp van de scriptactie](hdinsight-hadoop-customize-cluster-linux.md)

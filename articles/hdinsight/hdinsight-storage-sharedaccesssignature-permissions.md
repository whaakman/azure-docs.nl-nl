---
title: Beperken van toegang met Shared Access Signatures - Azure HDInsight | Microsoft Docs
description: Informatie over het gebruik van handtekeningen voor gedeelde toegang HDInsight toegang tot gegevens die zijn opgeslagen in Azure storage-blobs te beperken.
services: hdinsight
documentationcenter: ''
author: Blackmist
manager: cgronlun
editor: cgronlun
ms.assetid: 7bcad2dd-edea-467c-9130-44cffc005ff3
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/25/2018
ms.author: larryfr
ms.openlocfilehash: d3e37ba0f590cf0572b84a53bdd407af63a19d36
ms.sourcegitcommit: 6fcd9e220b9cd4cb2d4365de0299bf48fbb18c17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/05/2018
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>Gebruik Azure Storage handtekeningen voor gedeelde toegang om toegang te beperken tot gegevens in HDInsight

HDInsight heeft volledige toegang tot gegevens in de Azure Storage-accounts die zijn gekoppeld aan het cluster. U kunt handtekeningen voor gedeelde toegang op de blob-container gebruiken om toegang te beperken tot de gegevens. Als u bijvoorbeeld alleen-lezen toegang tot de gegevens te bieden. Shared Access Signatures (SAS) zijn een functie van Azure storage-accounts waarmee u toegang tot gegevens te beperken. Bijvoorbeeld, alleen-lezen toegang verlenen tot gegevens.

> [!IMPORTANT]
> Voor een oplossing met behulp van Apache Zwerver, kunt u met HDInsight domein. Zie voor meer informatie de [HDInsight domein configureren](./domain-joined/apache-domain-joined-configure.md) document.

> [!WARNING]
> HDInsight moet hebben volledige toegang tot de opslag van de standaard voor het cluster.

## <a name="requirements"></a>Vereisten

* Een Azure-abonnement
* C# of Python. C#-voorbeeldcode is opgegeven als een Visual Studio-oplossing.

  * Visual Studio moet versie 2013 of 2015 2017
  * Python moet 2.7 of hoger zijn

* Een Linux gebaseerde HDInsight-cluster of [Azure PowerShell] [ powershell] -als u een bestaand cluster op basis van Linux hebt, kunt u Ambari Shared Access Signature toevoegen aan het cluster. Als dat niet het geval is, kunt u Azure PowerShell een cluster maken en toevoegen van een Shared Access Signature tijdens het maken van het cluster.

    > [!IMPORTANT]
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* In het voorbeeld bestanden van [ https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature ](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Deze bibliotheek bevat de volgende items:

  * Een Visual Studio-project dat een storage-container, opgeslagen beleid en SAS voor gebruik met HDInsight maken kunt
  * Een pythonscript die een storage-container, opgeslagen beleid en SAS voor gebruik met HDInsight maken kunt
  * Een PowerShell-script waarmee een HDInsight-cluster maken en configureren voor het gebruik van de SAS.

## <a name="shared-access-signatures"></a>Handtekeningen voor gedeelde toegang

Er zijn twee soorten handtekeningen voor gedeelde toegang:

* Ad hoc: de begintijd, verlooptijd en machtigingen voor de SAS zijn alle opgegeven op de SAS-URI.

* Opgeslagen toegangsbeleid: een opgeslagen-beleid is gedefinieerd in een resourcecontainer, zoals een blob-container. Een beleid kan worden gebruikt voor het beheren van beperkingen voor een of meer handtekeningen voor gedeelde toegang. Wanneer u een SAS aan een opgeslagen toegangsbeleid koppelt, neemt de SAS de beperkingen - de begintijd, verlooptijd en machtigingen - gedefinieerd voor het opgeslagen toegangsbeleid.

Het verschil tussen de twee formulieren is belangrijk voor één key '-scenario: intrekken. Een SAS is een URL, zodat iedereen die de SAS verkrijgt kan worden gebruikt, ongeacht wie het eerst hebt aangevraagd. Als een SAS openbaar wordt gepubliceerd, kan deze worden gebruikt door iedereen in de hele wereld. Een SA's dat wordt gedistribueerd is geldig tot vier dingen gebeurt:

1. De verlooptijd opgegeven voor de SAS is bereikt.

2. De verlooptijd opgegeven op het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is bereikt. De volgende scenario's ervoor zorgen dat het verlooptijdstip worden bereikt:

    * Het tijdsinterval is verstreken.
    * Het toegangsbeleid opgeslagen is als u wilt dat een verlooptijd in het verleden gewijzigd. Het wijzigen van het verlooptijdstip is één manier om in te trekken van de SAS.

3. Het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS wordt verwijderd, die een andere manier om in te trekken van de SAS is. Als u het opgeslagen toegangsbeleid met dezelfde naam opnieuw maken, zijn alle SAS-tokens voor het vorige beleid geldig (als de verlooptijd van de SAS niet verstreken is). Als u van plan bent om in te trekken van de SAS, zorg er dan voor dat een andere naam gebruiken als u het toegangsbeleid met een verlooptijd in de toekomst opnieuw maken.

4. De sleutel van het account dat is gebruikt voor het maken van de SAS is gegenereerd. Opnieuw genereren van de sleutel zorgt ervoor dat alle toepassingen die gebruikmaken van de vorige sleutel verificatie moet mislukken. Alle onderdelen die zijn bijgewerkt naar de nieuwe sleutel.

> [!IMPORTANT]
> Een shared access signature-URI is gekoppeld aan de sleutel van het account gebruikt voor het maken van de handtekening en de bijbehorende opgeslagen-beleid (indien aanwezig). Als er geen opgeslagen toegangsbeleid is opgegeven, is de enige manier om een shared access signature intrekken om de accountsleutel te wijzigen.

Het is raadzaam om altijd opgeslagen toegangsbeleid te gebruiken. Als u opgeslagen beleid gebruikt, kunt u handtekeningen intrekken of de vervaldatum uitbreiden naar behoefte. De stappen in dit document opgeslagen toegangsbeleid om SAS te genereren.

Zie voor meer informatie over handtekeningen voor gedeelde toegang [inzicht in het SAS-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Maak een opgeslagen beleid en de SAS met C\#

1. Open de oplossing in Visual Studio.

2. Klik in Solution Explorer met de rechtermuisknop op de **SASToken** project en selecteer **eigenschappen**.

3. Selecteer **instellingen** en waarden voor de volgende vermeldingen toe te voegen:

   * StorageConnectionString: De verbindingsreeks voor het opslagaccount dat u wilt maken van een opgeslagen beleid en een SAS voor. De indeling moet `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` waar `myaccount` is de naam van uw opslagaccount en `mykey` is de sleutel voor het opslagaccount.

   * ContainerName: De container in het opslagaccount dat u wilt beperken van toegang tot.

   * SASPolicyName: De naam moet worden gebruikt voor het opgeslagen beleid maken.

   * FileToUpload: Het pad naar een bestand dat is geüpload naar de container.

4. Voer het project. Informatie die vergelijkbaar is met de volgende tekst wordt weergegeven wanneer de SAS is gegenereerd:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Sla de SAS-token van beleid, opslagaccountnaam en containernaam. Deze waarden worden gebruikt wanneer het storage-account koppelen aan uw HDInsight-cluster.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Maak een opgeslagen beleid en de SAS met behulp van Python

1. Open het bestand SASToken.py en pas de volgende waarden:

   * beleid\_naam: de naam moet worden gebruikt voor het opgeslagen beleid maken.

   * opslag\_account\_naam: de naam van uw opslagaccount.

   * opslag\_account\_sleutel: de sleutel voor het opslagaccount.

   * opslag\_container\_naam: de container in het opslagaccount dat u wilt beperken van toegang tot.

   * voorbeeld\_bestand\_pad: het pad naar een bestand dat is geüpload naar de container.

2. Voer het script uit. Wanneer het script is voltooid, wordt de SAS-token van de volgende strekking weergegeven:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Sla de SAS-token van beleid, opslagaccountnaam en containernaam. Deze waarden worden gebruikt wanneer het storage-account koppelen aan uw HDInsight-cluster.

## <a name="use-the-sas-with-hdinsight"></a>De SAS gebruiken met HDInsight

Wanneer u een HDInsight-cluster maakt, moet u een primaire opslagaccount opgeven en kunt u eventueel extra opslagaccounts opgeven. Beide methoden voor het toevoegen van opslag vereist volledige toegang tot de storage-accounts en containers die worden gebruikt.

Toevoegen als u een Shared Access Signature wilt beperken van toegang tot een container, een aangepaste vermelding voor de **core site** configuratie voor het cluster.

* Voor **Windows gebaseerde** of **op basis van Linux** HDInsight-clusters, kunt u de vermelding toevoegen tijdens het maken van het cluster met behulp van PowerShell.
* Voor **op basis van Linux** HDInsight-clusters, de configuratie na het maken van het cluster met Ambari wijzigen.

### <a name="create-a-cluster-that-uses-the-sas"></a>Maken van een cluster dat gebruik maakt van de SAS

Een voorbeeld van het maken van een HDInsight-cluster dat gebruik maakt van de SAS is opgenomen in de `CreateCluster` map van de opslagplaats. Als u wilt gebruiken, gebruikt u de volgende stappen uit:

1. Open de `CreateCluster\HDInsightSAS.ps1` bestand in een teksteditor en wijzig de volgende waarden aan het begin van het document.

    ```powershell
    # Replace 'mycluster' with the name of the cluster to be created
    $clusterName = 'mycluster'
    # Valid values are 'Linux' and 'Windows'
    $osType = 'Linux'
    # Replace 'myresourcegroup' with the name of the group to be created
    $resourceGroupName = 'myresourcegroup'
    # Replace with the Azure data center you want to the cluster to live in
    $location = 'North Europe'
    # Replace with the name of the default storage account to be created
    $defaultStorageAccountName = 'mystorageaccount'
    # Replace with the name of the SAS container created earlier
    $SASContainerName = 'sascontainer'
    # Replace with the name of the SAS storage account created earlier
    $SASStorageAccountName = 'sasaccount'
    # Replace with the SAS token generated earlier
    $SASToken = 'sastoken'
    # Set the number of worker nodes in the cluster
    $clusterSizeInNodes = 3
    ```

    Wijzig bijvoorbeeld `'mycluster'` op de naam van het cluster dat u wilt maken. De SAS-waarden moeten overeenkomen met de waarden uit de vorige stappen biedt bij het maken van een opslagaccount en SAS-token.

    Zodra u de waarden hebt gewijzigd, sla het bestand.

2. Open een nieuw Azure PowerShell-opdrachtprompt. Als u niet bekend met Azure PowerShell bent of niet hebt geïnstalleerd, raadpleegt u [installeren en configureren van Azure PowerShell][powershell].

1. Gebruik de volgende opdracht bij de verificatie voor uw Azure-abonnement achter de opdrachtprompt:

    ```powershell
    Login-AzureRmAccount
    ```

    Wanneer u wordt gevraagd, aanmelden met het account voor uw Azure-abonnement.

    Als uw account gekoppeld aan meerdere Azure-abonnementen is, moet u mogelijk gebruik `Select-AzureRmSubscription` selecteren van het abonnement dat u wilt gebruiken.

4. Achter de opdrachtprompt, wijzig de mappen op de `CreateCluster` directory waarin het bestand HDInsightSAS.ps1. Gebruik vervolgens de volgende opdracht het script uitvoeren

    ```powershell
    .\HDInsightSAS.ps1
    ```

    Als het script wordt uitgevoerd, geregistreerd in het logboek uitvoer naar de PowerShell-prompt omdat dit de resource-groep en storage-accounts maakt. U wordt gevraagd om in te voeren van de HTTP-gebruiker voor het HDInsight-cluster. Dit account wordt gebruikt voor het beveiligen van HTTP/s-toegang tot het cluster.

    Als u een cluster op basis van Linux maakt, wordt u gevraagd om een SSH-gebruikersnaam voor account en wachtwoord. Dit account wordt gebruikt op afstand aan te melden bij het cluster.

   > [!IMPORTANT]
   > Als u wordt gevraagd om de HTTP/s of SSH-gebruikersnaam en wachtwoord, moet u een wachtwoord dat voldoet aan de volgende criteria opgeven:
   >
   > * Ten minste 10 tekens lang moet zijn
   > * Moet ten minste één cijfer bevatten
   > * Moet ten minste één niet-alfanumeriek teken bevatten
   > * Moet ten minste één hoofdletter of kleine letter bevatten

Het duurt even voor dit script te voltooien, meestal ongeveer 15 minuten. Wanneer het script is voltooid zonder fouten, is het cluster gemaakt.

### <a name="use-the-sas-with-an-existing-cluster"></a>Gebruik de SAS met een bestaand cluster

Als u een bestaand cluster op basis van Linux hebt, kunt u de SAS naar toevoegen de **core site** configuratie met behulp van de volgende stappen uit:

1. Open de Ambari-webgebruikersinterface voor uw cluster. Het adres voor deze pagina is https://YOURCLUSTERNAME.azurehdinsight.net. Wanneer u wordt gevraagd, worden geverifieerd met het cluster via de naam van de beheerder (admin) en het wachtwoord die u hebt gebruikt toen u het cluster maakt.

2. Selecteer in de linkerkant van de Ambari-webgebruikersinterface **HDFS** en selecteer vervolgens de **Configs** tabblad in het midden van de pagina.

3. Selecteer de **Geavanceerd** tabblad en schuif totdat u de **aangepaste core-site** sectie.

4. Vouw de **aangepaste core-site** sectie en blader naar het einde en selecteer de **eigenschap toevoegen...**  koppeling. Gebruik de volgende waarden voor de **sleutel** en **waarde** velden:

   * **Key**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Waarde**: de SAS geretourneerd door de C# of Python-toepassing die u eerder hebt uitgevoerd.

     Vervang **CONTAINERNAME** met de containernaam van de die u gebruikt met de C# of SAS-toepassing. Vervang **STORAGEACCOUNTNAME** met de opslagaccountnaam die u hebt gebruikt.

5. Klik op de **toevoegen** klikken om het opslaan van deze sleutel en waarde en klik vervolgens op de **opslaan** om op te slaan van wijzigingen in de configuratie. Wanneer u wordt gevraagd, een beschrijving van de wijziging ('toe te voegen toegang tot de SAS-opslag' bijvoorbeeld) toevoegen en klik vervolgens op **opslaan**.

    Klik op **OK** wanneer de wijzigingen zijn voltooid.

   > [!IMPORTANT]
   > U moet verschillende services opnieuw starten voordat de wijziging van kracht.

6. Selecteer in de Ambari-webgebruikersinterface, **HDFS** uit de lijst aan de linkerkant en selecteer vervolgens **start opnieuw alle van invloed op een** van de **serviceacties** vervolgkeuzelijst aan de rechterkant. Wanneer u wordt gevraagd, selecteert u __start opnieuw alle bevestigen__.

    Herhaal dit proces voor MapReduce2 en YARN.

7. Als de services opnieuw hebt opgestart, selecteert u elk criterium en uitschakelen van onderhoudsmodus van de **serviceacties** vervolgkeuzelijst.

## <a name="test-restricted-access"></a>Testen met beperkte toegang

Om te controleren of u toegang hebt beperkt, moet u de volgende methoden gebruiken:

* Voor **Windows gebaseerde** HDInsight-clusters, extern bureaublad gebruiken voor verbinding met het cluster. Zie voor meer informatie [verbinding maken met HDInsight met behulp van RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

    Eenmaal zijn verbonden, gebruiken de **Hadoop opdrachtregelprogramma** pictogram op het bureaublad om een opdrachtprompt te openen.

* Voor **op basis van Linux** HDInsight-clusters via SSH verbinding maken met het cluster. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

Eenmaal zijn verbonden met het cluster, gebruikt u de volgende stappen uit om te controleren dat u alleen lezen en de lijst met items op het account van de SAS-opslag kunt:

1. Als de inhoud van de container wilt weergeven, gebruikt u de volgende opdracht achter de opdrachtprompt: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Vervang **SASCONTAINER** met de naam van de container gemaakt voor het account van de SAS-opslag. Vervang **SASACCOUNTNAME** met de naam van het opslagaccount gebruikt voor de SAS.

    De lijst bevat het bestand geüpload wanneer de container en SAS zijn gemaakt.

2. Gebruik de volgende opdracht om te controleren dat u de inhoud van het bestand kan lezen. Vervang de **SASCONTAINER** en **SASACCOUNTNAME** zoals in de vorige stap. Vervang **FILENAME** met de naam van het bestand weergegeven in de vorige opdracht:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Met deze opdracht wordt de inhoud van het bestand.

3. Gebruik de volgende opdracht in het bestand te downloaden naar het lokale bestandssysteem:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Met deze opdracht downloadt u het bestand naar een lokaal bestand met de naam **testbestand.txt**.

4. Gebruik de volgende opdracht voor het uploaden van het lokale bestand naar een nieuw bestand met de naam **testupload.txt** op de SAS-opslag:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    U ontvangt een bericht dat lijkt op de volgende tekst:

        put: java.io.IOException

    Deze fout treedt op omdat de opslaglocatie lezen + alleen lijst. Gebruik de volgende opdracht om de gegevens op de standaard-opslag voor het cluster, alleen-lezenmap is:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    Deze tijd de bewerking is voltooid.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="a-task-was-canceled"></a>Een taak is geannuleerd

**Symptomen**: bij het maken van een cluster met behulp van het PowerShell-script, verschijnt het volgende foutbericht weergegeven:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Oorzaak**: deze fout kan optreden als u een wachtwoord voor de gebruiker admin/HTTP voor het cluster of (voor op basis van Linux-clusters) de SSH-gebruiker.

**Resolutie**: geen wachtwoord gebruiken dat voldoet aan de volgende criteria:

* Ten minste 10 tekens lang moet zijn
* Moet ten minste één cijfer bevatten
* Moet ten minste één niet-alfanumeriek teken bevatten
* Moet ten minste één hoofdletter of kleine letter bevatten

## <a name="next-steps"></a>Volgende stappen

Nu dat u hebt geleerd hoe beperkte toegang opslag toevoegen aan uw HDInsight-cluster, moet u meer informatie over andere manieren om te werken met gegevens op het cluster:

* [Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs

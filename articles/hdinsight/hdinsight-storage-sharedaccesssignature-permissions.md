---
title: Toegang beperken met handtekeningen voor gedeelde toegang - Azure HDInsight
description: Informatie over het gebruik van handtekeningen voor gedeelde toegang HDInsight toegang tot gegevens die zijn opgeslagen in Azure storage-blobs beperken.
services: hdinsight
author: hrasheed-msft
ms.reviewer: jasonh
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 04/23/2018
ms.author: hrasheed
ms.openlocfilehash: 100c9266718d618b8b00a3169c3d88ac7d501791
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53409918"
---
# <a name="use-azure-storage-shared-access-signatures-to-restrict-access-to-data-in-hdinsight"></a>Azure Storage Shared Access Signatures gebruiken om u te beperken van toegang tot gegevens in HDInsight

HDInsight heeft volledige toegang tot gegevens in de Azure Storage-accounts die zijn gekoppeld aan het cluster. U kunt handtekeningen voor gedeelde toegang gebruiken voor de blobcontainer toegang tot de gegevens te beperken. Shared Access Signatures (SAS) zijn een functie van Azure storage-accounts waarmee u toegang tot gegevens te beperken. Bijvoorbeeld, bieden alleen-lezen toegang tot gegevens.

> [!IMPORTANT]  
> Voor een oplossing met behulp van Apache Ranger, kunt u met behulp van aan domein gekoppelde HDInsight. Zie voor meer informatie de [configureren-domain-joined HDInsight](./domain-joined/apache-domain-joined-configure.md) document.

> [!WARNING]  
> HDInsight moet hebben volledige toegang tot de standaardopslag voor het cluster.

## <a name="requirements"></a>Vereisten

* Een Azure-abonnement
* C# of Python. C#-voorbeeldcode wordt geleverd als een Visual Studio-oplossing.

  * Visual Studio moet versie 2013, 2015 of 2017
  * Python moet 2.7 of hoger zijn

* Een Linux gebaseerde HDInsight-cluster of [Azure PowerShell] [ powershell] -als u een bestaande op basis van Linux-cluster hebt, kunt u Apache Ambari een handtekening voor gedeelde toegang toevoegen aan het cluster. Als dat niet het geval is, kunt u Azure PowerShell een cluster maken en toevoegen van een handtekening voor gedeelde toegang tijdens het maken van clusters.

    > [!IMPORTANT]  
    > Linux is het enige besturingssysteem dat wordt gebruikt in HDInsight-versie 3.4 of hoger. Zie [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement) (HDInsight buiten gebruik gestel voor Windows) voor meer informatie.

* Het voorbeeld van de bestanden van [ https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature ](https://github.com/Azure-Samples/hdinsight-dotnet-python-azure-storage-shared-access-signature). Deze bibliotheek bevat de volgende items:

  * Een Visual Studio-project van een storage-container, het opgeslagen beleid en de SAS voor gebruik met HDInsight maken kunt
  * Een pythonscript waarmee u een storage-container, een opgeslagen beleid en een SAS voor gebruik met HDInsight maken kunt
  * Een PowerShell-script dat kan een HDInsight-cluster maken en configureren voor het gebruik van de SAS.

## <a name="shared-access-signatures"></a>Shared Access Signatures

Er zijn twee soorten handtekeningen voor gedeelde toegang:

* Ad hoc: De begintijd, verlooptijd en machtigingen voor de SAS zijn alle opgegeven op de SAS-URI.

* Opgeslagen toegangsbeleid: Een opgeslagen toegangsbeleid is gedefinieerd in een resourcecontainer, zoals een blob-container. Een beleid kan worden gebruikt voor het beheren van beperkingen voor een of meer handtekeningen voor gedeelde toegang. Wanneer u een SAS aan een opgeslagen toegangsbeleid koppelen, neemt de SA's over de beperkingen - de begintijd, verlooptijd en machtigingen - gedefinieerd voor de opgeslagen toegangsbeleid.

Het verschil tussen de twee vormen is belangrijk voor een key-scenario: intrekken. Een SAS is een URL, zodat iedereen die de SAS verkrijgt, ongeacht wie deze begint met aangevraagd kunt gebruiken. Als een SAS openbaar is gepubliceerd, kan deze worden gebruikt door iedereen in de hele wereld. Een SAS die wordt gedistribueerd is geldig tot een van de vier dingen gebeurt:

1. Het verlooptijdstip die is opgegeven voor de SAS is bereikt.

2. Het verlooptijdstip opgegeven op het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is bereikt. De volgende scenario's ervoor zorgen dat het verlooptijdstip worden bereikt:

    * Het tijdsinterval is verstreken.
    * Het opgeslagen toegangsbeleid wordt gewijzigd zodat een verlooptijd in het verleden. Wijzigen van het verlooptijdstip is één manier om in te trekken van de SAS.

3. Het opgeslagen toegangsbeleid waarnaar wordt verwezen door de SAS is verwijderd, die een andere manier om in te trekken van de SAS is. Als u opnieuw het opgeslagen toegangsbeleid met dezelfde naam maken, worden alle SAS-tokens voor het vorige beleid geldig (als het verlooptijdstip voor de SAS niet verstreken is). Als u van plan bent om in te trekken van de SAS, zorg er dan voor dat een andere naam gebruiken als u het toegangsbeleid met een verlooptijd in de toekomst opnieuw maken.

4. De sleutel die is gebruikt voor het maken van de SAS is gegenereerd. De sleutel opnieuw genereren zorgt ervoor dat alle toepassingen die gebruikmaken van de vorige sleutel verificatie moet mislukken. Alle onderdelen bijwerken naar de nieuwe sleutel.

> [!IMPORTANT]  
> Een shared access signature-URI is gekoppeld aan de accountsleutel die wordt gebruikt om de handtekening te maken en de bijbehorende opgeslagen toegangsbeleid (indien aanwezig). Als er geen opgeslagen toegangsbeleid wordt opgegeven, is de enige manier om een shared access signature intrekken om de accountsleutel te wijzigen.

U wordt aangeraden dat u altijd opgeslagen toegangsbeleid gebruiken. Wanneer u beleid voor opgeslagen, kunt u handtekeningen intrekken of de vervaldatum naar wens uitbreiden. De stappen in dit document opgeslagen toegangsbeleid voor het genereren van SAS.

Zie voor meer informatie over handtekeningen voor gedeelde toegang [inzicht in het SAS-model](../storage/common/storage-dotnet-shared-access-signature-part-1.md).

### <a name="create-a-stored-policy-and-sas-using-c"></a>Een opgeslagen beleid en de SAS met C maken\#

1. Open de oplossing in Visual Studio.

2. Klik in Solution Explorer met de rechtermuisknop op de **SASToken** project en selecteer **eigenschappen**.

3. Selecteer **instellingen** en voeg waarden toe voor de volgende items:

   * StorageConnectionString: De verbindingsreeks voor de storage-account dat u wilt maken van een opgeslagen beleid en de SAS voor. De indeling moet `DefaultEndpointsProtocol=https;AccountName=myaccount;AccountKey=mykey` waar `myaccount` is de naam van uw opslagaccount en `mykey` is de sleutel voor het opslagaccount.

   * ContainerName: De container in het opslagaccount dat u wilt toegang te beperken.

   * SASPolicyName: De naam moet worden gebruikt voor het opgeslagen beleid te maken.

   * FileToUpload: Het pad naar een bestand dat is geüpload naar de container.

4. Voer het project. Informatie die vergelijkbaar is met de volgende tekst wordt weergegeven wanneer de SAS is gegenereerd:

        Container SAS token using stored access policy: sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Sla de SAS-token van beleid, opslagaccountnaam en containernaam. Deze waarden worden gebruikt wanneer het storage-account koppelen aan uw HDInsight-cluster.

### <a name="create-a-stored-policy-and-sas-using-python"></a>Maken van een opgeslagen beleid en de SAS met behulp van Python

1. Open het bestand SASToken.py en wijzigt u de volgende waarden:

   * beleid\_naam: De naam moet worden gebruikt voor het opgeslagen beleid te maken.

   * opslag\_account\_naam: De naam van uw storage-account.

   * opslag\_account\_sleutel: De sleutel voor het opslagaccount.

   * opslag\_container\_naam: De container in het opslagaccount dat u wilt toegang te beperken.

   * voorbeeld\_bestand\_pad: Het pad naar een bestand dat is geüpload naar de container.

2. Voer het script uit. Wanneer het script is voltooid, wordt de SAS-token die vergelijkbaar is met de volgende tekst weergegeven:

        sr=c&si=policyname&sig=dOAi8CXuz5Fm15EjRUu5dHlOzYNtcK3Afp1xqxniEps%3D&sv=2014-02-14

    Sla de SAS-token van beleid, opslagaccountnaam en containernaam. Deze waarden worden gebruikt wanneer het storage-account koppelen aan uw HDInsight-cluster.

## <a name="use-the-sas-with-hdinsight"></a>De SAS gebruiken met HDInsight

Wanneer u een HDInsight-cluster maakt, moet u een primaire opslagaccount opgeven en kunt u eventueel extra opslagaccounts opgeven. Beide methoden van het toevoegen van opslag vereist volledige toegang tot de storage-accounts en de containers die worden gebruikt.

Toevoegen als u een handtekening voor gedeelde toegang wilt beperken van toegang tot een container, een aangepaste vermelding voor de **core-site** configuratie van het cluster.

* Voor **op basis van Windows** of **op basis van Linux** HDInsight-clusters, kunt u de vermelding toevoegen tijdens het maken van een cluster met behulp van PowerShell.
* Voor **op basis van Linux** HDInsight-clusters, de configuratie na het maken van een cluster met behulp van Ambari wijzigen.

### <a name="create-a-cluster-that-uses-the-sas"></a>Maken van een cluster dat gebruik maakt van de SAS

Een voorbeeld van het maken van een HDInsight-cluster dat gebruik maakt van de SAS is opgenomen in de `CreateCluster` map van de opslagplaats. Als u wilt gebruiken, gebruikt u de volgende stappen uit:

1. Open de `CreateCluster\HDInsightSAS.ps1` -bestand in een teksteditor en wijzig de volgende waarden aan het begin van het document.

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

    Nadat u de waarden hebt gewijzigd, sla het bestand.

2. Open een nieuwe Azure PowerShell-prompt. Als u niet bekend met Azure PowerShell bent of nog niet hebt geïnstalleerd, raadpleegt u [installeren en configureren van Azure PowerShell][powershell].

1. Gebruik de volgende opdracht om te verifiëren bij uw Azure-abonnement achter de opdrachtprompt:

    ```powershell
    Connect-AzureRmAccount
    ```

    Wanneer u hierom wordt gevraagd, meld u aan met het account voor uw Azure-abonnement.

    Als uw account gekoppeld aan meerdere Azure-abonnementen is, moet u mogelijk gebruik `Select-AzureRmSubscription` om te selecteren van het abonnement dat u wilt gebruiken.

4. De opdrachtprompt en wijzig de mappen op de `CreateCluster` directory waarin het bestand HDInsightSAS.ps1. Gebruik vervolgens de volgende opdracht om uit te voeren van het script

    ```powershell
    .\HDInsightSAS.ps1
    ```

    Als het script wordt uitgevoerd, geregistreerd in het logboek uitvoer naar de PowerShell-prompt als deze de resource-group en storage-accounts maakt. U wordt gevraagd om in te voeren van de HTTP-gebruiker voor het HDInsight-cluster. Dit account wordt gebruikt voor het beveiligen van toegang tot het cluster HTTP/s.

    Als u een cluster op basis van Linux maakt, wordt u gevraagd om een SSH-gebruikersnaam voor account en wachtwoord. Dit account wordt gebruikt op afstand aan te melden het cluster.

   > [!IMPORTANT]  
   > Wanneer u hierom wordt gevraagd om de HTTP/s of SSH-gebruikersnaam en wachtwoord, moet u een wachtwoord dat voldoet aan de volgende criteria opgeven:
   >
   > * Ten minste 10 tekens lang moet zijn
   > * Moet ten minste één cijfer bevatten
   > * Moet ten minste één niet-alfanumerieke tekens bevatten
   > * Moet ten minste één hoofdletters of kleine letter bevatten

Het duurt even voor dit script om uit te voeren, meestal ongeveer 15 minuten. Wanneer het script is voltooid zonder fouten, is het cluster gemaakt.

### <a name="use-the-sas-with-an-existing-cluster"></a>Gebruik de SAS met een bestaand cluster

Als u een bestaande op basis van Linux-cluster hebt, kunt u de SAS toevoegen de **core-site** configuratie met behulp van de volgende stappen uit:

1. Open de Ambari-Webinterface voor uw cluster. Het adres voor deze pagina is https://YOURCLUSTERNAME.azurehdinsight.net. Wanneer u hierom wordt gevraagd, verifiëren met het cluster via beheerdersnaam voor de (beheerder) en het wachtwoord die u hebt gebruikt tijdens het maken van het cluster.

2. Selecteer in de linkerkant van de Ambari-Webgebruikersinterface, **HDFS** en selecteer vervolgens de **Peeringconfiguraties** tabblad in het midden van de pagina.

3. Selecteer de **Geavanceerd** tabblad en schuif totdat u de **aangepaste core-site** sectie.

4. Vouw de **aangepaste core-site** sectie en blader naar het einde en selecteer de **eigenschap toevoegen...**  koppeling. Gebruik de volgende waarden voor de **sleutel** en **waarde** velden:

   * **Sleutel**: fs.azure.sas.CONTAINERNAME.STORAGEACCOUNTNAME.blob.core.windows.net
   * **Waarde**: De SAS die wordt geretourneerd door de C# of Python-toepassing die u eerder hebt uitgevoerd

     Vervang **CONTAINERNAME** met de containernaam van de die u gebruikt met de C# of SAS-toepassing. Vervang **STORAGEACCOUNTNAME** met de opslagaccountnaam die u hebt gebruikt.

5. Klik op de **toevoegen** klikken om deze sleutel en waarde opslaan en klik vervolgens op de **opslaan** om op te slaan van de configuratiewijzigingen. Wanneer u hierom wordt gevraagd, een beschrijving van de wijziging ('toe te voegen toegang tot de SAS-opslag' bijvoorbeeld) toevoegen en klik vervolgens op **opslaan**.

    Klik op **OK** wanneer de wijzigingen zijn voltooid.

   > [!IMPORTANT]  
   > U moet verschillende services opnieuw starten voordat de wijziging wordt van kracht.

6. Selecteer in de Ambari-Webgebruikersinterface **HDFS** in de lijst aan de linkerkant en selecteer vervolgens **start opnieuw op alle betrokken** uit de **serviceacties** vervolgkeuzelijst aan de rechterkant. Wanneer u hierom wordt gevraagd, selecteert u __alle opnieuw starten bevestigen__.

    Herhaal dit proces voor MapReduce2 en YARN.

7. Zodra de services opnieuw hebt opgestart, selecteert u elk en uitschakelen van onderhoudsmodus van de **serviceacties** vervolgkeuzelijst.

## <a name="test-restricted-access"></a>Testen met beperkte toegang

Als u wilt controleren of u toegang hebt beperkt, moet u de volgende methoden gebruiken:

* Voor **op basis van Windows** HDInsight-clusters, extern bureaublad gebruiken voor verbinding met het cluster. Zie voor meer informatie, [verbinding maken met HDInsight met behulp van RDP](hdinsight-administer-use-management-portal.md#connect-to-clusters-using-rdp).

    Zodra de verbinding is gemaakt, gebruikt u de **Hadoop opdrachtregel** pictogram op het bureaublad om een opdrachtprompt te openen.

* Voor **op basis van Linux** HDInsight-clusters gebruikmaken van SSH verbinding maken met het cluster. Zie [SSH gebruiken met HDInsight](hdinsight-hadoop-linux-use-ssh-unix.md) voor meer informatie.

Eenmaal verbinding hebben met het cluster, gebruikt u de volgende stappen uit om te controleren of dat u alleen lezen en de lijst met items in de SAS-storage-account kunt:

1. Als u de inhoud van de container, gebruik de volgende opdracht achter de opdrachtprompt: 

    ```bash
    hdfs dfs -ls wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/
    ```

    Vervang **SASCONTAINER** met de naam van de container voor de SAS-storage-account hebben gemaakt. Vervang **SASACCOUNTNAME** met de naam van het opslagaccount die wordt gebruikt voor de SAS.

    De lijst bevat het bestand geüpload wanneer de container en de SAS zijn gemaakt.

2. Gebruik de volgende opdracht om te controleren of u kunt de inhoud van het bestand lezen. Vervang de **SASCONTAINER** en **SASACCOUNTNAME** zoals in de vorige stap. Vervang **FILENAME** met de naam van het bestand dat wordt weergegeven in de voorgaande opdracht:

    ```bash
    hdfs dfs -text wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME
    ```

    Met deze opdracht worden de inhoud van het bestand.

3. Gebruik de volgende opdracht om te downloaden van het bestand naar het lokale bestandssysteem:

    ```bash
    hdfs dfs -get wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/FILENAME testfile.txt
    ```

    Met deze opdracht wordt het bestand gedownload naar een lokaal bestand met de naam **testbestand.txt**.

4. Gebruik de volgende opdracht voor het uploaden van het lokale bestand naar een nieuw bestand met de naam **testupload.txt** op de SAS-opslag:

    ```bash
    hdfs dfs -put testfile.txt wasb://SASCONTAINER@SASACCOUNTNAME.blob.core.windows.net/testupload.txt
    ```

    U ontvangt een bericht dat lijkt op de volgende tekst:

        put: java.io.IOException

    Deze fout treedt op omdat de opslaglocatie lezen + alleen lijst wijkt. Gebruik de volgende opdracht om de gegevens op de standaardopslag voor het cluster, die kan worden bewerkt:

    ```bash
    hdfs dfs -put testfile.txt wasb:///testupload.txt
    ```

    Deze tijd voltooid de bewerking is.

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="a-task-was-canceled"></a>Een taak is geannuleerd

**Symptomen**: Bij het maken van een cluster met behulp van het PowerShell-script, wordt de volgende strekking weergegeven:

    New-AzureRmHDInsightCluster : A task was canceled.
    At C:\Users\larryfr\Documents\GitHub\hdinsight-azure-storage-sas\CreateCluster\HDInsightSAS.ps1:62 char:5
    +     New-AzureRmHDInsightCluster `
    +     ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
        + CategoryInfo          : NotSpecified: (:) [New-AzureRmHDInsightCluster], CloudException
        + FullyQualifiedErrorId : Hyak.Common.CloudException,Microsoft.Azure.Commands.HDInsight.NewAzureHDInsightClusterCommand

**Oorzaak**: Deze fout kan optreden als u de SSH-gebruiker een wachtwoord voor de beheerder/HTTP-gebruiker voor het cluster, of (voor Linux gebaseerde clusters) gebruikt.

**Resolutie**: Gebruik een wachtwoord dat voldoet aan de volgende criteria:

* Ten minste 10 tekens lang moet zijn
* Moet ten minste één cijfer bevatten
* Moet ten minste één niet-alfanumerieke tekens bevatten
* Moet ten minste één hoofdletters of kleine letter bevatten

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u beperkte toegang opslag toevoegen aan uw HDInsight-cluster, informatie over andere manieren om te werken met gegevens in uw cluster:

* [Apache Hive gebruiken met HDInsight](hadoop/hdinsight-use-hive.md)
* [Apache Pig gebruiken met HDInsight](hadoop/hdinsight-use-pig.md)
* [MapReduce gebruiken met HDInsight](hadoop/hdinsight-use-mapreduce.md)

[powershell]: /powershell/azureps-cmdlets-docs

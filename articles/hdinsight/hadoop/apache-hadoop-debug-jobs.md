---
title: 'Fouten opsporen in een Apache Hadoop: Logboekbestanden weergeven en interpreteer foutberichten - Azure HDInsight'
description: Meer informatie over de foutberichten die u mogelijk ontvangt bij het beheren van HDInsight met behulp van PowerShell en de stappen die u kunt nemen om te herstellen.
services: hdinsight
ms.reviewer: jasonh
author: ashishthaps
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 11/14/2017
ms.author: ashishth
ms.openlocfilehash: 4d3d45434a90e89e011bf8aa7c3fd37909e25386
ms.sourcegitcommit: dec7947393fc25c7a8247a35e562362e3600552f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58203061"
---
# <a name="analyze-apache-hadoop-logs"></a>Apache Hadoop-logboeken analyseren

Elke Apache Hadoop-cluster in Azure HDInsight is een Azure storage-account gebruikt als het standaardbestandssysteem. Het opslagaccount dat wordt aangeduid als het standaardaccount voor opslag. Cluster maakt gebruik van de Azure Table storage en de Blob-opslag op het standaardopslagaccount voor het opslaan van de logboeken.  Als u wilt het standaardopslagaccount voor uw cluster weten, Zie [beheren Apache Hadoop-clusters in HDInsight](../hdinsight-administer-use-portal-linux.md#find-the-storage-accounts). De logboeken behouden in de Storage-account, zelfs nadat het cluster wordt verwijderd.

## <a name="logs-written-to-azure-tables"></a>Logboeken geschreven naar Azure Tables

De logboeken geschreven naar Azure Tables bieden één niveau van inzicht in wat met een HDInsight-cluster gebeurt er.

Wanneer u een HDInsight-cluster maakt, worden automatisch zes tabellen voor op basis van Linux-clusters in de standaardopslag voor tabel gemaakt:

* hdinsightagentlog
* syslog
* daemonlog
* hadoopservicelog
* ambariserverlog
* ambariagentlog

De namen van de tabel zijn **u<ClusterName>DDMonYYYYatHHMMSSsss<TableName>**.

Deze tabellen bevatten de volgende velden:

* ClusterDnsName
* ComponentName
* EventTimestamp
* Host
* MALoggingHash
* Bericht
* N
* PreciseTimeStamp
* Rol
* RowIndex
* Tenant
* TIJDSTEMPEL
* TraceLevel

### <a name="tools-for-accessing-the-logs"></a>Hulpprogramma's voor toegang tot de logboeken
Er zijn veel hulpprogramma's beschikbaar zijn voor toegang tot gegevens in deze tabellen:

* Visual Studio
* Azure Opslagverkenner
* Power Query voor Excel

#### <a name="use-power-query-for-excel"></a>Gebruik Power Query voor Excel
Power Query kan worden geïnstalleerd vanaf [Microsoft Power Query voor Excel](https://www.microsoft.com/en-us/download/details.aspx?id=39379). Zie de downloadpagina voor de systeemvereisten.

**Gebruik Power Query om te openen en analyseren van de service-logboek**

1. Open **Microsoft Excel**.
2. Uit de **Power Query** menu, klikt u op **van Azure**, en klik vervolgens op **van Microsoft Azure Table storage**.
   
    ![HDInsight Hadoop Excel PowerQuery open Azure Table storage](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Voer de naam van het opslagaccount, de korte naam of de FQDN-naam.
4. Voer de opslagaccountsleutel. U ziet een lijst met tabellen:
   
    ![HDInsight Hadoop-logboeken die zijn opgeslagen in Azure Table storage](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Met de rechtermuisknop op de tabel hadoopservicelog in de **Navigator** deelvenster en selecteer **bewerken**. Vier kolommen wordt weergegeven. Verwijder desgewenst, de **partitiesleutel**, **rijsleutel**, en **Timestamp** kolommen door ze te selecteren en vervolgens te klikken op **kolommen verwijderen** uit de opties in het lint.
6. Klik op het uitvouwpictogram in de kolom inhoud om te kiezen van de kolommen die u wilt importeren in de Excel-spreadsheet. Ik heb geselecteerd voor deze demonstratie TraceLevel en ComponentName: Geeft me basisinformatie op te geven waarop onderdelen problemen had.
   
    ![HDInsight Hadoop-logboeken kiezen kolommen](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Klik op **OK** om de gegevens te importeren.
8. Selecteer de **TraceLevel**, rol, en **ComponentName** kolommen, en klik vervolgens op **Group By** besturingselement in het lint.
9. Klik op **OK** in het dialoogvenster Group By
10. Klik op ** toepassen & sluiten **.

U kunt nu Excel gebruiken om te filteren en sorteren indien nodig. U wilt opnemen van andere kolommen (zoals bericht) als u wilt inzoomen op problemen wanneer ze zich voordoen, maar selecteren en groeperen van de kolommen die hierboven worden beschreven, een goede beeld biedt van wat met Hadoop-services gebeurt er. Het hetzelfde idee kan worden toegepast op het bestand en hadoopinstalllog tabellen.

#### <a name="use-visual-studio"></a>Visual Studio gebruiken
**Visual Studio gebruiken**

1. Open Visual Studio.
2. Uit de **weergave** menu, klikt u op **Cloud Explorer**. Of klik gewoon **CTRL +\, CTRL + X**.
3. Van **Cloud Explorer**, selecteer **resourcetypen**.  De andere beschikbare optie **resourcegroepen**.
4. Vouw **Opslagaccounts**, het standaardopslagaccount voor uw cluster, en vervolgens **tabellen**.
5. Dubbelklik op **hadoopservicelog**.
6. Voeg een filter toe. Bijvoorbeeld:
   
        TraceLevel eq 'ERROR'
   
    ![HDInsight Hadoop-logboeken kiezen kolommen](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Zie voor meer informatie over het maken van filters [filtertekenreeksen maken voor de Table Designer](../../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-to-azure-blob-storage"></a>Logboeken geschreven naar Azure Blob-opslag
De logboeken geschreven naar Azure Tables bieden één niveau van inzicht in wat met een HDInsight-cluster gebeurt er. Deze tabellen bieden echter geen in Logboeken van de taak op het niveau, die nuttig bij het analyseren zijn kunnen verdere problemen wanneer ze zich voordoen. HDInsight-clusters zijn voor deze volgende detailniveau, geconfigureerd voor de taak logboeken naar uw Blob Storage-account voor elke taak die wordt ingediend via Templeton schrijven. Dit betekent nagenoeg, taken die worden ingediend met behulp van de Microsoft Azure PowerShell-cmdlets of de .NET-taak indienen API's, niet de taken die zijn verzonden via RDP/vanaf de opdrachtregel-line toegang tot het cluster. 

Als u de logboeken, Zie [toegang Apache Hadoop YARN-toepassingslogboeken in Linux gebaseerde HDInsight](../hdinsight-hadoop-access-yarn-app-logs-linux.md).


Zie voor meer informatie over toepassingslogboeken [vereenvoudigen van beheer van gebruiker-logboeken en toegang in Apache Hadoop YARN](https://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).


## <a name="view-cluster-health-and-job-logs"></a>Cluster-status en taak logboeken bekijken
### <a name="access-the-ambari-ui"></a>Toegang tot de Ambari-gebruikersinterface
Klik op de naam van een HDInsight-cluster om de cluster-deelvenster te openen vanuit de Azure-portal. Klik in het deelvenster cluster **Dashboard**.

![Starten van de cluster-dashboard](./media/apache-hadoop-debug-jobs/hdi-debug-launch-dashboard.png)


### <a name="access-the-yarn-ui"></a>Toegang tot de Yarn-gebruikersinterface
Klik op de naam van een HDInsight-cluster om de cluster-deelvenster te openen vanuit de Azure-portal. Klik in het deelvenster cluster **Dashboard**. Voer desgevraagd de referenties van de cluster. Selecteer in de Ambari, **YARN** uit de lijst met services aan de linkerkant. Selecteer op de pagina die wordt weergegeven, **snelkoppelingen**, selecteert u de actieve hoofdknooppunt vermelding en resourcemanager UI.

De gebruikersinterface van YARN kunt u het volgende doen:

* **Clusterstatus ophalen**. Vouw in het linkerdeelvenster **Cluster**, en klikt u op **over**. Deze aanwezig cluster statusdetails zoals Totaal toegewezen geheugen en kernen gebruikt, status van de cluster resourcemanager, clusterversie, enzovoort.
  
    ![Starten van de cluster-dashboard](./media/apache-hadoop-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Knooppuntstatus van ophalen**. Vouw in het linkerdeelvenster **Cluster**, en klikt u op **knooppunten**. Dit geeft een lijst van alle knooppunten in het cluster, HTTP-adres van elk knooppunt resources toegewezen aan elk knooppunt, enzovoort.
* **Taakstatus controleren**. Vouw in het linkerdeelvenster **Cluster**, en klik vervolgens op **toepassingen** om alle taken in het cluster weer te geven. Als u wilt kijken naar taken in een specifieke status (zoals de nieuwe, verzonden, wordt uitgevoerd, enzovoort), klikt u op de desbetreffende koppeling onder **toepassingen**. Verder kunt u de naam van de taak voor meer informatie over de taak die met inbegrip van de uitvoer, logboekbestanden, enzovoort.

### <a name="access-the-hbase-ui"></a>Toegang tot de HBase-gebruikersinterface
Klik op de naam van een HDInsight HBase-cluster om de cluster-deelvenster te openen vanuit de Azure-portal. Klik in het deelvenster cluster **Dashboard**. Voer desgevraagd de referenties van de cluster. Selecteer in Ambari, HBase uit de lijst met services. Selecteer **snelkoppelingen** boven aan de pagina, wijst u de actieve Zookeeper-knooppuntkoppeling en klik vervolgens op HBase Master UI.

## <a name="hdinsight-error-codes"></a>HDInsight-foutcodes
De foutberichten die is gespecificeerd in deze sectie vindt u aan zodat de gebruikers van Hadoop in Azure HDInsight mogelijke fouten die optreden bij het beheer van de service met behulp van Azure PowerShell kan begrijpen en te adviseren ze op de stappen die kunnen worden uitgevoerd om te herstellen van de fout.

Sommige van deze foutberichten kan ook worden weergegeven in de Azure-portal als deze wordt gebruikt voor het beheren van HDInsight-clusters. Andere foutberichten die mogelijk optreden, maar er zijn minder gedetailleerd vanwege de beperkingen met betrekking tot de corrigerende acties mogelijk in deze context. Andere foutberichten vindt u in de context waarin de risicobeperking duidelijk is. 

### <a id="AtLeastOneSqlMetastoreMustBeProvided"></a>AtLeastOneSqlMetastoreMustBeProvided
* **Beschrijving**: Geef informatie over de Azure SQL database voor ten minste één onderdeel om te kunnen gebruiken van aangepaste instellingen voor Hive en Oozie metastores.
* **Risicobeperking**: De gebruiker moet een geldige SQL Azure-metastore opgeven en de aanvraag opnieuw proberen.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Beschrijving**: Kan geen cluster maken in de regio *nameOfYourRegion*. Gebruik een geldige HDInsight-regio en de aanvraag opnieuw proberen.
* **Risicobeperking**: Klant moet maken van het cluster de regio die momenteel worden ondersteund: Zuidoost-Azië, West-Europa, Noord-Europa, VS-Oost of VS-West.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Beschrijving**: De server kan de aangevraagde cluster-record niet vinden.  
* **Risicobeperking**: De bewerking opnieuw proberen.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Beschrijving**: Cluster DNS-naam *yourDnsName* is ongeldig. Controleer of de naam begint en eindigt met alfanumerieke tekens en mag alleen '-' speciaal teken  
* **Risicobeperking**: Zorg ervoor dat u een geldige DNS-naam hebt gebruikt voor het cluster dat begint en eindigt met alfanumerieke tekens en bevat geen speciale tekens dan het streepje '-' en voer de bewerking vervolgens opnieuw uit.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Beschrijving**: De naam van cluster *yourClusterName* is niet beschikbaar. Kies een andere naam.  
* **Risicobeperking**: De gebruiker moet opgeven van een clusternaam die uniek is en niet bestaat en probeer het opnieuw. Als de gebruiker via de Portal, de gebruikersinterface wordt ze hierover te informeren als de naam van een cluster al wordt gebruikt tijdens de stappen maken.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Beschrijving**: Wachtwoord voor het cluster is ongeldig. Wachtwoord moet ten minste 10 tekens lang zijn en moet ten minste één cijfer, hoofdletter, kleine letter en speciaal teken zonder spaties bevatten en mag niet de gebruikersnaam als onderdeel hiervan.  
* **Risicobeperking**: Geef een geldig clusterwachtwoord op en probeer het opnieuw.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Beschrijving**: Cluster-gebruikersnaam is ongeldig. Controleer of de gebruikersnaam geen speciale tekens of spaties bevatten.  
* **Risicobeperking**: Geef een geldig cluster-gebruikersnaam en probeer het opnieuw.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Beschrijving**: Cluster DNS-naam *yourDnsClusterName* is ongeldig. Controleer of de naam begint en eindigt met alfanumerieke tekens en mag alleen '-' speciaal teken  
* **Risicobeperking**: Geef een geldige gebruikersnaam van de DNS-cluster en voer de bewerking opnieuw uit.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Beschrijving**: De containernaam van de in URI *yourcontainerURI* en DNS-naam *yourDnsName* in de aanvraag hoofdtekst moet hetzelfde zijn.  
* **Risicobeperking**: Zorg ervoor dat de naam van de container en uw DNS-naam hetzelfde zijn, en probeer het opnieuw.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Beschrijving**: Ongeldige clusterconfiguratie. Kan niet alle gegevens Knooppuntdefinities in de grootte van knooppunt vinden.  
* **Risicobeperking**: De bewerking opnieuw proberen.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Beschrijving**: Verwijderen van de implementatie is mislukt voor het Cluster  
* **Risicobeperking**: Opnieuw proberen te verwijderen.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Beschrijving**: Fout bij het configureren van de service. Vereiste DNS-toewijzingsgegevens niet gevonden.  
* **Risicobeperking**: Cluster verwijderen en een nieuw cluster maken.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Beschrijving**: Poging om dubbele cluster container maken. Record bestaat voor *nameOfYourContainer* maar Etags komen niet overeen.
* **Risicobeperking**: Geef een unieke naam voor de container en probeer het opnieuw maken.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Beschrijving**: De gehoste service *nameOfYourHostedService* bevat al een cluster. Een gehoste service kan niet meerdere clusters bevatten  
* **Risicobeperking**: Het cluster in een andere gehoste service hosten.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Beschrijving**: De server kan de status van de implementatie van het cluster niet bijwerken.  
* **Risicobeperking**: De bewerking opnieuw proberen. Als dit herhaaldelijk gebeurt, moet u contact op met CSS.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Beschrijving**: Cluster *yourClusterName* is verwijderd als onderdeel van onderhoud. Maak een nieuwe het cluster.
* **Risicobeperking**: Maak het cluster opnieuw.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Beschrijving**: Ongeldige clusterconfiguratie. De configuratie van de vereiste hoofdknooppunt is niet gevonden in de grootte van.
* **Risicobeperking**: De bewerking opnieuw proberen.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Beschrijving**: Kan niet worden gemaakt van de gehoste service *nameOfYourHostedService*. Probeer de aanvraag.  
* **Risicobeperking**: De aanvraag opnieuw.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Beschrijving**: De gehoste Service *nameOfYourHostedService* heeft al een productie-implementatie. Een gehoste service kan niet meerdere productie-implementaties bevatten. De aanvraag met een andere clusternaam op opnieuw proberen.
* **Risicobeperking**: Gebruik een andere clusternaam op en probeer de aanvraag opnieuw.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Beschrijving**: De gehoste Service *nameOfYourHostedService* voor het cluster kan niet worden gevonden.  
* **Risicobeperking**: Als het cluster zich in de foutstatus, verwijderen en probeer het vervolgens opnieuw.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Beschrijving**: De gehoste Service *nameOfYourHostedService* heeft geen gekoppelde implementatie.  
* **Risicobeperking**: Als het cluster zich in de foutstatus, verwijderen en probeer het vervolgens opnieuw.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Beschrijving**: De abonnements-id *yourSubscriptionId* heeft geen kerngeheugens links-cluster maakt *yourClusterName*. Vereist: *resourcesRequired*, beschikbaar: *resourcesAvailable*.  
* **Risicobeperking**: Resources in uw abonnement vrij te maken of de beschikbare resources voor het abonnement te verhogen en probeer het opnieuw maken van het cluster.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Beschrijving**: Abonnements-ID *yourSubscriptionId* heeft geen quota voor een nieuwe HostedService cluster maken *yourClusterName*.  
* **Risicobeperking**: Resources in uw abonnement vrij te maken of de beschikbare resources voor het abonnement te verhogen en probeer het opnieuw maken van het cluster.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Beschrijving**: Er is een interne fout opgetreden op de server. Probeer de aanvraag.  
* **Risicobeperking**: De aanvraag opnieuw.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Beschrijving**: Azure Storage-locatie *dataRegionName* is geen geldige locatie. Controleer of dat de regio juist is en de aanvraag opnieuw proberen.
* **Risicobeperking**: Selecteer een locatie voor de opslag die ondersteuning biedt voor HDInsight, Controleer of het cluster CO-locatie is en probeer het opnieuw.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Beschrijving**: Ongeldige VM-grootte voor gegevensknooppunten. Alleen de grootte van de grote virtuele machine wordt ondersteund voor alle gegevensknooppunten.  
* **Risicobeperking**: Geef de knooppuntgrootte van de ondersteunde voor op het gegevensknooppunt en voer de bewerking opnieuw uit.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Beschrijving**: Ongeldige VM-grootte voor het hoofdknooppunt. Alleen 'zijn VM-grootte wordt ondersteund voor het hoofdknooppunt.  
* **Risicobeperking**: Geef de grootte van het ondersteunde knooppunt voor het hoofdknooppunt en probeer het opnieuw

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Beschrijving**: Abonnements-ID *yourSubscriptionId* gebruikte heeft geen voldoende machtigingen voor het uitvoeren van de verwijderbewerking voor cluster *yourClusterName*.  
* **Risicobeperking**: Als het cluster zich in de foutstatus, zet het neer in en probeer het opnieuw.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Beschrijving**: Naam van de externe opslag account blob-container *yourContainerName* is ongeldig. Zorg ervoor dat de naam begint met een letter en bevat alleen kleine letters, cijfers en streepjes.  
* **Risicobeperking**: Geef een geldige blob-container op en probeer het opnieuw.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Beschrijving**: Configuratie voor het externe opslagaccount *yourStorageAccountName* is vereist om details van de geheime sleutel moet worden ingesteld.  
* **Risicobeperking**: Geef een geldige geheime sleutel voor het opslagaccount en voer de bewerking opnieuw uit.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Beschrijving**: Version-kopwaarde *yourVersionHeader* is geen geldige notatie jjjj-mm-dd.  
* **Risicobeperking**: Geef een geldige indeling voor de versie-header en probeer de aanvraag.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Beschrijving**: Ongeldige clusterconfiguratie. Configuratie van meer dan één hoofdknooppunt gevonden.  
* **Risicobeperking**: De configuratie wijzigen zodat deze slechts één hoofdknooppunt is opgegeven.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Beschrijving**: De bewerking kan niet worden voltooid binnen de toegestane tijd of het maximaal aantal nieuwe pogingen mogelijk. Probeer de aanvraag.  
* **Risicobeperking**: De aanvraag opnieuw.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Beschrijving**: Parameter *yourParameterName* mag niet null of leeg zijn.  
* **Risicobeperking**: Geef een geldige waarde voor de parameter.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Beschrijving**: Een of meer van de cluster-invoer voor het maken van aanvraag is niet geldig. Zorg ervoor dat de ingevoerde waarden correct zijn en de aanvraag opnieuw proberen.  
* **Risicobeperking**: Zorg ervoor dat de ingevoerde waarden correct zijn en de aanvraag opnieuw proberen.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Beschrijving**: Mogelijkheid van de regio niet beschikbaar voor de regio *yourRegionName* en abonnements-ID *yourSubscriptionId*.  
* **Risicobeperking**: Geef een regio die ondersteuning biedt voor HDInsight-clusters. De openbaar ondersteunde regio's zijn: Zuidoost-Azië, West-Europa, Noord-Europa, VS-Oost of VS-West.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Beschrijving**: Storage-account *yourStorageAccountName* is in de regio *currentRegionName*. Deze moet gelijk zijn aan het cluster de regio *yourClusterRegionName*.  
* **Risicobeperking**: Geef een opslagaccount in dezelfde regio als uw cluster zich in of als uw gegevens zich al in de storage-account, een nieuw cluster maken in dezelfde regio als het bestaande storage-account. Als u van de Portal gebruikmaakt, wordt de gebruikersinterface van dit probleem vooraf kennis.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Beschrijving**: Abonnements-ID opgegeven *yourSubscriptionId* is niet actief.  
* **Risicobeperking**: Uw abonnement opnieuw activeren op of maak een nieuw geldig abonnement.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Beschrijving**: Abonnements-ID *yourSubscriptionId* kan niet worden gevonden.  
* **Risicobeperking**: Controleer of uw abonnements-ID geldig is en probeer het opnieuw.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Beschrijving**: Kan niet worden omgezet DNS *yourDnsUrl*. Controleer of dat de volledig gekwalificeerde URL voor de blobeindpunt is opgegeven.  
* **Risicobeperking**: Geef een geldige blob-URL. De URL moet worden volledig geldig is, met inbegrip van beginnen met *http://* en eindigt op *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Beschrijving**: Kan niet verifiëren van de locatie van de resource *yourDnsUrl*. Controleer of dat de volledig gekwalificeerde URL voor de blobeindpunt is opgegeven.  
* **Risicobeperking**: Geef een geldige blob-URL. De URL moet worden volledig geldig is, met inbegrip van beginnen met *http://* en eindigt op *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Beschrijving**: Versie-mogelijkheid is niet beschikbaar zijn voor versie *specifiedVersion* en abonnements-ID *yourSubscriptionId*.  
* **Risicobeperking**: Kies een versie die beschikbaar is en probeer het opnieuw.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Beschrijving**: Versie *specifiedVersion* niet ondersteund.
* **Risicobeperking**: Kies een versie die wordt ondersteund en probeer het opnieuw.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Beschrijving**: Versie *specifiedVersion* is niet beschikbaar in Azure-regio *specifiedRegion*.  
* **Risicobeperking**: Kies een versie die wordt ondersteund in de regio die is opgegeven en probeer het opnieuw.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Beschrijving**: Ongeldige clusterconfiguratie. Vereiste WASB-accountconfiguratie niet gevonden in externe accounts.  
* **Risicobeperking**: Controleer of het account bestaat en correct is opgegeven in configuratie en probeer het opnieuw.

## <a name="next-steps"></a>Volgende stappen

* [Apache Ambari-weergaven gebruiken om op te sporen Apache Tez-taken in HDInsight](../hdinsight-debug-ambari-tez-view.md)
* [Heapdumps voor Apache Hadoop-services op Linux gebaseerde HDInsight inschakelen](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)
* [HDInsight-clusters beheren met behulp van de Apache Ambari-webinterface](../hdinsight-hadoop-manage-ambari.md)

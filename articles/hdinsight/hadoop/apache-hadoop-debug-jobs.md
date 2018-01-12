---
title: 'Fouten opsporen in Hadoop in HDInsight: logboekbestanden weergeven en foutberichten - Azure interpreteren | Microsoft Docs'
description: Meer informatie over de foutberichten die kan worden weergegeven bij het beheren van HDInsight met behulp van PowerShell en de stappen die u kunt uitvoeren om te herstellen.
services: hdinsight
tags: azure-portal
editor: cgronlun
manager: jhubbard
author: ashishthaps
documentationcenter: 
ms.assetid: 7e6ceb0e-8be8-4911-bc80-20714030a3ad
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: ashish
ms.openlocfilehash: 682b73aefff2ac20cbd38f6780b73cde859378ed
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="analyze-hadoop-logs"></a>Hadoop-logboeken analyseren

Elke Hadoop-cluster in Azure HDInsight is een Azure storage-account gebruikt als het standaardbestandssysteem. Het opslagaccount wordt verwezen als het standaardopslagaccount. Cluster maakt gebruik van de Azure Table storage en de Blob-opslag op het standaardopslagaccount voor het opslaan van de logboeken.  Zie voor meer informatie over het standaardopslagaccount voor uw cluster, [beheren Hadoop-clusters in HDInsight](../hdinsight-administer-use-management-portal.md#find-the-default-storage-account). De logboeken behouden in het opslagaccount, zelfs nadat het cluster wordt verwijderd.

## <a name="logs-written-to-azure-tables"></a>Logboeken geschreven naar de Azure-tabellen

De logboeken geschreven naar de Azure-tabellen bieden Eén niveau van inzicht in wat er met een HDInsight-cluster gebeurt.

Wanneer u een HDInsight-cluster maakt, worden zes tabellen voor automatisch gemaakt op basis van Linux-clusters in de tabel standaard opslag:

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
* eventTimestamp
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
Power Query kan worden geïnstalleerd vanaf [Microsoft Power Query voor Excel](http://www.microsoft.com/en-us/download/details.aspx?id=39379). Zie de downloadpagina voor de systeemvereisten.

**Gebruik Power Query om te openen en analyseren van het serviceaccount voor aanmelden**

1. Open **Microsoft Excel**.
2. Van de **Power Query** menu, klikt u op **van Azure**, en klik vervolgens op **van Microsoft Azure Table storage**.
   
    ![HDInsight Hadoop Excel PowerQuery openen Azure-tabelopslag](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-open.png)
3. Voer de naam van het opslagaccount, de korte naam of de FQDN-naam.
4. Voer de sleutel van het opslagaccount. U ziet een lijst met tabellen:
   
    ![HDInsight Hadoop-logboeken die zijn opgeslagen in Azure Table storage](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-table-names.png)
5. Met de rechtermuisknop op de tabel hadoopservicelog in de **Navigator** deelvenster en selecteer **bewerken**. U ziet vier kolommen. Verwijder desgewenst, de **partitiesleutel**, **rijsleutel**, en **tijdstempel** kolommen te selecteren en vervolgens op **kolommen verwijderen** uit de opties in het lint.
6. Klik op het uitvouwpictogram op de inhoud kolom kiezen van de kolommen die u wilt importeren in het Excel-werkblad. Voor deze demonstratie gekozen TraceLevel en ComponentName: het mij basisinformatie waarop onderdelen problemen had kan geven.
   
    ![HDInsight Hadoop-logboeken kiezen kolommen](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-using-excel-power-query-filter.png)
7. Klik op **OK** om de gegevens te importeren.
8. Selecteer de **TraceLevel**, rol, en **ComponentName** kolommen en klik vervolgens op **Group By** besturingselement in het lint.
9. Klik op **OK** in het dialoogvenster Groeperen op
10. Klik op ** toepassen & sluiten **.

U kunt nu Excel gebruiken om te filteren en sorteren indien nodig. U kunt andere kolommen (zoals bericht) als u wilt inzoomen op problemen wanneer ze worden uitgevoerd, maar selecteren en groeperen van de kolommen die hierboven worden beschreven, een goede beeld biedt van wat er met Hadoop-services gebeurt omvatten. De dezelfde idee kan worden toegepast op het bestand en hadoopinstalllog tabellen.

#### <a name="use-visual-studio"></a>Visual Studio gebruiken
**Visual Studio gebruiken**

1. Open Visual Studio.
2. Van de **weergave** menu, klikt u op **Cloud Explorer**. Of klik op **CTRL +\, CTRL + X**.
3. Van **Cloud Explorer**, selecteer **brontypen**.  De beschikbare optie **resourcegroepen**.
4. Vouw **Opslagaccounts**, het standaardopslagaccount voor uw cluster en vervolgens **tabellen**.
5. Dubbelklik op **hadoopservicelog**.
6. Een filter toevoegen. Bijvoorbeeld:
   
        TraceLevel eq 'ERROR'
   
    ![HDInsight Hadoop-logboeken kiezen kolommen](./media/apache-hadoop-debug-jobs/hdinsight-hadoop-analyze-logs-visual-studio-filter.png)
   
    Zie voor meer informatie over het maken van filters [filtertekenreeksen maken voor de ontwerpfunctie voor tabellen](../../vs-azure-tools-table-designer-construct-filter-strings.md).

## <a name="logs-written-to-azure-blob-storage"></a>Logboeken geschreven naar Azure Blob Storage
[De logboeken geschreven naar de Azure-tabellen](#log-written-to-azure-tables) één niveau van inzicht in wat er met een HDInsight-cluster gebeurt. Deze tabellen bieden echter geen in taak niveau Logboeken, dit kunnen nuttig zijn bij het analyseren dieper in problemen wanneer deze optreden. HDInsight-clusters zijn voor deze volgende detailniveau, geconfigureerd voor de logboeken van de taak schrijven naar de Blob Storage-account voor elke taak die wordt ingediend via Templeton. Dit betekent vrijwel, taken die worden verzonden met behulp van de Microsoft Azure PowerShell-cmdlets of de .NET-taak verzending van API's, geen taken die worden verzonden via RDP/vanaf de opdrachtregel-line toegang tot het cluster. 

Zie de logboeken bekijken [toepassingslogboeken op Linux gebaseerde HDInsight YARN toegang](../hdinsight-hadoop-access-yarn-app-logs-linux.md).

Zie voor meer informatie over toepassingslogboeken [vereenvoudigen van beheer van de gebruiker-logboeken en toegang in YARN](http://hortonworks.com/blog/simplifying-user-logs-management-and-access-in-yarn/).

## <a name="view-cluster-health-and-job-logs"></a>Bekijk de logboeken van de status en taak cluster
### <a name="access-the-ambari-ui"></a>Toegang tot de Ambari-gebruikersinterface
Klik op de naam van een HDInsight-cluster om de cluster-deelvenster te openen vanuit de Azure-portal. Klik in het deelvenster cluster **Dashboard**.

![Starten van de cluster-dashboard](./media/apache-hadoop-debug-jobs/hdi-debug-launch-dashboard.png)


### <a name="access-the-yarn-ui"></a>Toegang tot de gebruikersinterface van Yarn
Klik op de naam van een HDInsight-cluster om de cluster-deelvenster te openen vanuit de Azure-portal. Klik in het deelvenster cluster **Dashboard**. Voer desgevraagd de referenties van de cluster-beheerder. Selecteer in de Ambari, **YARN** uit de lijst met services aan de linkerkant. Selecteer op de pagina die wordt weergegeven, **snelkoppelingen**, selecteer vervolgens de actieve hoofdknooppunt post en het resourcemanager UI.

De gebruikersinterface van YARN kunt u het volgende doen:

* **Clusterstatus ophalen**. Vouw in het linkerdeelvenster **Cluster**, en klik op **over**. Deze aanwezig cluster statusdetails zoals Totaal toegewezen geheugen, kernen gebruikt, status van de cluster resourcemanager, cluster-versie, enzovoort.
  
    ![Starten van de cluster-dashboard](./media/apache-hadoop-debug-jobs/hdi-debug-yarn-cluster-state.png)
* **Knooppunt status ophalen voor**. Vouw in het linkerdeelvenster **Cluster**, en klik op **knooppunten**. Hier ziet u alle knooppunten in het cluster, HTTP-adres van elk knooppunt resources toegewezen aan elk knooppunt, enzovoort.
* **Taakstatus controleren**. Vouw in het linkerdeelvenster **Cluster**, en klik vervolgens op **toepassingen** voor een lijst met alle taken in het cluster. Als u wilt kijken taken in een specifieke status (zoals nieuwe, verzonden, die wordt uitgevoerd, enzovoort), klikt u op de koppeling onder **toepassingen**. Verder kunt u de naam van de taak voor meer informatie over de taak die is inclusief de uitvoer, Logboeken, enzovoort.

### <a name="access-the-hbase-ui"></a>Toegang tot de HBase-gebruikersinterface
Klik op de naam van een HDInsight HBase-cluster om de cluster-deelvenster te openen vanuit de Azure-portal. Klik in het deelvenster cluster **Dashboard**. Voer desgevraagd de referenties van de cluster-beheerder. Selecteer in Ambari, HBase uit de lijst met services. Selecteer **snelle koppelingen** boven aan de pagina wijst u de koppeling om de actieve Zookeeper knooppunt en klik vervolgens op HBase Master UI.

## <a name="hdinsight-error-codes"></a>HDInsight-foutcodes
De foutberichten die is gespecificeerd in deze sectie vindt u de gebruikers van Hadoop in Azure HDInsight mogelijke fouten die optreden kunnen bij het beheren van de service met Azure PowerShell te begrijpen en ze advies geven over de stappen beschreven die kunnen worden uitgevoerd voor het herstellen van de fout.

Sommige van deze foutberichten kan ook worden gezien in de Azure portal als deze wordt gebruikt voor het beheren van HDInsight-clusters. Andere foutberichten die mogelijk optreden, maar er zijn minder gedetailleerd is als gevolg van de beperkingen op de corrigerende acties mogelijk in deze context. Andere foutberichten zijn in de context waarin de vermindering duidelijk is opgegeven. 

### <a id="AtleastOneSqlMetastoreMustBeProvided"></a>AtleastOneSqlMetastoreMustBeProvided
* **Beschrijving**: Geef informatie over de Azure SQL database voor ten minste één onderdeel om aangepaste instellingen gebruiken voor Hive en Oozie metastores.
* **Risicobeperking**: de gebruiker moet een geldige SQL Azure-metastore leveren en de aanvraag opnieuw proberen.  

### <a id="AzureRegionNotSupported"></a>AzureRegionNotSupported
* **Beschrijving**: kan geen cluster maken in regio *nameOfYourRegion*. Gebruik een geldige HDInsight-regio en de aanvraag opnieuw proberen.
* **Risicobeperking**: klant moet maken van het cluster de regio die momenteel worden ondersteund: Zuidoost-Azië, West-Europa, VS-Oost, Noord-Europa of VS-West.  

### <a id="ClusterContainerRecordNotFound"></a>ClusterContainerRecordNotFound
* **Beschrijving**: de server kan de aangevraagde cluster-record niet vinden.  
* **Risicobeperking**: Voer de bewerking opnieuw uit.

### <a id="ClusterDnsNameInvalidReservedWord"></a>ClusterDnsNameInvalidReservedWord
* **Beschrijving**: Cluster DNS-naam *yourDnsName* is ongeldig. Zorg ervoor dat de naam begint en eindigt op alfanumerieke en mogen alleen '-' speciaal teken  
* **Risicobeperking**: Zorg ervoor dat u hebt een geldige DNS-naam gebruikt voor uw cluster die begint en eindigt op alfanumerieke en bevat geen speciale tekens dan het streepje '-' en probeer het opnieuw.

### <a id="ClusterNameUnavailable"></a>ClusterNameUnavailable
* **Beschrijving**: clusternaam *yourClusterName* is niet beschikbaar. Kies een andere naam.  
* **Risicobeperking**: de gebruiker moet opgeven van een clusternaam die uniek is en niet bestaat en probeer het opnieuw. Als de gebruiker via de Portal, de gebruikersinterface wordt het weten als de naam van een cluster al tijdens de stappen maken gebruikt wordt.

### <a id="ClusterPasswordInvalid"></a>ClusterPasswordInvalid
* **Beschrijving**: Cluster wachtwoord is ongeldig. Wachtwoord moet ten minste 10 tekens lang zijn en moet ten minste één cijfer, hoofdletter, kleine letter en speciale tekens zonder spaties bevatten en mag niet de gebruikersnaam als onderdeel van deze.  
* **Risicobeperking**: Geef een geldige clusternaam-wachtwoord op en probeer het opnieuw.

### <a id="ClusterUserNameInvalid"></a>ClusterUserNameInvalid
* **Beschrijving**: Cluster gebruikersnaam is ongeldig. Zorg ervoor dat er geen gebruikersnaam bevat speciale tekens of spaties.  
* **Risicobeperking**: Geef een geldige clusternaam-gebruikersnaam en probeer het opnieuw.

### <a id="ClusterUserNameInvalidReservedWord"></a>ClusterUserNameInvalidReservedWord
* **Beschrijving**: Cluster DNS-naam *yourDnsClusterName* is ongeldig. Zorg ervoor dat de naam begint en eindigt op alfanumerieke en mogen alleen '-' speciaal teken  
* **Risicobeperking**: Geef een geldige gebruikersnaam van de DNS-cluster en probeer het opnieuw.

### <a id="ContainerNameMisMatchWithDnsName"></a>ContainerNameMisMatchWithDnsName
* **Beschrijving**: containernaam in URI *yourcontainerURI* en DNS-naam *yourDnsName* in de aanvraag hoofdtekst moet hetzelfde zijn.  
* **Risicobeperking**: Zorg ervoor dat de naam van de container en de DNS-naam hetzelfde zijn, en probeer het opnieuw.

### <a id="DataNodeDefinitionNotFound"></a>DataNodeDefinitionNotFound
* **Beschrijving**: ongeldige clusterconfiguratie. Kan knooppunt gegevensdefinities in knooppuntgrootte vinden.  
* **Risicobeperking**: Voer de bewerking opnieuw uit.

### <a id="DeploymentDeletionFailure"></a>DeploymentDeletionFailure
* **Beschrijving**: verwijderen van de implementatie is mislukt voor het Cluster  
* **Risicobeperking**: Voer de verwijderbewerking opnieuw uit.

### <a id="DnsMappingNotFound"></a>DnsMappingNotFound
* **Beschrijving**: Service is een configuratiefout. Vereiste informatie in DNS-toewijzing niet vinden.  
* **Risicobeperking**: cluster verwijderen en een nieuw cluster maken.

### <a id="DuplicateClusterContainerRequest"></a>DuplicateClusterContainerRequest
* **Beschrijving**: dubbele cluster container maken poging. Record bestaat voor *nameOfYourContainer* maar Etags komen niet overeen.
* **Risicobeperking**: Geef een unieke naam voor de container op en probeer het opnieuw maken.

### <a id="DuplicateClusterInHostedService"></a>DuplicateClusterInHostedService
* **Beschrijving**: de gehoste service *nameOfYourHostedService* bevat al een cluster. Een gehoste service kan niet meerdere clusters bevatten  
* **Risicobeperking**: het cluster in een andere gehoste service Host.

### <a id="FailureToUpdateDeploymentStatus"></a>FailureToUpdateDeploymentStatus
* **Beschrijving**: de server kan de status van de implementatie van het cluster niet bijwerken.  
* **Risicobeperking**: Voer de bewerking opnieuw uit. Als dit herhaaldelijk gebeurt, moet u contact op met CSS.

### <a id="HdiRestoreClusterAltered"></a>HdiRestoreClusterAltered
* **Beschrijving**: Cluster *yourClusterName* is verwijderd als onderdeel van onderhoudsmodus. Neem Maak het cluster opnieuw.
* **Risicobeperking**: Maak het cluster opnieuw.

### <a id="HeadNodeConfigNotFound"></a>HeadNodeConfigNotFound
* **Beschrijving**: ongeldige clusterconfiguratie. Hoofdknooppunt configuratie is niet gevonden in knooppuntgrootten vereist.
* **Risicobeperking**: Voer de bewerking opnieuw uit.

### <a id="HostedServiceCreationFailure"></a>HostedServiceCreationFailure
* **Beschrijving**: kan niet worden gemaakt van gehoste service *nameOfYourHostedService*. Probeer de aanvraag.  
* **Risicobeperking**: de aanvraag opnieuw proberen.

### <a id="HostedServiceHasProductionDeployment"></a>HostedServiceHasProductionDeployment
* **Beschrijving**: gehoste Service *nameOfYourHostedService* heeft al een productie-implementatie. Een gehoste service kan niet meerdere productie-implementaties bevatten. Probeer de aanvraag met een andere clusternaam op.
* **Risicobeperking**: gebruik van een andere clusternaam op en probeer de aanvraag.

### <a id="HostedServiceNotFound"></a>HostedServiceNotFound
* **Beschrijving**: gehoste Service *nameOfYourHostedService* voor het cluster is niet gevonden.  
* **Risicobeperking**: als het cluster foutstatus is, verwijderen en probeer het opnieuw.

### <a id="HostedServiceWithNoDeployment"></a>HostedServiceWithNoDeployment
* **Beschrijving**: gehoste Service *nameOfYourHostedService* heeft geen gekoppelde implementatie.  
* **Risicobeperking**: als het cluster foutstatus is, verwijderen en probeer het opnieuw.

### <a id="InsufficientResourcesCores"></a>InsufficientResourcesCores
* **Beschrijving**: de abonnements-id *yourSubscriptionId* heeft geen kernen links cluster maken *yourClusterName*. Vereist: *resourcesRequired*, beschikbaar: *resourcesAvailable*.  
* **Risicobeperking**: resources in uw abonnement vrij te maken of de beschikbare bronnen voor het abonnement te verhogen en probeer het opnieuw maken van het cluster.

### <a id="InsufficientResourcesHostedServices"></a>InsufficientResourcesHostedServices
* **Beschrijving**: abonnements-ID *yourSubscriptionId* heeft geen quota voor een nieuwe HostedService cluster maken *yourClusterName*.  
* **Risicobeperking**: resources in uw abonnement vrij te maken of de beschikbare bronnen voor het abonnement te verhogen en probeer het opnieuw maken van het cluster.

### <a id="InternalErrorRetryRequest"></a>InternalErrorRetryRequest
* **Beschrijving**: de server heeft een interne fout. Probeer de aanvraag.  
* **Risicobeperking**: de aanvraag opnieuw proberen.

### <a id="InvalidAzureStorageLocation"></a>InvalidAzureStorageLocation
* **Beschrijving**: Azure-opslaglocatie *dataRegionName* is geen geldige locatie. Controleer of dat de regio juist is en de aanvraag opnieuw proberen.
* **Risicobeperking**: Selecteer een opslaglocatie die ondersteuning biedt voor HDInsight, Controleer of het cluster geplaatst is en probeer het opnieuw.

### <a id="InvalidNodeSizeForDataNode"></a>InvalidNodeSizeForDataNode
* **Beschrijving**: ongeldige VM-grootte voor gegevensknooppunten. Grootte van de grote virtuele machine alleen wordt ondersteund voor alle gegevensknooppunten.  
* **Risicobeperking**: Geef de knooppuntgrootte van de ondersteunde voor het knooppunt data op en probeer het opnieuw.

### <a id="InvalidNodeSizeForHeadNode"></a>InvalidNodeSizeForHeadNode
* **Beschrijving**: ongeldige VM-grootte voor het hoofdknooppunt. Alleen de grootte 'Zijn VM' wordt ondersteund voor het hoofdknooppunt.  
* **Risicobeperking**: Geef de knooppuntgrootte van de ondersteunde voor het hoofdknooppunt en probeer het opnieuw

### <a id="InvalidRightsForDeploymentDeletion"></a>InvalidRightsForDeploymentDeletion
* **Beschrijving**: abonnements-ID *yourSubscriptionId* wordt gebruikt heeft niet voldoende machtigingen om uit te voeren delete-bewerking voor cluster *yourClusterName*.  
* **Risicobeperking**: als het cluster zich in de foutstatus, verwijdert u deze en probeer het opnieuw.  

### <a id="InvalidStorageAccountBlobContainerName"></a>InvalidStorageAccountBlobContainerName
* **Beschrijving**: externe blob-container opslagaccountnaam *yourContainerName* is ongeldig. Zorg ervoor dat de naam begint met een letter en bevat alleen kleine letters, cijfers en streepjes.  
* **Risicobeperking**: Geef een geldige blob-container opslagaccountnaam en probeer het opnieuw.

### <a id="InvalidStorageAccountConfigurationSecretKey"></a>InvalidStorageAccountConfigurationSecretKey
* **Beschrijving**: configuratie voor externe opslagaccount *yourStorageAccountName* is vereist om details van geheime sleutel moet worden ingesteld.  
* **Risicobeperking**: Geef een geldige geheime sleutel voor het opslagaccount op en probeer het opnieuw.

### <a id="InvalidVersionHeaderFormat"></a>InvalidVersionHeaderFormat
* **Beschrijving**: versiekop *yourVersionHeader* is geen geldige indeling jjjj-mm-dd.  
* **Risicobeperking**: Geef een geldige indeling voor de versiekop en de aanvraag opnieuw proberen.

### <a id="MoreThanOneHeadNode"></a>MoreThanOneHeadNode
* **Beschrijving**: ongeldige clusterconfiguratie. Meer dan één hoofdknooppunt configuratie gevonden.  
* **Risicobeperking**: de configuratie te bewerken, zodat die slechts één hoofdknooppunt is opgegeven.

### <a id="OperationTimedOutRetryRequest"></a>OperationTimedOutRetryRequest
* **Beschrijving**: de bewerking kan niet worden voltooid binnen de toegestane tijd of het maximale aantal nieuwe pogingen mogelijk. Probeer de aanvraag.  
* **Risicobeperking**: de aanvraag opnieuw proberen.

### <a id="ParameterNullOrEmpty"></a>ParameterNullOrEmpty
* **Beschrijving**: Parameter *yourParameterName* mag niet null of leeg.  
* **Risicobeperking**: Geef een geldige waarde voor de parameter.

### <a id="PreClusterCreationValidationFailure"></a>PreClusterCreationValidationFailure
* **Beschrijving**: een of meer van de invoerwaarden cluster maken van het verzoek is niet geldig. Zorg ervoor dat de invoerwaarden juist zijn en de aanvraag opnieuw proberen.  
* **Risicobeperking**: Controleer of de invoerwaarden juist zijn en de aanvraag opnieuw proberen.

### <a id="RegionCapabilityNotAvailable"></a>RegionCapabilityNotAvailable
* **Beschrijving**: regio mogelijkheid is niet beschikbaar voor de regio *yourRegionName* en abonnements-ID *yourSubscriptionId*.  
* **Risicobeperking**: Geef een regio die ondersteuning biedt voor HDInsight-clusters. Het openbaar ondersteunde regio's zijn: Zuidoost-Azië, West-Europa, VS-Oost, Noord-Europa of VS-West.

### <a id="StorageAccountNotColocated"></a>StorageAccountNotColocated
* **Beschrijving**: opslagaccount *yourStorageAccountName* bevindt zich in de regio *currentRegionName*. Dit moet hetzelfde zijn als het cluster gebied *yourClusterRegionName*.  
* **Risicobeperking**: Geef een opslagaccount in dezelfde regio als uw cluster in gebruik is of als uw gegevens al in het opslagaccount is, een nieuw cluster maken in dezelfde regio als het bestaande opslagaccount. Als u de Portal gebruikt, verwittigen de gebruikersinterface voor het van dit probleem van tevoren.

### <a id="SubscriptionIdNotActive"></a>SubscriptionIdNotActive
* **Beschrijving**: opgegeven abonnements-ID *yourSubscriptionId* is niet actief.  
* **Risicobeperking**: uw abonnement opnieuw activeren of een nieuw geldig abonnement ophalen.

### <a id="SubscriptionIdNotFound"></a>SubscriptionIdNotFound
* **Beschrijving**: abonnements-ID *yourSubscriptionId* is niet gevonden.  
* **Risicobeperking**: Controleer of uw abonnements-ID geldig is en probeer het opnieuw.

### <a id="UnableToResolveDNS"></a>UnableToResolveDNS
* **Beschrijving**: kan niet worden omgezet DNS *yourDnsUrl*. Zorg ervoor dat de volledige URL voor het blobeindpunt is opgegeven.  
* **Risicobeperking**: Geef een geldige blob-URL. De URL moet volledig geldig zijn, inclusief beginnen met *http://* en eindigt op *.com*.

### <a id="UnableToVerifyLocationOfResource"></a>UnableToVerifyLocationOfResource
* **Beschrijving**: kan niet verifiëren van de locatie van de resource *yourDnsUrl*. Zorg ervoor dat de volledige URL voor het blobeindpunt is opgegeven.  
* **Risicobeperking**: Geef een geldige blob-URL. De URL moet volledig geldig zijn, inclusief beginnen met *http://* en eindigt op *.com*.

### <a id="VersionCapabilityNotAvailable"></a>VersionCapabilityNotAvailable
* **Beschrijving**: versie mogelijkheid is niet beschikbaar voor versie *specifiedVersion* en abonnements-ID *yourSubscriptionId*.  
* **Risicobeperking**: Kies een versie die beschikbaar is en probeer het opnieuw.

### <a id="VersionNotSupported"></a>VersionNotSupported
* **Beschrijving**: versie *specifiedVersion* niet ondersteund.
* **Risicobeperking**: Kies een versie die wordt ondersteund en probeer het opnieuw.

### <a id="VersionNotSupportedInRegion"></a>VersionNotSupportedInRegion
* **Beschrijving**: versie *specifiedVersion* is niet beschikbaar in Azure-regio *specifiedRegion*.  
* **Risicobeperking**: Kies een versie die wordt ondersteund in de regio is opgegeven en probeer het opnieuw.

### <a id="WasbAccountConfigNotFound"></a>WasbAccountConfigNotFound
* **Beschrijving**: ongeldige clusterconfiguratie. Vereiste WASB-accountconfiguratie niet gevonden in externe accounts.  
* **Risicobeperking**: Controleer of het account bestaat en correct is opgegeven in configuratie en probeer het opnieuw.

## <a name="next-steps"></a>Volgende stappen

* [Ambari-weergaven gebruiken om op te sporen Tez-taken in HDInsight](../hdinsight-debug-ambari-tez-view.md)
* [Dumpbestanden voor Hadoop-services op Linux gebaseerde HDInsight heap inschakelen](../hdinsight-hadoop-collect-debug-heap-dump-linux.md)

<!--
TODO  * [Manage HDInsight clusters by using the Ambari Web UI](hdinsight-hadoop-manage-ambari.md)
-->

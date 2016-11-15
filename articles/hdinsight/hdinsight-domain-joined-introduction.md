---
title: Overzicht van Secure HDInsight| Microsoft Docs
description: Meer informatie...
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 10/31/2016
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 40b0d0660f4693d5ebedf847734ffe037c4ec7ca


---
# <a name="an-introduction-to-domainjoined-hdinsight-clusters-preview"></a>An introduction to Domain-joined HDInsight clusters (Preview) (Een introductie tot HDInsight-clusters die zijn gekoppeld aan een domein (preview-versie))
Tot nu toe werd in Azure HDInsight alleen ondersteuning geboden voor één lokale beheerder voor gebruikers. Dit werkte heel goed voor kleinere toepassingsteams of afdelingen. Naarmate Hadoop-workloads populairder werden bij grote bedrijven, werden functies die voor dergelijke bedrijven relevant zijn, zoals AD-verificatie, ondersteuning voor meerdere gebruikers en toegangsbeheer op basis van rollen, steeds belangrijker. Als u gebruikmaakt van aan een domein gekoppelde HDInsight-clusters, kunt u een HDInsight-cluster maken dat is gekoppeld aan een Active Directory-domein, en een lijst met werknemers uit het bedrijf configureren die zich kunnen verifiëren via Azure Active Directory om zich aan te melden bij het HDInsight-cluster. Mensen van buiten het bedrijf kunnen zich niet aanmelden bij het HDInsight-cluster en hebben er geen toegang toe. De bedrijfsbeheerder kan toegangsbeheer op basis van rollen configureren voor Hive-beveiliging via [Apache Ranger](http://hortonworks.com/apache/ranger/). Op deze manier wordt de toegang tot gegevens alleen beperkt voor zover dit noodzakelijk is. Daarnaast kan de beheerder de toegang tot gegevens controleren voor werknemers en zien wanneer er wijzigingen worden aangebracht aan het toegangscontrolebeleid. Dit zorgt voor een hoge mate van beheer van de bedrijfsresources.

> [!NOTE]
> De nieuwe functies die in deze preview worden beschreven, zijn alleen beschikbaar met HDInsight-clusters op basis van Linux voor Hive-workloads. De andere workloads, zoals HBase, Spark, Storm en Kafka, komen beschikbaar in toekomstige releases. 
> 
> 

## <a name="benefits"></a>Voordelen
Bedrijfsbeveiliging bestaat uit vier kernonderdelen: perimeterbeveiliging, verificatie, autorisatie en versleuteling.

![De voordelen van HDInsight-clusters die zijn gekoppeld aan een domein](./media/hdinsight-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

### <a name="perimeter-security"></a>Perimeterbeveiliging
Perimeterbeveiliging in HDInsight wordt bereikt met virtuele netwerken en de Gateway-service. Vanaf nu kan een bedrijfsbeheerder een HDInsight-cluster in een virtueel netwerk maken en netwerkbeveiligingsgroepen (firewallregels voor binnenkomend en uitgaand verkeer) gebruiken om de toegang tot het virtuele netwerk te beperken. Er kan alleen worden gecommuniceerd met het HDInsight-cluster via de IP-adressen die zijn gedefinieerd in de firewallregels voor binnenkomend verkeer. Op deze manier wordt perimeterbeveiliging geboden. Een andere perimeterbeveiligingslaag wordt tot stand gebracht via de Gateway-service. De Gateway is de service die als eerste beveiliging optreedt tijdens alle aanvragen die binnenkomen bij de HDInsight-cluster. Via de service worden aanvragen geaccepteerd en gevalideerd, en vervolgens pas doorgegeven aan de andere knooppunten in het cluster. Op deze manier wordt perimeterbeveiliging gewaarborgd voor andere naam- en gegevensknooppunten in het cluster.

### <a name="authentication"></a>Authentication
Met deze openbare preview-versie kan een bedrijfsbeheerder in een [virtueel netwerk](https://azure.microsoft.com/services/virtual-network/) een HDInsight-cluster inrichten dat is toegevoegd aan een domein. De knooppunten van het HDInsight-cluster worden gekoppeld aan het domein dat wordt beheerd via het bedrijf. Dit wordt bewerkstelligd via [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Alle knooppunten in het cluster zijn gekoppeld aan een domein dat wordt beheerd via het bedrijf. Met deze instelling kunnen de werknemers van het bedrijf zich met hun domeinreferenties aanmelden bij de clusterknooppunten. Ze kunnen hun domeinreferenties ook gebruiken om zich te verifiëren bij andere goedgekeurde eindpunten, zoals Hue, Ambari Views, ODBC, JDBC, PowerShell en REST API's, om met het cluster te communiceren. De beheerder heeft de volledige controle over het beperken van het aantal gebruikers dat via deze eindpunten met het cluster communiceert.

### <a name="authorization"></a>Autorisatie
De meeste bedrijven zorgen ervoor dat niet alle werknemers toegang hebben tot alle bedrijfsresources. Sinds deze release kan de beheerder ook toegangscontrolebeleid op basis van rollen definiëren voor de clusterresources. De beheerder kan bijvoorbeeld [Apache Ranger](http://hortonworks.com/apache/ranger/) configureren om toegangscontrolebeleid in te stellen voor Hive. Deze functionaliteit zorgt ervoor dat werknemers alleen toegang hebben tot de gegevens die ze nodig hebben om hun werk goed te kunnen uitvoeren. SSH-toegang tot het cluster is ook beperkt tot alleen de beheerder.

### <a name="auditing"></a>Controleren
Het controleren van alle clusterresources en de gegevens is niet alleen nodig om de HDInsight-clusterresources te beveiligen tegen onbevoegde gebruikers en om de gegevens te beveiligen, maar ook om onbevoegde of onbedoelde toegang tot de resources bij te houden. Met behulp van deze preview-versie kan de beheerder alle toegang tot de HDInsight-clusterresources en gegevens bekijken en rapporteren. De beheerder kan ook alle wijzigingen in het toegangscontrolebeleid die worden aangebracht in met Apache Ranger ondersteunde eindpunten, bekijken en rapporteren. Een aan een domein gekoppeld HDInsight-cluster maakt gebruik van de vertrouwde Apache Ranger-gebruikersinterface om naar controlelogboeken te zoeken. Ranger gebruik [Apache Solr](http://hortonworks.com/apache/solr/) in de back-end voor het opslaan van en zoeken naar logboeken.

### <a name="encryption"></a>Versleuteling
Het beveiligen van gegevens is belangrijk om te voldoen aan de vereisten voor organisatiebeveiliging en -compliance. Daarom moeten deze gegevens, naast het beperken van toegang tot gegevens voor onbevoegde werknemers, ook worden beveiligd via versleuteling. De twee opties voor gegevensopslag voor HDInsight-clusters, Azure Storage-blobopslag en Azure Data Lake-opslag, bieden ondersteuning voor transparante [versleuteling van data-at-rest](../storage/storage-service-encryption.md) aan de serverzijde. Beveiligde HDInsight-clusters werken naadloos samen met deze mogelijkheid tot versleuteling van data-at-rest aan de serverzijde.

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Aan een domein gekoppelde HDInsight-clusters configureren) om een HDInsight-cluster te configureren dat is gekoppeld aan een domein.
* Zie [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Aan een domein gekoppelde HDInsight-clusters beheren) om HDInsight-clusters te beheren die zijn gekoppeld aan een domein.
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie [Use SSH with Linux-based Hadoop on HDInsight from Linux, Unix, or OS X](hdinsight-hadoop-linux-use-ssh-unix.md#connect-to-a-domain-joined-hdinsight-cluster) (SSH met Hadoop op basis van Linux gebruiken op HDInsight in Linux, Unix of OS X) om Hive-query's uit te voeren met behulp van SSH op HDInsight-clusters die zijn gekoppeld aan een domein.




<!--HONumber=Nov16_HO2-->



---
title: Een inleiding tot Hadoop-beveiliging met Azure HDInsight-clusters domein
description: Informatie over hoe een domein-Azure HDInsight-clusters bieden ondersteuning voor de vier pijlers van beveiliging voor bedrijven.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 06/26/2018
ms.openlocfilehash: c13fd979562cc89831d031c5050fe9dbb184267b
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43041129"
---
# <a name="an-introduction-to-hadoop-security-with-domain-joined-hdinsight-clusters"></a>Een inleiding tot Hadoop-beveiliging met aan domein gekoppelde HDInsight-clusters

In het verleden, Azure HDInsight slechts één gebruiker ondersteund: lokale beheerder. Dit werkte heel goed voor kleinere toepassingsteams of afdelingen. Als de Hadoop-gebaseerde workloads opgedaan populairder in de enterprise-sector, de noodzaak voor ondersteuning van geavanceerde mogelijkheden, zoals verificatie op basis van Active Directory, met meerdere gebruikers, en op rollen gebaseerd toegangsbeheer steeds belangrijker. 

U kunt een HDInsight-cluster dat gekoppeld aan een Active Directory-domein maken. U kunt vervolgens een lijst met werknemers van de onderneming die zich via Azure Active Directory verifiëren kunnen voor aanmelding bij het HDInsight-cluster configureren. Iemand buiten de onderneming kan niet aanmelden of toegang tot het HDInsight-cluster. 

De enterprise-beheerder op rollen gebaseerd toegangsbeheer (RBAC) kunt configureren voor Hive-beveiliging met behulp van [Apache Ranger](http://hortonworks.com/apache/ranger/). RBAC configureren toegang wordt beperkt tot gegevens alleen wat u nodig hebt. De beheerder kan ten slotte de toegang tot de gegevens door werknemers en eventuele wijzigingen worden aangebracht aan het beleid voor toegangsbeheer controleren. De beheerder kan vervolgens een hoge mate van beheer van hun bedrijfsbronnen behalen.

> [!NOTE]
> De nieuwe functies die beschreven worden in dit artikel zijn beschikbaar in Preview-versie van alleen de volgende clustertypen: Hadoop, Spark en interactieve query's. Oozie is nu ingeschakeld in aan domein gekoppelde clusters. Voor toegang tot de Oozie-webgebruikersinterface, gebruikers moeten inschakelen [tunneling](../hdinsight-linux-ambari-ssh-tunnel.md).

Bedrijfsbeveiliging bestaat uit vier belangrijke onderdelen: perimeterbeveiliging, verificatie, autorisatie en versleuteling.

![Voordelen van aan domein gekoppelde HDInsight-clusters in de vier pijlers van beveiliging voor bedrijven](./media/apache-domain-joined-introduction/hdinsight-domain-joined-four-pillars.png).

## <a name="perimeter-security"></a>Perimeterbeveiliging
Perimeterbeveiliging in HDInsight wordt bereikt door middel van virtuele netwerken en de Azure VPN-Gateway-service. Een enterprise-beheerder kan een HDInsight-cluster in een virtueel netwerk maken en gebruiken van netwerkbeveiligingsgroepen (firewallregels) toegang te beperken tot het virtuele netwerk. Alleen de IP-adressen die zijn gedefinieerd in de firewallregels voor binnenkomend verkeer is mogelijk om te communiceren met het HDInsight-cluster. Deze configuratie biedt perimeterbeveiliging.

Een andere perimeterbeveiligingslaag wordt bereikt via de VPN-Gateway-service. De gateway fungeert als eerste verdedigingslinie voor elke inkomende aanvraag voor het HDInsight-cluster. Deze worden aanvragen geaccepteerd, wordt deze gevalideerd en alleen vervolgens kan de aanvraag worden doorgegeven aan de andere knooppunten in het cluster. De gateway biedt op deze manier perimeterbeveiliging naar een andere naam- en gegevensknooppunten in het cluster.

## <a name="authentication"></a>Verificatie
Een enterprise-beheerder kan maken met een HDInsight-cluster domein in een [virtueel netwerk](https://azure.microsoft.com/services/virtual-network/). Alle knooppunten van het HDInsight-cluster zijn gekoppeld aan het domein dat wordt beheerd via het bedrijf. Dit wordt bereikt met behulp van [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md). 

Met deze instelling kunnen enterprise werknemers aanmelden bij de clusterknooppunten met behulp van hun domeinreferenties. Ze kunnen hun domeinreferenties ook gebruiken om te verifiëren bij andere goedgekeurde eindpunten, zoals Ambari-weergaven, ODBC, JDBC, PowerShell en REST-API's om te communiceren met het cluster. De beheerder heeft volledige controle over het beperken van het aantal gebruikers die met het cluster via deze eindpunten werken.

## <a name="authorization"></a>Autorisatie
Een best practice die volgen op de meeste bedrijven is ervoor te zorgen dat niet alle werknemers toegang tot alle bedrijfsresources heeft. De beheerder kan ook op rollen gebaseerd beleid voor toegangsbeheer voor de clusterresources definiëren. 

De beheerder kan bijvoorbeeld [Apache Ranger](http://hortonworks.com/apache/ranger/) configureren om toegangscontrolebeleid in te stellen voor Hive. Deze functionaliteit zorgt ervoor dat werknemers toegang heeft tot alleen gegevens die ze nodig hebben om succesvol in hun werk te zijn. SSH-toegang tot het cluster is ook beperkt tot alleen de beheerder.

## <a name="auditing"></a>Controleren
Controle van alle toegang tot de resources van het cluster en de gegevens is die nodig zijn om bij te houden van onbevoegde of onbedoelde toegang van de resources. Het is net zo belangrijk als de HDInsight-clusterresources te beveiligen tegen onbevoegde gebruikers en de gegevens te beveiligen. 

De beheerder kan weergeven en rapporteren van alle toegang tot het HDInsight-clusterresources en gegevens. De beheerder kan ook weergeven en rapporteren van alle wijzigingen aan het beleid voor toegangsbeheer in Apache Ranger ondersteunde eindpunten hebt gemaakt. 

Een domein gekoppeld HDInsight-cluster maakt gebruik van de vertrouwde Apache Ranger-gebruikersinterface om te zoeken naar de logboeken voor controle. Op de back-end, Ranger maakt gebruik van [Apache Solr](http://hortonworks.com/apache/solr/) voor het opslaan en zoeken naar Logboeken.

## <a name="encryption"></a>Versleuteling
Beveiligen van gegevens is belangrijk voor vergadering beveiligings- en vereisten van de organisatie. Naast het beperken van toegang tot gegevens onbevoegde werknemers, moet u het versleutelen. 

Beide voor gegevensopslag voor HDInsight-clusters, Azure Blob storage en Azure Data Lake Storage Gen1--ondersteuning voor transparante serverzijde [versleuteling van gegevens](../../storage/common/storage-service-encryption.md) at-rest. Beveiligde HDInsight-clusters werken naadloos samen met deze mogelijkheid van server-side-versleuteling van data-at-rest.

## <a name="next-steps"></a>Volgende stappen
* [Plan voor aan domein gekoppelde HDInsight-clusters](apache-domain-joined-architecture.md)
* [Aan domein gekoppelde HDInsight-clusters configureren](apache-domain-joined-configure.md)
* [Aan domein gekoppelde HDInsight-clusters beheren](apache-domain-joined-manage.md)
* [Hive-beleidsregels voor aan domein gekoppelde HDInsight-clusters configureren](apache-domain-joined-run-hive.md)
* [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined)


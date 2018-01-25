---
title: Domein Azure HDInsight-architectuur | Microsoft Docs
description: Meer informatie over het plannen van aan domein gekoppelde HDInsight.
services: hdinsight
documentationcenter: 
author: saurinsh
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/14/2017
ms.author: saurinsh
ms.openlocfilehash: c5e6381e7abb18c01031e4168c64cfe9aafec7ae
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Azure Hadoop-clusters in aan domein gekoppelde HDInsight plannen

Het standaard HDInsight-cluster is een cluster met één gebruiker. Dit is geschikt voor de meeste bedrijven met kleinere toepassingsteams die workloads met veel gegevens maken. Als Hadoop ervaring populariteit, gestart vele ondernemingen gericht op een model waarin clusters worden beheerd door IT-teams en toepassing van meerdere teams share clusters. Daarom zijn functies met clusters voor meerdere gebruikers onder de meest gevraagde functies in Azure HDInsight.

In plaats van het bouwen van een eigen voor meerdere gebruikers verificatie en autorisatie is HDInsight is afhankelijk van de meest populaire id-provider--Active Directory (AD). De krachtige beveiligingsfuncties in AD kan worden gebruikt voor het beheren van meerdere gebruikers verificatie in HDInsight. Door te integreren HDInsight met AD, kunt u communiceren met de clusters met behulp van uw AD-referenties. De virtuele machines in HDInsight zijn domein is gekoppeld aan uw AD en die is hoe HDInsight wijst een AD-gebruiker toe aan een lokale gebruiker voor Hadoop, zodat alle services die worden uitgevoerd op HDInsight (Ambari, Hive-server, Zwerver, Spark thrift-server en andere) naadloos voor de geverifieerde gebruiker. Beheerders kunnen vervolgens sterk autorisatiebeleid met Apache Zwerver voor op rollen gebaseerde toegangsbeheer voor bronnen in HDInsight maken.


## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight integreren met Active Directory

Bij het integreren van HDInsight met Active Directory, zijn de clusterknooppunten HDInsight domein is gekoppeld aan een AD-domein. Kerberos-beveiliging is geconfigureerd voor het Hadoop-onderdelen op het cluster. Voor elk van de Hadoop-onderdelen, een service-principal gemaakt op de Active Directory. Een overeenkomstige machine principal wordt ook gemaakt voor elke computer die is gekoppeld aan het domein. Deze service-principals en machine principals kunnen bruikbaar blijft uw Active Directory. Als gevolg hiervan is het vereist om een organisatie-eenheid (OE) in de Active Directory, waar deze principals zijn geplaatst. 

Om samen te vatten, moet u een omgeving met instellen:

- Een Active Directory-domeincontroller met LDAPS geconfigureerd.
- De verbinding van het virtuele netwerk van HDInsight met uw Active Directory-domeincontroller.
- Een organisatie-eenheid op Active Directory gemaakt.
- Een serviceaccount met machtigingen voor:

    - Service-principals in de organisatie-eenheid maken.
    - Computers aan domein toevoegen en machine principals in de organisatie-eenheid maken.

De volgende schermafbeelding ziet een organisatie-eenheid gemaakt in contoso.com. Sommige van de service-principals en machine principals worden weergegeven in de schermafbeelding ook.

![Domein die lid zijn van HDInsight-clusters organisatie-eenheid](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="two-ways-of-bringing-your-own-active-directory-domain-controllers"></a>Twee manieren om uw eigen Active Directory-domeincontrollers

Er zijn twee manieren waarop u Active Directory-domeincontrollers voor het maken van domein HDInsight-clusters kunt brengen. 

- **Azure Active Directory Domain Services**: deze service biedt een beheerd domein van het Active Directory, die volledig compatibel met Windows Server Active Directory is. Microsoft zorgt voor beheer, patchen en de bewaking van het AD-domein. U kunt uw cluster implementeren zonder dat u over het beheren van domeincontrollers. Gebruikers, groepen en wachtwoorden worden gesynchroniseerd vanuit uw Azure Active Directory, zodat gebruikers aan te melden bij het cluster met hun bedrijfsreferenties. Zie voor meer informatie [configureren domein HDInsight-clusters met behulp van Azure Active Directory Domain Services](./apache-domain-joined-configure-using-azure-adds.md).

- **Active Directory op Azure IaaS VM's**: In deze optie kunt u implementeren en beheren van uw eigen Windows Server Active Directory-domein op Azure IaaS VM's. Zie voor meer informatie [configureren domein gekoppelde sandbox-omgeving](./apache-domain-joined-configure.md).

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Domain-joined HDInsight clusters](apache-domain-joined-configure.md) (Aan een domein gekoppelde HDInsight-clusters configureren) om een HDInsight-cluster te configureren dat is gekoppeld aan een domein.
* Zie [Manage Domain-joined HDInsight clusters](apache-domain-joined-manage.md) (Aan een domein gekoppelde HDInsight-clusters beheren) om HDInsight-clusters te beheren die zijn gekoppeld aan een domein.
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor informatie over het uitvoeren van Hive-query's met behulp van SSH op HDInsight-clusters domein [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

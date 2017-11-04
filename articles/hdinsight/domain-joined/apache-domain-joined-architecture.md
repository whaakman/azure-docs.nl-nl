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
ms.date: 09/21/2017
ms.author: saurinsh
ms.openlocfilehash: 9deb5e4dbd925c4fdc523d8d21afbcfbf85947b8
ms.sourcegitcommit: f8437edf5de144b40aed00af5c52a20e35d10ba1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/03/2017
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Azure Hadoop-clusters in aan domein gekoppelde HDInsight plannen

Het traditionele Hadoop is een cluster met één gebruiker. Dit is geschikt voor de meeste bedrijven met kleinere toepassingsteams die workloads met veel gegevens maken. Naarmate Hadoop populairder wordt, stappen veel bedrijven over op een model waarin clusters worden beheerd door IT-teams en meerdere toepassingsteams clusters delen. Daarom zijn functies met clusters voor meerdere gebruikers onder de meest gevraagde functies in Azure HDInsight.

In plaats van het bouwen van een eigen voor meerdere gebruikers verificatie en autorisatie is HDInsight is afhankelijk van de meest populaire id-provider--Active Directory (AD). De krachtige beveiligingsfuncties in AD kan worden gebruikt voor het beheren van meerdere gebruikers autorisatie in HDInsight. Door te integreren HDInsight met AD, kunt u communiceren met de clusters met behulp van uw AD-referenties. Een AD-gebruiker wordt HDInsight toegewezen aan een gebruiker met lokale Hadoop, zodat alle services die worden uitgevoerd op HDInsight (Ambari, Hive-server, Zwerver, Spark thrift-server en andere) naadloos voor de geverifieerde gebruiker.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight integreren met Active Directory

Wanneer u HDInsight met Active Directory integreert, zijn de clusterknooppunten HDInsight domein is gekoppeld aan een AD-domein. HDInsight maakt service-principals voor het Hadoop-services uitgevoerd op het cluster en worden ze binnen een opgegeven organisatie-eenheid (OE) in het domein. HDInsight maakt ook omgekeerde DNS-toewijzingen in het domein voor de IP-adressen van de knooppunten die zijn gekoppeld aan het domein.

Er zijn twee implementatieopties voor Active Directory:
* **[Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md):** deze service biedt een beheerde Active Directory-domein dat volledig compatibel met Windows Server Active Directory is. Microsoft zorgt voor beheer, patchen en de bewaking van het AD-domein. U kunt uw cluster implementeren zonder dat u over het beheren van domeincontrollers. Gebruikers, groepen en wachtwoorden worden gesynchroniseerd vanuit uw Azure Active Directory, zodat gebruikers aan te melden bij het cluster met hun bedrijfsreferenties.

* **Windows Server Active Directory-domein op Azure IaaS VM's:** In deze optie kunt u implementeren en beheren van uw eigen Windows Server Active Directory-domein op Azure IaaS VM's. 

U kunt deze configuratie bereiken met behulp van verschillende architecturen. U kunt kiezen uit de volgende architecturen.


### <a name="hdinsight-integrated-with-an-azure-ad-domain-services-managed-ad-domain"></a>HDInsight is geïntegreerd met een Azure AD Domain Services beheerd AD-domein
U kunt een [Azure Active Directory Domain Services](../../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS) beheerd domein. Azure AD DS biedt een beheerd domein AD in Azure, dit wordt beheerd, bijgewerkt en bewaakt door Microsoft. Deze maakt u twee domeincontrollers voor hoge beschikbaarheid en DNS-services bevat. U kunt vervolgens het HDInsight-cluster integreren met dit beheerde domein. Met deze Implementatieoptie hoeft u niet hoeft te beheren, patches, bijwerken en bewaken van domeincontrollers.

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Vereisten voor het integreren met Azure AD Domain Services:

* [Inrichten van een Azure AD Domain Services beheerd domein](../../active-directory-domain-services/active-directory-ds-getting-started.md).
* Maak een [organisatie-eenheid](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)waarbinnen u plaats het HDInsight-cluster virtuele machines en de service-principals die zijn gebruikt door het cluster.
* Setup [LDAPS](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md), wanneer u Azure AD DS configureren. Het certificaat dat wordt gebruikt voor het instellen van LDAPS moet worden uitgegeven door een openbare certificeringsinstantie (niet een zelfondertekend certificaat).
* Omgekeerde DNS-zones maken voor het beheerde domein voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* Configureer [synchronisatie van de wachtwoordhashes voor NTLM en Kerberos-verificatie](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) van Azure AD aan het beheerde domein van Azure AD DS.
* Een service-account of gebruikersaccount is vereist. Gebruik dit account om de HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Azure AD-domein


### <a name="hdinsight-integrated-with-windows-server-ad-running-on-azure-iaas"></a>HDInsight is geïntegreerd met Windows Server AD uitgevoerd op Azure IaaS

U kunt de Windows Server Active Directory Domain Services-rol op een (of meerdere) virtuele machines (VM's) in Azure implementeren en promoveren domeincontrollers. Deze virtuele machines van de domeincontroller kan worden geïmplementeerd met behulp van het implementatiemodel van resource manager in hetzelfde virtuele netwerk als het HDInsight-cluster. Als de domeincontrollers zijn geïmplementeerd in een ander virtueel netwerk, moet u deze virtuele netwerken met behulp van peer [VNet-naar-VNet-peering](../../virtual-network/virtual-network-create-peering.md). 

[Meer informatie - implementatie van Windows Server Active Directory op Azure Virtual machines](../../active-directory/virtual-networks-windows-server-active-directory-virtual-machines.md)

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/apache-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> In deze architectuur kunt u Azure Data Lake Store niet gebruiken met HDInsight-cluster.


Vereisten voor de integratie met Windows Server Active Directory op Azure Virtual machines:

* Er moet een [organisatie-eenheid](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de VM's van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* [Lightweight Directory Access protocollen](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPs) moeten worden ingesteld voor communicatie met AD. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* Een service-account of gebruikersaccount is vereist. Gebruik dit account om de HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein


## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Domain-joined HDInsight clusters](apache-domain-joined-configure.md) (Aan een domein gekoppelde HDInsight-clusters configureren) om een HDInsight-cluster te configureren dat is gekoppeld aan een domein.
* Zie [Manage Domain-joined HDInsight clusters](apache-domain-joined-manage.md) (Aan een domein gekoppelde HDInsight-clusters beheren) om HDInsight-clusters te beheren die zijn gekoppeld aan een domein.
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor informatie over het uitvoeren van Hive-query's met behulp van SSH op HDInsight-clusters domein [SSH gebruiken met HDInsight](../hdinsight-hadoop-linux-use-ssh-unix.md).

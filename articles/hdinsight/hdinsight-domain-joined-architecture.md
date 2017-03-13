---
title: Architectuur van aan domein gekoppelde Azure HDInsight | Microsoft Docs
description: Meer informatie over het plannen van aan domein gekoppelde HDInsight.
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
ms.date: 02/03/2017
ms.author: saurinsh
translationtype: Human Translation
ms.sourcegitcommit: 4ba44128ec19d3937643ac934ca3e787cb9819a3
ms.openlocfilehash: 690ba97d2b0634548a0ec424d441ad34d70667b9
ms.lasthandoff: 02/27/2017


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Azure Hadoop-clusters in aan domein gekoppelde HDInsight plannen

Het traditionele Hadoop is een cluster met één gebruiker. Dit is geschikt voor de meeste bedrijven met kleinere toepassingsteams die workloads met veel gegevens maken. Naarmate Hadoop populairder wordt, stappen veel bedrijven over op een model waarin clusters worden beheerd door IT-teams en meerdere toepassingsteams clusters delen. Daarom zijn functies met clusters voor meerdere gebruikers onder de meest gevraagde functies in Azure HDInsight.

In plaats van een eigen verificatie en autorisatie voor meerdere gebruikers in te stellen, maakt HDInsight gebruik van de populairste id-provider: Azure Active Directory (Azure AD). De krachtige beveiligingsfunctionaliteit in Azure AD kan worden gebruikt om autorisatie voor meerdere gebruikers in HDInsight te beheren. Door HDInsight te integreren met Azure AD, kunt u communiceren met de clusters met behulp van uw Azure AD-referenties. HDInsight wijst een Azure AD-gebruiker toe aan een lokale Hadoop-gebruiker, zodat alle services die op HDInsight worden uitgevoerd (Ambari, Hive-server, Ranger, Spark Thrift-server, enzovoort), naadloos voor de geverifieerde gebruiker werken.

## <a name="integrate-hdinsight-with-azure-ad"></a>HDInsight integreren met Azure AD

Door HDInsight te integreren met Azure AD, worden de HDInsight-clusterknooppunten toegevoegd aan het Azure AD-domein. HDInsight maakt service-principals voor de Hadoop-services die op het cluster worden uitgevoerd en plaatst deze in een opgegeven OU (organisatie-eenheid) van Azure AD. HDInsight maakt ook omgekeerde DNS-toewijzingen in het Azure AD-domein voor de IP-adressen van de knooppunten die lid zijn van het domein.

U kunt deze configuratie bereiken met behulp van verschillende architecturen. U kunt kiezen uit de volgende architecturen.

**HDInsight geïntegreerd met Azure AD dat wordt uitgevoerd op Azure IaaS**

Dit is de eenvoudigste architectuur om HDInsight te integreren met Azure AD. De Azure AD-domeincontroller wordt uitgevoerd op een (of meer) virtuele machines (VM's) in Azure. Deze VM's bevinden zich meestal in een virtueel netwerk. U stelt een ander virtueel netwerk in voor het HDInsight-cluster. Om ervoor te zorgen dat HDInsight zichtbaar is voor Azure AD, moet u deze virtuele netwerken peeren met behulp van [VNet-naar-VNet-peering](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md).

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> In deze architectuur kunt u Azure Data Lake Store niet gebruiken met HDInsight-cluster.


Vereisten voor Azure AD:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de VM's van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* [Lightweight Directory Access Protocols](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) (LDAPS) moeten voor de communicatie met Azure AD worden ingesteld. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* Een service-account of gebruikersaccount is vereist. Gebruik dit account om de HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein

**HDInsight geïntegreerd met een alleen-cloud Azure AD**

Configureer een domeincontroller voor alleen-cloud Azure AD, zodat HDInsight kan worden geïntegreerd met Azure AD. Daarvoor gebruikt u [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Azure AD DS maakt domeincontrollermachines in de cloud en biedt de IP-adressen van deze machines. Voor maximale beschikbaarheid worden er twee domeincontrollers gemaakt.

Azure AD DS bestaat op dit moment alleen in klassieke virtuele netwerken. Het is alleen toegankelijk via de klassieke Azure-portal. Het virtuele netwerk van HDInsight bestaat in Azure Portal, die met behulp van VNet-naar-VNet-peering moet worden gepeerd met het klassieke virtuele netwerk.

> [!NOTE]
> Voor peering tussen een klassiek virtueel netwerk en een virtueel netwerk van Azure Resource Manager moeten beide virtuele netwerken zich in dezelfde regio bevinden en hetzelfde Azure-abonnement hebben.

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Vereisten voor Azure AD:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de VM's van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* Tijdens de configuratie van Azure AD DS moet [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) worden ingesteld. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* [Wachtwoord-hashes](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) moeten vanuit Azure AD worden gesynchroniseerd met Azure AD DS.
* Een service-account of gebruikersaccount is vereist. Gebruik dit account om de HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Azure AD-domein

**HDInsight geïntegreerd met een on-premises Active Directory via VPN**

Deze architectuur is vergelijkbaar met HDInsight geïntegreerd met Azure AD dat wordt uitgevoerd op Azure IaaS. Het enige verschil is dat Azure AD on-premises is en HDInsight voor Azure AD gebruikmaakt van een [VPN-verbinding van Azure naar een on-premises netwerk](../expressroute/expressroute-introduction.md).

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> In deze architectuur kunt u Azure Data Lake Store niet gebruiken met HDInsight-cluster.

Vereisten voor Azure AD:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de VM's van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) moet worden ingesteld voor de communicatie met Azure AD. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* Een service-account of gebruikersaccount is vereist. Gebruik dit account om de HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Azure AD-domein

**HDInsight geïntegreerd met een on-premises Active Directory die is gesynchroniseerd met Azure AD**

Deze architectuur is vergelijkbaar met HDInsight geïntegreerd met alleen-cloud Azure AD. Het enige verschil is dat de on-premises Active Directory is gesynchroniseerd met Azure AD. Configureer een domeincontroller in de cloud zodat HDInsight kan worden geïntegreerd met Azure AD. Daarvoor gebruikt u [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md). Azure AD DS maakt domeincontrollermachines in de cloud en biedt de IP-adressen van deze machines. Voor maximale beschikbaarheid worden er twee domeincontrollers gemaakt.

Azure AD DS bestaat op dit moment alleen in klassieke virtuele netwerken. Het is alleen toegankelijk via de klassieke Azure-portal. Het virtuele netwerk van HDInsight bestaat in Azure Portal, die met behulp van VNet-naar-VNet-peering moet worden gepeerd met het klassieke virtuele netwerk.

> [!NOTE]
> Voor peering tussen een klassiek virtueel netwerk en een virtueel netwerk van Azure Resource Manager moeten beide virtuele netwerken zich in dezelfde regio bevinden en hetzelfde Azure-abonnement hebben.

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Vereisten voor Azure AD:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de VM's van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* Tijdens de configuratie van Azure AD DS moet [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) worden ingesteld. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* [Wachtwoord-hashes](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) moeten vanuit Azure AD worden gesynchroniseerd met Azure AD DS.
* Een service-account of gebruikersaccount is vereist. Gebruik dit account om de HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein

**HDInsight geïntegreerd met een niet-standaard Azure AD (alleen aanbevolen voor testen en ontwikkeling)**

Deze architectuur is vergelijkbaar met HDInsight geïntegreerd met alleen-cloud Azure AD. Voor de meeste bedrijven is de beheerderstoegang tot Azure AD beperkt tot enkele personen. Als u wilt controleren of een bepaald concept werkt of het maken van een cluster in een domein wilt testen, is het daarom misschien wel handig om een Azure AD-instantie in het abonnement te maken in plaats van te wachten tot een beheerder alle vereisten in Azure AD heeft geconfigureerd. Aangezien dit een Azure AD-instantie is die u hebt gemaakt, hebt u volledige machtigingen voor Azure AD om Azure AD DS te configureren.

Azure AD DS maakt domeincontrollermachines in de cloud en biedt de IP-adressen van deze machines. Voor maximale beschikbaarheid worden er twee domeincontrollers gemaakt.

Azure AD DS bestaat alleen in klassieke virtuele netwerken, waardoor u toegang nodig hebt tot de klassieke Azure-portal, en u dient een klassieke virtueel netwerk te maken voor het configureren van Azure AD DS. Het virtuele netwerk van HDInsight bestaat in Azure Portal, die met behulp van VNet-naar-VNet-peering moet worden gepeerd met het klassieke virtuele netwerk.

> [!NOTE]
> Voor peering tussen klassieke virtuele netwerken en virtuele netwerken van Azure Resource Manager moeten beide virtuele netwerken zich in dezelfde regio bevinden en hetzelfde Azure-abonnement hebben.

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Vereisten voor Azure AD:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de VM's van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* Tijdens de configuratie van Azure AD DS moet [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) worden ingesteld. U kunt u een [zelfondertekend certificaat](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) maken om LDAPS configureren. U moet echter een uitzondering van <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a> aanvragen om een zelfondertekend certificaat te gebruiken.
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* [Wachtwoord-hashes](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) moeten vanuit Azure AD worden gesynchroniseerd met Azure AD DS.
* Een service-account of gebruikersaccount is vereist. Gebruik dit account om de HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Azure Active Directory-domein

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Aan een domein gekoppelde HDInsight-clusters configureren) om een HDInsight-cluster te configureren dat is gekoppeld aan een domein.
* Zie [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Aan een domein gekoppelde HDInsight-clusters beheren) om HDInsight-clusters te beheren die zijn gekoppeld aan een domein.
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](hdinsight-hadoop-linux-use-ssh-unix.md) om Hive-query's uit te voeren met behulp van SSH op HDInsight-clusters die zijn gekoppeld aan een domein.


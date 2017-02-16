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
ms.sourcegitcommit: b5cd321f3cd4c415f5e97ca550b7ab1679324921
ms.openlocfilehash: 94ef8147f1f73b293e818fc508096789373df7fc


---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Azure Hadoop-clusters in aan domein gekoppelde HDInsight plannen

Het traditionele Hadoop is een cluster met één gebruiker. Dit is geschikt voor de meeste bedrijven met kleinere toepassingsteams die hun big data-workloads maken. Naarmate Hadoop populairder wordt, stappen veel ondernemingen over op een model waarin clusters worden beheerd door IT-teams en meerdere toepassingsteams clusters delen. Clusters met meerdere gebruikers vormen dus een van de meest gevraagde functies in HDInsight.

In plaats een eigen verificatie en autorisatie voor meerdere gebruikers in te stellen, maakt HDInsight gebruik van de populairste id-provider: Active Directory (AD). De krachtige functionaliteit op basis van beveiligingsgroepen in Active Directory kan worden gebruikt om autorisatie voor meerdere gebruikers in HDInsight te beheren. Door HDInsight te integreren met Active Directory, kunnen Active Directory-gebruikers met behulp van hun Active Directory-referenties met de clusters communiceren. HDInsight wijst een Active Directory-gebruiker toe aan een lokale Hadoop-gebruiker, zodat alle services die op HDInsight worden uitgevoerd (Ambari, Hive-server, Ranger, Spark Thrift-server, enzovoort), naadloos voor de geverifieerde gebruiker werken.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight integreren met Active Directory

Door HDInsight te integreren met Active Directory, worden de HDInsight-clusterknooppunten toegevoegd aan het Active Directory-domein. HDInsight maakt service-principals voor de Hadoop-services die op het cluster worden uitgevoerd en plaatst deze in een opgegeven OU (organisatie-eenheid) van Active Directory. HDInsight maakt ook omgekeerde DNS-toewijzingen in het Active Directory-domein voor de IP-adressen van de knooppunten die lid zijn van het domein.

U kunt meerdere architecturen volgen om een dergelijke constructie te verkrijgen. U moet zelf bepalen welke architectuur voor u het beste werkt.

**1. HDInsight geïntegreerd met AD dat wordt uitgevoerd op Azure IAAS**

Dit is de eenvoudigste architectuur om HDInsight te integreren met Active Directory. De Active Directory-domeincontroller wordt uitgevoerd op een (of meer) virtuele machines (VM's) in Azure. Deze virtuele machines bevinden zich meestal in een virtueel netwerk. U stelt een ander virtueel netwerk in voor het HDInsight-cluster. Om ervoor te zorgen dat HDInsight zichtbaar is voor Active Directory, moet u deze virtuele netwerken peeren met behulp van [VNet-naar-VNet-peering](../virtual-network/virtual-networks-create-vnetpeering-arm-portal.md).

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_1.png)

> [!NOTE]
> In deze architectuur kunt u Azure Data Lake Store niet gebruiken met HDInsight-cluster.
 

Vereisten voor de Active Directory:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de virtuele machines van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) moet worden ingesteld voor de communicatie met Active Directory. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDInsight-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* U hebt een service- of gebruikersaccount nodig, dat wordt uitgegeven om het HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken.
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein.

**2. HDInsight geïntegreerd met een alleen-cloud Azure AD**

Voor een alleen-cloud Azure Active Directory (Azure AD) moet u een domeincontroller zo configureren dat HDInsight kan worden geïntegreerd met uw Azure Active Directory. Daarvoor gebruikt u [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (Azure AD DS). Azure AD DS maakt domeincontrollermachines in de cloud en verstrekt u de IP-adressen van deze machines. Voor maximale beschikbaarheid worden er twee domeincontrollers gemaakt.

Azure AD DS bestaat momenteel alleen in klassieke VNets. Het is alleen toegankelijk via de klassieke Azure-portal. Het HDInsight VNet bestaat in Azure Portal, die met behulp van VNet-naar-VNet-peering moet worden gepeerd met het klassieke VNet.

> [!NOTE]
> Voor peering tussen een klassiek VNet en een Azure Resource Manager-VNet moeten beide VNets zich in dezelfde regio bevinden en hetzelfde Azure-abonnement hebben.

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Vereisten voor de Active Directory:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de virtuele machines van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst. 
* Tijdens de configuratie van AD DS moet [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) worden ingesteld. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDI-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding). 
* [Wachtwoord-hashes](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) moeten vanuit Azure AD worden gesynchroniseerd met AD DS.
* U hebt een service- of gebruikersaccount nodig, dat wordt gebruikt om het HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken.
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein.

**3. HDInsight geïntegreerd met een on-premises AD via VPN**

Deze architectuur is vergelijkbaar met architectuur 1. Het enige verschil is dat Active Directory on-premises is en HDInsight voor Active Directory gebruikmaakt van een [VPN-verbinding van Azure naar een on-premises netwerk](../expressroute/expressroute-introduction.md).

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_3.png)

> [!NOTE]
> In deze architectuur kunt u Azure Data Lake Store niet gebruiken met HDInsight-cluster.

Vereisten voor de Active Directory:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de virtuele machines van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst.
* [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) moet worden ingesteld voor de communicatie met Active Directory. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDI-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding).
* U hebt een service- of gebruikersaccount nodig, dat wordt gebruikt om het HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken.
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein.

**4. HDInsight geïntegreerd met een on-premises AD die is gesynchroniseerd met een Azure AD**

Deze architectuur is vergelijkbaar met architectuur 2. Het enige verschil is dat de on-premises Active Directory is gesynchroniseerd met de Azure Active Directory. U moet een domeincontroller in de cloud configureren, zodat HDInsight kan worden geïntegreerd met uw Azure Active Directory. Daarvoor gebruikt u [Azure Active Directory Domain Services](../active-directory-domain-services/active-directory-ds-overview.md) (AD DS). AD DS maakt domeincontrollermachines in de cloud en verstrekt u de IP-adressen van deze machines. Voor maximale beschikbaarheid worden er twee domeincontrollers gemaakt.

Azure AD DS bestaat momenteel alleen in klassieke VNets. Het is alleen toegankelijk via de klassieke Azure-portal. Het HDInsight VNet bestaat in Azure Portal, die met behulp van VNet-naar-VNet-peering moet worden gepeerd met het klassieke VNet.

> [!NOTE]
> Voor peering tussen een klassiek VNet en een Azure Resource Manager-VNet moeten beide VNets zich in dezelfde regio bevinden en hetzelfde Azure-abonnement hebben.

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Vereisten voor de Active Directory:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de virtuele machines van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst. 
* Tijdens de configuratie van AD DS moet [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) worden ingesteld. Het certificaat dat voor het instellen van LDAPS wordt gebruikt, moet een echt certificaat zijn (niet een zelfondertekend certificaat).
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDI-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding). 
* [Wachtwoord-hashes](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) moeten vanuit Azure AD worden gesynchroniseerd met AD DS.
* U hebt een service- of gebruikersaccount nodig, dat wordt gebruikt om het HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken.
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein.

**5. HDInsight geïntegreerd met een niet-standaard Azure AD (alleen aanbevolen voor testen en ontwikkeling)**

Deze architectuur is vergelijkbaar met architectuur 2. Voor de meeste bedrijven is de beheerderstoegang tot Active Directory beperkt tot slechts enkele personen. Als u wilt controleren of een bepaald concept werkt of het maken van een cluster in een domein wilt testen, is het daarom misschien wel handig om een nieuwe Azure Active Directory in het abonnement te maken in plaats van te wachten tot de beheerder alle vereisten in de Active Directory heeft geconfigureerd. Aangezien dit een Azure AD is die u hebt gemaakt, hebt u volledige machtigingen voor deze Azure AD om de AD DS te configureren.

AD DS maakt domeincontrollermachines in de cloud en verstrekt u de IP-adressen van deze machines. Voor maximale beschikbaarheid worden er twee domeincontrollers gemaakt.

De AD DS bestaat momenteel alleen in de klassieke VNets. Daarom moet u naar de klassieke portal gaan en een klassiek VNet maken om AD DS te configureren. Het HDInsight VNet bestaat in Azure Portal, die met behulp van VNet-naar-VNet-peering moet worden gepeerd met het klassieke VNet.

> [!NOTE]
> Voor peering tussen klassieke en Azure Resource Manager-VNets moeten beide VNets zich in dezelfde regio bevinden en hetzelfde Azure-abonnement hebben.

![Topologie van aan domein gekoppeld HDInsight-cluster](./media/hdinsight-domain-joined-architecture/hdinsight-domain-joined-architecture_2.png)

Vereisten voor de Active Directory:

* Er moet een [organisatie-eenheid](../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md) worden gemaakt waarin u de virtuele machines van het HDInsight-cluster en de service-principals die door het cluster worden gebruikt, plaatst. 
* Tijdens de configuratie van AD DS moet [LDAPS](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) worden ingesteld. U kunt u een [zelfondertekend certificaat](../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md) maken om LDAPS configureren. U moet echter een uitzondering van <a href="mailto:hdipreview@microsoft.com">hdipreview@microsoft.com</a> aanvragen om een zelfondertekend certificaat te gebruiken.
* Er moeten omgekeerde DNS-zones in het domein worden gemaakt voor het IP-adresbereik van het HDI-subnet (bijvoorbeeld 10.2.0.0/24 in de vorige afbeelding). 
* [Wachtwoord-hashes](../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md) moeten vanuit Azure AD worden gesynchroniseerd met AD DS.
* U hebt een service- of gebruikersaccount nodig, dat wordt gebruikt om het HDInsight-cluster te maken. Dit account moet de volgende machtigingen hebben:

    - Machtigingen om service-principal- en machineobjecten in de organisatie-eenheid te maken.
    - Machtigingen om regels voor omgekeerde DNS-proxy's te maken.
    - Machtigingen om machines lid te maken van het Active Directory-domein.

## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Domain-joined HDInsight clusters](hdinsight-domain-joined-configure.md) (Aan een domein gekoppelde HDInsight-clusters configureren) om een HDInsight-cluster te configureren dat is gekoppeld aan een domein.
* Zie [Manage Domain-joined HDInsight clusters](hdinsight-domain-joined-manage.md) (Aan domein gekoppelde HDInsight-clusters beheren) om HDInsight-clusters te beheren die zijn gekoppeld aan een domein.
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](hdinsight-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie [Use SSH with Linux-based Hadoop on HDInsight from Linux, Unix, or OS X](hdinsight-hadoop-linux-use-ssh-unix.md) (SSH met Hadoop op basis van Linux gebruiken op HDInsight in Linux, Unix of OS X) om Hive-query's uit te voeren met behulp van SSH op HDInsight-clusters die zijn gekoppeld aan een domein.




<!--HONumber=Feb17_HO1-->



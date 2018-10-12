---
title: Azure HDInsight-architectuur met Enterprise-beveiligingspakket
description: Leer hoe u HDInsight-beveiliging met Enterprise-beveiligingspakket van plan bent.
services: hdinsight
ms.service: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: omidm
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: 8d344adc367eb9b93e52d9423a2ab4dda657b298
ms.sourcegitcommit: 4eddd89f8f2406f9605d1a46796caf188c458f64
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49115536"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Enterprise-beveiligingspakket gebruiken in HDInsight

De standard-Azure HDInsight-cluster is een cluster met één gebruiker. Het is geschikt voor de meeste bedrijven met kleinere toepassingsteams die workloads met veel gegevens. Elke gebruiker kan een specifieke cluster op aanvraag maken en vernietigen zodra deze niet meer nodig. 

Veel bedrijven hebben verplaatst naar een model waarin clusters worden beheerd door IT-teams en meerdere toepassingsteams clusters delen. Deze grote ondernemingen met meerdere gebruikers toegang nodig tot elk cluster in Azure HDInsight.

HDInsight is afhankelijk van een populaire id-provider: Active Directory, op een beheerde manier. Door de integratie van HDInsight met [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), kunt u toegang krijgen tot de clusters met behulp van de domeinreferenties van uw. 

De virtuele machines (VM's) in HDInsight zijn van een domein is toegevoegd aan het opgegeven domein. Dus alle services die worden uitgevoerd op HDInsight (Ambari, Hive-server, Ranger, Spark thrift-server en andere) werken naadloos voor de geverifieerde gebruiker. Beheerders kunnen vervolgens met het maken van sterke autorisatiebeleid met behulp van Apache Ranger voor op rollen gebaseerd toegangsbeheer voor bronnen in het cluster.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight integreren met Active Directory

Open source Hadoop is afhankelijk van Kerberos voor verificatie en beveiliging. Daarom zijn HDInsight-clusterknooppunten met Enterprise Security Package (ESP) gekoppeld aan een domein dat wordt beheerd door Azure AD DS. Kerberos-beveiliging is geconfigureerd voor het Hadoop-componenten op het cluster. 

De volgende bewerkingen worden automatisch gemaakt:
- een service-principal voor elk onderdeel Hadoop 
- een machine-principal voor elke machine die gekoppeld aan het domein
- een organisatie-eenheid (OE) voor elk cluster voor het opslaan van deze service en machine-principals 

Om samen te vatten, moet u voor het instellen van een omgeving met:

- Active Directory-domein (beheerd door Azure AD DS).
- Secure LDAP (LDAPS) ingeschakeld in Azure AD DS.
- Juiste netwerken connectiviteit van het virtuele netwerk van HDInsight met de Azure AD DS virtueel netwerk, als u afzonderlijke virtuele netwerken voor hen kiest. Een virtuele machine binnen het virtuele netwerk van HDInsight moet een verbinding met Azure AD DS via peering op virtueel netwerk hebben. Als HDInsight en Azure AD DS zijn geïmplementeerd in hetzelfde virtuele netwerk, de verbinding automatisch wordt geleverd en is geen verdere actie nodig.

## <a name="set-up-different-domain-controllers"></a>Instellen van de verschillende domeincontrollers
HDInsight ondersteunt momenteel alleen Azure AD DS als de primaire domeincontroller die het cluster voor communicatie van Kerberos gebruikt. Maar andere complexe Active Directory-instellingen zijn mogelijk, zolang dergelijke installatie leidt tot het inschakelen van Azure AD DS voor HDInsight-toegang.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) biedt een beheerd domein die volledig compatibel is met Windows Server Active Directory. Microsoft zorgt beheren, toepassen van patches en bewaking van het domein in een configuratie voor hoge beschikbaarheid (HA). U kunt uw cluster implementeren zonder u zorgen te maken over het onderhouden van domeincontrollers. 

Gebruikers, groepen en wachtwoorden worden gesynchroniseerd vanuit de Azure Active Directory (Azure AD). De synchronisatie in één richting van uw Azure AD-instantie met Azure AD DS kan gebruikers zich aanmelden bij het cluster met behulp van de zakelijke referenties. 

Zie voor meer informatie, [configureren HDInsight-clusters met behulp van Azure AD DS ESP](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>On-premises Active Directory of Active Directory op virtuele IaaS-machines

Als u een on-premises Active Directory-exemplaar of meer complexe Active Directory-instellingen voor uw domein hebt, kunt u deze identiteiten met Azure AD synchroniseren met behulp van Azure AD Connect. Vervolgens kunt u Azure AD DS op deze Active Directory-tenant. 

Omdat Kerberos is afhankelijk van wachtwoord-hashes, moet u [wachtwoordhashsynchronisatie op Azure AD DS inschakelen](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Als u Federatie met Active Directory Federation Services (AD FS), kunt u eventueel instellen van synchronisatie van wachtwoordhashes als een back-up als uw AD FS-infrastructuur is mislukt. Zie voor meer informatie, [synchronisatie van wachtwoordhashes met Azure AD Connect-synchronisatie inschakelen](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

Met on-premises Active Directory of Active Directory op IaaS-VM's alleen, zonder dat u Azure AD en Azure AD DS, is geen ondersteunde configuratie voor HDInsight-clusters met ESP.

## <a name="next-steps"></a>Volgende stappen

* [HDInsight-clusters configureren met ESP](apache-domain-joined-configure-using-azure-adds.md)
* [Hive-beleidsregels voor HDInsight-clusters configureren met ESP](apache-domain-joined-run-hive.md)
* [HDInsight-clusters met ESP beheren](apache-domain-joined-manage.md) 

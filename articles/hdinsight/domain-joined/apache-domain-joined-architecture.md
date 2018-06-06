---
title: Domein Azure HDInsight-architectuur | Microsoft Docs
description: Meer informatie over het plannen van aan domein gekoppelde HDInsight.
services: hdinsight
documentationcenter: ''
author: omidm1
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 7dc6847d-10d4-4b5c-9c83-cc513cf91965
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: ''
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: f4380f5d6ec379d5807f697294623a672bd270ae
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715238"
---
# <a name="plan-azure-domain-joined-hadoop-clusters-in-hdinsight"></a>Azure Hadoop-clusters in aan domein gekoppelde HDInsight plannen

Het standaard HDInsight-cluster is een cluster met één gebruiker. Dit is geschikt voor de meeste bedrijven met kleinere toepassingsteams die workloads met veel gegevens maken. Elke gebruiker kan een ander cluster hebt toegewezen aan zichzelf op aanvraag en vernietigen wanneer deze niet meer nodig. Veel bedrijven gestart echter gericht op een model waarin clusters worden beheerd door IT-teams en toepassing van meerdere teams share clusters. Dus is voor meerdere gebruikers toegang tot het cluster in Azure HDInsight nodig voor deze grote ondernemingen.

HDInsight is afhankelijk van de meest populaire id-provider--Active Directory (AD) op een beheerde manier. Door integratie van HDInsight met [Azure Active Directory Domain Services (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md), u toegang hebt tot de clusters met behulp van de domeinreferenties van uw. De virtuele machines in HDInsight worden dus alle services die worden uitgevoerd op HDInsight domein is gekoppeld aan het opgegeven domein (Ambari, Hive-server, Zwerver, Spark thrift-server en andere) naadloos voor de geverifieerde gebruiker. Beheerders kunnen vervolgens sterk autorisatiebeleid met Apache Zwerver voor op rollen gebaseerde toegangsbeheer voor bronnen in het cluster maken.


## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight integreren met Active Directory

Open source Hadoop is afhankelijk van Kerberos voor het bieden van authenticatie en beveiliging. HDInsight-clusterknooppunten zijn daarom domein is gekoppeld aan een domein dat wordt beheerd door AAD DS. Kerberos-beveiliging is geconfigureerd voor het Hadoop-onderdelen op het cluster. Voor elk van de Hadoop-onderdelen, wordt automatisch een service-principal gemaakt. Een overeenkomstige machine principal wordt ook gemaakt voor elke computer die is gekoppeld aan het domein. Om te kunnen opslaan van deze service en de machine-principals, is het vereist om een organisatie-eenheid (OE) binnen de domeincontroller (AAD-DS), waar deze principals zijn geplaatst. 

Om samen te vatten, moet u voor het instellen van een omgeving met:

- Active Directory-domein (beheerd door de AAD-DS)
- Beveiligde LDAP (LDAPS) ingeschakeld in het AAD-DS.
- Goede netwerken verbinding hebben met HDInsight van VNET naar het VNET AAD-DS als u ervoor kiest scheiden VNETs voor hen. Een virtuele machine binnen het VNET HDI moet een regel zicht in AAD DS met behulp van VNET-peering. Als zowel de HDI en de AAD-DS in hetzelfde VNET zijn geïmplementeerd, de verbinding automatisch wordt opgegeven en is geen verdere actie nodig.
- Een organisatie-eenheid (OE) [op AAD-DS gemaakt](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)
- Een serviceaccount met machtigingen voor:
    - Service-principals in de organisatie-eenheid maken.
    - Machines toevoegen aan het domein en de machine-principals in de organisatie-eenheid maken.

De volgende schermafbeelding ziet een organisatie-eenheid gemaakt in contoso.com. Sommige van de service-principals en machine principals worden weergegeven in de schermafbeelding ook.

![Domein die lid zijn van HDInsight-clusters organisatie-eenheid](./media/apache-domain-joined-architecture/hdinsight-domain-joined-ou.png).

### <a name="different-domain-controllers-setup"></a>Andere domein domeincontrollers instellen:
HDInsight ondersteunt momenteel alleen AAD-DS als de primaire domeincontroller het cluster zullen communiceren met Kerberise het cluster. Andere complexe AD-instellingen zijn echter ook mogelijk, zolang deze leidt tot het AAD-DS inschakelen voor HDI-toegang.

- **[Azure Active Directory Domain Services (AAD-DS)](../../active-directory-domain-services/active-directory-ds-overview.md)**: deze service biedt een beheerd domein, volledig compatibel met Windows Server Active Directory is. Microsoft zorgt voor beheer, patchen en de bewaking van het domein in de instellingen voor een maximaal Available(HA). U kunt uw cluster implementeren zonder dat u over het beheren van domeincontrollers. Gebruikers, groepen en wachtwoorden worden gesynchroniseerd vanuit uw Azure Active Directory(AAD) [synchronisatie in één richting van AAD naar AAD DS], waardoor gebruikers aan te melden bij het cluster met dezelfde zakelijke referenties. Zie voor meer informatie [hoe configureren domein HDInsight-clusters met AAD DS](./apache-domain-joined-configure-using-azure-adds.md).
- **On-premises AD of AD op IaaS VM's**: als u een on-premises AD of andere complexe AD hebt instellingen voor uw domein, kunt u deze identiteiten met AAD met AD Connect vervolgens inschakelen tenant van AAD-DS op dat AD synchroniseren. Omdat Kerberos op wachtwoord-hashes vertrouwt, moet u [wachtwoordhashsynchronisatie op AAD-DS inschakelen](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). Als u gebruikmaakt van Federatie met AD-federation Services (ADFS), kunt u eventueel wachtwoordhashsynchronisatie als een back-up instellen als uw AD FS-infrastructuur is mislukt. Zie voor meer informatie [wachtwoordhashsynchronisatie met AAD Connect-synchronisatie inschakelen](../../active-directory/connect/active-directory-aadconnectsync-implement-password-hash-synchronization.md). On-premises AD of AD met op IaaS VM's alleen, is zonder AAD en AAD-DS niet een ondersteunde configuratie voor het lidmaatschap van domein van HDI cluster.

## <a name="next-steps"></a>Volgende stappen
* [HDInsight-clusters domein configureren](apache-domain-joined-configure-using-azure-adds.md).
* [Hive beleidsregels configureren voor HDInsight-clusters domein](apache-domain-joined-run-hive.md).
* [Domein-HDInsight-clusters beheren](apache-domain-joined-manage.md). 

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
ms.openlocfilehash: 7e71f27ab8d577602dd4b02f83d57ff84a92858a
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58088091"
---
# <a name="use-enterprise-security-package-in-hdinsight"></a>Enterprise-beveiligingspakket gebruiken in HDInsight

De standard-Azure HDInsight-cluster is een cluster met één gebruiker. Het is geschikt voor de meeste bedrijven met kleinere toepassingsteams die workloads met veel gegevens. Elke gebruiker kan een specifieke cluster op aanvraag maken en vernietigen zodra deze niet meer nodig. 

Veel bedrijven hebben verplaatst naar een model waarin IT teams-clusters beheren en meerdere toepassingsteams clusters delen. Deze grote ondernemingen met meerdere gebruikers toegang nodig tot elk cluster in Azure HDInsight.

HDInsight is afhankelijk van een populaire id-provider: Active Directory, op een beheerde manier. Door de integratie van HDInsight met [Azure Active Directory Domain Services (Azure AD DS)](../../active-directory-domain-services/active-directory-ds-overview.md), kunt u toegang krijgen tot de clusters met behulp van de domeinreferenties van uw. 

De virtuele machines (VM's) in HDInsight zijn van een domein is toegevoegd aan het opgegeven domein. Dus werken alle services die worden uitgevoerd op HDInsight (Apache Ambari, server Apache Hive, Apache Ranger, Apache Spark thrift-server en anderen) naadloos voor de geverifieerde gebruiker. Beheerders kunnen vervolgens met het maken van sterke autorisatiebeleid met behulp van Apache Ranger voor op rollen gebaseerd toegangsbeheer voor bronnen in het cluster.

## <a name="integrate-hdinsight-with-active-directory"></a>HDInsight integreren met Active Directory

Open-source Apache Hadoop, is afhankelijk van het Kerberos-protocol voor verificatie en beveiliging. Daarom zijn HDInsight-clusterknooppunten met Enterprise Security Package (ESP) gekoppeld aan een domein dat wordt beheerd door Azure AD DS. Kerberos-beveiliging is geconfigureerd voor het Hadoop-componenten op het cluster. 

De volgende bewerkingen worden automatisch gemaakt:

- een service-principal voor elk onderdeel Hadoop
- een machine-principal voor elke machine die gekoppeld aan het domein
- een organisatie-eenheid (OE) voor elk cluster voor het opslaan van deze service en machine-principals

Om samen te vatten, moet u voor het instellen van een omgeving met:

- Active Directory-domein (beheerd door Azure AD DS).
- Secure LDAP (LDAPS) ingeschakeld in Azure AD DS.
- Juiste netwerken connectiviteit van het virtuele netwerk van HDInsight met de Azure AD DS virtueel netwerk, als u afzonderlijke virtuele netwerken voor hen kiest. Een virtuele machine binnen het virtuele netwerk van HDInsight moet een verbinding met Azure AD DS via peering op virtueel netwerk hebben. Als HDInsight en Azure AD DS zijn geïmplementeerd in hetzelfde virtuele netwerk, de verbinding automatisch wordt geleverd, is geen verdere actie te ondernemen.

## <a name="set-up-different-domain-controllers"></a>Instellen van de verschillende domeincontrollers
HDInsight ondersteunt momenteel alleen Azure AD DS als de primaire domeincontroller die het cluster voor communicatie van Kerberos gebruikt. Maar andere complexe Active Directory-instellingen zijn mogelijk, zolang dergelijke installatie leidt tot het inschakelen van Azure AD DS voor HDInsight-toegang.

### <a name="azure-active-directory-domain-services"></a>Azure Active Directory Domain Services
[Azure AD DS](../../active-directory-domain-services/active-directory-ds-overview.md) biedt een beheerd domein die volledig compatibel is met Windows Server Active Directory. Microsoft zorgt beheren, toepassen van patches en bewaking van het domein in een configuratie voor hoge beschikbaarheid (HA). U kunt uw cluster implementeren zonder u zorgen te maken over het onderhouden van domeincontrollers. 

Gebruikers, groepen en wachtwoorden worden gesynchroniseerd vanuit Azure AD. De synchronisatie in één richting van uw Azure AD-instantie met Azure AD DS kan gebruikers zich aanmelden bij het cluster met behulp van de zakelijke referenties. 

Zie voor meer informatie, [configureren HDInsight-clusters met behulp van Azure AD DS ESP](./apache-domain-joined-configure-using-azure-adds.md).

### <a name="on-premises-active-directory-or-active-directory-on-iaas-vms"></a>On-premises Active Directory of Active Directory op virtuele IaaS-machines

Als u een on-premises Active Directory-exemplaar of meer complexe Active Directory-instellingen voor uw domein hebt, kunt u deze identiteiten met Azure AD synchroniseren met behulp van Azure AD Connect. Vervolgens kunt u Azure AD DS op deze Active Directory-tenant. 

Omdat Kerberos is afhankelijk van wachtwoord-hashes, moet u [wachtwoordhashsynchronisatie op Azure AD DS inschakelen](../../active-directory-domain-services/active-directory-ds-getting-started-password-sync.md). 

Als u Federatie met Active Directory Federation Services (AD FS), moet u synchronisatie van wachtwoordhashes inschakelen. (Zie voor een aanbevolen instelling [in deze video](https://youtu.be/qQruArbu2Ew).) Synchronisatie van wachtwoordhashes helpt bij herstel na noodgevallen in het geval de AD FS-infrastructuur is mislukt en het helpt tevens gelekte referentie-beveiliging bieden. Zie voor meer informatie, [synchronisatie van wachtwoordhashes met Azure AD Connect-synchronisatie inschakelen](../../active-directory/hybrid/how-to-connect-password-hash-synchronization.md). 

Met behulp van on-premises Active Directory of Active Directory op IaaS-VM's alleen zonder Azure AD en Azure AD DS, is niet een ondersteunde configuratie voor HDInsight-clusters met ESP.

Als federation wordt gebruikt en wachtwoord-hashes juist zijn gesynchroniseerd, maar u verificatiefouten ontvangt, controleert u of cloud-wachtwoordverificatie is ingeschakeld voor de PowerShell-service-principal. Als u niet het geval is, moet u instellen een [start Realm detectie (HRD) beleid](../../active-directory/manage-apps/configure-authentication-for-federated-users-portal.md) voor uw Azure AD-tenant. Om te controleren en stelt u het HRD-beleid:

1. Installeer de Azure AD PowerShell-module.

   ```
   Install-Module AzureAD
   ```

2. Voer `Connect-AzureAD` met behulp van referenties van de globale beheerder (tenantbeheerder).

3. Controleer als de Microsoft Azure PowerShell service-principal is al gemaakt.

   ```
   $powershellSPN = Get-AzureADServicePrincipal -SearchString "Microsoft Azure Powershell"
   ```

4. Als deze niet bestaat (dat wil zeggen als `($powershellSPN -eq $null)`), klikt u vervolgens de service-principal te maken.

   ```
   $powershellSPN = New-AzureADServicePrincipal -AppId 1950a258-227b-4e31-a9cf-717495945fc2
   ```

5. Maken en koppelen van het beleid aan deze service-principal.

   ```
   $policy = New-AzureADPolicy -Definition @("{`"HomeRealmDiscoveryPolicy`":{`"AllowCloudPasswordValidation`":true}}") -DisplayName EnableDirectAuth -Type HomeRealmDiscoveryPolicy

   Add-AzureADServicePrincipalPolicy -Id $powershellSPN.ObjectId -refObjectID $policy.ID
   ```

## <a name="next-steps"></a>Volgende stappen

* [HDInsight-clusters configureren met ESP](apache-domain-joined-configure-using-azure-adds.md)
* [Apache Hive-beleid voor HDInsight-clusters configureren met ESP](apache-domain-joined-run-hive.md)
* [HDInsight-clusters met ESP beheren](apache-domain-joined-manage.md) 

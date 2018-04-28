---
title: Domein-HDInsight-clusters met AAD DS configureren
description: Meer informatie over het instellen en domein HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 04/17/2018
ms.author: omidm
ms.openlocfilehash: 060ca8040f514ec1df48c2ca4568cbbb2a529267
ms.sourcegitcommit: 59914a06e1f337399e4db3c6f3bc15c573079832
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/19/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Domein-HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren

Domein-clusters bieden de onderneming met meerdere gebruikers beveiligingsmogelijkheden in HDInsight. HDInsight-clusters domein zijn verbonden met active directory-domeinen, zodat gebruikers van een domein hun domeinreferenties gebruiken kunnen voor verificatie met de clusters en big data-taken uitvoeren. 

In dit artikel leert u hoe een domein HDInsight-cluster met behulp van Azure Active Directory Domain Services configureren.

> [!NOTE]
> Active Directory op Azure IaaS VM's wordt niet langer ondersteund.

## <a name="create-azure-adds"></a>Azure ADDS maken

U moet maken van een Azure AD DS voordat u een HDInsight-cluster kunt maken. Zie voor het maken van een Azure wordt toegevoegd, [inschakelen Azure Active Directory Domain Services met Azure portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Alleen de pachterbeheerders hebben de bevoegdheden voor het maken van de domeinservices. Als u Azure Data Lake Storage (ADLS) als de standaard-opslag voor HDInsight, Controleer of dat de standaard Azure AD-tenant voor ADLS is hetzelfde als het domein voor het HDInsight-cluster. Multi-factor authentication-server moet worden uitgeschakeld voor gebruikers die toegang tot het cluster voor deze ingesteld om te werken met Azure Data Lake Store.

Nadat de AAD-domein-service is ingericht, moet u een serviceaccount in AAD (die worden gesynchroniseerd naar AAD-DS) maken met de juiste machtigingen om de HDInsight-cluster te maken. Als deze serviceaccount al bestaat, moet u opnieuw instellen van het wachtwoord en wacht totdat deze wordt gesynchroniseerd met AAD-DS (deze bewerking leidt ertoe dat het maken van het kerberos-wachtwoord-hash en het kan tot 30 minuten duren). Deze serviceaccount mag de volgende bevoegdheid hebben:

- Computers toevoegen aan het domein en plaats machine principals binnen de organisatie-eenheid die u tijdens het maken van het cluster opgeeft.
- Service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.

Beveiligde LDAP voor Azure AD Domain Services beheerd domein, moet u inschakelen. Zie voor het inschakelen van beveiligde LDAP [configureren beveiligde LDAP (LDAPS) voor een Azure AD Domain Services beheerd domein](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Een domein HDInsight-cluster maken

De volgende stap is het maken van het HDInsight-cluster met behulp van de AAD-DS en het serviceaccount in de vorige sectie hebt gemaakt.

Het is eenvoudiger om de service Azure AD-domein en het HDInsight-cluster in de dezelfde Azure virtuele network(VNet). Als ze zich in verschillende VNets, moet u beide VNets peer. Zie voor meer informatie [virtuele netwerk peering](../../virtual-network/virtual-network-peering-overview.md).

Wanneer u een domein HDInsight-cluster maakt, moet u de volgende parameters opgeven:

- **Domeinnaam**: de domeinnaam die is gekoppeld aan Azure AD DS. Bijvoorbeeld: contoso.onmicrosoft.com
- **Domeingebruikersnaam**: de serviceaccount in de Azure AD-domeincontroller die is gemaakt in de vorige sectie. Bijvoorbeeld hdiadmin@contoso.onmicrosoft.com. Deze domeingebruiker worden de beheerder van dit domein HDInsight-cluster.
- **Domeinwachtwoord**: het wachtwoord van het serviceaccount.
- **Organisatie-eenheid**: de DN-naam van de organisatie-eenheid die u wilt gebruiken met HDInsight-cluster. Bijvoorbeeld: OU = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Als deze organisatie-eenheid niet bestaat, wordt de HDInsight-cluster probeert te maken van deze organisatie-eenheid. 
- **LDAPS URL**: bijvoorbeeld ldaps://contoso.onmicrosoft.com:636
- **Gebruikersgroep toegang**: de beveiligingsgroepen waarvan gebruikers die u wilt synchroniseren met het cluster. Bijvoorbeeld: HiveUsers. Als u meerdere gebruikersgroepen opgeven wilt, gescheiden door komma's ','.
 
> [!NOTE]
> Omdat de domeinnaam Apache Zeppelin gebruikt om de administrative-service-account te verifiëren, moet het serviceaccount dezelfde domeinnaam als de UPN-achtervoegsel voor Apache Zeppelin goed te laten functioneren hebben.
 
De volgende schermafbeelding ziet de configuraties in de Azure portal:

![Azure HDInsight domein Active Directory Domain Services-configuratie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor het gebruik van SSH verbinding maken met domein HDInsight-clusters [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).


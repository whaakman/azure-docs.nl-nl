---
title: Configureren van een domein gekoppeld HDInsight-cluster met behulp van Azure AD DS
description: Meer informatie over het instellen en configureren van een domein gekoppeld HDInsight-cluster met behulp van Azure Active Directory Domain Services
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
editor: jasonwhowell
ms.topic: conceptual
ms.date: 07/17/2018
ms.openlocfilehash: 0d44812c92fd14bf87aac9a942241f8de55f2eec
ms.sourcegitcommit: 1f0587f29dc1e5aef1502f4f15d5a2079d7683e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/07/2018
ms.locfileid: "39590577"
---
# <a name="configure-a-domain-joined-hdinsight-cluster-by-using-azure-active-directory-domain-services"></a>Configureren van een domein gekoppeld HDInsight-cluster met behulp van Azure Active Directory Domain Services

Aan domein gekoppelde clusters bieden met meerdere gebruikers toegang op Azure HDInsight-clusters. Aan domein gekoppelde HDInsight-clusters zijn verbonden met een domein, zodat gebruikers van een domein hun domeinreferenties gebruiken kunnen om te verifiëren met de clusters en het uitvoeren van taken met big data. 

In dit artikel leert u hoe u een domein gekoppeld HDInsight-cluster configureren met behulp van Azure Active Directory Domain Services (Azure AD DS).

## <a name="enable-azure-ad-ds"></a>Azure AD DS inschakelen

Inschakelen van Azure AD DS is een vereiste voordat u een domein gekoppeld HDInsight-cluster kunt maken. Zie voor meer informatie, [inschakelen Azure Active Directory Domain Services met behulp van de Azure-portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Alleen tenantbeheerders hebben de bevoegdheden voor het maken van een Azure AD DS-exemplaar. Als u Azure Data Lake Storage Gen1 als de standaardopslag voor HDInsight gebruikt, zorg ervoor dat de standaard Azure AD-tenant voor Data Lake Storage Gen1 hetzelfde als het domein voor het HDInsight-cluster is. Omdat Hadoop, is afhankelijk van Kerberos en basisverificatie wordt gebruikt, moet de multi-factor authentication voor gebruikers die toegang het cluster tot worden uitgeschakeld.

Nadat u de Azure AD DS-exemplaar inricht, moet u een serviceaccount in Azure Active Directory (Azure AD) maken met de juiste machtigingen. Als deze serviceaccount al bestaat, wordt het wachtwoord wijzigen en wacht totdat deze wordt gesynchroniseerd met Azure AD DS. Deze bewerking zal leiden tot het maken van de Kerberos-wachtwoord-hash en het duurt maximaal 30 minuten wilt synchroniseren met Azure AD DS. 

Het serviceaccount moet de volgende bevoegdheden:

- Machines toevoegen aan het domein en machine-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft plaatsen.
- Service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.

> [!NOTE]
> Omdat Apache Zeppelin maakt gebruik van de domeinnaam verifiëren van de administrative-service-account, het serviceaccount *moet* hebben dezelfde domeinnaam als de UPN-achtervoegsel voor Apache Zeppelin te laten functioneren.

Zie voor meer informatie over de organisatie-eenheden en hoe deze te beheren, [maken een organisatie-eenheid op een beheerd domein van Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

Secure LDAP is voor een beheerd domein van Azure AD DS. Zie voor meer informatie, [secure LDAP configureren voor een Azure AD DS beheerd domein](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Een aan domein gekoppelde HDInsight-cluster maken

De volgende stap is het maken van het HDInsight-cluster met behulp van Azure AD DS en het serviceaccount dat u in de vorige sectie hebt gemaakt.

Het is eenvoudiger om zowel de Azure AD DS-exemplaar als het HDInsight-cluster in hetzelfde Azure virtual network. Als u ervoor kiest om ze in verschillende virtuele netwerken, moet u deze virtuele netwerken koppelen zodat HDInsight virtuele machines een verbinding met de domeincontroller hebben voor het lidmaatschap van de virtuele machines. Zie voor meer informatie, [peering van virtuele netwerken](../../virtual-network/virtual-network-peering-overview.md).

Wanneer u een domein gekoppeld HDInsight-cluster maakt, moet u de volgende parameters opgeven:

- **Domeinnaam**: de naam van het domein dat is gekoppeld aan Azure AD DS. Een voorbeeld is contoso.onmicrosoft.com.

- **Domeingebruikersnaam**: het serviceaccount in de DC Azure toegevoegd beheerd domein dat u in de vorige sectie hebt gemaakt. Een voorbeeld is hdiadmin@contoso.onmicrosoft.com. In dit de domeingebruiker is de beheerder van dit HDInsight-cluster.

- **Domeinwachtwoord**: het wachtwoord van het serviceaccount.

- **Organisatie-eenheid**: de DN-naam van de organisatie-eenheid die u wilt gebruiken voor het HDInsight-cluster. Een voorbeeld is de organisatie-eenheid = HDInsightOU, DC = contoso, DC = onmicrosoft gebruikt, DC = com. Als deze organisatie-eenheid niet bestaat, wordt het HDInsight-cluster probeert te maken van de organisatie-eenheid met behulp van de bevoegdheden die de serviceaccount heeft. Bijvoorbeeld, als het serviceaccount zich in de groep beheerders van Azure AD DS, heeft deze de juiste machtigingen voor het maken van een organisatie-eenheid. Anders moet u eerst de organisatie-eenheid maken en geef de service-account van volledige controle over de organisatie-eenheid. Zie voor meer informatie, [maken een organisatie-eenheid op een beheerd domein van Azure AD DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md).

    > [!IMPORTANT]
    > Omvat alle DC's, gescheiden door komma's, na de organisatie-eenheid (bijvoorbeeld organisatie-eenheid HDInsightOU, DC = contoso, DC = = onmicrosoft gebruikt, DC = com).

- **LDAPS-URL**: een voorbeeld is ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Voer de volledige URL, met inbegrip van ' ldaps: / / ' en het poortnummer (: 636).

- **Gebruikersgroep openen**: de beveiligingsgroepen waarvan gebruikers die u wilt synchroniseren met het cluster. Bijvoorbeeld: HiveUsers. Als u meerdere gebruikersgroepen opgeven wilt, gescheiden door puntkomma (;). De groep(en) moet bestaan in de map voordat het wordt ingericht. Zie voor meer informatie, [een groep maken en leden toevoegen in Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Als de groep niet bestaat, treedt er een fout op: "Groep HiveUsers niet gevonden in Active Directory."

De volgende schermafbeelding ziet u de configuraties in Azure portal:

   ![Azure HDInsight domein Active Directory Domain Services-configuratie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Volgende stappen
* Zie voor Hive-beleid configureren en uitvoeren van Hive-query's, [configureren Hive-beleid voor aan domein gekoppelde HDInsight-clusters](apache-domain-joined-run-hive.md).
* Zie voor het gebruik van SSH verbinding maken met HDInsight-clusters domein [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).


---
title: Een HDInsight-cluster configureren met Enterprise-beveiligingspakket met behulp van Azure AD DS-
description: Meer informatie over het instellen en configureren van een Enterprise-beveiligingspakket van HDInsight-cluster met behulp van Azure Active Directory Domain Services
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: jasonh
ms.topic: conceptual
ms.date: 09/24/2018
ms.openlocfilehash: a5b377381fd540c2a9f1d85e0cb7edce32c2dae8
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46968370"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Een HDInsight-cluster configureren met Enterprise-beveiligingspakket met behulp van Azure Active Directory Domain Services

Enterprise Security Package (ESP)-clusters bieden toegang door meerdere gebruikers in Azure HDInsight-clusters. HDInsight-clusters met ESP zijn verbonden met een domein, zodat gebruikers van een domein hun domeinreferenties gebruiken kunnen om te verifiëren met de clusters en het uitvoeren van taken met big data. 

In dit artikel leert u hoe u een HDInsight-cluster met ESP configureren met behulp van Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>ESP is beschikbaar in HDI 3.6 + voor Spark, interactieve en Hadoop. ESP voor HBase-clustertypen is beschikbaar als preview.


## <a name="enable-azure-ad-ds"></a>Inschakelen van Azure AD DS

Het is een vereiste voor het inschakelen van Azure AD DS-, voordat u een HDInsight-cluster met ESP maken kunt. Zie voor meer informatie, [inschakelen Azure Active Directory Domain Services met behulp van de Azure-portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Alleen tenantbeheerders hebben de bevoegdheden voor het maken van een Azure AD DS-exemplaar. Als u Azure Data Lake Storage Gen1 als de standaardopslag voor HDInsight gebruikt, zorg ervoor dat de standaard Azure AD-tenant voor Data Lake Storage Gen1 hetzelfde als het domein voor het HDInsight-cluster is. Omdat Hadoop, is afhankelijk van Kerberos en basisverificatie wordt gebruikt, moet de multi-factor authentication voor gebruikers die toegang het cluster tot worden uitgeschakeld.

Secure LDAP is voor een beheerd domein van Azure AD DS. Bij het inschakelen van LDAPS, plaatst u de domeinnaam in de onderwerpnaam of alternatieve onderwerpnaam in het certificaat. Zie voor meer informatie, [configureren secure LDAP voor een Azure AD DS-domein beheerd](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="add-managed-identity"></a>Beheerde identiteit toevoegen

Nadat u Azure AD DS-hebt ingeschakeld, maken van een beheerde identiteit en wijs deze toe aan de **HDInsight Domain Services Inzender** rol in Azure AD DS-toegangsbeheer.

![Active Directory Domain Services Access control van Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Zie voor meer informatie, [wat is beheerde identiteiten voor Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>Een HDInsight-cluster maken met ESP

De volgende stap is het maken van het HDInsight-cluster met ESP ingeschakeld met behulp van Azure AD DS.

Het is eenvoudiger om zowel de Azure AD DS-exemplaar als het HDInsight-cluster in hetzelfde Azure virtual network. Als u ervoor kiest om ze in verschillende virtuele netwerken, moet u deze virtuele netwerken koppelen zodat HDInsight virtuele machines een verbinding met de domeincontroller hebben voor het lidmaatschap van de virtuele machines. Zie voor meer informatie, [peering van virtuele netwerken](../../virtual-network/virtual-network-peering-overview.md).

Wanneer u een HDInsight-cluster maakt, hebt u de optie voor het inschakelen van Enterprise-beveiligingspakket om uw cluster met Azure AD DS-verbinding te maken. 

![Azure HDInsight-beveiliging en netwerken](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Zodra u ESP inschakelt, worden veelvoorkomende onjuiste configuraties die zijn gerelateerd aan Azure AD DS-automatisch gedetecteerd en gevalideerd.

![Azure HDInsight Enterprise security package-domeinvalidatie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Vroegtijdig bespaart u tijd doordat u kunt fouten corrigeren voordat u het cluster te maken.

![Azure HDInsight Enterprise-beveiligingspakket domeinvalidatie is mislukt](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Wanneer u een HDInsight-cluster met ESP maken, moet u de volgende parameters opgeven:

- **Gebruiker met beheerdersrechten cluster**: Kies een beheerder voor uw cluster vanaf uw gesynchroniseerde Azure AD DS-.

- **Cluster-toegangsgroepen**: de waarvan u wilt synchroniseren met het cluster de gebruikers gesynchroniseerd en beschikbaar in Azure AD DS worden moeten-beveiligingsgroepen. Bijvoorbeeld: HiveUsers. Als u meerdere gebruikersgroepen opgeven wilt, gescheiden door puntkomma (;). De groep(en) moet bestaan in de map voordat het wordt ingericht. Zie voor meer informatie, [een groep maken en leden toevoegen in Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md). Als de groep niet bestaat, treedt er een fout op: "Groep HiveUsers niet gevonden in Active Directory."

- **LDAPS-URL**: een voorbeeld is ldaps://contoso.onmicrosoft.com:636.

    > [!IMPORTANT]
    > Voer de volledige URL, met inbegrip van ' ldaps: / / ' en het poortnummer (: 636).

De volgende schermafbeelding ziet u de configuraties in Azure portal:

   ![Azure HDInsight ESP Active Directory Domain Services-configuratie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Volgende stappen
* Zie voor Hive-beleid configureren en uitvoeren van Hive-query's, [configureren Hive-beleid voor HDInsight-clusters met ESP](apache-domain-joined-run-hive.md).
* Zie voor het gebruik van SSH verbinding maken met HDInsight-clusters met ESP [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).


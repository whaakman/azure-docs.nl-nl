---
title: Domein-HDInsight-clusters met AAD DS configureren
description: Meer informatie over het instellen en domein HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren
services: hdinsight
author: omidm1
manager: jhubbard
editor: cgronlun
ms.service: hdinsight
ms.topic: conceptual
ms.date: 05/30/2018
ms.author: omidm
ms.openlocfilehash: 7bde1c834038bdc2a010987b4e32fa49222101ee
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34715272"
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Domein-HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren

Domein-clusters bieden de toegang door meerdere gebruikers op HDInsight-clusters. HDInsight-clusters domein zijn verbonden met een domein, zodat gebruikers van een domein hun domeinreferenties gebruiken kunnen voor verificatie met de clusters en big data-taken uitvoeren. 

In dit artikel leert u hoe een domein HDInsight-cluster met behulp van Azure Active Directory Domain Services configureren.

> [!NOTE]
> Maken van een domein HDInsight-cluster, moet Azure Active Directory Domain Services. Maken van een domein HDInsight cluster gebruikmaakt van Active Directory die worden gehost in Azure IaaS virtuele machines niet meer wordt ondersteund.

## <a name="create-azure-adds"></a>Azure ADDS maken

Inschakelen van Azure AD Domain Services (AAD-DS) is een vereiste voordat u een domein HDInsight-cluster kunt maken. Zie voor het inschakelen van een exemplaar van de AAD-DS [het inschakelen van AAD-DS met behulp van de Azure-portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Alleen de pachterbeheerders hebben de bevoegdheden een AAD-DS-exemplaar te maken. Als u Azure Data Lake Storage (ADLS) als de standaard-opslag voor HDInsight, Controleer of dat de standaard Azure AD-tenant voor ADLS is hetzelfde als het domein voor het HDInsight-cluster. Sincde Hadoop is afhankelijk van de Kerberos- en basic Authentication, MFA moet worden uitgeschakeld voor gebruikers die toegang tot het cluster.

Nadat het AAD-DS-exemplaar is ingericht, moet u een serviceaccount in AAD (die worden gesynchroniseerd naar AAD-DS) maken met de juiste machtigingen. Als deze serviceaccount al bestaat, moet u het wachtwoord opnieuw instellen en wacht totdat deze wordt gesynchroniseerd met AAD-DS (deze bewerking leidt ertoe dat het maken van het kerberos-wachtwoord-hash en het kan maximaal 30 min om te synchroniseren met AAD-DS duren). Deze serviceaccount mag de volgende bevoegdheden hebben:

- Computers toevoegen aan het domein en plaats machine principals binnen de organisatie-eenheid die u tijdens het maken van het cluster opgeeft.
- Service-principals binnen de organisatie-eenheid die u tijdens het maken van een cluster opgeeft maken.

> [!NOTE]
> Omdat de domeinnaam Apache Zeppelin gebruikt om de administrative-service-account te verifiëren, moet het serviceaccount dezelfde domeinnaam als de UPN-achtervoegsel voor Apache Zeppelin goed te laten functioneren hebben.

Zie voor meer informatie over de organisatie-eenheden en hoe deze te beheren, [een organisatie-eenheid maken op een AAD-DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md). 

Beveiligde LDAP voor AAD-DS beheerd domein is vereist. Zie voor het inschakelen van beveiligde LDAP [beveiligde LDAP (LDAPS) configureren voor een AAD-DS beheerd domein](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Een domein HDInsight-cluster maken

De volgende stap is het maken van het HDInsight-cluster met behulp van de AAD-DS en het serviceaccount in de vorige sectie hebt gemaakt.

Het is gemakkelijker zowel de AAD-DS en het HDInsight-cluster in de dezelfde Azure virtuele network(VNet) plaatsen. Als u opslaan in verschillende VNets wilt, moet u deze VNets peer zodat HDI VM's een regel zicht naar de domeincontroller hebben voor het lidmaatschap van de virtuele machines. Zie voor meer informatie [virtuele netwerk peering](../../virtual-network/virtual-network-peering-overview.md).

Wanneer u een domein HDInsight-cluster maakt, moet u de volgende parameters opgeven:

- **Domeinnaam**: de domeinnaam die is gekoppeld aan de AAD-DS. Bijvoorbeeld: contoso.onmicrosoft.com
- **Domeingebruikersnaam**: het serviceaccount in het beheerde domein dat is gemaakt in de vorige sectie. Bijvoorbeeld hdiadmin@contoso.onmicrosoft.com. Deze domeingebruiker worden de beheerder van deze HDInsight-cluster.
- **Domeinwachtwoord**: het wachtwoord van het serviceaccount.
- **Organisatie-eenheid**: de DN-naam van de organisatie-eenheid die u wilt gebruiken met HDInsight-cluster. Bijvoorbeeld: OU = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Als deze organisatie-eenheid niet bestaat, probeert de HDInsight-cluster te maken van de organisatie-eenheid met de rechten die de serviceaccount heeft. (Bijvoorbeeld als het serviceaccount zich in de groep Administrators AAD-DS, hieraan de juiste machtigingen voor het maken van een organisatie-eenheid, anders moet u mogelijk eerst de organisatie-eenheid maken en de service account volledige controle over de organisatie-eenheid eerst geven - Zie [een organisatie-eenheid maken op een AAD-DS](../../active-directory-domain-services/active-directory-ds-admin-guide-create-ou.md)).

    > [!IMPORTANT]
    > Het is belangrijk om alle te nemen van de DC's sepearated door een komma na de organisatie-eenheid (bijvoorbeeld OE = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com).

- **LDAPS URL**: bijvoorbeeld ldaps://contoso.onmicrosoft.com:636

    > [!IMPORTANT]
    > Voer de volledige url waaronder de ldaps: / / en het poortnummer (: 636)

- **Gebruikersgroep toegang**: de beveiligingsgroepen waarvan gebruikers die u wilt synchroniseren met het cluster. Bijvoorbeeld: HiveUsers. Als u meerdere gebruikersgroepen opgeven wilt, gescheiden door komma's ','.
 
De volgende schermafbeelding ziet de configuraties in de Azure portal:

![Azure HDInsight domein Active Directory Domain Services-configuratie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor het gebruik van SSH verbinding maken met domein HDInsight-clusters [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).


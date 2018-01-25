---
title: Domein-HDInsight-clusters met behulp van Azure Active Directory Domain Services - Azure configureren | Microsoft Docs
description: Meer informatie over het instellen en domein HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren
services: hdinsight
documentationcenter: 
author: bprakash
manager: jhubbard
editor: cgronlun
tags: 
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 11/10/2017
ms.author: bhanupr
ms.openlocfilehash: 77478616eae27828a57a36dc0aaf3884e80ce403
ms.sourcegitcommit: 28178ca0364e498318e2630f51ba6158e4a09a89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="configure-domain-joined-hdinsight-clusters-using-azure-active-directory-domain-services"></a>Domein-HDInsight-clusters met behulp van Azure Active Directory Domain Services configureren

Domein-clusters bieden de onderneming met meerdere gebruikers beveiligingsmogelijkheden in HDInsight. HDInsight-clusters domein zijn verbonden met active directory-domeinen, zodat gebruikers van een domein hun domeinreferenties gebruiken kunnen voor verificatie met de clusters en big data-taken uitvoeren. 

Er zijn twee manieren voor het instellen van een domeincontroller, zodat een domein HDInsight-cluster verbinding met maken kan:

- Azure Active Directory Domain Services (Azure AD DS)
- Active Directory-domeincontroller op Azure IaaS VM 's

In dit artikel leert u hoe een domein HDInsight-cluster met behulp van Azure Active Directory Domain Services configureren.

## <a name="create-azure-adds"></a>Azure ADDS maken

U moet maken van een Azure AD DS voordat u een HDInsight-cluster kunt maken. Zie voor het maken van een Azure wordt toegevoegd, [inschakelen Azure Active Directory Domain Services met Azure portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

> [!NOTE]
> Alleen de pachterbeheerders hebben de bevoegdheden voor het maken van de domeinservices. Als u Azure Data Lake Storage (ADLS) als de standaard-opslag voor HDInsight, Controleer of dat de standaard Azure AD-tenant voor ADLS is hetzelfde als het domein voor het HDInsight-cluster. 

Nadat de domeinservice is ingericht, moet u voor het maken van een serviceaccount in de **beheerders van Azure AD-DC** groep maken van het HDInsight-cluster. Het serviceaccount moet een globale beheerder op de Azure AD.

## <a name="create-a-domain-joined-hdinsight-cluster"></a>Een domein HDInsight-cluster maken

De volgende stap is het maken van het HDInsight-cluster met behulp van de AAD-DS en het serviceaccount in de vorige sectie hebt gemaakt.

Het is eenvoudiger om de service Azure AD-domein en het HDInsight-cluster in de dezelfde Azure virtuele network(VNet). Als ze zich in verschillende VNets, moet u beide VNets peer. Zie voor meer informatie [virtuele netwerk peering](../../virtual-network/virtual-network-peering-overview.md).

Wanneer u een domein HDInsight-cluster maakt, moet u de volgende parameters opgeven:

- **Domeinnaam**: de domeinnaam die is gekoppeld aan Azure AD DS. Bijvoorbeeld: contoso.onmicrosoft.com
- **Domeingebruikersnaam**: de serviceaccount in de groep beheerders van Azure AD-domeincontroller die is gemaakt in de vorige sectie. Bijvoorbeeld hdiadmin@contoso.onmicrosoft.com. Deze domeingebruiker is de beheerder van dit domein HDInsight-cluster.
- **Domeinwachtwoord**: het wachtwoord van het serviceaccount.
- **Organisatie-eenheid**: de DN-naam van de organisatie-eenheid die u wilt gebruiken met HDInsight-cluster. Bijvoorbeeld: OU = HDInsightOU, DC = contoso, DC = onmicrosohift, DC = com. Als deze organisatie-eenheid niet bestaat, wordt de HDInsight-cluster probeert te maken van deze organisatie-eenheid. 
- **LDAPS URL**: bijvoorbeeld ldaps://contoso.onmicrosoft.com:636
- **Gebruikersgroep toegang**: de beveiligingsgroepen waarvan gebruikers die u wilt synchroniseren met het cluster. Bijvoorbeeld: HiveUsers. Als u meerdere gebruikersgroepen opgeven wilt, gescheiden door komma's ','.
 
De volgende schermafbeelding ziet de configuraties in de Azure portal:

![Azure HDInsight domein Active Directory Domain Services-configuratie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).


## <a name="next-steps"></a>Volgende stappen
* Zie [Configure Hive policies for Domain-joined HDInsight clusters](apache-domain-joined-run-hive.md) (Hive-beleid configureren voor aan een domein gekoppelde HDInsight-clusters) om Hive-beleid te configureren en Hive-query's uit te voeren.
* Zie voor het gebruik van SSH verbinding maken met domein HDInsight-clusters [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).


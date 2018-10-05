---
title: Een HDInsight-cluster configureren met Enterprise-beveiligingspakket met behulp van Azure AD DS-
description: Informatie over het instellen en configureren van een Enterprise-beveiligingspakket van HDInsight-cluster met behulp van Azure Active Directory Domain Services.
services: hdinsight
ms.service: hdinsight
author: omidm1
ms.author: omidm
ms.reviewer: hrasheed
ms.topic: conceptual
ms.date: 10/3/2018
ms.openlocfilehash: 84ee24b9002237d0993a30190944dbd6dd190ac8
ms.sourcegitcommit: 4edf9354a00bb63082c3b844b979165b64f46286
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2018
ms.locfileid: "48784932"
---
# <a name="configure-a-hdinsight-cluster-with-enterprise-security-package-by-using-azure-active-directory-domain-services"></a>Een HDInsight-cluster configureren met Enterprise-beveiligingspakket met behulp van Azure Active Directory Domain Services

Enterprise Security Package (ESP)-clusters bieden toegang door meerdere gebruikers in Azure HDInsight-clusters. HDInsight-clusters met ESP zijn verbonden met een domein, zodat gebruikers van een domein hun domeinreferenties gebruiken kunnen om te verifiëren met de clusters en het uitvoeren van taken met big data. 

In dit artikel leert u hoe u een HDInsight-cluster met ESP configureren met behulp van Azure Active Directory Domain Services (Azure AD-DS).

>[!NOTE]
>ESP is algemeen beschikbaar in HDI 3.6 voor Spark, interactieve en Hadoop. ESP voor HBase en Kafka-cluster is beschikbaar als preview.

## <a name="enable-azure-ad-ds"></a>Inschakelen van Azure AD DS

Inschakelen van DS-AzureAD is een vereiste voordat u een HDInsight-cluster met ESP maken kunt. Zie voor meer informatie, [inschakelen Azure Active Directory Domain Services met behulp van de Azure-portal](../../active-directory-domain-services/active-directory-ds-getting-started.md). 

Wanneer Azure AD DS-is ingeschakeld, start alle gebruikers en objecten synchroniseren uit Azure Active Directory naar Azure AD DS-standaard. De lengte van de synchronisatiebewerking is afhankelijk van het aantal objecten in Azure AD. De synchronisatie kan enkele dagen voor honderden of duizenden objecten krijgen. 

Klanten kunnen kiezen om te synchroniseren van alleen de groepen die toegang nodig tot de HDInsight-clusters. Deze optie alleen bepaalde groepen synchroniseren van de heet *binnen het bereik van synchronisatie*. Zie [configureren binnen het bereik van synchronisatie van Azure AD met uw beheerde domein](https://docs.microsoft.com/en-us/azure/active-directory-domain-services/active-directory-ds-scoped-synchronization) voor instructies.

Nadat u Azure AD DS-inschakelt, wordt een lokale Domain Name Service (DNS)-server wordt uitgevoerd op de AD-virtuele Machines (VM's). Configureer uw Azure AD DS Virtual Network (VNET) voor het gebruik van deze aangepaste DNS-servers. Als u wilt zoeken in de juiste IP-adressen, selecteer **eigenschappen** onder de **beheren** categorie en bekijk de IP-adressen die worden vermeld onder **IP-adres op Virtueelnetwerk**.

![IP-adressen voor de lokale DNS-Servers vinden](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-dns.png)

Wijzig de configuratie van de DNS-servers in het Azure AD DS-VNET aan het gebruik van deze aangepaste IP-adressen door **DNS-Servers** onder de **instellingen** categorie. Klik vervolgens op het keuzerondje bij **aangepaste**, voer het eerste IP-adres in het onderstaande tekstvak en klikt u op **opslaan**. Voeg extra IP-adressen met dezelfde stappen.

![De VNET DNS-configuratie bijwerken](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-vnet-configuration.png)

> [!NOTE]
> Alleen tenantbeheerders hebben de bevoegdheden voor het maken van een Azure AD DS-exemplaar. Multi-factor authentication moet worden uitgeschakeld alleen voor gebruikers die toegang het cluster tot.

Bij het inschakelen van secure LDAP, plaatst u de domeinnaam in de onderwerpnaam of alternatieve onderwerpnaam in het certificaat. Bijvoorbeeld, als uw domeinnaam *contoso.com*, zorg ervoor dat de exacte naam bestaat in de onderwerpnaam of alternatieve naam voor onderwerp. Zie voor meer informatie, [configureren secure LDAP voor een Azure AD DS-domein beheerd](../../active-directory-domain-services/active-directory-ds-admin-guide-configure-secure-ldap.md).

## <a name="check-azure-ad-ds-health-status"></a>Azure AD DS-health-status controleren

De status van uw Azure Active Directory Domain Services weergeven door te selecteren **Health** onder de **beheren** categorie. Zorg ervoor dat de status van Azure AD DS-groen (die wordt uitgevoerd) en de synchronisatie is voltooid.

![Azure Active Directory Domain Services-status](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-health.png)

## <a name="add-managed-identity"></a>Beheerde identiteit toevoegen

Maak een beheerde identiteit voor de gebruiker toegewezen als u er nog geen hebt. Zie [maken, lijst, verwijderen of wijs een rol aan een gebruiker toegewezen beheerde identiteit met Azure portal](https://docs.microsoft.com/en-us/azure/active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal) voor instructies. 

De beheerde identiteit wordt gebruikt om domain services-bewerkingen te vereenvoudigen. Deze identiteit heeft toegang tot het lezen, maken, wijzigen en verwijderen van domain services-bewerkingen die nodig zijn voor de HDInsight Enterprise-beveiligingspakket, zoals het maken van de organisatie-eenheden en principes van de service.

Nadat u Azure AD DS-hebt ingeschakeld, maken van een gebruiker toegewezen beheerde identiteit en wijs deze toe aan de **HDInsight Domain Services Inzender** rol in Azure AD DS-toegangsbeheer.

![Active Directory Domain Services Access control van Azure](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-configure-managed-identity.png)

Toewijzen van een beheerde identiteit op de **HDInsight Domain Services Inzender** rol zorgt ervoor dat de identiteit van de juiste toegang heeft tot bepaalde domain services bewerkingen uitvoeren op het Azure AD DS-domein. Zie voor meer informatie, [wat is beheerde identiteiten voor Azure-resources](../../active-directory/managed-identities-azure-resources/overview.md).

## <a name="create-a-hdinsight-cluster-with-esp"></a>Een HDInsight-cluster maken met ESP

De volgende stap is het maken van het HDInsight-cluster met ESP ingeschakeld met behulp van Azure AD DS.

Het is eenvoudiger om zowel de Azure AD DS-exemplaar als het HDInsight-cluster in hetzelfde Azure virtual network. Als ze zich in verschillende virtuele netwerken, moet u deze virtuele netwerken koppelen zodat HDInsight VM's zichtbaar voor de domeincontroller zijn en kunnen worden toegevoegd aan het domein. Zie voor meer informatie, [peering van virtuele netwerken](../../virtual-network/virtual-network-peering-overview.md). Als u wilt testen, als u peering correct is uitgevoerd, een virtuele machine toevoegen aan het HDInsight VNET/Subnet en ping de domeinnaam of uitvoeren **ldp.exe** voor toegang tot Azure AD DS-domein.

Nadat de VNETs zijn gekoppeld, configureert u het HDInsight VNET voor het gebruik van een aangepaste DNS-server en voer de privé-IP's van Azure AD DS-als de adressen van de DNS-server. Wanneer beide VNETs de dezelfde DNS-servers gebruikt, wordt de naam van uw aangepaste domein wordt omgezet in het juiste IP-adres en bereikbaar is vanuit HDInsight. Bijvoorbeeld als naam van het domein 'contoso.com' vervolgens na deze stap wordt niet pingen 'contoso.com' moet worden omgezet naar het recht voor IP-Adressen van Azure AD DS. Vervolgens kunt u een virtuele machine toevoegen aan dit domein.

![Aangepaste DNS-Servers configureren voor het gekoppelde VNET](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-aadds-peered-vnet-configuration.png)

Wanneer u een HDInsight-cluster maakt, kunt u de Enterprise-beveiligingspakket inschakelen in het aangepaste tabblad.

![Azure HDInsight-beveiliging en netwerken](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-security-networking.png)

Zodra u ESP inschakelt, worden veelvoorkomende onjuiste configuraties die zijn gerelateerd aan Azure AD DS-automatisch gedetecteerd en gevalideerd.

![Azure HDInsight Enterprise security package-domeinvalidatie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate.png)

Vroegtijdig bespaart u tijd doordat u kunt fouten corrigeren voordat u het cluster te maken.

![Azure HDInsight Enterprise-beveiligingspakket domeinvalidatie is mislukt](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-create-cluster-esp-domain-validate-failed.png)

Wanneer u een HDInsight-cluster met ESP maken, moet u de volgende parameters opgeven:

- **Gebruiker met beheerdersrechten cluster**: Kies een beheerder voor uw cluster vanaf uw gesynchroniseerde Azure AD DS-. Dit account moet al zijn gesynchroniseerd en beschikbaar in Azure AD DS.

- **Cluster-toegangsgroepen**: de waarvan gebruikers die u wilt synchroniseren met het cluster beschikbaar zijn in Azure AD DS moet-beveiligingsgroepen. Bijvoorbeeld: HiveUsers groep. Zie voor meer informatie, [een groep maken en leden toevoegen in Azure Active Directory](../../active-directory/fundamentals/active-directory-groups-create-azure-portal.md).

- **LDAPS-URL**: een voorbeeld is ldaps://contoso.com:636.

De volgende schermafbeelding ziet u een juiste configuratie in Azure portal:

![Azure HDInsight ESP Active Directory Domain Services-configuratie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-domain-joined-configuration-azure-aads-portal.png).

De beheerde identiteit die u hebt gemaakt kan worden gekozen in de vervolgkeuzelijst beheerde identiteit gebruiker toegewezen bij het maken van een nieuw cluster.

![Azure HDInsight ESP Active Directory Domain Services-configuratie](./media/apache-domain-joined-configure-using-azure-adds/hdinsight-identity-managed-identity.png).


## <a name="next-steps"></a>Volgende stappen
* Zie voor Hive-beleid configureren en uitvoeren van Hive-query's, [configureren Hive-beleid voor HDInsight-clusters met ESP](apache-domain-joined-run-hive.md).
* Zie voor het gebruik van SSH verbinding maken met HDInsight-clusters met ESP [SSH gebruiken met Hadoop op basis van Linux in HDInsight via Linux, Unix of OS X](../hdinsight-hadoop-linux-use-ssh-unix.md#domainjoined).

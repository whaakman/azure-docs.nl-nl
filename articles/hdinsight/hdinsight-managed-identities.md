---
title: Beheerde identiteiten in Azure HDInsight
description: Bevat een overzicht van de implementatie van beheerde identiteiten in Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.service: hdinsight
ms.custom: hdinsightactive
ms.topic: conceptual
ms.date: 03/12/2019
ms.author: hrasheed
ms.openlocfilehash: 9386bcb8e455bff5ceed1fccdf55874caf7b6507
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58175783"
---
# <a name="managed-identities-in-azure-hdinsight"></a>Beheerde identiteiten in Azure HDInsight

Een beheerde identiteit is een identiteit die is geregistreerd bij Azure Active Directory (Azure AD) waarvan de referenties worden beheerd door Azure. Met beheerde identiteiten moet u geen service-principals in Azure AD registreren of beheren van referenties, zoals certificaten.

Beheerde identiteiten kunnen worden gebruikt in Azure HDInsight om toe te staan uw clusters toegang krijgen tot Azure AD domain services, toegang tot Azure Key Vault of toegang tot bestanden in Azure Data Lake Storage Gen2.

Er zijn twee soorten beheerde identiteiten: gebruiker toegewezen en het systeem is toegewezen. Azure HDInsight maakt gebruik van beheerde identiteiten gebruiker toegewezen. Een gebruiker toegewezen beheerde identiteit wordt gemaakt als zelfstandige Azure-resource die u vervolgens aan een of meer Azure-service-exemplaren toewijzen kunt. Daarentegen is een beheerde identiteit voor het systeem toegewezen in Azure AD hebt gemaakt en vervolgens automatisch rechtstreeks op een bepaald Azure-service-exemplaar is ingeschakeld. De levensduur van die door het systeem toegewezen beheerde identiteit wordt vervolgens gekoppeld aan de levensduur van de service-exemplaar dat ingeschakeld op.

## <a name="hdinsight-managed-identity-implementation"></a>Implementatie van HDInsight beheerde identiteit

In Azure HDInsight, worden beheerde identiteiten ingericht op elk knooppunt van het cluster. Deze identiteitsonderdelen zijn echter alleen kan worden gebruikt door de HDInsight-service. Er is momenteel geen ondersteunde methode voor het genereren van tokens voor toegang met behulp van de beheerde identiteiten op HDInsight-clusterknooppunten is geïnstalleerd. Beheerde identiteiten zijn voor sommige Azure-services geïmplementeerd met een eindpunt dat u gebruiken kunt voor het verkrijgen van toegang tot tokens voor interactie met andere Azure-services op uw eigen.

## <a name="create-a-managed-identity"></a>Maken van een beheerde identiteit

Beheerde identiteiten kunnen worden gemaakt met een van de volgende methoden:

* [Azure-portal](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-portal.md)
* [Azure PowerShell](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-powershell.md)
* [Azure Resource Manager](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-arm.md)
* [Azure-CLI](../active-directory/managed-identities-azure-resources/how-to-manage-ua-identity-cli.md)

De resterende stappen voor het configureren van de beheerde identiteit, is afhankelijk van het scenario waarin deze worden gebruikt.

## <a name="managed-identity-scenarios-in-azure-hdinsight"></a>Scenario's voor beheerde identiteit in Azure HDInsight

Beheerde identiteiten worden gebruikt in Azure HDInsight in meerdere scenario's. Zie de gerelateerde documenten voor de gedetailleerde installatie en configuratie-instructies:

* [Azure Data Lake Storage Gen2](hdinsight-hadoop-use-data-lake-storage-gen2.md#create-a-user-managed-identity)
* [Enterprise Security Package](domain-joined/apache-domain-joined-configure-using-azure-adds.md#create-and-authorize-a-managed-identity)
* [Kafka Bring Your Own Key (BYOK)](kafka/apache-kafka-byok.md#get-started-with-byok)

## <a name="next-steps"></a>Volgende stappen

* [Wat zijn beheerde identiteiten voor Azure-resources?](../active-directory/managed-identities-azure-resources/overview.md)
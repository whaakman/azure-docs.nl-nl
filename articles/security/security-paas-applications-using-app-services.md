---
title: Beveiligen van PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure App Service | Microsoft Docs
description: " Meer informatie over Azure App Service security best practices voor het beveiligen van uw PaaS-web- en mobiele toepassingen. "
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/21/2017
ms.author: terrylan
ms.openlocfilehash: dec45d91ad1a73306b3e2656dd9bf7fdbe456720
ms.sourcegitcommit: 62eaa376437687de4ef2e325ac3d7e195d158f9f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2017
---
# <a name="securing-paas-web-and-mobile-applications-using-azure-app-service"></a>Beveiligen van PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure App Service

In dit artikel bespreken we een verzameling van [Azure App Service](https://azure.microsoft.com/services/app-service/) aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure en de ervaringen van klanten zoals zelf.

## <a name="azure-app-service"></a>Azure App Service
[Azure App Service](../app-service/app-service-web-overview.md) is een PaaS die aanbieding kunt u web- en mobiele apps voor alle platforms en apparaten maken en verbinding maken met gegevens overal en in de cloud of on-premises. App Service omvat het web en mobiel mogelijkheden die werden eerder afzonderlijk verkrijgbaar Azure Websites en Azure Mobile Services. Deze service bevat ook nieuwe mogelijkheden voor het automatiseren van bedrijfsprocessen en het hosten van cloud-API's. App Service wordt als een geïntegreerde service, een uitgebreide reeks mogelijkheden voor web-, mobiele en integratiescenario's.

## <a name="best-practices"></a>Aanbevolen procedures

Wanneer u App Service, volgt u deze aanbevolen procedures:

- [Verifiëren via Azure Active Directory (AD)](../app-service/app-service-authentication-overview.md). App Service biedt een OAuth 2.0-service voor uw id-provider. OAuth 2.0 richt zich op de client developer eenvoud tijdens het ontwikkelen van specifieke autorisatie stromen voor webtoepassingen, bureaubladtoepassingen en mobiele telefoons. Azure AD maakt gebruik van OAuth 2.0, zodat u toegang tot mobiele en webtoepassingen autoriseren.
- De toegang op basis van de noodzaak om te weten en minimale bevoegdheden beveiligingsprincipes beperken. Beperken van de toegang is van cruciaal belang voor organisaties die willen beveiligingsbeleid instellen voor toegang tot gegevens. Op rollen gebaseerde toegangsbeheer (RBAC) kan worden gebruikt om machtigingen te wijzen aan gebruikers, groepen en toepassingen op een bepaalde scope. Zie voor meer informatie over gebruikers geen toegang verlenen tot toepassingen, [aan de slag met toegangsbeheer](../active-directory/role-based-access-control-what-is.md).
- Beveiligen van uw sleutels. Het maakt niet uit hoe goed is de beveiliging van uw als u uw abonnement sleutels verliest. Met Azure Key Vault kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. U kunt met Key Vault sleutels en geheimen versleutelen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels, PFX-bestanden en wachtwoorden) door gebruik te maken van sleutels die worden beveiligd met HSM's. Voor extra zekerheid kunt u de sleutels importeren of genereren in HSM's. Zie [Azure Key Vault](../key-vault/key-vault-whatis.md) voor meer informatie. U kunt ook Sleutelkluis gebruiken voor het beheren van de TLS-certificaten met automatische verlenging.
- Binnenkomende bron-IP-adressen beperken. [App Service-omgeving](../app-service/environment/intro.md) beschikt over een virtueel netwerk integratie-functie waarmee u binnenkomende bron-IP-adressen via netwerkbeveiligingsgroepen (nsg's) beperken. Als u niet bekend bent met Azure Virtual Networks (vnet's), is dit een functie waarmee u veel van uw Azure-resources in een internet-routeerbaar netwerk die u toegang tot te plaatsen. Zie voor meer informatie, [uw app integreren met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Volgende stappen
In dit artikel is geïntroduceerd aan een verzameling van App Service aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Zie voor meer informatie over het beveiligen van uw PaaS-implementaties:

- [PaaS-implementaties beveiligen](security-paas-deployments.md)
- [PaaS-webtoepassingen en mobiele toepassingen met behulp van Azure SQL Database en SQL Data Warehouse beveiligen](security-paas-applications-using-sql.md)

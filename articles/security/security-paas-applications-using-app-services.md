---
title: PaaS-web- en mobiele toepassingen met Azure App Service beveiligen | Microsoft Docs
description: 'Meer informatie over de beveiliging van Azure App Service aanbevolen procedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. '
services: security
documentationcenter: na
author: techlake
manager: MBaldwin
editor: ''
ms.assetid: ''
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/28/2018
ms.author: terrylan
ms.openlocfilehash: bece63c36bf0027524dea051e78d290c407ca349
ms.sourcegitcommit: 549070d281bb2b5bf282bc7d46f6feab337ef248
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/21/2018
ms.locfileid: "53716086"
---
# <a name="best-practices-for-securing-paas-web-and-mobile-applications-using-azure-app-service"></a>Aanbevolen procedures voor het beveiligen van PaaS-webtoepassingen en mobiele toepassingen met Azure App Service

In dit artikel bespreken we een verzameling van [Azure App Service](../app-service/overview.md) aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Deze aanbevolen procedures zijn afgeleid van onze ervaring met Azure en de ervaringen van klanten zoals zelf.

Azure App Service is een platform-as-a-service (PaaS) aanbieding waarmee u web- en mobiele apps voor elk platform of apparaat maken en verbinding maken met gegevens waar dan ook, in de cloud of on-premises. App Service bevat de web- en mobiele mogelijkheden die zijn eerder afzonderlijk verkrijgbaar waren in Azure Websites en Azure Mobile Services. Deze service bevat ook nieuwe mogelijkheden voor het automatiseren van bedrijfsprocessen en het hosten van cloud-API's. Als een geïntegreerde service heeft App Service een uitgebreide set mogelijkheden voor het web, mobiele en integratiescenario's.

## <a name="authenticate-through-azure-active-directory-ad"></a>Verificatie via Azure Active Directory (AD)
App Service biedt een OAuth 2.0-service voor uw id-provider. OAuth 2.0 is gericht op client-ontwikkelaar eenvoudig te houden terwijl er specifieke autorisatiestromen voor webtoepassingen, bureaubladtoepassingen en mobiele telefoons. Azure AD maakt gebruik van OAuth 2.0 waarmee u kunt toestaan dat de toegang tot mobiele en webtoepassingen. Zie voor meer informatie, [verificatie en autorisatie in Azure App Service](../app-service/overview-authentication-authorization.md).

## <a name="restrict-access-based-on-role"></a>Beperken van toegang op basis van rollen 
Beperken van de toegang is van cruciaal belang voor organisaties die willen beveiligingsbeleid voor toegang tot gegevens afdwingen. Op rollen gebaseerd toegangsbeheer (RBAC) kunt u machtigingen toewijzen aan gebruikers, groepen en toepassingen met een bepaald bereik, zoals de noodzaak om te weten en minimale bevoegdheden beveiligingsprincipes. Zie voor meer informatie over het verlenen van toegang tot toepassingen voor gebruikers, [wat is op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md).

## <a name="protect-your-keys"></a>Beveiligen van uw sleutels
Het maakt niet uit hoe goed uw beveiliging is als u uw abonnementssleutels verliest. Met Azure Sleutelkluis kunt u de cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services worden gebruikt. Met Key Vault, kunt u sleutels en geheimen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels, versleutelen. PFX-bestanden en wachtwoorden) met behulp van sleutels die worden beveiligd door hardware security modules (HSM's). Voor extra zekerheid kunt u de sleutels importeren of genereren in HSM's. U kunt ook de Key Vault gebruiken voor het beheren van de TLS-certificaten met automatische verlenging. Zie [wat is Azure Key Vault](../key-vault/key-vault-whatis.md) voor meer informatie. 

## <a name="restrict-incoming-source-ip-addresses"></a>Binnenkomende bron-IP-adressen beperken
[App Service-omgevingen](../app-service/environment/intro.md) heeft een virtueel netwerk-integratie-functie waarmee u beperkingen instellen voor binnenkomende bron-IP-adressen via netwerkbeveiligingsgroepen (nsg's). Als u niet bekend bent met Azure-netwerken (VNETs), is dit een mogelijkheid waarmee u veel van uw Azure-resources in een internet-, routeerbaar netwerk dat u toegang tot plaatsen. Zie voor meer informatie, [uw app integreren met een Azure Virtual Network](../app-service/web-sites-integrate-with-vnet.md).

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kennisgemaakt met een verzameling van App Service aanbevolen beveiligingsprocedures voor het beveiligen van uw PaaS-webtoepassingen en mobiele toepassingen. Zie voor meer informatie over het beveiligen van uw PaaS-implementaties:

- [PaaS-implementaties beveiligen](security-paas-deployments.md)
- [PaaS-databases in Azure beveiligen](security-paas-applications-using-sql.md)

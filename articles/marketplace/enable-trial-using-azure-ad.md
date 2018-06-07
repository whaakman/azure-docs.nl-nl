---
title: Proefversie gebruikmaken van Azure AD inschakelen | Azure
description: Vermeldingstype proefversie met Azure Active Directory (Azure AD) in Azure Marketplace en AppSource voor uitgevers van de app en service inschakelen
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/04/2018
ms.author: ellacroi
ms.openlocfilehash: 4140ba98c0c65c22674c61dc7266818af904e777
ms.sourcegitcommit: 3017211a7d51efd6cd87e8210ee13d57585c7e3b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/06/2018
ms.locfileid: "34825116"
---
# <a name="enable-trial-using-azure-ad"></a>Proefversie gebruikmaken van Azure AD inschakelen  
Azure Active Directory (Azure AD) is een cloud identity-service waarmee verificatie werkt met een Microsoft- of schoolaccount met behulp van industriestandaard frameworks: OAuth en OpenID Connect.  
*   Voor meer informatie over Azure AD, gaat u naar de Azure Active Directory pagina zich bevindt op [azure.microsoft.com/services/active-directory](https://azure.microsoft.com/services/active-directory).  

U en uw klanten zijn op de marketplace gebruikmaken van Azure AD geverifieerd. Nadat uw klant uw aanbieding op marketplace evaluatieversie selecteert, wordt uw klant omgeleid naar uw proefabonnement omgeving.  In uw omgeving proefversie, mag u uw klant instellen zonder aanvullende stappen voor aanmelden. Uw app of de aanbieding ontvangt een token van Azure AD tijdens de verificatie die waardevolle gebruikersgegevens voor het maken van een gebruikersaccount in uw app of de aanbieding bevat. U kunt uw installatie automatiseren en vergroot de kans van conversie.  
*   Voor meer informatie over het token dat is verzonden vanaf de Azure AD tijdens de verificatie, gaat u naar de voorbeeld-Tokens sectie zich bevindt op [docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens](https://docs.microsoft.com/azure/active-directory/develop/active-directory-token-and-claims#sample-tokens)

Azure AD gebruikt één muisklik verificatie in uw app of de evaluatieversie in te schakelen.  
*   De gebruikerservaring van marketplace evaluatieversie stroomlijnen.  
*   Onderhouden de werking van een product-ervaring, zelfs wanneer de gebruiker wordt omgeleid in de Marketplace aan uw domein of de proefversie-omgeving.  
*   De kans stopzetting op omleiding, omdat er geen extra stappen aanmelden.  
*   Verminder implementatie barrières voor het vullen van de grote van Azure AD-gebruikers.  

## <a name="verify-your-azure-ad-integration-on-the-marketplace-multitenant-apps"></a>Controleer of uw Azure AD-integratie voor de marketplace: Multitenant-Apps  
Ondersteuning voor de volgende opties voor uw oplossing met behulp van Azure AD.  
*   Uw app registreren in de winkelobjecten op marketplace.  
*   De ondersteuning voor multitenancy inschakelen in Azure AD voor een proefabonnement ervaring met één klik.  
    *   Voor meer informatie over app-registratie, gaat u naar de toepassingen integratie met Azure Active Directory pagina zich bevindt op [docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications).  

Als u niet bekend bent voor het gebruik van Azure AD voor federatieve eenmalige aanmelding (SSO) en vervolgens als volgt te werk.  
1.  Registreren van uw app in de marketplace. 
2.  Eenmalige aanmelding met Azure AD ontwikkelen met behulp van OAuth 2.0- of OpenID Connect.  
    *   Voor meer informatie over het OAuth 2.0, gaat u naar het OAuth 2.0 pagina zich bevindt op [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-oauth-code).  
    *   Voor meer informatie over het Open ID Connect, gaat u naar het OpenID Connect pagina zich bevindt op [docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code](https://docs.microsoft.com/azure/active-directory/develop/active-directory-protocols-openid-connect-code).  
3.  De ondersteuning voor multitenancy inschakelen in Azure AD om een proef één muisklik-ervaring te bieden.  
    *   Voor meer informatie over AppSource certificering, gaat u naar de pagina van de certificeringsinstantie zich bevindt op AppSource [docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-appsource-certified). 

## <a name="verify-your-azure-ad-integration-on-the-marketplace-single-tenant-apps"></a>Controleer of uw Azure AD-integratie voor de marketplace: Single-Tenant-Apps  
Ondersteuning voor een van de volgende opties voor uw oplossing voor één tenant.  
*   Gebruikers toevoegen aan uw directory als gastgebruikers met behulp van Azure AD B2B.  
    *   Voor meer informatie over Azure AD B2B, gaat u naar de wat is Azure AD B2B-samenwerking pagina zich bevindt op [docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b).
*   Proefversies voor klanten die gebruikmaken van contact met mij handmatig instellen  
*   Ontwikkel een station van de test per klant.  
*   Een voorbeeld van multitenant-demo-app met eenmalige aanmelding bouwen.  

## <a name="next-steps"></a>Volgende stappen
*   Ga naar de [AppSource Publisher handleiding en Azure Marketplace](./marketplace-publishers-guide.md) pagina.  
 
---  


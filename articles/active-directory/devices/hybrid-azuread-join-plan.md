---
title: Hybride Azure Active Directory-gekoppelde apparaten configureren| Microsoft Docs
description: Lees hoe u hybride Azure Active Directory-gekoppelde apparaten kunt configureren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.component: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2018
ms.author: markvi
ms.reviewer: sandeo
ms.openlocfilehash: b22f79195a7246c87a8d5d5b4b5e012cc30a62dd
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53274561"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Hoe u uw hybride Azure Active Directory join-implementatie plannen

Net zoals een gebruiker, wordt een apparaat ook een identiteit die u wilt beschermen en die u wilt gebruiken om uw bronnen altijd en overal te beschermen. U kunt dit doel bereiken door de identiteiten van uw apparaten naar Azure AD te brengen met een van de volgende methoden:

- Azure AD-koppeling
- Hybride Azure AD-koppeling
- Azure AD-registratie

Door uw apparaten naar Azure AD te brengen, optimaliseert u de productiviteit van uw gebruikers via eenmalige aanmelding (SSO) bij al uw on-premises bronnen en cloudbronnen. Tegelijk kunt u de toegang tot uw on-premises bronnen en cloudbronnen beveiligen met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

Als u een on-premises Active Directory-omgeving hebt en u uw domein-gekoppelde apparaten aan Azure AD wilt koppelen, kunt u dit doen door hybride Azure AD-gekoppelde apparaten te configureren. Dit artikel vindt u met de bijbehorende stappen voor het implementeren van een hybride Azure AD join in uw omgeving. 


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent de [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md).


## <a name="plan-your-implementation"></a>Uw implementatie plannen

Als u wilt uw hybride Azure AD-implementatie te plannen, moet u vertrouwd raken met:

|   |   |
|---|---|
|![Selecteren][1]|Apparaten controleren die worden ondersteund|
|![Selecteren][1]|Lees wat die u moet weten|
|![Selecteren][1]|Lees hoe u de hybride Azure AD join van uw apparaten beheren|
|![Selecteren][1]|Selecteer uw scenario|


 


## <a name="review-supported-devices"></a>Apparaten controleren die worden ondersteund 

Hybride Azure AD join ondersteunt een breed bereik van de Windows-apparaten. Omdat de configuratie voor apparaten met oudere versies van Windows voor aanvullende of andere stappen vereist, worden de ondersteunde apparaten gegroepeerd in twee categorieën:

**Huidige Windows-apparaten**

- Windows 10
    
- Windows Server 2016


Voor apparaten met het besturingssysteem voor Windows-bureaublad, de ondersteunde versie is de Windows 10 Jubileumupdate (versie 1607) of hoger. Als een best practice, een upgrade uitvoeren naar de nieuwste versie van Windows 10.



 **Windows downlevel-apparaten**

- Windows 8.1
 
- Windows 7

- Windows Server 2012 R2
 
- Windows Server 2012 
 
- Windows Server 2008 R2 


U moet als eerste stap planning uw omgeving controleren en bepalen of u nodig hebt om Windows downlevel-apparaten te ondersteunen.



## <a name="review-things-you-should-know"></a>Lees wat die u moet weten

U kunt een hybride Azure AD join niet gebruiken als uw omgeving bestaat uit één forest die identiteitsgegevens in meer dan één Azure AD-tenant gesynchroniseerd.

Als u het hulpprogramma voor systeemvoorbereiding (Sysprep) zijn afhankelijk, zorg er dan voor dat u installatiekopieën maken van een installatie van Windows die niet is geconfigureerd voor de hybride Azure AD join.

Als u al op de momentopname van een virtuele Machine (VM vertrouwen,) te maken van aanvullende virtuele machines, zorg er dan voor dat u een momentopname van een virtuele machine die niet is geconfigureerd voor de hybride Azure AD join.

Hybrid Azure AD join van Windows downlevel-apparaten:

- **Is** ondersteund in omgevingen met niet-gefedereerde [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 

- **Is niet** ondersteund bij het gebruik van Azure AD Pass-through-verificatie zonder naadloze eenmalige aanmelding.

- **Is niet** ondersteund bij gebruik van zwervende referenties of gebruikersprofiel voor roaming of bij het gebruik van virtuele desktopinfrastructuur (VDI).


De registratie van Windows Server met de rol van domeincontroller (DC) wordt niet ondersteund.

Als uw organisatie internettoegang via een geverifieerde uitgaande proxy vereist, moet u ervoor zorgen dat uw Windows 10-computers succesvol kunnen verifiëren bij de uitgaande proxy. Omdat Windows 10-computers apparaatregistratie uitvoeren via machinecontext, moet u verificatie van een uitgaande proxy configureren via machinecontext.


Hybride Azure AD join is een proces naar uw on-premises domein apparaten automatisch wordt geregistreerd bij Azure AD. Er zijn gevallen waarin u niet wilt dat al uw apparaten automatisch te registreren. Als dit het geval is, raadpleegt u [over het beheren van de hybride Azure AD join van uw apparaten](hybrid-azuread-join-control.md).

## <a name="review-how-to-control-the-hybrid-azure-ad-join-of-your-devices"></a>Lees hoe u de hybride Azure AD join van uw apparaten beheren

Hybride Azure AD join is een proces naar uw on-premises domein apparaten automatisch wordt geregistreerd bij Azure AD. Er zijn gevallen waarin u niet wilt dat al uw apparaten automatisch te registreren. Dit is voor voorbeeld true, wordt tijdens de initiële implementatie om te controleren of alles werkt zoals verwacht.

Zie voor meer informatie, [de hybride Azure AD join van uw apparaten beheren](hybrid-azuread-join-control.md)

## <a name="select-your-scenario"></a>Selecteer uw scenario

U kunt lid worden van hybride Azure AD voor de volgende scenario's kunt configureren:

- Beheerde domeinen
- Federatieve domeinen  



Als uw omgeving heeft beheerde domeinen, ondersteunt hybride Azure AD join:

- Doorgeven van verificatie (PTA)

- Synchronisatie van Wachtwoordhashes (WHS)

Vanaf versie 1.1.819.0 bevat Azure AD Connect een wizard om hybride Azure AD-koppeling te configureren. Met de wizard kunt u het configuratieproces aanzienlijk vereenvoudigen. Zie voor meer informatie:

- [Hybride Azure Active Directory-deelname configureren voor federatieve domeinen](hybrid-azuread-join-federated-domains.md)


- [Hybride Azure Active Directory-deelname configureren voor beheerde domeinen](hybrid-azuread-join-managed-domains.md)


 Als het installeren van de vereiste versie van Azure AD Connect kan niet worden gebruikt voor u, raadpleegt u [het handmatig configureren van device Registration service](../device-management-hybrid-azuread-joined-devices-setup.md). 


## <a name="alternate-login-id-support-in-hybrid-azure-ad-join"></a>Ondersteuning voor alternatieve aanmeldings-Id in hybride Azure AD join

Windows 10 hybride Azure AD join biedt beperkte ondersteuning voor [alternatieve aanmeldings-id's](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/configuring-alternate-login-id) op basis van het type van de alternatieve aanmeldings-id, [verificatiemethode](https://docs.microsoft.com/en-us/azure/security/azure-ad-choose-authn), domeintype en Windows 10-versie. Er zijn twee soort alternatieve aanmeldings-id's die kunnen optreden in uw omgeving.

 - Routeerbaar alternatieve aanmeldings-id: Een routeerbaar alternatieve aanmeldings-id heeft een ongeldig geverifieerd domein, die is geregistreerd bij een domeinregistrar. Bijvoorbeeld, als het primaire domein contoso.com is contoso.org en contoso.co.uk zijn geldige domeinen die eigendom zijn van Contoso en [geverifieerd in Azure AD](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/add-custom-domain)
 
 - Niet-routeerbare alternatieve aanmeldings-id: Een niet-routeerbare alternatieve aanmeldings-id beschikt niet over een geverifieerd domein. Het is van toepassing alleen binnen het particuliere netwerk van uw organisatie. Bijvoorbeeld, als het primaire domein contoso.com is contoso.local is niet een geverifieerd domein in het internet, maar binnen Contoso netwerk wordt gebruikt.
 
De onderstaande tabel bevat informatie over ondersteuning voor een van deze alternatieve aanmeldings-id's in Windows 10 Hybrid Azure AD join

|Type van de alternatieve aanmeldings-id|Domeintype|Windows 10-versie|Description|
|-----|-----|-----|-----|
|Routeerbaar|Federatief |Vanaf versie 1703|Algemeen beschikbaar|
|Routeerbaar|Managed|Vanaf versie 1709|Momenteel in private preview. Azure AD SSPR wordt niet ondersteund. |
|Niet-routeerbare|Federatief|Vanaf versie 1803|Algemeen beschikbaar|
|Niet-routeerbare|Managed|Niet ondersteund||



## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Configure hybride Azure Active Directory join voor federatieve domeinen](hybrid-azuread-join-federated-domains.md)
> [configureren hybride Azure Active Directory join voor beheerde domeinen](hybrid-azuread-join-managed-domains.md)




<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png

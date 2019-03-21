---
title: Het plannen van hybride Azure Active Directory join-implementatie in Azure Active Directory (Azure AD) | Microsoft Docs
description: Lees hoe u hybride Azure Active Directory-gekoppelde apparaten kunt configureren.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 54e1b01b-03ee-4c46-bcf0-e01affc0419d
ms.service: active-directory
ms.subservice: devices
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2019
ms.author: markvi
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: de80825ccdd331f57dcd31d307196dc0b45b9cc9
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58294583"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Procedure: Uw hybride Azure Active Directory join-implementatie plannen

Net zoals een gebruiker, wordt een apparaat ook een identiteit die u wilt beschermen en die u wilt gebruiken om uw bronnen altijd en overal te beschermen. U kunt dit doel bereiken door de identiteiten van uw apparaten naar Azure AD te brengen met een van de volgende methoden:

- Azure AD-koppeling
- Hybride Azure AD-koppeling
- Azure AD-registratie

Door uw apparaten naar Azure AD te brengen, optimaliseert u de productiviteit van uw gebruikers via eenmalige aanmelding (SSO) bij al uw on-premises bronnen en cloudbronnen. Tegelijk kunt u de toegang tot uw on-premises bronnen en cloudbronnen beveiligen met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

Als u een on-premises Active Directory-omgeving hebt en u uw domein-gekoppelde apparaten aan Azure AD wilt koppelen, kunt u dit doen door hybride Azure AD-gekoppelde apparaten te configureren. Dit artikel vindt u met de bijbehorende stappen voor het implementeren van een hybride Azure AD join in uw omgeving. 


## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent de [Inleiding tot Apparaatbeheer in Azure Active Directory](../device-management-introduction.md).

>[!NOTE]
>  De minimaal vereiste functionele domein en forest functionele niveaus voor Windows 10 hybride Azure AD join Windows Server 2008 R2 is. Op de lagere versies, de gebruiker kan niet een primaire vernieuwen-Token ophalen tijdens het aanmelden vanwege problemen met de LSA Windows 

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

Als u het hulpprogramma voor systeemvoorbereiding (Sysprep) zijn afhankelijk, zorg ervoor dat installatiekopieën die zijn gemaakt op basis van een installatie van Windows 10-1803 of eerder zijn niet geconfigureerd voor de hybride Azure AD join.

Als u al op de momentopname van een virtuele Machine (VM vertrouwen,) te maken van aanvullende virtuele machines, zorg er dan voor dat u een momentopname van een virtuele machine die niet is geconfigureerd voor de hybride Azure AD join.

Hybrid Azure AD join van Windows downlevel-apparaten:

- **Is** ondersteund in omgevingen met niet-gefedereerde [Azure Active Directory naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso-quick-start). 

- **Is niet** ondersteund bij het gebruik van Azure AD Pass-through-verificatie zonder naadloze eenmalige aanmelding.

- **Is niet** ondersteund bij gebruik van zwervende referenties of gebruikersprofiel voor roaming of bij het gebruik van virtuele desktopinfrastructuur (VDI).


De registratie van Windows Server met de rol van domeincontroller (DC) wordt niet ondersteund.

Als uw organisatie internettoegang via een geverifieerde uitgaande proxy vereist, moet u ervoor zorgen dat uw Windows 10-computers succesvol kunnen verifiëren bij de uitgaande proxy. Omdat Windows 10-computers apparaatregistratie uitvoeren via machinecontext, moet u verificatie van een uitgaande proxy configureren via machinecontext.


Hybride Azure AD join is een proces naar uw on-premises domein apparaten automatisch wordt geregistreerd bij Azure AD. Er zijn gevallen waarin u niet wilt dat al uw apparaten automatisch te registreren. Als dit het geval is, raadpleegt u [over het beheren van de hybride Azure AD join van uw apparaten](hybrid-azuread-join-control.md).

Als uw Windows 10 domein apparaten al zijn [geregistreerd bij Azure AD](https://docs.microsoft.com/azure/active-directory/devices/overview#azure-ad-registered-devices) uw tenant, is het raadzaam deze status verwijderen voordat u Hybrid Azure AD join inschakelt. Van Windows 10 1809 release, zijn de volgende wijzigingen aangebracht om te voorkomen dat deze twee status: 
 - Eventuele bestaande geregistreerd bij Azure AD-status zou worden automatisch verwijderd nadat het apparaat is toegevoegd aan Hybrid Azure AD. 
 - U kunt voorkomen dat uw apparaat dat lid is domein wordt Azure AD geregistreerd door toe te voegen deze registersleutel - HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin" = dword: 00000001.
 - Deze wijziging is nu beschikbaar voor Windows 10-1803 release met KB4489894.

FIPS-compatibele TPM's worden niet ondersteund voor hybride Azure AD join. Als uw apparaten FIPS-compatibele TPM's hebt, moet u deze uitschakelen voordat u doorgaat met Hybrid Azure AD join. Microsoft biedt niet alle hulpprogramma's voor het uitschakelen van FIPS-modus voor TPM's, zoals dit afhankelijk van de TPM-fabrikant is. Neem contact op met uw OEM-hardware voor ondersteuning.

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


 Als het installeren van de vereiste versie van Azure AD Connect kan niet worden gebruikt voor u, raadpleegt u [het handmatig configureren van device Registration service](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual). 


## <a name="on-premises-ad-upn-support-in-hybrid-azure-ad-join"></a>Ondersteuning voor on-premises AD UPN in hybride Azure AD join

Soms, uw on-premises AD UPN's kunnen afwijken van uw Azure AD UPN's. In dergelijke gevallen, Windows 10 Hybrid Azure AD join geeft beperkte ondersteuning voor on-premises AD UPN's op basis van de [verificatiemethode](https://docs.microsoft.com/azure/security/azure-ad-choose-authn), domeintype en Windows 10-versie. Er zijn twee typen van on-premises AD UPN's die in uw omgeving optreden kunnen:

 - Routeerbaar UPN: Een routeerbaar UPN is een geldige geverifieerd domein, die is geregistreerd bij een domeinregistrar. Bijvoorbeeld, als het primaire domein contoso.com in Azure AD is, contoso.org is het primaire domein in de on-premises AD die eigendom zijn van Contoso en [geverifieerd in Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)
 
 - Niet-routeerbare UPN: Een niet-routeerbare UPN beschikt niet over een geverifieerd domein. Het is van toepassing alleen binnen het particuliere netwerk van uw organisatie. Bijvoorbeeld, als het primaire domein contoso.com in Azure AD is, contoso.local is het primaire domein in on-premises AD, maar is niet een geverifieerd domein in de internet- en alleen binnen Contoso gebruikt het netwerk.
 
De volgende tabel biedt details over ondersteuning voor deze on-premises AD UPN's in Windows 10 Hybrid Azure AD join

|Type van de on-premises AD UPN|Domeintype|Windows 10-versie|Description|
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

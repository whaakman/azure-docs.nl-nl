---
title: Het plannen van hybride Azure Active Directory join-implementatie in Azure Active Directory (Azure AD) | Microsoft Docs
description: Lees hoe u hybride Azure Active Directory-gekoppelde apparaten kunt configureren.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: abb050eb527e65b4fd31f3251d37fef7d51e867e
ms.sourcegitcommit: cf438e4b4e351b64fd0320bf17cc02489e61406a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/08/2019
ms.locfileid: "67655980"
---
# <a name="how-to-plan-your-hybrid-azure-active-directory-join-implementation"></a>Procedure: Uw hybride Azure Active Directory join-implementatie plannen

Een apparaat is op een soortgelijke manier aan een gebruiker een andere core-identiteit die u wilt beveiligen en gebruiken om uw resources beveiligen op elk gewenst moment en vanaf elke locatie. U kunt dit doel te bereiken door te halen en het beheren van apparaat-id's in Azure AD met behulp van een van de volgende methoden:

- Azure AD-koppeling
- Hybride Azure AD-koppeling
- Azure AD-registratie

Door uw apparaten naar Azure AD te brengen, optimaliseert u de productiviteit van uw gebruikers via eenmalige aanmelding (SSO) bij al uw on-premises bronnen en cloudbronnen. Op hetzelfde moment, kunt u veilige toegang tot uw cloud en on-premises resources met [voorwaardelijke toegang](../active-directory-conditional-access-azure-portal.md).

Als u een on-premises Active Directory (AD)-omgeving hebt en u wilt dat uw AD-domein computers toevoegen aan Azure AD, kunt u dit doen met hybride Azure AD join uitvoeren. Dit artikel vindt u met de bijbehorende stappen voor het implementeren van een hybride Azure AD join in uw omgeving. 

## <a name="prerequisites"></a>Vereisten

In dit artikel wordt ervan uitgegaan dat u bekend met bent de [Inleiding tot identiteit Apparaatbeheer in Azure Active Directory](../device-management-introduction.md).

> [!NOTE]
> De minimaal vereiste functionele domein en forest functionele niveaus voor Windows 10 hybride Azure AD join Windows Server 2008 R2 is.

## <a name="plan-your-implementation"></a>Uw implementatie plannen

Als u wilt uw hybride Azure AD-implementatie te plannen, moet u vertrouwd raken met:

|   |   |
| --- | --- |
| ![Selecteren][1] | Apparaten controleren die worden ondersteund |
| ![Selecteren][1] | Lees wat die u moet weten |
| ![Selecteren][1] | Gecontroleerde validatie van hybride Azure AD join controleren |
| ![Selecteren][1] | Selecteer uw scenario op basis van uw infrastructuur voor identiteiten |
| ![Selecteren][1] | Beoordeling on-premises AD UPN-ondersteuning voor hybride Azure AD join |

## <a name="review-supported-devices"></a>Apparaten controleren die worden ondersteund

Hybride Azure AD join ondersteunt een breed bereik van de Windows-apparaten. Omdat de configuratie voor apparaten met oudere versies van Windows voor aanvullende of andere stappen vereist, worden de ondersteunde apparaten gegroepeerd in twee categorieën:

### <a name="windows-current-devices"></a>Huidige Windows-apparaten

- Windows 10
- Windows Server 2016
- Windows Server 2019

Voor apparaten met het besturingssysteem voor Windows-bureaublad, de ondersteunde versie worden vermeld in dit artikel [release-informatie voor Windows 10](https://docs.microsoft.com/windows/release-information/). Als een best practice raadt Microsoft dat u een upgrade uitvoert naar de nieuwste versie van Windows 10.

### <a name="windows-down-level-devices"></a>Windows downlevel-apparaten

- Windows 8.1
- Windows 7. Lees dit artikel voor meer informatie over Windows 7, [beëindigt de ondersteuning voor Windows 7](https://www.microsoft.com/en-us/windowsforbusiness/end-of-windows-7-support)
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2

U moet als eerste stap planning uw omgeving controleren en bepalen of u nodig hebt om Windows downlevel-apparaten te ondersteunen.

## <a name="review-things-you-should-know"></a>Lees wat die u moet weten

Hybride Azure AD join is momenteel niet ondersteund als uw omgeving bestaat uit één AD-forest synchroniseren van identiteitsgegevens met meer dan één Azure AD-tenant.

Hybride Azure AD join is momenteel niet ondersteund bij het gebruik van virtuele desktopinfrastructuur (VDI).

Hybride Azure AD join wordt niet ondersteund voor FIPS-compatibele TPM's. Als uw apparaten FIPS-compatibele TPM's hebt, moet u deze uitschakelen voordat u doorgaat met Hybrid Azure AD join. Microsoft biedt niet alle hulpprogramma's voor het uitschakelen van FIPS-modus voor TPM's, zoals dit afhankelijk van de TPM-fabrikant is. Neem contact op met uw OEM-hardware voor ondersteuning.

Hybride Azure AD join wordt niet ondersteund voor Windows Server met de rol van domeincontroller (DC).

Hybride Azure AD join wordt niet ondersteund op Windows downlevel-apparaten bij het gebruik van zwervende referenties of gebruiker profiel roaming.

Als u het hulpprogramma voor systeemvoorbereiding (Sysprep) zijn afhankelijk en als u een **vóór Windows 10 1809** installatiekopie voor de installatie, zorg ervoor dat de installatiekopie is niet van een apparaat dat is al geregistreerd bij Azure AD als Hybrid Azure AD join.

Als u al op de momentopname van een virtuele Machine (VM vertrouwen,) te maken van aanvullende virtuele machines, zorg er dan voor dat die momentopname is niet afkomstig van een virtuele machine die is al geregistreerd bij Azure AD als Hybrid Azure AD join.

Als uw Windows 10 domein apparaten al zijn [geregistreerd bij Azure AD](https://docs.microsoft.com/azure/active-directory/devices/overview#getting-devices-in-azure-ad) uw tenant, is het raadzaam deze status verwijderen voordat u Hybrid Azure AD join inschakelt. Van Windows 10 1809 release, zijn de volgende wijzigingen aangebracht om te voorkomen dat deze twee status:

- Eventuele bestaande geregistreerd bij Azure AD-status zou worden automatisch verwijderd nadat het apparaat is toegevoegd aan Hybrid Azure AD.
- U kunt voorkomen dat uw apparaat dat lid is domein wordt Azure AD geregistreerd door toe te voegen deze registersleutel - HKLM\SOFTWARE\Policies\Microsoft\Windows\WorkplaceJoin, "BlockAADWorkplaceJoin" = dword: 00000001.
- Deze wijziging is nu beschikbaar voor Windows 10-1803 release met KB4489894 toegepast. De gebruiker is echter vereist voor het re-setup Windows Hello voor bedrijven na de status van de dubbele opschonen hebt u Windows Hello voor bedrijven die zijn geconfigureerd.

## <a name="review-controlled-validation-of-hybrid-azure-ad-join"></a>Gecontroleerde validatie van hybride Azure AD join controleren

Wanneer alle van de vereisten gemaakt zijn, wordt automatisch Windows-apparaten registreren als apparaten in uw Azure AD-tenant. De status van deze apparaat-id's in Azure AD wordt verwezen als hybride Azure AD join. Meer informatie over de concepten die in dit artikel vindt u in de artikelen [Inleiding tot identiteit Apparaatbeheer in Azure Active Directory](overview.md) en [uw hybride Azure Active Directory join plannen implementatie](hybrid-azuread-join-plan.md).

Organisaties kunnen wilt een gecontroleerde validatie van hybride Azure AD join voordat u dit inschakelt in de hele organisatie in één keer te doen. Lees het artikel [beheerd validatie van hybride Azure AD join](hybrid-azuread-join-control.md) om te begrijpen hoe u deze uitvoert.

## <a name="select-your-scenario-based-on-your-identity-infrastructure"></a>Selecteer uw scenario op basis van uw infrastructuur voor identiteiten

Hybride Azure AD join werkt met zowel beheerde als federatieve omgevingen.  

### <a name="managed-environment"></a>Beheerde omgeving

Een beheerde omgeving kan worden geïmplementeerd via [wachtwoord hash-synchronisatie (WHS)](https://docs.microsoft.com/azure/active-directory/hybrid/whatis-phs) of [doorgeven via verificatie (PTA)](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta) met [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso).

Deze scenario's hoeft u niet een federatieserver voor verificatie te configureren.

### <a name="federated-environment"></a>Federatieve omgeving

Een federatieve omgeving moet beschikken over een id-provider die ondersteuning biedt voor de volgende vereisten:

- **WS-Trust-protocol:** Dit protocol is vereist voor het verifiëren van Windows huidige hybride Azure AD gekoppelde apparaten met Azure AD.
- **WIAORMULTIAUTHN claim:** Deze claim is vereist voor het doen van hybride Azure AD join voor Windows downlevel-apparaten.

Hebt u een federatieve omgeving met behulp van Active Directory Federation Services (AD FS), worden al de bovenstaande vereisten ondersteund.

> [!NOTE]
> Azure AD biedt geen ondersteuning voor smartcards of certificaten in beheerde domeinen.

Vanaf versie 1.1.819.0 bevat Azure AD Connect een wizard om hybride Azure AD-koppeling te configureren. Met de wizard kunt u het configuratieproces aanzienlijk vereenvoudigen. Als het installeren van de vereiste versie van Azure AD Connect kan niet worden gebruikt voor u, raadpleegt u [het handmatig configureren van device Registration service](https://docs.microsoft.com/azure/active-directory/devices/hybrid-azuread-join-manual). 

Op basis van het scenario dat overeenkomt met uw infrastructuur voor identiteiten, Zie:

- [Lid worden van hybride Azure Active Directory voor federatieve omgeving configureren](hybrid-azuread-join-federated-domains.md)
- [Lid worden van hybride Azure Active Directory voor beheerde omgeving configureren](hybrid-azuread-join-managed-domains.md)

## <a name="review-on-premises-ad-upn-support-for-hybrid-azure-ad-join"></a>Beoordeling ondersteuning voor on-premises AD UPN voor Hybrid Azure AD join

Soms, uw on-premises AD UPN's kunnen afwijken van uw Azure AD UPN's. In dergelijke gevallen, Windows 10 Hybrid Azure AD join geeft beperkte ondersteuning voor on-premises AD UPN's op basis van de [verificatiemethode](https://docs.microsoft.com/azure/security/azure-ad-choose-authn), domeintype en Windows 10-versie. Er zijn twee typen van on-premises AD UPN's die in uw omgeving optreden kunnen:

- Routeerbaar UPN: Een routeerbaar UPN is een geldige geverifieerd domein, die is geregistreerd bij een domeinregistrar. Bijvoorbeeld, als het primaire domein contoso.com in Azure AD is, contoso.org is het primaire domein in de on-premises AD die eigendom zijn van Contoso en [geverifieerd in Azure AD](https://docs.microsoft.com/azure/active-directory/fundamentals/add-custom-domain)
- Niet-routeerbare UPN: Een niet-routeerbare UPN beschikt niet over een geverifieerd domein. Het is van toepassing alleen binnen het particuliere netwerk van uw organisatie. Bijvoorbeeld, als het primaire domein contoso.com in Azure AD is, contoso.local is het primaire domein in on-premises AD, maar is niet een geverifieerd domein in de internet- en alleen binnen Contoso gebruikt het netwerk.

De volgende tabel biedt details over ondersteuning voor deze on-premises AD UPN's in Windows 10 Hybrid Azure AD join

| Type van de on-premises AD UPN | Domeintype | Windows 10-versie | Description |
| ----- | ----- | ----- | ----- |
| Routeerbaar | Federatieve | Vanaf versie 1703 | Algemeen beschikbaar |
| Niet-routeerbare | Federatieve | Vanaf versie 1803 | Algemeen beschikbaar |
| Routeerbaar | Managed | Niet ondersteund | |
| Niet-routeerbare | Managed | Niet ondersteund | |

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Configure hybride Azure Active Directory join voor federatieve omgeving](hybrid-azuread-join-federated-domains.md)
> [configureren hybride Azure Active Directory join voor beheerde omgeving](hybrid-azuread-join-managed-domains.md)

<!--Image references-->
[1]: ./media/hybrid-azuread-join-plan/12.png

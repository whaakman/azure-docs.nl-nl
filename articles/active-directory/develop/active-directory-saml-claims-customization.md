---
title: In het SAML-token voor bedrijfstoepassingen in Azure AD uitgegeven claims aanpassen | Microsoft Docs
description: Informatie over het aanpassen van de uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure AD.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
editor: ''
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/11/2018
ms.author: celested
ms.reviewer: jeedes
ms.custom: aaddev
ms.openlocfilehash: 5633dfbf59396e79226b196c2b699981409092ab
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48902022"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Hoe: in het SAML-token voor bedrijfstoepassingen uitgegeven claims aanpassen

Vandaag nog Azure Active Directory (Azure AD) biedt ondersteuning voor eenmalige aanmelding op met de meeste zakelijke toepassingen, met inbegrip van beide toepassingen vooraf geïntegreerd in de galerie van Azure AD-app, evenals aangepaste toepassingen. Wanneer een gebruiker zich bij een toepassing via Azure AD met behulp van het SAML 2.0-protocol verifieert, worden in Azure AD een token verzonden naar de toepassing (via een HTTP POST). En vervolgens de toepassing valideert en gebruikt het token aan te melden van de gebruiker in plaats van dat u wordt gevraagd een gebruikersnaam en wachtwoord. Deze tokens SAML bevatten informatie over de gebruiker bekend als 'claims'.

Een *claim* zijn gegevens met de mededeling van een id-provider van een gebruiker in het token dat ze voor die gebruiker uitgeven. In [SAML-token](http://en.wikipedia.org/wiki/SAML_2.0), deze gegevens is meestal is opgeslagen in de instructie SAML-kenmerk. De unieke ID van de gebruiker wordt gewoonlijk weergegeven in het onderwerp van de SAML-naam-id ook wel.

Standaard verzendt Azure AD een SAML-token aan uw toepassing met een claim NameIdentifier met de waarde van de gebruiker gebruikersnaam (AKA UPN) in Azure AD. Deze waarde kan unieke identificatie van de gebruiker. Het SAML-token bevat ook aanvullende claims met e-mailadres van de gebruiker, de voornaam en achternaam op.

Als u wilt weergeven of bewerken van de claims in het SAML-token is verleend aan de toepassing, open de toepassing in Azure portal. Selecteer vervolgens de **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie van de toepassing.

![Sectie voor kenmerken van gebruiker][1]

Er zijn twee mogelijke redenen waarom u mogelijk de uitgegeven claims in het SAML-token bewerken:
* De toepassing is naar een andere set claim URI's is vereist of claimwaarden geschreven.
* De toepassing is geïmplementeerd op een manier die de claim NameIdentifier dan de gebruikersnaam (AKA gebruiker principal name) die zijn opgeslagen in Azure AD is vereist.

U kunt een van de standaardwaarden van de claim bewerken. Selecteer de rij claim in de tabel voor SAML-token kenmerken. Hiermee opent u de **kenmerk bewerken** sectie en u vervolgens de claimnaam van de, de waarde en de naamruimte die is gekoppeld aan de claim kunt bewerken.

![Gebruikerskenmerk bewerken][2]

U kunt ook verwijderen (met uitzondering van NameIdentifier) met behulp van claims het contextmenu dat wordt geopend door te klikken op de **...**  pictogram. U kunt ook nieuwe claims met toevoegen de **kenmerk toevoegen** knop.

![Gebruikerskenmerk bewerken][3]

## <a name="editing-the-nameidentifier-claim"></a>De claim NameIdentifier bewerken
Voor het probleem waarbij de toepassing is geïmplementeerd met behulp van een andere gebruikersnaam, klikt u op de **gebruikers-id** vervolgkeuzelijst de **gebruikerskenmerken** sectie. Deze actie wordt een dialoogvenster met verschillende opties:

![Gebruikerskenmerk bewerken][4]

Selecteer in de vervolgkeuzelijst **user.mail** om in te stellen van de claim NameIdentifier e-mailadres van de gebruiker in de map. Of selecteer **user.onpremisessamaccountname** om in te stellen voor de gebruiker de SAM-accountnaam die zijn gesynchroniseerd van on-premises Azure AD.

U kunt ook de speciale **ExtractMailPrefix()** functie voor het verwijderen van het domeinachtervoegsel van het e-mailadres, SAM-accountnaam of de user principal name. Alleen het eerste deel van de naam van de gebruiker wordt doorgegeven via geëxtraheerd (bijvoorbeeld 'joe_smith' in plaats van joe_smith@contoso.com).

![Gebruikerskenmerk bewerken][5]

We hebben nu ook toegevoegd de **join()** functie aan het geverifieerde domein met de gebruikers-id-waarde. Wanneer u de functie join() in selecteert de **gebruikers-id** selecteert u eerst de gebruikers-id, zoals e-mailadres of gebruikersnaam principal-naam en selecteer vervolgens uw geverifieerde domein in de tweede vervolgkeuzelijst. Als u het e-mailadres met het geverifieerde domein selecteert, wordt Azure AD de gebruikersnaam geëxtraheerd uit de eerste waarde joe_smith van joe_smith@contoso.com en voegt deze toe met contoso.onmicrosoft.com. Zie het volgende voorbeeld:

![Gebruikerskenmerk bewerken][6]

## <a name="adding-claims"></a>Claims toevoegen
Wanneer een claim toevoegen, kunt u de naam van het kenmerk (die strikt hoeft niet te volgen een patroon URI aan de hand van de SAML-specificatie) opgeven. Stel de waarde op elk gebruikerskenmerk die zijn opgeslagen in de map.

![Gebruikerskenmerk toevoegen][7]

Bijvoorbeeld, moet u de afdeling die deel uitmaakt van de gebruiker te verzenden in hun organisatie die als een claim (zoals omzet). Voer de claimnaam van de zoals verwacht door de toepassing en selecteer vervolgens **user.department** als de waarde.

> [!NOTE]
> Als er geen waarde die is opgeslagen voor een geselecteerd kenmerk voor een bepaalde gebruiker is, wordt er niet die claim in het token wordt uitgegeven.

> [!TIP]
> De **user.onpremisesecurityidentifier** en **user.onpremisesamaccountname** worden alleen ondersteund bij het synchroniseren van gebruikersgegevens van on-premises Active Directory met de [Azure AD Verbinding maken met hulpprogramma](../hybrid/whatis-hybrid-identity.md).

## <a name="restricted-claims"></a>Beperkte claims

Er zijn enkele beperkingen claims in SAML. Als u deze claims toevoegt, klikt u vervolgens in Azure AD niet deze claims verzenden. Hieronder vindt u de set van de claim SAML beperkt:

    | Claimtype (URI) |
    | ------------------- |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expiration |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/expired |
    | http://schemas.microsoft.com/identity/claims/accesstoken |
    | http://schemas.microsoft.com/identity/claims/openid2_id |
    | http://schemas.microsoft.com/identity/claims/identityprovider |
    | http://schemas.microsoft.com/identity/claims/objectidentifier |
    | http://schemas.microsoft.com/identity/claims/puid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier[MR1] |
    | http://schemas.microsoft.com/identity/claims/tenantid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationinstant |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod |
    | http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groups |
    | http://schemas.microsoft.com/claims/groups.link |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/role |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/wids |
    | http://schemas.microsoft.com/2014/09/devicecontext/claims/iscompliant |
    | http://schemas.microsoft.com/2014/02/devicecontext/claims/isknown |
    | http://schemas.microsoft.com/2012/01/devicecontext/claims/ismanaged |
    | http://schemas.microsoft.com/2014/03/psso |
    | http://schemas.microsoft.com/claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/identity/claims/actor |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/samlissuername |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/confirmationkey |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/sid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarygroupsid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/denyonlysid |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/denyonlywindowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdeviceclaim |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsfqbnversion |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowssubauthority |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/spn |
    | http://schemas.microsoft.com/ws/2008/06/identity/claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/identity/claims/privatepersonalidentifier |
    | http://schemas.microsoft.com/identity/claims/scope |

## <a name="next-steps"></a>Volgende stappen

* [Toepassingsbeheer in Azure AD](../manage-apps/what-is-application-management.md)
* [Configureren van eenmalige aanmelding voor toepassingen die zich niet in de Azure AD-toepassingsgalerie](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Oplossen van problemen met SAML gebaseerde eenmalige aanmelding](howto-v1-debug-saml-sso-issues.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png

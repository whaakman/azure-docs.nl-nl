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
ms.date: 10/20/2018
ms.author: celested
ms.reviewer: luleon, jeedes
ms.custom: aaddev
ms.openlocfilehash: 11132426bb8adb6ede564e706e18f3eddd649bef
ms.sourcegitcommit: c31a2dd686ea1b0824e7e695157adbc219d9074f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2019
ms.locfileid: "54401874"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Procedure: In het SAML-token voor bedrijfstoepassingen uitgegeven claims aanpassen

Vandaag nog Azure Active Directory (Azure AD) biedt ondersteuning voor eenmalige aanmelding op met de meeste zakelijke toepassingen, met inbegrip van beide toepassingen vooraf geïntegreerd in de galerie van Azure AD-app, evenals aangepaste toepassingen. Wanneer een gebruiker zich bij een toepassing via Azure AD met behulp van het SAML 2.0-protocol verifieert, worden in Azure AD een token verzonden naar de toepassing (via een HTTP POST). En vervolgens de toepassing valideert en gebruikt het token aan te melden van de gebruiker in plaats van dat u wordt gevraagd een gebruikersnaam en wachtwoord. Deze tokens SAML bevatten informatie over de gebruiker bekend als 'claims'.

Een *claim* zijn gegevens met de mededeling van een id-provider van een gebruiker in het token dat ze voor die gebruiker uitgeven. In [SAML-token](https://en.wikipedia.org/wiki/SAML_2.0), deze gegevens is meestal is opgeslagen in de instructie SAML-kenmerk. De unieke ID van de gebruiker wordt gewoonlijk weergegeven in het onderwerp van de SAML-naam-id ook wel.

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

Voor het probleem waarbij de toepassing is geïmplementeerd met behulp van een andere gebruikersnaam, selecteert u in de **gebruikers-id** vervolgkeuzelijst de **gebruikerskenmerken** sectie. Deze actie wordt een dialoogvenster met verschillende opties:

![Gebruikerskenmerk bewerken][4]

### <a name="attributes"></a>Kenmerken

Selecteer de gewenste bron voor de `NameIdentifier` (of NameID) claim. U kunt kiezen uit de volgende opties.

| Name | Description |
|------|-------------|
| Email | Het e-mailadres van de gebruiker |
| userprincipalName | De user principal name (UPN) van de gebruiker |
| onpremisessamaccount | SAM-accountnaam die zijn gesynchroniseerd van on-premises Azure AD |
| objectID | De object-id van de gebruiker in Azure AD |
| Werknemer-id | De werknemer-id van de gebruiker |
| Uitbreidingen van de directory | Mapextensies [vanuit on-premises Active Directory met behulp van Azure AD Connect Sync gesynchroniseerd](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| Extensiekenmerken 1-15 | On-premises extensiekenmerken gebruikt de Azure AD-schema uit te breiden |

### <a name="transformations"></a>Transformaties

U kunt ook de speciale claims transformaties-functies gebruiken.

| Function | Description |
|----------|-------------|
| **ExtractMailPrefix()** | Hiermee verwijdert u het domeinachtervoegsel van het e-mailadres, SAM-accountnaam of de user principal name. Alleen het eerste deel van de naam van de gebruiker wordt doorgegeven via geëxtraheerd (bijvoorbeeld 'joe_smith' in plaats van joe_smith@contoso.com). |
| **join()** | Lid wordt van een kenmerk met een geverifieerd domein. Als de geselecteerde gebruiker-id-waarde een domein heeft, wordt deze de gebruikersnaam voor het toevoegen van het geverifieerde domein voor geselecteerde extraheren. Bijvoorbeeld, als u het e-mailbericht (joe_smith@contoso.com) als de waarde van de gebruiker-id en selecteer contoso.onmicrosoft.com als het geverifieerde domein voor, resulteert dit in joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Zet de tekens van het geselecteerde kenmerk in kleine letters. |
| **ToUpper()** | Zet de tekens van het geselecteerde kenmerk in hoofdletters. |

## <a name="adding-claims"></a>Claims toevoegen

Wanneer een claim toevoegen, kunt u de naam van het kenmerk (die strikt hoeft niet te volgen een patroon URI aan de hand van de SAML-specificatie) opgeven. Stel de waarde voor elk gebruikerskenmerk die zijn opgeslagen in de map of gebruik de waarde van een constante geretourneerd als een statische vermelding voor alle gebruikers in uw organisatie.

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
* [Oplossen van SAML gebaseerde eenmalige aanmelding](howto-v1-debug-saml-sso-issues.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png

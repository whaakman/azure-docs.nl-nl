---
title: Uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure Active Directory aanpassen | Microsoft Docs
description: Informatie over het aanpassen van de uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure Active Directory
services: active-directory
documentationcenter: 
author: jeevansd
manager: mtillman
editor: 
ms.assetid: f1daad62-ac8a-44cd-ac76-e97455e47803
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: jeedes
ms.custom: aaddev
ms.openlocfilehash: 7394857f55493b072e6ea549c8eeec54a808f5e5
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="customizing-claims-issued-in-the-saml-token-for-enterprise-applications-in-azure-active-directory"></a>Uitgegeven claims in het SAML-token voor bedrijfstoepassingen in Azure Active Directory aanpassen
Vandaag de dag ondersteunt Azure Active Directory voor eenmalige op met de meeste zakelijke toepassingen, met inbegrip van beide toepassingen die vooraf zijn geïntegreerd in de galerie van Azure AD-app, evenals aangepaste toepassingen. Wanneer een gebruiker wordt geverifieerd op een toepassing via Azure AD dat gebruikmaakt van het SAML 2.0-protocol, verzendt Azure AD een token naar de toepassing (via een HTTP POST). En vervolgens de toepassing valideert en gebruikt het token aan te melden van de gebruiker in in plaats van naar een gebruikersnaam en wachtwoord wordt gevraagd. Deze tokens SAML bevatten stukjes informatie over de gebruiker bekend als 'claims'.

In de identity-uitspreken, een "claim" informatie een id-provider wordt gemeld dat een gebruiker in het token dat ze voor die gebruiker uitgeven. In [SAML-token](http://en.wikipedia.org/wiki/SAML_2.0), deze gegevens meestal is opgeslagen in de instructie SAML-kenmerk. De unieke ID van de gebruiker wordt meestal weergegeven in het onderwerp van het SAML-naam-id ook wel.

Standaard wordt in Azure Active Directory een SAML-token verstrekt aan uw toepassing waarin een claim NameIdentifier met een waarde van de gebruikersnaam van (AKA UPN-naam) in Azure AD. Deze waarde kan een unieke identificatie van de gebruiker. De SAML-token bevat ook extra claims met e-mailadres van de gebruiker, de voornaam en achternaam op.

Als u wilt weergeven of bewerken van de uitgegeven claims in het SAML-token naar de toepassing, de toepassing in Azure-portal te openen. Selecteer vervolgens de **weergeven en bewerken van alle andere gebruikerskenmerken** selectievakje in de **gebruikerskenmerken** sectie van de toepassing.

![Sectie van de kenmerken gebruiker][1]

Er zijn twee mogelijke redenen waarom moet u mogelijk de uitgegeven claims in het SAML-token bewerken:
* De toepassing is geschreven naar een andere set claimregels URI's is vereist of claimwaarden.
* De toepassing is geïmplementeerd op een manier die de NameIdentifier claim iets anders dan de gebruikersnaam (AKA UPN-naam) opgeslagen in Azure Active Directory vereist.

U kunt een van de standaardwaarden van de claim te bewerken. Selecteer de rij claim in de tabel SAML-token kenmerken. Hiermee opent u de **kenmerk bewerken** sectie en u vervolgens de claimnaam van de, waarde en naamruimte die is gekoppeld aan de claim kunt bewerken.

![Gebruikerskenmerk bewerken][2]

U kunt ook verwijderen claims (met uitzondering van NameIdentifier) op basis van het snelmenu dat wordt geopend door te klikken op de **...**  pictogram.  U kunt ook toevoegen nieuwe claims op basis van de **toevoegen kenmerk** knop.

![Gebruikerskenmerk bewerken][3]

## <a name="editing-the-nameidentifier-claim"></a>De claim NameIdentifier bewerken
Voor het oplossen van het probleem waarbij de toepassing is geïmplementeerd met behulp van een andere gebruikersnaam, klikt u op de **gebruikers-id** vervolgkeuzelijst de **gebruikerskenmerken** sectie. Deze actie geeft een dialoogvenster met verschillende opties:

![Gebruikerskenmerk bewerken][4]

Selecteer in de vervolgkeuzelijst **user.mail** om in te stellen van de claim NameIdentifier e-mailadres van de gebruiker in de map. Of selecteer **user.onpremisessamaccountname** in te stellen op de gebruiker de SAM-accountnaam van on-premises Azure AD zijn gesynchroniseerd.

U kunt ook de speciale **ExtractMailPrefix()** functie voor het verwijderen van het domeinachtervoegsel van het e-mailadres, SAM-accountnaam of de UPN-naam. Alleen het eerste deel van de gebruikersnaam die wordt doorgegeven geëxtraheerd (bijvoorbeeld 'joe_smith' in plaats van joe_smith@contoso.com).

![Gebruikerskenmerk bewerken][5]

We nu ook hebt toegevoegd de **join()** functie aan het geverifieerde domein met de waarde van de gebruiker-id. Wanneer u de functie join() in selecteert de **gebruikers-id** selecteert u eerst de gebruikers-id zoals zoals e-mailadres of de gebruiker principal-naam en selecteer vervolgens het geverifieerde domein in de tweede vervolgkeuzelijst. Als u het e-mailadres met het geverifieerde domein selecteert, wordt Azure AD de gebruikersnaam geëxtraheerd uit de eerste waarde joe_smith van joe_smith@contoso.com en voegt deze toe met contoso.onmicrosoft.com. Zie het volgende voorbeeld:

![Gebruikerskenmerk bewerken][6]

## <a name="adding-claims"></a>Claims toevoegen
Wanneer een claim toevoegen, kunt u de naam van het kenmerk (die niet strikt moet een patroon URI volgens de specificatie SAML volgen) opgeven. Stel de waarde voor elk gebruikerskenmerk die is opgeslagen in de map.

![Gebruikerskenmerk toevoegen][7]

Bijvoorbeeld, moet u het verzenden van de afdeling die de gebruiker behoort binnen hun organisatie als een claim (zoals Sales). Geef de claimnaam zoals werd verwacht door de toepassing en selecteer vervolgens **user.department** als de waarde.

> [!NOTE]
> Als voor een bepaalde gebruiker er geen waarde voor een geselecteerd kenmerk opgeslagen, is er geen die claim in het token wordt uitgegeven.

> [!TIP]
> De **user.onpremisesecurityidentifier** en **user.onpremisesamaccountname** worden alleen ondersteund bij het synchroniseren van gebruikersgegevens van on-premises Active Directory met het [Azure AD Connect-hulpprogramma](../active-directory-aadconnect.md).

## <a name="restricted-claims"></a>Beperkte claims

Er zijn enkele beperkte claims in SAML. Als u deze claims toevoegt, klikt u vervolgens in Azure AD niet deze claims verzenden. Hieronder volgen de claimset SAML beperkt:

    | Claimtype (URI) |
    | ------------------- |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/Expiration |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/EXPIRED |
    | http://schemas.Microsoft.com/Identity/claims/accesstoken |
    | http://schemas.Microsoft.com/Identity/claims/openid2_id |
    | http://schemas.Microsoft.com/Identity/claims/identityprovider |
    | http://schemas.Microsoft.com/Identity/claims/objectidentifier |
    | http://schemas.Microsoft.com/Identity/claims/PUID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/NameIdentifier [MR1] |
    | http://schemas.Microsoft.com/Identity/claims/tenantid |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/AuthenticationInstant |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/AuthenticationMethod |
    | http://schemas.Microsoft.com/AccessControlService/2010/07/claims/identityprovider |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/Groups |
    | http://schemas.Microsoft.com/claims/groups.Link |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/Role |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/wids |
    | http://schemas.Microsoft.com/2014/09/devicecontext/claims/iscompliant |
    | http://schemas.Microsoft.com/2014/02/devicecontext/claims/isknown |
    | http://schemas.Microsoft.com/2012/01/devicecontext/claims/ismanaged |
    | http://schemas.Microsoft.com/2014/03/psso |
    | http://schemas.Microsoft.com/claims/authnmethodsreferences |
    | http://schemas.xmlsoap.org/ws/2009/09/Identity/claims/actor |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/samlissuername |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/confirmationkey |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsaccountname |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/primarygroupsid |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/primarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/authorizationdecision |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/Authentication |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/SID |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/denyonlyprimarygroupsid |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/denyonlyprimarysid |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/denyonlysid |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/denyonlywindowsdevicegroup |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsdeviceclaim |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsdevicegroup |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsfqbnversion |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowssubauthority |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/windowsuserclaim |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/x500distinguishedname |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/UPN |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/GroupSID |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/SPN |
    | http://schemas.Microsoft.com/ws/2008/06/Identity/claims/ispersistent |
    | http://schemas.xmlsoap.org/ws/2005/05/Identity/claims/privatepersonalidentifier |
    | http://schemas.Microsoft.com/Identity/claims/scope |

## <a name="next-steps"></a>Volgende stappen
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](../application-config-sso-how-to-configure-federated-sso-non-gallery.md) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
* [Het oplossen van problemen op basis van SAML eenmalige aanmelding](active-directory-saml-debugging.md)

<!--Image references-->
[1]: ./media/active-directory-saml-claims-customization/user-attribute-section.png
[2]: ./media/active-directory-saml-claims-customization/edit-claim-name-value.png
[3]: ./media/active-directory-saml-claims-customization/delete-claim.png
[4]: ./media/active-directory-saml-claims-customization/user-identifier.png
[5]: ./media/active-directory-saml-claims-customization/extractemailprefix-function.png
[6]: ./media/active-directory-saml-claims-customization/join-function.png
[7]: ./media/active-directory-saml-claims-customization/add-attribute.png

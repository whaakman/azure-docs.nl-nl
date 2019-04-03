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
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/03/2019
ms.author: celested
ms.reviewer: luleon, paulgarn, jeedes
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: c6fe74852824c10d24729f785e5e33a17b793161
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58878567"
---
# <a name="how-to-customize-claims-issued-in-the-saml-token-for-enterprise-applications"></a>Procedure: In het SAML-token voor bedrijfstoepassingen uitgegeven claims aanpassen

Vandaag, Azure Active Directory (Azure AD) biedt ondersteuning voor eenmalige aanmelding (SSO) met de meeste zakelijke toepassingen, met inbegrip van beide toepassingen vooraf geïntegreerd in de galerie van Azure AD-app, evenals aangepaste toepassingen. Wanneer een gebruiker zich bij een toepassing via Azure AD met behulp van het SAML 2.0-protocol verifieert, worden in Azure AD een token verzonden naar de toepassing (via een HTTP POST). En vervolgens de toepassing valideert en gebruikt het token aan te melden van de gebruiker in plaats van dat u wordt gevraagd een gebruikersnaam en wachtwoord. Deze tokens SAML bevatten informatie over de gebruiker die ook wel *claims*.

Een *claim* zijn gegevens met de mededeling van een id-provider van een gebruiker in het token dat ze voor die gebruiker uitgeven. In [SAML-token](https://en.wikipedia.org/wiki/SAML_2.0), deze gegevens is meestal is opgeslagen in de instructie SAML-kenmerk. De unieke ID van de gebruiker wordt gewoonlijk weergegeven in het onderwerp van de SAML-naam-id ook wel.

Azure AD geeft standaard een SAML-token aan uw toepassing met een `NameIdentifier` claim met een waarde van de gebruikersnaam van de gebruiker (ook wel bekend als de user principal name) in Azure AD. Hierbij worden afzonderlijk geïdentificeerd kan de gebruiker. Het SAML-token bevat ook aanvullende claims met e-mailadres van de gebruiker, de voornaam en achternaam op.

Als u wilt weergeven of bewerken van de claims in het SAML-token is verleend aan de toepassing, open de toepassing in Azure portal. Open vervolgens de **gebruikerskenmerken en Claims** sectie.

![Sectie gebruikerskenmerken en Claims](./media/active-directory-saml-claims-customization/sso-saml-user-attributes-claims.png)

Er zijn twee mogelijke redenen waarom u mogelijk de uitgegeven claims in het SAML-token bewerken:

* De toepassing, moet de `NameIdentifier` of NameID beweren te zijn iets anders dan de gebruikersnaam (of user principal name) die zijn opgeslagen in Azure AD.
* De toepassing is naar een andere set claim URI's is vereist of claimwaarden geschreven.

## <a name="editing-nameid"></a>Bewerken van NameID

Bewerken van de NameID (naam-id-waarde):

1. Open de **naam id-waarde** pagina.
1. Selecteer het kenmerk of de transformatie die u wilt toepassen op het kenmerk. Desgewenst kunt u de indeling die u wilt dat hij de claim NameID hebben.

   ![Bewerk de waarde NameID (naam-id)](./media/active-directory-saml-claims-customization/saml-sso-manage-user-claims.png)

### <a name="nameid-format"></a>NameID-indeling

Als de SAML-aanvraag het element NameIDPolicy met een specifieke indeling bevat, wordt de indeling in de aanvraag gehandhaafd door Azure AD.

Als de SAML-aanvraag een element voor NameIDPolicy bevat, kunnen Azure AD wordt de NameID probleem met de indeling die u opgeeft. Als er geen indeling is opgegeven Azure AD wordt gebruikt de standaardindeling van de bron die is gekoppeld aan de claimbron geselecteerd.

Uit de **id indeling kiezen** vervolgkeuzelijst, kunt u een van de volgende opties selecteren.

| NameID-indeling | Description |
|---------------|-------------|
| **Standaard** | Azure AD wordt de standaardindeling voor de gegevensbron gebruikt. |
| **Permanent** | Azure AD wordt Persistent gebruikt als de NameID-indeling. |
| **EmailAddress** | Azure AD gebruikt EmailAddress als de NameID-indeling. |
| **Niet opgegeven** | Azure AD gebruikt als de NameID-indeling niet opgegeven. |
| **Tijdelijke** | Azure AD wordt tijdelijk gebruikt als de NameID-indeling. |

Zie voor meer informatie over het kenmerk NameIDPolicy [Single Sign-On SAML-protocol](single-sign-on-saml-protocol.md).

### <a name="attributes"></a>Kenmerken

Selecteer de gewenste bron voor de `NameIdentifier` (of NameID) claim. U kunt kiezen uit de volgende opties.

| Name | Beschrijving |
|------|-------------|
| Email | E-mailadres van de gebruiker |
| userprincipalName | User principal name (UPN) van de gebruiker |
| onpremisessamaccount | SAM-accountnaam die zijn gesynchroniseerd van on-premises Azure AD |
| object-id | object-id van de gebruiker in Azure AD |
| employeeid | werknemer-id van de gebruiker |
| Uitbreidingen van de directory | Mapextensies [vanuit on-premises Active Directory met behulp van Azure AD Connect Sync gesynchroniseerd](../hybrid/how-to-connect-sync-feature-directory-extensions.md) |
| Extensiekenmerken 1-15 | On-premises extensiekenmerken gebruikt de Azure AD-schema uit te breiden |

Zie voor meer informatie, [tabel 3: Id-waarden per bron](active-directory-claims-mapping.md#table-3-valid-id-values-per-source).

### <a name="special-claims---transformations"></a>Speciale claims - transformaties

U kunt ook de claims transformaties-functies gebruiken.

| Function | Description |
|----------|-------------|
| **ExtractMailPrefix()** | Hiermee verwijdert u het domeinachtervoegsel van het e-mailadres of de user principal name. Alleen het eerste deel van de naam van de gebruiker wordt doorgegeven via geëxtraheerd (bijvoorbeeld 'joe_smith' in plaats van joe_smith@contoso.com). |
| **Join()** | Lid wordt van een kenmerk met een geverifieerd domein. Als de geselecteerde gebruiker-id-waarde een domein heeft, wordt deze de gebruikersnaam voor het toevoegen van het geverifieerde domein voor geselecteerde extraheren. Bijvoorbeeld, als u het e-mailbericht (joe_smith@contoso.com) als de waarde van de gebruiker-id en selecteer contoso.onmicrosoft.com als het geverifieerde domein voor, resulteert dit in joe_smith@contoso.onmicrosoft.com. |
| **ToLower()** | Zet de tekens van het geselecteerde kenmerk in kleine letters. |
| **ToUpper()** | Zet de tekens van het geselecteerde kenmerk in hoofdletters. |

## <a name="adding-application-specific-claims"></a>Toepassingsspecifieke claims toe te voegen

Om toe te voegen toepassingsspecifieke claims:

1. In **gebruikerskenmerken en Claims**, selecteer **toevoegen nieuwe claim** openen de **gebruikersclaims beheren** pagina.
1. Voer de **naam** van de claims. De waarde moet niet strikt worden als volgt URI, per de SAML-specificaties. Als u een patroon URI nodig hebt, kunt u die plaatsen de **Namespace** veld.
1. Selecteer de **bron** waar de claim is gaan om op te halen van de waarde ervan. U kunt een gebruikerskenmerk selecteren in de vervolgkeuzelijst bron-kenmerk of een transformatie van toepassing op het gebruikerskenmerk voordat deze als een claim verzenden.

### <a name="application-specific-claims---transformations"></a>Toepassingsspecifieke claims - transformaties

U kunt ook de claims transformaties-functies gebruiken.

| Function | Description |
|----------|-------------|
| **ExtractMailPrefix()** | Hiermee verwijdert u het domeinachtervoegsel van het e-mailadres of de user principal name. Alleen het eerste deel van de naam van de gebruiker wordt doorgegeven via geëxtraheerd (bijvoorbeeld 'joe_smith' in plaats van joe_smith@contoso.com). |
| **Join()** | Hiermee maakt een nieuwe waarde lid wordt van twee kenmerken. Desgewenst kunt u een scheidingsteken tussen de twee kenmerken. |
| **ToLower()** | Zet de tekens van het geselecteerde kenmerk in kleine letters. |
| **ToUpper()** | Zet de tekens van het geselecteerde kenmerk in hoofdletters. |
| **Contains()** | Voert een kenmerk of een constante als de invoer komt overeen met de opgegeven waarde. Anders kunt u een andere uitvoer opgeven als er geen overeenkomst.<br/>Bijvoorbeeld, als u wilt verzenden van een claim waarin de waarde van de gebruiker e-mailadres is als de waarde van het domein '@contoso.com', anders u wilt uitvoeren van de user principal name. Om dit te doen, zou u de volgende waarden configureren:<br/>*Parameter 1(input)*: user.email<br/>*Waarde*: "@contoso.com"<br/>Parameter 2 (output): user.email<br/>Parameter 3 (uitvoer als er geen overeenkomst): user.userprincipalname |
| **EndWith()** | Voert een kenmerk of een constante als de invoer met de opgegeven waarde eindigt. Anders kunt u een andere uitvoer opgeven als er geen overeenkomst.<br/>Bijvoorbeeld, als u wilt verzenden van een claim waarin de waarde van de gebruiker werknemer-id is als de id van de werknemer eindigt op "000", u anders uitvoer van een kenmerk toestelnummer. Om dit te doen, zou u de volgende waarden configureren:<br/>*Parameter 1(input)*: user.employeeid<br/>*Waarde*: "000"<br/>Parameter 2 (uitvoer): user.employeeid<br/>Parameter 3 (uitvoer als er geen overeenkomst): user.extensionattribute1 |
| **StartWith()** | Voert een kenmerk of een constante als de invoer wordt gestart met de opgegeven waarde. Anders kunt u een andere uitvoer opgeven als er geen overeenkomst.<br/>Bijvoorbeeld, als u wilt verzenden van een claim waarin de waarde van de gebruiker werknemer-id is als het land met "US begint", u anders uitvoer van een kenmerk toestelnummer. Om dit te doen, zou u de volgende waarden configureren:<br/>*Parameter 1(input)*: zoals user.country<br/>*Waarde*: "US"<br/>Parameter 2 (uitvoer): user.employeeid<br/>Parameter 3 (uitvoer als er geen overeenkomst): user.extensionattribute1 |
| **Extract() - na die overeenkomt met** | Retourneert de subtekenreeks nadat deze overeenkomt met de opgegeven waarde.<br/>Als de waarde van de invoer is 'Finance_BSimon', de overeenkomende waarde is 'Finance_' en de uitvoer van de claim 'BSimon' is. |
| **Extract() - voordat u die overeenkomt met** | Retourneert de subtekenreeks totdat deze overeenkomt met de opgegeven waarde.<br/>Als de waarde van de invoer is 'BSimon_US', de overeenkomende waarde is '_US' en de uitvoer van de claim 'BSimon' is. |
| **Extract() - tussen overeenkomende** | Retourneert de subtekenreeks totdat deze overeenkomt met de opgegeven waarde.<br/>Bijvoorbeeld, als de waarde van de invoer is 'Finance_BSimon_US', de eerste overeenkomende waarde is 'Finance_', de tweede, overeenkomende waarde is '_US' en uitvoer van de claim 'BSimon' is. |
| **ExtractAlpha() - Prefix** | Retourneert het voorvoegsel alfabetische deel van de tekenreeks.<br/>Bijvoorbeeld, als de waarde van de invoer is 'BSimon_123', resultaat is het 'BSimon'. |
| **ExtractAlpha() - Suffix** | Retourneert het achtervoegsel alfabetische deel van de tekenreeks.<br/>Bijvoorbeeld, als de waarde van de invoer is '123_Simon', resultaat is het 'BSimon'. |
| **ExtractNumeric() - voorvoegsel** | Retourneert het numerieke gedeelte voorvoegsel van de tekenreeks.<br/>Bijvoorbeeld, als de waarde van de invoer is '123_BSimon', resultaat is het '123 ' omgekeerd. |
| **ExtractNumeric() - Suffix** | Retourneert het numerieke gedeelte achtervoegsel van de tekenreeks.<br/>Bijvoorbeeld, als de waarde van de invoer is 'BSimon_123', resultaat is het '123 ' omgekeerd. |
| **IfEmpty()** | Voert een kenmerk of een constante als de invoer null of leeg zijn is.<br/>Bijvoorbeeld, als u wilt uitvoeren van een kenmerk die zijn opgeslagen in een extensionattribute als de werknemer-id voor een bepaalde gebruiker leeg is. Om dit te doen, zou u de volgende waarden configureren:<br/>Parameter 1(input): user.employeeid<br/>Parameter 2 (output): user.extensionattribute1<br/>Parameter 3 (uitvoer als er geen overeenkomst): user.employeeid |
| **IfNotEmpty()** | Voert een kenmerk of een constante als de invoer niet null of leeg zijn is.<br/>Bijvoorbeeld, als u wilt uitvoeren van een kenmerk die zijn opgeslagen in een extensionattribute als de werknemer-id voor een bepaalde gebruiker niet leeg zijn is. Om dit te doen, zou u de volgende waarden configureren:<br/>Parameter 1(input): user.employeeid<br/>Parameter 2 (output): user.extensionattribute1 |

Als u meer transformaties, verzend uw idee in de [in Azure AD-Feedbackforum](https://feedback.azure.com/forums/169401-azure-active-directory?category_id=160599) onder de *SaaS-toepassing* categorie.

## <a name="next-steps"></a>Volgende stappen

* [Toepassingsbeheer in Azure AD](../manage-apps/what-is-application-management.md)
* [Configureren van eenmalige aanmelding voor toepassingen die zich niet in de Azure AD-toepassingsgalerie](../manage-apps/configure-federated-single-sign-on-non-gallery-applications.md)
* [Oplossen van SAML gebaseerde eenmalige aanmelding](howto-v1-debug-saml-sso-issues.md)

---
title: Fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory | Microsoft Docs
description: 'Meer informatie over fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory '
services: active-directory
author: CelesteDG
documentationcenter: na
manager: mtillman
ms.assetid: edbe492b-1050-4fca-a48a-d1fa97d47815
ms.service: active-directory
ms.component: develop
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/20/2017
ms.author: celested
ms.custom: aaddev
ms.reviewer: dastrock; smalser
ms.openlocfilehash: 1a33b5ab9e26ed497e3be2d430f66ef41402733d
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="how-to-debug-saml-based-single-sign-on-to-applications-in-azure-active-directory"></a>Fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure Active Directory

Wanneer een toepassing op basis van SAML-integratie foutopsporing, is het vaak nuttig zijn voor het gebruik van een hulpprogramma zoals [Fiddler](http://www.telerik.com/fiddler) om te zien van de SAML-aanvraag en de SAML-reactie die het SAML-token dat is uitgegeven voor een toepassing bevatten. 

![Fiddler][1]

De problemen zijn vaak betrekking heeft op een onjuiste configuratie aan Azure Active Directory of aan de kant van de toepassing. Afhankelijk van waar u de fout ziet, hebt u de SAML-aanvraag of antwoord bekijken:

- Zie ik een fout in mijn bedrijf aanmeldingspagina [koppelingen naar de sectie]
- Een fout op de pagina van de toepassing wordt weergegeven na het aanmelden [koppelingen naar de sectie]

## <a name="i-see-an-error-in-my-company-sign-in-page"></a>Er wordt een foutbericht in mijn bedrijf-aanmeldingspagina.

U vindt een-op-een toewijzing tussen de foutcode en de stappen voor het oplossen in [problemen die zijn aangemeld bij een galerij-toepassing is geconfigureerd voor federatieve eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML).

Als er een fout in uw bedrijf-aanmeldingspagina weergegeven, moet u de foutmelding en SAML-aanvraag voor foutopsporing van het probleem.

### <a name="how-can-i-get-the-error--message"></a>Hoe kan ik het foutbericht krijgen?

Als u het foutbericht wordt weergegeven, bekijkt u de rechterbenedenhoek van de pagina. Er is een fout met:

- Een CorrelationID
- Een tijdstempel
- Een bericht

![Fout][2] 

 
De correlatie-Id en het tijdstempel vormen een unieke id die u gebruiken kunt om te zoeken naar de back-end-logboeken die zijn gekoppeld aan de aanmelding mislukt. Deze waarden zijn belangrijk wanneer u een ondersteuningsaanvraag met Microsoft maken omdat ze de engineers helpen voor een snellere oplossing voor uw probleem.

Het bericht is de hoofdoorzaak van het probleem aanmelden. 


### <a name="how-can-i-review-the-saml-request"></a>Hoe kan ik de SAML-aanvraag controleren?

De SAML-aanvraag verzonden door de toepassing met Azure Active Directory is meestal het laatste HTTP 200-antwoord van [ https://login.microsoftonline.com ](https://login.microsoftonline.com).
 
Met Fiddler, kunt u de SAML-aanvraag weergeven door deze regel te selecteren en vervolgens te klikken op de **Inspectors > webformulieren** tabblad in het rechterpaneel. Met de rechtermuisknop op de **SAMLRequest** waarde en selecteert u vervolgens **verzenden naar TextWizard**. Selecteer vervolgens **van Base64** van de **transformeren** menu voor het decoderen van het token en de inhoud weergeven. 

Bovendien kunt u ook de waarde in de SAMLrequest kopiëren en gebruiken van een andere Base64-codering.

    <samlp:AuthnRequest
    xmlns="urn:oasis:names:tc:SAML:2.0:metadata"
    ID="id6c1c178c166d486687be4aaf5e482730"
    Version="2.0" IssueInstant="2013-03-18T03:28:54.1839884Z"
    Destination=https://login.microsoftonline.com/<Tenant-ID>/saml2
    AssertionConsumerServiceURL="https://contoso.applicationname.com/acs"
    xmlns:samlp="urn:oasis:names:tc:SAML:2.0:protocol">
    <Issuer xmlns="urn:oasis:names:tc:SAML:2.0:assertion">https://www.contoso.com</Issuer>
    </samlp:AuthnRequest>

Met de SAML-aanvraag gedecodeerd, controleert u het volgende:

1. **Bestemming** overeenkomen met de SAML Single Sign-On Service-URL ophalen uit Azure Active Directory in de SAML-aanvraag.
 
2. **Certificaatverlener** aanvraag is in het SAML hetzelfde **id** u hebt geconfigureerd voor de toepassing in Azure Active Directory. Azure AD maakt gebruik van de verlener vinden van een toepassing in uw directory.

3. **AssertionConsumerServiceURL** is waar de toepassing wordt verwacht voor het ontvangen van het SAML-token van Azure Active Directory. U kunt deze waarde configureren in Azure Active Directory, maar dit is niet verplicht als deze deel uitmaakt van de SAML-aanvraag.


## <a name="i-see-an-error-on-the-applications-page-after-signing-in"></a>Een fout op de pagina van de toepassing wordt weergegeven na het aanmelden

In dit scenario accepteert de toepassing geen het antwoord dat is uitgegeven door Azure AD. Het is belangrijk dat u controleert of het antwoord van Azure AD met het SAML-token met de leverancier van de toepassing om te weten wat ontbreekt of is onjuist is. 

### <a name="how-can-i-get-and-review-the-saml-response"></a>Hoe kan ik ophalen en bekijk de SAML-reactie?

Het antwoord van Azure AD met het SAML-token is doorgaans een dat na een HTTP 302-omleiding van https://login.microsoftonline.com en wordt verzonden naar de geconfigureerde **antwoord-URL** van de toepassing. 

U kunt het SAML-token weergeven door deze regel te selecteren en vervolgens te klikken op de **Inspectors > webformulieren** tabblad in het rechterpaneel. Van daaruit met de rechtermuisknop op de **SAMLResponse** waarde en selecteert u **verzenden naar TextWizard**. Selecteer vervolgens **van Base64** van de **transformeren** menu voor het decoderen van het token en de inhoud van een andere Base64 decoder gebruiken weergeven. 

U kunt ook kopiëren met de waarde in de **SAMLrequest** en gebruik een andere Base64-codering.

Ga naar [fout op de pagina na het aanmelden van de toepassing](https://docs.microsoft.com/azure/active-directory/application-sign-in-problem-federated-sso-gallery?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML) richtlijnen voor meer informatie over welke ontbrekende of onjuiste in het SAML-antwoord worden kunnen voor probleemoplossing.

Voor informatie over het controleren van het SAML antwoord gaat u naar het artikel [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference?/?WT.mc_id=DOC_AAD_How_to_Debug_SAML#response).


## <a name="related-articles"></a>Verwante artikelen
* [Article Index for Application Management in Azure Active Directory](../active-directory-apps-index.md) (Artikelindex voor toepassingsbeheer in Azure Active Directory)
* [Configuring single sign-on to applications that are not in the Azure Active Directory application gallery](../application-config-sso-how-to-configure-federated-sso-non-gallery.md) (Eenmalige aanmelding configureren voor toepassingen die zich niet in de Azure Active Directory-toepassingsgalerie bevinden)
* [Het aanpassen van uitgegeven Claims in het SAML-Token voor vooraf geïntegreerde Apps](active-directory-saml-claims-customization.md)

<!--Image references-->
[1]: ../media/active-directory-saml-debugging/fiddler.png
[2]: ../media/active-directory-saml-debugging/error.png

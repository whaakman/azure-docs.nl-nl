---
title: Problemen met aanmelden bij een toepassing in de galerie geconfigureerd voor federatieve eenmalige aanmelding | Microsoft Docs
description: Richtlijnen voor de specifieke fouten tijdens het aanmelden bij een toepassing die u hebt geconfigureerd voor SAML gebaseerde federatieve eenmalige aanmelding met Azure AD
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/18/2019
ms.author: celested
ms.reviewer: luleon, asteen
ms.collection: M365-identity-device-management
ms.openlocfilehash: 623d684f701df8b1a7c4b84a2bd3840f039ad174
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58312696"
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Problemen met aanmelden bij een toepassing in de galerie geconfigureerd voor federatieve eenmalige aanmelding

Om op te lossen de aanmeldingsproblemen hieronder, raden we dat u volgt u deze suggesties voor het ophalen van betere diagnose en het automatiseren van de stappen voor het oplossen:

- Installeer de [mijn Apps beveiligen browserextensie](access-panel-extension-problem-installing.md) voor Azure Active Directory (Azure AD) voor betere diagnose en oplossingen bij het gebruik van de tests-ervaring in Azure portal.
- Reproduceer het fout bij het gebruik van de test-ervaring in de configuratiepagina van de app in Azure portal. Meer informatie over [fouten opsporen in SAML-toepassingen op basis van eenmalige aanmelding](../develop/howto-v1-debug-saml-sso-issues.md)


## <a name="application-not-found-in-directory"></a>Toepassing is niet gevonden in de directory

*Fout AADSTS70001: Toepassing met id ' https:\//contoso.com' is niet gevonden in de map*.

**Mogelijke oorzaak**

De `Issuer` kenmerk verzonden vanuit de toepassing naar Azure AD in de SAML-aanvraag komt niet overeen met de id-waarde die geconfigureerd voor de toepassing in Azure AD.

**Resolutie**

Zorg ervoor dat de `Issuer` kenmerk in de SAML-aanvraag overeenkomt met de id-waarde die is geconfigureerd in Azure AD. Als u de [ervaring testen](../develop/howto-v1-debug-saml-sso-issues.md) in Azure portal met de browserextensie mijn Apps-beveiligen, moet u niet handmatig als volgt te werk.

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

1.  Open de **Azure Active Directory-extensie** hiervoor **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

1.  Type **'Azure Active Directory'** in het zoekvak van filter en selecteer de **Azure Active Directory** item.

1.  Selecteer **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

1.  Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

1.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

1.  Nadat de toepassing wordt geladen, opent u **Basic SAML-configuratie**. Controleer of dat de waarde in het tekstvak id overeenkomt met de waarde voor de id-waarde in de volgende fout weergegeven.



## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Het antwoordadres komt niet overeen met de antwoordadressen die zijn geconfigureerd voor de toepassing

*Fout AADSTS50011: Het antwoordadres op dat ' https:\//contoso.com' komt niet overeen met de antwoordadressen die is geconfigureerd voor de toepassing*

**Mogelijke oorzaak**

De `AssertionConsumerServiceURL` waarde in de SAML-aanvraag komt niet overeen met de antwoord-URL-waarde of patroon dat is geconfigureerd in Azure AD. De `AssertionConsumerServiceURL` waarde in de SAML-aanvraag is de URL die u in de volgende fout ziet.

**Resolutie**

Zorg ervoor dat de `AssertionConsumerServiceURL` waarde in de SAML-aanvraag overeenkomt met de waarde van de antwoord-URL geconfigureerd in Azure AD. Als u de [ervaring testen](../develop/howto-v1-debug-saml-sso-issues.md) in Azure portal met de browserextensie mijn Apps-beveiligen, moet u niet handmatig als volgt te werk.

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

1.  Open de **Azure Active Directory-extensie** hiervoor **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

1.  Type **'Azure Active Directory'** in het zoekvak van filter en selecteer de **Azure Active Directory** item.

1.  Selecteer **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

1.  Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

1.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

1.  Nadat de toepassing wordt geladen, opent u **Basic SAML-configuratie**. Controleer of werk de waarde in het tekstvak de antwoord-URL zodat deze overeenkomen met de `AssertionConsumerServiceURL` waarde in de SAML-aanvraag.    
    
Nadat u de antwoord-URL-waarde in Azure AD hebt bijgewerkt en deze overeenkomt met de waarde die door de toepassing in de SAML-aanvraag is verzonden, zou het mogelijk om aan te melden bij de toepassing.

## <a name="user-not-assigned-a-role"></a>Gebruiker met een niet toegewezen

*Error AADSTS50105: De aangemelde gebruiker ' brian\@contoso.com' is niet toegewezen aan een rol voor de toepassing*.

**Mogelijke oorzaak**

De gebruiker heeft geen toegang tot de toepassing in Azure AD is verleend.

**Resolutie**

Als u wilt toewijzen een of meer gebruikers rechtstreeks aan een toepassing, de volgende stappen uit te voeren. Als u de [ervaring testen](../develop/howto-v1-debug-saml-sso-issues.md) in Azure portal met de browserextensie mijn Apps-beveiligen, moet u niet handmatig als volgt te werk.

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder**.

1.  Open de **Azure Active Directory-extensie** hiervoor **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

1.  Type **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

1.  Selecteer **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

1.  Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

1.  Selecteer uit de lijst met toepassingen die u wilt toewijzen aan een gebruiker aan.

1.  Zodra de toepassing wordt geladen, selecteert u **gebruikers en groepen** in het navigatiemenu aan de van de toepassing.

1.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

1.  Selecteer de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

1. In de **zoeken op naam of e-mailadres** het zoekvak, typ de volledige naam of e-mailadres van de gebruiker die u wilt toevoegen.

1. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker de **geselecteerde** lijst.

1. **Optioneel:** Als u wilt **toevoegen van meer dan één gebruiker**, typ een andere volledige naam of e-mailadres in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje in om toe te voegen van de gebruiker de **geselecteerd**  lijst.

1. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

1. **Optioneel:** Klik op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

1. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, is de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten.

## <a name="not-a-valid-saml-request"></a>Geen geldige SAML-aanvraag

*Fout AADSTS75005: De aanvraag is geen geldige Saml2-protocol-bericht.*

**Mogelijke oorzaak**

Azure AD biedt geen ondersteuning voor de SAML-aanvraag verzonden door de toepassing voor eenmalige aanmelding. Enkele veelvoorkomende problemen zijn:

-   Ontbrekende vereiste velden in de SAML-aanvraag
-   Gecodeerde methode voor SAML-aanvraag

**Resolutie**

1. Leg de SAML-aanvraag. Volg de zelfstudie [fouten opsporen in SAML gebaseerde eenmalige aanmelding tot toepassingen in Azure AD](../develop/howto-v1-debug-saml-sso-issues.md) voor informatie over het vastleggen van de SAML-aanvraag.

1. Neem contact op met de leverancier van de toepassing en delen van de volgende gegevens:

   -   SAML-aanvraag

   -   [Vereisten voor Azure AD-Single Sign-on SAML-protocol](../develop/single-sign-on-saml-protocol.md)

Leverancier van de toepassing moet valideren dat ze de Azure AD-SAML-implementatie ondersteuning voor eenmalige aanmelding.

## <a name="misconfigured-application"></a>Onjuist geconfigureerde toepassing

*Fout AADSTS650056: Onjuist geconfigureerde toepassing. Dit kan een van de volgende oorzaken hebben: De client heeft geen machtigingen voor 'AAD Graph' in de machtigingen die zijn aangevraagd in de registratie van de toepassing van de client niet weergegeven. Of de beheerder heeft niet toegestaan in de tenant. Of Controleer de toepassings-id in de aanvraag zodat deze overeenkomt met de geconfigureerde client toepassings-id. Neem contact op met uw beheerder om te herstellen van de configuratie of toestemming geven namens de tenant.* .

**Mogelijke oorzaak**

De `Issuer` kenmerk verzonden vanuit de toepassing naar Azure AD in de SAML-aanvraag komt niet overeen met de id-waarde die is geconfigureerd voor de toepassing in Azure AD.

**Resolutie**

Zorg ervoor dat de `Issuer` kenmerk in de SAML-aanvraag overeenkomt met de id-waarde die is geconfigureerd in Azure AD. Als u de [ervaring testen](../develop/howto-v1-debug-saml-sso-issues.md) in Azure portal met de browserextensie mijn Apps-beveiligen, moet u niet handmatig als volgt te werk:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

1.  Open de **Azure Active Directory-extensie** hiervoor **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

1.  Type **'Azure Active Directory'** in het zoekvak van filter en selecteer de **Azure Active Directory** item.

1.  Selecteer **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

1.  Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

1.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

1.  Nadat de toepassing wordt geladen, opent u **Basic SAML-configuratie**. Controleer of dat de waarde in het tekstvak id overeenkomt met de waarde voor de id-waarde in de volgende fout weergegeven.


## <a name="certificate-or-key-not-configured"></a>Certificaat of de sleutel niet geconfigureerd

*Fout AADSTS50003: Er is geen ondertekeningssleutel die is geconfigureerd.*

**Mogelijke oorzaak**

Het toepassingsobject is beschadigd en Azure AD niet wordt herkend door het certificaat dat is geconfigureerd voor de toepassing.

**Resolutie**

Als u wilt verwijderen en maak een nieuw certificaat, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

1. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

1. Type **'Azure Active Directory'** in het zoekvak van filter en selecteer de **Azure Active Directory** item.

1. Selecteer **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

1. Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

1. Selecteer de toepassing die u wilt configureren van eenmalige aanmelding

1. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

1. Selecteer **nieuw certificaat maken** onder de **SAML-certificaat voor ondertekening** sectie.

1. Selecteer de datum van afloop en klik vervolgens op **opslaan**.

1. Controleer **nieuw certificaat activeren** voor de onderdrukking van het actieve certificaat. Klik vervolgens op **opslaan** aan de bovenkant van het deelvenster en accepteer om het rollovercertificaat te activeren.

1. Onder de **SAML-handtekeningcertificaat** sectie, klikt u op **verwijderen** verwijderen van de **ongebruikt** certificaat.

## <a name="saml-request-not-present-in-the-request"></a>SAML-aanvraag is niet aanwezig in de aanvraag

*Fout AADSTS750054: SAMLRequest of SAMLResponse zijn aanwezig als tekenreeks queryparameters in HTTP-aanvraag voor het omleiden van SAML-binding.*

**Mogelijke oorzaak**

Er is geen Azure AD kunnen identificeren van de SAML-aanvraag in de URL-parameters in de HTTP-aanvraag. Dit kan gebeuren als de toepassing niet van HTTP-omleiding gebruikmaakt wanneer u de SAML-aanvraag verzendt naar Azure AD-binding.

**Resolutie**

De toepassing nodig heeft voor het verzenden van de SAML-aanvraag die is gecodeerd in de location-header met behulp van HTTP omleiden binding. Voor meer informatie over het implementeren, leest u de sectie HTTP omleiden Binding in de [SAML-protocol-specificatiedocument](https://docs.oasis-open.org/security/saml/v2.0/saml-bindings-2.0-os.pdf).

## <a name="azure-ad-is-sending-the-token-to-an-incorrect-endpoint"></a>Azure AD wordt het token verzonden naar een onjuiste eindpunt

**Mogelijke oorzaak**

Tijdens eenmalige aanmelding, vertrouwen als de aanmeldingsaanvraag geen een expliciete antwoord-URL (URL van de Bevestigingsconsumerservice) bevat en vervolgens u Azure AD een van de geconfigureerde selecteert wordt URL's voor de toepassing. Zelfs als de toepassing een expliciete antwoord-URL die is geconfigureerd heeft, wordt de gebruiker mogelijk omgeleid https://127.0.0.1:444. 

Toen de toepassing werd toegevoegd als niet-galerie-app, is deze antwoord-URL in Azure Active Directory gemaakt als standaardwaarde. Dit gedrag is gewijzigd en deze URL wordt niet meer standaard toegevoegd in Azure Active Directory. 

**Resolutie**

Verwijder de ongebruikte antwoord-URL's geconfigureerd voor de toepassing.

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** hiervoor **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Type **'Azure Active Directory'** in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Selecteer **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Selecteer **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

6.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, opent u **Basic SAML-configuratie**. In de **antwoord-URL (URL van de Bevestigingsconsumerservice)**, niet-gebruikte verwijderen of standaard antwoord-URL's die zijn gemaakt door het systeem. Bijvoorbeeld `https://127.0.0.1:444/applications/default.aspx`.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Probleem bij het aanpassen van de SAML-claims verzonden naar een toepassing

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [Claims toewijzen in Azure Active Directory](../develop/active-directory-claims-mapping.md).

## <a name="next-steps"></a>Volgende stappen

[Fouten opsporen in SAML gebaseerde eenmalige aanmelding tot toepassingen in Azure AD](../develop/howto-v1-debug-saml-sso-issues.md)

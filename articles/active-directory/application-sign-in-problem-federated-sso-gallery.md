---
title: Problemen met aanmelden bij een galerij-toepassing is geconfigureerd voor federatieve eenmalige aanmelding | Microsoft Docs
description: Richtlijnen voor de specifieke fouten bij het ondertekenen van een toepassing die u hebt geconfigureerd voor op basis van SAML federatieve eenmalige aanmelding met Azure AD
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 0fc5a8eb3d033d60bf6082d61bf1698924ab91c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="problems-signing-in-to-a-gallery-application-configured-for-federated-single-sign-on"></a>Problemen met aanmelden bij een galerij-toepassing die is geconfigureerd voor federatieve eenmalige aanmelding

Om het probleem kunt oplossen, moet u controleren of de configuratie van de toepassing in Azure AD als volgt:

-   U kunt de configuratiestappen voor de toepassing van de galerie van Azure AD hebt gevolgd.

-   De id en de antwoord-URL geconfigureerd in AAD overeen ze verwachte waarden in de toepassing

-   U hebt gebruikers toegewezen aan de toepassing

## <a name="application-not-found-in-directory"></a>Toepassing is niet gevonden in map

*Fout AADSTS70001: Toepassing met id 'https://contoso.com' is niet gevonden in de map*.

**Mogelijke oorzaak**

De verlener kenmerk uit de toepassing naar Azure AD in de SAML-aanvraag verzendt komt niet overeen met de id-waarde die is geconfigureerd in de Azure AD-toepassing.

**Naamomzetting**

Zorg ervoor dat het kenmerk uitgever in de SAML-aanvraag die deze komt overeen met de id die is geconfigureerd in Azure AD:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Ga naar **domein en de URL's** sectie. Controleer of dat wordt de waarde in het tekstvak voor de id die overeenkomt met de waarde voor de id-waarde in de volgende fout weergegeven.

Nadat u de id-waarde hebben bijgewerkt in Azure AD en deze met de waarde verzendt door de toepassing in de SAML-aanvraag overeen komt, moet u mogelijk zijn aan te melden bij de toepassing.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Het antwoordadres komt niet overeen met de antwoordadressen geconfigureerd voor de toepassing.

*Fout AADSTS50011: Het antwoordadres 'https://contoso.com' komt niet overeen met de antwoordadressen geconfigureerd voor de toepassing*

**Mogelijke oorzaak**

De waarde AssertionConsumerServiceURL in de SAML-aanvraag komt niet overeen met de antwoord-URL-waarde of patroon dat is geconfigureerd in Azure AD. De waarde AssertionConsumerServiceURL in de SAML-aanvraag is de URL die u in de fout ziet.

**Naamomzetting**

Zorg ervoor dat de waarde AssertionConsumerServiceURL in de SAML-aanvraag die deze komt overeen met de antwoord-URL die is geconfigureerd in Azure AD.

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Ga naar **domein en de URL's** sectie. Controleer of de waarde in het tekstvak voor de antwoord-URL overeenkomt met de waarde AssertionConsumerServiceURL in de SAML-aanvraag bijwerken.  
    * Als u het tekstvak voor de antwoord-URL niet ziet, selecteert u de **weergeven geavanceerde instellingen voor URL** selectievakje.

Nadat u de antwoord-URL-waarde hebben bijgewerkt in Azure AD en deze met de waarde verzendt door de toepassing in de SAML-aanvraag overeen komt, moet u mogelijk zijn aan te melden bij de toepassing.

## <a name="user-not-assigned-a-role"></a>Gebruiker met een niet toegewezen

*Fout AADSTS50105: De aangemelde gebruiker 'brian@contoso.com' is niet toegewezen aan een rol voor de toepassing*.

**Mogelijke oorzaak**

De gebruiker heeft geen toegang gekregen tot de toepassing in Azure AD.

**Naamomzetting**

Als u wilt toewijzen een of meer gebruikers rechtstreeks naar een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** van navigatiemenu links aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** blade.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** blade.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo voor uw gebruiker toevoegen aan de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met het selecteren van gebruikers, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** blade te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde gebruikers.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten.

## <a name="not-a-valid-saml-request"></a>Geen een geldige SAML aanvragen

*Fout AADSTS75005: De aanvraag is niet een geldig Saml2-protocolbericht.*

**Mogelijke oorzaak**

Azure AD biedt geen ondersteuning voor SAML Request verzonden door de toepassing voor eenmalige aanmelding. Enkele veelvoorkomende problemen zijn:

-   Ontbrekende vereiste velden in de SAML-aanvraag

-   SAML gecodeerd aanvraagmethode

**Naamomzetting**

1.  Vastleggen van SAML-aanvraag. Volg de zelfstudie [fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) voor informatie over het vastleggen van de SAML-aanvraag.

2.  Neem contact op met de leverancier van toepassing en -share:

   -   SAML-aanvraag

   -   [Vereisten voor Azure AD Single Sign-on SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Ze moeten valideren deze ondersteuning bieden voor de implementatie van de Azure AD SAML voor eenmalige aanmelding.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Er is geen resource in de lijst requiredResourceAccess

*Fout AADSTS65005: de clienttoepassing toegang tot de resource heeft aangevraagd ' 00000002-0000-0000-c000-000000000000'. Deze aanvraag is mislukt omdat de client niet voor deze bron in de lijst met requiredResourceAccess opgegeven is*.

**Mogelijke oorzaak**

Het toepassingsobject is beschadigd.

**Oplossing: optie 1**

Toevoegen om het probleem kunt oplossen met de unieke id-waarde in de configuratie van Azure AD. Als u wilt toevoegen van een id-waarde, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Zodra de toepassing wordt geladen, klik op de **eenmalige aanmelding** van de toepassing linkerkant navigatiemenu

8.  Onder de **domein en de URL** sectie, controleert u op de **weergeven geavanceerde instellingen voor URL**.

9.  in de **id** textbox Typ een unieke id voor de toepassing.

10. **Sla** de configuratie.


**Optie 2 voor naamomzetting**

Als optie 1 hierboven niet werkt, probeert u de toepassing worden verwijderd uit de directory. Vervolgens toevoegen en configureren van de toepassing, volgt u de volgende stappen uit:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Klik op **verwijderen** op de linkerbovenhoek van de toepassing **overzicht** blade.

8.  Vernieuwen van Azure AD en voegt u de toepassing van de galerie van Azure AD. Configureer vervolgens de toepassing

<span id="_Hlk477190176" class="anchor"></span>Na het opnieuw configureren van de toepassing, moet u het volgende kunnen aanmelden bij de toepassing.

## <a name="certificate-or-key-not-configured"></a>Certificaat of de sleutel niet geconfigureerd

*Fout AADSTS50003: Er is geen ondertekeningssleutel geconfigureerd.*

**Mogelijke oorzaak**

Het toepassingsobject is beschadigd en Azure AD niet wordt herkend door het certificaat dat is geconfigureerd voor de toepassing.

**Naamomzetting**

Als u wilt verwijderen en maak een nieuw certificaat, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

 * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Klik op **nieuw certificaat maken** onder de **SAML handtekeningcertificaat** sectie.

9.  Selecteer de vervaldatum. Klik vervolgens op **opslaan.**

10. Controleer **nieuwe certificaat activeren** voor het onderdrukken van het actieve certificaat. Klik vervolgens op **opslaan** boven aan de blade en voor het activeren van de rollovercertificaat accepteren.

11. Onder de **SAML-certificaat voor ondertekening van** sectie, klikt u op **verwijderen** verwijderen van de **ongebruikt** certificaat.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Probleem bij het aanpassen van de SAML-claims verzonden naar een toepassing

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [toewijzen in Azure Active Directory-Claims](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Fouten opsporen in op basis van SAML eenmalige aanmelding tot toepassingen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging)

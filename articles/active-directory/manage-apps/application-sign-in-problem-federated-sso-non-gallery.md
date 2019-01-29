---
title: Problemen met aanmelden bij een toepassing buiten de galerie geconfigureerd voor federatieve eenmalige aanmelding | Microsoft Docs
description: Richtlijnen voor de specifieke problemen die zich voordoen kunnen bij het aanmelden bij een toepassing die is geconfigureerd voor SAML gebaseerde federatieve eenmalige aanmelding met Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: daveba
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 279f0290bb873e06146ec83d921caa50d482f924
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55182877"
---
# <a name="problems-signing-in-to-a-non-gallery-application-configured-for-federated-single-sign-on"></a>Problemen met aanmelden bij een toepassing buiten de galerie geconfigureerd voor federatieve eenmalige aanmelding

Als u wilt uw probleem op te lossen, moet u controleren of de configuratie van de toepassing in Azure AD als volgt:

-   U kunt de configuratiestappen voor de toepassing in Azure AD-galerie hebt gevolgd.

-   De id en de antwoord-URL geconfigureerd in AAD overeen ze verwachte waarden in de toepassing

-   U kunt gebruikers hebt toegewezen aan de toepassing

## <a name="application-not-found-in-directory"></a>Toepassing is niet gevonden in de directory

*Fout AADSTS70001: Toepassing met id 'https://contoso.com' is niet gevonden in de map*.

**Mogelijke oorzaak**

Uitgever van het kenmerk van de toepassing naar Azure AD in de SAML-aanvraag verzendt, komt niet overeen met de id-waarde die is geconfigureerd in de Azure AD-toepassing.

**Resolutie**

Zorg ervoor dat het kenmerk uitgever in de SAML-aanvraag die deze komt overeen met de id waarde die is geconfigureerd in Azure AD:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  <span id="_Hlk477190042" class="anchor"></span>Ga naar **domein en URL's** sectie. Controleer of dat wordt de waarde in het tekstvak id die overeenkomt met de waarde voor de id-waarde in de volgende fout weergegeven.

Nadat u de id-waarde in Azure AD hebt bijgewerkt en wordt deze die overeenkomt met de waarde verzendt door de toepassing in de SAML-aanvraag, zou het mogelijk om aan te melden bij de toepassing.

## <a name="the-reply-address-does-not-match-the-reply-addresses-configured-for-the-application"></a>Het antwoordadres op dat komt niet overeen met de antwoordadressen die is geconfigureerd voor de toepassing. 

*Fout AADSTS50011: Het antwoordadres https://contoso.com' komt niet overeen met de antwoordadressen die is geconfigureerd voor de toepassing* 

**Mogelijke oorzaak** 

De waarde AssertionConsumerServiceURL in de SAML-aanvraag komt niet overeen met de antwoord-URL-waarde of patroon dat is geconfigureerd in Azure AD. De waarde AssertionConsumerServiceURL in de SAML-aanvraag is de URL die u in de volgende fout ziet. 

**Resolutie** 

Zorg ervoor dat de waarde AssertionConsumerServiceURL in de SAML-aanvraag die deze komt overeen met de antwoord-URL waarde die is geconfigureerd in Azure AD. 
 
1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.** 

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie. 

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item. 

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory. 

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven. 

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**
  
6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Ga naar **domein en URL's** sectie. Controleer of de waarde in het tekstvak de antwoord-URL zodat deze overeenkomen met de waarde AssertionConsumerServiceURL in de SAML-aanvraag wordt bijgewerkt.

  * Als u het antwoord-URL-tekstvak niet ziet, selecteert u de **geavanceerde URL-instellingen weergeven** selectievakje. 

Nadat u de antwoord-URL-waarde in Azure AD hebt bijgewerkt en wordt deze die overeenkomt met de waarde verzendt door de toepassing in de SAML-aanvraag, zou het mogelijk om aan te melden bij de toepassing.

## <a name="user-not-assigned-a-role"></a>Gebruiker met een niet toegewezen

*Error AADSTS50105: De aangemelde gebruiker 'brian@contoso.com' is niet toegewezen aan een rol voor de toepassing*

**Mogelijke oorzaak**

De gebruiker heeft geen toegang tot de toepassing in Azure AD is verleend.

**Resolutie**

Als u wilt toewijzen een of meer gebruikers rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** in het navigatiemenu aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9.  Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** het zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten.

## <a name="not-a-valid-saml-request"></a>Niet een geldig SAML-aanvraag

*Fout AADSTS75005: De aanvraag is geen geldige Saml2-protocol-bericht.*

**Mogelijke oorzaak**

Azure AD biedt geen ondersteuning voor de SAML-aanvraag die door de toepassing is verzonden voor eenmalige aanmelding. Enkele veelvoorkomende problemen zijn:

-   Ontbrekende vereiste velden in de SAML-aanvraag

-   Methode voor SAML-aanvraag gecodeerd

**Resolutie**

1.  SAML-aanvraag vastleggen. Volg de zelfstudie op [fouten opsporen in SAML gebaseerde eenmalige aanmelding tot toepassingen in Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-saml-debugging) voor informatie over het vastleggen van de SAML-aanvraag.

2.  Neem contact op met de leverancier van toepassing en -share:

    -   SAML-aanvraag

    -   [Vereisten voor Azure AD-Single Sign-on SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

Ze moeten valideren deze ondersteuning bieden voor de Azure AD-SAML-implementatie voor eenmalige aanmelding.

## <a name="no-resource-in-requiredresourceaccess-list"></a>Er is geen resource in de lijst met requiredResourceAccess

*Error AADSTS65005: De clienttoepassing heeft toegang aangevraagd tot resource ' 00000002-0000-0000-c000-000000000000'. Deze aanvraag is mislukt omdat de client niet voor deze resource in de lijst met requiredResourceAccess opgegeven is*.

**Mogelijke oorzaak**

Het toepassingsobject is beschadigd.

**Resolutie**

Als oplossing voor het probleem door de toepassing te verwijderen uit de map. Vervolgens kunt toevoegen en configureren van de toepassing, de volgende stappen:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Klik op **verwijderen** in de linkerbovenhoek van de toepassing **overzicht** deelvenster.

8.  Vernieuwen van Azure AD en de toepassing uit de galerie van Azure AD toevoegen. Configureer vervolgens de toepassing opnieuw uit.

Na het opnieuw configureren van de toepassing, moet u het volgende kunnen aanmelden bij de toepassing.

## <a name="certificate-or-key-not-configured"></a>Certificaat of de sleutel niet geconfigureerd

Fout AADSTS50003: Er is geen ondertekeningssleutel die is geconfigureerd.

**Mogelijke oorzaak**

Het toepassingsobject is beschadigd en Azure AD niet wordt herkend door het certificaat dat is geconfigureerd voor de toepassing.

**Resolutie**

Als u wilt verwijderen en maak een nieuw certificaat, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Klik op **nieuw certificaat maken** onder de **SAML-certificaat voor ondertekening** sectie.

9.  Selecteer vervaldatum. Klik vervolgens op **opslaan.**

10. Controleer **nieuw certificaat activeren** voor de onderdrukking van het actieve certificaat. Klik vervolgens op **opslaan** aan de bovenkant van het deelvenster en accepteer om het rollovercertificaat te activeren.

11. Onder de **SAML-handtekeningcertificaat** sectie, klikt u op **verwijderen** verwijderen van de **ongebruikt** certificaat.

## <a name="problem-when-customizing-the-saml-claims-sent-to-an-application"></a>Probleem bij het aanpassen van de SAML-claims verzonden naar een toepassing

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [Claims toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Vereisten voor Azure AD-Single Sign-on SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference)

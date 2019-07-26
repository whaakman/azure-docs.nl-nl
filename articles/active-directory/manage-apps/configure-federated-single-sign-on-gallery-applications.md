---
title: Federatieve eenmalige aanmelding configureren voor een toepassing in de Azure AD-galerie | Microsoft Docs
description: Federatieve eenmalige aanmelding configureren voor een bestaande Azure AD Gallery-toepassing en zelf studies gebruiken om snel aan de slag te gaan
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: mimart
ms.collection: M365-identity-device-management
ROBOTS: NOINDEX
ms.openlocfilehash: bb5d2c693047dd0aa53430ba531dfd246cc77be9
ms.sourcegitcommit: 198c3a585dd2d6f6809a1a25b9a732c0ad4a704f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/23/2019
ms.locfileid: "68422526"
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Federatieve eenmalige aanmelding configureren voor een toepassing in de Azure AD-galerie

Voor alle toepassingen in de Azure AD-galerie die is ingeschakeld met de Enter prise-functionaliteit voor eenmalige aanmelding, is een stapsgewijze zelf studie beschikbaar. U kunt toegang krijgen tot de [lijst met zelf studies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor gedetailleerde stapsgewijze instructies.

## <a name="overview-of-steps-required"></a>Overzicht van de stappen die vereist zijn
Als u een toepassing vanuit de Azure AD-galerie wilt configureren, moet u het volgende doen:

-   [Een toepassing toevoegen vanuit de Azure AD-galerie](#add-an-application-from-the-azure-ad-gallery)

-   [De meta gegevens waarden van de toepassing configureren in azure AD (aanmelden op URL, id, antwoord-URL)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Gebruikers-id selecteren en gebruikers kenmerken toevoegen die naar de toepassing moeten worden verzonden](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD-meta gegevens en-certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Meta gegevens van Azure AD in de toepassing configureren (aanmeldings-URL, verlener, afmeldings-URL en certificaat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Gebruikers toewijzen aan de toepassing](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen vanuit de Azure AD-galerie

Voer de volgende stappen uit om een toepassing toe te voegen vanuit de Azure AD-galerie:

1.  Open de [Azure Portal](https://portal.azure.com) en meld u aan als **globale beheerder** of **co-** beheerder

2.  Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5.  Klik op de knop **toevoegen** in de rechter bovenhoek van het deel venster **bedrijfs toepassingen** .

6.  Typ de naam van de toepassing in het tekstvak **Geef een naam** op in de sectie **toevoegen vanuit de galerie** .

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toevoegt, kunt u de naam ervan wijzigen in het tekstvak **naam** .

9.  Klik op de knop **toevoegen** om de toepassing toe te voegen.

Na een korte periode kunt u het configuratie venster van de toepassing bekijken.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Eenmalige aanmelding configureren voor een toepassing vanuit de Azure AD-galerie

Volg de onderstaande stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1. Open de [**Azure Portal**](https://portal.azure.com/) en meld u aan als **globale beheerder** of **co-** beheerder.

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing waarvoor u eenmalige aanmelding wilt configureren.

7. Zodra de toepassing is geladen, klikt u op de **eenmalige aanmelding** in het navigatie menu aan de linkerkant van de toepassing.

8. Selecteer **op SAML gebaseerde aanmelding** in de **modus** vervolg keuzelijst.

9. Voer de vereiste waarden in het **domein en de url's in.** U moet deze waarden ophalen van de leverancier van de toepassing.

   1. De aanmeldings-URL is een vereiste waarde om de toepassing te configureren als door SP geïnitieerde SSO. Voor sommige toepassingen is de id ook een vereiste waarde.

   2. Als u de toepassing wilt configureren als door IdP geïnitieerde SSO, de antwoord-URL is een vereiste waarde. Voor sommige toepassingen is de id ook een vereiste waarde.

10. **Optioneel:** Klik op **Geavanceerde URL-instellingen weer geven** als u de niet-vereiste waarden wilt zien.

11. Selecteer in de **gebruikers kenmerken**de unieke id voor uw gebruikers in de vervolg keuzelijst **gebruikers-id** .

12. **Optioneel:** Klik op **alle andere gebruikers kenmerken weer geven en bewerken** om de kenmerken te bewerken die naar de toepassing in het SAML-token moeten worden verzonden wanneer gebruikers zich aanmelden.

    Een kenmerk toevoegen:
   
    1. Klik op **kenmerk toevoegen**. Voer de **naam** in en selecteer de **waarde** in de vervolg keuzelijst.

    1. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

13. Klik **op &lt;toepassings naam&gt; configureren** voor toegang tot documentatie over het configureren van eenmalige aanmelding in de toepassing. Daarnaast hebt u de Url's van de meta gegevens en het certificaat dat vereist is voor de installatie van SSO met de toepassing.

14. Klik op **Opslaan** om de configuratie op te slaan.

15. Gebruikers toewijzen aan de toepassing.

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Gebruikers-id selecteren en gebruikers kenmerken toevoegen die naar de toepassing moeten worden verzonden

Volg de onderstaande stappen om de gebruikers-id te selecteren of gebruikers kenmerken toe te voegen:

1. Open de [**Azure Portal**](https://portal.azure.com/) en meld u aan als **globale beheerder** of **co-beheerder.**

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing waarvoor u eenmalige aanmelding hebt geconfigureerd.

7. Zodra de toepassing is geladen, klikt u op de **eenmalige aanmelding** in het navigatie menu aan de linkerkant van de toepassing.

8. Selecteer in de sectie **gebruikers kenmerken** de unieke id voor uw gebruikers in de vervolg keuzelijst **gebruikers-id** . De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

   >[!NOTE] 
   >Azure AD Selecteer de indeling voor het NameID-kenmerk (gebruikers-id) op basis van de geselecteerde waarde of de indeling die door de toepassing is aangevraagd in het SAML-AuthRequest. Ga naar het [SAML-protocol voor eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) via de sectie NameIDPolicy voor meer informatie.
   >
   >

9. Om gebruikers kenmerken toe te voegen, klikt u op **alle andere gebruikers kenmerken weer geven en bewerken** om de kenmerken te bewerken die naar de toepassing in het SAML-token moeten worden verzonden wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:
  
   1. Klik op **kenmerk toevoegen**. Voer de **naam** in en selecteer de **waarde** in de vervolg keuzelijst.

   2. Klik op **Opslaan**. U kunt het nieuwe kenmerk in de tabel zien.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>De meta gegevens of het certificaat van Azure AD downloaden

Volg de onderstaande stappen om de meta gegevens van de toepassing of het certificaat te downloaden van Azure AD:

1. Open de [**Azure Portal**](https://portal.azure.com/) en meld u aan als **globale beheerder** of **co-beheerder.**

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   *  Als u de toepassing die u wilt weer geven hier niet ziet, gebruikt u het **filter** besturings element boven aan de **lijst alle toepassingen** en stelt u de optie voor het **weer geven** van **alle toepassingen**in.

6. Selecteer de toepassing waarvoor u eenmalige aanmelding hebt geconfigureerd.

7. Zodra de toepassing is geladen, klikt u op de **eenmalige aanmelding** in het navigatie menu aan de linkerkant van de toepassing.

8. Ga naar de sectie **SAML-handtekening certificaat** en klik vervolgens op kolom waarde **downloaden** . Afhankelijk van wat de toepassing nodig heeft om eenmalige aanmelding te configureren, ziet u de optie voor het downloaden van de meta gegevens-XML of het certificaat.

Azure AD biedt ook een URL om de meta gegevens op te halen. Volg dit patroon om de meta gegevens-URL op te halen die specifiek is voor de toepassing:`https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`

## <a name="assign-users-to-the-application"></a>Gebruikers toewijzen aan een toepassing

Als u wilt toewijzen een of meer gebruikers rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **uitbrei ding Azure Active Directory** door te klikken op **alle services** boven aan het hoofd navigatie menu aan de linkerkant.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfs toepassingen** in het navigatie menu Azure Active Directory aan de linkerkant.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7. Zodra de toepassing is geladen, klikt u op **gebruikers en groepen** in het navigatie menu aan de linkerkant van de toepassing.

8. Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9. Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u **meer dan één gebruiker wilt toevoegen**, typt u een andere **volledige naam** of een ander **e-mail adres** in het zoekvak **zoeken op naam of e-mail adres** en klikt u op het selectie vakje om deze gebruiker aan de **geselecteerde** lijst toe te voegen.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>De SAML-claims aanpassen die worden verzonden naar een toepassing

Zie [claim toewijzing in azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie over het aanpassen van de SAML-kenmerk claims die naar uw toepassing worden verzonden.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)




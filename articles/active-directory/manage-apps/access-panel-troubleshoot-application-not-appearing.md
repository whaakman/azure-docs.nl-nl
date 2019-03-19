---
title: Een toegewezen toepassing wordt niet weergegeven in het toegangsvenster | Microsoft Docs
description: Waarom een toepassing wordt niet weergegeven in het toegangsvenster
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
ms.topic: article
ms.date: 09/09/2018
ms.author: celested
ms.reviwer: japere
ms.collection: M365-identity-device-management
ms.openlocfilehash: 9bac0e78c53657cda1716dd8ab6ffd0e2453290f
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58117418"
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a>Een toegewezen toepassing wordt niet weergegeven in het toegangsvenster

Het toegangsvenster is een portal op Internet, waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) om te bekijken en starten van cloud-gebaseerde toepassingen die Azure AD-beheerder heeft deze toegang verleend aan. Deze toepassingen zijn namens de gebruiker in de Azure AD-portal geconfigureerd. De toepassing moet worden geconfigureerd en toegewezen aan de gebruiker of een groep die de gebruiker een lid van is om te zien van de toepassing in het toegangsvenster.

Het type van een gebruiker wordt mogelijk weergegeven apps kunnen worden onderverdeeld in de volgende categorieën:

-   Office 365-toepassingen

-   Microsoft en derden door toepassingen die zijn geconfigureerd met op basis van een federatieve eenmalige aanmelding

-   Wachtwoord gebaseerde SSO-toepassingen

-   Toepassingen met bestaande oplossingen voor eenmalige aanmelding

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Als een toepassing is zojuist hebt toegevoegd aan een gebruiker, proberen aan te melden en afmelden opnieuw in het toegangsvenster van de gebruiker na een paar minuten om te zien als de toepassing wordt toegevoegd.

-   Als u een licentie is verwijderd uit een gebruiker of groep is de gebruiker dat lid is van dit duurt lang duren, afhankelijk van de grootte en complexiteit van de groep voor wijzigingen worden aangebracht. Kan extra tijd voordat het toegangsvenster aan te melden.

## <a name="problems-related-to-application-configuration"></a>Problemen met betrekking tot de toepassing configureren

Een toepassing kan niet worden weergegeven in het toegangsvenster van een gebruiker omdat deze niet correct is geconfigureerd. Hieronder vindt u enkele manieren waarop u problemen met betrekking tot de toepassingsconfiguratie van de kunt oplossen:

-   [Het configureren van eenmalige aanmelding in voor een toepassing in Azure AD-galerie](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Het configureren van eenmalige aanmelding in voor een toepassing buiten de galerie](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Een wachtwoord eenmalige aanmelding toepassing configureren voor een toepassing in Azure AD-galerie](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Een wachtwoord eenmalige aanmelding toepassing configureren voor een toepassing buiten de galerie](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van eenmalige aanmelding in voor een toepassing in Azure AD-galerie

Alle toepassingen in de Azure AD-galerie ingeschakeld met Enterprise Single Sign-On-functionaliteit is een stapsgewijze zelfstudie beschikbaar. U hebt toegang tot de [lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor een stapsgewijze instructies voor details.

Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   [Een toepassing toevoegen via de Azure AD-galerie](#add-an-application-from-the-azure-ad-gallery)

-   [Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [De metagegevens van de Azure AD en het certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Waarden voor metagegevens van Azure AD configureren in de toepassing (aanmelding URL, uitgever, afmeldings-URL en certificaat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen via de Azure AD-galerie

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** tekstvak van de **toevoegen vanuit de galerie** sectie, typt u de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toe te voegen, kunt u de naam van de **naam** tekstvak.

9.  Klik op **toevoegen** knop om toe te voegen van de toepassing.

Na een korte periode, kunt u zich aan het deelvenster van de configuratie van de toepassing zien.

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Configureren van eenmalige aanmelding voor een toepassing uit de Azure AD-galerie

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Selecteer **SAML gebaseerde aanmelding** uit de **modus** vervolgkeuzelijst.

9. Voer de vereiste waarden in **domein en URL's.** U krijgt deze waarden van de leverancier van de toepassing.

   1. Het is een vereiste waarde voor het configureren van de toepassing als Serviceprovider geïnitieerde eenmalige aanmelding, de aanmeldings-URL. De id is voor sommige toepassingen, ook een vereiste waarde.

   2. Het is een vereiste waarde voor het configureren van de toepassing als id-provider geïnitieerde eenmalige aanmelding, de antwoord-URL. De id is voor sommige toepassingen, ook een vereiste waarde.

10. **Optioneel:** klikt u op **geavanceerde URL-instellingen weergeven** als u wilt zien van de waarden niet vereist.

11. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

12. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

    Een kenmerk toevoegen:

    1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

    2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

13. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang documentatie over het configureren van eenmalige aanmelding in de toepassing. U hebt ook de metagegevens-URL's en het certificaat voor het instellen van eenmalige aanmelding met de toepassing.

14. Klik op **opslaan** aan de configuratie op te slaan.

15. Gebruikers toewijzen aan de toepassing.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen

Als u wilt de gebruikers-ID selecteren of toevoegen van gebruikerskenmerken, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing niet ziet u hier weergegeven, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar  **Alle toepassingen.**

6. Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Onder de **gebruikerskenmerken** sectie, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst. De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

   >[!NOTE] 
   >Azure AD wordt de indeling voor het kenmerk van NameID (gebruikers-id) op basis van de geselecteerde waarde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest geselecteerd. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
   >
   >

9. Als u wilt toevoegen gebruikerskenmerken, klikt u op **weergeven en bewerk alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **opslaan.** Hier ziet u het nieuwe kenmerk in de tabel.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

   Azure AD biedt geen een URL waarmee u de metagegevens ophalen. De metagegevens kan alleen worden opgehaald als een XML-bestand.

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Het configureren van eenmalige aanmelding in voor een toepassing buiten de galerie

Voor het configureren van een toepassing buiten de galerie, moet u beschikken over Azure AD premium en de toepassing ondersteunt SAML 2.0. Voor meer informatie over de versies van Azure AD, gaat u naar [prijzen voor Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [De metagegevens van de Azure AD en het certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Waarden voor metagegevens van Azure AD configureren in de toepassing (aanmelding URL, uitgever, afmeldings-URL en certificaat)](#configure-the-application-for-password-single-sign-on-1)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren

Voor het configureren van eenmalige aanmelding voor een toepassing die zich niet in de Azure AD-galerie, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6. Klik op **niet in de galerij toepassing** in de **toevoegen aan uw eigen app** sectie.

7. Voer de naam van de toepassing in de **naam** tekstvak.

8. Klik op **toevoegen** knop om toe te voegen van de toepassing.

9. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

10. Selecteer **SAML gebaseerde aanmelding** in de **modus** vervolgkeuzelijst.

11. Voer de vereiste waarden in **domein en URL's.** U krijgt deze waarden van de leverancier van de toepassing.

    1. Voer de antwoord-URL en de id voor het configureren van de toepassing als id-provider geïnitieerde eenmalige aanmelding.

    2.  **Optioneel:** Het is een vereiste waarde voor het configureren van de toepassing als Serviceprovider geïnitieerde eenmalige aanmelding, de aanmeldings-URL.

12. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

13. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

    Een kenmerk toevoegen:

    1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

    2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

14. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang documentatie over het configureren van eenmalige aanmelding in de toepassing. Bovendien hebt u Azure AD-URL's en certificaten die vereist zijn voor de toepassing.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen

Als u wilt de gebruikers-ID selecteren of toevoegen van gebruikerskenmerken, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Onder de **gebruikerskenmerken** sectie, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst. De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

   >[!NOTE] 
   >Azure AD wordt de indeling voor het kenmerk van NameID (gebruikers-id) op basis van de geselecteerde waarde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest geselecteerd. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
   >
   >

9. Als u wilt toevoegen gebruikerskenmerken, klikt u op **weergeven en bewerk alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt geen een URL waarmee u de metagegevens ophalen. De metagegevens kan alleen worden opgehaald als een XML-bestand.

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing in Azure AD-galerie

Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   [Een toepassing toevoegen via de Azure AD-galerie](#add-an-application-from-the-azure-ad-gallery)

-   [De toepassing configureren voor eenmalige aanmelding wachtwoord](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen via de Azure AD-galerie

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  In de **Voer een naam** tekstvak van de **toevoegen vanuit de galerie** sectie, typt u de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toe te voegen, kunt u de naam van de **naam** tekstvak.

9.  Klik op **toevoegen** knop om toe te voegen van de toepassing.

Na een korte periode ziet u deelvenster van de configuratie van de toepassing.

#### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing configureren voor eenmalige aanmelding wachtwoord

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7. Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8. Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9. [Gebruikers toewijzen aan de toepassing](#how-to-assign-a-user-to-an-application-directly).

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor een toepassing buiten de galerie

Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   [Een toepassing buiten de galerie toevoegen](#add-a-non-gallery-application)

-   [De toepassing configureren voor eenmalige aanmelding wachtwoord](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Een toepassing buiten de galerie toevoegen

Als u wilt een toepassing hebt toegevoegd vanuit de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [Azure-portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  Klik op **niet in de galerij-toepassing.**

7.  Voer de naam van uw toepassing in de **naam** tekstvak. Selecteer **toevoegen.**

Na een korte periode, kunt u zich aan het deelvenster van de configuratie van de toepassing zien.

#### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing configureren voor eenmalige aanmelding wachtwoord

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    1.  Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer de modus **wachtwoord gebaseerde aanmelding.**

9.  Voer de **aanmeldings-URL**. Dit is de URL waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij invoeren. Zorg ervoor dat de aanmelding-velden worden weergegeven op de URL.

10. [Gebruikers toewijzen aan de toepassing](#how-to-assign-a-user-to-an-application-directly).

11. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers selecteren en te klikken op **updatereferenties** en de gebruikersnaam en het wachtwoord namens de gebruikers in te voeren. Anders wordt gebruikers gevraagd om in te voeren van de referenties zich bij het starten.

## <a name="problems-related-to-assigning-applications-to-users"></a>Problemen met betrekking tot toepassingen aan gebruikers toewijzen

Een gebruiker mogelijk niet ziet u een toepassing in hun Toegangsvenster omdat ze niet zijn toegewezen aan de toepassing. Hieronder vindt u enkele manieren om te controleren:

-   [Controleren of een gebruiker is toegewezen aan de toepassing](#check-if-a-user-is-assigned-to-the-application)

-   [Een gebruiker aan een toepassing rechtstreeks toewijzen](#how-to-assign-a-user-to-an-application-directly)

-   [Controleren of een gebruiker is toegewezen aan een licentie met betrekking tot de toepassing](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Hoe u een licentie toewijzen aan een gebruiker](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a>Controleren of een gebruiker is toegewezen aan de toepassing

Om te zien als een gebruiker is toegewezen aan de toepassing, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

6. **Search** voor de naam van de betreffende toepassing.

7. Klik op **gebruikers en groepen**.

8. Controleer of de gebruiker is toegewezen aan de toepassing.

   * Als dit niet de stappen in 'Hoe u een gebruiker toewijzen aan een toepassing rechtstreeks' om dit te doen.

### <a name="how-to-assign-a-user-to-an-application-directly"></a>Een gebruiker aan een toepassing rechtstreeks toewijzen

Als u wilt toewijzen een of meer gebruikers rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder**.

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7. Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** in het navigatiemenu aan de van de toepassing.

8. Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9. Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige naam** of **e-mailadres** van de gebruiker die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **gebruiker** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de gebruiker of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** het zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen in het toegangsvenster te starten.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Controleren of een gebruiker een licentie met betrekking tot de toepassing

Om te controleren of de toegewezen licenties van een gebruiker, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **gebruikers en groepen** in het navigatiemenu.

5. Klik op **alle gebruikers**.

6. **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7. Klik op **licenties** om te zien welke licenties die de gebruiker die momenteel is toegewezen.

   * Als de gebruiker is toegewezen aan een Office-licentie hierdoor eerste partij Office-toepassingen worden weergegeven in het toegangsvenster van de gebruiker.

### <a name="how-to-assign-a-user-a-license"></a>Een gebruiker een licentie toewijzen 

Als u wilt een licentie toewijzen aan een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de gebruiker die momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item om toe te wijzen granulair producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze gebruiker.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Problemen met betrekking tot toepassingen aan groepen toewijzen

Een gebruiker mogelijk ziet u een toepassing in hun Toegangsvenster omdat ze deel uitmaken van een groep die de toepassing is toegewezen. Hieronder vindt u enkele manieren om te controleren:

-   [Controleren van groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

-   [Een toepassing aan een groep rechtstreeks toewijzen](#how-to-assign-an-application-to-a-group-directly)

-   [Als een gebruiker deel uit van de groep die is toegewezen aan een licentie maakt controleren](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Hoe u een licentie toewijzen aan een groep](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>Controleren van groepslidmaatschappen van een gebruiker

Als u wilt controleren van een groep lidmaatschap, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **gebruikers en groepen** in het navigatiemenu.

5. Klik op **alle gebruikers**.

6. **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7. Klik op **groepen**.

8. Controleer of de gebruiker deel uitmaakt van een groep die is toegewezen aan de toepassing is.

   * Als u wilt verwijderen van de gebruiker uit de groep **klikt u op de rij** van de groep en selecteer verwijderen.

### <a name="how-to-assign-an-application-to-a-group-directly"></a>Een toepassing aan een groep rechtstreeks toewijzen

Als u wilt toewijzen een of meer groepen rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder**.

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5. Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6. Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7. Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** in het navigatiemenu aan de van de toepassing.

8. Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst om de **toevoegen toewijzing** deelvenster.

9. Klik op de **gebruikers en groepen** selector vanaf de **toevoegen toewijzing** deelvenster.

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Klik op het selectievakje naast de profielfoto van de groep of het logo om toe te voegen van de gebruiker naar de **geselecteerde** lijst.

12. **Optioneel:** Als u wilt **toevoegen van meer dan één groep**, type in een andere **volledige groepsnaam** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje in als u wilt toevoegen van deze groep aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** om toe te wijzen van de toepassing aan de geselecteerde groepen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen in het toegangsvenster te starten.

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a>Als een gebruiker deel uit van de groep die is toegewezen aan een licentie maakt controleren

1. Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2. Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3. Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4. Klik op **gebruikers en groepen** in het navigatiemenu.

5. Klik op **alle gebruikers**.

6. **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7. Klik op **groepen**.

8. Klik op de rij van een specifieke groep.

9. Klik op **licenties** om te zien die de groep licenties zijn toegewezen aan.

   * Als de groep is toegewezen aan een Office-licentie die dit mogelijk bepaalde eerste partij Office-toepassingen worden weergegeven in het toegangsvenster van de gebruiker.

### <a name="how-to-assign-a-license-to-a-group"></a>Hoe u een licentie toewijzen aan een groep

Als u wilt een licentie toewijzen aan een groep, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep waarin u geïnteresseerd bent en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien welke licenties die de groep momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item om toe te wijzen granulair producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze groep. Dit kan lang duren, afhankelijk van de grootte en complexiteit van de groep.

>[!NOTE]
>Om dit te doen sneller, kunt u tijdelijk een licentie rechtstreeks aan de gebruiker toewijzen. 
>
>

## <a name="next-steps"></a>Volgende stappen
[Nieuwe gebruikers toevoegen aan Azure Active Directory](./../fundamentals/add-users-azure-active-directory.md)


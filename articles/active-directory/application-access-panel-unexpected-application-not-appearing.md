---
title: Een toegewezen toepassing wordt niet weergegeven in het deelvenster toegang | Microsoft Docs
description: Waarom een toepassing wordt niet weergegeven in het deelvenster toegang
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.reviwer: japere
ms.openlocfilehash: 0abb9e992f408ab6df073891047c9afc7716054f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="an-assigned-application-is-not-appearing-on-the-access-panel"></a>Een toegewezen toepassing wordt niet weergegeven op het toegangsvenster

Het Toegangspaneel is een portal op Internet waarmee een gebruiker met een account voor werk of school in Azure Active Directory (Azure AD) weergeven en starten van cloud-gebaseerde toepassingen die de Azure AD-beheerder heeft deze toegang verleend. Deze toepassingen zijn geconfigureerd namens de gebruiker in de Azure AD-portal. De toepassing moet worden geconfigureerd en toegewezen aan de gebruiker of een groep die de gebruiker is lid van de toepassing in het deelvenster toegang zien.

Het type van een gebruiker ziet u mogelijk apps kunnen worden onderverdeeld in de volgende categorieën:

-   Office 365-toepassingen

-   Microsoft en derden toepassingen die zijn geconfigureerd op basis van een federatieve aanmelding bij

-   SSO-toepassingen op basis van wachtwoorden

-   Toepassingen met bestaande oplossingen voor eenmalige aanmelding

## <a name="general-issues-to-check-first"></a>Algemene problemen om eerst te controleren

-   Als een toepassing is zojuist toegevoegd aan een gebruiker, probeert aan te melden en afmelden opnieuw naar de gebruiker toegang deelvenster na een paar minuten om te zien als de toepassing is toegevoegd.

-   Als u een licentie is verwijderd uit een gebruiker of groep die de gebruiker is dat lid van deze kan lang duren, afhankelijk van de omvang en complexiteit van de groep voor wijzigingen worden aangebracht. Voor extra tijd voordat u zich in het deelvenster toegang toestaan.

## <a name="problems-related-to-application-configuration"></a>Problemen met betrekking tot de configuratie van toepassing

Een toepassing kan niet worden weergegeven in het deelvenster voor een gebruiker toegang omdat deze niet correct is geconfigureerd. Hieronder vindt u enkele manieren waarop u kunt problemen met betrekking tot de configuratie van toepassing:

-   [Het configureren van federatieve eenmalige aanmelding voor een toepassing van de galerie van Azure AD](#how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application)

-   [Het configureren van federatieve eenmalige aanmelding voor de toepassing van een niet-galerie](#how-to-configure-federated-single-sign-on-for-a-non-gallery-application)

-   [Het wachtwoord één aanmelding toepassing voor een toepassing van de galerie van Azure AD configureren](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

-   [Het wachtwoord één aanmelding toepassing voor een toepassing niet galerie configureren](#how-to-configure-password-single-sign-on-for-a-non-gallery-application)

### <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van federatieve eenmalige aanmelding voor een toepassing van de galerie van Azure AD

Alle toepassingen in de galerie van Azure AD ingeschakeld met Enterprise Single Sign-On-functionaliteit is een stapsgewijze zelfstudie beschikbaar. U hebt toegang tot de [lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor een stapsgewijze instructies voor details.

Een toepassing uit de galerie van Azure AD te hoeft configureren:

-   [Een toepassing toevoegen uit de galerie van Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [Waarden voor metagegevens van de toepassing configureren in Azure AD (URL,-id en antwoord-URL van de aanmelding)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selecteer gebruikers-id en gebruikerskenmerken worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD-metagegevens en certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Azure AD metagegevenswaarden configureren in de toepassing (aanmelding URL, uitgever, afmelding URL en certificaat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen uit de galerie van Azure AD

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit te voeren:

1.  Open de [Azure Portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** blade.

6.  In de **Voer een naam** textbox uit de **toevoegen uit de galerie** sectie, typ de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toevoegt, kunt u de naam van de **naam** textbox.

9.  Klik op **toevoegen** knop, de toepassing toevoegen.

Na een korte periode is het mogelijk om te zien van de toepassing configuration blade.

#### <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Eenmalige aanmelding voor een toepassing uit de galerie van Azure AD configureren

Volg de onderstaande stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Selecteer **op basis van SAML aanmelding** van de **modus** vervolgkeuzelijst.

9.  Voer de vereiste waarden in **domein en de URL's.** U moet deze waarden ophalen van de leverancier van de toepassing.

   1. Als u de toepassing configureren als Serviceprovider geïnitieerde eenmalige aanmelding, het teken op de URL voor is deze waarde vereist. Voor sommige toepassingen is de id ook een vereiste waarde.

   2. Het is een vereiste waarde voor het configureren van de toepassing als IdP geïnitieerde eenmalige aanmelding, de antwoord-URL. Voor sommige toepassingen is de id ook een vereiste waarde.

10. **Optioneel:** klikt u op **weergeven geavanceerde instellingen voor URL** als u wilt zien van de waarden niet vereist.

11. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

12. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken worden verzonden naar de toepassing in het SAML-token wanneer de gebruiker zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **toevoegen kenmerk**. Voer de **naam** en selecteer de **waarde** uit de vervolgkeuzelijst.

   2. Klik op **opslaan.** Ziet u het nieuwe kenmerk in de tabel.

13. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang tot documentatie over het configureren van eenmalige aanmelding in de toepassing. Bovendien heeft u de metagegevens-URL's en het certificaat dat is vereist voor het instellen van eenmalige aanmelding met de toepassing.

14. Klik op **opslaan** aan de configuratie op te slaan.

15. Gebruikers toewijzen aan de toepassing.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en gebruikerskenmerken worden verzonden naar de toepassing toevoegen

Als u de gebruikers-ID selecteren of gebruikerskenmerken toevoegen, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing niet ziet u wilt het hier weergegeven, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar  **Alle aanvragen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Onder de **gebruikerskenmerken** sectie, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst. De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

   >[!NOTE] 
   >Azure AD selecteren de indeling voor het kenmerk NameID (gebruikers-id) op basis van de waarde geselecteerd of de indeling die is aangevraagd door de toepassing in de SAML-AuthRequest. Raadpleeg voor meer informatie het artikel [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
   >
   >

9.  Gebruikerskenmerken toevoegen: klik op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken worden verzonden naar de toepassing in het SAML-token wanneer de gebruiker zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **toevoegen kenmerk**. Voer de **naam** en selecteer de **waarde** uit de vervolgkeuzelijst.

   2. Klik op **opslaan.** Hier ziet u het nieuwe kenmerk in de tabel.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

Voor het downloaden van de metagegevens van de toepassing of het certificaat van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Ga naar **SAML-certificaat voor ondertekening van** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding vereist, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

    Azure AD biedt een URL als u de metagegevens niet. De metagegevens kan alleen worden opgehaald als een XML-bestand.

### <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Het configureren van federatieve eenmalige aanmelding voor de toepassing van een niet-galerie

Als u een toepassing niet galerie configureren, moet u beschikken over Azure AD premium en de toepassing SAML 2.0 ondersteunt. Voor meer informatie over Azure AD-versies, gaat u naar [prijzen van Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

-   [Waarden voor metagegevens van de toepassing configureren in Azure AD (URL,-id en antwoord-URL van de aanmelding)](#configuring-single-sign-on)

-   [Selecteer gebruikers-id en gebruikerskenmerken worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD-metagegevens en certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Azure AD metagegevenswaarden configureren in de toepassing (aanmelding URL, uitgever, afmelding URL en certificaat)](#configuring-single-sign-on)

#### <a name="configure-the-applications-metadata-values-in-azure-ad-sign-on-url-identifier-reply-url"></a>Waarden voor metagegevens van de toepassing configureren in Azure AD (URL,-id en antwoord-URL van de aanmelding)

Volg de onderstaande stappen voor het configureren van eenmalige aanmelding voor een toepassing die zich niet in de galerie van Azure AD:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** blade.

6.  Klik op **Non-galerie toepassing** in de **uw eigen app toevoegen** sectie.

7.  Voer de naam van de toepassing in de **naam** textbox.

8.  Klik op **toevoegen** knop, de toepassing toevoegen.

9.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

10. Selecteer **op basis van SAML aanmelding** in de **modus** vervolgkeuzelijst.

11. Voer de vereiste waarden in **domein en de URL's.** U moet deze waarden ophalen van de leverancier van de toepassing.

   1. Voer de antwoord-URL en de id voor het configureren van de toepassing als IdP geïnitieerde eenmalige aanmelding.

   2.  **Optioneel:** voor het configureren van de toepassing als Serviceprovider geïnitieerde eenmalige aanmelding, het teken op de URL voor deze waarde is vereist.

12. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

13. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken worden verzonden naar de toepassing in het SAML-token wanneer de gebruiker zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **toevoegen kenmerk**. Voer de **naam** en selecteer de **waarde** uit de vervolgkeuzelijst.

   2. Klik op **opslaan.** Ziet u het nieuwe kenmerk in de tabel.

14. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang tot documentatie over het configureren van eenmalige aanmelding in de toepassing. Bovendien heeft u Azure AD-URL's en het certificaat dat is vereist voor de toepassing.

#### <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en gebruikerskenmerken worden verzonden naar de toepassing toevoegen

Als u de gebruikers-ID selecteren of gebruikerskenmerken toevoegen, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Onder de **gebruikerskenmerken** sectie, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst. De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

   >[!NOTE] 
   >Azure AD selecteren de indeling voor het kenmerk NameID (gebruikers-id) op basis van de waarde geselecteerd of de indeling die is aangevraagd door de toepassing in de SAML-AuthRequest. Raadpleeg voor meer informatie het artikel [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
   >
   >

9.  Gebruikerskenmerken toevoegen: klik op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken worden verzonden naar de toepassing in het SAML-token wanneer de gebruiker zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **toevoegen kenmerk**. Voer de **naam** en selecteer de **waarde** uit de vervolgkeuzelijst.

   2. Klik op **opslaan.** Ziet u het nieuwe kenmerk in de tabel.

#### <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

Voor het downloaden van de metagegevens van de toepassing of het certificaat van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Ga naar **SAML-certificaat voor ondertekening van** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding vereist, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt een URL als u de metagegevens niet. De metagegevens kan alleen worden opgehaald als een XML-bestand.

### <a name="how-to-configure-password-single-sign-on-for-an-azure-ad-gallery-application"></a>Het wachtwoord eenmalige aanmelding voor een toepassing van de galerie van Azure AD configureren

Een toepassing uit de galerie van Azure AD te hoeft configureren:

-   [Een toepassing toevoegen uit de galerie van Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [De toepassing voor eenmalige aanmelding wachtwoord configureren](#configure-the-application-for-password-single-sign-on)

#### <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen uit de galerie van Azure AD

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit te voeren:

1.  Open de [Azure Portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** blade.

6.  In de **Voer een naam** textbox uit de **toevoegen uit de galerie** sectie, typ de naam van de toepassing.

7.  Selecteer de toepassing die u wilt configureren voor eenmalige aanmelding.

8.  Voordat u de toepassing toevoegt, kunt u de naam van de **naam** textbox.

9.  Klik op **toevoegen** knop, de toepassing toevoegen.

Na een korte periode is het mogelijk om te zien van de toepassing configuration blade.

#### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing voor eenmalige aanmelding wachtwoord configureren

Volg de onderstaande stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  [Gebruikers toewijzen aan de toepassing](#how-to-assign-a-user-to-an-application-directly).

10. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers te selecteren en te klikken op **updatereferenties** en de gebruikersnaam en wachtwoord in te voeren namens de gebruikers. Anders wordt gebruikers gevraagd de referenties zich bij het starten in te voeren.

### <a name="how-to-configure-password-single-sign-on-for-a-non-gallery-application"></a>Het configureren van wachtwoord eenmalige aanmelding voor de toepassing van een niet-galerie

Een toepassing uit de galerie van Azure AD te hoeft configureren:

-   [Toevoegen van een toepassing niet-galerie](#add-a-non-gallery-application)

-   [De toepassing voor eenmalige aanmelding wachtwoord configureren](#configure-the-application-for-password-single-sign-on)

#### <a name="add-a-non-gallery-application"></a>Toevoegen van een toepassing niet-galerie

Als u wilt een toepassing uit de galerie van Azure AD toevoegt, de volgende stappen uit te voeren:

1.  Open de [Azure Portal](https://portal.azure.com) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op de **toevoegen** middenonder in de rechterbovenhoek op het **bedrijfstoepassingen** blade.

6.  Klik op **Non-galerie-toepassing.**

7.  Voer de naam van uw toepassing in de **naam** textbox. Selecteer **toevoegen.**

Na een korte periode is het mogelijk om te zien van de toepassing configuration blade.

#### <a name="configure-the-application-for-password-single-sign-on"></a>De toepassing voor eenmalige aanmelding wachtwoord configureren

Volg de onderstaande stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

    1.  Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt configureren eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Selecteer de modus **op basis van wachtwoorden eenmalige aanmelding.**

9.  Voer de **aanmeldings-URL**. Dit is de URL waar gebruikers hun gebruikersnaam en wachtwoord aanmelden bij invoeren. Zorg ervoor dat de aanmeldingspagina velden zijn zichtbaar in de URL.

10. [Gebruikers toewijzen aan de toepassing](#how-to-assign-a-user-to-an-application-directly).

11. Bovendien kunt u ook referenties opgeven namens de gebruiker door de rijen van de gebruikers te selecteren en te klikken op **updatereferenties** en de gebruikersnaam en wachtwoord in te voeren namens de gebruikers. Anders wordt gebruikers gevraagd de referenties zich bij het starten in te voeren.

## <a name="problems-related-to-assigning-applications-to-users"></a>Problemen met betrekking tot het toewijzen van toepassingen aan gebruikers

Een gebruiker mogelijk niet ziet u een toepassing op hun Toegangsvenster omdat ze niet zijn toegewezen aan de toepassing. Hieronder vindt u een aantal manieren om te controleren:

-   [Controleren of een gebruiker is toegewezen aan de toepassing](#check-if-a-user-is-assigned-to-the-application)

-   [Een gebruiker rechtstreeks naar een toepassing toewijzen](#how-to-assign-a-user-to-an-application-directly)

-   [Controleren of een gebruiker is toegewezen aan een licentie die is gerelateerd aan de toepassing](#check-if-a-user-is-under-a-license-related-to-the-application)

-   [Een licentie toewijzen aan een gebruiker](#how-to-assign-a-user-a-license)

### <a name="check-if-a-user-is-assigned-to-the-application"></a>Controleren of een gebruiker is toegewezen aan de toepassing

Als u wilt controleren of een gebruiker is toegewezen aan de toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

6.  **Search** voor de naam van de betreffende toepassing.

7.  Klik op **gebruikers en groepen**.

8.  Controleer of de gebruiker is toegewezen aan de toepassing.

   * Als dat niet de stappen in 'Hoe een gebruiker toewijzen aan een toepassing rechtstreeks' om dit te doen.

### <a name="how-to-assign-a-user-to-an-application-directly"></a>Een gebruiker rechtstreeks naar een toepassing toewijzen

Als u wilt toewijzen een of meer gebruikers rechtstreeks naar een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder**.

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

Na een korte periode de geselecteerde gebruikers mogelijk om deze toepassingen in het deelvenster toegang te starten.

### <a name="check-if-a-user-is-under-a-license-related-to-the-application"></a>Controleer of een gebruiker zich onder een licentie die is gerelateerd aan de toepassing

Als u wilt controleren toegewezen licenties van een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de gebruiker momenteel is toegewezen.

  * Als de gebruiker is toegewezen aan een Office licentie deze inschakelen eerste partij Office-toepassingen worden weergegeven op het toegangsvenster van de gebruiker.

### <a name="how-to-assign-a-user-a-license"></a>Een gebruiker een licentie toewijzen 

Als u wilt een licentie toewijzen aan een gebruiker, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de gebruiker momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item granulair toewijzen producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze gebruiker.

## <a name="problems-related-to-assigning-applications-to-groups"></a>Problemen met betrekking tot het toewijzen van toepassingen aan groepen

Een gebruiker mogelijk ziet u een toepassing op hun Toegangsvenster omdat ze deel uitmaken van een groep die de toepassing is toegewezen. Hieronder vindt u een aantal manieren om te controleren:

-   [Controleer de groepslidmaatschappen van een gebruiker](#check-a-users-group-memberships)

-   [Hoe een toepassing toewijzen aan een groep rechtstreeks](#how-to-assign-an-application-to-a-group-directly)

-   [Als een gebruiker deel uit van de groep die is toegewezen aan een licentie maakt controleren](#check-if-a-user-is-part-of-group-assigned-to-a-license)

-   [Een licentie toewijzen aan een groep](#how-to-assign-a-license-to-a-group)

### <a name="check-a-users-group-memberships"></a>Controleer de groepslidmaatschappen van een gebruiker

Als u wilt controleren lidmaatschap van een groep, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen**.

8.  Controleer of uw gebruiker deel van een groep die is toegewezen aan de toepassing uitmaakt.

  * Als u wilt verwijderen van de gebruiker uit de groep **klikt u op de rij** van het Groepsbeleid en selecteer verwijderen.

### <a name="how-to-assign-an-application-to-a-group-directly"></a>Hoe een toepassing toewijzen aan een groep rechtstreeks

Als u wilt toewijzen een of meer groepen rechtstreeks aan een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt toewijzen van een gebruiker in de lijst.

7.  Nadat de toepassing wordt geladen, klikt u op **gebruikers en groepen** van navigatiemenu links aan de van de toepassing.

8.  Klik op de **toevoegen** knop boven de **gebruikers en groepen** lijst openen de **toevoegen toewijzing** blade.

9.  Klik op de **gebruikers en groepen** selector van de **toevoegen toewijzing** blade.

10. Typ in het **volledige groepsnaam** van de groep die u geïnteresseerd bent in toewijzen in de **zoeken op naam of e-mailadres** zoekvak.

11. Beweeg de muisaanwijzer over de **groep** in de lijst om weer te geven een **selectievakje**. Schakel het selectievakje in naast de profielfoto of het logo voor uw gebruiker toevoegen aan de groep de **geselecteerde** lijst.

12. **Optioneel:** als u wilt **toevoegen van meer dan één groep**, type in een andere **volledige groepsnaam** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje in om toe te voegen van deze groep de **geselecteerde** lijst.

13. Wanneer u klaar bent met groepen te selecteren, klikt u op de **Selecteer** om toe te voegen aan de lijst met gebruikers en groepen kunnen worden toegewezen aan de toepassing.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** blade te selecteren van een rol toewijzen aan de groepen die u hebt geselecteerd.

15. Klik op de **toewijzen** knop de toepassing toewijzen aan de geselecteerde groepen.

Na een korte periode de geselecteerde gebruikers mogelijk om deze toepassingen in het deelvenster toegang te starten.

### <a name="check-if-a-user-is-part-of-group-assigned-to-a-license"></a>Als een gebruiker deel uit van de groep die is toegewezen aan een licentie maakt controleren

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle gebruikers**.

6.  **Search** voor de gebruiker die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **groepen**.

8.  Klik op de rij van een specifieke groep.

9.  Klik op **licenties** om te zien die de groep licenties is toegewezen.

   * Als de groep is toegewezen aan een Office-licentie hierdoor bepaalde eerste partij Office-toepassingen worden weergegeven op het toegangsvenster van de gebruiker.

### <a name="how-to-assign-a-license-to-a-group"></a>Een licentie toewijzen aan een groep

Als u wilt een licentie toewijzen aan een groep, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **alle groepen**.

6.  **Search** voor de groep die u geïnteresseerd bent in en **klikt u op de rij** om te selecteren.

7.  Klik op **licenties** om te zien die verleent aan de groep momenteel is toegewezen.

8.  Klik op de **toewijzen** knop.

9.  Selecteer **een of meer producten** uit de lijst met beschikbare producten.

10. **Optionele** klikt u op de **toewijzingsopties** item granulair toewijzen producten. Klik op **Ok** wanneer dit is voltooid.

11. Klik op de **toewijzen** knop deze licenties toewijzen aan deze groep. Dit kan lang duren, afhankelijk van de omvang en complexiteit van de groep.

>[!NOTE]
>U kunt tijdelijk toewijzen van een licentie aan de gebruiker rechtstreeks hiervoor sneller. 
>
>

## <a name="next-steps"></a>Volgende stappen
[Nieuwe gebruikers toevoegen aan Azure Active Directory](active-directory-users-create-azure-portal.md)


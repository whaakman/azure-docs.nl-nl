---
title: Het configureren van federatieve eenmalige aanmelding voor een galerie van Azure AD-toepassing | Microsoft Docs
description: Het configureren van federatieve eenmalige aanmelding voor een bestaande-galerie van Azure AD-toepassing en het gebruik zelfstudies snel aan de slag
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
ms.openlocfilehash: 1b1d00718981b2c7d11f5b88428d02e16dd0b34d
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van federatieve eenmalige aanmelding voor een galerie van Azure AD-toepassing

Alle toepassingen in de galerie van Azure AD ingeschakeld met Enterprise single sign-on-functionaliteit is een stapsgewijze zelfstudie beschikbaar. U hebt toegang tot de [lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor gedetailleerde stapsgewijze instructies.

## <a name="overview-of-steps-required"></a>Overzicht van stappen vereist
Een toepassing uit de galerie van Azure AD te hoeft configureren:

-   [Een toepassing toevoegen uit de galerie van Azure AD](#add-an-application-from-the-azure-ad-gallery)

-   [Waarden voor metagegevens van de toepassing configureren in Azure AD (URL,-id en antwoord-URL van de aanmelding)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selecteer gebruikers-id en gebruikerskenmerken worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [Azure AD-metagegevens en certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Azure AD metagegevenswaarden configureren in de toepassing (aanmelding URL, uitgever, afmelding URL en certificaat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Gebruikers toewijzen aan de toepassing](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen uit de galerie van Azure AD

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

Na een korte periode, is het mogelijk om te zien van de toepassing configuration blade.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Eenmalige aanmelding voor een toepassing uit de galerie van Azure AD configureren

Volg de onderstaande stappen voor het configureren van eenmalige aanmelding voor een toepassing:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

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

   1. Klik op **opslaan.** Ziet u het nieuwe kenmerk in de tabel.

13. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang tot documentatie over het configureren van eenmalige aanmelding in de toepassing. Bovendien heeft u de metagegevens-URL's en het certificaat dat is vereist voor het instellen van eenmalige aanmelding met de toepassing.

14. Klik op **opslaan** aan de configuratie op te slaan.

15. Gebruikers toewijzen aan de toepassing.

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en gebruikerskenmerken worden verzonden naar de toepassing toevoegen

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

   2. Klik op **Opslaan**. Ziet u het nieuwe kenmerk in de tabel.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

Voor het downloaden van de metagegevens van de toepassing of het certificaat van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  *  Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** van navigatiemenu links aan de van de toepassing.

8.  Ga naar **SAML-certificaat voor ondertekening van** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding vereist, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt een URL als u de metagegevens niet. De metagegevens kan alleen worden opgehaald als een XML-bestand.

## <a name="assign-users-to-the-application"></a>Gebruikers toewijzen aan de toepassing

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

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>De SAML-claims verzonden naar een toepassing aanpassen

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [toewijzen in Azure Active Directory-Claims](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Geef één aanmelding bij uw apps met toepassingsproxy](active-directory-application-proxy-sso-using-kcd.md)




---
title: Het configureren van eenmalige aanmelding in voor een toepassing buiten de galerie | Microsoft Docs
description: Het configureren van eenmalige aanmelding in voor een aangepaste toepassing buiten de galerie die u wilt integreren met Azure AD
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 07/11/2017
ms.author: barbkess
ms.openlocfilehash: 0a9444c4abce9845efeca808b24264ba7b135aa9
ms.sourcegitcommit: af9cb4c4d9aaa1fbe4901af4fc3e49ef2c4e8d5e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44356786"
---
# <a name="how-to-configure-federated-single-sign-on-for-a-non-gallery-application"></a>Het configureren van eenmalige aanmelding in voor een toepassing buiten de galerie

Het configureren van eenmalige aanmelding voor een toepassing buiten de galerie *zonder code te schrijven*, moet u beschikken over een abonnement of Azure AD Premium en de toepassing moeten ondersteuning van SAML 2.0. Voor meer informatie over de versies van Azure AD, gaat u naar [prijzen voor Azure AD](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="overview-of-steps-required"></a>Overzicht van stappen die nodig zijn
Hieronder vindt u een overzicht van de vereiste stappen voor het configureren van federatieve eenmalige aanmelding met SAML 2.0 voor een (bijvoorbeeld aangepaste) toepassing buiten de galerie.

-   [Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren](#_Configuring_single_sign-on)

-   [Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [De metagegevens van de Azure AD en het certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Waarden voor metagegevens van Azure AD configureren in de toepassing (aanmelding URL, uitgever, afmeldings-URL en certificaat)](#_Configuring_single_sign-on)

-   [Gebruikers toewijzen aan de toepassing](#_Assign_users_to_the_application)

## <a name="configuring-single-sign-on-to-non-gallery-applications"></a>Single sign-on bij niet-galerietoepassingen configureren

Voor het configureren van eenmalige aanmelding voor een toepassing die zich niet in de Azure AD-galerie, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op de **toevoegen** knop in de rechterbovenhoek op het **bedrijfstoepassingen** deelvenster.

6.  Klik op **niet in de galerij toepassing** in de **toevoegen aan uw eigen app** sectie

7.  Voer de naam van de toepassing in de **naam** tekstvak.

8.  Klik op **toevoegen** knop om toe te voegen van de toepassing.

9.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

10. Selecteer **SAML gebaseerde aanmelding** in de **modus** vervolgkeuzelijst.

11. Voer de vereiste waarden in **domein en URL's.** U krijgt deze waarden van de leverancier van de toepassing.

   1. Voer de antwoord-URL en de id voor het configureren van de toepassing als id-provider geïnitieerde eenmalige aanmelding.

   2. **Optioneel:** configureren van de toepassing als Serviceprovider geïnitieerde eenmalige aanmelding, de aanmeldings-URL is een vereiste waarde.

12. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

13. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

14. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang documentatie over het configureren van eenmalige aanmelding in de toepassing. Bovendien heeft u Azure AD-URL's en dat is vereist voor de toepassing.

15. [Gebruikers toewijzen aan de toepassing.](#assign-users-to-the-application)

## <a name="select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application"></a>Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen

Als u wilt de gebruikers-ID selecteren of toevoegen van gebruikerskenmerken, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Onder de **gebruikerskenmerken** sectie, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst. De geselecteerde optie moet overeenkomen met de verwachte waarde in de toepassing om de gebruiker te verifiëren.

 >[! Houd er rekening mee} Azure AD de indeling voor het kenmerk van NameID (gebruikers-id) op basis van de geselecteerde waarde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest selecteren. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
 >
 >

9.  Als u wilt toevoegen gebruikerskenmerken, klikt u op **weergeven en bewerk alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:

   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt ook een URL waarmee u de metagegevens ophalen. Ga als volgt dit patroon om op te halen de metagegevens-URL voor de toepassing: https://login.microsoftonline.com/ <Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>.

## <a name="assign-users-to-the-application"></a>Gebruikers toewijzen aan een toepassing

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

12. **Optioneel:** als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>De SAML-claims verzonden naar een toepassing aanpassen

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [Claims toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)

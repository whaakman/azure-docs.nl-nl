---
title: Het configureren van eenmalige aanmelding in voor een toepassing in Azure AD-galerie | Microsoft Docs
description: Hoe om te configureren voor federatieve eenmalige aanmelding voor een bestaande toepassing in de Azure AD-galerie en gebruik zelfstudies voor het snel aan de slag
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
ms.openlocfilehash: e1555830fb8e355ec4e291dc49b7d6983f87ffc5
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55168393"
---
# <a name="how-to-configure-federated-single-sign-on-for-an-azure-ad-gallery-application"></a>Het configureren van eenmalige aanmelding in voor een toepassing in Azure AD-galerie

Alle toepassingen in de Azure AD-galerie ingeschakeld met één Enterprise aanmeldings-functionaliteit is een stapsgewijze zelfstudie beschikbaar. U hebt toegang tot de [lijst met zelfstudies over het integreren van SaaS-apps met Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-saas-tutorial-list/) voor gedetailleerde stapsgewijze instructies.

## <a name="overview-of-steps-required"></a>Overzicht van stappen die nodig zijn
Een toepassing uit de Azure AD-galerie die u wilt configureren:

-   [Een toepassing toevoegen via de Azure AD-galerie](#add-an-application-from-the-azure-ad-gallery)

-   [Waarden van de metagegevens van de toepassing in Azure AD (aanmeldings-URL-id en antwoord-URL) configureren](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Selecteer gebruikers-id en de kenmerken van de gebruiker moet worden verzonden naar de toepassing toevoegen](#select-user-identifier-and-add-user-attributes-to-be-sent-to-the-application)

-   [De metagegevens van de Azure AD en het certificaat ophalen](#download-the-azure-ad-metadata-or-certificate)

-   [Waarden voor metagegevens van Azure AD configureren in de toepassing (aanmelding URL, uitgever, afmeldings-URL en certificaat)](#configure-single-sign-on-for-an-application-from-the-azure-ad-gallery)

-   [Gebruikers toewijzen aan de toepassing](#assign-users-to-the-application)

## <a name="add-an-application-from-the-azure-ad-gallery"></a>Een toepassing toevoegen via de Azure AD-galerie

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

Na een korte periode kunt u zich aan het deelvenster van de configuratie van de toepassing zien.

## <a name="configure-single-sign-on-for-an-application-from-the-azure-ad-gallery"></a>Configureren van eenmalige aanmelding voor een toepassing uit de Azure AD-galerie

Voor het configureren van eenmalige aanmelding voor een toepassing, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder**.

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

   * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt configureren van eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Selecteer **SAML gebaseerde aanmelding** uit de **modus** vervolgkeuzelijst.

9.  Voer de vereiste waarden in **domein en URL's.** U krijgt deze waarden van de leverancier van de toepassing.

   1. Het is een vereiste waarde voor het configureren van de toepassing als Serviceprovider geïnitieerde eenmalige aanmelding, de aanmeldings-URL. De id is voor sommige toepassingen, ook een vereiste waarde.

   2. Het is een vereiste waarde voor het configureren van de toepassing als id-provider geïnitieerde eenmalige aanmelding, de antwoord-URL. De id is voor sommige toepassingen, ook een vereiste waarde.

10. **Optioneel:** klikt u op **geavanceerde URL-instellingen weergeven** als u wilt zien van de waarden niet vereist.

11. In de **gebruikerskenmerken**, selecteert u de unieke id voor uw gebruikers in de **gebruikers-id** vervolgkeuzelijst.

12. **Optioneel:** klikt u op **weergeven en bewerken van alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

  Een kenmerk toevoegen:
   
   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   1. Klik op **opslaan.** U kunt het nieuwe kenmerk in de tabel zien.

13. Klik op **configureren &lt;toepassingsnaam&gt;**  toegang documentatie over het configureren van eenmalige aanmelding in de toepassing. U hebt ook de metagegevens-URL's en dat is vereist voor het instellen van eenmalige aanmelding met de toepassing.

14. Klik op **opslaan** aan de configuratie op te slaan.

15. Gebruikers toewijzen aan de toepassing.

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

  >[!NOTE] 
  >Azure AD selecteren de indeling voor het kenmerk van NameID (gebruikers-id) op basis van het geselecteerde of de indeling die is aangevraagd door de toepassing in het SAML-AuthRequest. Ga naar het artikel voor meer informatie [Single Sign-On SAML-protocol](https://docs.microsoft.com/azure/active-directory/develop/active-directory-single-sign-on-protocol-reference#authnrequest) onder de sectie NameIDPolicy.
  >
  >

9.  Als u wilt toevoegen gebruikerskenmerken, klikt u op **weergeven en bewerk alle andere gebruikerskenmerken** bewerken van de kenmerken die moeten worden verzonden naar de toepassing in het SAML-token wanneer gebruikers zich aanmelden.

   Een kenmerk toevoegen:
  
   1. Klik op **kenmerk toevoegen**. Voer de **naam** en selecteer de **waarde** in de vervolgkeuzelijst.

   2. Klik op **Opslaan**. U kunt het nieuwe kenmerk in de tabel zien.

## <a name="download-the-azure-ad-metadata-or-certificate"></a>De metagegevens van de Azure AD of het certificaat downloaden

De metagegevens van de toepassing of het certificaat downloaden van Azure AD, de volgende stappen uit te voeren:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  *  Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen**.

6.  Selecteer de toepassing die u hebt geconfigureerd eenmalige aanmelding.

7.  Nadat de toepassing wordt geladen, klikt u op de **eenmalige aanmelding** in het navigatiemenu aan de van de toepassing.

8.  Ga naar **SAML-handtekeningcertificaat** sectie en klik vervolgens op **downloaden** waarde in de kolom. Afhankelijk van wat de toepassing configureren van eenmalige aanmelding moet, ziet u ofwel de optie voor het downloaden van de Metadata XML of het certificaat.

Azure AD biedt ook een URL waarmee u de metagegevens ophalen. Ga als volgt dit patroon om op te halen de metagegevens-URL voor de toepassing: `https://login.microsoftonline.com/<Directory ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<Application ID>`

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

12. **Optioneel:** Als u wilt **toevoegen van meer dan één gebruiker**, type in een andere **volledige naam** of **e-mailadres** in de **zoeken op naam of e-mailadres** het zoekvak en klik op het selectievakje voor deze gebruiker toevoegen aan de **geselecteerde** lijst.

13. Wanneer u klaar bent met gebruikers te selecteren, klikt u op de **Selecteer** knop aan de lijst met gebruikers en groepen die moeten worden toegewezen aan de toepassing wilt toevoegen.

14. **Optioneel:** klikt u op de **rol selecteren** selector in de **toevoegen toewijzing** deelvenster te selecteren van een rol toewijzen aan de gebruikers die u hebt geselecteerd.

15. Klik op de **toewijzen** knop om de toepassing aan de geselecteerde gebruikers te wijzen.

Na een korte periode, de gebruikers die u hebt geselecteerd mogelijk om deze toepassingen met behulp van de methoden die worden beschreven in de sectie oplossing beschrijving te starten.

## <a name="customizing-the-saml-claims-sent-to-an-application"></a>De SAML-claims verzonden naar een toepassing aanpassen

Zie voor meer informatie over het aanpassen van de SAML-kenmerk claims verzonden naar uw toepassing, [Claims toewijzen in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-claims-mapping) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
[Geef single sign-on bij uw apps met Application Proxy](application-proxy-configure-single-sign-on-with-kcd.md)




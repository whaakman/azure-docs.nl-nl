---
title: Onverwachte toepassing in de lijst met mijn toepassingen | Microsoft Docs
description: Hoe zien alle toepassingen in uw tenant en u begrijpt hoe toepassingen worden weergegeven in de lijst met alle toepassingen onder bedrijfstoepassingen
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
ms.openlocfilehash: 4765b71714e88ee91cb9938ad4bb34033cf12422
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="unexpected-application-in-my-applications-list"></a>Onverwachte toepassing in de lijst met mijn toepassingen

In dit artikel helpen te begrijpen hoe toepassingen worden weergegeven in uw **alle toepassingen** lijst onder **bedrijfstoepassingen**. 

## <a name="how-to-see-all-applications-in-your-tenant"></a>Hoe ziet u alle toepassingen in uw tenant

Overzicht van alle toepassingen in uw tenant, moet u gebruiken de **Filter** besturingselement om weer te geven **alle toepassingen** onder de **alle toepassingen** lijst. U doet dit door de volgende stappen uit te voeren:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

6.  Klik op het gebruik de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**.

7.  Op de **Filter** blade ingesteld de **weergeven** optie naar **alle toepassingen.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Waarom wordt er een specifieke toepassing weergegeven in de lijst met alle toepassingen?

Wanneer gefilterd op **alle toepassingen**, wordt de **alle toepassingen** **lijst** toont elk Service-Principal-object in uw tenant. Service-Principal-objecten kunnen worden weergegeven in deze lijst op een verschillende manieren:

1.  Wanneer u een toepassing uit de galerie met toepassingen toevoegt, waaronder:

   1. **Azure AD-galerie toepassingen** : een toepassing die vooraf geïntegreerde voor eenmalige aanmelding met Azure AD is.

   2. **Application Proxy toepassingen** : een toepassing die wordt uitgevoerd in uw on-premises omgeving die u wilt extern beveiligde eenmalige aanmelding op te geven.

   3. **Aangepaste ontwikkelde toepassingen** : een toepassing die uw organisatie wil ontwikkelen op het Azure AD-toepassing ontwikkelplatform, maar die mogelijk niet bestaat.

   4. **Niet-galerie toepassingen** : Breng uw eigen toepassingen! Een webkoppeling die u wilt dat alle toepassingen die een veld de gebruikersnaam en wachtwoord SAML of OpenID Connect-protocollen ondersteunt of ondersteunt SCIM die u wilt integreren voor eenmalige aanmelding met Azure AD.

2.  Wanneer zich aanmelden voor of aanmelden bij een 3<sup>rd</sup> of een toepassing die is geïntegreerd met Azure Active Directory. Een voorbeeld hiervan is [Smartsheet](https://app.smartsheet.com/b/home) of [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).

3.  Wanneer u zich aanmelden voor of een licentie toe te voegen aan een gebruiker of een groep aan een eerste toepassing van derden, zoals [Microsoft Office 365](http://products.office.com/).

4.  Wanneer u een nieuwe toepassingsregistratie toevoegen door het maken van een toepassing aangepaste ontwikkeld met de [toepassingsregister](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration).

5.  Wanneer u een nieuwe toepassingsregistratie toevoegen door het maken van een toepassing aangepaste ontwikkeld met de [V2.0-Portal voor registratie van toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration#visit-the-microsoft-app-registration-portal).

6.  Wanneer u een toepassing toevoegen u ontwikkelt met Visual Studio [ASP.net verificatiemethoden](http://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) of [verbonden Services](http://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx).

7.  Wanneer u maakt een service-principal-object met de [Azure AD PowerShell-Module](/powershell/azure/install-adv2?view=azureadps-2.0).

8.  Wanneer u [toestemming geeft om een toepassing te](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) als beheerder om gegevens te gebruiken in uw tenant.

9.  Wanneer een [gebruiker hiermee akkoord gaat tot een toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent) om gegevens te gebruiken in uw tenant.

10. Wanneer u bepaalde services waarmee gegevens worden opgeslagen in uw tenant inschakelen. Een voorbeeld hiervan is wachtwoord opnieuw instellen, die is gemodelleerd als een service-principal voor het opslaan van uw wachtwoord opnieuw instellen van beleid veilig.

Als u meer informatie over hoe apps worden toegevoegd aan uw directory, lezen [hoe en waarom toepassingen worden toegevoegd aan Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Ik wil een specifieke gebruiker of groep van toewijzing aan een toepassing verwijderen

Als u wilt verwijderen van een gebruiker of groepstoewijzing aan een toepassing, volg de stappen in de [de toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Ik wil alle toegang tot een toepassing voor elke gebruiker uitschakelen

Schakel alle gebruikersaanmeldingen tot een toepassing die de stappen die worden vermeld in de [gebruikersaanmeldingen een enterprise-App in Azure Active Directory uitschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.

## <a name="i-want-to-delete-an-application-entirely"></a>Ik wil een toepassing volledig verwijderen

Naar **verwijderen van een toepassing**, volgt u onderstaande instructies:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het menu van Azure Active Directory linkerkant navigatie.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle toepassingen.**

6.  Selecteer de toepassing die u wilt verwijderen.

7.  Nadat de toepassing wordt geladen, klikt u op **verwijderen** pictogram van de bovenste toepassing **overzicht** blade.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Ik wil alle bewerkingen van toekomstige gebruikers toestemming voor elke toepassing uitschakelen

Toestemming van de gebruiker uitschakelt voor uw hele directory te voorkomen dat eindgebruikers ermee akkoord dat elke toepassing. Beheerders kunnen nog steeds op behalves van de gebruiker toestemming. Meer informatie over de toepassing toestemming en waarom u niet wilt doen, of lezen [wat gebruikers- en toestemming van de beheerder](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview#understanding-user-and-admin-consent).

Naar **uitschakelen alle toekomstige gebruikers toestemming bewerkingen in uw hele directory**, volgt u onderstaande instructies:

1.  Open de [ **Azure Portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **meer services** onderaan in het navigatiemenu belangrijkste linkerkant.

3.  Typ in **' Azure Active Directory**' in het zoekvak filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **gebruikersinstellingen**.

6.  Alle toekomstige gebruikers toestemming bewerkingen uitschakelen door in te stellen de **gebruikers kunnen toestaan dat apps toegang tot hun gegevens** in-of uitschakelen op **Nee** en klik op de **opslaan** knop.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](active-directory-enable-sso-scenario.md)

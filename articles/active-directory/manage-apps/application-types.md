---
title: Onverwachte toepassing in de lijst met mijn toepassingen | Microsoft Docs
description: Over het bekijken van alle toepassingen in uw tenant en te begrijpen hoe toepassingen worden weergegeven in de lijst van alle toepassingen in bedrijfstoepassingen
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
ms.date: 07/11/2017
ms.author: celested
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3037d1de83e34aa8489498c65dbc5fd3423b081b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56162659"
---
# <a name="unexpected-application-in-my-applications-list"></a>Onverwachte toepassing in de lijst met mijn toepassingen

In dit artikel kunt u beter begrijpt hoe toepassingen worden weergegeven in uw **alle toepassingen** lijst onder **bedrijfstoepassingen**. 

## <a name="how-to-see-all-applications-in-your-tenant"></a>Hoe ziet u alle toepassingen in uw tenant

Als u wilt zien van alle toepassingen in uw tenant, moet u gebruiken de **Filter** besturingselement om weer te geven **alle toepassingen** onder de **alle toepassingen** lijst. Volg deze stappen:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

6.  Klik op het gebruik de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen**.

7.  Op de **Filter** instellen in het deelvenster de **weergeven** optie naar **alle toepassingen.**

## <a name="why-does-a-specific-application-appear-in-my-all-applications-list"></a>Waarom wordt een specifieke toepassing weergegeven in de lijst met alle toepassingen?

Wanneer gefilterd op **alle toepassingen**, wordt de **alle toepassingen** **lijst** toont elk Service-Principal-object in uw tenant. Service-Principal-objecten kunnen worden weergegeven in deze lijst op een verschillende manieren:

1.  Wanneer u een toepassing hebt toegevoegd uit de galerie, waaronder:

   1. **Azure AD-Galerietoepassingen** : een toepassing die vooraf geïntegreerde voor eenmalige aanmelding met Azure AD is

   2. **Application Proxy toepassingen** : een toepassing die wordt uitgevoerd in uw on-premises-omgeving die u wilt beveiligde eenmalige aanmelding bij extern bieden

   3. **Speciaal ontwikkelde toepassingen** : een toepassing die uw organisatie wil ontwikkelen op Azure AD Application Development Platform, maar die nog niet kunnen voorkomen

   4. **Niet-Galerietoepassingen** : Breng uw eigen toepassingen. Een webkoppeling die u wilt dat elke toepassing die het veld gebruikersnaam en wachtwoord, wordt weergegeven ondersteuning biedt voor SAML- of OpenID Connect-protocol of SCIM die u wilt integreren voor eenmalige aanmelding met Azure AD ondersteunt.

2.  Wanneer u zich aanmelden voor of aanmelden bij een 3<sup>rd</sup> party-toepassingen geïntegreerd met Azure Active Directory. Een voorbeeld hiervan is [Smartsheet](https://app.smartsheet.com/b/home) of [DocuSign](https://www.docusign.net/member/MemberLogin.aspx).

3.  Wanneer u zich aanmelden voor, of een licentie toe te voegen aan een gebruiker of groep aan een eerste toepassing van derden, zoals [Microsoft Office 365](https://products.office.com/)

4.  Wanneer u de registratie van een nieuwe toepassing toevoegen door het maken van een aangepaste toepassing met de [toepassingsregister](https://docs.microsoft.com/azure/active-directory/active-directory-app-registration)

5.  Wanneer u de registratie van een nieuwe toepassing toevoegen door het maken van een aangepaste toepassing met de [V2.0-registratieportal voor toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-v2-app-registration)

6.  Wanneer u een toepassing toevoegen ontwikkelt u met behulp van Visual Studio [ASP.net-verificatiemethoden](https://www.asp.net/visual-studio/overview/2013/creating-web-projects-in-visual-studio#orgauthoptions) of [Connected Services](https://blogs.msdn.com/b/visualstudio/archive/2014/11/19/connecting-to-cloud-services.aspx)

7.  Wanneer u een service-principal-object met maakt de [Azure AD PowerShell-Module](/powershell/azure/install-adv2?view=azureadps-2.0)

8.  Wanneer u [instemmen met een toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) als beheerder om gegevens te gebruiken in uw tenant

9.  Wanneer een [gebruiker toestemming heeft voor een toepassing](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview) om gegevens te gebruiken in uw tenant

10. Wanneer u bepaalde services waarbij de gegevens opslaan in uw tenant inschakelt. Een voorbeeld hiervan is voor wachtwoordherstel, die is gemodelleerd als een service-principal voor het opslaan van uw wachtwoord opnieuw instellen van beleid veilig.

Als u meer informatie over hoe apps worden toegevoegd aan uw directory, lezen [hoe en waarom worden toepassingen toegevoegd aan Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-how-applications-are-added).

## <a name="i-want-to-remove-a-specific-users-or-groups-assignment-to-an-application"></a>Ik wil een specifieke gebruiker of groep van toewijzing aan een toepassing verwijderen

Als u wilt verwijderen van een gebruiker of groepstoewijzing aan een toepassing, volgt u de stappen in de [de toewijzing van een gebruiker of groep verwijderen uit een enterprise-app in Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-remove-assignment-azure-portal) artikel.

## <a name="i-want-to-disable-all-access-to-an-application-for-every-user"></a>Ik wil alle toegang tot een toepassing voor elke gebruiker uitschakelen

Als u wilt alle gebruikers zich aangemeld bij een toepassing uitschakelen, volgt u de stappen in de [gebruikersaanmeldingen voor een enterprise-app in Azure Active Directory uitschakelen](https://docs.microsoft.com/azure/active-directory/active-directory-coreapps-disable-app-azure-portal) artikel.

## <a name="i-want-to-delete-an-application-entirely"></a>Ik wil een toepassing volledig verwijderen

Naar **verwijderen van een toepassing**, als volgt te werk:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **hoofdbeheerder** of **Co-beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **bedrijfstoepassingen** in het navigatiemenu aan Azure Active Directory.

5.  Klik op **alle toepassingen** om een lijst met al uw toepassingen weer te geven.

  * Als u de toepassing die u wilt weergeven die hier niet ziet, gebruikt u de **Filter** besturingselement aan de bovenkant van de **lijst met alle toepassingen** en stel de **weergeven** optie naar **alle Toepassingen.**

6.  Selecteer de toepassing die u wilt verwijderen.

7.  Nadat de toepassing wordt geladen, klikt u op **verwijderen** pictogram in de bovenste toepassing **overzicht** deelvenster.

## <a name="i-want-to-disable-all-future-user-consent-operations-to-any-application"></a>Ik wil alle bewerkingen van toekomstige gebruikers toestemming voor elke toepassing uitschakelen

Toestemming van de gebruiker wordt uitgeschakeld voor uw hele directory te voorkomen dat eindgebruikers ermee akkoord dat voor elke toepassing. Beheerders kunnen nog steeds op van de gebruiker behalves instemmen. Voor meer informatie over de toestemming van de toepassing en waarom u niet wilt mogelijk accepteren, of Lees [inzicht in gebruikers- en toestemming van een beheerder](https://docs.microsoft.com/azure/active-directory/develop/active-directory-devhowto-multi-tenant-overview).

Naar **uitschakelen van alle toekomstige gebruikers toestemming bewerkingen in uw hele directory**, als volgt te werk:

1.  Open de [ **Azure-portal** ](https://portal.azure.com/) en meld u aan als een **globale beheerder.**

2.  Open de **Azure Active Directory-extensie** door te klikken op **alle services** aan de bovenkant van het menu links hoofdgedeelte voor navigatie.

3.  Typ in **' Azure Active Directory**' in het zoekvak van filter en selecteer de **Azure Active Directory** item.

4.  Klik op **gebruikers en groepen** in het navigatiemenu.

5.  Klik op **gebruikersinstellingen**.

6.  Alle toekomstige gebruikers toestemming bewerkingen uitschakelen door in te stellen de **gebruikers kunnen toestaan dat apps toegang tot hun gegevens** overzet naar **Nee** en klikt u op de **opslaan** knop.

## <a name="next-steps"></a>Volgende stappen
[Toepassingen beheren met Azure Active Directory](what-is-application-management.md)

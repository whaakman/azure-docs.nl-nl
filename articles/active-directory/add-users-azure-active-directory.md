---
title: Toevoegen of verwijderen van gebruikers in Azure Active Directory | Microsoft Docs
description: Wordt uitgelegd hoe u nieuwe gebruikers toevoegen of bestaande gebruikers in Azure Active Directory verwijderen
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: curtand
ms.reviewer: jeffsta
ms.custom: it-pro
ms.openlocfilehash: f0f18f377f194e78c05e63e9b6cbc31c1b945335
ms.sourcegitcommit: 6fb44d6fbce161b26328f863479ef09c5303090f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="quickstart-add-new-users-to-azure-active-directory"></a>Snelstartgids: Nieuwe gebruikers toevoegen aan Azure Active Directory
In dit artikel wordt uitgelegd hoe verwijderen of toevoegen van gebruikers in uw organisatie in uw orgnization Azure Active Directory (Azure AD)-tenant met de Azure portal of door het synchroniseren van uw on-premises Windows Server AD-gegevens voor account van gebruiker. 

## <a name="add-cloud-based-users"></a>Cloud-gebaseerde gebruikers toevoegen
1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **Azure Active Directory** en vervolgens **gebruikers en groepen**.
3. Op **gebruikers en groepen**, selecteer **alle gebruikers**, en selecteer vervolgens **nieuwe gebruiker**.
   ![De opdracht Add selecteren](./media/add-users-azure-active-directory/add-user.png)
4. Voer details voor de gebruiker, zoals **naam** en **gebruikersnaam**. Het domeingedeelte van de naam van de gebruikersnaam moet worden de oorspronkelijke standaard domain name '[domeinnaam].onmicrosoft.com' of een geverifieerde niet-gefedereerde [aangepaste domeinnaam](add-custom-domain.md) zoals 'contoso.com'.
5. Kopieer of het wachtwoord van de gebruiker gegenereerde anders opmerking zodat u deze informatie aan de gebruiker verstrekt kunt nadat dit proces voltooid is.
6. U kunt desgewenst openen en vul de informatie in **profiel**, **groepen**, of **functie Directory** voor de gebruiker. Zie [Assigning administrator roles in Azure AD](active-directory-assign-admin-roles-azure-portal.md) (Engelstalig) voor meer informatie over gebruikers- en beheerdersrollen.
7. Op **gebruiker**, selecteer **maken**.
8. Het gegenereerde wachtwoord naar de nieuwe gebruiker veilig distribueren zodat de gebruiker zich kan aanmelden.

> [!TIP]
> U kunt ook gegevens van gebruikersaccounts uit de lokale Windows Server AD synchroniseren. Microsoft identiteitsoplossingen span on-premises en cloud-gebaseerde mogelijkheden voor het maken van de identiteit van een enkele gebruiker voor verificatie en autorisatie voor alle netwerkbronnen, ongeacht de locatie. We noemen deze hybride identiteit. [Azure AD Connect](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect) kan worden gebruikt om uw on-premises adreslijsten integreren met Azure Active Directory voor hybride identiteit scenario's. Hiermee kunt u uw gebruikers een algemene identiteit bieden voor Office 365, Azure en SaaS toepassingen die zijn geïntegreerd met Azure AD. 

## <a name="delete-users-from-azure-ad"></a>Gebruikers verwijderen van Azure AD
1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **gebruikers en groepen**.
3. Op de **gebruikers en groepen** blade, selecteer de gebruiker wilt verwijderen uit de lijst. 
4. Selecteer op de blade voor de geselecteerde gebruiker **overzicht**, en selecteer vervolgens in de opdrachtbalk **verwijderen**.
   ![De opdracht Add selecteren](./media/add-users-azure-active-directory/delete-user.png)


### <a name="learn-more"></a>Meer informatie 
* [Gastgebruikers van een andere map toevoegen](active-directory-b2b-what-is-azure-ad-b2b.md) 
* [Een gebruiker toewijzen aan een rol in uw Azure AD](active-directory-users-assign-role-azure-portal.md)
* [Gebruikersprofielen beheren](active-directory-users-profile-azure-portal.md)
* [Een verwijderde gebruiker herstellen](active-directory-users-restore.md)



## <a name="next-steps"></a>Volgende stappen
In deze snelstartgids hebt u geleerd hoe u nieuwe gebruikers toevoegen aan Azure AD Premium. 

De volgende koppeling kunt u een nieuwe gebruiker in Azure AD via de Azure portal maken.

> [!div class="nextstepaction"]
> [Gebruikers toevoegen aan Azure AD](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/All users) 
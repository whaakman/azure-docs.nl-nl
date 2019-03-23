---
title: Toestemming van een beheerder voor LinkedIn-accountverbindingen - Azure Active Directory | Microsoft Docs
description: Wordt uitgelegd hoe u in- of uitschakelen van LinkedIn-accountverbindingen in Microsoft-apps in Azure Active Directory-integratie
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 03/21/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 2e07c53192ea2c8b792256af944c81c9c909dc55
ms.sourcegitcommit: 49c8204824c4f7b067cd35dbd0d44352f7e1f95e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/22/2019
ms.locfileid: "58368116"
---
# <a name="consent-to-linkedin-account-connections-for-an-azure-active-directory-organization"></a>Instemmen met LinkedIn-accountverbindingen voor een Azure Active Directory-organisatie

U kunt gebruikers toestaan in uw organisatie voor toegang tot hun verbindingen LinkedIn binnen bepaalde Microsoft-apps. Er zijn geen gegevens worden gedeeld totdat gebruikers toestemming geven voor hun accounts koppelen. U kunt uw organisatie in de Azure Active Directory (Azure AD) integreren [beheercentrum](https://aad.portal.azure.com).

> [!IMPORTANT]
> De LinkedIn-accountverbindingen instelling is beschikbaar gemaakt aan Azure AD-organisaties. Wanneer deze is geïmplementeerd voor uw organisatie, is standaard ingeschakeld.
> 
> Uitzonderingen:
> * De instelling is niet beschikbaar voor klanten die gebruikmaken van Microsoft-Cloud voor Amerikaanse overheid, Microsoft Cloud Duitsland, of Azure en Office 365 uitgevoerd door 21Vianet in China.
> * De instelling is standaard ingeschakeld voor tenants die zijn ingericht in Duitsland uitgeschakeld. Houd er rekening mee dat de instelling is niet beschikbaar voor klanten die gebruikmaken van Microsoft Cloud Duitsland.
> * De instelling is standaard ingeschakeld voor tenants die zijn ingericht in Frankrijk uitgeschakeld.
>
> Zodra de LinkedIn-accountverbindingen zijn ingeschakeld voor uw organisatie, wordt de accountverbindingen werken nadat gebruikers apps toestemming geven om toegang tot bedrijfsgegevens in hun naam. Zie voor meer informatie over de instelling van de gebruiker toestemming [verwijderen van een gebruiker toegang tot een toepassing](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="use-the-azure-portal-to-enable-linkedin-account-connections"></a>De Azure portal gebruiken voor het inschakelen van LinkedIn-accountverbindingen

U kunt de LinkedIn-accountverbindingen voor alleen de gebruikers die u toegang van uw hele organisatie naar alleen geselecteerde gebruikers in uw organisatie wilt inschakelen.

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com/) met een account dat een globale beheerder voor de Azure AD-organisatie.
2. Selecteer **gebruikers**.
3. Op de **gebruikers** Selecteer **gebruikersinstellingen**.
4. Onder **LinkedIn-accountverbindingen**, kunnen gebruikers verbinding maken met hun accounts voor toegang tot hun LinkedIn-verbindingen in sommige Microsoft-apps. Er zijn geen gegevens worden gedeeld totdat gebruikers toestemming geven voor hun accounts koppelen.

  * Selecteer **Ja** akkoord gaan met de service voor alle gebruikers in de organisatie
  * Selecteer **geselecteerde** om in te stemmen voor alleen geselecteerde gebruikers in de organisatie
  * Selecteer **Nee** toestemming voor gebruikers in uw organisatie in te trekken

    ![LinkedIn-accountverbindingen in de organisatie integreren](./media/linkedin-integration/linkedin-integration.png)

5. Wanneer u klaar bent, selecteert u **opslaan** uw instellingen op te slaan.
     
## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>Group Policy gebruiken om in te schakelen van LinkedIn-accountverbindingen

1. Download de [beheersjabloon van Office 2016-bestanden (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Pak de **ADMX** bestanden en kopieer deze naar uw centrale store.
3. Open Groepsbeleidsbeheer.
4. Maak een Group Policy Object met de volgende instelling: **Gebruikersconfiguratie** > **Beheersjablonen** > **Microsoft Office 2016** > **diverse**  >  **Weergeven LinkedIn-functies in Office-toepassingen**.
5. Selecteer **ingeschakeld** of **uitgeschakelde**.
  
   Status | Effect
   ------ | ------
   **Ingeschakeld** | De **weergeven LinkedIn-functies in Office-toepassingen** instelling in de opties voor Office 2016 is ingeschakeld. Gebruikers in uw organisatie kunnen LinkedIn-functies gebruiken in hun Office 2016-toepassingen.
   **Uitgeschakeld** | De **weergeven LinkedIn-functies in Office-toepassingen** instelling in de opties voor Office 2016 is uitgeschakeld en eindgebruikers deze instelling niet wijzigen. Gebruikers in uw organisatie kunnen LinkedIn-functies gebruiken in hun Office 2016-toepassingen.

Dit groepsbeleid is van invloed op alleen apps van Office 2016 voor een lokale computer. Als gebruikers LinkedIn in hun apps voor Office 2016 uitschakelt, zien ze nog steeds LinkedIn-functies in Office 365.

## <a name="next-steps"></a>Volgende stappen

* [Toestemming van de gebruiker en gegevens delen voor LinkedIn](linkedin-user-consent.md)

* [Gegevens van LinkedIn en functies in uw Microsoft-apps](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn help en ondersteuning](https://www.linkedin.com/help/linkedin)

* [De instelling van de huidige LinkedIn-integratie in Azure portal bekijken](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)

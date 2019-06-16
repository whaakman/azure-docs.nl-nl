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
ms.date: 04/29/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1130885cc3168582935264ffaad9fd7a8ba3c60b
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64920239"
---
# <a name="integrate-linkedin-account-connections-in-azure-active-directory"></a>LinkedIn-accountverbindingen in Azure Active Directory integreren

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

## <a name="enable-linkedin-account-connections-in-the-azure-portal"></a>LinkedIn-accountverbindingen in Azure portal inschakelen

U kunt de LinkedIn-accountverbindingen voor alleen de gebruikers die u toegang van uw hele organisatie naar alleen geselecteerde gebruikers in uw organisatie wilt inschakelen.

1. Aanmelden bij de [Azure AD-beheercentrum](https://aad.portal.azure.com/) met een account dat een globale beheerder voor de Azure AD-organisatie.
1. Selecteer **gebruikers**.
1. Op de **gebruikers** Selecteer **gebruikersinstellingen**.
1. Onder **LinkedIn-accountverbindingen**, kunnen gebruikers verbinding maken met hun accounts voor toegang tot hun LinkedIn-verbindingen in sommige Microsoft-apps. Er zijn geen gegevens worden gedeeld totdat gebruikers toestemming geven voor hun accounts koppelen.

    * Selecteer **Ja** om in te schakelen van de service voor alle gebruikers in uw organisatie
    * Selecteer **geselecteerde** zodat alleen een groep van geselecteerde gebruikers in uw organisatie
    * Selecteer **Nee** toestemming in te trekken van alle gebruikers in uw organisatie

    ![LinkedIn-accountverbindingen in de organisatie integreren](./media/linkedin-integration/linkedin-integration.png)

1. Wanneer u klaar bent, selecteert u **opslaan** uw instellingen op te slaan.

> [!Important]
> LinkedIn-integratie is niet volledig ingeschakeld voor uw gebruikers totdat ze toestemming geven om hun account verbinding te maken. Er zijn geen gegevens worden gedeeld wanneer u een accountverbindingen voor uw gebruikers inschakelt.

### <a name="assign-selected-users-with-a-group"></a>Geselecteerde gebruikers toewijzen aan een groep
De optie 'Geselecteerde' die Hiermee geeft u een lijst van gebruikers met de optie voor het selecteren van een groep gebruikers, zodat u de mogelijkheid om te verbinden, LinkedIn en Microsoft-accounts voor één groep in plaats van veel afzonderlijke gebruikers kunt inschakelen, hebben we vervangen. Als u geen LinkedIn-accountverbindingen ingeschakeld voor de geselecteerde afzonderlijke gebruikers, moet u niet verder niets te doen. Als u eerder LinkedIn-accountverbindingen voor geselecteerde afzonderlijke gebruikers hebt ingeschakeld, voert u de volgende stappen uit:

1. De huidige lijst met individuele gebruikers ophalen
1. De momenteel ingeschakelde afzonderlijke gebruikers te verplaatsen naar een groep
1. De groep van de vorige gebruiken als de geselecteerde groep in de LinkedIn-accountverbindingen instellen in de Azure AD-beheercentrum.

> [!NOTE]
> Zelfs als u niet de momenteel geselecteerde afzonderlijke gebruikers aan een groep verplaatst, zien ze nog steeds LinkedIn-gegevens in Microsoft-apps.

### <a name="get-the-current-list-of-selected-users"></a>De huidige lijst met geselecteerde gebruikers

1. Meld u aan bij Microsoft 365 met uw beheerdersaccount.
1. Ga naar https://linkedinselectedusermigration.azurewebsites.net/. U ziet de lijst met gebruikers die zijn geselecteerd voor LinkedIn-accountverbindingen.
1. De lijst exporteren naar een CSV-bestand.

### <a name="move-the-currently-selected-individual-users-to-a-group"></a>De geselecteerde afzonderlijke gebruikers te verplaatsen naar een groep

1. PowerShell starten
1. Azure AD-module installeren door te voeren `Install-Module AzureAD`
1. Voer het volgende script uit:

  ``` PowerShell
  $groupId = "GUID of the target group"
  
  $users = Get-Content 
  Path to the CSV file
  
  $i = 1
  foreach($user in $users} { Add-AzureADGroupMember -ObjectId $groupId -RefObjectId $user ; Write-Host $i Added $user ; $i++ ; Start-Sleep -Milliseconds 10 }
  ```

Zie voor het gebruik van de groep uit stap 2 als de geselecteerde groep in de LinkedIn-accountverbindingen instellen in de Azure AD-beheercentrum, [inschakelen LinkedIn-accountverbindingen in Azure portal](#enable-linkedin-account-connections-in-the-azure-portal).

## <a name="use-group-policy-to-enable-linkedin-account-connections"></a>Group Policy gebruiken om in te schakelen van LinkedIn-accountverbindingen

1. Download de [beheersjabloon van Office 2016-bestanden (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
1. Pak de **ADMX** bestanden en kopieer deze naar uw centrale store.
1. Open Groepsbeleidsbeheer.
1. Maak een Group Policy Object met de volgende instelling: **Gebruikersconfiguratie** > **Beheersjablonen** > **Microsoft Office 2016** > **diverse**  >  **Weergeven LinkedIn-functies in Office-toepassingen**.
1. Selecteer **ingeschakeld** of **uitgeschakelde**.
  
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

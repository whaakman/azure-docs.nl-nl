---
title: LinkedIn-integratie in Microsoft-apps - Azure Active Directory inschakelen | Microsoft Docs
description: Wordt uitgelegd hoe u in- of uitschakelen van LinkedIn-integratie voor Microsoft-apps in Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: d86b8dc271eead1196d946895ec7676935135cef
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56202482"
---
# <a name="linkedin-integration"></a>LinkedIn-integratie

In dit artikel leert u hoe u kunt in- of uitschakelen van LinkedIn-integratie voor uw tenant in het beheercentrum van Azure Active Directory (Azure AD).

> [!IMPORTANT]
> De instelling van de LinkedIn-integratie wordt momenteel geïmplementeerd naar Azure AD-tenants. Wanneer deze is geïmplementeerd voor uw tenant, is standaard ingeschakeld.
> 
> Uitzonderingen:
> * De instelling is niet beschikbaar voor klanten die gebruikmaken van Microsoft-Cloud voor Amerikaanse overheid, Microsoft Cloud Duitsland, of Azure en Office 365 uitgevoerd door 21Vianet in China.
> * De instelling is standaard ingeschakeld voor tenants die zijn ingericht in Duitsland uitgeschakeld. Houd er rekening mee dat de instelling is niet beschikbaar voor klanten die gebruikmaken van Microsoft Cloud Duitsland.
> * De instelling is standaard ingeschakeld voor tenants die zijn ingericht in Frankrijk uitgeschakeld.

> De integratie werkt alleen als u dit ingeschakeld hebt *en* als u toestaan dat gebruikers toestemming geven voor apps die toegang tot bedrijfsgegevens in hun naam. Zie voor meer informatie over de instelling van de toestemming [verwijderen van een gebruiker toegang tot een toepassing](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-the-azure-portal"></a>In- of uitschakelen van LinkedIn-integratie voor uw gebruikers in Azure portal

U kunt inschakelen of uitschakelen van LinkedIn-integratie voor uw hele tenant of alleen geselecteerde gebruikers in uw tenant.

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) met een account dat een globale beheerder voor de Azure AD-tenant.
2. Selecteer **gebruikers**.
3. Op de **gebruikers** Selecteer **gebruikersinstellingen**.
4. Onder **LinkedIn-integratie**:
  * Selecteer **Ja** LinkedIn-integratie voor alle gebruikers in uw tenant inschakelen
  * Selecteer **geselecteerde** LinkedIn-integratie voor alleen geselecteerde tenantgebruikers inschakelen
  * Selecteer **Nee** LinkedIn-integratie voor alle gebruikers uitschakelen ![inschakelen van LinkedIn-integratie](./media/linkedin-integration/linkedin-integration.png)
5. Uw instellingen opslaan wanneer u klaar bent met het selecteren van **opslaan**.

## <a name="enable-or-disable-linkedin-integration-for-your-users-in-group-policy"></a>In- of uitschakelen van LinkedIn-integratie voor uw gebruikers in Groepsbeleid

1. Download de [beheersjabloon van Office 2016-bestanden (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Pak de **ADMX** bestanden en kopieer deze naar uw centrale store.
3. Open Groepsbeleidsbeheer.
4. Maak een Group Policy Object met de volgende instelling: **Gebruikersconfiguratie** > **Beheersjablonen** > **Microsoft Office 2016** > **diverse**  >  **Weergeven LinkedIn-functies in Office-toepassingen**.
5. Selecteer **ingeschakeld** of **uitgeschakelde**.
  
 Status | Effect
------ | ------
**Ingeschakeld** | De **weergeven LinkedIn-functies in Office-toepassingen** instelling in de opties voor Office 2016 is ingeschakeld. Gebruikers in uw organisatie kunnen LinkedIn-functies gebruiken in hun Office-toepassingen.
 **Uitgeschakeld** | De **weergeven LinkedIn-functies in Office-toepassingen** instelling in de opties voor Office 2016 is uitgeschakeld en eindgebruikers deze instelling niet wijzigen. Gebruikers in uw organisatie kunnen LinkedIn-functies gebruiken in hun Office 2016-toepassingen.

Dit groepsbeleid is van invloed op alleen apps van Office 2016 voor een lokale computer. Gebruikers kunnen LinkedIn-functies in profiel kaarten in het hele Office 365 zien, zelfs als ze LinkedIn in hun Office 2016-apps uitschakelen.

## <a name="learn-more"></a>Meer informatie

* [LinkedIn integreren in uw organisatie](linkedin-user-consent.md)

* [Gegevens van LinkedIn en functies in uw Microsoft-apps](https://go.microsoft.com/fwlink/?linkid=850740)

* [LinkedIn help en ondersteuning](https://www.linkedin.com/help/linkedin)

## <a name="next-steps"></a>Volgende stappen

Gebruik de volgende koppeling om te zien van uw huidige LinkedIn-integratie instellen in Azure portal:

[De instelling van de huidige LinkedIn-integratie in Azure portal bekijken](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings)

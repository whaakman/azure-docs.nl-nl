---
title: Integratie van LinkedIn-verbindingen in Azure Active Directory inschakelen | Microsoft Docs
description: Wordt uitgelegd hoe u in- of uitschakelen van LinkedIn-accountverbindingen voor Microsoft-apps in Azure Active Directory
services: active-directory
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 09/11/2018
ms.author: curtand
ms.reviewer: beengen
ms.custom: it-pro
ms.openlocfilehash: 0eaa2656313ecd9b64503051265dc16285f0a1f3
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44392247"
---
# <a name="linkedin-account-connections"></a>LinkedIn-accountverbindingen

In dit artikel leert u hoe u kunt in- of uitschakelen van LinkedIn-accountverbindingen voor uw tenant in het beheercentrum van Azure Active Directory (Azure AD).

> [!IMPORTANT]
> De LinkedIn-accountverbindingen instellen wordt momenteel geïmplementeerd naar Azure AD-tenants. Wanneer deze is geïmplementeerd voor uw tenant, is standaard ingeschakeld. 
> 
> Uitzonderingen:
> * De instelling is niet beschikbaar voor klanten die gebruikmaken van Microsoft-Cloud voor Amerikaanse overheid, Microsoft Cloud Duitsland, of Azure en Office 365 uitgevoerd door 21Vianet in China.
> * De instelling is standaard ingeschakeld voor tenants die zijn ingericht in Duitsland uitgeschakeld. Houd er rekening mee dat de instelling is niet beschikbaar voor klanten die gebruikmaken van Microsoft Cloud Duitsland.
> * De instelling is standaard ingeschakeld voor tenants die zijn ingericht in Frankrijk uitgeschakeld.

> De integratie werkt alleen als u dit ingeschakeld hebt *en* als u toestaan dat gebruikers toestemming geven voor apps die toegang tot bedrijfsgegevens in hun naam. Zie voor meer informatie over de instelling van de toestemming [verwijderen van een gebruiker toegang tot een toepassing](https://docs.microsoft.com/azure/active-directory/application-access-assignment-how-to-remove-assignment).

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-in-the-azure-portal"></a>In- of uitschakelen van LinkedIn-accountverbindingen voor uw tenant in Azure portal

U kunt inschakelen of uitschakelen van LinkedIn-accountverbindingen voor uw hele tenant of alleen geselecteerde gebruikers in uw tenant.

1. Aanmelden bij de [Azure Active Directory-beheercentrum](https://aad.portal.azure.com/) met een account dat een globale beheerder voor de Azure AD-tenant.
2. Selecteer **gebruikers**.
3. Op de **gebruikers** Selecteer **gebruikersinstellingen**.
4. Onder **LinkedIn-accountverbindingen**:
  * Selecteer **Ja** LinkedIn-accountverbindingen voor alle gebruikers in uw tenant inschakelen
  * Selecteer **geselecteerde** om in te schakelen LinkedIn-account-verbindingen voor alleen geselecteerde tenant gebruikers
  * Selecteer **Nee** LinkedIn-accountverbindingen voor alle gebruikers uitschakelen ![inschakelen van LinkedIn-accountverbindingen](./media/linkedin-integration/linkedin-integration.png)
5. Uw instellingen opslaan wanneer u klaar bent met het selecteren van **opslaan**.

## <a name="enable-or-disable-linkedin-account-connections-for-your-tenant-using-group-policy"></a>In- of uitschakelen van LinkedIn-accountverbindingen voor uw tenant met behulp van Groepsbeleid

1. Download de [beheersjabloon van Office 2016-bestanden (ADMX/ADML)](https://www.microsoft.com/download/details.aspx?id=49030)
2. Pak de **ADMX** bestanden en kopieer deze naar uw centrale store.
3. Open Groepsbeleidsbeheer.
4. Een Group Policy Object maken met de volgende instellingen: **Gebruikersconfiguratie** > **Beheersjablonen** > **Microsoft Office 2016**  >  **Diversen** > **weergeven LinkedIn-functies in Office-toepassingen**.
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
Gebruik de volgende koppeling om te zien van uw huidige LinkedIn-accountverbindingen instellen in Azure portal:

[LinkedIn-accountverbindingen configureren](https://aad.portal.azure.com/#blade/Microsoft_AAD_IAM/UserManagementMenuBlade/UserSettings) 
---
title: Verificatie en gebruikersmachtigingen in Azure Analysis Services | Microsoft Docs
description: Meer informatie over verificatie en gebruikersmachtigingen in Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: a629c598c94e44713f8990871c46272dc48b1359
ms.sourcegitcommit: cf606b01726df2c9c1789d851de326c873f4209a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/19/2018
ms.locfileid: "46296274"
---
# <a name="authentication-and-user-permissions"></a>Verificatie en gebruikersmachtigingen
Azure Analysis Services gebruikt Azure Active Directory (Azure AD) voor beheer en verificatie. Een gebruiker maken, beheren of verbinding maken met een Azure Analysis Services server moet een geldige gebruikers-id een [Azure AD-tenant](../active-directory/fundamentals/active-directory-administer.md) in hetzelfde abonnement.

Azure Analysis Services ondersteunt [Azure AD B2B-samenwerking](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md). Met B2B, kunnen gebruikers van buiten een organisatie worden uitgenodigd als gastgebruikers ook kunnen in een Azure AD-directory. Gasten kunnen afkomstig zijn uit een andere Azure AD-tenant-directory of een geldig e-mailadres. Eenmaal uitgenodigd en de gebruiker de voorwaarden accepteert de uitnodiging per e-mail verzonden van Azure, de gebruikers-id wordt toegevoegd aan de tenant-directory. Deze identiteiten kunnen worden toegevoegd aan beveiligingsgroepen, of als leden van een beheerder of de database van een serverfunctie.

![Azure Analysis Services-authenticatie-architectuur](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Verificatie
Alle client-toepassingen en hulpprogramma's gebruiken een of meer van de Analysis Services [clientbibliotheken](analysis-services-data-providers.md) (AMO, MSOLAP, ADOMD) verbinding maken met een server. 

Alle drie deze clientbibliotheken ondersteuning voor Azure AD interactieve stroom en niet-interactieve verificatiemethoden. De twee methoden voor het niet-interactieve, de Active Directory-wachtwoord en de Active Directory Integrated Authentication methoden kunnen worden gebruikt in toepassingen die gebruikmaken van AMOMD en MSOLAP. Deze twee methoden worden nooit pop-dialoogvensters leiden.

Clienttoepassingen, zoals Excel en Power BI Desktop en hulpprogramma's zoals SSMS en SSDT installeren de nieuwste versies van de bibliotheken wanneer bijgewerkt naar de meest recente versie. Power BI Desktop, SSMS en SSDT wordt maandelijks bijgewerkt. Excel is [bijgewerkt met Office 365](https://support.office.com/en-us/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Office 365-updates zijn minder frequent optreden, en sommige organisaties gebruiken het uitgestelde kanaal, betekenis worden uitgesteld van drie maanden.

Afhankelijk van de clienttoepassing of -hulpprogramma dat u wilt gebruiken, kan het type verificatie en hoe u zich aanmeldt afwijken. Elke toepassing kan ondersteuning voor verschillende functies voor verbinding met cloudservices zoals Azure Analysis Services.

Power BI Desktop, SSDT en SSMS ondersteuning voor universele verificatie van Active Directory, een interactieve methode die ondersteuning biedt voor Azure multi-factor Authentication (MFA). Azure MFA helpt beveiligen de toegang tot gegevens en toepassingen terwijl er een eenvoudige aanmeldprocedure. Het biedt een robuuste verificatie met verschillende opties voor verificatie (telefoonoproep, SMS-bericht, smartcards en pincode of mobiele app-meldingen). Interactieve MFA met Azure AD kan resulteren in een pop-updialoogvenster voor validatie. **Universele authenticatie wordt aanbevolen**.

Als ze zich aanmelden bij Azure met behulp van een Windows-account en universele authenticatie niet is ingeschakeld of beschikbaar (Excel), [Active Directory Federation Services (AD FS)](../active-directory/hybrid/how-to-connect-fed-azure-adfs.md) is vereist. Met Federatie, Azure AD en Office 365-gebruikers worden geverifieerd met behulp van on-premises referenties en toegang tot Azure-resources.

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)
Azure Analysis Services-servers ondersteunen verbindingen van [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) en hoger met behulp van Windows-verificatie, wachtwoordverificatie voor Active Directory en Active Directory universele authenticatie. In het algemeen wordt aangeraden dat u Active Directory universele authenticatie gebruiken, omdat:

*  Biedt ondersteuning voor interactieve en niet-interactieve verificatiemethoden.

*  Biedt ondersteuning voor Azure B2B-gastgebruikers uitgenodigd voor de tenant Azure AS. Wanneer u verbinding maakt met een server, moeten gastgebruikers ook kunnen Active Directory universele authenticatie selecteren bij het verbinden met de server.

*  Biedt ondersteuning voor multi-factor Authentication (MFA). Azure MFA helpt beveiligen de toegang tot gegevens en toepassingen met een scala aan opties voor verificatie: telefoonoproep, SMS-bericht, smartcards en pincode of mobiele app-meldingen. Interactieve MFA met Azure AD kan resulteren in een pop-updialoogvenster voor validatie.

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)
SSDT maakt verbinding met Azure Analysis Services met behulp van Active Directory universele authenticatie met ondersteuning voor MFA. Gebruikers wordt gevraagd om u te melden bij Azure bij de eerste implementatie. Gebruikers moeten zich aanmelden bij Azure met een account met machtigingen voor serverbeheerders op de server die ze implementeert. Aan te melden bij Azure de eerste keer, is een token toegewezen. SSDT slaat de token in het geheugen voor toekomstige aantal hersteld.

### <a name="power-bi-desktop"></a>Power BI Desktop
Power BI Desktop maakt verbinding met Azure Analysis Services met behulp van Active Directory universele authenticatie met ondersteuning voor MFA. Gebruikers wordt gevraagd om u te melden bij Azure op de eerste verbinding. Gebruikers moeten aanmelden bij Azure met een account dat is opgenomen in een serverbeheerder of een databaserol.

### <a name="excel"></a>Excel
Excel-gebruikers kunnen verbinding maken met een server met behulp van een Windows-account, een organisatie-ID (e-mailadres) of een externe e-mailadres. Externe e-id's moeten zich in de Azure AD als een gastgebruiker.

## <a name="user-permissions"></a>Gebruikersmachtigingen

**Serverbeheerders** specifiek zijn voor een Azure Analysis Services-server-exemplaar. Ze verbinding maken met hulpprogramma's als Azure portal, SSMS en SSDT om uit te voeren taken, zoals databases toe te voegen en gebruikersrollen beheren. Standaard wordt de gebruiker die wordt gemaakt van de server automatisch toegevoegd als de beheerder van een Analysis Services-server. Andere beheerders kunnen worden toegevoegd met behulp van Azure portal of SSMS. Serverbeheerders moeten een account hebben in de Azure AD-tenant in hetzelfde abonnement. Zie voor meer informatie, [serverbeheerders beheren](analysis-services-server-admins.md). 

**Gebruikers van de database** model databases verbinden met behulp van clienttoepassingen, zoals Excel of Power BI. Gebruikers moeten worden toegevoegd aan de databaserollen. Databaserollen definiëren administrator, verwerken of machtigingen voor lezen voor een database. Het is belangrijk om te begrijpen databasegebruikers in een rol met de beheerdersmachtigingen is anders dan de server-beheerders. Standaard zijn server-beheerders echter ook databasebeheerders. Zie voor meer informatie, [databaserollen en gebruikers beheren](analysis-services-database-users.md).

**Azure-resource-eigenaren**. Resource-eigenaars beheren van resources voor een Azure-abonnement. Resource-eigenaren kunnen toevoegen gebruikers-id's van Azure AD-eigenaar of Inzender rollen binnen een abonnement met behulp van **toegangsbeheer** in Azure portal of met Azure Resource Manager-sjablonen. 

![Toegangsbeheer in Azure portal](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Rollen op dit niveau van toepassing op gebruikers of accounts die u nodig hebt om uit te voeren taken die kunnen worden uitgevoerd in de portal of met behulp van Azure Resource Manager-sjablonen. Zie voor meer informatie, [Role-Based Access Control](../role-based-access-control/overview.md). 


## <a name="database-roles"></a>Databaserollen

 Rollen die zijn gedefinieerd voor een model in tabelvorm zijn databaserollen. Dat wil zeggen, de rollen bevatten leden die bestaat uit een Azure AD-gebruikers en beveiligingsgroepen waarop specifieke machtigingen die de actie definiëren die leden kunnen worden uitgevoerd op een modeldatabase. Een databaserol wordt gemaakt als een afzonderlijk object in de database en is alleen van toepassing op de database waarin die rol is gemaakt.   
  
 Standaard, wanneer u een nieuw project voor tabellair model maakt heeft het project voor een model geen rollen. Rollen kunnen worden gedefinieerd met behulp van het dialoogvenster Role Manager in SSDT. Wanneer er rollen zijn gedefinieerd tijdens het ontwerpen van model-project, worden ze alleen toegepast op de werkruimtedatabase model. Wanneer het model is geïmplementeerd, wordt dezelfde rollen worden toegepast op de geïmplementeerd model. Nadat een model is geïmplementeerd, server en databasebeheerders kunnen rollen en leden beheren met behulp van SSMS. Zie voor meer informatie, [databaserollen en gebruikers beheren](analysis-services-database-users.md).
  


## <a name="next-steps"></a>Volgende stappen

[Toegang tot resources beheren met Azure Active Directory-groepen](../active-directory/fundamentals/active-directory-manage-groups.md)   
[Databaserollen en gebruikers beheren](analysis-services-database-users.md)  
[Serverbeheerders beheren](analysis-services-server-admins.md)  
[Op rollen gebaseerd toegangsbeheer](../role-based-access-control/overview.md)  
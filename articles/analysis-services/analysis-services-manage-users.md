---
title: Verificatie en gebruikersmachtigingen in Azure Analysis Services | Microsoft Docs
description: Meer informatie over verificatie en gebruikersmachtigingen in Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 10/09/2017
ms.author: owend
ms.openlocfilehash: e7fdb55ba29fbdc2f3d89fbb19c8b77bf2c05795
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="authentication-and-user-permissions"></a>Verificatie en gebruikersmachtigingen
Azure Analysis Services gebruikt Azure Active Directory (Azure AD) voor identity management- en gebruikersverificatie. Een willekeurige gebruiker maken, beheren of verbinding maken met een Azure Analysis Services server moet een geldige gebruikers-id een [Azure AD-tenant](../active-directory/active-directory-administer.md) in hetzelfde abonnement.

Azure Analysis Services ondersteunt [Azure AD B2B-samenwerking](../active-directory/active-directory-b2b-what-is-azure-ad-b2b.md). Met B2B, kunnen gebruikers van buiten een organisatie worden uitgenodigd als gastgebruikers in een Azure Active directory. Gasten kunnen afkomstig zijn uit een andere Azure AD-tenant directory of een geldig e-mailadres. Eenmaal uitgenodigd en de gebruiker accepteert de uitnodiging per e-mail verzonden van Azure, de gebruikers-id wordt toegevoegd aan de tenantmap. Deze identiteiten kunnen worden toegevoegd aan beveiligingsgroepen of als leden van een server-rol van systeembeheerder of de database.

![Authenticatie-architectuur van Azure Analysis Services](./media/analysis-services-manage-users/aas-manage-users-arch.png)

## <a name="authentication"></a>Authentication
Alle client-toepassingen en hulpprogramma's gebruiken een of meer van de Analysis Services [clientbibliotheken](analysis-services-data-providers.md) (AMO MSOLAP, ADOMD) verbinding maken met een server. 

Alle drie clientbibliotheken ondersteuning voor Azure AD interactieve stroom en niet-interactieve verificatiemethoden. De Active Directory Integrated Authentication methoden van twee methoden voor niet-interactieve, Active Directory-wachtwoord en kunnen worden gebruikt in toepassingen die gebruikmaken van AMOMD en MSOLAP. Deze twee methoden worden nooit leiden tot pop-dialoogvensters.

Clienttoepassingen, zoals Excel en Power BI Desktop en hulpmiddelen zoals SSMS en SSDT installeren de nieuwste versies van de bibliotheken wanneer bijgewerkt naar de meest recente versie. Power BI Desktop SSMS en SSDT maandelijks bijgewerkt. Is Excel [bijgewerkt met Office 365](https://support.office.com/en-us/article/When-do-I-get-the-newest-features-in-Office-2016-for-Office-365-da36192c-58b9-4bc9-8d51-bb6eed468516). Office 365-updates zijn minder frequente en sommige organisaties gebruiken om de uitgestelde kanaal, betekenis updates worden uitgesteld tot drie maanden.

Afhankelijk van de client-toepassing of een hulpprogramma waarmee u kan het type verificatie en hoe u zich aanmeldt afwijken. Elke toepassing kan verschillende functies die ondersteuning voor het verbinden met cloudservices zoals Azure Analysis Services.

Power BI Desktop en SSDT SSMS ondersteuning voor universele verificatie van Active Directory, een interactieve methode biedt ook ondersteuning voor Azure multi-factor Authentication (MFA). Azure MFA helpt beveiliging toegang tot gegevens en toepassingen waarbij een eenvoudig proces aanmelden. Geavanceerde verificatie met verschillende opties voor verificatie (telefoongesprek, tekstbericht, smartcards en pincode of mobiele-appmelding) levert. Interactieve MFA met Azure AD kan resulteren in een pop-dialoogvenster voor validatie. **Universele-verificatie wordt aanbevolen**.

Als het aanmelden bij Azure met behulp van een Windows-account en universele verificatie niet is ingeschakeld of beschikbaar (Excel) [Active Directory Federation Services (AD FS)](../active-directory/connect/active-directory-aadconnect-azure-adfs.md) is vereist. Met Federatie, Azure AD en Office 365-gebruikers worden geverifieerd met behulp van on-premises referenties en toegang tot Azure-resources.

### <a name="sql-server-management-studio-ssms"></a>SQL Server Management Studio (SSMS)
Azure Analysis Services-servers ondersteunen verbindingen van [SSMS V17.1](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms) en hoger met behulp van Windows-verificatie, Active Directory-wachtwoordverificatie en universele verificatie van Active Directory. In het algemeen kunt dat u Active Directory-Universal-verificatie gebruiken, omdat:

*  Biedt ondersteuning voor interactieve en niet-interactieve verificatiemethoden.

*  Biedt ondersteuning voor Azure B2B gastgebruikers uitgenodigd in de AS Azure-tenant. Bij het verbinden met een server, moeten gastgebruikers Universal verificatie van Active Directory selecteren bij het verbinden met de server.

*  Biedt ondersteuning voor multi-factor Authentication (MFA). Azure MFA helpt beveiliging toegang tot gegevens en toepassingen met een aantal opties voor verificatie: telefoonoproep, tekstbericht, smartcards en pincode of mobiele app-melding. Interactieve MFA met Azure AD kan resulteren in een pop-dialoogvenster voor validatie.

### <a name="sql-server-data-tools-ssdt"></a>SQL Server Data Tools (SSDT)
SSDT maakt verbinding met Azure Analysis Services met behulp van Active Directory-Universal-verificatie met MFA-ondersteuning. Aanmelden bij Azure bij de eerste implementatie worden gebruikers gevraagd. Gebruikers moeten aanmelden bij Azure met een account met machtigingen voor server-beheerder op de server die naar implementeert. Tijdens het aanmelden bij Azure de eerste keer, is een token toegewezen. SSDT slaat de token in het geheugen voor toekomstige aantal hersteld.

### <a name="power-bi-desktop"></a>Power BI Desktop
Power BI Desktop verbindt met Azure Analysis Services met behulp van Active Directory-Universal-verificatie met ondersteuning voor MFA. Aanmelden bij Azure op de eerste verbinding worden gebruikers gevraagd. Gebruikers moeten aanmelden bij Azure met een account dat is opgenomen in een serverbeheerder of een databaserol.

### <a name="excel"></a>Excel
Excel-gebruikers kunnen verbinding maken met een server met behulp van een Windows-account, een organisatie-ID (e-mailadres) of een externe e-mailadres. Externe e-identiteiten moeten in de Azure AD als gastgebruiker bestaan.

## <a name="user-permissions"></a>Gebruikersmachtigingen

**Serverbeheerders** specifiek zijn voor een Azure Analysis Services-server-exemplaar. Ze verbinding maken met hulpmiddelen zoals Azure-portal, SSMS en SSDT voor taken zoals het toevoegen van databases en gebruikersrollen beheren. Standaard wordt de gebruiker die de server maakt automatisch toegevoegd als een Analysis Services-server-beheerder. Andere beheerders kunnen met behulp van Azure-portal of SSMS worden toegevoegd. Serverbeheerders moeten een account hebben in de Azure AD-tenant in hetzelfde abonnement. Zie voor meer informatie, [serverbeheerders beheren](analysis-services-server-admins.md). 

**Gebruikers van de database** verbinding maken met databases model met client-toepassingen zoals Excel of Power BI. Gebruikers moeten worden toegevoegd aan databaserollen. Databaserollen definiëren administrator, proces of leesmachtigingen voor een database. Het is belangrijk te weten databasegebruikers met een rol met beheerdermachtigingen is anders dan serverbeheerders. Standaard zijn serverbeheerders echter ook databasebeheerders. Zie voor meer informatie, [databaserollen en gebruikers beheren](analysis-services-database-users.md).

**Azure-resource eigenaars**. Resource-eigenaars beheren resources voor een Azure-abonnement. Resource-eigenaars kunnen toevoegen gebruikers-id's van Azure AD-eigenaar of bijdrager rollen binnen een abonnement met **toegangsbeheer** in Azure portal of met Azure Resource Manager-sjablonen. 

![Toegangsbeheer in Azure-portal](./media/analysis-services-manage-users/aas-manage-users-rbac.png)

Rollen op dit niveau van toepassing op gebruikers of de accounts die u nodig hebt voor het uitvoeren van taken die kunnen worden uitgevoerd in de portal of met behulp van Azure Resource Manager-sjablonen. Zie voor meer informatie, [toegangsbeheer op basis van rollen](../active-directory/role-based-access-control-what-is.md). 


## <a name="database-roles"></a>Databaserollen

 Rollen die zijn gedefinieerd voor een model in tabelvorm zijn databaserollen. Dat wil zeggen, de rollen bevatten leden die bestaan uit Azure AD-gebruikers en beveiligingsgroepen die specifieke machtigingen die definieert de actie die leden hebben een modeldatabase kunnen overnemen. Een databaserol als een afzonderlijk object in de database wordt gemaakt, en geldt alleen voor de database waarin die rol is gemaakt.   
  
 Standaard, wanneer u een nieuw project voor het model in tabelvorm maakt heeft het modelproject geen rollen. Rollen kunnen worden gedefinieerd met behulp van het dialoogvenster rolbeheer in SSDT. Wanneer er rollen zijn gedefinieerd tijdens het modelontwerp project, worden ze alleen toegepast op de database van de werkruimte model. Wanneer het model wordt geïmplementeerd, worden dezelfde rollen worden toegepast op de geïmplementeerde model. Nadat een model is geïmplementeerd, server en databasebeheerders kunnen rollen en leden beheren met behulp van SSMS. Zie voor meer informatie, [databaserollen en gebruikers beheren](analysis-services-database-users.md).
  


## <a name="next-steps"></a>Volgende stappen

[Toegang tot resources beheren met Azure Active Directory-groepen](../active-directory/active-directory-manage-groups.md)   
[Databaserollen en gebruikers beheren](analysis-services-database-users.md)  
[Serverbeheerders beheren](analysis-services-server-admins.md)  
[Op rollen gebaseerd toegangsbeheer](../active-directory/role-based-access-control-what-is.md)  
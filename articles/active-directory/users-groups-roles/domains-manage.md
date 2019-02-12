---
title: Toevoegen en controleren van aangepaste domeinnamen - Azure Active Directory | Microsoft Docs
description: Management-concepten en procedures voor het beheren van een domeinnaam in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: article
ms.date: 01/31/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 7822b5abe78bf969171f7f6ee7d706dadb8d0188
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56100445"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Aangepaste domeinnamen in uw Azure Active Directory beheren

De naam van een domein is een belangrijk onderdeel van de id voor veel directoryresources: het deel uitmaakt van een gebruikersnaam of e-mailadres voor een gebruiker, onderdeel van het adres voor een groep, en is het soms onderdeel van de app-ID-URI voor een toepassing. Een resource in Azure Active Directory (Azure AD) zijn de domeinnaam van een dat eigendom van de map waarin de resource. Alleen een globale beheerder kan domeinen beheren in Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Instellen van de primaire domeinnaam voor uw Azure AD-directory

Wanneer de map wordt gemaakt, wordt de initiële domeinnaam, zoals 'contoso.onmicrosoft.com', is ook de naam van het primaire domein. Het primaire domein is de standaard-domeinnaam voor een nieuwe gebruiker bij het maken van een nieuwe gebruiker. Instellen van een primaire domeinnaam stroomlijnt het gehele proces waarmee een beheerder om te maken van nieuwe gebruikers in de portal. Naam van het primaire domein wijzigen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat een globale beheerder voor de map.
2. Selecteer **Azure Active Directory**.
3. Selecteer **Namen van aangepaste domeinen**.
  
   ![Gebruikersbeheer openen](./media/domains-manage/add-custom-domain.png)
4. Selecteer de naam van het domein dat u wilt worden van het primaire domein.
5. Selecteer de **instellen als primaire domeinnaam** opdracht. Bevestig uw keuze wanneer hierom wordt gevraagd.
  
   ![Een domeinnaam instellen als primaire domeinnaam](./media/domains-manage/make-primary-domain.png)

U kunt de primaire domeinnaam voor uw directory moet een gecontroleerd aangepast domein dat niet is gefedereerd wijzigen. Wijzigen van het primaire domein voor uw directory, niet de naam van de gebruiker voor alle bestaande gebruikers gewijzigd.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Aangepaste domeinnamen toevoegt aan uw Azure AD-tenant

U kunt maximaal 900 beheerde domeinnamen toevoegen. Als u uw domeinen voor federatie met on-premises Active Directory configureert, kunt u maximaal 450 domeinnamen toevoegen in elke map.

## <a name="add-subdomains-of-a-custom-domain"></a>Subdomeinen van een aangepast domein toevoegen

Als u wilt een derde niveau domeinnaam zoals 'europe.contoso.com' toevoegen aan uw directory, moet u eerst toevoegen en controleer of het domein op het tweede niveau, zoals contoso.com. Het subdomein wordt automatisch geverifieerd met Azure AD. Als u wilt zien dat het subdomein dat u hebt toegevoegd is geverifieerd, de domeinlijst in de browser te vernieuwen.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Wat te doen als u de DNS-registratieservice voor uw aangepaste domeinnaam wijzigen

Als u de DNS-registrars wijzigt, zijn er geen aanvullende configuratie-taken in Azure AD. U kunt met behulp van de domeinnaam met Azure AD zonder onderbreking. Als u uw aangepaste domeinnaam met Office 365, Intune of andere services die afhankelijk van aangepaste domeinnamen in Azure AD gebruikt zijn, Zie de documentatie voor de services.

## <a name="delete-a-custom-domain-name"></a>De naam van een aangepast domein verwijderen

Als uw organisatie die domeinnaam niet meer gebruikt of als u wilt gebruiken die domeinnaam met een andere Azure AD, kunt u een aangepaste domeinnaam verwijderen uit uw Azure AD.

Als u wilt verwijderen van een aangepaste domeinnaam, moet u eerst controleren of er zijn geen resources in uw directory afhankelijk van de domeinnaam zijn. U kunt een domeinnaam verwijderen uit uw directory als:

* Elke gebruiker heeft een gebruikersnaam, het e-mailadres of de proxy-adres dat de domeinnaam bevat.
* Een groep heeft een e-mailadres of de proxy-adres dat de domeinnaam bevat.
* Elke toepassing in uw Azure AD heeft een app-ID-URI die de domeinnaam bevat.

U moet wijzigen of verwijderen van een dergelijke resource in Azure Active directory voordat u de aangepaste domeinnaam kunt verwijderen.

### <a name="forcedelete-option"></a>ForceDelete-optie

U kunt **ForceDelete** een domeinnaam in de [Azure AD-beheercentrum](https://aad.portal.azure.com) of met behulp van [Microsoft Graph API](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta). Deze opties gebruiken een asynchrone bewerking en werk alle verwijzingen van de aangepaste domeinnaam, zoals 'user@contoso.com'aan de aanvankelijke standaarddomeinnaam zoals"user@contoso.onmicrosoft.com." 

Om aan te roepen **ForceDelete** in Azure portal, moet u ervoor zorgen dat er minder dan 1000 verwijzingen naar de domeinnaam, en alle verwijzingen waarop Exchange de provisioning-service wordt moeten worden bijgewerkt of verwijderd uit de [ Exchange-beheercentrum](https://outlook.office365.com/ecp/). Dit omvat Exchange Mail-Enabled-beveiligingsgroepen en gedistribueerde lijsten; Zie voor meer informatie, [verwijderen van beveiligingsgroepen met mailmogelijkheden](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups). Ook de **ForceDelete** bewerking wordt niet uitgevoerd als een van de volgende voorwaarden wordt voldaan:

* U hebt aangeschaft van een domein via Office 365-abonnement domeinservices
* U bent een partner beheren namens een andere tenant van de klant

De volgende acties worden uitgevoerd als onderdeel van de **ForceDelete** bewerking:

* Wijzigt de naam van de UPN, EmailAddress en ProxyAddress van gebruikers met verwijzingen naar de aangepaste domeinnaam voor de initiële standaarddomeinnaam.
* Wijzigt de naam van de EmailAddress van groepen met verwijzingen naar de aangepaste domeinnaam voor de initiële standaarddomeinnaam.
* Wijzigt de naam van de identifierUris van toepassingen met verwijzingen naar de aangepaste domeinnaam voor de initiële standaarddomeinnaam.

Een fout wordt geretourneerd wanneer:

* Het aantal objecten dat moet worden gewijzigd, is groter is dan 1000
* Een van de toepassingen te worden gewijzigd, is een multitenant-app

### <a name="frequently-asked-questions"></a>Veelgestelde vragen

**V: Waarom is het domein verwijderen mislukt met een fout met de mededeling dat ik heb onder de knie Exchange-groepen op de domeinnaam van dit?** <br>
**A:** Op dit moment, bepaalde groepen, zoals berichtgrootte-beveiligingsgroepen en gedistribueerde lijsten zijn ingericht door Exchange en moeten handmatig worden opgeschoond [Exchange Admin Center (EAC)](https://outlook.office365.com/ecp/). Mogelijk worden er ProxyAddresses die afhankelijk zijn van de aangepaste domeinnaam en moet handmatig worden bijgewerkt naar een andere domeinnaam achtergebleven. 

**V: Ik ben aangemeld als admin@contoso.com , maar ik kan de domeinnaam 'contoso.com' niet verwijderen?**<br>
**A:** U kunt de aangepaste domeinnaam die u probeert te verwijderen in de naam van uw gebruikersaccount kan niet verwijzen naar. Zorg ervoor dat het account voor globale beheerders met behulp van de aanvankelijke standaarddomeinnaam (. onmicrosoft.com) zoals admin@contoso.onmicrosoft.com. Meld u aan met een andere globale beheerder zoals die account admin@contoso.onmicrosoft.com of een andere aangepaste domeinnaam zoals 'fabrikam.com"waar het account is admin@fabrikam.com.

**V: Ik klikte op de knop verwijderen-domein en Zie `In Progress` status voor de bewerking verwijderen. Hoe lang duurt het voordat? Wat gebeurt er als dit mislukt?**<br>
**A:** De bewerking voor het verwijderen van domein is een asynchrone achtergrondtaak die wijzigt de naam van alle verwijzingen naar de domeinnaam. Het zou binnen een of twee minuten voltooid. Als het domein verwijderen is mislukt, zorgt u ervoor dat u hebt geen:

* Apps die zijn geconfigureerd op de naam van het domein met de appIdentifierURI
* Een e-mailgroep verwijst naar de aangepaste domeinnaam
* Meer dan 1000 verwijzingen naar de domeinnaam

Als u een van de voorwaarden nog niet is voldaan, handmatig opschonen van de referenties en probeer opnieuw te verwijderen het domein.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Gebruik PowerShell of Graph API om domeinnamen te beheren

De meeste beheertaken voor domeinnamen in Azure Active Directory kunnen ook worden uitgevoerd met behulp van PowerShell voor Microsoft of programmatisch met behulp van Azure AD Graph API.

* [Met behulp van PowerShell voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Met behulp van Graph API om domeinnamen te beheren in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Volgende stappen

* [Aangepaste domeinnamen toevoegen](/azure/active-directory/fundamentals/add-custom-domain?context=azure/active-directory/users-groups-roles/context/ugr-context)
* [Verwijderen van de Exchange-e-mailadres beveiligingsgroepen in Exchange-beheercentrum op een aangepaste domeinnaam in Azure AD](https://technet.microsoft.com/library/bb123521(v=exchg.160).aspx#Remove%20mail-enabled%20security%20groups)
* [Een aangepaste domeinnaam ForceDelete met Microsoft Graph API](https://docs.microsoft.com/graph/api/domain-forcedelete?view=graph-rest-beta)

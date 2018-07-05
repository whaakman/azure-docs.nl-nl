---
title: Aangepaste domeinnamen in uw Azure Active Directory beheren | Microsoft Docs
description: Management-concepten en procedures voor het beheren van een domeinnaam in Azure Active Directory
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.workload: identity
ms.component: users-groups-roles
ms.topic: article
ms.date: 11/14/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.openlocfilehash: b503ef4c5cd922d4f7b58940cfc98a83a78ae986
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37449919"
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Aangepaste domeinnamen in uw Azure Active Directory beheren
De naam van een domein is een belangrijk onderdeel van de id voor veel directoryresources: dit maakt deel uit van een gebruikersnaam of e-mailadres voor een gebruiker, onderdeel van het adres voor een groep, en kunnen deel uitmaken van de app-ID-URI voor een toepassing. Een resource in Azure Active Directory (Azure AD) kan een domeinnaam die al is geverifieerd als eigendom van de map waarin de resource bevatten. Alleen een globale beheerder kan domein-beheertaken uitvoeren in Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>Instellen van de primaire domeinnaam voor uw Azure AD-directory
Wanneer de map wordt gemaakt, wordt de initiële domeinnaam, zoals 'contoso.onmicrosoft.com', is ook de naam van het primaire domein. Het primaire domein is de standaard-domeinnaam voor een nieuwe gebruiker bij het maken van een nieuwe gebruiker. Instellen van een primaire domeinnaam stroomlijnt het gehele proces waarmee een beheerder om te maken van nieuwe gebruikers in de portal. Naam van het primaire domein wijzigen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account dat een globale beheerder voor de map.
2. Selecteer **Azure Active Directory**.
3. Selecteer **aangepaste-domeinnamen**.
     
   ![Gebruikersbeheer openen](./media/domains-manage/add-custom-domain.png)
4. Selecteer de naam van het domein dat u wilt worden van het primaire domein.
5. Selecteer de **instellen als primaire domeinnaam** opdracht. Bevestig uw keuze wanneer hierom wordt gevraagd.
   
   ![Een domeinnaam instellen als primaire domeinnaam](./media/domains-manage/make-primary-domain.png)

U kunt de primaire domeinnaam voor uw directory moet een gecontroleerd aangepast domein dat niet is gefedereerd wijzigen. Wijzigen van het primaire domein voor uw directory, worden de gebruikersnamen voor alle bestaande gebruikers niet gewijzigd.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Aangepaste domeinnamen toevoegt aan uw Azure AD-tenant
U kunt tot maximaal 900 domeinnamen van het beheerde toevoegen. Als u uw domeinen voor federatie met on-premises Active Directory configureren wilt, kunt u zich kan oplopen tot een maximum van 450 domeinnamen in elke map. Zie voor meer informatie, [federatieve en beheerde domeinnamen](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="add-subdomains-of-a-custom-domain"></a>Subdomeinen van een aangepast domein toevoegen
Als u wilt een derde niveau domeinnaam zoals 'europe.contoso.com' toevoegen aan uw directory, moet u eerst toevoegen en controleer of het domein op het tweede niveau, zoals contoso.com. Het subdomein zal automatisch worden gecontroleerd door Azure AD. Als u wilt zien dat het subdomein dat u zojuist hebt toegevoegd is geverifieerd, de pagina in de browser met een lijst met de domeinen te vernieuwen.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Wat te doen als u de DNS-registratieservice voor uw aangepaste domeinnaam wijzigen
Als u de DNS-registratieservice voor uw aangepaste domeinnaam wijzigt, kunt u echter ook doorgaan met de naam van uw aangepaste domein met Azure AD zelf zonder onderbreking en zonder aanvullende configuratietaken. Als u uw aangepaste domeinnaam met Office 365, Intune of andere services die afhankelijk van aangepaste domeinnamen in Azure AD zijn gebruikt, raadpleegt u de documentatie voor die services.

## <a name="delete-a-custom-domain-name"></a>De naam van een aangepast domein verwijderen
Als uw organisatie die domeinnaam niet meer gebruikt of als u wilt gebruiken die domeinnaam met een andere Azure AD, kunt u een aangepaste domeinnaam verwijderen uit uw Azure AD.

Als u wilt verwijderen van een aangepaste domeinnaam, moet u eerst controleren of er zijn geen resources in uw directory afhankelijk van de domeinnaam zijn. U kunt een domeinnaam verwijderen uit uw directory als:

* Elke gebruiker heeft een gebruikersnaam, het e-mailadres of de proxy-adres dat de domeinnaam bevat.
* Een groep heeft een e-mailadres of de proxy-adres dat de domeinnaam bevat.
* Elke toepassing in uw Azure AD heeft een app-ID-URI die de domeinnaam bevat.

U moet wijzigen of verwijderen van een dergelijke resource in Azure Active directory voordat u de aangepaste domeinnaam kunt verwijderen.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Gebruik PowerShell of Graph API om domeinnamen te beheren
De meeste beheertaken voor domeinnamen in Azure Active Directory kunnen ook worden uitgevoerd met behulp van PowerShell voor Microsoft of programmatisch met behulp van Azure AD Graph API.

* [Met behulp van PowerShell voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Met behulp van Graph API om domeinnamen te beheren in Azure AD](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Volgende stappen
* [Aangepaste domeinnamen toevoegen](../fundamentals/add-custom-domain.md)


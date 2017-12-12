---
title: Aangepaste domeinnamen in uw Azure Active Directory beheren | Microsoft Docs
description: Beheerconcepten en uitleg over het beheren van een domeinnaam in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: 5063cd0a-dba2-4ba9-aa65-b8117490d73a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/14/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.openlocfilehash: 64c1be4358305a736ac1dd8a1b7194c80100d256
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Aangepaste domeinnamen in uw Azure Active Directory beheren
Een domeinnaam is een belangrijk onderdeel van de id voor veel directory-resources: het deel uitmaakt van een gebruikersnaam of e-mailadres voor een gebruiker deel van het adres voor een groep, en kan deel uitmaken van de app-ID-URI voor een toepassing. Een resource in Azure Active Directory (Azure AD), kan een domeinnaam die al is geverifieerd als eigendom van de map die de bron bevat bevatten. Alleen een globale beheerder kunt domein-beheertaken uitvoeren in Azure AD.

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>De naam van het primaire domein voor uw Azure AD-directory instellen
Wanneer de map wordt gemaakt, wordt de initiële domeinnaam, zoals 'contoso.onmicrosoft.com', is ook de naam van de primaire domeincontroller. Het primaire domein is de naam van het standaarddomein voor een nieuwe gebruiker bij het maken van een nieuwe gebruiker. Een primaire domeinnaam instellen stroomlijnt het proces voor de beheerder nieuwe gebruikers maken in de portal. De naam van de primaire domeincontroller wijzigen:

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) met een account met globale beheerdersrechten voor de map.
2. Selecteer **Azure Active Directory**.
3. Selecteer **aangepaste domeinnamen**.
     
   ![Gebruikersbeheer openen](./media/active-directory-domains-manage-azure-portal/add-custom-domain.png)
4. Selecteer de naam van het domein dat u wilt worden van het primaire domein.
5. Selecteer de **primair** opdracht. Bevestig uw keuze wanneer u wordt gevraagd.
   
   ![Een domeinnaam primair maken](./media/active-directory-domains-manage-azure-portal/make-primary-domain.png)

U kunt de primaire domeinnaam voor uw directory om te worden geverifieerd aangepaste domeinen die niet federatief wijzigen. Het wijzigen van het primaire domein voor uw directory heeft geen invloed op de gebruikersnamen voor alle bestaande gebruikers.

## <a name="add-custom-domain-names-to-your-azure-ad-tenant"></a>Aangepaste domeinnamen toevoegen aan uw Azure AD-tenant
U kunt tot maximaal 900 beheerde domeinnamen toevoegen. Als u uw domeinen voor federatie met lokale Active Directory configureren wilt, kunt u met een maximum van 450 domeinnamen in elke directory up toevoegen. Zie voor meer informatie [federatieve en beheerde domeinnamen](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain-concepts#federated-and-managed-domain-names).

## <a name="add-subdomains-of-a-custom-domain"></a>Toevoegen van subdomeinen van een aangepast domein
Als u wilt een derde niveau domeinnaam zoals 'europe.contoso.com' toevoegen aan uw directory, moet u eerst toevoegen en controleren van het domein van de tweede niveau, zoals contoso.com. Het subdomein wordt automatisch geverifieerd door Azure AD. Overzicht van het subdomein dat u zojuist hebt toegevoegd is geverifieerd, vernieuw de pagina in de browser die domeinen.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Wat te doen als u de DNS-registratieservice voor uw aangepaste domeinnaam wijzigen
Als u de DNS-registratieservice voor uw aangepaste domeinnaam wijzigt, kunt u uw aangepaste domeinnaam gebruiken met Azure AD zelf zonder onderbreking en zonder aanvullende configuratietaken blijven. Als u uw aangepaste domeinnaam met Office 365, Intune of andere services die afhankelijk van aangepaste domeinnamen in Azure AD gebruiken zijn, Raadpleeg de documentatie voor deze services.

## <a name="delete-a-custom-domain-name"></a>Verwijderen van een aangepaste domeinnaam
Als uw organisatie die domeinnaam niet langer gebruikt, of als u wilt gebruiken die domeinnaam met een andere Azure AD, kunt u een aangepaste domeinnaam van uw Azure AD verwijderen.

Als u wilt verwijderen van een aangepaste domeinnaam, moet u eerst controleren of geen resources in uw directory zijn afhankelijk van de domeinnaam. U kunt een domeinnaam niet verwijderen uit de map als:

* Elke gebruiker heeft een gebruikersnaam, het e-mailadres of de proxy-adres dat de domeinnaam bevat.
* Een groep heeft een e-mailadres of de proxy-adres dat de domeinnaam bevat.
* Elke toepassing in uw Azure AD heeft een app ID URI die de domeinnaam bevat.

U moet wijzigen of verwijderen van een dergelijke resource in uw Azure AD-directory voordat u de aangepaste domeinnaam kunt verwijderen.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Gebruik PowerShell of Graph API voor het beheren van domeinnamen
De meeste beheertaken voor domeinnamen in Azure Active Directory kunnen ook worden uitgevoerd met behulp van Microsoft PowerShell of programmatisch met behulp van Azure AD Graph API.

* [Met behulp van PowerShell voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Domeinnamen in Azure AD beheren met behulp van Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Volgende stappen
* [Aangepaste domeinnamen toevoegen](add-custom-domain.md)


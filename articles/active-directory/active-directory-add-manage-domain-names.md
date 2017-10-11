---
title: Aangepaste domeinnamen in uw Azure Active Directory beheren | Microsoft Docs
description: Beheerconcepten en uitleg over het beheren van een aangepast domein in Azure Active Directory
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cf3523bd-9ee0-439e-963d-ccea038867b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
robots: NOINDEX
ms.openlocfilehash: 5ae19bb370064de96cf466ca09b13d02563d65a4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="managing-custom-domain-names-in-your-azure-active-directory"></a>Aangepaste domeinnamen in uw Azure Active Directory beheren
Een domeinnaam zijn een belangrijk id voor veel directoryresources als onderdeel van:

* Een gebruikersnaam of e-mailadres voor een gebruiker
* Het adres voor een groep
* De app-ID-URI voor een toepassing

Een resource in Azure Active Directory (Azure AD), kan een domeinnaam die al is gecontroleerd en eigendom zijn van de map waarin de resource bevatten. Alleen een globale beheerder kunt domein-beheertaken uitvoeren in Azure AD.

> [!IMPORTANT]
> Microsoft raadt u aan Azure AD te beheren met het [Azure AD-beheercentrum](https://aad.portal.azure.com) in Azure Portal in plaats van de klassieke Azure portal waarnaar in dit artikel wordt verwezen. Voor informatie over het beheren van uw domeinnamen in de Azure AD-beheercentrum, Zie [aangepaste domeinnamen in uw Azure Active Directory beheren](active-directory-domains-manage-azure-portal.md).

## <a name="set-the-primary-domain-name-for-your-azure-ad-directory"></a>De naam van het primaire domein voor uw Azure AD-directory instellen
Wanneer de map wordt gemaakt, wordt de initiële domeinnaam, zoals 'contoso.onmicrosoft.com', is ook de primaire domeinnaam voor uw directory. Het primaire domein is de standaard-domeinnaam voor een nieuwe gebruiker bij het maken van een nieuwe gebruiker in de [klassieke Azure-portal](https://manage.windowsazure.com/), of andere portals zoals Office 365-beheerportal. Dit stroomlijnt het proces voor de beheerder nieuwe gebruikers maken in de portal.

De naam van de primaire domeincontroller voor uw directory wijzigen:

1. Meld u aan bij de [klassieke Azure Portal](https://manage.windowsazure.com/) met een gebruikersaccount met de rechten voor globale beheerder van uw Azure AD-directory.
2. Selecteer **Active Directory** op de linkernavigatiebalk.
3. Open uw directory.
4. Selecteer de **domeinen** tabblad.
5. Selecteer de **primaire wijziging** knop op de opdrachtbalk.
6. Selecteer het domein dat u wilt worden van het nieuwe primaire domein voor uw directory.

U kunt de primaire domeinnaam voor uw directory om te worden geverifieerd aangepaste domeinen die niet federatief wijzigen. Het wijzigen van het primaire domein voor uw directory heeft geen invloed op de gebruikersnamen voor alle bestaande gebruikers.

## <a name="add-custom-domain-names-to-your-azure-ad"></a>Aangepaste domeinnamen toevoegen aan uw Azure AD
U kunt maximaal 900 aangepaste domeinnamen toevoegen aan elke Azure AD-directory. Het proces voor het [een aanvullende aangepaste domeinnaam toevoegen](active-directory-add-domain.md) is hetzelfde voor de eerste aangepaste domeinnaam.

## <a name="add-subdomains-of-a-custom-domain"></a>Toevoegen van subdomeinen van een aangepast domein
Als u wilt een derde niveau domeinnaam zoals 'europe.contoso.com' toevoegen aan uw directory, moet u eerst toevoegen en controleren van het domein van de tweede niveau, zoals contoso.com. Het subdomein wordt automatisch geverifieerd door Azure AD. Overzicht van het subdomein dat u zojuist hebt toegevoegd is geverifieerd, vernieuw de pagina in de browser met een lijst met de domeinen in uw directory.

## <a name="what-to-do-if-you-change-the-dns-registrar-for-your-custom-domain-name"></a>Wat te doen als u de DNS-registratieservice voor uw aangepaste domeinnaam wijzigen
Als u de DNS-registratieservice voor uw aangepaste domeinnaam wijzigt, kunt u uw aangepaste domeinnaam gebruiken met Azure AD zelf zonder onderbreking en zonder aanvullende configuratietaken blijven. Als u uw aangepaste domeinnaam met Office 365, Intune of andere services die afhankelijk van aangepaste domeinnamen in Azure AD gebruiken zijn, Raadpleeg de documentatie voor deze services.

## <a name="delete-a-custom-domain-name"></a>Verwijderen van een aangepaste domeinnaam
Als uw organisatie die domeinnaam niet langer gebruikt, of als u wilt gebruiken die domeinnaam met een andere Azure AD, kunt u een aangepaste domeinnaam van uw Azure AD verwijderen.

Als u wilt verwijderen van een aangepaste domeinnaam, moet u eerst controleren of geen resources in uw directory zijn afhankelijk van de domeinnaam. U kunt een domeinnaam niet verwijderen uit de map als:

* Elke gebruiker heeft een gebruikersnaam, het e-mailadres of de proxy-adres dat de domeinnaam omvatten.
* Een groep heeft een e-mailadres of de proxy-adres dat de domeinnaam bevat.
* Elke toepassing in uw Azure AD heeft een app ID URI die de domeinnaam bevat.

U moet wijzigen of verwijderen van een dergelijke resource in uw Azure AD-directory voordat u de aangepaste domeinnaam kunt verwijderen.

## <a name="use-powershell-or-graph-api-to-manage-domain-names"></a>Gebruik PowerShell of Graph API voor het beheren van domeinnamen
De meeste beheertaken voor domeinnamen in Azure Active Directory kunnen ook worden uitgevoerd met behulp van Microsoft PowerShell of programmatisch met behulp van Azure AD Graph API.

* [Met behulp van PowerShell voor het beheren van domeinnamen in Azure AD](https://msdn.microsoft.com/library/azure/e1ef403f-3347-4409-8f46-d72dafa116e0#BKMK_ManageDomains)
* [Domeinnamen in Azure AD beheren met behulp van Graph API](https://msdn.microsoft.com/Library/Azure/Ad/Graph/api/domains-operations)

## <a name="next-steps"></a>Volgende stappen
* [Meer informatie over domeinnamen in Azure AD](active-directory-add-domain-concepts.md)
* [Aangepaste domeinnamen beheren](active-directory-add-manage-domain-names.md)


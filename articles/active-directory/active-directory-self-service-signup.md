---
title: Selfservice of zeer snelle registratie in Azure Active Directory | Microsoft Docs
description: Gebruik van de toepassing met selfserviceregistratie in een tenant van Azure Active Directory (Azure AD)
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/03/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 2b41bb1b72cc773c29d464228c3177fbd1d9f5e0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="what-is-self-service-signup-for-azure-active-directory"></a>Wat is een toepassing met selfserviceregistratie voor Azure Active Directory?
Dit artikel wordt uitgelegd toepassing met selfserviceregistratie en hoe u voor de ondersteuning in Azure Active Directory (Azure AD). Als u wilt een domeinnaam overnemen van een niet-beheerde Azure AD-tenant, Zie [overnemen van een niet-beheerde directory als administrator](domains-admin-takeover.md).

## <a name="why-use-self-service-signup"></a>Waarom toepassing met selfserviceregistratie gebruiken?
* Klanten krijgen met services die ze sneller willen
* Op basis van e-mail aanbiedingen voor een service maken
* Maken van de aanmelding op basis van e-stromen die snel gebruikers kunnen zich met hun werk gemakkelijk te onthouden e-aliassen identiteiten maken
* Een zelfstandige-verkeer gemaakte Azure AD-directory kan worden omgezet in een beheerde map die kan worden gebruikt voor andere services

## <a name="terms-and-definitions"></a>Termen en definities
* **Toepassing met selfserviceregistratie**: dit is de methode waarmee een gebruiker zich aanmeldt voor een cloudservice en heeft een identiteit die automatisch voor hen zijn gemaakt in Azure AD op basis van hun e-maildomein.
* **Azure AD-directory zonder begeleiding**: dit is de directory waar die identiteit is gemaakt. Een niet-beheerde map is een map die geen globale beheerder heeft.
* **E-mailbericht geverifieerde gebruiker**: dit is een type gebruikersaccount in Azure AD. Een gebruiker met een automatisch gemaakt na het aanmelden voor een aanbieding selfservice identiteit staat bekend als een gebruiker met e-mailadres gecontroleerd. Een e-mailbericht geverifieerde gebruiker reguliere lid is van een map die is gemarkeerd met creationmethod = EmailVerified.

## <a name="how-do-i-control-self-service-settings"></a>Hoe bepaal ik selfservice-instellingen
Beheerders hebben twee selfservice besturingselementen vandaag. Ze kunnen bepalen of:

* Gebruikers kunnen lid worden van de map via e-mail.
* Gebruikers kunnen zichzelf licentie voor toepassingen en services.

### <a name="how-can-i-control-these-capabilities"></a>Hoe kan ik deze mogelijkheden bepalen?
Een beheerder kan deze mogelijkheden met de volgende Azure AD-cmdlet Set-MsolCompanySettings-parameters kunt configureren:

* **AllowEmailVerifiedUsers** bepaalt of een gebruiker kunt maken of koppelen van een niet-beheerde directory. Als u deze parameter op $false instelt, kunnen geen e-mailbericht geverifieerde gebruikers lid worden van de map.
* **AllowAdHocSubscriptions** Hiermee bepaalt u de mogelijkheid voor gebruikers om uit te voeren van de toepassing met selfserviceregistratie. Als u deze parameter op $false instelt, kunnen geen gebruikers de toepassing met selfserviceregistratie uitvoeren.

### <a name="how-do-the-controls-work-together"></a>Hoe werken de besturingselementen samen?
Deze twee parameters kunnen worden gebruikt in combinatie meer controle over de toepassing met selfserviceregistratie definiëren. Bijvoorbeeld de volgende opdracht, kunnen gebruikers toepassing met selfserviceregistratie, maar alleen uitvoeren als deze gebruikers al een account in Azure AD hebben (met andere woorden, gebruikers die een e-mailadres gecontroleerd account moet worden gemaakt moet eerst niet uitvoeren toepassing met selfserviceregistratie):

````
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
````
Het volgende stroomdiagram beschrijft de verschillende combinaties voor deze parameters en de resulterende voorwaarden voor de map en de toepassing met selfserviceregistratie.

![][1]

Zie voor meer informatie over en voorbeelden van het gebruik van deze parameters, [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Volgende stappen
* [Een aangepaste domeinnaam toevoegen aan Azure AD](add-custom-domain.md)
* [Azure PowerShell installeren en configureren](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-cmdlet-naslaginformatie](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png

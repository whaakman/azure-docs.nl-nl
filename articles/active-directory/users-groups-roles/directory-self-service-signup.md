---
title: Self-serviceregistratie voor via e-mail geverifieerde gebruikersaccounts - Azure Active Directory | Microsoft Docs
description: Aanmelding via selfservice gebruiken in een tenant Azure Active Directory (Azure AD)
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.subservice: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 03/18/2019
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d949b746f05eb440f5ae28f683dfc838217ab47
ms.sourcegitcommit: 24fd3f9de6c73b01b0cee3bcd587c267898cbbee
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/20/2019
ms.locfileid: "65956508"
---
# <a name="what-is-self-service-sign-up-for-azure-active-directory"></a>Wat is selfservice Meld u aan voor Azure Active Directory?

In dit artikel wordt uitgelegd hoe u self-service-aanmelding gebruiken voor het vullen van een organisatie in Azure Active Directory (Azure AD). Als u wilt een domeinnaam overnemen van een niet-beheerde Azure AD-organisatie, Zie [overnemen van een niet-beheerde adreslijst als administrator](domains-admin-takeover.md).

## <a name="why-use-self-service-sign-up"></a>Waarom self-service-aanmelding gebruiken?
* Breng uw klanten naar services die ze sneller willen
* Aanbiedingen op basis van een e-mailadres voor een service maken
* E-mailadres op basis van aanmelding stromen maken waarmee snel gebruikers te maken met behulp van hun e-mailadres werk gemakkelijk te onthouden aliassen identiteiten
* Een zelf-Self-service gemaakte Azure AD-directory kan worden omgezet in een beheerde adreslijst die kan worden gebruikt voor andere services

## <a name="terms-and-definitions"></a>Termen en definities
* **Self-service-aanmelding**: Dit is de methode waarmee een gebruiker zich aanmeldt voor een service in de cloud en heeft een identiteit die automatisch voor ze zijn gemaakt in Azure AD op basis van hun e-maildomein.
* **Niet-beheerde Azure AD-directory**: Dit is de map waar deze identiteit is gemaakt. Een niet-beheerde adreslijst is een map die geen globale beheerder heeft.
* **Via e-mail geverifieerde gebruiker**: Dit is een type gebruikersaccount in Azure AD. Een gebruiker met een automatisch gemaakt nadat het aanmelden voor een aanbieding selfservice staat bekend als een gebruiker via e-mail geverifieerde identiteit. Een gebruiker via e-mail geverifieerde reguliere lid is van een map die is gemarkeerd met creationmethod = EmailVerified.

## <a name="how-do-i-control-self-service-settings"></a>Hoe beheers ik selfservice-instellingen
Beheerders hebben nu twee besturingselementen voor selfservice. Ze kunnen bepalen of:

* Gebruikers kunnen deelnemen aan de directory via e-mail
* Gebruikers kunnen zelf een licentie voor toepassingen en services

### <a name="how-can-i-control-these-capabilities"></a>Hoe kan ik deze functies bepalen?
Een beheerder kan deze mogelijkheden met de volgende parameters van de Azure AD-cmdlet Set-MsolCompanySettings configureren:

* **AllowEmailVerifiedUsers** bepaalt of een gebruiker kunt maken of lid worden van een map. Als u deze parameter op $false instelt, kan er geen gebruiker via e-mail geverifieerde Neem deel aan de directory.
* **AllowAdHocSubscriptions** Hiermee bepaalt u de mogelijkheid voor gebruikers om uit te voeren van de self-service-aanmelding. Als u deze parameter op $false instelt, kan er geen gebruiker self-service-aanmelding kunt uitvoeren.
  
AllowEmailVerifiedUsers en AllowAdHocSubscriptions zijn hele adreslijst-instellingen die kunnen worden toegepast op een beheerde of onbeheerde map. Hier volgt een voorbeeld waarin:

* Beheren van een map met een geverifieerde domeinnaam bijvoorbeeld contoso.com
* U B2B-samenwerking vanuit een andere map gebruiken om uit te nodigen van een gebruiker die nog niet bestaat (userdoesnotexist@contoso.com) in de basismap van contoso.com
* De basismap heeft de AllowEmailVerifiedUsers ingeschakeld

Als de vorige voorwaarden voldaan wordt, klikt u vervolgens een gebruiker lid wordt gemaakt in de basismap en een gastgebruiker B2B is gemaakt in de uitnodigende map.

Flow en PowerApps proefversie registraties niet wordt beheerd door de **AllowAdHocSubscriptions** instelling. Raadpleeg voor meer informatie de volgende artikelen:

* [Hoe kan ik voorkomen dat mijn bestaande gebruikers Power BI gaan gebruiken?](https://support.office.com/article/Power-BI-in-your-Organization-d7941332-8aec-4e5e-87e8-92073ce73dc5#bkmk_preventjoining)
* [Flow in uw organisatie Q & A](https://docs.microsoft.com/flow/organization-q-and-a)

### <a name="how-do-the-controls-work-together"></a>Hoe de besturingselementen werken samen?
Deze twee parameters kunnen in combinatie worden gebruikt voor het definiëren van meer nauwkeurige controle over self-service-aanmelding. Bijvoorbeeld, de volgende opdracht uit, kunnen gebruikers om uit te voeren van selfservice aanmelding, maar alleen als deze gebruikers hebben al een account in Azure AD (met andere woorden, gebruikers die een worden gemaakt via e-mail geverifieerde account moet eerst niet uitvoeren self-service-aanmelding):

```powershell
    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true
```

Het volgende stroomdiagram wordt uitgelegd voor de verschillende combinaties van deze parameters en de resulterende voorwaarden voor de directory en de self-service-aanmelding.

![Stroomdiagram van selfservice aanmelding besturingselementen](./media/directory-self-service-signup/SelfServiceSignUpControls.png)

Zie voor meer informatie en voorbeelden van het gebruik van deze parameters [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="next-steps"></a>Volgende stappen

* [Een aangepaste domeinnaam toevoegen aan Azure AD](../fundamentals/add-custom-domain.md)
* [Azure PowerShell installeren en configureren](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-cmdlet-naslaginformatie](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)
* [Sluit uw werk of school-account in een niet-beheerde adreslijst](users-close-account.md)

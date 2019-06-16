---
title: Bedrijf-werkaccounts en Partner Center
description: Hoe om te controleren of uw bedrijf een werkaccount heeft instellen met Microsoft, maak een nieuw werkaccount of instellen van meerdere werkaccounts gebruiken met de Partner Center.
author: mattwojo
manager: evansma
ms.author: parthp
ms.service: marketplace
ms.topic: how-to
ms.date: 05/30/2019
ms.openlocfilehash: df8ab370e8874e07f8985ecfb3a772848a2e2b21
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65806279"
---
# <a name="company-work-accounts-and-partner-center"></a>Bedrijf-werkaccounts en Partner Center

Partner Center maakt gebruik van werkaccounts bedrijf, ook wel bekend als Azure Active Directory (AD)-tenants, accounttoegang voor meerdere gebruikers, machtigingen voor beheer, hostgroepen en toepassingen beheren en onderhouden van de profielgegevens. Domein account werk-e-mailadres van uw bedrijf koppelt aan uw Partner Center-account, kunnen werknemers van uw bedrijf aanmelden bij Partner Center voor het beheren van marketplace-aanbiedingen met behulp van hun eigen werk gebruikersnamen en wachtwoorden.

## <a name="check-whether-your-company-already-has-a-work-account"></a>Controleer of uw bedrijf al een werkaccount heeft

Als uw bedrijf is geabonneerd op een Microsoft-cloudservice zoals Azure, Microsoft Intune of Office 365, hebt u al een e-mailbericht account werkdomein (ook wel een Azure Active Directory-tenant genoemd) die kan worden gebruikt met de Partner Center.

Volg deze stappen om te controleren:
1. Aanmelden bij de Azure-beheerportal bij https://portal.azure.com.
2. Selecteer **Azure Active Directory** vanuit het menu van de navigatie aan de linkerkant en selecteer vervolgens **aangepaste-domeinnamen**.
3. Als u al een werkaccount hebt, kunt u de domeinnaam van uw wordt weergegeven.

Als uw bedrijf niet al een werkaccount, wordt een voor u gemaakt tijdens het inschrijvingsproces Partner Center.

## <a name="set-up-multiple-work-accounts"></a>Instellen van meerdere werkaccounts

Voordat u besluit om een bestaande werkaccount te gebruiken, houd rekening met het aantal gebruikers in het werkaccount moet toegang tot Partner Center. Als u gebruikers in het werkaccount dat die hoeft niet te krijgen tot Partner Center hebt, kunt u rekening houden met het maken van meerdere werkaccounts, zodat alleen gebruikers die toegang tot Partner Center moet worden weergegeven op een bepaald account.

## <a name="create-a-new-work-account"></a>Een nieuw werkaccount maken

Als u wilt een nieuw werkaccount maken voor uw bedrijf, de volgende stappen uit te voeren. Mogelijk moet u om hulp vragen van degene die beheerdersmachtigingen op de Microsoft Azure-account van uw bedrijf heeft.

1. Meld u aan bij de [Microsoft Azure Portal](https://portal.azure.com).
2. Selecteer in het navigatiemenu links de **Azure Active Directory** -> **gebruikers**.
3. Selecteer **nieuwe gebruiker** en maak een nieuwe Azure-werkaccount door in te voeren van een naam en e-mailadres. Zorg ervoor dat de **maprol** is ingesteld op **gebruiker** en selecteer de **wachtwoord weergeven** selectievakje onderaan om te bekijken en noteer het wachtwoord van de automatisch gegenereerde.
4. Selecteer **maken** om op te slaan van de nieuwe gebruiker.

Het e-mailadres voor het gebruikersaccount moet een geverifieerde domeinnaam op in uw directory. U kunt alle geverifieerde domeinen in uw directory weergeven door te selecteren **Azure Active Directory** -> **aangepaste-domeinnamen** in het menu van de navigatie aan de linkerkant.

Zie voor meer informatie over het toevoegen van aangepaste domeinen in Azure Active Directory, [toevoegen of koppelen aan een domein in Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-add-domain).

## <a name="troubleshoot-work-email-sign-in"></a>Werk e-mailadres aanmelden oplossen

Als u problemen ondervindt bij het aanmelden bij uw werkaccount (ook wel bekend als uw Azure AD-tenant), zoek het scenario in het diagram hieronder die het meest overeenkomt met uw situatie en volg de aanbevolen stappen.

![Diagram voor het oplossen van aanmelding voor work-account](./media/onboarding-aad-flow.png)

## <a name="next-steps"></a>Volgende stappen

- [Uw commerciële Marketplace-account in de Partner Center beheren](./manage-account.md) 

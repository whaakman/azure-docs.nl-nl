---
title: Opnieuw activeren uitgeschakelde naamruimten van de Azure Access controleservice (ACS)
description: Meer informatie over het zoeken en inschakelen van uw Azure Access Control Service (ACS)-naamruimten en aanvragen van een uitbreiding van bewaring ingeschakeld tot en met 4 februari 2019.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.component: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 11/07/2018
ms.author: celested
ms.reviewer: jlu
ms.custom: aaddev
ms.openlocfilehash: 0f0de122dc3dbd770e91a8412430423bee222b30
ms.sourcegitcommit: 0fc99ab4fbc6922064fc27d64161be6072896b21
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51577943"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Hoe: uitgeschakeld Access Control Service naamruimten opnieuw activeren

In November 2017, hebben we aangekondigd dat Microsoft Azure Access Control Service (ACS), een service van Azure Active Directory (Azure AD), wordt stopgezet op 7 November 2018.

Aangezien vervolgens e-mailberichten naar de ACS-abonnementen e-mailadres beheerder over de ACS buiten gebruik stellen 12 maanden, 9 maanden, 6 maanden, drie maanden, 1 maand, 2 weken, 1 week, en 1 dag vóór de vervaldatum van 7 November 2018 is verzonden.

Op 3 oktober 2018, hebben we aangekondigd (via e-mail en [een blogbericht](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) een extensie-aanbieding voor klanten die hun worden gemigreerd om de 7 November 2018 kan niet worden voltooid. De aankondiging had ook instructies voor het aanvragen van de extensie.

## <a name="why-your-namespace-is-disabled"></a>Waarom uw naamruimte is uitgeschakeld

Als u dit nog niet hebt aangemeld voor de extensie, gaan we het uitschakelen van ACS-naamruimten vanaf 7 November 2018. Als u de communicatie gemist en er nog opt-in voor de uitbreiding voor 4 februari 2019, volgt u de instructies in de volgende secties.

> [!NOTE]
> U moet een servicebeheerder of medebeheerder van het abonnement voor het uitvoeren van de PowerShell-opdrachten en verlengen.

## <a name="find-and-enable-your-acs-namespaces"></a>Zoeken en inschakelen van de ACS-naamruimten

U kunt PowerShell ACS lijst met alle ACS-naamruimten en opnieuw activeren die zijn uitgeschakeld.

1. Downloaden en installeren van ACS PowerShell:
    1. Ga naar de PowerShell Gallery en download [Acs.Namespaces](https://www.powershellgallery.com/packages/Acs.Namespaces/1.0.2).
    1. De module installeren:

        ```powershell
        Install-Module -Name Acs.Namespaces
        ```

    1. Haal een lijst van alle mogelijke opdrachten:

        ```powershell
        Get-Command -Module Acs.Namespaces
        ```

        Hulp op een specifieke opdracht uitvoeren:

        ```
        Get-Help [Command-Name] -Full
        ```
    
        waar `[Command-Name]` is de naam van de ACS-opdracht.
1. Verbinding maken met ACS met behulp van de **Connect AcsAccount** cmdlet. 

    U moet mogelijk uw uitvoeringsbeleid wijzigen door te voeren **Set-ExecutionPolicy** voordat u de opdracht kunt uitvoeren.
1. Lijst van uw beschikbare Azure-abonnementen met behulp van de **Get-AcsSubscription** cmdlet.
1. Lijst met uw ACS-naamruimten op met de **Get-AcsNamespace** cmdlet.
1. Bevestig dat de naamruimten zijn uitgeschakeld door te bevestigen dat `State` is `Disabled`.

    [![Bevestig dat de naamruimten zijn uitgeschakeld](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png)](./media/howto-reactivate-disabled-acs-namespaces/confirm-disabled-namespace.png#lightbox)

    U kunt ook `nslookup {your-namespace}.accesscontrol.windows.net` om te controleren of het domein nog steeds actief is.

1. Schakel uw ACS namespace(s) met de **inschakelen AcsNamespace** cmdlet.

    Nadat u uw namespace(s) hebt ingeschakeld, kunt u een uitbreiding aanvragen, zodat de namespace(s) wordt niet zijn uitgeschakeld opnieuw voordat u 4 februari 2019. Na die datum mislukken alle aanvragen voor ACS.

## <a name="request-an-extension"></a>Een verlenging van aanvragen

1. Navigeer naar de ACS-naamruimte-beheerportal door te gaan naar `https://{your-namespace}.accesscontrol.windows.net`.
1. Selecteer de **voorwaarden lezen** knop lezen de [bijgewerkt gebruiksvoorwaarden](https://azure.microsoft.com/support/legal/access-control/), die wordt u doorgeleid naar een pagina met de bijgewerkte gebruiksvoorwaarden.

    [![Selecteer de knop voorwaarden lezen](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Selecteer **aanvragen extensie** op de banner boven aan de pagina. De knop wordt alleen worden ingeschakeld nadat u de [gebruiksvoorwaarden bijgewerkt](https://azure.microsoft.com/support/legal/access-control/).

    [![Selecteer de knop Request-extensie](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. Nadat de aanvraag voor de domeinnaamextensie is geregistreerd, wordt de pagina wordt vernieuwd met een nieuwe banner boven aan de pagina.

    [![Bijgewerkte pagina met vernieuwd banner](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)

## <a name="help-and-support"></a>Help en ondersteuning

- Als u problemen ondervindt na het volgen van deze instructies, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Hebt u vragen of feedback over ACS buiten gebruik stellen, contact met ons op acsfeedback@microsoft.com.

## <a name="next-steps"></a>Volgende stappen

- Lees de informatie over ACS buiten gebruik stellen in [hoe: migreren van de Azure Access Control Service](active-directory-acs-migration.md).

---
title: Opnieuw activeren uitgeschakelde naamruimten van de Azure Access controleservice (ACS)
description: Meer informatie over het zoeken en inschakelen van uw Azure Access Control Service (ACS)-naamruimten en aanvragen van een uitbreiding van bewaring ingeschakeld tot en met 4 februari 2019.
services: active-directory
documentationcenter: ''
author: CelesteDG
manager: mtillman
ms.service: active-directory
ms.subservice: develop
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/21/2019
ms.author: celested
ms.reviewer: jlu
ms.custom: aaddev
ms.collection: M365-identity-device-management
ms.openlocfilehash: d0078c260e09ec9fd31ecf2124bc4bf7ad7f92b0
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56173739"
---
# <a name="how-to-reactivate-disabled-access-control-service-namespaces"></a>Procedure: Uitgeschakelde naamruimten van de Access Control Service opnieuw activeren

In November 2017, hebben we aangekondigd dat Microsoft Azure Access Control Service (ACS), een service van Azure Active Directory (Azure AD), wordt stopgezet op 7 November 2018.

Aangezien vervolgens e-mailberichten naar de ACS-abonnementen e-mailadres beheerder over de ACS buiten gebruik stellen 12 maanden, 9 maanden, 6 maanden, drie maanden, 1 maand, 2 weken, 1 week, en 1 dag vóór de vervaldatum van 7 November 2018 is verzonden.

Op 3 oktober 2018, hebben we aangekondigd (via e-mail en [een blogbericht](https://azure.microsoft.com/blog/one-month-retirement-notice-access-control-service/)) een extensie-aanbieding voor klanten die hun worden gemigreerd om de 7 November 2018 kan niet worden voltooid. De aankondiging had ook instructies voor het aanvragen van de extensie.

## <a name="why-your-namespace-is-disabled"></a>Waarom uw naamruimte is uitgeschakeld

Als u dit nog niet hebt aangemeld voor de extensie, gaan we het uitschakelen van ACS-naamruimten vanaf 7 November 2018. U moet hebt aangevraagd de extensie voor 4 februari 2019 al; anders niet mogelijk om in te schakelen van de naamruimten via PowerShell.

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

        ```powershell
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

We nemen nieuwe extensieaanvragen vanaf 21 januari 2019.

Gaan we het uitschakelen van naamruimten voor klanten die uitbreidingen voor 4 februari 2019 hebt aangevraagd. U kunt nog steeds naamruimten via PowerShell weer inschakelen, maar de naamruimten wordt uitgeschakeld na 48 uur opnieuw.

Na 4 maart 2019 wordt klanten niet langer opnieuw inschakelen van alle naamruimten via PowerShell.

Verder wordt niet meer automatisch worden goedgekeurd. Als u meer tijd om te migreren, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/#create/Microsoft.Support) voor een gedetailleerde migratie-tijdlijn.

### <a name="to-request-an-extension"></a>Om aan te vragen van een uitbreiding

1. Meld u aan bij de Azure-portal en maak een [nieuwe ondersteuningsaanvraag](https://portal.azure.com/#create/Microsoft.Support).
1. Vul in nieuw aanvraagformulier voor de ondersteuning, zoals weergegeven in het volgende voorbeeld.

    | Ondersteuning voor het veld | Value |
    |-----------------------|--------------------|
    | **Probleemtype** | `Technical` |
    | **Abonnement** | Ingesteld op uw abonnement |
    | **Service** | `All services` |
    | **Resource** | `General question/Resource not available` |
    | **Probleemtype** | `ACS to SAS Migration` |
    | **Onderwerp** | Het probleem beschrijven |

  ![Nieuwe aanvraag voor technische ondersteuning](./media/howto-reactivate-disabled-acs-namespaces/new-technical-support-request.png)

<!--

1. Navigate to your ACS namespace's management portal by going to `https://{your-namespace}.accesscontrol.windows.net`.
1. Select the **Read Terms** button to read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/), which will direct you to a page with the updated Terms of Use.

    [![Select the Read Terms button](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/read-terms-button-expanded.png#lightbox)

1. Select **Request Extension** on the banner at the top of the page. The button will only be enabled after you read the [updated Terms of Use](https://azure.microsoft.com/support/legal/access-control/).

    [![Select the Request Extension button](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/request-extension-button-expanded.png#lightbox)

1. After the extension request is registered, the page will refresh with a new banner at the top of the page.

    [![Updated page with refreshed banner](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png)](./media/howto-reactivate-disabled-acs-namespaces/updated-banner-expanded.png#lightbox)
-->

## <a name="help-and-support"></a>Help en ondersteuning

- Als u problemen ondervindt na het volgen van deze instructies, neem dan contact op met [ondersteuning van Azure](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview).
- Hebt u vragen of feedback over ACS buiten gebruik stellen, contact met ons op acsfeedback@microsoft.com.

## <a name="next-steps"></a>Volgende stappen

- Lees de informatie over ACS buiten gebruik stellen in [het: Migreren van de Azure Access controleservice](active-directory-acs-migration.md).

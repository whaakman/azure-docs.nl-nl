---
title: Azure Stack-validatie als een Service opmerkingen bij de release | Microsoft Docs
description: Azure Stack-validatie als een Service opmerkingen bij de release.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/11/2019
ms.author: mabrigg
ms.reviewer: johnhas
ms.lastreviewed: 03/11/2019
ms.openlocfilehash: 3cda9d487d9548251c68d61db51bf2fb826b6f79
ms.sourcegitcommit: 5fbca3354f47d936e46582e76ff49b77a989f299
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/12/2019
ms.locfileid: "57775285"
---
# <a name="release-notes-for-validation-as-a-service"></a>Opmerkingen bij de release voor validatie als een Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

In dit artikel heeft de opmerkingen bij de release voor Azure Stack-validatie als een Service.

## <a name="version-405"></a>Versie 4.0.5

2019 17 januari

- Test van schijf-ID bijgewerkt naar adres storage pool inconsistenties. Versie: 5.1.14.0 -> 5.1.15.0
- Azure Stack maandelijkse Update verificatie bijgewerkt naar adres goedgekeurde software en inhoud validatie inconsistenties. Versie: 5.1.14.0 -> 5.1.17.0
- OEM-extensie pakket verificatie bijgewerkt en voer de vereiste controles vóór de stap van de update Azure Stack. Versie: 5.1.14.0 -> 5.1.16.0
- Interne oplossingen voor problemen

## <a name="version-402"></a>Versie 4.0.2

2019 januari 7

Als u de werkstroom van de Azure Stack maandelijkse Update verificatie uitvoert en de versie voor uw OEM-updatepakket niet 1810 of hoger is, wordt een foutbericht worden weergegeven wanneer u naar de OEM-update-stap. Dit is een fout. Een oplossing is ontwikkeld. De stappen zijn als volgt:

1.  De OEM-update worden uitgevoerd als normaal.
2.  Test-AzureStack uitvoeren na geslaagde toepassing van het pakket en sla de uitvoer.
3.  Annuleren van de test.
4.  Verzenden van de opgeslagen uitvoer naar VaaSHelp@microsoft.com om resultaten te geven voor de uitvoering te ontvangen.

## <a name="version-402"></a>Versie 4.0.2

En met 30 November 2018

- Interne oplossingen voor problemen

## <a name="version-401"></a>Versie 4.0.1

8 oktober 2018

- VaaS vereisten

    `Install-VaaSPrerequisites` vereist niet langer cloud-beheerdersreferenties. Als u de meest recente versie van deze cmdlet uitvoert, Zie [Download en installeer de agent](azure-stack-vaas-local-agent.md#download-and-install-the-agent) voor de nieuwe opdrachten voor het installeren van vereisten. Dit zijn de opdrachten:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region
    ```

## <a name="version-400"></a>Versie 4.0.0

29 augustus 2018

- VaaS vereisten en de VHD-updates

    `Install-VaaSPrerequisites` nu vereist dat cloud-beheerdersreferenties voor het oplossen van een probleem tijdens de validatie van het pakket. De documentatie op [Download en installeer de agent](azure-stack-vaas-local-agent.md#download-and-install-the-agent) is bijgewerkt met het volgende:

    ```PowerShell
    $ServiceAdminCreds = New-Object System.Management.Automation.PSCredential "<aadServiceAdminUser>", (ConvertTo-SecureString "<aadServiceAdminPassword>" -AsPlainText -Force)
    $CloudAdminCreds = New-Object System.Management.Automation.PSCredential "<cloudAdminDomain\username>", (ConvertTo-SecureString "<cloudAdminPassword>" -AsPlainText -Force)
    Import-Module .\VaaSPreReqs.psm1 -Force
    Install-VaaSPrerequisites -AadTenantId $AadTenantId `
                              -ServiceAdminCreds $ServiceAdminCreds `
                              -ArmEndpoint https://adminmanagement.$ExternalFqdn `
                              -Region $Region `
                              -CloudAdminCredentials $CloudAdminCreds
    ```
    > [!NOTE]
    > De `$CloudAdminCreds` vereist door het script zijn voor de Azure Stack exemplaar worden gevalideerd. Ze zijn niet de Azure Active Directory-referenties die worden gebruikt door de tenant VaaS.

- Lokale agent bijwerken

    De vorige versie van de lokale agent is niet compatibel met de huidige 4.0.0-versie van de service. Alle gebruikers moeten hun lokale agents bijwerken. Zie [Download en installeer de agent](azure-stack-vaas-local-agent.md#download-and-install-the-agent) voor instructies over het installeren van de nieuwste agent.

- PowerShell automatiseringsupdate

    Wijzigingen zijn aangebracht in `LaunchVaaSTests` PowerShell-scripts waarvoor de nieuwste versie van het uitvoeren van scripts pakketten. Zie [starten van de werkstroom testronde](azure-stack-vaas-automate-with-powershell.md#launch-the-test-pass-workflow) voor instructies over het installeren van de meest recente versie van het uitvoeren van scripts pakket.

- Validatie uit als een serviceportal

  - Pakket voor het ondertekenen van meldingen

    Als een OEM-aanpassing-pakket wordt verzonden als onderdeel van de werkstroom voor validatie van het pakket, wordt de indeling van het pakket worden gevalideerd om ervoor te zorgen dat deze de gepubliceerde specificatie volgt. Als het pakket niet voldoet, wordt de uitvoering mislukt. E-mailmeldingen worden worden verzonden naar het e-mailadres van de geregistreerde Azure Active Directory-contactpersoon voor de tenant.

  - Interactieve test categorie

    De **interactief** test categorie is toegevoegd. Deze tests oefenen interactieve, niet-geautomatiseerde Azure Stack-scenario's.

  - Functie voor interactieve verificatie

    De mogelijkheid gerichte om feedback te geven voor bepaalde functies is nu beschikbaar in de werkstroom testronde. De `OEM Update on Azure Stack 1806 RC Validation 5.1.4.0` test wordt gecontroleerd om te zien als specifieke updates correct zijn toegepast en vervolgens verzamelt feedback.

## <a name="next-steps"></a>Volgende stappen

- [Validatie as a Service oplossen](azure-stack-vaas-troubleshoot.md)

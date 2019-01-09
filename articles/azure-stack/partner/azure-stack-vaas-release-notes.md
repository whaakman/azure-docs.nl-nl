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
ms.date: 11/26/2018
ms.author: mabrigg
ms.reviewer: johnhas
ms.openlocfilehash: 687e47eff000679f24088a59297a258a1469f3fd
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54106270"
---
# <a name="release-notes-for-validation-as-a-service"></a>Opmerkingen bij de release voor validatie als een Service

[!INCLUDE [Azure_Stack_Partner](./includes/azure-stack-partner-appliesto.md)]

In dit artikel heeft de opmerkingen bij de release voor Azure Stack-validatie als een Service.

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

    `Install-VaaSPrerequisites` nu vereist dat cloud-beheerdersreferenties voor het oplossen van een probleem tijdens de validatie van de oplossing. De documentatie op [Download en installeer de agent](azure-stack-vaas-local-agent.md#download-and-install-the-agent) is bijgewerkt met het volgende:

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

    Als een OEM-aanpassing-pakket wordt verzonden als onderdeel van de werkstroom voor validatie van de oplossing, wordt de indeling van het pakket worden gevalideerd om ervoor te zorgen dat deze de gepubliceerde specificatie volgt. Als het pakket niet voldoet, wordt de uitvoering mislukt. E-mailmeldingen worden worden verzonden naar het e-mailadres van de geregistreerde Azure Active Directory-contactpersoon voor de tenant.

  - Interactieve test categorie

    De **interactief** test categorie is toegevoegd. Deze tests kunnen partners om interactieve, niet-geautomatiseerde Azure Stack-scenario's.

  - Functie voor interactieve verificatie

    De mogelijkheid gerichte om feedback te geven voor bepaalde functies is nu beschikbaar in de werkstroom testronde. De `OEM Update on Azure Stack 1806 RC Validation 5.1.4.0` test wordt gecontroleerd om te zien als specifieke updates correct zijn toegepast en vervolgens verzamelt feedback.

## <a name="next-steps"></a>Volgende stappen

- [Validatie as a Service oplossen](azure-stack-vaas-troubleshoot.md)
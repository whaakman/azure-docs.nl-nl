---
services: virtual-machines
title: Instellen van PowerShell
author: JoeDavies-MSFT
solutions: ''
manager: timlt
editor: tysonn
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 05/12/2015
ms.author: rasquill
ms.openlocfilehash: b96e8e6e31817f6d261f41dbf3b3047dd49c29ba
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258242"
---
## <a name="setting-up-powershell"></a>Instellen van PowerShell
Voordat u Azure PowerShell gebruiken kunt, volgt u deze stappen.

### <a name="verify-powershell-versions"></a>Controleer of versies van PowerShell
Voordat u Windows PowerShell gebruiken kunt, kunt u Windows PowerShell, versie 3.0 of 4.0 moet hebben. Als u wilt zien welke versie van Windows PowerShell, typ de volgende opdracht achter de opdrachtprompt van Windows PowerShell.

    $PSVersionTable

U ziet er ongeveer als volgt.

    Name                           Value
    ----                           -----
    PSVersion                      3.0
    WSManStackVersion              3.0
    SerializationVersion           1.1.0.1
    CLRVersion                     4.0.30319.18444
    BuildVersion                   6.2.9200.16481
    PSCompatibleVersions           {1.0, 2.0, 3.0}
    PSRemotingProtocolVersion      2.2

Controleer de waarde van **PSVersion** 3.0 of 4.0 is. Zie voor het installeren van een compatibele versie [Windows Management Framework 3.0](https://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](https://www.microsoft.com/download/details.aspx?id=40855).

U hebt ook Azure PowerShell versie 0.8.0 of hoger. U kunt controleren welke versie van Azure PowerShell die u hebt geïnstalleerd met de volgende opdracht bij de Azure PowerShell-opdrachtprompt.

    Get-Module azure | format-table version

U ziet er ongeveer als volgt.

    Version
    -------
    0.8.16.1

Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="set-your-azure-account-and-subscription"></a>Uw Azure-account en -abonnement instellen
Als u nog een Azure-abonnement hebt, kunt u uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) zich ook aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

Een Azure PowerShell-opdrachtprompt openen en aanmelden bij Azure met deze opdracht.

    Add-AzureAccount

Als u meerdere Azure-abonnementen hebt, kunt u uw Azure-abonnementen met deze opdracht weergeven.

    Get-AzureSubscription

U ontvangt het volgende type informatie:

    SubscriptionId            : fd22919d-eaca-4f2b-841a-e4ac6770g92e
    SubscriptionName          : Visual Studio Ultimate with MSDN
    Environment               : AzureCloud
    SupportedModes            : AzureServiceManagement,AzureResourceManager
    DefaultAccount            : johndoe@contoso.com
    Accounts                  : {johndoe@contoso.com}
    IsDefault                 : True
    IsCurrent                 : True
    CurrentStorageAccountName : 
    TenantId                  : 32fa88b4-86f1-419f-93ab-2d7ce016dba7

U kunt het huidige Azure-abonnement instellen door het uitvoeren van deze opdrachten bij de Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens in, met inbegrip van de < en > tekens, met de juiste naam.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current    

Zie voor meer informatie over Azure-abonnementen en accounts [hoe: verbinding maken met uw abonnement](/powershell/azureps-cmdlets-docs#Connect).


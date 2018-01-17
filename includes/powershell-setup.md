---
services: virtual-machines
title: Instellen van PowerShell
author: JoeDavies-MSFT
solutions: 
manager: timlt
editor: tysonn
ms.service: virtual-machines
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 05/12/2015
ms.author: rasquill
ms.openlocfilehash: 19c704d965ff3e2fc9ac8c5b623aeb386cb0b974
ms.sourcegitcommit: e19f6a1709b0fe0f898386118fbef858d430e19d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2018
---
## <a name="setting-up-powershell"></a>Instellen van PowerShell
Voordat u Azure PowerShell gebruiken kunt, volg deze stappen.

### <a name="verify-powershell-versions"></a>Controleer of de PowerShell-versies
Voordat u Windows PowerShell gebruiken kunt, moet u Windows PowerShell, versie 3.0 of 4.0 hebben. Ga voor de versie van Windows PowerShell, typ de volgende opdracht achter de opdrachtprompt van Windows PowerShell.

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

Controleer de waarde van **PSVersion** 3.0 of 4.0. Zie het installeren van een compatibele versie [Windows Management Framework 3.0](http://www.microsoft.com/download/details.aspx?id=34595) of [Windows Management Framework 4.0](http://www.microsoft.com/download/details.aspx?id=40855).

U hebt ook Azure PowerShell versie 0.8.0 of hoger. U kunt controleren de versie van Azure PowerShell die u hebt geïnstalleerd met deze opdracht bij de Azure PowerShell-opdrachtprompt.

    Get-Module azure | format-table version

U ziet er ongeveer als volgt.

    Version
    -------
    0.8.16.1

Zie voor instructies en een koppeling naar de nieuwste versie [installeren en configureren van Azure PowerShell](/powershell/azureps-cmdlets-docs).

### <a name="set-your-azure-account-and-subscription"></a>Uw Azure-account en -abonnement instellen
Als u nog geen Azure-abonnement hebt, kunt u activeren uw [voordelen als MSDN-abonnee](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) of zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).

Open een Azure PowerShell-opdrachtprompt en meld u aan bij Azure met deze opdracht.

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

U kunt de huidige Azure-abonnement instellen door het uitvoeren van deze opdrachten bij de Azure PowerShell-opdrachtprompt. Vervang alles binnen de aanhalingstekens, met inbegrip van de < en > tekens lang zijn en de juiste naam.

    $subscr="<SubscriptionName from the display of Get-AzureSubscription>"
    Select-AzureSubscription -SubscriptionName $subscr -Current    

Zie voor meer informatie over Azure-abonnementen en accounts [hoe: verbinding maken met uw abonnement](/powershell/azureps-cmdlets-docs#Connect).


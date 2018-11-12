---
title: Notification Hubs implementeren en beheren met PowerShell
description: Over het maken en beheren van Notification Hubs met behulp van PowerShell voor Automation
services: notification-hubs
documentationcenter: ''
author: dimazaid
manager: kpiteira
editor: spelluru
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 04/14/2018
ms.author: dimazaid
ms.openlocfilehash: 7107c4ebd3f7df5966dd4c66aa8f453bd4f3d1d6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51228348"
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Notification Hubs implementeren en beheren met PowerShell
## <a name="overview"></a>Overzicht
Dit artikel leest u hoe het gebruik van maken en beheren van Azure Notification Hubs met behulp van PowerShell. De volgende algemene automatiseringstaken worden weergegeven in dit artikel.

* Een Notification Hub maken
* Referenties instellen

Als u ook een nieuwe service bus-naamruimte voor uw notification hubs maken moet, raadpleegt u [Service Bus beheren met PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Notification Hubs beheren wordt rechtstreeks door de opgenomen met Azure PowerShell-cmdlets niet ondersteund. De aanbevolen aanpak van PowerShell is om te verwijzen naar de assembly Microsoft.Azure.NotificationHubs.dll. De assembly wordt geleverd bij de [Microsoft Azure Notification Hubs NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Azure is een platform op basis van een abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [Aankoopopties], [aanbiedingen voor leden], of [gratis proefversie].
* Een computer met Azure PowerShell. Zie voor instructies [installeren en configureren van Azure PowerShell].
* Een algemeen begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de .NET-assembly voor Service Bus
Beheren van Azure Notification Hubs is nog niet opgenomen in de PowerShell-cmdlets in Azure PowerShell. Voor het inrichten van notification hubs, kunt u de .NET-client die is opgegeven in de [Microsoft Azure Notification Hubs NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Eerst, zorg ervoor dat uw script vindt de **Microsoft.Azure.NotificationHubs.dll** assembly die is geïnstalleerd als een NuGet-pakket in een Visual Studio-project. Om flexibele, voert het script als volgt:

1. Bepaalt het pad waar het is aangeroepen.
2. Via het pad wordt verzonden, totdat er een map met de naam `packages`. Deze map wordt gemaakt tijdens de installatie van NuGet-pakketten voor Visual Studio-projecten.
3. Zoekopdrachten recursief de `packages` map voor een assembly met de naam **Microsoft.Azure.NotificationHubs.dll**.
4. Verwijst naar de assembly die de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen zijn geïmplementeerd in een PowerShell-script:

``` powershell

try
{
    # WARNING: Make sure to reference the latest version of Microsoft.Azure.NotificationHubs.dll
    Write-Output "Adding the [Microsoft.Azure.NotificationHubs.dll] assembly to the script..."
    $scriptPath = Split-Path (Get-Variable MyInvocation -Scope 0).Value.MyCommand.Path
    $packagesFolder = (Split-Path $scriptPath -Parent) + "\packages"
    $assembly = Get-ChildItem $packagesFolder -Include "Microsoft.Azure.NotificationHubs.dll" -Recurse
    Add-Type -Path $assembly.FullName

    Write-Output "The [Microsoft.Azure.NotificationHubs.dll] assembly has been successfully added to the script."
}

catch [System.Exception]
{
    Write-Error("Could not add the Microsoft.Azure.NotificationHubs.dll assembly to the script. Make sure you build the solution before running the provisioning script.")
}
```

## <a name="create-the-namespacemanager-class"></a>De klasse NamespaceManager maken
Voor het inrichten van Notification Hubs, maak een instantie van de [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) klasse van de SDK. 

U kunt de [Get-AzureSBAuthorizationRule] cmdlet opgenomen met Azure PowerShell om op te halen van een autorisatieregel die wordt gebruikt voor een verbindingsreeks. Een verwijzing naar de `NamespaceManager` exemplaar wordt opgeslagen in de `$NamespaceManager` variabele. `$NamespaceManager` wordt gebruikt voor het inrichten van een notification hub.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Een nieuwe Notification Hub inrichten
Gebruiken voor het inrichten van een nieuwe notification hub, de [.NET API voor Notification Hubs].

In dit gedeelte van het script maakt instellen u vier lokale variabelen. 

1. `$Namespace`: Stel dit in op de naam van de naamruimte waarin u wilt maken van een notification hub.
2. `$Path`: Stel dit pad op de naam van de nieuwe notification hub.  Bijvoorbeeld: 'MyHub'.    
3. `$WnsPackageSid`: Stel dit in op de pakket-SID voor uw Windows-App uit de [Windows Dev Center](https://developer.microsoft.com/en-us/windows).
4. `$WnsSecretkey`: Stel dit in op de geheime sleutel voor uw Windows-App uit de [Windows Dev Center](https://developer.microsoft.com/en-us/windows).

Deze variabelen worden gebruikt om verbinding maken met uw naamruimte en een nieuwe Notification Hub is geconfigureerd voor het afhandelen van Windows Notification Services (WNS) verzenden met WNS-referenties voor een Windows-App maken. Voor meer informatie over het verkrijgen van het pakket-SID en geheime sleutel zien, de [aan de slag met Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) zelfstudie. 

* Het script-fragment wordt de `NamespaceManager` object om te controleren om te zien als de Notification Hub wordt geïdentificeerd door `$Path` bestaat.
* Als deze niet bestaat, maakt het script `NotificationHubDescription` referenties met WNS en doorgegeven aan de `NamespaceManager` klasse `CreateNotificationHub` methode.

``` powershell

$Namespace = "<Enter your namespace>"
$Path  = "<Enter a name for your notification hub>"
$WnsPackageSid = "<your package sid>"
$WnsSecretkey = "<enter your secret key>"

$WnsCredential = New-Object -TypeName Microsoft.Azure.NotificationHubs.WnsCredential -ArgumentList $WnsPackageSid,$WnsSecretkey

# Query the namespace
$CurrentNamespace = Get-AzureSBNamespace -Name $Namespace

# Check if the namespace already exists
if ($CurrentNamespace)
{
    Write-Output "The namespace [$Namespace] in the [$($CurrentNamespace.Region)] region was found."

    # Create the NamespaceManager object used to create a new notification hub
    $sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
    Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
    $NamespaceManager = [Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
    Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."

    # Check to see if the Notification Hub already exists
    if ($NamespaceManager.NotificationHubExists($Path))
    {
        Write-Output "The [$Path] notification hub already exists in the [$Namespace] namespace."  
    }
    else
    {
        Write-Output "Creating the [$Path] notification hub in the [$Namespace] namespace."
        $NHDescription = New-Object -TypeName Microsoft.Azure.NotificationHubs.NotificationHubDescription -ArgumentList $Path;
        $NHDescription.WnsCredential = $WnsCredential;
        $NamespaceManager.CreateNotificationHub($NHDescription);
        Write-Output "The [$Path] notification hub was created in the [$Namespace] namespace."
    }
}
else
{
    Write-Host "The [$Namespace] namespace does not exist."
}
```




## <a name="additional-resources"></a>Aanvullende resources
* [Service Bus met PowerShell beheren](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [Over het maken van Service Bus-wachtrijen, onderwerpen en abonnementen die gebruikmaken van een PowerShell-script](https://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Over het maken van een Service Bus-Namespace en een Event Hub met behulp van een PowerShell-script](https://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Sommige kant-en-scripts zijn ook beschikbaar voor downloaden:

* [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[Aankoopopties]: http://azure.microsoft.com/pricing/purchase-options/
[Aanbiedingen voor leden]: http://azure.microsoft.com/pricing/member-offers/
[Gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[installeren en configureren van Azure PowerShell]: /powershell/azureps-cmdlets-docs
[.NET API voor Notification Hubs]: https://docs.microsoft.com/dotnet/api/overview/azure/notification-hubs?view=azure-dotnet
[Get-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbnamespace
[New-AzureSBNamespace]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/new-azuresbnamespace
[Get-AzureSBAuthorizationRule]: https://docs.microsoft.com/powershell/module/servicemanagement/azure/get-azuresbauthorizationrule


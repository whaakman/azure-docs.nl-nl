---
title: Notification Hubs implementeren en beheren met PowerShell
description: Het maken en beheren van Notification Hubs met behulp van PowerShell voor automatisering
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 7c58f2c8-0399-42bc-9e1e-a7f073426451
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: powershell
ms.devlang: na
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 4db058e4bd91dc287b14e887abc6c378c65c4a2b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="deploy-and-manage-notification-hubs-using-powershell"></a>Notification Hubs implementeren en beheren met PowerShell
## <a name="overview"></a>Overzicht
Dit artikel laat zien hoe u maken en beheren van Azure Notification Hubs met behulp van PowerShell. De volgende algemene automatiseringstaken worden weergegeven in dit onderwerp.

* Een Notification Hub maken
* Referenties instellen

Als u ook een nieuwe service bus-naamruimte voor uw notification hubs maken moet, Zie [Service Bus beheren met PowerShell](../service-bus-messaging/service-bus-powershell-how-to-provision.md).

Het beheren van meldingen Hubs wordt rechtstreeks door de opgenomen met Azure PowerShell-cmdlets niet ondersteund. De aanbevolen aanpak van PowerShell is om te verwijzen naar de assembly Microsoft.Azure.NotificationHubs.dll. De assembly wordt verdeeld met het [Microsoft Azure Notification Hubs NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* Een Azure-abonnement. Azure is een platform op basis van abonnement. Zie voor meer informatie over het verkrijgen van een abonnement [koopopties], [lid biedt], of [gratis proefversie].
* Een computer met Azure PowerShell. Zie voor instructies [installeren en configureren van Azure PowerShell].
* Een algemeen begrip van de PowerShell-scripts, NuGet-pakketten en .NET Framework.

## <a name="including-a-reference-to-the-net-assembly-for-service-bus"></a>Een verwijzing naar de .NET-assembly voor Service Bus
Het beheren van Azure Notification Hubs is nog niet opgenomen in de PowerShell-cmdlets in Azure PowerShell. Voor het inrichten van notification hubs, kunt u de .NET-client die is opgegeven in de [Microsoft Azure Notification Hubs NuGet-pakket](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).

Controleer eerst of uw script kan vinden de **Microsoft.Azure.NotificationHubs.dll** assembly die wordt geïnstalleerd als een NuGet-pakket in Visual Studio-project. Om te kunnen worden flexibele, voert het script als volgt:

1. Hiermee bepaalt u het pad waarop deze is aangeroepen.
2. Het pad passeert totdat er een map met de naam gevonden `packages`. Deze map wordt gemaakt tijdens de installatie van NuGet-pakketten voor Visual Studio-projecten.
3. Recursief zoekopdrachten de `packages` map voor een assembly met de naam **Microsoft.Azure.NotificationHubs.dll**.
4. Verwijst naar de assembly zodat de typen beschikbaar voor later gebruik zijn.

Hier ziet u hoe deze stappen worden geïmplementeerd in een PowerShell-script:

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
Voor het inrichten van Notification Hubs, geen exemplaar maken van de [NamespaceManager](https://msdn.microsoft.com/library/azure/microsoft.azure.notificationhubs.namespacemanager.aspx) klasse van de SDK. 

U kunt de [Get-AzureSBAuthorizationRule] cmdlet die deel uitmaakt van Azure PowerShell voor het ophalen van een autorisatieregel die wordt gebruikt voor een verbindingsreeks opgeven. Bewaren wij een verwijzing naar de `NamespaceManager` exemplaar de `$NamespaceManager` variabele. We gebruiken `$NamespaceManager` voor het inrichten van een notification hub.

``` powershell
$sbr = Get-AzureSBAuthorizationRule -Namespace $Namespace
# Create the NamespaceManager object to create the hub
Write-Output "Creating a NamespaceManager object for the [$Namespace] namespace..."
$NamespaceManager=[Microsoft.Azure.NotificationHubs.NamespaceManager]::CreateFromConnectionString($sbr.ConnectionString);
Write-Output "NamespaceManager object for the [$Namespace] namespace has been successfully created."
```


## <a name="provisioning-a-new-notification-hub"></a>Inrichting van een nieuwe Notification Hub
Voor het inrichten van een nieuwe notification hub gebruiken de [.NET API voor Notification Hubs].

In dit gedeelte van het script moet u vier lokale variabelen instellen. 

1. `$Namespace`: Stel dit in op de naam van de naamruimte waarin u wilt maken van een notification hub.
2. `$Path`: Stel dit pad op de naam van de nieuwe notification hub.  Bijvoorbeeld 'MyHub'.    
3. `$WnsPackageSid`: Stel dit in op de pakket-SID voor u Windows-App uit de [Windows-ontwikkelaarscentrum](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).
4. `$WnsSecretkey`: Stel dit in op de geheime sleutel voor u Windows-App uit de [Windows-ontwikkelaarscentrum](http://go.microsoft.com/fwlink/p/?linkid=266582&clcid=0x409).

Deze variabelen worden gebruikt voor verbinding maken met uw naamruimte en maak een nieuwe Notification Hub is geconfigureerd voor het afhandelen van meldingen van de Windows Notification Services (WNS) met WNS-referenties voor een Windows-App. Voor informatie over het verkrijgen van het pakket SID en de geheime sleutel zien, de [aan de slag met Notification Hubs](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) zelfstudie. 

* Het script-codefragment maakt gebruik van de `NamespaceManager` object om te controleren om te zien als de Notification Hub wordt geïdentificeerd door `$Path` bestaat.
* Als deze niet bestaat nog, maakt het script een `NotificationHubDescription` referenties met WNS en geef die naar de `NamespaceManager` klasse `CreateNotificationHub` methode.

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
* [Servicebus met PowerShell beheren](../service-bus-messaging/service-bus-powershell-how-to-provision.md)
* [Het maken van Service Bus-wachtrijen, onderwerpen en abonnementen op basis van een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/02/how-to-create-a-service-bus-queues-topics-and-subscriptions-using-a-powershell-script.aspx)
* [Het maken van een Service Bus-Namespace en een Event Hub met een PowerShell-script](http://blogs.msdn.com/b/paolos/archive/2014/12/01/how-to-create-a-service-bus-namespace-and-an-event-hub-using-a-powershell-script.aspx)

Er zijn ook enkele kant-en-scripts downloaden:

* [Service Bus PowerShell-Scripts](https://code.msdn.microsoft.com/windowsazure/Service-Bus-PowerShell-a46b7059)

[koopopties]: http://azure.microsoft.com/pricing/purchase-options/
[lid biedt]: http://azure.microsoft.com/pricing/member-offers/
[gratis proefversie]: http://azure.microsoft.com/pricing/free-trial/
[installeren en configureren van Azure PowerShell]: /powershell/azureps-cmdlets-docs
[.NET API voor Notification Hubs]: https://msdn.microsoft.com/library/azure/mt414893.aspx
[Get-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495122.aspx
[New-AzureSBNamespace]: https://msdn.microsoft.com/library/azure/dn495165.aspx
[Get-AzureSBAuthorizationRule]: https://msdn.microsoft.com/library/azure/dn495113.aspx


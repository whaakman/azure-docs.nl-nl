---
title: Klonen van Web-App met behulp van PowerShell
description: Informatie over het klonen van uw Web-Apps naar de nieuwe Web-Apps met behulp van PowerShell.
services: app-service\web
documentationcenter: 
author: ahmedelnably
manager: stefsch
editor: 
ms.assetid: f9a5cfa1-fbb0-41e6-95d1-75d457347a35
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/13/2016
ms.author: aelnably
ms.openlocfilehash: dc252903571857b5fc89d1d9a2c63cd6b44e9021
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-app-service-app-cloning-using-powershell"></a>Azure App Service-App met behulp van PowerShell klonen
Met de release van Microsoft Azure PowerShell versie 1.1.0 is een nieuwe optie zijn toegevoegd aan New-AzureRMWebApp die de gebruiker de mogelijkheid geven voor het klonen van een bestaande Web-App naar een nieuwe app in een andere regio of in dezelfde regio. Hiermee schakelt u klanten een aantal apps implementeren in verschillende regio's snel en eenvoudig.

Klonen van de App is momenteel alleen ondersteund voor premium-laag app service-abonnementen. De nieuwe functie maakt gebruik van dezelfde beperkingen als de functie Web Apps back-up, Zie [Back-up van een web-app in Azure App Service](web-sites-backup.md).

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="cloning-an-existing-app"></a>Klonen van een bestaande App
Scenario: Een bestaande web-app in Zuid-centraal VS regio, de gebruiker wil de inhoud naar een nieuwe web-app in de regio Noord-centraal VS klonen. Dit kan worden bewerkstelligd met behulp van de Azure Resource Manager-versie van de PowerShell-cmdlet voor het maken van een nieuwe web-app met de optie - SourceWebApp.

Naam van de resourcegroep met de bron-web-app weet, kunnen we de volgende PowerShell-opdracht ophalen van informatie van de bron web-app (in dit geval bron webapp genoemd) gebruiken:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

Voor het maken van een nieuwe App Service-Plan, kunnen we de opdracht New-AzureRmAppServicePlan zoals in het volgende voorbeeld gebruiken

    New-AzureRmAppServicePlan -Location "South Central US" -ResourceGroupName DestinationAzureResourceGroup -Name NewAppServicePlan -Tier Premium

Met de opdracht New-AzureRmWebApp we kunnen de nieuwe web-app maken in de regio Noord-centraal VS, en deze koppelen aan een bestaand App Service-Plan premium-laag, bovendien we gebruik van dezelfde resourcegroep bevinden als de bron-web-app of een nieuwe resourcegroep definiëren, het volgende voorbeeld toont die:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp

Voor het klonen van een bestaande web-app, inclusief alle gekoppelde implementatie sleuven, de gebruiker moet de parameter IncludeSourceWebAppSlots gebruiken, de volgende PowerShell-opdracht laat zien hoe u deze parameter met de opdracht New-AzureRmWebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -IncludeSourceWebAppSlots

Als u wilt klonen van een bestaande web-app in dezelfde regio, moet de gebruiker een nieuwe resourcegroep en een nieuwe app service maken plannen in dezelfde regio en klik vervolgens met behulp van de volgende PowerShell-opdracht voor het klonen van de web-app

    $destapp = New-AzureRmWebApp -ResourceGroupName NewAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan NewAppServicePlan -SourceWebApp $srcap

## <a name="cloning-an-existing-app-to-an-app-service-environment"></a>Klonen van een bestaande App aan een App-serviceomgeving
Scenario: Een bestaande web-app in Zuid-centraal VS regio, de gebruiker wil graag klonen van de inhoud naar een nieuwe web-app aan een bestaand App Service omgeving (as-omgeving).

Naam van de resourcegroep met de bron-web-app weet, kunnen we de volgende PowerShell-opdracht ophalen van informatie van de bron web-app (in dit geval bron webapp genoemd) gebruiken:

    $srcapp = Get-AzureRmWebApp -ResourceGroupName SourceAzureResourceGroup -Name source-webapp

De naam van de as-omgeving en de naam van de resourcegroep die deel uitmaakt van de as-omgeving te weten, kunnen gebruikers de opdracht New-AzureRmWebApp gebruiken voor het maken van de nieuwe web-app in de bestaande as-omgeving, het volgende voorbeeld toont die:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -ASEName DestinationASE -ASEResourceGroupName DestinationASEResourceGroupName -SourceWebApp $srcapp

De locatie-parameter is vereist om verouderde reden, maar in het geval van een app maken in een as-omgeving worden genegeerd. 

## <a name="cloning-an-existing-app-slot"></a>Klonen van een bestaande App-sleuf
Scenario: De gebruiker wil graag klonen van een bestaande Web-Appsite ofwel een nieuwe Web-App of een nieuwe sleuf voor Web-App. De nieuwe Web-App kunnen zich in dezelfde regio bevinden als de oorspronkelijke sleuf voor Web-App of in een andere regio.

Naam van de resourcegroep met de bron-web-app weet, kunnen we de volgende PowerShell-opdracht gebruiken voor de bron web-appsite van informatie (in dit geval bron webappslot genoemd) gekoppeld aan de bron-Web-App Web-App:

    $srcappslot = Get-AzureRmWebAppSlot -ResourceGroupName SourceAzureResourceGroup -Name source-webapp -Slot source-webappslot

Het volgende voorbeeld toont het maken van een kloon van de bron-web-app in een nieuwe WebApp:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "North Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcappslot

## <a name="configuring-traffic-manager-while-cloning-a-app"></a>Traffic Manager tijdens het klonen van een App configureren
Meerdere landen/regio-web-apps maken en configureren van Azure Traffic Manager om verkeer te routeren naar alle deze web-apps, is een n belangrijke scenario om ervoor te zorgen dat klanten apps maximaal beschikbaar zijn, wordt bij het klonen van een bestaande web-app hebt u de optie voor beide web-apps naar een traffic manager-profiel of een bestaande verbinding - die alleen Azure Resource Manager-versie van Traffic Manager wordt ondersteund.

### <a name="creating-a-new-traffic-manager-profile-while-cloning-a-app"></a>Een nieuw Traffic Manager-profiel maken tijdens het klonen van een App
Scenario: De gebruiker wil graag klonen van een web-app naar een andere regio tijdens het configureren van een Azure Resource Manager traffic manager-profiel die zijn zowel Webapps. Het volgende voorbeeld toont een kloon van de bron-web-app in een nieuwe WebApp gemaakt tijdens het configureren van een nieuw Traffic Manager-profiel:

    $destapp = New-AzureRmWebApp -ResourceGroupName DestinationAzureResourceGroup -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileName newTrafficManagerProfile

### <a name="adding-new-cloned-web-app-to-an-existing-traffic-manager-profile"></a>Toevoegen van nieuwe Web-App gekloond aan een bestaand Traffic Manager-profiel
Scenario: De gebruiker al een Azure Resource Manager traffic manager-profiel dat hij wil graag beide WebApps als eindpunten toevoegen. Hiervoor moeten we eerst voor het samenstellen van de bestaande traffic manager-profiel-id, moeten we de abonnements-id, de naam van resourcegroep en de bestaande profielnaam voor traffic manager.

    $TMProfileID = "/subscriptions/<Your subscription ID goes here>/resourceGroups/<Your resource group name goes here>/providers/Microsoft.TrafficManagerProfiles/ExistingTrafficManagerProfileName"

Nadat ik de traffic Manager-id, het volgende voorbeeld toont een kloon van de bron-web-app in een nieuwe WebApp maakt terwijl het deze toe te voegen aan een bestaand Traffic Manager-profiel:

    $destapp = New-AzureRmWebApp -ResourceGroupName <Resource group name> -Name dest-webapp -Location "South Central US" -AppServicePlan DestinationAppServicePlan -SourceWebApp $srcapp -TrafficManagerProfileId $TMProfileID

## <a name="current-restrictions"></a>Huidige beperkingen
Deze functie is momenteel in preview, wij werken als u wilt toevoegen van nieuwe mogelijkheden na verloop van tijd, in de volgende lijst worden de bekende beperkingen van de huidige versie van het klonen van de app:

* Instellingen voor automatisch schalen worden niet gekloond.
* Back-upschema instellingen worden niet gekloond.
* VNET-instellingen worden niet gekloond.
* App Insights worden niet automatisch ingesteld op de doel-web-app
* Eenvoudige verificatie-instellingen worden niet gekloond.
* Kudu extensie worden niet gekloond.
* TiP regels worden niet gekloond.
* Database-inhoud worden niet gekloond.

### <a name="references"></a>Verwijzingen
* [Klonen van de Web-App](app-service-web-app-cloning.md)
* [Back-up van een web-app in Azure App Service](web-sites-backup.md)
* [Azure Resource Manager-ondersteuning voor Azure Traffic Manager-Preview](../traffic-manager/traffic-manager-powershell-arm.md)
* [Inleiding tot de App Service-omgeving](environment/intro.md)
* [Azure PowerShell gebruiken met Azure Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)


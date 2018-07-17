---
title: Een Kubernetes-ontwikkelomgeving in de cloud maken met .NET Core en VS Code | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 07/09/2018
ms.topic: tutorial
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 4da5b42ddd235fa26834e582a911140116692d34
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38299556"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core"></a>Aan de slag in Azure Dev Spaces met behulp van .NET Core

[!INCLUDE[](includes/learning-objectives.md)]

[!INCLUDE[](includes/see-troubleshooting.md)]

U kunt nu een op Kubernetes gebaseerde ontwikkelomgeving maken in Azure.

[!INCLUDE[](includes/portal-aks-cluster.md)]

## <a name="install-the-azure-cli"></a>Azure-CLI installeren
Azure Dev Spaces vereist minimale instellingen voor de lokale computer. De configuratie van uw ontwikkelomgeving wordt grotendeels opgeslagen in de cloud en kan worden gedeeld met andere gebruikers. Begin met het downloaden en uitvoeren van de [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest). 

> [!IMPORTANT]
> Als u de Azure CLI al hebt geïnstalleerd, controleert u of u versie 2.0.38 of hoger gebruikt.

[!INCLUDE[](includes/sign-into-azure.md)]

[!INCLUDE[](includes/use-dev-spaces.md)]

[!INCLUDE[](includes/install-vscode-extension.md)]

Terwijl u wacht totdat het cluster wordt gemaakt, kunt u beginnen met het ontwikkelen van code.

## <a name="create-a-web-app-running-in-a-container"></a>Een web-app maken die wordt uitgevoerd in een container

In deze sectie maakt u een ASP.NET Core-web-app en voert u deze uit in een container in Kubernetes.

### <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken
Als u [.NET Core](https://www.microsoft.com/net) hebt geïnstalleerd, kunt u snel een ASP.NET Core-web-app maken in een map met de naam `webfrontend`.
    
```cmd
dotnet new mvc --name webfrontend
```

Of **download voorbeeldcode vanuit GitHub**  door te navigeren naar https://github.com/Azure/dev-spaces en selecteer **Clone or Download** om de GitHub-opslagplaats naar uw lokale omgeving te downloaden. De code voor deze handleiding bevindt zich in `samples/dotnetcore/getting-started/webfrontend`.

[!INCLUDE[](includes/azds-prep.md)]

[!INCLUDE[](includes/build-run-k8s-cli.md)]

### <a name="update-a-content-file"></a>Een inhoudsbestand bijwerken
Azure Dev Spaces draait niet alleen om het ophalen van code die wordt uitgevoerd in Kubernetes. Het gaat er om dat u de codewijzigingen snel en iteratief toegepast kunt zien in een Kubernetes-omgeving in de cloud.

1. Zoek het bestand `./Views/Home/Index.cshtml` en bewerk de HTML-code. Wijzig bijvoorbeeld regel 70 waar `<h2>Application uses</h2>` staat in iets als: `<h2>Hello k8s in Azure!</h2>`
1. Sla het bestand op. Enkele ogenblikken later ziet u in het terminalvenster een bericht met de melding dat een bestand in de actieve container is bijgewerkt.
1. Ga naar de browser en vernieuw de pagina. De bijgewerkte HTML-code wordt op de webpagina weergegeven.

Wat is er gebeurd? Bewerkingen van inhoudsbestanden, zoals HTML en CSS, vereisen geen hercompilatie in een .NET Core-web-app, dus worden gewijzigde inhoudsbestanden in de actieve container in Azure door een actieve `azds up`-opdracht automatisch gesynchroniseerd, zodat u uw inhoudsbewerkingen meteen kunt zien.

### <a name="update-a-code-file"></a>Een codebestand bijwerken
Het bijwerken van codebestanden vereist iets meer werk, omdat een .NET Core-app bijgewerkte binaire toepassingsbestanden moet bouwen en produceren.

1. Druk in het terminalvenster op `Ctrl+C` (om `azds up` te stoppen).
1. Open het codebestand met de naam `Controllers/HomeController.cs` en bewerk het bericht dat op de pagina Info wordt weergegeven: `ViewData["Message"] = "Your application description page.";`
1. Sla het bestand op.
1. Voer `azds up` uit in het terminalvenster. 

Met deze opdracht wordt de installatiekopie van de container opnieuw gebouwd en het Helm-diagram opnieuw geïmplementeerd. Ga naar het menu Info in de web-app om te zien of uw codewijzigingen zijn doorgevoerd in de actieve toepassing.


Er bestaat echter een nog *snellere methode* voor het ontwikkelen van code. Deze methode gaat u in de volgende sectie verkennen. 

## <a name="debug-a-container-in-kubernetes"></a>Fouten opsporen in een Kubernetes-container

[!INCLUDE[](includes/debug-intro.md)]

[!INCLUDE[](includes/init-debug-assets-vscode.md)]


### <a name="select-the-azds-debug-configuration"></a>Selecteer de AZDS-foutopsporingsconfiguratie
1. Om de foutopsporingsweergave te openen, klikt u op het pictogram Foutopsporing in de **activiteitenbalk** van VS Code.
1. Selecteer **.NET Core starten (AZDS)** als de actieve foutopsporingsconfiguratie.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Als u geen Azure Dev Spaces-opdrachten ziet in het Opdrachtenpalet, controleert u of u de VS Code-extensie hebt geïnstalleerd voor Azure Dev Spaces. Zorg ervoor dat de werkruimte die u hebt geopend in VS Code de map is die azds.yaml bevat.


### <a name="debug-the-container-in-kubernetes"></a>Fouten opsporen in de container in Kubernetes
Druk op **F5** om fouten in uw code op te sporen in Kubernetes.

Net als bij de opdracht `up` wordt code gesynchroniseerd met de ontwikkelomgeving en wordt een container gemaakt en geïmplementeerd in Kubernetes. Op dit moment is het foutopsporingsprogramma uiteraard gekoppeld aan de externe container.

[!INCLUDE[](includes/tip-vscode-status-bar-url.md)]

Stel een onderbrekingspunt in een codebestand aan serverzijde, bijvoorbeeld binnen de functie `Index()` in het bronbestand `Controllers/HomeController.cs`. Als de browserpagina wordt vernieuwd, wordt het onderbrekingspunt bereikt.

U hebt volledige toegang tot foutopsporingsgegevens, net alsof de code lokaal wordt uitgevoerd. Denk hierbij aan de aanroep-stack, lokale variabelen en informatie over uitzonderingen, enzovoort.

### <a name="edit-code-and-refresh"></a>Code bewerken en vernieuwen
Maak een codewijziging terwijl het foutopsporingsprogramma actief is. Wijzig bijvoorbeeld het bericht op de pagina Info in `Controllers/HomeController.cs`. 

```csharp
public IActionResult About()
{
    ViewData["Message"] = "My custom message in the About page.";
    return View();
}
```

Sla het bestand op en klik in het deelvenster **Debug actions** op de knop **Refresh**. 

![](media/get-started-netcore/debug-action-refresh.png)

In plaats van telkens als codewijzigingen zijn aangebracht een nieuwe containerinstallatiekopie opnieuw te bouwen en opnieuw te implementeren, wat vaak behoorlijk wat tijd kost, hercompileert Azure Dev Spaces incrementeel code binnen de bestaande container voor een snellere bewerkings-/foutopsporingslus.

Vernieuw de web-app in de browser en ga naar de pagina Info. U ziet dat uw aangepaste bericht wordt weergegeven in de gebruikersinterface.

**U beschikt nu over een methode om code snel te ontwikkelen en foutopsporing rechtstreeks uit te voeren in Kubernetes.** Hierna ziet u hoe u een tweede container kunt maken en aanroepen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over teamontwikkeling](team-development-netcore.md)
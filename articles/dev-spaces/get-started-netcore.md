---
title: Een Kubernetes-dev-ruimte in de cloud met behulp van .NET Core en VS-Code maken
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/26/2018
ms.topic: tutorial
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s
ms.openlocfilehash: e05dbc570836741a69ed229fc93eb32a7dfd01dd
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503165"
---
# <a name="get-started-on-azure-dev-spaces-with-net-core"></a>Aan de slag in Azure Dev Spaces met behulp van .NET Core

In deze handleiding leert u het volgende:

- Een Kubernetes-omgeving maakt in Azure die is geoptimaliseerd voor ontwikkeling - een _dev-ruimte_.
- Iteratief code ontwikkelt in containers met behulp van VS Code en de opdrachtregel.
- Uw code op een productieve manier ontwikkelen en testen in een teamomgeving.

> [!Note]
> **Als u zitten** op elk gewenst moment, Zie de [probleemoplossing](troubleshooting.md) sectie.

## <a name="install-the-azure-cli"></a>Azure-CLI installeren
Azure Dev Spaces vereist minimale instellingen voor de lokale computer. De configuratie van uw ontwikkelomgeving wordt grotendeels opgeslagen in de cloud en kan worden gedeeld met andere gebruikers. Begin met het downloaden en uitvoeren van de [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest).

### <a name="sign-in-to-azure-cli"></a>Aanmelden bij Azure CLI
Meld u aan bij Azure. Typ de volgende opdracht in een terminalvenster:

```cmd
az login
```

> [!Note]
> Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free) maken.

#### <a name="if-you-have-multiple-azure-subscriptions"></a>Als u meerdere Azure-abonnementen hebt...
U kunt uw abonnementen bekijken door het volgende uit te voeren: 

```cmd
az account list
```
Zoek naar het abonnement dat `isDefault: true` heeft in de JSON-uitvoer.
Als dit niet het abonnement is dat u wilt gebruiken, kunt u het standaardabonnement wijzigen:

```cmd
az account set --subscription <subscription ID>
```

## <a name="create-a-kubernetes-cluster-enabled-for-azure-dev-spaces"></a>Een Kubernetes-cluster maken dat is ingeschakeld voor Azure Dev Spaces

Bij de opdrachtprompt, maak de resourcegroep in een [regio die ondersteuning biedt voor Azure Dev spaties][supported-regions].

```cmd
az group create --name MyResourceGroup --location <region>
```

Maak een Kubernetes-cluster met de volgende opdracht:

```cmd
az aks create -g MyResourceGroup -n MyAKS --location <region> --disable-rbac --generate-ssh-keys
```

Het duurt een paar minuten om het cluster te maken.

### <a name="configure-your-aks-cluster-to-use-azure-dev-spaces"></a>Uw AKS-cluster configureren voor het gebruik van Azure Dev Spaces

Voer de volgende Azure CLI-opdracht in. Gebruik daarbij de resourcegroep die uw AKS-cluster en de naam van uw AKS-cluster bevat. Met de opdracht wordt uw cluster geconfigureerd met ondersteuning voor Azure Dev Spaces.

   ```cmd
   az aks use-dev-spaces -g MyResourceGroup -n MyAKS
   ```
   
> [!IMPORTANT]
> Met het Azure Dev Spaces-configuratieproces wordt de eventueel aanwezige `azds`-naamruimte in het cluster verwijderd.

## <a name="get-kubernetes-debugging-for-vs-code"></a>Kubernetes-foutopsporing voor VS Code downloaden
Uitgebreide functies, zoals Kubernetes-foutopsporing, zijn beschikbaar voor .NET Core en Node.js-ontwikkelaars die VS Code gebruiken.

1. Installeer [VS Code](https://code.visualstudio.com/Download) als u dit nog niet hebt.
1. Download en installeer de [extensie VS Azure Dev Spaces](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds). Klik eenmaal op Installeren op de Microsoft Azure Marketplace-pagina van de extensie en nog eens in VS Code. 

## <a name="create-a-web-app-running-in-a-container"></a>Een web-app maken die wordt uitgevoerd in een container

In deze sectie maakt u een ASP.NET Core web-app maken en deze uitvoeren in een container in Kubernetes.

### <a name="create-an-aspnet-core-web-app"></a>Een ASP.NET Core-web-app maken
Kloon of download de [voorbeeldtoepassing voor Azure Dev Spaces](https://github.com/Azure/dev-spaces). In dit artikel wordt de code gebruikt uit de map *samples/dotnetcore/getting-started/webfrontend*.

## <a name="preparing-code-for-docker-and-kubernetes-development"></a>Code voorbereiden voor de ontwikkeling van Docker en Kubernetes
U hebt nu een eenvoudige web-app die lokaal kan worden uitgevoerd. U gaat hier nu een container van maken door assets te maken waarmee de container van de app wordt gedefinieerd evenals hoe deze in Kubernetes wordt geïmplementeerd. Deze taak kan eenvoudig worden uitgevoerd met Azure Dev Spaces: 

1. Start VS Code en open de map `webfrontend`. (U kunt eventuele standaardprompts negeren om foutopsporingsassets toe te voegen of om het project te herstellen.)
1. Open de geïntegreerde terminal in VS Code (via het menu **Weergave > Geïntegreerde Terminal**).
1. Voer de volgende opdracht uit (controleer of **webfrontend** de huidige map is):

    ```cmd
    azds prep --public
    ```

Met de opdracht `azds prep` van Azure-CLI worden Docker- en Kubernetes-assets met standaardinstellingen gemaakt:
* In `./Dockerfile` wordt beschreven wat de containerinstallatiekopie van de app is, en hoe de broncode is opgebouwd en wordt uitgevoerd in de container.
* Met een [Helm-grafiek](https://docs.helm.sh) die zich onder `./charts/webfrontend` bevindt, wordt beschreven hoe de container in Kubernetes moet worden geïmplementeerd.

Op dit moment is het nog niet nodig om de volledige inhoud van deze bestanden te begrijpen. Wat u wel moet weten is dat **de dezelfde assets voor 'configuratie als code' in Docker en Kubernetes kunnen worden gebruikt van ontwikkeling tot productie, waardoor er meer consistentie tussen de verschillende omgevingen bestaat.**
 
Er wordt met de opdracht `prep` ook een bestand met de naam `./azds.yaml` gegenereerd. Dit is het configuratiebestand voor Azure Dev Spaces. Dit vormt een aanvulling op de Docker- en Kubernetes-artefacten met extra configuraties die herhaalbare ontwikkelingsmogelijkheden in Azure bieden.

## <a name="build-and-run-code-in-kubernetes"></a>Code schrijven en uitvoeren in Kubernetes
We gaan onze code uitvoeren! In het terminalvenster voert u deze opdracht uit vanuit de **hoofdcodemap**, webfrontend:

```cmd
azds up
```

Houd de uitvoer van de opdracht in de gaten. Er zijn meerdere zaken die u zullen opvallen tijdens de uitvoering:
- De broncode is gesynchroniseerd met de dev-ruimte in Azure.
- Er wordt een containerinstallatiekopie gemaakt in Azure, zoals is aangegeven door de Docker-assets in uw codemap.
- Er worden Kubernetes-objecten gemaakt die de containerinstallatiekopie gebruiken zoals wordt aangegeven in de Helm-grafiek in uw codemap.
- Er wordt informatie over het eindpunt of de eindpunten van de container weergegeven. In ons geval verwachten we een openbare HTTP-URL.
- Ervan uitgaande dat de fases hierboven goed worden afgerond, moet u nu `stdout`-uitvoer (en `stderr`) gaan zien terwijl de container wordt opgestart.

> [!Note]
> Deze stappen nemen de eerste keer dat de opdracht `up` wordt uitgevoerd, meer tijd in beslag, maar latere uitvoeringen zullen sneller verlopen.

### <a name="test-the-web-app"></a>De web-app testen
Scannen van de console-uitvoer voor de *toepassing aan de slag* bericht bevestigen dat de `up` opdracht is voltooid:

```
Service 'webfrontend' port 80 (TCP) is available at 'http://localhost:<port>'
Service 'webfrontend' port 'http' is available at http://webfrontend.1234567890abcdef1234.eus.azds.io/
Microsoft (R) Build Engine version 15.9.20+g88f5fadfbe for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  webfrontend -> /src/bin/Debug/netcoreapp2.2/webfrontend.dll
  webfrontend -> /src/bin/Debug/netcoreapp2.2/webfrontend.Views.dll

Build succeeded.
    0 Warning(s)
    0 Error(s)

Time Elapsed 00:00:00.94
[...]
webfrontend-5798f9dc44-99fsd: Now listening on: http://[::]:80
webfrontend-5798f9dc44-99fsd: Application started. Press Ctrl+C to shut down.
```

Identificeren van de openbare URL voor de service in de uitvoer van de `up` opdracht. Deze eindigt op `.azds.io`. In het bovenstaande voorbeeld wordt de openbare URL is `http://webfrontend.1234567890abcdef1234.eus.azds.io/`.

Als u wilt zien van uw web-app, moet u de openbare URL openen in een browser. U ziet ook `stdout` en `stderr` uitvoer wordt gestreamd naar de *azds trace* terminal-venster als u communiceren met uw web-app. U ziet ook de gegevens voor HTTP-aanvragen bijhouden terwijl ze het systeem. Dit maakt het eenvoudiger voor u om bij te houden van complexe meerdere services aanroepen tijdens de ontwikkeling. Met dit instrumentatiepakket toegevoegd door Dev spaties bevat deze aanvraag bijhouden.

![azds traceren terminal-venster](media/get-started-netcore/azds-trace.png)


> [!Note]
> Naast de openbare URL, kunt u de alternatieve `http://localhost:<portnumber>` URL die wordt weergegeven in de console-uitvoer. Als u de localhost-URL gebruikt, lijkt het misschien alsof de container lokaal wordt uitgevoerd, maar wordt deze feitelijk uitgevoerd in AKS. Azure Dev opslagruimten maakt gebruik van Kubernetes *poort-zone voor forward* functionaliteit de localhost-poort toewijzen aan de container die wordt uitgevoerd in AKS. Dit vereenvoudigt het uitvoeren van interactie met de service van uw lokale computer.

### <a name="update-a-content-file"></a>Een inhoudsbestand bijwerken
Azure Dev Spaces draait niet alleen om het ophalen van code die wordt uitgevoerd in Kubernetes. Het gaat er om dat u de codewijzigingen snel en iteratief toegepast kunt zien in een Kubernetes-omgeving in de cloud.

1. Zoek het bestand `./Views/Home/Index.cshtml` en bewerk de HTML-code. Wijzig bijvoorbeeld [regel 73 die leest `<h2>Application uses</h2>` ](https://github.com/Azure/dev-spaces/blob/master/samples/dotnetcore/getting-started/webfrontend/Views/Home/Index.cshtml#L73) naar ongeveer als volgt: 

    ```html
    <h2>Hello k8s in Azure!</h2>
    ```

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

In deze sectie gaat u VS Code gebruiken om direct fouten op te sporen in de container die in Azure wordt uitgevoerd. U leert ook hoe u een cyclus voor bewerken-uitvoeren-testen sneller kunt uitvoeren.

![](media/common/edit-refresh-see.png)

> [!Note]
> **Als u op enig moment niet verder kunt**, kunt u de [probleemoplossingssectie](troubleshooting.md) raadplegen of een opmerking op deze pagina plaatsen.

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Foutopsporingsassets initialiseren met de VS Code-extensie
U moet eerst uw codeproject configureren, zodat de VS Code met onze dev-ruimte in Azure gaat communiceren. De VS Code-extensie voor Azure Dev Spaces bevat een helpopdracht om de foutopsporingsconfiguratie in te stellen. 

Open het **Opdrachtenpalet** (via het menu **Beeld | Opdrachtenpalet**), en gebruik automatisch aanvullen om te typen en deze opdracht te selecteren: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`. 

Hiermee wordt de foutopsporingsconfiguratie voor Azure Dev Spaces toegevoegd onder de map `.vscode`. Deze opdracht moet niet worden verward met de opdracht `azds prep`, waarmee u het project configureert voor implementatie.

![](media/common/command-palette.png)


### <a name="select-the-azds-debug-configuration"></a>Selecteer de AZDS-foutopsporingsconfiguratie
1. Om de foutopsporingsweergave te openen, klikt u op het pictogram Foutopsporing in de **activiteitenbalk** van VS Code.
1. Selecteer **.NET Core starten (AZDS)** als de actieve foutopsporingsconfiguratie.

![](media/get-started-netcore/debug-configuration.png)

> [!Note]
> Als u geen Azure Dev Spaces-opdrachten ziet in het Opdrachtenpalet, controleert u of u de VS Code-extensie hebt geïnstalleerd voor Azure Dev Spaces. Zorg ervoor dat de werkruimte die u hebt geopend in VS Code de map is die azds.yaml bevat.


### <a name="debug-the-container-in-kubernetes"></a>Fouten opsporen in de container in Kubernetes
Druk op **F5** om fouten in uw code op te sporen in Kubernetes.

Net als bij de opdracht `up` wordt code gesynchroniseerd met de ontwikkelomgeving en wordt een container gemaakt en geïmplementeerd in Kubernetes. Op dit moment is het foutopsporingsprogramma uiteraard gekoppeld aan de externe container.

> [!Tip]
> De VS Code-statusbalk schakelt het oranje, waarmee wordt aangegeven dat het foutopsporingsprogramma is gekoppeld. Een geklikt URL, kunt u uw website te openen, worden ook weergegeven.

![](media/common/vscode-status-bar-url.png)

Stel een onderbrekingspunt in een codebestand aan serverzijde, bijvoorbeeld binnen de functie `About()` in het bronbestand `Controllers/HomeController.cs`. Als de browserpagina wordt vernieuwd, wordt het onderbrekingspunt bereikt.

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

Sla het bestand en in de **foutopsporing actiedeelvenster**, klikt u op de **opnieuw** knop. 

![](media/common/debug-action-refresh.png)

In plaats van telkens als codewijzigingen zijn aangebracht een nieuwe containerinstallatiekopie opnieuw te bouwen en opnieuw te implementeren, wat vaak behoorlijk wat tijd kost, hercompileert Azure Dev Spaces incrementeel code binnen de bestaande container voor een snellere bewerkings-/foutopsporingslus.

Vernieuw de web-app in de browser en ga naar de pagina Info. U ziet dat uw aangepaste bericht wordt weergegeven in de gebruikersinterface.

**U beschikt nu over een methode om code snel te ontwikkelen en foutopsporing rechtstreeks uit te voeren in Kubernetes.** Hierna ziet u hoe u een tweede container kunt maken en aanroepen.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Informatie over ontwikkelen van meerdere services](multi-service-netcore.md)


[supported-regions]: about.md#supported-regions-and-configurations
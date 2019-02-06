---
title: Teamontwikkeling voor Azure Dev Spaces met .NET Core en Visual Studio Team | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.custom: vs-azure
ms.workload: azure-vs
ms.subservice: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 07/09/2018
ms.topic: tutorial
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: ecacb7d3d4576b18eee3faf88c2a598d6acf94a0
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55465364"
---
# <a name="team-development-with-azure-dev-spaces"></a>Teamontwikkeling met Azure Dev Spaces

In deze zelfstudie leert u hoe u gelijktijdig kunt werken in verschillende ontwikkelomgevingen met behulp van meerdere ontwikkelruimten, om het werk gescheiden te houden in afzonderlijke ontwikkelruimten in hetzelfde cluster.

## <a name="call-another-container"></a>Een andere container aanroepen
In deze sectie maakt u een tweede service, `mywebapi`, en laat u deze aanroepen in `webfrontend`. Elke service wordt uitgevoerd in een afzonderlijke container. Vervolgens spoort u fouten op in beide containers.

![](media/common/multi-container.png)

### <a name="download-sample-code-for-mywebapi"></a>Voorbeeldcode voor *mywebapi* downloaden
Omwille van de tijd downloaden we voorbeeldcode uit een GitHub-opslagplaats. Ga naar https://github.com/Azure/dev-spaces en selecteer **Clone or Download** om de GitHub-opslagplaats te downloaden. De code voor deze sectie bevindt zich in `samples/dotnetcore/getting-started/mywebapi`.

### <a name="run-mywebapi"></a>*mywebapi* uitvoeren
1. Open het `mywebapi`-project in een *afzonderlijk Visual Studio-venster*.
1. Selecteer **Azure Dev Spaces** in de vervolgkeuzelijst met opstartinstellingen zoals u eerder hebt gedaan voor het `webfrontend`-project. In plaats van een nieuw AKS-cluster te maken, kunt u nu de ontwikkelomgeving selecteren die u al hebt gemaakt. Laat ook hier de standaard bij Space ingesteld op `default` en klik op **OK**. In het uitvoervenster ziet u mogelijk dat deze nieuwe service in de ontwikkelomgeving wordt ‘opgewarmd’ in Visual Studio om ervoor te zorgen dat het proces sneller verloopt wanneer u begint met de foutopsporing.
1. Druk op F5 en wacht tot de service is gebouwd en geïmplementeerd. Dit proces is voltooid, zodra de Visual Studio-statusbalk oranje wordt
1. Noteer de eindpunt-URL die wordt weergegeven in het deelvenster **Azure Dev Spaces for AKS** in het **uitvoervenster**. Deze ziet er ongeveer als volgt uit: http://localhost:\<portnumber\>. Het lijkt misschien alsof de container lokaal wordt uitgevoerd, maar dat is niet zo. De container wordt uitgevoerd in de ontwikkelomgeving in Azure.
2. Wanneer `mywebapi` klaar is, opent u de browser naar het localhost-adres en voegt u `/api/values` toe aan het eind van de URL om de standaard-GET API voor de `ValuesController` aan te roepen. 
3. Als alle stappen zijn voltooid, ziet u een reactie van de `mywebapi`-service die er ongeveer als volgt uitziet.

    ![](media/get-started-netcore-visualstudio/WebAPIResponse.png)

### <a name="make-a-request-from-webfrontend-to-mywebapi"></a>Verzend een aanvraag van *webfrontend* naar *mywebapi*
Nu gaan we code schrijven in `webfrontend` waarmee een aanvraag wordt verzonden naar `mywebapi`. Schakel over naar het Visual Studio-venster met het `webfrontend`-project. *Vervang* de code in het `HomeController.cs`-bestand voor de About-methode door de volgende code:

   ```csharp
   public async Task<IActionResult> About()
   {
      ViewData["Message"] = "Hello from webfrontend";

      using (var client = new System.Net.Http.HttpClient())
            {
                // Call *mywebapi*, and display its response in the page
                var request = new System.Net.Http.HttpRequestMessage();
                request.RequestUri = new Uri("http://mywebapi/api/values/1");
                if (this.Request.Headers.ContainsKey("azds-route-as"))
                {
                    // Propagate the dev space routing header
                    request.Headers.Add("azds-route-as", this.Request.Headers["azds-route-as"] as IEnumerable<string>);
                }
                var response = await client.SendAsync(request);
                ViewData["Message"] += " and " + await response.Content.ReadAsStringAsync();
            }

      return View();
   }
   ```

In het vorige codevoorbeeld wordt de `azds-route-as`-header van de binnenkomende aanvraag doorgestuurd naar de uitgaande aanvraag. Verderop ziet u hoe deze afgeleide klasse een productievere ontwikkeling in teamscenario's vergemakkelijkt.

### <a name="debug-across-multiple-services"></a>Foutopsporing in meerdere services
1. Op dit punt moet `mywebapi` nog steeds worden uitgevoerd met het bijgevoegde foutopsporingsprogramma. Als dit niet het geval is, drukt u op F5 in het `mywebapi`-project.
1. Stel een onderbrekingspunt in de `Get(int id)`-methode in het `Controllers/ValuesController.cs`-bestand in waarmee `api/values/{id}` GET-aanvragen worden verwerkt.
1. Stel in het `webfrontend`-project waarin u de bovenstaande code hebt geplakt, een onderbrekingspunt in vlak voordat een GET-aanvraag wordt verzonden naar `mywebapi/api/values`.
1. Druk op F5 in het `webfrontend`-project. In Visual Studio wordt opnieuw een browser geopend op de juiste localhost-poort en de web-app wordt weergegeven.
1. Klik boven aan de pagina op de koppeling **About** om het onderbrekingspunt te activeren in het `webfrontend`-project. 
1. Druk F10 om door te gaan. Het onderbrekingspunt in het `mywebapi`-project wordt nu geactiveerd.
1. Druk op F5 om door te gaan. U wordt teruggeleid naar de code in het `webfrontend`-project.
1. Als u nog een keer op F5 drukt, wordt de aanvraag voltooid en gaat u terug naar een pagina in de browser. In de web-app wordt nu op de pagina About een bericht weergegeven dat is samengesteld uit de twee services: "Hello from webfrontend and Hello from mywebapi."

Dat is dus gelukt. U hebt nu een toepassing met meerdere containers waarin elke container afzonderlijk kan worden ontwikkeld en geïmplementeerd.

## <a name="learn-about-team-development"></a>Meer informatie over teamontwikkeling

Tot nu toe hebt u de code van de toepassing uitgevoerd alsof u de enige ontwikkelaar bent die werkt aan de app. In deze sectie leert u hoe teamontwikkeling wordt gestroomlijnd met Azure Dev Spaces:
* Laat een team ontwikkelaars in dezelfde omgeving werken door ze naar behoefte in een gedeelde ontwikkelruimte of in individuele ontwikkelruimten te laten werken.
* Biedt ondersteuning om alle ontwikkelaars hun code geïsoleerd te laten herhalen, zonder dat ze het gevaar lopen dat ze andermans code breken.
* Test code end-to-end, voordat u de code doorvoert, zonder nagebootste code te maken of afhankelijkheden te simuleren.

### <a name="challenges-with-developing-microservices"></a>Uitdagingen bij het ontwikkelen van microservices
Momenteel is uw voorbeeldcode niet erg complex. Maar in de pratijk krijgt u bij het ontwikkelen al snel te maken met uitdagingen wanneer u meer services toevoegt en het ontwikkelteam groeit.

Stel, bijvoorbeeld, dat u werkt aan een service die werkt met een heel aantal andere services.

- Alles lokaal uitvoeren voor ontwikkeling is dan misschien niet zo realistisch. Uw ontwikkelmachine heeft mogelijk niet genoeg resources om de hele app uit te voeren. Of misschien heeft uw app eindpunten die openbaar bereikbaar moeten zijn (bijvoorbeeld als de app reageert op een webhook vanuit een SaaS-app).
- U kunt proberen om alleen de services uit te voeren waarvan u afhankelijk bent, maar dit betekent dat u bekend moet zijn met de volledige sluiting van afhankelijkheden (bijvoorbeeld de afhankelijkheden van afhankelijkheden). Of misschien weet u zo gauw niet hoe u de afhankelijkheden bouwt en uitvoert omdat u niet met ze hebt gewerkt.
- Sommige ontwikkelaars maken gebruik van het simuleren of nabootsen van veel van hun serviceafhankelijkheden. Dit kan soms helpen, maar het beheren van deze nabootsingen brengt ook al snel uitdagingen op het gebied van ontwikkelen met zich mee. Bovendien ziet uw ontwikkelomgeving er dan anders uit dan de productieomgeving, waardoor subtiele fouten kunnen optreden.
- Een direct gevolg is dat end-to-endtesten moeilijk wordt. De integratie kan alleen realistisch worden getest na een doorvoering, wat betekent dat problemen pas later in de ontwikkelcyclus zichtbaar worden.

    ![](media/common/microservices-challenges.png)

### <a name="work-in-a-shared-dev-space"></a>In een gedeelde ontwikkelruimte werken
Met Azure Dev Spaces kunt u een *gedeelde* ontwikkelruimte instellen in Azure. Elke ontwikkelaar kan zich richten op het eigen deel van de toepassing, en kan *vooraf doorgevoerde code* iteratief doorvoeren in een ontwikkelruimte die alle andere services en cloudresources al bevat waarvan de scenario’s afhankelijk zijn. Afhankelijkheden zijn altijd up-to-date en ontwikkelaars werken op een manier die de productie weerspiegelt.

### <a name="work-in-your-own-space"></a>Werken in uw eigen ruimte
Terwijl u code ontwikkelt voor uw service en voordat u klaar bent om deze uit te geven, is de codestatus meestal niet in orde. U bent de code nog steeds iteratief aan het vormen en testen, en aan het experimenten met oplossingen. Azure Dev Spaces biedt het concept van een **ruimte**, waardoor u geïsoleerd kunt werken, zonder dat u de code van teamleden per ongeluk breekt.

Doe het volgende om ervoor te zorgen dat uw `webfrontend`- en `mywebapi`-services beide worden uitgevoerd **in de `default` ontwikkelomgeving**.
1. Sluit alle F5-foutopsporingssessies voor beide services, maar houd de projecten open in de bijbehorende Visual Studio-vensters.
2. Schakel over naar het Visual Studio-venster met het `mywebapi`-project en druk op Ctrl+F5 om de service uit te voeren zonder het bijgevoegde foutopsporingsprogramma
3. Schakel over naar het Visual Studio-venster met het `webfrontend`-project en druk op Ctrl+F5 om dit ook uit te voeren.

> [!Note]
> Soms is het noodzakelijk om de browser te vernieuwen nadat de webpagina de eerste keer wordt uitgevoerd na Ctrl+F5.

Iedereen die de openbare URL opent en naar de web-app navigeert, roept het codepad aan dat u hebt geschreven en dat actief is in beide services met behulp van de `default`-standaardruimte. Stel nu dat u wilt verdergaan met het ontwikkelen van `mywebapi`. Hoe kunt u dit doen zonder andere ontwikkelaars te onderbreken die de ontwikkelomgeving ook gebruiken? Hiervoor moet u uw eigen ruimte instellen.

### <a name="create-a-new-dev-space"></a>Een nieuwe ontwikkelomgeving maken
U kunt vanuit Visual Studio extra ruimten maken die worden gebruikt wanneer u F5 of Ctrl+F5 gebruikt voor de service. U kunt een ruimte elke gewenste naam geven en u kunt vrij bepalen wat deze betekent (bijvoorbeeld `sprint4` of `demo`).

Doe het volgende om een nieuwe ruimte te maken:
1. Schakel over naar het Visual Studio-venster met het `mywebapi`-project.
2. Klik in **Solution Explorer** met de rechtermuisknop op het project en selecteer **Properties**.
3. Selecteer aan de linkerkant het tabblad **Debug** om de Azure Dev Spaces-instellingen weer te geven.
4. Van hieruit kunt u het cluster en/of de ruimte maken die wordt gebruikt wanneer u F5 of Ctrl+F5 gebruikt. *Zorg ervoor dat de Azure Dev-ruimte is geselecteerd die u eerder hebt gemaakt*.
5. Selecteer **<Nieuwe ruimte maken...>** in de vervolgkeuze lijst met ruimtes.

    ![](media/get-started-netcore-visualstudio/Settings.png)

6. Stel in het dialoogvenster **Ruimte toevoegen** de bovenliggende ruimte in op **standaard** en voer een naam in voor de nieuwe ruimte. U kunt uw naam gebruiken voor de nieuwe ruimte (bijvoorbeeld Scott), zodat het duidelijk is dat dit de ruimte is waarin u werkt. Klik op **OK**.

    ![](media/get-started-netcore-visualstudio/AddSpace.png)

7. Nu zijn uw AKS-cluster en Nieuwe ruimte geselecteerd op de pagina met projecteigenschappen.

    ![](media/get-started-netcore-visualstudio/Settings2.png)

### <a name="update-code-for-mywebapi"></a>Voorbeeldcode voor *mywebapi* bijwerken

1. Maak in het `mywebapi`-project een codewijziging in de `string Get(int id)`-methode in het bestand `Controllers/ValuesController.cs`. Dit doet u als volgt:
 
    ```csharp
    [HttpGet("{id}")]
    public string Get(int id)
    {
        return "mywebapi now says something new";
    }
    ```

2. Stel een onderbrekingspunt in dit bijgewerkte codeblok in (misschien hebt u al eerder een onderbrekingspunt ingesteld).
3. Druk op F5 om de service `mywebapi` te starten. Hiermee wordt de service in het cluster gestart met de geselecteerde ruimte. In dit geval is dat `scott`.

Hier ziet u een diagram waarin u de werking van de verschillende ruimtes kunt zien. Het paarse pad laat een aanvraag zien via de `default`-ruimte. Dit is het standaardpad dat wordt gebruikt als er geen ruimte is toegevoegd aan de URL. Het roze pad laat een aanvraag zien via de `default/scott`-ruimte.

![](media/common/Space-Routing.png)

Deze ingebouwde mogelijkheid van Azure Dev Spaces stelt u in staat om code end-to-end te testen in een gedeelde omgeving zonder dat ontwikkelaars de volledige groep services in hun ruimte opnieuw hoeven te maken. Voor deze routering moeten doorgifteheaders worden doorgestuurd in uw app-code, zoals uitgelegd in de vorige stap van deze handleiding.

### <a name="test-code-running-in-the-defaultscott-space"></a>Code testen die wordt uitgevoerd in de `default/scott`-ruimte
Als u de nieuwe versie van `mywebapi` wilt testen samen met `webfrontend`, opent u de URL van het openbare toegangspunt voor `webfrontend` in de browser (bijvoorbeeld http://webfrontend.123456abcdef.eastus.aksapp.io)) en gaat u naar de pagina About. Als het goed is, ziet u nu het oorspronkelijke bericht: Hello from webfrontend and Hello from mywebapi.

Voeg nu het gedeelte scott.s. toe aan de URL, zodat deze er ongeveer als volgt uitziet: http://scott.s.webfrontend.123456abcdef.eastus.aksapp.io. Vernieuw vervolgens de browser. Het onderbrekingspunt dat u hebt ingesteld in het `mywebapi`-project, wordt nu gevonden. Klik op F5 om door te gaan. In de browser ziet u nu het nieuwe bericht: Hello from webfrontend and mywebapi now says something new. Dit komt omdat het pad naar de bijgewerkte code in `mywebapi` wordt uitgevoerd in de `default/scott`-ruimte.

### <a name="well-done"></a>Dat is dus gelukt.
U hebt de introductiehandleiding voltooid! U hebt geleerd hoe u:

> [!div class="checklist"]
> * Azure Dev Spaces instellen met een beheerd Kubernetes-cluster in Azure.
> * Iteratief code ontwikkelen in containers.
> * Twee afzonderlijke services ontwikkelen en de DNS-servicedetectie van Kubernetes gebruiken om een andere service aan te roepen.
> * Uw code op een productieve manier ontwikkelen en testen in een teamomgeving.

Nu u Azure Dev Spaces hebt verkend, kunt u [uw dev-ruimte delen met teamleden](how-to/share-dev-spaces.md) en hen laten zien hoe eenvoudig het is om samen te werken.

## <a name="clean-up"></a>Opruimen
Als u een exemplaar van Azure Dev Spaces in een cluster volledig wilt verwijderen, waaronder alle ontwikkelruimtes en de actieve services die erin worden uitgevoerd, gebruikt u de opdracht `az aks remove-dev-spaces`. Houd er rekening mee dat deze actie niet kan worden teruggedraaid. U kunt later opnieuw ondersteuning voor Azure Dev Spaces toevoegen aan het cluster, maar u moet dan helemaal opnieuw beginnen. Uw oude services en ruimtes worden niet hersteld.

In het volgende voorbeeld worden de Azure Dev Spaces-controllers in uw actieve abonnement vermeld en wordt vervolgens de Azure Dev Spaces-controller die is gekoppeld aan het AKS-cluster 'myaks' in de resourcegroep 'myaks-rg' verwijderd.

```cmd
    azds controller list
    az aks remove-dev-spaces --name myaks --resource-group myaks-rg
```

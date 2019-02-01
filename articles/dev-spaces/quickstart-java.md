---
title: Een Kubernetes-dev-ruimte in de cloud maken | Microsoft Docs
titleSuffix: Azure Dev Spaces
author: stepro
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
ms.author: stephpr
ms.date: 09/26/2018
ms.topic: quickstart
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: mmontwil
ms.openlocfilehash: c448521d7e1ce70a44ca23f58d0034ca2041cbed
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55453427"
---
# <a name="quickstart-create-a-kubernetes-dev-space-with-azure-dev-spaces-java-and-vs-code"></a>Snelstartgids: Een Kubernetes-ontwikkelomgeving maken met Azure Dev Spaces (Java en VS Code)

In deze handleiding leert u het volgende:

- Azure Dev Spaces instellen met een beheerd Kubernetes-cluster in Azure.
- Iteratief code ontwikkelt in containers met behulp van VS Code en de opdrachtregel.
- Spoor fouten in de code van uw ontwikkelomgeving op met VS Code.

> [!Note]
> **Als u op enig moment niet verder kunt**, kunt u de [probleemoplossingssectie](troubleshooting.md) raadplegen of een opmerking op deze pagina plaatsen. U kunt ook de meer gedetailleerde [zelfstudie](get-started-netcore.md) proberen.

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen account hebt, kunt u [een gratis account aanmaken](https://azure.microsoft.com/free).
- [Visual Studio Code](https://code.visualstudio.com/download).
- [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) versie 2.0.43 of hoger.
- Een Kubernetes-cluster met Kubernetes 1.10.3 of hoger in de regio EastUS, EastUS2, CentralUS, WestUS2, WestEurope, SoutheastAsia, CanadaCentral of CanadaEast met **Routering van HTTP-toepassing** ingeschakeld.

    ```cmd
    az group create --name MyResourceGroup --location <region>
    az aks create -g MyResourceGroup -n myAKS --location <region> --kubernetes-version 1.10.9 --enable-addons http_application_routing --generate-ssh-keys
    ```

## <a name="set-up-azure-dev-spaces"></a>Azure Dev Spaces instellen

1. Dev Spaces instellen op uw AKS-cluster: `az aks use-dev-spaces -g MyResourceGroup -n MyAKS`
1. Download de extensie [Azure Dev Spaces ](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds) voor VS Code. Klik eenmaal op Installeren op de Microsoft Azure Marketplace-pagina van de extensie en nog eens in VS Code.
1. Download de extensie [Java Debugger voor Azure Dev Spaces ](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debugger-azds) voor VS Code. Klik eenmaal op Installeren op de Microsoft Azure Marketplace-pagina van de extensie en nog eens in VS Code.

## <a name="build-and-run-code-in-kubernetes"></a>Code schrijven en uitvoeren in Kubernetes

1. Voorbeeldcode van GitHub downloaden: [https://github.com/Azure/dev-spaces](https://github.com/Azure/dev-spaces) 
1. De map in de map webfrontend wijzigen: `cd dev-spaces/samples/java/getting-started/webfrontend`
1. Activa voor Docker- en Helm-grafieken genereren: `azds prep --public`
1. Compileer de code in AKS en voer deze uit. Voer in het terminalvenster de volgende opdracht uit vanuit de **map webfrontend**: `azds up`
1. Scan de console-uitvoer voor informatie over de URL die door de opdracht `up` is gemaakt. Het zal in deze vorm te zien zijn:

   ```output
    (pending registration) Service 'webfrontend' port 'http' will be available at <url>
    Service 'webfrontend' port 80 (TCP) is available at http://localhost:<port>
   ```

   Open deze URL in een browservenster. Dan ziet u dat de web-app wordt geladen.

   > [!Note]
   > Bij de eerste uitvoering kan het enkele minuten duren voordat de openbare DNS gereed is. Als de openbare URL niet wordt omgezet, kunt u de alternatieve URL http://localhost:<portnumber> gebruiken die wordt weergegeven in de console-uitvoer. Als u de localhost-URL gebruikt, lijkt het misschien alsof de container lokaal wordt uitgevoerd, maar wordt deze feitelijk uitgevoerd in AKS. Voor uw gemak en om interactie met de service mogelijk te maken vanaf de lokale computer, wordt in Azure Dev Spaces een tijdelijke SSH-tunnel gemaakt naar de container die wordt uitgevoerd in Azure. U kunt later terugkomen en de openbare URL proberen wanneer de DNS-record gereed is.

### <a name="update-a-code-file"></a>Een codebestand bijwerken

1. Druk in het terminalvenster op `Ctrl+C` (om `azds up` te stoppen).
1. Open het codebestand met de naam `src/main/java/com/ms/sample/webfrontend/Application.java` en bewerk het welkomstbericht: `return "Hello from webfrontend in Azure!";`
1. Sla het bestand op.
1. Voer `azds up` uit in het terminalvenster.

Met deze opdracht wordt de installatiekopie van de container opnieuw gebouwd en het Helm-diagram opnieuw geïmplementeerd. Vernieuw de browser om te zien of uw codewijzigingen zijn doorgevoerd in de actieve toepassing.

Er bestaat echter een nog *snellere methode* voor het ontwikkelen van code. Deze methode gaat u in de volgende sectie verkennen.

## <a name="debug-a-container-in-kubernetes"></a>Fouten opsporen in een Kubernetes-container

In deze sectie gaat u VS Code gebruiken om direct fouten op te sporen in de container die in Azure wordt uitgevoerd. U leert ook hoe u een cyclus voor bewerken-uitvoeren-testen sneller kunt uitvoeren.

![](./media/common/edit-refresh-see.png)

### <a name="initialize-debug-assets-with-the-vs-code-extension"></a>Foutopsporingsassets initialiseren met de VS Code-extensie
U moet eerst uw codeproject configureren, zodat VS Code met de ontwikkelomgeving in Azure gaat communiceren. De VS Code-extensie voor Azure Dev Spaces bevat een helpopdracht om de foutopsporingsconfiguratie in te stellen.

Open het **Opdrachtenpalet** (via het menu **Beeld | Opdrachtenpalet**), en gebruik automatisch aanvullen om te typen en deze opdracht te selecteren: `Azure Dev Spaces: Prepare configuration files for Azure Dev Spaces`.

Hiermee wordt de foutopsporingsconfiguratie voor Azure Dev Spaces toegevoegd onder de map `.vscode`.

![](./media/common/command-palette.png)

### <a name="select-the-azds-debug-configuration"></a>Selecteer de AZDS-foutopsporingsconfiguratie
1. Om de foutopsporingsweergave te openen, klikt u op het pictogram Foutopsporing in de **activiteitenbalk** van VS Code.
1. Selecteer **Launch Java Program (AZDS)** als de actieve foutopsporingsconfiguratie.

![](media/get-started-java/debug-configuration.png)

> [!Note]
> Als u geen Azure Dev Spaces-opdrachten ziet in het Opdrachtenpalet, controleert u of u de VS Code-extensie hebt geïnstalleerd voor Azure Dev Spaces. Zorg ervoor dat de werkruimte die u hebt geopend in VS Code de map is die azds.yaml bevat.

### <a name="debug-the-container-in-kubernetes"></a>Fouten opsporen in de container in Kubernetes
Druk op **F5** om fouten in uw code op te sporen in Kubernetes.

Net als bij de opdracht `up` wordt code gesynchroniseerd met de ontwikkelomgeving en wordt een container gemaakt en geïmplementeerd in Kubernetes. Op dit moment is het foutopsporingsprogramma uiteraard gekoppeld aan de externe container.

> [!Tip]
> Op de VS Code-statusbalk wordt een klikbare URL weergegeven.

Stel een onderbrekingspunt in een codebestand aan serverzijde, bijvoorbeeld binnen de functie `greeting()` in het bronbestand `src/main/java/com/ms/sample/webfrontend/Application.java`. Als de browserpagina wordt vernieuwd, wordt het onderbrekingspunt bereikt.

U hebt volledige toegang tot foutopsporingsgegevens, net alsof de code lokaal wordt uitgevoerd. Denk hierbij aan de aanroep-stack, lokale variabelen en informatie over uitzonderingen, enzovoort.

### <a name="edit-code-and-refresh"></a>Code bewerken en vernieuwen
Maak een codewijziging terwijl het foutopsporingsprogramma actief is. Wijzig de begroeting bijvoorbeeld in `src/main/java/com/ms/sample/webfrontend/Application.java`. 

```java
public String greeting()
{
    return "I'm debugging Java code in Azure!";
}
```

Sla het bestand op en klik in het deelvenster **Debug actions** op de knop **Refresh**. 

![](media/get-started-java/debug-action-refresh.png)

In plaats van telkens als codewijzigingen zijn aangebracht een nieuwe containerinstallatiekopie opnieuw te bouwen en opnieuw te implementeren, wat vaak behoorlijk wat tijd kost, hercompileert Azure Dev Spaces incrementeel code binnen de bestaande container voor een snellere bewerkings-/foutopsporingslus.

Vernieuw de web-app in de browser. U ziet dat uw aangepaste bericht wordt weergegeven in de gebruikersinterface.

**U beschikt nu over een methode om code snel te ontwikkelen en foutopsporing rechtstreeks uit te voeren in Kubernetes.**

## <a name="next-steps"></a>Volgende stappen

Ontdek hoe Azure Dev Spaces u kan helpen om complexere apps te ontwikkelen binnen meerdere containers en hoe u ontwikkeling in samenwerkingsverband vereenvoudigt door in verschillende ruimten met verschillende versies of vertakkingen van uw code te werken.

> [!div class="nextstepaction"]
> [Werken met meerdere containers en teamontwikkeling](team-development-java.md)

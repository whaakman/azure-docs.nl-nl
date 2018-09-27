---
title: Oplossen van problemen | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: ghogen
ms.author: ghogen
ms.date: 09/11/2018
ms.topic: article
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
manager: douge
ms.openlocfilehash: 91bec065b2c83eac6b646ae6a55bc1ae0aae01db
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47226888"
---
# <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen

Deze handleiding bevat informatie over veelvoorkomende problemen die mogelijk hebt u bij het gebruik van Azure Dev spaties.

## <a name="enabling-detailed-logging"></a>Gedetailleerde logboekregistratie inschakelen

Om te kunnen oplossen van problemen effectiever, mogelijk kunt u meer gedetailleerde logboeken voor controle maken.

Voor de extensie voor Visual Studio, stelt u de `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` omgevingsvariabele op 1. Zorg ervoor dat opnieuw opstarten van Visual Studio voor de omgeving in te voeren. Eenmaal is ingeschakeld, gedetailleerde logboeken worden geschreven naar uw `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` directory.

In de CLI, kunt u meer informatie tijdens de uitvoering van de opdracht uitvoeren met behulp van de `--verbose` overschakelen. U kunt ook meer gedetailleerde logboeken in Bladeren `%TEMP%\Azure Dev Spaces`. De map TEMP op een Mac te vinden door te voeren `echo $TMPDIR` vanuit een terminal-venster. Op een Linux-computer, de map TEMP is meestal `/tmp`.

## <a name="debugging-services-with-multiple-instances"></a>Foutopsporing van services met meerdere exemplaren

Op dit moment werkt Azure Dev spaties het beste bij het opsporen van fouten in een enkele instantie (schil). Het bestand azds.yaml bevat een replicaCount, die aangeeft van het aantal schillen die wordt uitgevoerd voor uw service-instelling. Als u de replicaCount voor het configureren van uw app voor het uitvoeren van meerdere schillen zijn voor een bepaalde service wijzigt, wordt het foutopsporingsprogramma koppelen aan de eerste schil (wanneer alfabetisch). Als deze pod wordt gerecycled voor een bepaalde reden, het foutopsporingsprogramma zal worden gekoppeld aan een andere schil, wat mogelijk resulteert in onverwacht gedrag.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>Fout 'Failed to Azure Dev spaties controller maken'

U kunt deze fout tegenkomen wanneer er iets met het maken van de controller misgaat. Als het een tijdelijke fout, wordt verwijderen en opnieuw maken van de controller eraan.

### <a name="try"></a>Probeer:

Als u wilt verwijderen van de controller, gebruikt u Azure Dev spaties CLI. Het is niet mogelijk om dat te doen in Visual Studio of Cloud Shell. AZDS CLI installeren, installeert eerst de Azure CLI en voer vervolgens deze opdracht uit:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

En voer vervolgens deze opdracht voor het verwijderen van de controller:

```cmd
azds remove -g <resource group name> -n <cluster name>
```

De controller opnieuw kan worden gedaan in de CLI of Visual Studio. Volg de instructies in de zelfstudies als voor de eerste keer starten.


## <a name="error-service-cannot-be-started"></a>Fout 'Service kan niet worden gestart.'

U kunt deze fout tegenkomen wanneer de servicecode van uw niet kan worden gestart. De oorzaak is vaak in de gebruikerscode. Als u meer diagnostische gegevens, moet u de volgende wijzigingen aanbrengen aan uw opdrachten en -instellingen:

### <a name="try"></a>Probeer:

Op de opdrachtregel:

Bij het gebruik van _azds.exe_, gebruikt u de--uitgebreide opdrachtregeloptie te gebruiken, en gebruik de--output opdrachtregeloptie te gebruiken om op te geven van de indeling van de uitvoer.
 
    ```cmd
    azds up --verbose --output json
    ```

In Visual Studio:

1. Open **Extra > opties** en klikt u onder **projecten en oplossingen**, kiest u **bouwen en uitvoeren**.
2. Wijzig de instellingen voor **MSBuild-project build uitvoer uitgebreidheid** naar **gedetailleerd** of **diagnostische**.

    ![Schermafbeelding van de opties in menu Extra](media/common/VerbositySetting.PNG)
    
## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>DNS-naamomzetting voor een openbare URL die is gekoppeld aan een Dev-Spaces-service is mislukt

Wanneer DNS-naamomzetting is mislukt, ziet u mogelijk een 'Pagina kan niet worden weergegeven' of "deze site kan niet worden bereikt" fout in uw webbrowser wanneer er wordt geprobeerd verbinding maken met de openbare URL die is gekoppeld aan een service Dev spaties.

### <a name="try"></a>Probeer:

U kunt de volgende opdracht uit om alle URL's die zijn gekoppeld aan uw opslagruimten Dev-services:

```cmd
azds list-uris
```

Als een URL in de *in behandeling* staat, betekent dit dat Dev spaties nog voor DNS-registratie te voltooien. Soms duurt een paar minuten voor de registratie te voltooien. Dev opslagruimten wordt ook geopend wanneer u een tunnel ' localhost ' voor elke service, die u gebruiken kunt tijdens het wachten op DNS-registratie.

Als een URL in blijft de *in behandeling* status voor meer dan 5 minuten, kan dit wijzen op een probleem met de externe DNS-schil die het openbare eindpunt worden gemaakt en/of de schil nginx ingress-controller die krijgt van het openbare eindpunt. U kunt de volgende opdrachten gebruiken om te verwijderen van deze schillen. Ze zullen automatisch worden gemaakt.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

## <a name="error-required-tools-and-configurations-are-missing"></a>Fout 'Vereiste hulpprogramma's en configuraties ontbreken'

Deze fout kan optreden bij het starten van VS Code: "[Azure Dev spaties] vereiste hulpprogramma's en configuraties om te bouwen en fouten opsporen in '[naam project]' ontbreken."
De fout betekent dat azds.exe bevindt zich niet in de omgevingsvariabele PATH, zoals te zien is in VS Code.

### <a name="try"></a>Probeer:

Start VS Code vanaf een opdrachtprompt waarvan de omgevingsvariabele PATH correct is ingesteld.

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>Fout 'azds' wordt niet herkend als een interne of externe opdracht, programma of batch-bestand
 
U ziet deze fout als azds.exe is niet geïnstalleerd of niet juist geconfigureerd.

### <a name="try"></a>Probeer:

1. Controleer de locatie-%ProgramFiles%/Microsoft SDKs\Azure\Azure Dev spaties CLI (Preview) voor azds.exe. Als deze aanwezig is, moet u die locatie toevoegen aan de omgevingsvariabele PATH.
2. Als azds.exe niet is geïnstalleerd, moet u de volgende opdracht uitvoeren:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Waarschuwing 'docker-bestand kan niet worden gegenereerd vanwege een niet-ondersteunde taal '
Azure Dev opslagruimten biedt systeemeigen ondersteuning voor C# en Node.js. Bij het uitvoeren van *azds prep* in een map met code die is geschreven in een van deze talen, Azure Dev opslagruimten maakt automatisch een docker-bestand dat de juiste voor u.

U kunt nog steeds gebruik van Azure Dev spaties met code die in andere talen zijn geschreven, maar moet u de docker-bestand zelf maken vóór actief *azds van* voor de eerste keer.

### <a name="try"></a>Probeer:
Als uw toepassing is geschreven in een taal dat Azure Dev spaties geen systeemeigen ondersteuning biedt, moet u voor een juiste docker-bestand voor het bouwen van een containerinstallatiekopie uitvoeren van uw code. Docker biedt een [lijst met aanbevolen procedures voor het schrijven van docker-bestanden](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) , evenals een [Dockerfile-referentiemateriaal](https://docs.docker.com/engine/reference/builder/) die kunnen helpen bij het schrijven van een docker-bestand dat aansluit op uw behoeften.

Wanneer u een juiste Dockerfile geïmplementeerd hebt, kunt u doorgaan met uitvoeren *azds van* voor het uitvoeren van uw toepassing in Azure Dev spaties.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Fout 'upstream verbindingsfout of verbreken/reset voordat headers'
Mogelijk ziet u deze fout bij het openen van uw service. Bijvoorbeeld, wanneer u gaat naar de URL van de service in een browser. 

### <a name="reason"></a>Reden 
De poort van de container is niet beschikbaar. Dit probleem kan optreden omdat: 
* De container wordt nog steeds worden gebouwd en geïmplementeerd. Dit probleem kan zich voordoen als u uitvoeren `azds up` of het foutopsporingsprogramma start en vervolgens probeert te krijgen tot de container voordat deze is geïmplementeerd.
* Poortconfiguratie van de is niet consistent zijn tussen uw _Dockerfile_, Helm-diagram en servercode die wordt een poort geopend.

### <a name="try"></a>Probeer:
1. Als de container gebouwd wordt/geïmplementeerd, kunt u wacht 2-3 seconden en probeer het opnieuw openen van de service. 
1. Controleer de poortconfiguratie. De opgegeven poortnummers moet **identieke** in de volgende elementen:
    * **Docker-bestand:** opgegeven door de `EXPOSE` instructie.
    * **[Helm-diagram](https://docs.helm.sh):** opgegeven door de `externalPort` en `internalPort` waarden voor een service (vaak zich in een `values.yml` bestand),
    * Eventuele poorten die worden geopend in de toepassingscode, bijvoorbeeld in Node.js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Configuratiebestand niet gevonden
U hebt uitgevoerd `azds up` en het volgende foutbericht weergegeven: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Reden
U moet uitvoeren `azds up` vanuit de hoofdmap van de code die u wilt uitvoeren, en moet u de code-map om uit te voeren met Azure Dev spaties initialiseren.

### <a name="try"></a>Probeer:
1. Wijzig de huidige directory naar de hoofdmap met de servicecode van uw. 
1. Als u nog geen een _azds.yaml_ bestand in de map code uitvoeren `azds prep` voor het genereren van Docker, Kubernetes en Azure Dev spaties activa.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Fout: 'de pipe programma azds onverwacht afgesloten met code 126.'
Starten van het foutopsporingsprogramma VS Code kan soms leiden tot deze fout.

### <a name="try"></a>Probeer:
1. Sluiten en opnieuw openen van VS Code.
2. Druk nogmaals op F5.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>Fout 'Failed foutopsporingsprogramma vinden voor het type: coreclr'-foutopsporing
Het foutopsporingsprogramma VS Code wordt uitgevoerd, rapporteert de fout: `Failed to find debugger extension for type:coreclr.`

### <a name="reason"></a>Reden
U hebt niet de VS Code-extensie voor C# geïnstalleerd op uw ontwikkelcomputer. De C#-extensie bevat foutopsporing ondersteuning voor .net Core (CoreCLR).

### <a name="try"></a>Probeer:
Installeer de [VS Code-extensie voor C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Fout bij foutopsporing 'geconfigureerd foutopsporing type 'coreclr' wordt niet ondersteund'
Het foutopsporingsprogramma VS Code wordt uitgevoerd, rapporteert de fout: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Reden
U hebt niet de VS Code-extensie voor Azure Dev opslagruimten geïnstalleerd op uw ontwikkelcomputer.

### <a name="try"></a>Probeer:
Installeer de [VS Code-extensie voor Azure Dev spaties](get-started-netcore.md).

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>Het type of de naam 'MijnBibliotheek' kan niet worden gevonden.

### <a name="reason"></a>Reden 
De build-context is op het niveau van het project/service standaard, wordt niet dus een bibliotheek-project dat u gevonden.

### <a name="try"></a>Probeer:
Wat er moet gebeuren:
1. Wijzig de _azds.yaml_ bestand in te stellen van de build-context op het niveau van de oplossing.
2. Wijzig de _Dockerfile_ en _Dockerfile.develop_ bestanden om te verwijzen naar het project (_.csproj_) bestanden correct ten opzichte van de nieuwe bouwen context.
3. Plaats een _.dockerignore_ bestand naast het SLN-bestand en wijzig zo nodig.

U vindt een voorbeeld op https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>'Microsoft.DevSpaces/register/action' Autorisatiefout
U kunt de volgende fout tegenkomen wanneer u een Dev-adresruimte van Azure beheert en u werkt in een Azure-abonnement waarvoor u geen hebt eigenaar of bijdrager toegang.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Reden
Het geselecteerde Azure-abonnement is niet geregistreerd. de `Microsoft.DevSpaces` naamruimte.

### <a name="try"></a>Probeer:
Iemand met de eigenaar of bijdrager toegang tot het Azure-abonnement kan worden uitgevoerd de volgende Azure CLI-opdracht om handmatig te registreren de `Microsoft.DevSpaces` naamruimte:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev spaties lijkt niet te gebruiken van mijn bestaande docker-bestand voor het bouwen van een container 

### <a name="reason"></a>Reden
Azure Dev opslagruimten kunnen worden geconfigureerd om te verwijzen naar een specifieke _Dockerfile_ in uw project. Als deze wordt weergegeven met het Azure Dev opslagruimten wordt niet met behulp van de _Dockerfile_ u verwacht te maken van uw containers, moet u mogelijk u vertelt Azure Dev ruimten waar ze zich expliciet. 

### <a name="try"></a>Probeer:
Open de _azds.yaml_ -bestand dat is gegenereerd door Azure Dev spaties in uw project. Gebruik de `configurations->develop->build->dockerfile` richtlijn om te verwijzen naar de Dockerfile die u wilt gebruiken:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```

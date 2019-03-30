---
title: Problemen oplossen
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/11/2018
ms.topic: conceptual
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s '
ms.openlocfilehash: 5dd77d85e06a821d8dd359174bb5de6bca8b4d61
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58669773"
---
# <a name="troubleshooting-guide"></a>Handleiding voor het oplossen van problemen

Deze handleiding bevat informatie over veelvoorkomende problemen die mogelijk hebt u bij het gebruik van Azure Dev spaties.

Als u een probleem opgetreden hebt bij het gebruik van Azure Dev spaties, maakt u een [probleem in de Azure Dev spaties GitHub-opslagplaats](https://github.com/Azure/dev-spaces/issues).

## <a name="enabling-detailed-logging"></a>Gedetailleerde logboekregistratie inschakelen

Voor het oplossen van problemen effectiever, mogelijk kunt u meer gedetailleerde logboeken voor controle maken.

Voor de extensie voor Visual Studio, stelt u de `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` omgevingsvariabele op 1. Zorg ervoor dat opnieuw opstarten van Visual Studio voor de omgeving in te voeren. Eenmaal is ingeschakeld, gedetailleerde logboeken worden geschreven naar uw `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` directory.

In de CLI, kunt u meer informatie tijdens de uitvoering van de opdracht uitvoeren met behulp van de `--verbose` overschakelen. U kunt ook meer gedetailleerde logboeken in Bladeren `%TEMP%\Azure Dev Spaces`. De map TEMP op een Mac te vinden door te voeren `echo $TMPDIR` vanuit een terminal-venster. Op een Linux-computer, de map TEMP is meestal `/tmp`.

## <a name="debugging-services-with-multiple-instances"></a>Foutopsporing van services met meerdere exemplaren

Op dit moment werkt Azure Dev spaties aanbevolen bij het opsporen van fouten in één exemplaar of schil. Het bestand azds.yaml bevat een instelling *replicaCount*, die aangeeft dat het aantal schillen die Kubernetes voor uw service wordt uitgevoerd. Als u de replicaCount voor het configureren van uw app voor het uitvoeren van meerdere schillen zijn voor een bepaalde service wijzigt, wordt het foutopsporingsprogramma gekoppeld aan de eerste schil wanneer op alfabetische volgorde weergegeven. Het foutopsporingsprogramma koppelt aan een andere schil wanneer de oorspronkelijke schil wordt gerecycled, wat mogelijk resulteert in onverwacht gedrag.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>Fout 'Failed to Azure Dev spaties controller maken'

U kunt deze fout tegenkomen wanneer er iets met het maken van de controller misgaat. Als het een tijdelijke fout, verwijderen en opnieuw maken van de controller om dit te corrigeren.

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
    
### <a name="multi-stage-dockerfiles"></a>Meerdere fasen docker-bestanden:
U ontvangt een *Service kan niet worden gestart* fout bij het gebruik van een docker-bestand meerdere fasen. In dit geval bevat de uitgebreide uitvoer de volgende tekst:

```cmd
$ azds up -v
Using dev space 'default' with target 'AksClusterName'
Synchronizing files...6s
Installing Helm chart...2s
Waiting for container image build...10s
Building container image...
Step 1/12 : FROM [imagename:tag] AS base
Error parsing reference: "[imagename:tag] AS base" is not a valid repository/tag: invalid reference format
Failed to build container image.
Service cannot be started.
```

Deze fout treedt op omdat het uitvoeren van een oudere versie van Docker die geen ondersteuning biedt voor meerdere fasen AKS-knooppunten is gebaseerd. Om te voorkomen dat meerdere fasen builds, herschrijven uw docker-bestand.

### <a name="rerunning-a-service-after-controller-re-creation"></a>Opnieuw uitvoeren van een service na het opnieuw maken van een domeincontroller
U ontvangt een *Service kan niet worden gestart* fout wanneer u probeert een-service opnieuw nadat u hebt verwijderd en vervolgens opnieuw gemaakt op de Azure Dev spaties-controller die zijn gekoppeld aan dit cluster uit te voeren. In dit geval bevat de uitgebreide uitvoer de volgende tekst:

```cmd
Installing Helm chart...
Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
Helm install failed with exit code '1': Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
```

Deze fout treedt op omdat de controller Dev spaties worden niet verwijderd services die eerder zijn geïnstalleerd door de controller. De controller opnieuw te maken en vervolgens probeert om uit te voeren van de services met behulp van de nieuwe domeincontroller is mislukt omdat de oude services nog steeds voldaan is.

Om dit probleem op te lossen, gebruikt u de `kubectl delete` opdracht voor het handmatig verwijderen van de oude services van het cluster en vervolgens Dev opslagruimten voor het installeren van de nieuwe services opnieuw uitvoeren.

## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>DNS-naamomzetting voor een openbare URL die is gekoppeld aan een Dev-Spaces-service is mislukt

U kunt een openbare URL-eindpunt configureren voor uw service door op te geven de `--public` overschakelen naar de `azds prep` opdracht of selecteren door de `Publicly Accessible` selectievakje in Visual Studio. De openbare DNS-naam wordt automatisch geregistreerd wanneer u uw service in Dev spaties uitvoeren. Als deze DNS-naam niet is geregistreerd, ziet u een *pagina kan niet worden weergegeven* of *Site kan niet worden bereikt* fout in uw webbrowser bij het verbinden met de openbare URL.

### <a name="try"></a>Probeer:

U kunt de volgende opdracht uit om alle URL's die zijn gekoppeld aan uw opslagruimten Dev-services:

```cmd
azds list-uris
```

Als een URL in de *in behandeling* staat, betekent dit dat Dev spaties nog voor DNS-registratie te voltooien. Soms duurt een paar minuten voor de registratie te voltooien. Dev opslagruimten wordt ook geopend wanneer u een tunnel ' localhost ' voor elke service, die u gebruiken kunt tijdens het wachten op DNS-registratie.

Als een URL blijft de *in behandeling* status voor meer dan 5 minuten, kan dit wijzen op een probleem met de externe DNS-schil die het openbare eindpunt worden gemaakt of de schil nginx ingress-controller die krijgt van het openbare eindpunt. U kunt de volgende opdrachten gebruiken om te verwijderen van deze schillen. AKS stelt automatisch de verwijderde schillen.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

## <a name="error-required-tools-and-configurations-are-missing"></a>Fout 'Vereiste hulpprogramma's en configuraties ontbreken'

Deze fout kan optreden bij het starten van VS Code: "[Azure Dev spaties] vereiste hulpprogramma's en configuraties om te bouwen en fouten opsporen in '[naam project]' ontbreken."
De fout betekent dat azds.exe bevindt zich niet in de omgevingsvariabele PATH, zoals te zien is in VS Code.

### <a name="try"></a>Probeer:

Start VS Code vanaf een opdrachtprompt waarvan de omgevingsvariabele PATH correct is ingesteld.

## <a name="error-required-tools-to-build-and-debug-projectname-are-out-of-date"></a>Fout "vereiste hulpprogramma's om te bouwen en fouten opsporen in 'projectname' zijn verouderd."

U ziet deze fout in Visual Studio Code hebt u een nieuwere versie van de VS Code-extensie voor Azure Dev spaties, maar een oudere versie van de CLI van Azure Dev spaties.

### <a name="try"></a>Proberen

Download en installeer de nieuwste versie van de CLI van Azure Dev spaties:

* [Windows](https://aka.ms/get-azds-windows)
* [Mac](https://aka.ms/get-azds-mac)
* [Linux](https://aka.ms/get-azds-linux)

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

U kunt nog steeds gebruik van Azure Dev spaties met code die in andere talen zijn geschreven, maar moet u handmatig maken voordat u de docker-bestand *azds van* voor de eerste keer.

### <a name="try"></a>Probeer:
Als uw toepassing is geschreven in een taal dat Azure Dev spaties geen systeemeigen ondersteuning biedt, moet u voor een juiste docker-bestand voor het bouwen van een containerinstallatiekopie uitvoeren van uw code. Docker biedt een [lijst met aanbevolen procedures voor het schrijven van docker-bestanden](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) en een [Dockerfile-referentiemateriaal](https://docs.docker.com/engine/reference/builder/) die kunnen helpen bij het schrijven van een docker-bestand dat aansluit op uw behoeften.

Wanneer u een juiste Dockerfile geïmplementeerd hebt, kunt u doorgaan met uitvoeren *azds van* voor het uitvoeren van uw toepassing in Azure Dev spaties.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Fout 'upstream verbindingsfout of verbreken/reset voordat headers'
Mogelijk ziet u deze fout bij het openen van uw service. Bijvoorbeeld, wanneer u gaat naar de URL van de service in een browser. 

### <a name="reason"></a>Reden 
De poort van de container is niet beschikbaar. Dit probleem kan optreden omdat: 
* De container wordt nog steeds worden gebouwd en geïmplementeerd. Dit probleem kan zich voordoen als u uitvoeren `azds up` of het foutopsporingsprogramma start en vervolgens probeert te krijgen tot de container voordat deze is geïmplementeerd.
* Poortconfiguratie van de is niet consistent zijn tussen uw _Dockerfile_, Helm-diagram en servercode die wordt een poort geopend.

### <a name="try"></a>Probeer:
1. Als de container gebouwd wordt/geïmplementeerd, kunt u wacht 2-3 seconden en probeer het opnieuw openen van de service. 
1. Controleer de poortconfiguratie. De opgegeven poortnummers moet **identieke** in alle van de volgende elementen:
    * **Docker-bestand:** Opgegeven door de `EXPOSE` instructie.
    * **[Helm-diagram](https://docs.helm.sh):** Opgegeven door de `externalPort` en `internalPort` waarden voor een service (vaak zich in een `values.yml` bestand),
    * Eventuele poorten die worden geopend in de toepassingscode, bijvoorbeeld in Node.js: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Configuratiebestand niet gevonden
U hebt uitgevoerd `azds up` en het volgende foutbericht weergegeven: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Reden
U moet uitvoeren `azds up` vanuit de hoofdmap van de code die u wilt uitvoeren, en moet u de code-map om uit te voeren met Azure Dev spaties initialiseren.

### <a name="try"></a>Probeer:
1. Wijzig de huidige directory naar de hoofdmap met de servicecode van uw. 
1. Als u nog geen een _azds.yaml_ bestand in de map code uitvoeren `azds prep` voor het genereren van Docker, Kubernetes en Azure Dev spaties activa.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Fout: 'Het programma pipe 'azds' is onverwacht afgesloten met code 126.'
Starten van het foutopsporingsprogramma VS Code kan soms leiden tot deze fout.

### <a name="try"></a>Probeer:
1. Sluiten en opnieuw openen van VS Code.
2. Druk nogmaals op F5.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>Fout 'Failed foutopsporingsprogramma vinden voor het type: coreclr'-foutopsporing
Het foutopsporingsprogramma VS Code wordt uitgevoerd, rapporteert de fout: `Failed to find debugger extension for type:coreclr.`

### <a name="reason"></a>Reden
U hebt niet de VS Code-extensie voor C# geïnstalleerd op uw ontwikkelcomputer. De C# -extensie bevat ondersteuning voor .NET Core (CoreCLR) voor foutopsporing.

### <a name="try"></a>Probeer:
Installeer de [VS Code-extensie voor C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>Fout bij foutopsporing 'geconfigureerd foutopsporing type 'coreclr' wordt niet ondersteund'
Het foutopsporingsprogramma VS Code wordt uitgevoerd, rapporteert de fout: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Reden
U hebt niet de VS Code-extensie voor Azure Dev opslagruimten geïnstalleerd op uw ontwikkelcomputer.

### <a name="try"></a>Probeer:
Installeer de [VS Code-extensie voor Azure Dev spaties](get-started-netcore.md).

## <a name="debugging-error-invalid-cwd-value-src-the-system-cannot-find-the-file-specified-or-launch-program-srcpath-to-project-binary-does-not-exist"></a>Foutopsporing van de fout ' Ongeldige 'cwd' waarde ' / src'. Het systeem het opgegeven bestand vinden niet." of "starten: programma/src / [pad naar een binair project] bestaat niet"
De fout wordt uitgevoerd het foutopsporingsprogramma VS Code rapporten `Invalid 'cwd' value '/src'. The system cannot find the file specified.` en/of `launch: program '/src/[path to project executable]' does not exist`

### <a name="reason"></a>Reden
De VS Code-extensie gebruikt standaard `src` als de werkmap voor het project voor de container. Als u hebt bijgewerkt uw `Dockerfile` om op te geven van een andere werkmap, mag u deze fout ziet.

### <a name="try"></a>Probeer:
Update de `launch.json` bestand onder de `.vscode` submap van de projectmap. Wijzig de `configurations->cwd` richtlijn om te verwijzen naar dezelfde map als de `WORKDIR` gedefinieerd in van uw project `Dockerfile`. U moet mogelijk ook om bij te werken de `configurations->program` ook richtlijn.

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>Het type of de naam 'MijnBibliotheek' kan niet worden gevonden.

### <a name="reason"></a>Reden 
Worden dat de build-context op het niveau van het project/service standaard is daarom een bibliotheek-project dat u gebruikt kan niet gevonden.

### <a name="try"></a>Probeer:
Wat er moet gebeuren:
1. Wijzig de _azds.yaml_ bestand in te stellen van de build-context op het niveau van de oplossing.
2. Wijzig de _Dockerfile_ en _Dockerfile.develop_ bestanden om te verwijzen naar het project (_.csproj_) bestanden correct ten opzichte van de nieuwe bouwen context.
3. Plaats een _.dockerignore_ bestand naast het SLN-bestand en wijzig zo nodig.

U vindt een voorbeeld op https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>'Microsoft.DevSpaces/register/action' Autorisatiefout
U moet *eigenaar* of *Inzender* toegang in uw Azure-abonnement voor het beheren van Azure Dev spaties. Mogelijk ziet u deze fout als u probeert te ontwikkelen spaties beheren en u geen hebt *eigenaar* of *Inzender* toegang tot het gekoppelde Azure-abonnement.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Reden
Het geselecteerde Azure-abonnement is niet geregistreerd. de `Microsoft.DevSpaces` naamruimte.

### <a name="try"></a>Probeer:
Iemand met de eigenaar of bijdrager toegang tot het Azure-abonnement kan worden uitgevoerd de volgende Azure CLI-opdracht om handmatig te registreren de `Microsoft.DevSpaces` naamruimte:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="dev-spaces-times-out-at-waiting-for-container-image-build-step-with-aks-virtual-nodes"></a>Ontwikkel opslagruimten een time-out optreedt bij *wacht op build van container-installatiekopie...*  stap met virtuele AKS-knooppunten

### <a name="reason"></a>Reden
Deze time-out treedt op wanneer u probeert te ontwikkelen opslagruimten gebruiken om uit te voeren van een service die is geconfigureerd om te worden uitgevoerd op een [AKS virtueel knooppunt](https://docs.microsoft.com/azure/aks/virtual-nodes-portal). Ontwikkel opslagruimten biedt momenteel geen ondersteuning het bouwen of het opsporen van fouten in services op virtuele-knooppunten.

Als u `azds up` met de `--verbose` switch of uitgebreide logboekregistratie van inschakelen in Visual Studio ziet u aanvullende details:

```cmd
$ azds up --verbose

Installed chart in 2s
Waiting for container image build...
pods/mywebapi-76cf5f69bb-lgprv: Scheduled: Successfully assigned default/mywebapi-76cf5f69bb-lgprv to virtual-node-aci-linux
Streaming build container logs for service 'mywebapi' failed with: Timed out after 601.3037572 seconds trying to start build logs streaming operation. 10m 1s
Container image build failed
```

De bovenstaande opdracht laat zien dat de schil van de service is toegewezen aan *virtuele-knooppunt-aci-linux*, dit is een virtueel knooppunt.

### <a name="try"></a>Probeer:
Bijwerken van de Helm-diagram voor de service verwijdert *nodeSelector* en/of *tolerations* waarden waarmee de service uit te voeren op een virtuele-knooppunt. Deze waarden worden gewoonlijk gedefinieerd in de grafiek `values.yaml` bestand.

U kunt nog steeds een AKS-cluster waarop de functie virtuele knooppunten is ingeschakeld, als de service die build/debug wilt via Dev opslagruimten wordt uitgevoerd op een VM-knooppunt. Dit is de standaardconfiguratie.

## <a name="error-could-not-find-a-ready-tiller-pod-when-launching-dev-spaces"></a>' Fout: kan een schil gereed tiller niet vinden "bij het starten van Dev-opslagruimten

### <a name="reason"></a>Reden
Deze fout treedt op als de Helm-client kan niet meer communiceren met de Tiller-pod uitgevoerd in het cluster.

### <a name="try"></a>Probeer:
Dit probleem wordt opgelost als de agentknooppunten in het cluster meestal opnieuw te starten.

## <a name="azure-dev-spaces-proxy-can-interfere-with-other-pods-running-in-a-dev-space"></a>Azure Dev spaties proxy kan leiden tot problemen met andere schillen uitgevoerd in een dev-ruimte

### <a name="reason"></a>Reden
Wanneer u Dev spaties voor een naamruimte in uw AKS-cluster inschakelt, er een extra container met de naam _mindaro-proxy_ in elk van de pods die worden uitgevoerd binnen deze naamruimte is geïnstalleerd. Deze container onderschept aanroepen naar de services in de schil die integraal is voor de Dev spaties team ontwikkelfunctionaliteit; echter kan dit leiden tot problemen met bepaalde services die in deze schillen uitgevoerd. Het is bekend dat leiden tot problemen met schillen uitgevoerd van Azure Cache voor Redis, waardoor-verbindingsfouten en fouten in de master/slave-communicatie.

### <a name="try"></a>Probeer:
U kunt de betrokken schillen verplaatsen naar een naamruimte in het cluster dat wordt _niet_ Dev spaties bevatten die zijn ingeschakeld. De rest van uw toepassing kunt blijven uitvoeren binnen een naamruimte Dev spaties ingeschakeld. Dev opslagruimten wordt niet geïnstalleerd. de _mindaro-proxy_ container binnen Dev spaties ingeschakelde naamruimten.

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure Dev spaties lijkt niet te gebruiken van mijn bestaande docker-bestand voor het bouwen van een container

### <a name="reason"></a>Reden
Azure Dev opslagruimten kunnen worden geconfigureerd om te verwijzen naar een specifieke _Dockerfile_ in uw project. Als deze wordt weergegeven met het Azure Dev opslagruimten wordt niet met behulp van de _Dockerfile_ u verwacht te maken van uw containers, moet u mogelijk expliciet Azure Dev spaties zien welke docker-bestand te gebruiken. 

### <a name="try"></a>Probeer:
Open de _azds.yaml_ bestand die Azure Dev spaties gegenereerd in uw project. Gebruik de *configuraties -> ontwikkelen build -> docker-bestand ->* richtlijn om te verwijzen naar de Dockerfile die u wilt gebruiken:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```

## <a name="error-internal-watch-failed-watch-enospc-when-attaching-debugging-to-a-nodejs-application"></a>Fout ' interne controle is mislukt: Bekijk ENOSPC "bij het opsporen van fouten op een Node.js-toepassing toevoegen

### <a name="reason"></a>Reden

Het knooppunt de schil uitgevoerd met de Node.js-toepassing die u probeert te koppelen aan met een foutopsporingsprogramma is langer dan de *fs.inotify.max_user_watches* waarde. In sommige gevallen [de standaardwaarde van *fs.inotify.max_user_watches* mogelijk te klein voor het afhandelen van een foutopsporingsprogramma koppelen rechtstreeks naar een schil](https://github.com/Azure/AKS/issues/772).

### <a name="try"></a>Proberen
Tijdelijke oplossing voor dit probleem is het verhogen van de waarde van *fs.inotify.max_user_watches* op elk knooppunt in het cluster en start opnieuw op dat knooppunt voor de wijzigingen worden doorgevoerd.
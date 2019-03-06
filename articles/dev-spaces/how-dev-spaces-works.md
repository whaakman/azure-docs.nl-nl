---
title: Hoe Azure Dev spaties werkt en is geconfigureerd
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.component: azds-kubernetes
author: zr-msft
ms.author: zarhoads
ms.date: 03/04/2019
ms.topic: conceptual
description: Beschrijving van de processen die power Azure Dev spaties en hoe ze zijn geconfigureerd in het configuratiebestand azds.yaml
keywords: azds.yaml, Azure Dev Spaces, Dev Spaces, Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers
ms.openlocfilehash: 4d0b2d24335b03fb1afc729e4cb8f7fc07f8a77e
ms.sourcegitcommit: 7e772d8802f1bc9b5eb20860ae2df96d31908a32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/06/2019
ms.locfileid: "57456872"
---
# <a name="how-azure-dev-spaces-works-and-is-configured"></a>Hoe Azure Dev spaties werkt en is geconfigureerd

Het ontwikkelen van een Kubernetes-toepassing kan lastig zijn. U moet Docker en Kubernetes-configuratiebestanden. U moet bepalen hoe uw toepassing lokaal testen en te communiceren met andere afhankelijke services. U moet mogelijk voor het afhandelen van ontwikkelen en testen op meerdere services tegelijk en met een team van ontwikkelaars.

Azure Dev spaties helpt u bij ontwikkelen, implementeren en fouten opsporen in Kubernetes-toepassingen rechtstreeks in Azure Kubernetes Service (AKS). Azure Dev opslagruimten kan ook een team voor het delen van een dev-ruimte. Een dev-ruimte voor een team te delen, kunt afzonderlijke teamleden om te ontwikkelen in isolatie hoeven te repliceren of model maken van afhankelijkheden of andere toepassingen in het cluster.

Azure Dev opslagruimten maakt en een configuratiebestand gebruikt voor het implementeren, uitvoeren en foutopsporing van uw toepassingen met Kubernetes in AKS. Dit configuratie-item bevindt zich met de code van uw toepassing en kan worden toegevoegd aan uw versiebeheersysteem.

In dit artikel worden de processen beschreven die power Azure Dev spaties en hoe deze processen zijn geconfigureerd in het configuratiebestand Azure Dev spaties. U kunt Azure Dev spaties snel ophalen en zien in de praktijk, voert u een van de Quick starts:

* [Java met CLI en Visual Studio Code](quickstart-java.md)
* [.NET core met CLI en Visual Studio Code](quickstart-netcore.md)
* [.NET core met Visual Studio 2017](quickstart-netcore-visualstudio.md)
* [Node.js met CLI en Visual Studio Code](quickstart-nodejs.md)

## <a name="how-azure-dev-spaces-works"></a>De werking van Azure Dev spaties

Azure Dev spaties bestaat uit twee verschillende onderdelen die u ermee: de controller en de client-side-hulpprogramma's.

![Azure Dev spaties-onderdelen](media/how-dev-spaces-works/components.svg)

De controller worden de volgende acties uitgevoerd:

* Hiermee beheert u dev ruimte maken en selectie.
* Helm-diagram van uw toepassing wordt geïnstalleerd en Kubernetes-objecten maakt.
* De containerinstallatiekopie van uw toepassing is gebaseerd.
* Uw toepassing in AKS implementeert.
* Geen incrementele builds en opnieuw wordt gestart als de broncode wijzigt.
* Hiermee beheert u de logboeken en traceringen van HTTP.
* Stdout en stderr naar de client-side-tooling verzendt.
* Hiermee kunnen teamleden onderliggende dev opslagruimten is afgeleid van een bovenliggende dev-ruimte te maken.
* Hiermee configureert u routering voor toepassingen binnen een spatie en tussen bovenliggende en onderliggende spaties.

De controller bevindt zich buiten AKS. Deze stations het gedrag en de communicatie tussen de client-side-hulpmiddelen en het AKS-cluster. De controller is ingeschakeld met de Azure CLI bij het voorbereiden van uw cluster moet Azure Dev opslagruimten gebruiken. Zodra deze is ingeschakeld, kunt u er interactie mee met behulp van de client-side-hulpprogramma's.

De client-side-tooling kan de gebruiker:
* Een docker-bestand genereren Helm-diagram en Azure Dev spaties-configuratiebestand voor de toepassing.
* Bovenliggende en onderliggende dev opslagruimten maken.
* Laat de controller te bouwen en uw toepassing te starten.

Terwijl uw toepassing wordt uitgevoerd, de client-hulpprogramma's ook:
* Ontvangt en stdout en stderr van uw toepassing die wordt uitgevoerd in AKS worden weergegeven.
* Maakt gebruik van [poort-zone voor forward](https://kubernetes.io/docs/tasks/access-application-cluster/port-forward-access-application-cluster/) web toegang tot uw toepassing met http://locahost.
* Een foutopsporingsprogramma koppelt aan de toepassing wordt uitgevoerd in AKS.
* Synchronisaties broncode naar de adresruimte van uw dev wanneer een wijziging wordt gedetecteerd voor incrementele builds, waardoor een snelle iteratie.

U kunt de client-hulpprogramma's vanaf de opdrachtregel als onderdeel van de `azds` opdracht. U kunt ook de client-hulpprogramma's met:

* Met behulp van Visual Studio Code de [Azure Dev spaties extensie](https://marketplace.visualstudio.com/items?itemName=azuredevspaces.azds).
* Visual Studio 2017 met [Visual Studio Tools voor Kubernetes](https://aka.ms/get-vsk8stools).

Hier volgt de basisprincipes voor het instellen en gebruiken van Azure Dev spaties:
1. Voorbereiden van uw AKS-cluster voor Azure Dev spaties
1. Voorbereiden van uw code voor het uitvoeren op Azure Dev spaties
1. De code uitvoeren op een spatie ontwikkelen
1. Fouten opsporen in uw code op een spatie ontwikkelen
1. Delen van een dev-ruimte

Meer te weten over meer details van de werking van Azure Dev spaties in elk van de onderstaande secties.

## <a name="prepare-your-aks-cluster"></a>Voorbereiden van uw AKS-cluster

Voorbereiden van uw AKS-cluster bestaat uit:
* Controleren van uw AKS-cluster zich in een regio [ondersteund door Azure Dev spaties](https://docs.microsoft.com/azure/dev-spaces/#a-rapid,-iterative-kubernetes-development-experience-for-teams).
* Controleren van Kubernetes 1.10.3 wordt uitgevoerd of hoger.
* Inschakelen van Azure Dev spaties over het gebruik van uw cluster `az aks use-dev-spaces`

Zie voor meer informatie over het maken en configureren van een AKS-cluster voor Azure Dev spaties, een van de aan de slag-handleidingen:
* [Aan de slag met Azure Dev-ruimten met Java](get-started-java.md)
* [Aan de slag met Azure Dev-ruimten met .NET Core en Visual Studio](get-started-netcore-visualstudio.md)
* [Aan de slag met Azure Dev-ruimten met .NET Core](get-started-netcore.md)
* [Aan de slag met Azure Dev-ruimten met behulp van Node.js](get-started-nodejs.md)

Wanneer Azure Dev opslagruimten is ingeschakeld op uw AKS-cluster, installeert het de controller voor uw cluster. De controller is een afzonderlijke Azure-resources buiten het cluster en doet het volgende aan resources in uw cluster:

* Hiermee maakt of een Kubernetes-naamruimten wilt gebruiken als een spatie dev Hiermee wordt aangegeven.
* Hiermee verwijdert u een Kubernetes-naamruimte met de naam *azds*, indien deze bestaat, en maakt u een nieuwe.
* Implementeert een Kubernetes-initialisatiefunctie-object.

! [Azure Dev spaties voorbereiden cluster]] (media/how-dev-spaces-works/prepare-cluster.svg)

Als u wilt gebruiken Azure Dev spaties, moet er ten minste één dev-ruimte. Azure Dev opslagruimten maakt gebruik van Kubernetes-naamruimten in uw AKS-cluster voor dev spaties. Als een domeincontroller wordt geïnstalleerd, wordt u gevraagd om te maken van een nieuwe naamruimte met Kubernetes of kies een bestaande naamruimte te gebruiken als uw eerste dev-ruimte. Wanneer een naamruimte is aangewezen als een dev-ruimte, de controller voegt de *azds.io/space=true* label in op die naamruimte om het te identificeren als een dev-ruimte. Nadat u uw cluster hebt voorbereid, is de initiële dev-ruimte u maakt of aanwijzen standaard geselecteerd. Wanneer een spatie is geselecteerd, wordt deze gebruikt door Azure Dev opslagruimten voor het maken van nieuwe werkbelastingen.

De controller maakt standaard een dev-ruimte met de naam *standaard* door het upgraden van de bestaande *standaard* Kubernetes-naamruimten. De client-side-hulpmiddelen kunt u nieuwe dev opslagruimten maken en bestaande dev spaties verwijderen. Vanwege een beperking in Kubernetes, de *standaard* dev ruimte kan niet worden verwijderd. De controller verwijdert u ook een bestaand Kubernetes-naamruimten met de naam *azds* het voorkomen van conflicten met het `azds` opdracht die wordt gebruikt door de client-side-hulpprogramma's.

Het Kubernetes-initialisatiefunctie-object wordt gebruikt om te ' injecteren ' schillen met drie containers tijdens de implementatie voor instrumentatie: een container devspaces-proxy, een devspaces-proxy-init-container en een container devspaces-build. **Alle drie deze containers worden uitgevoerd met toegang tot de hoofdmap van uw AKS-cluster.**

![Azure Dev spaties Kubernetes initialisatiefunctie](media/how-dev-spaces-works/kubernetes-initializer.svg)

De container devspaces-proxy is een sidecar-container die verantwoordelijk is voor alle TCP-verkeer van en naar de container van de toepassing en helpt uitvoeren routering. De container devspaces-proxy HTTP-berichten wordt omgeleid als bepaalde spaties worden gebruikt. Bijvoorbeeld, kunt u routeren van HTTP-berichten tussen toepassingen in de bovenliggende en onderliggende spaties. Alle niet-HTTP-verkeer wordt doorgegeven via devspaces-proxy niet gewijzigd. De container devspaces-proxy ook registreert alle inkomende en uitgaande HTTP-berichten en stuurt deze naar de client-hulpprogramma's als traceringen. Deze traceringen kunnen vervolgens worden weergegeven door de ontwikkelaar te inspecteren van het gedrag van de toepassing.

De devspaces-proxy-init-container is een [init container](https://kubernetes.io/docs/concepts/workloads/pods/init-containers/) die aanvullende regels voor routering op basis van de ruimte-hiërarchie naar de container van uw toepassing wordt toegevoegd. Regels voor doorsturen wordt toegevoegd door het bijwerken van de toepassingscontainer */etc/resolv.conf* bestands- en iptables configuratie voordat deze wordt gestart. De updates voor */etc/resolv.conf* toestaan voor DNS-omzetting van services in de bovenliggende spaties. De configuratie-updates van iptables Zorg ervoor dat alle TCP-verkeer in en buiten de container van de toepassing worden gerouteerd via devspaces-proxy. Alle updates van devspaces-proxy-init gebeuren naast de regels die Kubernetes wordt toegevoegd.

De container devspaces-build is een container init en beschikt over de project-broncode en Docker-socket gekoppeld. De project-broncode en toegang tot Docker kunt de container van de toepassing kunnen worden gebouwd rechtstreeks door de schil.

> [!NOTE]
> Azure Dev opslagruimten maakt gebruik van hetzelfde knooppunt van uw toepassing container bouwen en uitvoeren. Azure Dev spaties heeft een externe container registry als gevolg hiervan niet nodig voor het bouwen en uitvoeren van uw toepassing.

Het object van de initialisatiefunctie Kubernetes luistert naar een nieuwe pod dat gemaakt in het AKS-cluster. Als deze pod wordt geïmplementeerd voor een naamruimte met de *azds.io/space=true* label, wordt deze pod injects met de extra containers. De container devspaces-build is alleen opgenomen als de container van de toepassing wordt uitgevoerd met behulp van de client-side-hulpprogramma's.

Nadat u uw AKS-cluster hebt voorbereid, kunt u de client-side-tooling voor het voorbereiden en uw code in uw dev-ruimte worden uitgevoerd.

## <a name="prepare-your-code"></a>Voorbereiden van uw code

Als u wilt uw toepassing in een dev-ruimte worden uitgevoerd, moet worden opgenomen in een container en moet u definiëren hoe deze moet worden geïmplementeerd op Kubernetes. Als u wilt uw toepassing in een container plaatsen, moet u een docker-bestand. Als u wilt definiëren hoe uw toepassing wordt geïmplementeerd naar Kubernetes, moet u een [Helm-diagram](https://docs.helm.sh/). Om u te helpen bij het maken van zowel de docker-bestand en de Helm-diagram voor uw toepassing, de client-side-hulpprogramma's bieden de `prep` opdracht:

```cmd
azds prep --public
```

De `prep` opdracht wordt de bestanden in uw project bekeken en probeert te maken van de docker-bestand en het Helm-diagram voor het uitvoeren van uw toepassing in Kubernetes. Op dit moment de `prep` opdracht genereert een Dockerfile en Helm-grafiek met de volgende talen:

* Java
* Node.js
* .NET Core

U *moet* uitvoeren de `prep` opdracht uit vanaf een map met de broncode. Met de `prep` opdracht van de juiste map staat u de client-side-tooling voor het identificeren van de taal en het maken van een juiste docker-bestand voor uw toepassing in een container plaatsen. U kunt ook uitvoeren de `prep` opdracht uit vanaf een map met een *pom.xml* -bestand voor Java-projecten.

Als u de `prep` opdracht uit vanuit de map die geen broncode, de client-side-tooling wordt een docker-bestand niet genereren. Er worden ook de uitspraak van een fout weergegeven: *Docker-bestand kan niet worden gegenereerd vanwege een niet-ondersteunde taal*. Deze fout treedt ook op als het projecttype niet wordt herkend door de client-side-hulpprogramma's.

Bij het uitvoeren van de `prep` opdracht, hebt u de optie voor het opgeven van de `--public` vlag. Deze vlag wordt aan de domeincontroller om een internet toegankelijk is via het eindpunt voor deze service te maken. Als u deze vlag niet opgeeft, wordt de service is alleen toegankelijk is vanuit het cluster of met behulp van de localhost-tunnel gemaakt door de client-side-hulpprogramma's. U kunt inschakelen of uitschakelen van dit gedrag nadat het `prep` opdracht door het bijwerken van de gegenereerde Helm-grafiek.

De `prep` opdracht wordt niet vervangen met een bestaande docker-bestanden of Helm grafieken in uw project hebt. Als een bestaande docker-bestand of Helm-grafiek dezelfde naamconventie als de bestanden die worden gegenereerd gebruikt door de `prep` opdracht, de `prep` opdracht genereren van die bestanden wordt overgeslagen. Anders wordt de `prep` opdracht een eigen docker-bestand wordt gegenereerd of Helm-grafiek naast de bestaande bestanden.

De `prep` opdracht genereert ook een `azds.yaml` bestand in de hoofdmap van uw project. Azure Dev opslagruimten maakt gebruik van dit bestand te bouwen, installeren, configureren en uw toepassing wordt uitgevoerd. Dit configuratie-item geeft een lijst van de locatie van uw docker-bestand en Helm-grafiek en biedt ook aanvullende configuratie op deze artefacten.

Hier volgt een voorbeeld van azds.yaml bestand gemaakt met behulp van [.NET Core-voorbeeldtoepassing](https://github.com/Azure/dev-spaces/tree/master/samples/dotnetcore/getting-started/webfrontend):

```yaml
kind: helm-release
apiVersion: 1.1
build:
  context: .
  dockerfile: Dockerfile
install:
  chart: charts/webfrontend
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    replicaCount: 1
    image:
      repository: webfrontend
      tag: $(tag)
      pullPolicy: Never
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik-azds
      hosts:
        # This expands to [space.s.][rootSpace.]webfrontend.<random suffix>.<region>.azds.io
        - $(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
    container:
      sync:
      - "**/Pages/**"
      - "**/Views/**"
      - "**/wwwroot/**"
      - "!**/*.{sln,csproj}"
      command: [dotnet, run, --no-restore, --no-build, --no-launch-profile, -c, "${BUILD_CONFIGURATION:-Debug}"]
      iterate:
        processesToKill: [dotnet, vsdbg]
        buildCommands:
        - [dotnet, build, --no-restore, -c, "${BUILD_CONFIGURATION:-Debug}"]
```

De `azds.yaml` bestand dat is gegenereerd door de `prep` opdracht moet goed werkt voor een scenario voor het ontwikkelen van eenvoudige, één project. Als uw specifieke project is toegenomen complexiteit, moet u mogelijk om bij te werken van dit bestand nadat de `prep` opdracht. Uw project kan bijvoorbeeld worden aangepast aan uw build of proces op basis van het ontwikkelen van apps of behoeften foutopsporing starten. Ook mogelijk hebt u meerdere toepassingen in uw project, waarvoor meerdere buildprocessen of de inhoud van een ander buildnummer.

## <a name="run-your-code"></a>De code uitvoeren

Voor het uitvoeren van uw code in een ruimte dev geven de `up` opdracht in dezelfde map als uw `azds.yaml` bestand:

```cmd
azds up
```

De `up` opdracht uploadt de bronbestanden van uw toepassing en andere artefacten die nodig zijn om te bouwen en uw project uitvoeren op de dev-ruimte. Van daaruit de controller in de adresruimte van uw dev:

1. Hiermee maakt u de Kubernetes-objecten om uw toepassing te implementeren.
1. De container voor uw toepassing is gebaseerd.
1. Implementeert de toepassing naar de dev-ruimte.
1. Hiermee maakt u een openbaar toegankelijke DNS-naam voor het toepassingseindpunt van uw als geconfigureerd.
1. Maakt gebruik van *poort-zone voor forward* voor toegang tot uw toepassing eindpunt met http://locahost.
1. Stdout en stderr naar de client-side-tooling verzendt.


### <a name="starting-a-service"></a>Starten van een service

Wanneer u een service in een ruimte dev start, wordt de client-side-hulpprogramma's en de domeincontroller werken in coördinatie nodig voor het synchroniseren van de bronbestanden van uw, uw container en Kubernetes-objecten maken en uitvoeren van uw toepassing.

Op een meer gedetailleerd niveau, dit is wat er gebeurt tijdens het uitvoeren van `azds up`:

1. Bestanden worden gesynchroniseerd vanuit de computer van de gebruiker naar een Azure-bestandsopslag die uniek is voor de AKS-cluster van de gebruiker. De broncode, Helm-diagram en -configuratiebestanden worden geüpload. Meer informatie over het synchronisatieproces zijn beschikbaar in de volgende sectie.
1. De controller maakt een aanvraag om een nieuwe sessie te starten. Deze aanvraag bevat verschillende eigenschappen, met inbegrip van een unieke ID, de naam van ruimte, pad naar de broncode en een vlag voor foutopsporing.
1. De controller vervangt de *$(tag)* tijdelijke aanduiding in het Helm-diagram met de unieke sessie-ID en de Helm-grafiek voor uw service wordt geïnstalleerd. Het toevoegen van dat een verwijzing naar de unieke sessie-ID aan de Helm-grafiek kan de container geïmplementeerd in het AKS-cluster voor deze specifieke sessie te worden teruggekoppeld naar de sessie-aanvraag en informatie die is gekoppeld.
1. De initialisatiefunctie Kubernetes object tijdens de installatie van de Helm-grafiek wordt meer containers aan van uw toepassing pod voor instrumentatie en toegang tot de broncode van uw project toegevoegd. De devspaces-proxy en devspaces-proxy-init-containers worden toegevoegd om HTTP-tracering en ruimte routering te bieden. De devspaces-build-container wordt toegevoegd aan de schil voorzien van toegang tot het Docker-exemplaar en de project-broncode voor het bouwen van uw toepassing container.
1. Wanneer de schil van de toepassing wordt gestart, worden de devspaces-build-container en devspaces-proxy-init-container gebruikt voor het bouwen van de container van de toepassing. De App-container en de devspaces proxy-containers worden vervolgens gestart.
1. Nadat de container van de toepassing is gestart, de client-side-functionaliteit maakt gebruik van de Kubernetes *poort-zone voor forward* functionaliteit te bieden voor HTTP-toegang tot uw toepassing http://localhost. Uw ontwikkelcomputer verbinding dit doorsturen via poort met de service in uw dev-ruimte.
1. Wanneer alle containers in de schil hebt gestart, wordt de service wordt uitgevoerd. Op dit moment begint de client-side-functionaliteit om de HTTP-traceringen, stdout en stderr streamen. Deze informatie wordt weergegeven door de client-side-functionaliteit voor ontwikkelaars.

### <a name="updating-a-running-service"></a>Een actieve service bijwerken

Tijdens een service wordt uitgevoerd, heeft Azure Dev spaties de mogelijkheid om bij te werken die service als een van de bronbestanden van het project wilt wijzigen. Ontwikkel spaties verwerkt ook bijwerken van de service anders, afhankelijk van het type bestand dat wordt gewijzigd. Er zijn drie manieren die DEV spaties kunt een actieve service bijwerken:

* Een bestand bijwerken rechtstreeks
* Opnieuw opbouwen en opnieuw starten van de toepassing proces binnen de actieve App-container
* Opnieuw opbouwen en de container van de toepassing opnieuw te implementeren

![Met Azure Dev spaties file sync](media/how-dev-spaces-works/file-sync.svg)

Bepaalde projectbestanden die statische assets, zoals html, css en cshtml-bestanden, kunnen rechtstreeks in de container van de toepassing zonder opnieuw te starten worden bijgewerkt. Als een statische asset wordt gewijzigd, wordt het nieuwe bestand gesynchroniseerd met de dev-ruimte en vervolgens worden gebruikt door de container die wordt uitgevoerd.

Wijzigingen in bestanden zoals broncode en configuratiebestanden van de toepassing kunnen worden toegepast door het proces van de toepassing binnen de container die wordt uitgevoerd opnieuw te starten. Wanneer deze bestanden worden gesynchroniseerd, het proces van de toepassing opnieuw wordt opgestart in de actieve container met de *devhostagent* proces. Bij het maken van de container van de toepassing in eerste instantie, de controller de opdracht voor opstarten voor de toepassing vervangt door een ander proces met de naam *devhostagent*. Proces van de toepassing wordt vervolgens uitgevoerd als onderliggend proces onder *devhostagent*, en de uitvoer is doorgesluisd uit met *devhostagent*-uitvoer. De *devhostagent* proces maakt ook deel uit van de Dev-ruimten en opdrachten kunt uitvoeren in de actieve container namens Dev spaties. Bij het uitvoeren van een herstart *devhostagent*:

* Stopt het huidige proces of de processen die zijn gekoppeld aan de toepassing
* De toepassing wordt opnieuw gemaakt
* Het proces of de processen die zijn gekoppeld aan de toepassing opnieuw is opgestart

De manier waarop *devhostagent* voert de voorgaande stappen is geconfigureerd in de `azds.yaml` configuratiebestand. Deze configuratie wordt beschreven in een volgende sectie.

Updates voor de project-bestanden, zoals docker-bestanden, csproj-bestanden of een deel van het Helm-diagram vereisen de container van de toepassing worden opnieuw opgebouwd en wordt opnieuw geïmplementeerd. De domeincontroller wordt uitgevoerd wanneer een van deze bestanden is gesynchroniseerd met de dev-ruimte, de [helm upgrade](https://helm.sh/docs/helm/#helm-upgrade) opdracht en de container van de toepassing is opnieuw opgebouwd en geïmplementeerd.

### <a name="file-synchronization"></a>Bestandssynchronisatie

De eerste keer dat een toepassing wordt gestart in een dev-ruimte, worden de bronbestanden van de toepassing geüpload. Terwijl de toepassing wordt uitgevoerd en op latere opnieuw wordt opgestart, wordt alleen de gewijzigde bestanden worden geüpload. Twee bestanden worden gebruikt voor de coördinatie van dit proces: een client-side-bestand en een bestand controller aan clientzijde.

Het client-side-bestand wordt opgeslagen in een tijdelijke map en de naam op basis van een hash van de projectmap die u in Dev spaties uitvoert. Bijvoorbeeld, voor Windows hebt u een bestand, zoals *Users\USERNAME\AppData\Local\Temp\1234567890abcdef1234567890abcdef1234567890abcdef1234567890abcdef.synclog* voor uw project. Op Linux, de client-side-bestand wordt opgeslagen in de *map/Tmp* directory. U kunt de map vinden op macOS door het uitvoeren van de `echo $TMPDIR` opdracht.

Dit bestand wordt in JSON-indeling en bevat:

* Een vermelding voor elk projectbestand dat wordt gesynchroniseerd met de dev-ruimte
* Een synchronisatie-ID
* De tijdstempel van de laatste synchronisatiebewerking

Elk item project-bestand bevat een pad naar het bestand en de tijdstempel.

De controller aan clientzijde-bestand is opgeslagen op het AKS-cluster. Het bevat de synchronisatie-ID en de tijdstempel van de laatste synchronisatie.

Een synchronisatie treedt op wanneer de synchronisatie-tijdstempels niet tussen de client en de bestanden van de controller aan clientzijde overeen komen. Tijdens een synchronisatie is de client-side-tooling herhalen van de vermeldingen in het client-side-bestand. Als de timestamp van het bestand na de synchronisatie-timestamp is, is dat bestand gesynchroniseerd met de dev-ruimte. Nadat de synchronisatie voltooid is, worden de tijdstempels synchronisatie bijgewerkt op de client en de controller aan clientzijde bestanden.

Alle van de project-bestanden worden gesynchroniseerd als de client-side-bestand niet aanwezig is. Dit gedrag kunt u een volledige synchronisatie afdwingen met het verwijderen van de client-side-bestand.

### <a name="how-routing-works"></a>Hoe Routering werkt

Een dev-ruimte is gebaseerd op AKS en maakt gebruik van dezelfde [netwerkconcepten](../aks/concepts-network.md). Azure Dev spaties heeft ook een gecentraliseerde *ingressmanager* service en implementeert u een eigen Controller voor binnenkomend verkeer naar het AKS-cluster. De *ingressmanager* monitors AKS-clusters met opslagruimten ontwikkelen en verbetert de Ingangscontroller Azure Dev spaties in het cluster met inkomende objecten voor de routering naar toepassingspods-service. De container devspaces-proxy in elke schil wordt toegevoegd een `azds-route-as` HTTP-header voor HTTP-verkeer naar een dev-ruimte op basis van de URL. Bijvoorbeeld, een aanvraag voor de URL *http://azureuser.s.default.serviceA.fedcba09...azds.io* ontstaat er een HTTP-header met `azds-route-as: azureuser`. De container devspaces-proxy niet wordt toegevoegd een `azds-route-as` header als een al aanwezig is.

Wanneer een HTTP-aanvraag wordt gedaan bij een service van buiten het cluster, wordt de aanvraag wordt gerouteerd naar de controller voor binnenkomend verkeer. De controller voor binnenkomend verkeer stuurt de aanvraag rechtstreeks naar de juiste schil op basis van de inkomende objecten en regels. De container devspaces-proxy in de schil toegevoegd, ontvangt de aanvraag de `azds-route-as` koptekst op basis van de URL en stuurt de aanvraag door naar de toepassingscontainer.

Wanneer een HTTP-aanvraag wordt gedaan bij een service van een andere service binnen het cluster, verloopt de aanvraag eerst via de aanroepende service devspaces proxy-container. De container devspaces-proxy kijkt naar de HTTP-aanvraag en controles de `azds-route-as` header. Op basis van de koptekst, ziet de container devspaces-proxy van de IP-adres van de service die is gekoppeld aan de waarde voor header. Als een IP-adres wordt gevonden, wordt de container devspaces-proxy omgeleid met de aanvraag voor het IP-adres. Als een IP-adres niet wordt gevonden, stuurt de container devspaces-proxy de aanvraag door naar de container van de bovenliggende toepassing.

Bijvoorbeeld, de toepassingen *serviceA* en *serviceB* zijn geïmplementeerd op een bovenliggende dev ruimte, het zogenaamde *standaard*. *serviceA* is afhankelijk van *serviceB* en zorgt ervoor dat HTTP-aanroepen naar deze. Azure-gebruiker wordt gemaakt van een onderliggende dev-ruimte op basis van de *standaard* ruimte, het zogenaamde *azureuser*. Azure-gebruiker implementeert ook een eigen versie van *serviceA* naar hun onderliggende-ruimte. Wanneer een aanvraag wordt gedaan bij *http://azureuser.s.default.serviceA.fedcba09...azds.io*:

![Azure Dev spaties routering](media/how-dev-spaces-works/routing.svg)

1. De controller voor binnenkomend verkeer zoekt naar het IP-adres voor de schil die zijn gekoppeld aan de URL die *serviceA.azureuser*.
1. De controller voor binnenkomend verkeer wordt het IP-adres gevonden voor de schil in de Azure-gebruiker dev ruimte en stuurt de aanvraag naar de *serviceA.azureuser* schil.
1. De container devspaces-proxy in het *serviceA.azureuser* pod ontvangt de aanvraag en voegt `azds-route-as: azureuser` als een HTTP-header.
1. De container devspaces-proxy in het *serviceA.azureuser* pod stuurt de aanvraag naar de *serviceA* toepassingscontainer in de *serviceA.azureuser* schil.
1. De *serviceA* toepassing in de *serviceA.azureuser* pod Hiermee wordt een aanroep naar *serviceB*. De *serviceA* toepassing bevat ook code voor het behouden van de bestaande `azds-route-as` koptekst, die in dit geval `azds-route-as: azureuser`.
1. De container devspaces-proxy in het *serviceA.azureuser* pod ontvangt de aanvraag en zoekt naar het IP-adres van *serviceB* op basis van de waarde van de `azds-route-as` header.
1. De container devspaces-proxy in het *serviceA.azureuser* pod vindt geen IP-adres voor *serviceB.azureuser*.
1. De container devspaces-proxy in het *serviceA.azureuser* pod zoekt naar het IP-adres voor *serviceB* in de bovenliggende-ruimte is *serviceB.default*.
1. De container devspaces-proxy in het *serviceA.azureuser* pod vindt het IP-adres voor *serviceB.default* en stuurt de aanvraag naar de *serviceB.default* schil.
1. De container devspaces-proxy in het *serviceB.default* pod ontvangt de aanvraag en stuurt de aanvraag naar de *serviceB* toepassingscontainer in de *serviceB.default*schil.
1. De *serviceB* toepassing in de *serviceB.default* pod retourneert een antwoord naar de *serviceA.azureuser* schil.
1. De container devspaces-proxy in het *serviceA.azureuser* pod-antwoord is ontvangen en stuurt het antwoord op de *serviceA* toepassingscontainer in de *serviceA.azureuser* schil.
1. De *serviceA* toepassing ontvangt van het antwoord en retourneert vervolgens het eigen antwoord.
1. De container devspaces-proxy in het *serviceA.azureuser* pod ontvangt het antwoord van de *serviceA* toepassingscontainer en stuurt het antwoord op de oorspronkelijke aanvrager buiten het cluster.

Alle andere TCP-verkeer is geen HTTP wordt doorgegeven via de controller voor binnenkomend verkeer en devspaces-proxy containers ongewijzigd.

### <a name="how-running-your-code-is-configured"></a>Het uitvoeren van uw code is geconfigureerd

Maakt gebruik van Azure Dev spaties de `azds.yaml` dat u wilt installeren en configureren van uw service. De controller gebruikt de `install` eigenschap in de `azds.yaml` dat u wilt het Helm-diagram installeren en maken van de Kubernetes-objecten:

```yaml
...
install:
  chart: charts/webfrontend
  values:
  - values.dev.yaml?
  - secrets.dev.yaml?
  set:
    replicaCount: 1
    image:
      repository: webfrontend
      tag: $(tag)
      pullPolicy: Never
    ingress:
      annotations:
        kubernetes.io/ingress.class: traefik-azds
      hosts:
      # This expands to [space.s.][rootSpace.]webfrontend.<random suffix>.<region>.azds.io
      - $(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)
...
```

Standaard de `prep` opdracht genereert het Helm-diagram. Stelt ook het *install.chart* eigenschap naar de map van het Helm-diagram. Als u een Helm-grafiek gebruiken in een andere locatie wilt, kunt u deze eigenschap voor het gebruik van die locatie kunt bijwerken.

Bij het installeren van de Helm-grafieken, biedt Azure Dev opslagruimten een manier om het overschrijven van waarden in het Helm-diagram. De standaardwaarden voor de Helm-grafiek zich in `charts/APP_NAME/values.yaml`.

Met behulp van de *install.values* eigenschap, kunt u een of meer bestanden die de waarden die u vervangen in het Helm-diagram wilt definiëren weergeven. Bijvoorbeeld, als u een hostnaam of het database-configuratie specifiek wanneer uw toepassing wordt uitgevoerd in een dev-ruimte wilt, kunt u deze functionaliteit overschrijven. U kunt ook toevoegen een *?* aan het einde van een van de bestandsnamen in te stellen als optioneel.

De *install.set* eigenschap kunt u een of meer waarden die u vervangen in het Helm-diagram wilt te configureren. Alle waarden die zijn geconfigureerd *install.set* overschrijven de waarden die zijn geconfigureerd in de bestanden die vermeld staan *install.values*. De eigenschappen in onder *install.set* afhankelijk zijn van de waarden in het Helm-diagram en kunnen mogelijk verschillen, afhankelijk van het gegenereerde Helm-diagram.

In het bovenstaande voorbeeld de *install.set.replicaCount* eigenschap vertelt ons de controller hoeveel exemplaren van uw toepassing om uit te voeren in uw dev-ruimte. Afhankelijk van uw scenario kunt u deze waarde verhogen, maar het is een invloed op een foutopsporingsprogramma koppelen aan de schil van uw toepassing. Zie voor meer informatie de [probleemoplossingsartikel](troubleshooting.md).

In het gegenereerde Helm-diagram wordt de containerinstallatiekopie is ingesteld op *{{. Values.Image.Repository}} :{{. Values.Image.tag}}*. De `azds.yaml` -bestand definieert *install.set.image.tag* eigenschap *$(tag)* standaard, die wordt gebruikt als de waarde voor *{{. Values.Image.tag}}*. Door in te stellen de *install.set.image.tag* eigenschap in op deze manier wordt de installatiekopie van de container voor uw toepassing in een afzonderlijke manier worden getagd bij het uitvoeren van Azure Dev spaties. In dit specifieke geval, de installatiekopie is gemarkeerd als  *<value from image.repository>: $(tag)*. Moet u de *$(tag)* als de waarde van variabele *install.set.image.tag* opdat Dev spaties herkennen en zoek naar de container in het AKS-cluster.

In het bovenstaande voorbeeld `azds.yaml` definieert *install.set.ingress.hosts*. De *install.set.ingress.hosts* eigenschap definieert een indeling voor de host voor de openbare eindpunten. Deze eigenschap gebruikt ook *$(spacePrefix)*, *$(rootSpacePrefix)*, en *$(hostSuffix)*, die de waarden geleverd door de controller zijn. 

De *$(spacePrefix)* is de naam van de onderliggende dev-ruimte, waarbij de vorm van *SPACENAME.s*. De *$(rootSpacePrefix)* is de naam van de bovenliggende ruimte. Bijvoorbeeld, als *azureuser* is een ruimte onderliggende van *standaard*, de waarde voor *$(rootSpacePrefix)* is *standaard* en de waarde van *$(spacePrefix)* is *azureuser.s*. Als de ruimte niet een spatie onderliggende *$(spacePrefix)* is leeg. Bijvoorbeeld, als de *standaard* ruimte heeft geen bovenliggende ruimte, de waarde voor *$(rootSpacePrefix)* is *standaard* en de waarde van *$(spacePrefix)* is leeg. De *$(hostSuffix)* is een DNS-achtervoegsel dat verwijst naar de Azure Dev spaties Controller voor binnenkomend verkeer die in uw AKS-cluster wordt uitgevoerd. Dit DNS-achtervoegsel komt overeen met een DNS-vermelding van het jokerteken, bijvoorbeeld  *\*. RANDOM_VALUE.eus.azds.IO*, die is gemaakt wanneer de controller Azure Dev spaties naar uw AKS-cluster is toegevoegd.

In het bovenstaande `azds.yaml` -bestand, kunt u ook bijwerken *install.set.ingress.hosts* te wijzigen van de hostnaam van uw toepassing. Bijvoorbeeld, als u wilt vereenvoudigen, de hostnaam van uw toepassing vanuit *$(spacePrefix)$(rootSpacePrefix)webfrontend$(hostSuffix)* naar *$(spacePrefix)$(rootSpacePrefix)web$(hostSuffix)*.

Voor het bouwen van de container voor uw toepassing gebruikmaakt van de controller de onderstaande secties van de `azds.yaml` configuratiebestand:

```yaml
build:
  context: .
  dockerfile: Dockerfile
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
      useGitIgnore: true
      args:
        BUILD_CONFIGURATION: ${BUILD_CONFIGURATION:-Debug}
...
```

De controller maakt gebruik van een docker-bestand naar uw toepassing bouwen en uitvoeren.

De *build.context* eigenschap geeft een lijst van de map waar de docker-bestanden bestaan. De *build.dockerfile* eigenschap bepaalt de naam van de docker-bestand voor het bouwen van de productieversie van de toepassing. De *configurations.develop.build.dockerfile* eigenschap configureert u de naam van de docker-bestand voor de versie van de ontwikkeling van de toepassing.

Met andere docker-bestanden voor de ontwikkeling en productie, kunt u bepaalde dingen tijdens de ontwikkeling en uitschakelen van de items voor productie-implementaties. Bijvoorbeeld, kunt u fouten gaat opsporen of een uitgebreidere logboekregistratie tijdens de ontwikkeling en uitschakelen in een productieomgeving. U kunt deze eigenschappen ook bijwerken als uw docker-bestanden zijn een andere naam of op een andere locatie.

Als u wilt, kunt u snel herhalen tijdens de ontwikkeling wordt Azure Dev spaties wijzigingen uit uw lokale project synchroniseren en incrementeel bijwerken van uw toepassing. De onderstaande sectie in de `azds.yaml` configuratiebestand wordt gebruikt voor het configureren van de synchronisatie en bijwerken:

```yaml
...
configurations:
  develop:
    ...
    container:
      sync:
      - "**/Pages/**"
      - "**/Views/**"
      - "**/wwwroot/**"
      - "!**/*.{sln,csproj}"
      command: [dotnet, run, --no-restore, --no-build, --no-launch-profile, -c, "${BUILD_CONFIGURATION:-Debug}"]
      iterate:
        processesToKill: [dotnet, vsdbg]
        buildCommands:
        - [dotnet, build, --no-restore, -c, "${BUILD_CONFIGURATION:-Debug}"]
...
```

De bestanden en mappen die wijzigingen worden gesynchroniseerd, worden vermeld in de *configurations.develop.container.sync* eigenschap. Deze mappen zijn in eerste instantie gesynchroniseerd tijdens het uitvoeren van de `up` opdracht evenals wanneer wijzigingen worden gedetecteerd. Als er extra of andere mappen hebt die zijn gesynchroniseerd met de adresruimte van uw dev, kunt u deze eigenschap kunt wijzigen.

De *configurations.develop.container.iterate.buildCommands* -eigenschap geeft over het bouwen van de toepassing in een scenario voor het ontwikkelen. De *configurations.develop.container.command* eigenschap bevat de opdracht voor het uitvoeren van de toepassing in een scenario voor het ontwikkelen. U kunt een van deze eigenschappen bijwerken als er extra build of runtime vlaggen of parameters die u wilt gebruiken tijdens de ontwikkeling.

De *configurations.develop.container.iterate.processesToKill* geeft een lijst van de processen afsluiten om de toepassing te stoppen. Mogelijk wilt u deze eigenschap bijwerken als u wilt wijzigen van het gedrag voor opnieuw opstarten van uw toepassing tijdens de ontwikkeling. Als u bijvoorbeeld de *configurations.develop.container.iterate.buildCommands* of *configurations.develop.container.command* eigenschappen te wijzigen hoe de toepassing is gemaakt of aan de slag, u mogelijk wilt wijzigen welke processen zijn gestopt.

Tijdens het voorbereiden van uw code met de `azds prep` opdracht, u hebt de mogelijkheid van het toevoegen van de `--public` vlag. Toevoegen van de `--public` vlag maakt u een openbaar toegankelijke URL voor uw toepassing. Als u deze vlag niet opgeeft, is de toepassing alleen toegankelijk binnen het cluster of met behulp van de localhost-tunnel. Nadat u de `azds prep` opdracht, kunt u deze instelling wijzigen de *ingress.enabled* eigenschap in `charts/APPNAME/values.yaml`:

```yaml
ingress:
  enabled: true
```

## <a name="debug-your-code"></a>Fouten opsporen in uw code

U kunt uw toepassing die wordt uitgevoerd in de adresruimte van uw ontwikkelen met behulp van Visual Studio Code of Visual Studio 2017 rechtstreeks fouten opsporen voor Java, .NET en Node.js-toepassingen. Visual Studio Code en Visual Studio 2017 bieden hulpprogramma's om verbinding maken met uw dev-ruimte, start uw toepassing en voeg een debugger toe. Nadat het is uitgevoerd `azds prep`, kunt u uw project openen in Visual Studio Code of Visual Studio 2017. Visual Studio Code of Visual Studio 2017 hun eigen configuratiebestanden om verbinding te maken dat is gescheiden van het uitvoeren wordt gegenereerd `azds prep`. Vanuit Visual Studio Code of Visual Studio 2017, kunt u onderbrekingspunten instellen en starten van uw toepassing in de adresruimte van uw dev.

![Foutopsporing van uw code](media/get-started-node/debug-configuration-nodejs2.png)

Wanneer u uw toepassing met behulp van Visual Studio Code of Visual Studio 2017 voor foutopsporing starten, deze verwerken starten en verbinding maken met uw dev-ruimte op dezelfde manier als actief `azds up`. De client-side-hulpmiddelen in Visual Studio Code en Visual Studio 2017 bieden ook een extra parameter met de specifieke informatie voor foutopsporing. De parameter bevat de naam van het foutopsporingsprogramma afbeelding, de locatie van het foutopsporingsprogramma binnen in de afbeelding van de foutopsporing en de bestemmingslocatie van de in de container van de toepassing naar de map foutopsporingsprogramma koppelen. 

De installatiekopie van het foutopsporingsprogramma wordt automatisch bepaald door de client-side-hulpprogramma's. Een methode die vergelijkbaar is met de gebruikt tijdens de docker-bestand wordt gebruikt en Helm-diagram te genereren bij het uitvoeren van `azds prep`. Nadat het foutopsporingsprogramma in de afbeelding van de toepassing is gekoppeld, deze wordt uitgevoerd met behulp van `azds exec`.

## <a name="sharing-a-dev-space"></a>Delen van een dev-ruimte

Als u werkt met een team, kunt u [delen van een dev-ruimte op een hele team](how-to/share-dev-spaces.md) en afgeleide dev opslagruimten te maken. Een dev-ruimte kan worden gebruikt door iedereen met Inzender-toegang tot de resourcegroep de dev-ruimte.

U kunt ook een nieuwe dev-ruimte die is afgeleid van een andere dev ruimte maken. Wanneer u een spatie afgeleide ontwikkelen, maakt de *azds.io/bovenliggende-ruimte = naam van een BOVENLIGGENDE ruimte* label wordt toegevoegd aan de naamruimte van de afgeleide dev-ruimte. Ook worden alle toepassingen vanuit de bovenliggende dev-ruimte gedeeld met de afgeleide dev-ruimte. Als u een bijgewerkte versie van de afgeleide dev-ruimte van een toepassing implementeert, bestaat deze alleen in de afgeleide dev-ruimte en de bovenliggende dev-ruimte blijft ongewijzigd. U kunt maximaal drie niveaus van afgeleide dev-adresruimten hebben of *grootvader* spaties.

De afgeleide dev-ruimte wordt ook op een route aanvragen tussen een eigen toepassingen en de toepassingen van de bovenliggende gedeeld. De routering werkt volgens het probeert te route-aanvraag aan een toepassing in de ruimte afgeleide ontwikkelen en terug te vallen op de gedeelde toepassing vanuit de bovenliggende dev-ruimte. De routering wordt terugvallen op de gedeelde toepassing in de ruimte grootvader als de toepassing zich niet in de bovenliggende ruimte.

Bijvoorbeeld:
* De ruimte dev *standaard* toepassingen *serviceA* en *serviceB* .
* De ruimte dev *azureuser* is afgeleid van *standaard*.
* Een bijgewerkte versie van *serviceA* wordt geïmplementeerd op *azureuser*.

Bij het gebruik van *azureuser*, alle aanvragen naar *serviceA* worden doorgestuurd naar de bijgewerkte versie in *azureuser*. Een aanvraag voor *serviceB* wordt eerst geprobeerd om te worden doorgestuurd naar de *azureuser* versie van *serviceB*. Omdat deze niet bestaat, wordt deze doorgestuurd naar de *standaard* versie van *serviceB*. Als de *azureuser* versie van *serviceA* wordt verwijderd, alle aanvragen naar *serviceA* wordt terugvallen op met behulp van de *standaard* versie van *serviceA*.

## <a name="next-steps"></a>Volgende stappen

Als u wilt aan de slag met Azure Dev spaties, Zie de volgende snelstartgidsen:

* [Java met CLI en Visual Studio Code](quickstart-java.md)
* [.NET core met CLI en Visual Studio Code](quickstart-netcore.md)
* [.NET core met Visual Studio 2017](quickstart-netcore-visualstudio.md)
* [Node.js met CLI en Visual Studio Code](quickstart-nodejs.md)

Als u wilt beginnen met ontwikkelen in teamverband, Zie de volgende praktische artikelen:

* [Teamontwikkeling - Java met CLI en Visual Studio Code](team-development-java.md)
* [Teamontwikkeling - .NET Core met CLI en Visual Studio Code](team-development-netcore.md)
* [Teamontwikkeling - .NET Core met Visual Studio 2017](team-development-netcore-visualstudio.md)
* [Teamontwikkeling - Node.js met CLI en Visual Studio Code](team-development-nodejs.md)

---
title: Teamontwikkeling op Kubernetes met Azure Dev spaties
titleSuffix: Azure Dev Spaces
author: zr-msft
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.author: zarhoads
ms.date: 04/25/2019
ms.topic: quickstart
description: Kubernetes-ontwikkeling met containers en microservices op Azure-team
keywords: Docker, Kubernetes, Azure, AKS, Azure Kubernetes Service, containers, Helm, NET service, service mesh-routering, kubectl, k8s
manager: jeconnoc
ms.openlocfilehash: 74ed1981ba6bf62413d7d7bfd51282eb04afd403
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66393401"
---
# <a name="quickstart-team-development-on-kubernetes-using-azure-dev-spaces"></a>Quickstart: Teamontwikkeling op Kubernetes met Azure Dev spaties

In deze handleiding leert u het volgende:

- Azure Dev spaties ingesteld op een beheerde Kubernetes-cluster in Azure.
- Een grote toepassing met meerdere microservices implementeren naar een dev-ruimte.
- Een enkele microservice in een geïsoleerde dev-ruimte in de context van de volledige toepassing testen.

![Azure Dev spaties teams voor ontwikkelen](media/azure-dev-spaces/collaborate-graphic.gif)

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u geen Azure-abonnement hebt, kunt u een [gratis account](https://azure.microsoft.com/free) maken.
- [Azure CLI geïnstalleerd](/cli/azure/install-azure-cli?view=azure-cli-latest).
- [Helm 2,13 of hoger geïnstalleerd](https://github.com/helm/helm/blob/master/docs/install.md).

## <a name="create-an-azure-kubernetes-service-cluster"></a>Een Azure Kubernetes Service-cluster maken

Moet u een AKS-cluster in een [ondersteunde regio][supported-regions]. De onderstaande opdrachten maakt u een resourcegroep met de naam *MyResourceGroup* en een AKS-cluster met de naam *MyAKS*.

```cmd
az group create --name MyResourceGroup --location eastus
az aks create -g MyResourceGroup -n MyAKS --location eastus --node-vm-size Standard_DS2_v2 --node-count 1 --disable-rbac --generate-ssh-keys
```

De *MyAKS* cluster ook met één knooppunt is gemaakt met behulp van de *Standard_DS2_v2* grootte, en met RBAC uitgeschakeld.

## <a name="enable-azure-dev-spaces-on-your-aks-cluster"></a>Azure Dev spaties inschakelen in uw AKS-cluster

Gebruik de `use-dev-spaces` opdracht uit om te ontwikkelen spaties inschakelen in uw AKS-cluster en volg de aanwijzingen. De onderstaande opdracht schakelt u Dev opslagruimten op de *MyAKS* -cluster in de *MyResourceGroup* groeperen en maakt u een dev-ruimte met de naam *dev*.

```cmd
az aks use-dev-spaces -g MyResourceGroup -n MyAKS --space dev --yes
```

## <a name="get-sample-application-code"></a>Voorbeeldcode voor toepassing

In dit artikel gebruikt u de [voorbeeld-toepassing voor delen van Azure Dev spaties fiets](https://github.com/Azure/dev-spaces/tree/master/samples/BikeSharingApp) te laten zien hoe u Azure Dev spaties.

Kloon de toepassing uit GitHub en navigeer naar de bijbehorende map:

```cmd
git clone https://github.com/Azure/dev-spaces
cd dev-spaces/samples/BikeSharingApp/
```

## <a name="retrieve-the-hostsuffix-for-dev"></a>Ophalen van de HostSuffix voor *ontwikkelen*

Gebruik de `azds show-context` opdracht om weer te geven van de HostSuffix voor *dev*.

```cmd
$ azds show-context

Name                ResourceGroup     DevSpace  HostSuffix
------------------  ----------------  --------  -----------------------
MyAKS               MyResourceGroup   dev       fedcab0987.eus.azds.io
```

## <a name="update-the-helm-chart-with-your-hostsuffix"></a>Het Helm-diagram met uw HostSuffix bijwerken

Open [charts/values.yaml](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/charts/values.yaml) en Vervang alle exemplaren van `<REPLACE_ME_WITH_HOST_SUFFIX>` met de HostSuffix-waarde die u eerder hebt opgehaald. Sla uw wijzigingen op en sluit het bestand.

## <a name="run-the-sample-application-in-kubernetes"></a>De voorbeeldtoepassing uitvoeren in Kubernetes

De opdrachten voor het uitvoeren van de voorbeeldtoepassing op Kubernetes deel uitmaken van een bestaand proces en niet afhankelijk zijn van op Azure Dev spaties hulpprogramma's. In dit geval Helm is de tooling die wordt gebruikt voor het uitvoeren van deze voorbeeldtoepassing, maar andere hulpprogramma's kan worden gebruikt om uw gehele toepassing uitvoeren in een naamruimte binnen een cluster. De Helm-opdrachten zijn die gericht is op de dev-ruimte met de naam *dev* u eerder hebt gemaakt, maar deze dev-ruimte is ook een Kubernetes-naamruimten. Als gevolg hiervan kunnen ontwikkelen spaties doel zijn van andere hulpprogramma's hetzelfde als andere naamruimten.

Nadat een toepassing wordt uitgevoerd in een cluster, ongeacht de tooling die wordt gebruikt om het te implementeren, kunt u Azure Dev opslagruimten voor het ontwikkelen van een team.

Gebruik de `helm init` en `helm install` opdrachten voor het instellen en installeren van de voorbeeldtoepassing op uw cluster.

```cmd
cd charts/
helm init --wait
helm install -n bikesharing . --dep-up --namespace dev --atomic --wait
```

De `helm install` opdracht kan enkele minuten duren. De uitvoer van de opdracht toont de status van alle services die wordt geïmplementeerd in het cluster wanneer dit is voltooid:

```cmd
$ cd charts/
$ helm init --wait
...
Happy Helming!

$ helm install -n bikesharing . --dep-up --namespace dev --atomic --wait

Hang tight while we grab the latest from your chart repositories...
...
NAME               READY  UP-TO-DATE  AVAILABLE  AGE
bikes              1/1    1           1          4m32s
bikesharingweb     1/1    1           1          4m32s
billing            1/1    1           1          4m32s
gateway            1/1    1           1          4m32s
reservation        1/1    1           1          4m32s
reservationengine  1/1    1           1          4m32s
users              1/1    1           1          4m32s
```

Het voorbeeld de toepassing wordt geïnstalleerd op uw cluster en omdat u Dev spaties ingeschakeld op uw cluster hebt, gebruikt u de `azds list-uris` opdracht voor het weergeven van de URL's voor de voorbeeldtoepassing in *dev* die momenteel is geselecteerd.

```cmd
$ azds list-uris
Uri                                                 Status
--------------------------------------------------  ---------
http://dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://dev.gateway.fedcab0987.eus.azds.io/         Available
```

Navigeer naar de *bikesharingweb* service door het openen van de openbare URL van de `azds list-uris` opdracht. In het bovenstaande voorbeeld wordt de openbare URL voor de *bikesharingweb* -service is `http://dev.bikesharingweb.fedcab0987.eus.azds.io/`. Selecteer *Aurelia Briggs (klant)* als de gebruiker. Controleer of de tekst *Hi Aurelia Briggs | Afmelden* aan de bovenkant.

![Azure voorbeeld-toepassing voor delen van Dev spaties fiets](media/quickstart-team-development/bikeshare.png)

## <a name="create-child-dev-spaces"></a>Onderliggende dev opslagruimten maken

Gebruik de `azds space select` opdracht voor het maken van twee spaties voor onderliggende onder *dev*:

```cmd
azds space select -n dev/azureuser1 -y
azds space select -n dev/azureuser2 -y
```

De bovenstaande opdrachten maakt twee onderliggende spaties onder *dev* met de naam *azureuser1* en *azureuser2*. Deze twee onderliggende spaties staan voor afzonderlijke dev spaties voor de ontwikkelaars *azureuser1* en *azureuser2* moet worden gebruikt voor het aanbrengen van wijzigingen in de voorbeeldtoepassing.

Gebruik de `azds space list` opdracht lijst met alle dev spaties en bevestig *dev/azureuser2* is geselecteerd.

```cmd
$ azds space list
Name            Selected
--------------  --------
default         False
dev             False
dev/azureuser1  False
dev/azureuser2  True
```

Gebruik de `azds list-uris` om weer te geven van de URL's voor de voorbeeldtoepassing in de momenteel geselecteerde ruimte die is *dev/azureuser2*.

```cmd
$ azds list-uris
Uri                                                             Status
--------------------------------------------------              ---------
http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser2.s.dev.gateway.fedcab0987.eus.azds.io/         Available
```

Bevestig dat de URL's weergegeven door de `azds list-uris` opdracht hebben de *azureuser2.s.dev* voorvoegsel. Dit voorvoegsel wordt bevestigd dat de huidige ruimte die geselecteerd is *azureuser2*, dit is een onderliggend element van *dev*.

Navigeer naar de *bikesharingweb* service voor de *dev/azureuser2* dev ruimte door het openen van de openbare URL van de `azds list-uris` opdracht. In het bovenstaande voorbeeld wordt de openbare URL voor de *bikesharingweb* -service is `http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/`. Selecteer *Aurelia Briggs (klant)* als de gebruiker. Controleer of de tekst *Hi Aurelia Briggs | Meld u af* aan de bovenkant.

## <a name="update-code"></a>Code bijwerken

Open *BikeSharingWeb/components/Header.js* met een teksteditor en wijzig de tekst in de [span-element met de `userSignOut` className](https://github.com/Azure/dev-spaces/blob/master/samples/BikeSharingApp/BikeSharingWeb/components/Header.js#L16).

```html
<span className="userSignOut">
    <Link href="/devsignin"><span tabIndex="0">Welcome {props.userName} | Sign out</span></Link>
</span>
```

Sla uw wijzigingen op en sluit het bestand.

## <a name="build-and-run-the-updated-bikesharingweb-service-in-the-devazureuser2-dev-space"></a>Ontwikkelen en uitvoeren van de bijgewerkte bikesharingweb-service in de *dev/azureuser2* dev ruimte

Navigeer naar de *BikeSharingWeb /* Active directory en werken met de `azds up` opdracht.

```cmd
$ cd ../BikeSharingWeb/
$ azds up

Using dev space 'dev/azureuser2' with target 'MyAKS'
Synchronizing files...2s
...
Service 'bikesharingweb' port 'http' is available at http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/
Service 'bikesharingweb' port 80 (http) is available at http://localhost:54256
...
```

Met deze opdracht bouwt en wordt uitgevoerd de *bikesharingweb* service in de *dev/azureuser2* dev-ruimte. Deze service wordt uitgevoerd in aanvulling op de *bikesharingweb* -service wordt uitgevoerd *dev* en wordt alleen gebruikt voor aanvragen met de *azureuser2.s* URL-voorvoegsel. Zie voor meer informatie over de werking routering tussen bovenliggende en onderliggende dev spaties [hoe Azure Dev spaties werkt en is geconfigureerd](how-dev-spaces-works.md).

Navigeer naar de *bikesharingweb* service voor de *dev/azureuser2* dev ruimte door het openen van de openbare URL weergegeven in de uitvoer van de `azds up` opdracht. Selecteer *Aurelia Briggs (klant)* als de gebruiker. Controleer of dat u de bijgewerkte tekst in de rechterbovenhoek ziet. Mogelijk moet u de pagina vernieuwen of van uw browser cache wissen als u deze wijziging niet onmiddellijk ziet.

![Azure Dev spaties fiets voorbeeld-toepassing voor delen bijgewerkt](media/quickstart-team-development/bikeshare-update.png)

## <a name="verify-other-dev-spaces-are-unchanged"></a>Controleer of dat andere Dev spaties zijn niet gewijzigd

Als de `azds up` opdracht nog steeds actief is, drukt u op *Ctrl + c*.

```cmd
$ azds list-uris --all
Uri                                                             Status
--------------------------------------------------              ---------
http://azureuser1.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser1.s.dev.gateway.fedcab0987.eus.azds.io/         Available
http://azureuser2.s.dev.bikesharingweb.fedcab0987.eus.azds.io/  Available
http://azureuser2.s.dev.gateway.fedcab0987.eus.azds.io/         Available
http://dev.bikesharingweb.fedcab0987.eus.azds.io/               Available
http://dev.gateway.fedcab0987.eus.azds.io/                      Available
```

Navigeer naar de *dev* versie van *bikesharingweb* in uw browser, kies *Aurelia Briggs (klant)* als de gebruiker, en controleer of u de oorspronkelijke tekst in de rechterbovenhoek ziet hoek. Herhaal deze stappen met het *dev/azureuser1* URL. U ziet dat de wijzigingen worden alleen toegepast op de *dev/azureuser2* versie van *bikesharingweb*. Deze isolatie van wijzigingen in *dev/azureuser2* kunt u *azureuser2* aanbrengen zonder *azureuser1*.

Deze wijzigingen hebben *dev* en *dev/azureuser1*, moet u de bestaande werkstroom of het CI/CD-pijplijn van uw team volgen. Deze werkstroom zijn mogelijk doorvoeren van de wijziging aan uw versiebeheersysteem en implementeren van de update met behulp van een CI/CD-pijplijn of hulpprogramma's zoals Helm.

## <a name="clean-up-your-azure-resources"></a>Opschonen van uw Azure-resources

```cmd
az group delete --name MyResourceGroup --yes --no-wait
```

## <a name="next-steps"></a>Volgende stappen

Ontdek hoe Azure Dev Spaces u kan helpen om complexere apps te ontwikkelen binnen meerdere containers en hoe u ontwikkeling in samenwerkingsverband vereenvoudigt door in verschillende ruimten met verschillende versies of vertakkingen van uw code te werken.

> [!div class="nextstepaction"]
> [Werken met meerdere containers en teamontwikkeling](multi-service-nodejs.md)


[supported-regions]: about.md#supported-regions-and-configurations

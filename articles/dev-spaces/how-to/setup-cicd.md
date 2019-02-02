---
title: CI/CD met Azure Dev spaties | Microsoft Docs
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
ms.subservice: azds-kubernetes
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: article
manager: yuvalm
description: Snelle Kubernetes-ontwikkeling met containers en microservices in Azure
keywords: Docker, Kubernetes, Azure, AKS, Azure Container Service, containers
ms.openlocfilehash: 0abe2902248c8203046cfe891d136ca7d5d0a75b
ms.sourcegitcommit: de32e8825542b91f02da9e5d899d29bcc2c37f28
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/02/2019
ms.locfileid: "55666896"
---
# <a name="use-cicd-with-azure-dev-spaces"></a>CI/CD met Azure Dev opslagruimten gebruiken

In dit artikel begeleidt u bij het instellen van continue integratie/continue implementatie (CI/CD) naar Azure Kubernetes Service (AKS) met Dev spaties ingeschakeld. CI/CD naar AKS kunt app-updates worden automatisch geïmplementeerd wanneer toegezegde code naar uw opslagplaats wordt gepusht. CI/CD in combinatie met een Dev-ruimten met is ingeschakelde cluster nuttig omdat deze een basislijn van de toepassing voor het team om te werken met up-to-date kunt houden.

![Voorbeeld van de CI/CD-diagram](../media/common/ci-cd-simple.png)

Hoewel dit artikel u met Azure DevOps helpt, zou dezelfde concepten worden toegepast met CI/CD-systemen, zoals Jenkins, TeamCity, enzovoort.

## <a name="prerequisites"></a>Vereisten
* [Azure Kubernetes Service (AKS)-cluster met Azure Dev spaties ingeschakeld](../get-started-netcore.md)
* [Azure Dev spaties CLI geïnstalleerd](upgrade-tools.md)
* [Azure DevOps-organisatie, met een project](https://docs.microsoft.com/azure/devops/user-guide/sign-up-invite-teammates?view=vsts)
* [Azure Container Registry (ACR)](../../container-registry/container-registry-get-started-azure-cli.md)
    * Azure Container Registry [administrator-account](../../container-registry/container-registry-authentication.md#admin-account) details beschikbaar
* [Toestaan dat uw AKS-cluster om op te halen uit uw Azure Container Registry](../../container-registry/container-registry-auth-aks.md)

## <a name="download-sample-code"></a>Download voorbeeldcode
Voor de tijd, maken we een fork van onze GitHub-opslagplaats van de voorbeeld-code. Ga naar https://github.com/Azure/dev-spaces en selecteer **Fork**. Zodra de fork-proces voltooid is, **kloon** uw Gevorkte versie van de opslagplaats lokaal. Standaard de _master_ branch wordt uitgecheckt, maar we hebben enkele wijzigingen tijdbesparende in opgenomen de _azds_updates_ worden aangebracht, wat ook tijdens de fork moet zijn overgedragen. De _azds_updates_ vertakking we u handmatig aanbrengen in de zelfstudie secties Dev spaties, evenals enkele vooraf gemaakte YAML en JSON-bestanden vragen voor het stroomlijnen van de implementatie van het systeem CI/CD-updates bevat. U kunt een opdracht zoals `git checkout -b azds_updates origin/azds_updates` uitchecken van het _azds_updates_ vertakking in uw lokale opslagplaats.

## <a name="dev-spaces-setup"></a>Opslagruimten dev-setup
Maak een nieuwe ruimte met de naam _dev_ met behulp van de `azds space select` opdracht. De _dev_ ruimte wordt gebruikt door uw CI/CD-pijplijn om uw code. Het wordt ook worden gebruikt voor het maken _onderliggende spaties_ op basis van _dev_.

```cmd
azds space select -n dev
```

Wanneer u hierom wordt gevraagd om te selecteren van een bovenliggende dev-ruimte, selecteert u  _\<geen\>_.

De _dev_ ruimte bevat altijd de meest recente status van de opslagplaats, een basislijn, zodat ontwikkelaars kunnen maken _onderliggende spaties_ van _dev_ voor het testen van hun geïsoleerde wijzigingen in de context van de grotere app. Dit concept wordt in meer detail beschreven in de zelfstudies Dev spaties.

## <a name="creating-the-build-definition"></a>Het maken van de build-definitie
Open het project Azure DevOps-team in een webbrowser en navigeer naar de _pijplijnen_ sectie. Eerst, klikt u op uw profielfoto in de rechterbovenhoek van de site met Azure DevOps, open het deelvenster van de preview-functies en uitschakelen de _interface voor het maken van nieuwe YAML-pijplijn_:

![De functies voorbeeldvenster openen](../media/common/preview-feature-open.png)

De optie om uit te schakelen:

![Optie voor appervaring maken van nieuwe YAML-pijplijn](../media/common/yaml-pipeline-preview-feature.png)

> [!Note]
> Het Azure DevOps _interface voor het maken van nieuwe YAML-pijplijn_ preview-functie is strijdig met het maken van vooraf gedefinieerde pijplijnen bouwen op dit moment. U moet deze voorlopig uitschakelen als u wilt implementeren van onze vooraf gedefinieerde build-pijplijn.

In de _azds_updates_ vertakking die we hebben een eenvoudige opgenomen [Azure pijplijn YAML](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema?view=vsts&tabs=schema) die definieert de build-stappen die nodig zijn voor *mywebapi* en *webfrontend* .

Afhankelijk van de taal die u hebt gekozen, is de pijplijn YAML ingecheckt in een pad vergelijkbaar met: `samples/dotnetcore/getting-started/azure-pipelines.dotnetcore.yml`

Een pijplijn te maken van dit bestand:
1. Navigeer in uw DevOps-project hoofdpagina aan pijplijnen > bouwt
1. Selecteer de optie voor het maken van een **nieuw** build-pijplijn
1. Selecteer **GitHub** als de bron autoriseren met uw GitHub-account indien nodig, en selecteer de _azds_updates_ vertakking van uw Gevorkte versie van de opslagplaats van de sampleapp dev-opslagruimten
1. Selecteer **configuratie als code**, of **YAML**, als uw sjabloon
1. Nu krijgt u een pagina van de configuratie van uw build-pijplijn. Zoals hierboven vermeld voert u de taalspecifieke pad voor de **YAML-bestandspad**. Bijvoorbeeld: `samples/dotnetcore/getting-started/azure-pipelines.dotnetcore.yml`
1. Ga naar het tabblad variabelen
1. Handmatig toevoegen _dockerId_ als een variabele, is dat de gebruikersnaam van uw [administrator-account voor Azure Container Registry](../../container-registry/container-registry-authentication.md#admin-account). (Wordt vermeld in de vereisten van het artikel)
1. Handmatig toevoegen _dockerPassword_ als een variabele, is dat het wachtwoord van uw [administrator-account voor Azure Container Registry](../../container-registry/container-registry-authentication.md#admin-account). Zorg ervoor dat u opgeeft _dockerPassword_ als een geheim (door het vergrendelingspictogram) om veiligheidsredenen.
1. Selecteer **opslaan en in de wachtrij**

U hebt nu een CI-oplossing die u automatisch bouwt *mywebapi* en *webfrontend* voor elke update gepusht naar de _azds_updates_ vertakking van uw GitHub-fork. U kunt controleren of de Docker-installatiekopieën zijn gepusht te navigeren naar de Azure-portal, selecteert u uw Azure Container Registry en bladeren door de _opslagplaatsen_ tabblad:

![Azure Container Registry-opslagplaatsen](../media/common/ci-cd-images-verify.png)

## <a name="creating-the-release-definition"></a>Het maken van de release-definitie

1. Navigeer in uw DevOps-project hoofdpagina aan pijplijnen > Releases
1. Als u in een gloednieuwe DevOps-Project dat de definitie van een release nog geen bevat werkt, moet u eerst maken een definitie van een lege release voordat u doorgaat. De optie importeren wordt niet weergegeven in de gebruikersinterface, totdat u een bestaande release-definitie.
1. Aan de linkerkant, klik op de **+ nieuw** knop en klik vervolgens op **importeren van een pijplijn**
1. Selecteer het .json-bestand op `samples/release.json`
1. Klik op Ok. U ziet het deelvenster van de pijplijn is geladen met de release-definitie bewerken-pagina. Er zijn ook enkele rode waarschuwing pictogrammen die wijzen op cluster-specifieke gegevens die nog moeten worden geconfigureerd.
1. Aan de linkerkant van de pijplijn-deelvenster, klikt u op de **toevoegen van een artefact** Bel.
1. In de **bron** vervolgkeuzelijst, selecteer de build-pijplijn we iets eerder in dit document hebt gemaakt.
1. Voor de **standaardversie**, het is raadzaam kiezen **nieuwste vanuit de hoofdvertakking van de build-pijplijn**. U hoeft niet alle tags worden opgegeven.
1. Stel de **alias van de gegevensbron** naar `drop`. De vooraf gedefinieerde release taken gebruik **alias van de gegevensbron** , zodat deze moet worden ingesteld.
1. Klik op **Add**.
1. Klik nu op het bliksempictogram bolt op het zojuist gemaakte `drop` artefact bron, zoals hieronder wordt weergegeven:

    ![Release artefact continue implementatie instellen](../media/common/release-artifact-cd-setup.png)
1. Schakel de **trigger voor continue implementatie**
1. Nu gaat u naar het deelvenster Taken. De _dev_ fase moet worden geselecteerd en u moet worden gepresenteerd met drie rode vervolgkeuzelijst besturingselementen, zoals hieronder:

    ![Release-definitie instellen](../media/common/release-setup-tasks.png)
1. Geef de Azure-abonnement, resourcegroep en cluster die u met Azure Dev spaties.
1. Er mag slechts één sectie op dit moment; rood weergegeven de **Agent-taak** sectie. U kunt daar en geef **gehost Ubuntu 1604** als de agent-pool voor deze fase.
1. Beweeg de muisaanwijzer over de lijst met taken aan de bovenkant, selecteer _prod_.
1. Geef de Azure-abonnement, resourcegroep en cluster die u met Azure Dev spaties.
1. Onder **Agent-taak**, Configureer **gehost Ubuntu 1604** als de agent-pool.
1. Klik op **opslaan** in de rechterbovenhoek en **OK**.
1. Klik op **+ Release** (naast de knop Opslaan), en **maken van een release**.
1. Controleer of de nieuwste build van uw build-pijplijn is geselecteerd in de sectie artefacten en druk op **maken**.

Een proces voor geautomatiseerde release nu, de implementatie begint de *mywebapi* en *webfrontend* grafieken voor uw Kubernetes-cluster in de _dev_ ruimte op het hoogste niveau. U kunt de voortgang van uw versie van het Azure DevOps-webportal.

> [!TIP]
> Als uw versie is mislukt met een foutbericht weergegeven zoals *bijwerken is mislukt: time-out voor de voorwaarde*, probeert de schillen in uw cluster inspecteren [met behulp van het Kubernetes-dashboard](../../aks/kubernetes-dashboard.md). Als u de schillen zijn mislukt bij foutberichten worden weergegeven, zoals *is mislukt voor het ophalen van de installatiekopie "azdsexample.azurecr.io/mywebapi:122": rpc-fout: code = onbekend desc = foutbericht van daemon: Ophalen https://azdsexample.azurecr.io/v2/mywebapi/manifests/122: niet-geautoriseerde: verificatie is vereist*, kan het zijn dat uw cluster is niet geautoriseerd om op te halen uit uw Azure Container Registry. Zorg ervoor dat u hebt de [autoriseren van uw AKS-cluster om op te halen uit uw Azure Container Registry](../../container-registry/container-registry-auth-aks.md) vereiste.

U hebt nu een volledig geautomatiseerde CI/CD-pijplijn voor uw GitHub-fork van de voorbeeld-apps ontwikkelen spaties. Telkens wanneer die u doorvoeren en push-code, de build-pijplijn bouwt en push de *mywebapi* en *webfrontend* afbeeldingen aan uw aangepaste ACR-exemplaar. Klik de release-pijplijn implementeert het Helm-diagram voor elke app in de _dev_ ruimte op het cluster opslagruimten Dev-functionaliteit.

## <a name="accessing-your-dev-services"></a>Toegang tot uw _dev_ services
Na de implementatie, de _dev_ versie van *webfrontend* kunnen worden geopend met een openbare URL, zoals: `http://dev.webfrontend.<hash>.<region>.aksapp.io`.

U vindt deze URL met behulp van de *kubectl* CLI:
```cmd
kubectl get ingress -n dev webfrontend -o=jsonpath="{.spec.rules[0].host}"
```

## <a name="deploying-to-production"></a>In productie wilt nemen
Klik op **bewerken** op uw release-definitie en u ziet dat er een _prod_ fase gedefinieerd:

![Productiefase](../media/common/prod-env.png)

Handmatig een specifieke versie te promoveren _prod_ met behulp van de CI/CD-systeem in deze zelfstudie hebt gemaakt:
1. Open een eerdere versie van de DevOps-portal
1. Beweeg de muisaanwijzer over de fase 'prod'
1. Selecteer implementeren

![Niveau verhogen naar productie](../media/common/prod-promote.png)

Ons CI/CD-pipeline-voorbeeld maakt gebruik van variabelen te wijzigen van de DNS-voorvoegsel voor *webfrontend* , afhankelijk van welke omgeving wordt geïmplementeerd. Dus voor toegang tot uw services 'prod', kunt u een URL, zoals: `http://prod.webfrontend.<hash>.<region>.aksapp.io`.

Na de implementatie, u vindt deze URL met behulp van de *kubectl* CLI: <!-- TODO update below to use list-uris when the product has been updated to list non-azds ingresses #769297 -->

```cmd
kubectl get ingress -n prod webfrontend -o=jsonpath="{.spec.rules[0].host}"
```

## <a name="dev-spaces-instrumentation-in-production"></a>Ontwikkel spaties instrumentatie in productie
Hoewel Dev spaties instrumentation is ontworpen _niet_ om te worden gehinderd door normale werking van uw toepassing, kunt u uw productieworkloads in een Kubernetes-naamruimte die niet is ingeschakeld met Dev spaties. Met behulp van dit type Kubernetes-naamruimten betekent dat u moet een maken uw productie-naamruimte met de `kubectl` CLI, of uw CI/CD-systeem om het te maken tijdens de eerste Helm-implementatie toe te staan. _Selecteren van_ of anders is het maken van een spatie spaties Dev tooling wordt Dev spaties instrumentatie toe te voegen die naamruimte.

Hier volgt een voorbeeld van de naamruimte-structuur die ondersteuning biedt voor ontwikkelen van functies, de omgeving 'Ontw' _en_ productie, allemaal op één Kubernetes-cluster:

![Voorbeeld van de naamruimte-structuur](../media/common/cicd-namespaces.png)

> [!Tip]
> Als u al hebt gemaakt een `prod` ruimte, en zou gewoon als aan deze wordt uitgesloten van Dev spaties instrumentatie (zonder deze te verwijderen.), kunt u doen met de volgende Dev spaties CLI-opdracht:
>
> `azds space remove -n prod --no-delete`
>
> U wilt verwijderen van alle schillen in de `prod` naamruimte hierna, zodat ze opnieuw kunnen worden gemaakt zonder spaties Dev instrumentation.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over teamontwikkeling van Azure Dev spaties](../team-development-netcore.md)
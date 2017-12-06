---
title: Zelfstudie voor Azure Container register - Push een bijgewerkte installatiekopie naar regionale implementaties
description: "Een installatiekopie van het gewijzigde Docker push naar uw Azure geogerepliceerde register bevatten, en vervolgens de wijzigingen vervolgens automatisch geïmplementeerd in WebApps die worden uitgevoerd in meerdere regio's te zien. Deel 2 van een reeks drie delen."
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: 359fdcabd579d277e40f02eba2d4603ebd9f5f1f
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="push-an-updated-image-to-regional-deployments"></a>Een installatiekopie van een bijgewerkte push naar regionale implementaties

Dit maakt deel uit drie in een zelfstudie reeks drie delen. In de [vorige zelfstudie](container-registry-tutorial-deploy-app.md), geo-replicatie is geconfigureerd voor twee verschillende regionale Web-App-implementaties. In deze zelfstudie maakt u eerst wijzigen van de toepassing, klikt u vervolgens een nieuwe container-installatiekopie bouwen en dit doorgeven aan het register geogerepliceerde. Ten slotte kunt u deze wijziging automatisch door Azure Container register webhooks, in beide gevallen Web-App geïmplementeerd weergeven.

In deze zelfstudie is het laatste gedeelte in de reeks:

> [!div class="checklist"]
> * De webtoepassing HTML wijzigen
> * Opbouwen en labels van de Docker-afbeelding
> * De wijziging in Azure Container register push
> * De bijgewerkte app weergeven in twee verschillende regio 's

Als u de twee nog niet hebt geconfigureerd *Web-App voor Containers* regionale implementaties terug naar de vorige zelfstudie in de reeks [web-app implementeren in Azure Container register](container-registry-tutorial-deploy-app.md).

## <a name="modify-the-web-application"></a>De webtoepassing wijzigen

In deze stap moet u een wijziging aanbrengt in de webtoepassing die maximaal zichtbaar zijn wanneer u de installatiekopie van het bijgewerkte container in Azure Container register pushen.

Zoek de `AcrHelloworld/Views/Home/Index.cshtml` bestand in de toepassingsbron die u [gekloond vanuit GitHub](container-registry-tutorial-prepare-registry.md#get-application-code) in een vorige zelfstudie en geopend in uw favoriete teksteditor. Voeg de volgende regel onder de bestaande `<h1>` regel:

```html
<h1>MODIFIED</h1>
```

De gewijzigde `Index.cshtml` moet er ongeveer uitzien:

```html
@{
    ViewData["Title"] = "Azure Container Registry :: Geo-replication";
}
<style>
    body {
        background-image: url('images/azure-regions.png');
        background-size: cover;
    }
    .footer {
        position: fixed;
        bottom: 0px;
        width: 100%;
    }
</style>

<h1 style="text-align:center;color:blue">Hello World from:  @ViewData["REGION"]</h1>
<h1>MODIFIED</h1>
<div class="footer">
    <ul>
        <li>Registry URL: @ViewData["REGISTRYURL"]</li>
        <li>Registry IP: @ViewData["REGISTRYIP"]</li>
        <li>Registry Region: @ViewData["REGION"]</li>
    </ul>
</div>
```

## <a name="rebuild-the-image"></a>De installatiekopie van het opnieuw samenstellen

Nu dat u de webtoepassing hebt bijgewerkt, opnieuw worden opgebouwd de installatiekopie van de container. De naam van de volledig gekwalificeerde installatiekopie, met inbegrip van de aanmeldings-server-URL voor het label als voorheen gebruiken:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="push-image-to-azure-container-registry"></a>Push-installatiekopie in Azure Container register

Push-nu de bijgewerkte *acr helloworld* container installatiekopie toe aan het register geo-replicatie. U bent hier, uitvoeren van een enkel `docker push` opdracht voor het implementeren van de bijgewerkte installatiekopie met de Register-replica's in zowel de *VS-West* en *VS-Oost* regio's.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

Uitvoer ziet er ongeveer als volgt:

```bash
The push refers to a repository [uniqueregistryname.azurecr.io/acr-helloworld]
5b9454e91555: Pushed
d6803756744a: Layer already exists
b7b1f3a15779: Layer already exists
a89567dff12d: Layer already exists
59c7b561ff56: Layer already exists
9a2f9413d9e4: Layer already exists
a75caa09eb1f: Layer already exists
v1: digest: sha256:4c3f2211569346fbe2d1006c18cbea2a4a9dcc1eb3a078608cef70d3a186ec7a size: 1792
```

## <a name="view-the-webhook-logs"></a>Bekijk de logboeken van de webhook

Terwijl de installatiekopie wordt gerepliceerd, ziet u de Azure-Container register webhooks wordt geactiveerd.

Om te zien van de regionale webhooks die zijn gemaakt tijdens de implementatie van de container *Web-Apps voor Containers* in een vorige zelfstudie gaat u naar het register van de container in Azure portal en selecteer vervolgens **Webhooks**onder **SERVICES**.

![Container register Webhooks in de Azure portal][tutorial-portal-01]

Selecteer elke Webhook om te zien van de geschiedenis van de aanroepen en antwoorden. Er is een rij voor de **push** actie in de logboeken van beide Webhooks. Hier het logboek voor de Webhook zich in de *VS-West* regio toont de **push** actie geactiveerd door de `docker push` in de vorige stap:

![Container register Webhook logboek in de Azure portal (VS-West)][tutorial-portal-02]

## <a name="view-the-updated-web-app"></a>De bijgewerkte web-app weergeven

De Webhooks waarschuwen Web-Apps dat een nieuwe installatiekopie in het register, die automatisch wordt de bijgewerkte container geïmplementeerd naar de twee regionale web-apps is is geactiveerd.

Controleer of dat de toepassing is bijgewerkt in beide implementaties door te navigeren naar beide regionale Web-App-implementaties in uw webbrowser. Als een herinnering vindt u de URL voor de geïmplementeerde web-app in de rechterbovenhoek van elke overzichtstabblad van App Service.

![Overzicht van App Service in de Azure portal][tutorial-portal-03]

Overzicht van de bijgewerkte toepassing, selecteer de koppeling in het overzicht van App Service. Hier volgt een van de voorbeeldweergave van de app actief *VS-West*:

![Browserweergave van gewijzigde web-app wordt uitgevoerd in de regio VS-West][deployed-app-westus-modified]

Controleer of de installatiekopie van het bijgewerkte container ook is geïmplementeerd naar de *VS-Oost* implementatie door weer te geven in uw browser.

![Browserweergave van gewijzigde web-app wordt uitgevoerd in de regio VS-Oost][deployed-app-eastus-modified]

Met één `docker push`u beide regionale Web-App-implementaties hebt bijgewerkt en Azure Container register geleverd de container-installatiekopieën van netwerk-en close-opslagplaatsen.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie, bijgewerkt en een nieuwe versie van de webcontainer van de toepassing toe aan het register geogerepliceerde gepusht. Webhooks in Azure Container register gewaarschuwd Web-Apps voor Containers van de update, wat een lokale pull vanuit de Register-replica's.

In dit de laatste zelfstudie in de reeks u:

> [!div class="checklist"]
> * De webtoepassing HTML bijgewerkt
> * Gebouwd en de Docker-afbeelding met tags
> * De wijziging in Azure Container register gepusht
> * De bijgewerkte app weergegeven in twee verschillende regio 's

<!-- IMAGES -->
[deployed-app-eastus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-eastus-modified.png
[deployed-app-westus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-westus-modified.png
[local-container-01]: ./media/container-registry-tutorial-deploy-update/local-container-01.png
[tutorial-portal-01]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-01.png
[tutorial-portal-02]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-02.png
[tutorial-portal-03]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-03.png
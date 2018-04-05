---
title: 'Zelfstudie voor Azure Container Registry: een bijgewerkte installatiekopie naar regionale implementaties pushen'
description: Push een aangepaste Docker-installatiekopie naar uw geo-gerepliceerde Azure-containerregister. Vervolgens worden de wijzigingen automatisch geïmplementeerd in web-apps die in meerdere regio's worden uitgevoerd. Deel drie van een serie van drie.
services: container-registry
author: mmacy
manager: timlt
ms.service: container-registry
ms.topic: tutorial
ms.date: 10/24/2017
ms.author: marsma
ms.custom: mvc
ms.openlocfilehash: f8eab93d1e6633ae4f17c5bb4836d96629d55cd4
ms.sourcegitcommit: d74657d1926467210454f58970c45b2fd3ca088d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/28/2018
---
# <a name="tutorial-push-an-updated-image-to-regional-deployments"></a>Zelfstudie: een bijgewerkte installatiekopie naar regionale implementaties pushen

Dit is deel drie van een serie met drie zelfstudies. In de [vorige zelfstudie](container-registry-tutorial-deploy-app.md) is geo-replicatie geconfigureerd voor twee verschillende regionale web-app-implementaties. In deze zelfstudie past u eerst de app aan, maakt u daarna een nieuwe containerinstallatiekopie en pusht u deze vervolgens naar het geo-gerepliceerde register. Ten slotte kunt u deze wijziging, die automatisch wordt geïmplementeerd door Azure Container Registry-webhooks, in beide web-app-instanties weergeven.

Deze zelfstudie, het laatste deel in de serie, gaat over het volgende:

> [!div class="checklist"]
> * De web-app-HTML wijzigen
> * De Docker-installatiekopie maken en taggen
> * De wijziging naar Azure Container Registry pushen
> * De bijgewerkte app in twee verschillende regio's weergeven

Als u de twee regionale *Web App for Containers*-implementaties nog niet hebt geconfigureerd, gaat u naar de vorige zelfstudie in de serie: [Web-app implementeren vanuit Azure Container Registry](container-registry-tutorial-deploy-app.md).

## <a name="modify-the-web-application"></a>De webtoepassing wijzigen

In deze stap brengt u een wijziging aan in de web-app die zeer zichtbaar is wanneer u de bijgewerkte containerinstallatiekopie naar Azure Container Registry pusht.

Zoek het bestand `AcrHelloworld/Views/Home/Index.cshtml` in de app-bron die u in een vorige zelfstudie hebt [gekloond vanuit GitHub](container-registry-tutorial-prepare-registry.md#get-application-code) en open het in uw favoriete teksteditor. Voeg de volgende regel toe onder de bestaande regel `<h1>`:

```html
<h1>MODIFIED</h1>
```

De aangepaste `Index.cshtml` moet er ongeveer zo uitzien:

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

## <a name="rebuild-the-image"></a>De installatiekopie herbouwen

Nu u de web-app hebt bijgewerkt, moet u de bijbehorende containerinstallatiekopie herbouwen. Gebruik net als eerder de naam van de volledig gekwalificeerde installatiekopie, met inbegrip van de URL voor de aanmeldingsserver, voor de tag:

```bash
docker build . -f ./AcrHelloworld/Dockerfile -t <acrName>.azurecr.io/acr-helloworld:v1
```

## <a name="push-image-to-azure-container-registry"></a>Installatiekopie pushen naar Azure Container Registry

Push nu de bijgewerkte containerinstallatiekopie *acr-helloworld* naar uw geo-gerepliceerde register. Hier voert u een enkele `docker push`-opdracht uit om de bijgewerkte installatiekopie naar de registerreplica's in de regio's *VS West* en *VS Oost* te implementeren.

```bash
docker push <acrName>.azurecr.io/acr-helloworld:v1
```

De uitvoer ziet er ongeveer als volgt uit:

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

## <a name="view-the-webhook-logs"></a>Logboeken van webhooks bekijken

Terwijl de installatiekopie wordt gerepliceerd, worden de Azure Container Registry-webhooks geactiveerd.

Als u de regionale webhooks wilt bekijken die zijn gemaakt tijdens de implementatie van de container naar *Web App for Containers* in een vorige zelfstudie, gaat u naar het containerregister in Azure Portal en selecteert u onder **SERVICES** de optie **Webhooks**.

![Webhooks van containerregisters in Azure Portal][tutorial-portal-01]

Selecteer elke webhook om de geschiedenis van de bijbehorende aanroepen en antwoorden te bekijken. U zou een rij moeten zien voor de actie **push** in de logboeken van beide webhooks. Hier toont het logboek voor de webhook in de regio *VS West* de actie **push** die is geactiveerd door de `docker push` in de vorige stap:

![Logboek voor containerregister-webhook in Azure Portal (VS West)][tutorial-portal-02]

## <a name="view-the-updated-web-app"></a>Bijgewerkte web-app weergeven

De webhooks geven door aan de web-apps dat een nieuwe installatiekopie naar het register is gepusht, en hierdoor wordt automatisch de bijgewerkte container naar de twee regionale web-apps geïmplementeerd.

Controleer of de app in beide implementaties is bijgewerkt door naar beide regionale web-app-implementaties te navigeren in uw webbrowser. U kunt de URL voor de geïmplementeerde web-app vinden in de rechterbovenhoek van elk App Service-overzichtstabblad.

![App Service-overzicht in Azure Portal][tutorial-portal-03]

Selecteer de link in het App Service-overzicht om de bijgewerkte app te bekijken. Hier volgt een voorbeeldweergave van de app die wordt uitgevoerd in *VS West*:

![Browserweergave van gewijzigde web-app die wordt uitgevoerd in de regio VS West][deployed-app-westus-modified]

Controleer of de bijgewerkte containerinstallatiekopie ook is geïmplementeerd naar *VS Oost* door deze in uw browser weer te geven.

![Browserweergave van gewijzigde web-app die wordt uitgevoerd in de regio VS Oost][deployed-app-eastus-modified]

Met één `docker push` hebt u beide regionale web-app-implementaties bijgewerkt, en Azure Container Registry heeft de containerinstallatiekopieën vanuit opslagplaatsen dicht bij het netwerk geleverd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een nieuwe versie van de web-app-container bijgewerkt en naar het geo-gerepliceerde register gepusht. Webhooks in Azure Container Registry gaven de update door aan Web App for Containers, waardoor een lokale pull vanuit de registerreplica's werd geactiveerd.

In deze zelfstudie, de laatste in deze serie, hebt u het volgende gedaan:

> [!div class="checklist"]
> * U hebt de web-app-HTML gewijzigd
> * U hebt de Docker-installatiekopie gemaakt en getagd
> * U hebt de wijziging naar Azure Container Registry gepusht
> * U hebt de bijgewerkte app in twee verschillende regio's weergegeven

<!-- IMAGES -->
[deployed-app-eastus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-eastus-modified.png
[deployed-app-westus-modified]: ./media/container-registry-tutorial-deploy-update/deployed-app-westus-modified.png
[local-container-01]: ./media/container-registry-tutorial-deploy-update/local-container-01.png
[tutorial-portal-01]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-01.png
[tutorial-portal-02]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-02.png
[tutorial-portal-03]: ./media/container-registry-tutorial-deploy-update/tutorial-portal-03.png
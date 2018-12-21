---
title: Een statische HTML-web-app maken - Azure App Service | Microsoft Docs
description: Ontdek hoe eenvoudig het is om web-apps uit te voeren in App Service door een voorbeeld van een statische HTML-app te implementeren.
services: app-service\web
documentationcenter: ''
author: msangapu
manager: jeconnoc
editor: ''
ms.assetid: 60495cc5-6963-4bf0-8174-52786d226c26
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 08/28/2018
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: c7d01d4c079eacfd2af1ebad4ac41d70f792a0bb
ms.sourcegitcommit: 1c1f258c6f32d6280677f899c4bb90b73eac3f2e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2018
ms.locfileid: "53253563"
---
# <a name="create-a-static-html-web-app-in-azure"></a>Een statische HTML-web-app maken in Azure

[Azure Web Apps](app-service-web-overview.md) biedt een uiterst schaalbare webhostingservice met self-patchfunctie. Deze Quickstart laat zien hoe u een eenvoudige HTML+CSS-site naar Azure Web Apps implementeert. U gaat deze Quickstart in [Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) doen, maar u kunt deze opdrachten ook lokaal uitvoeren met [Azure CLI](/cli/azure/install-azure-cli).

![Startpagina van voorbeeld-app](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="install-web-app-extension-for-cloud-shell"></a>Web-appuitbreiding voor Cloud Shell installeren

Om deze Quickstart te kunnen doen, installeert u de web-appuitbreiding met [az extension add](https://docs.microsoft.com/cli/azure/extension?view=azure-cli-latest#az-extension-add). Als de extensie al is geïnstalleerd, moet u deze bijwerken naar de nieuwste versie. U kunt de web-appuitbreiding bijwerken door `az extension update -n webapp` te typen.

Voer de volgende opdracht uit om het web-appuitbreiding te installeren:

```bash
az extension add --name webapp
```

Wanneer de uitbreiding is geïnstalleerd, toont de Cloud Shell gegevens voor het volgend voorbeeld:

```bash
The installed extension 'webapp' is in preview.
```

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Maak een map 'snelstart' in de Cloud Shell en ga er vervolgens naartoe.

```bash
mkdir quickstart

cd quickstart
```

Voer vervolgens de volgende opdracht uit om de voorbeeld-app-opslagplaats te klonen naar de map 'snelstart'.

```bash
git clone https://github.com/Azure-Samples/html-docs-hello-world.git
```

## <a name="create-a-web-app"></a>Een webtoepassing maken

Ga naar de map die de voorbeeldcode bevat en voer de opdracht `az webapp up` uit.

Vervang in de volgende opdracht <app-naam> door een unieke app-naam.

```bash
cd html-docs-hello-world

az webapp up --location westeurope --name <app_name>
```

Met de opdracht `az webapp up` worden de volgende acties uitgevoerd:

- Er wordt een standaardresourcegroep gemaakt.

- Er wordt een standaardserviceplan voor de app gemaakt.

- Er wordt een app met de opgegeven naam gemaakt.

- Er worden via zip bestanden van de huidige werkmap naar de web-app [geïmplementeerd](https://docs.microsoft.com/azure/app-service/app-service-deploy-zip).

Het kan enkele minuten duren voor deze opdracht is uitgevoerd. De opdracht geeft informatie weer die lijkt op het volgende voorbeeld:

```json
{
  "app_url": "https://<app_name>.azurewebsites.net",
  "location": "westeurope",
  "name": "<app_name>",
  "os": "Windows",
  "resourcegroup": "appsvc_rg_Windows_westeurope",
  "serverfarm": "appsvc_asp_Windows_westeurope",
  "sku": "FREE",
  "src_path": "/home/<username>/quickstart/html-docs-hello-world ",
  < JSON data removed for brevity. >
}
```

Noteer de waarde van `resourceGroup`. U hebt deze waarde nodig voor de sectie [Resources opschonen](#clean-up-resources).

## <a name="browse-to-the-app"></a>Bladeren naar de app

Ga in een browser naar de URL van de Azure-web-app: `http://<app_name>.azurewebsites.net`.

De pagina wordt als een web-app uitgevoerd in Azure App Service.

![Startpagina van voorbeeld-app](media/app-service-web-get-started-html/hello-world-in-browser-az.png)

**Gefeliciteerd!** U hebt uw eerste HTML-app geïmplementeerd in App Service.

## <a name="update-and-redeploy-the-app"></a>De app bijwerken en opnieuw implementeren

Typ `nano index.html` in Cloud Shell om de nano-teksteditor te openen. Wijzig in de tag van de `<h1>`-kop 'Azure App Service - Sample Static HTML Site' in 'Azure App Service', zoals hieronder wordt weergegeven.

![Nano index.html](media/app-service-web-get-started-html/nano-index-html.png)

Sla uw wijzigingen op en sluit nano af. Sla op met de opdracht `^O` en sluit af met `^X`.

U gaat de app nu opnieuw implementeren met dezelfde opdracht `az webapp up`.

```bash
az webapp up --location westeurope --name <app_name>
```

Wanneer de implementatie is voltooid, gaat u terug naar het browservenster dat is geopend in de stap **Bladeren naar de app** en vernieuwt u de pagina.

![Bijgewerkte startpagina van voorbeeld-app](media/app-service-web-get-started-html/hello-azure-in-browser-az.png)

## <a name="manage-your-new-azure-web-app"></a>Uw nieuwe Azure-web-app beheren

Ga naar <a href="https://portal.azure.com" target="_blank">Azure Portal</a> om de web-app te beheren die u hebt gemaakt.

Klik in het linkermenu op **App Services** en klik op de naam van uw Azure-web-app.

![Navigatie in de portal naar de Azure-web-app](./media/app-service-web-get-started-html/portal1.png)

De pagina Overzicht van uw web-app wordt weergegeven. Hier kunt u algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen.

![App Service-blade in Azure Portal](./media/app-service-web-get-started-html/portal2.png)

Het linkermenu bevat een aantal pagina's voor het configureren van uw app.

## <a name="clean-up-resources"></a>Resources opschonen

In de voorgaande stappen hebt u Azure-resources in een resourcegroep gemaakt. Als u deze resources niet meer nodig denkt te hebben, verwijdert u de resourcegroep door de volgende opdracht in Cloud Shell uit te voeren. De naam van de resourcegroep is automatisch gegenereerd in de stap [Een web-app maken](#create-a-web-app).

```bash
az group delete --name appsvc_rg_Windows_westeurope
```

Het kan een minuut duren voordat deze opdracht is uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Aangepast domein toewijzen](app-service-web-tutorial-custom-domain.md)

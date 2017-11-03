---
title: Een CDN toevoegen aan een Azure App Service | Microsoft Docs
description: Voeg een netwerk voor contentlevering toe aan een Azure App Service, zodat uw statische bestanden altijd worden gecached en geleverd vanaf een server dicht bij uw klanten, overal ter wereld.
services: app-service\web
author: syntaxc4
ms.author: cfowler
ms.date: 05/31/2017
ms.topic: tutorial
ms.service: app-service-web
manager: erikre
ms.workload: web
ms.custom: mvc
ms.openlocfilehash: 257b75d01f3904661c1a188a2d53ffcb74f48f06
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="add-a-content-delivery-network-cdn-to-an-azure-app-service"></a>Een netwerk voor contentlevering toevoegen aan een Azure App Service

[Azure Content Delivery Network (CDN)](../cdn/cdn-overview.md) slaat op strategisch geplaatste locaties statische webinhoud in de cache op om een maximale doorvoer voor de levering van inhoud te waarborgen. De CDN vermindert ook de serverbelasting van uw web-app. In deze zelfstudie leest u hoe u Azure CDN toevoegt aan een [web-app in de Azure App Service](app-service-web-overview.md). 

Dit is de startpagina van de statische HTML-voorbeeldsite die u gaat gebruiken:

![Startpagina van voorbeeld-app](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

Wat u leert het volgende:

> [!div class="checklist"]
> * Een CDN-eindpunt maken.
> * Assets in cache vernieuwen.
> * Queryreeksen gebruiken voor het beheren van versies in cache.
> * Een aangepast domein gebruiken voor het CDN-eindpunt.

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- [Git installeren](https://git-scm.com/)
- [Installeer Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-the-web-app"></a>De web-app maken

Volg voor het maken van de web-app u met werkt de [statische HTML-Quick Start](app-service-web-get-started-html.md) via de **Blader naar de app** stap.

### <a name="have-a-custom-domain-ready"></a>Houd een aangepast domein bij de hand

Als u wilt de stap van het aangepaste domein van deze zelfstudie hebt voltooid, moet u eigenaar bent van een aangepast domein en toegang hebben tot uw DNS-register voor uw domeinprovider (zoals GoDaddy). Als u bijvoorbeeld DNS-vermeldingen voor `contoso.com` en `www.contoso.com` wilt toevoegen, moet u de DNS-instellingen voor het hoofddomein van `contoso.com` kunnen configureren.

Als u nog geen domeinnaam hebt, overweeg dan de [Zelfstudie over App Service-domeinen](custom-dns-web-site-buydomains-web-app.md) te volgen om een domein aan te schaffen met de Azure Portal. 

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Open een browser en navigeer naar de [Azure Portal](https://portal.azure.com).

## <a name="create-a-cdn-profile-and-endpoint"></a>Een CDN-profiel en -eindpunt maken

Selecteer in het navigatiedeelvenster links **App Services** en selecteer vervolgens de app die u hebt gemaakt in de [Quickstart voor statische HTML](app-service-web-get-started-html.md).

![Een App Service-app in de portal selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

Selecteer op de pagina **App Service** in het gedeelte **Instellingen** de optie **Netwerken > Azure CDN configureren voor uw app**.

![CDN selecteren in de portal](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

Geef op de pagina **Azure Content Delivery Network** de instellingen voor **Nieuw eindpunt** op zoals aangegeven in de tabel.

![Een profiel en eindpunt maken in de portal](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Instelling | Voorgestelde waarde | Beschrijving |
| ------- | --------------- | ----------- |
| **CDN-profiel** | myCDNProfile | Selecteer **nieuw** een CDN-profiel maken. Een CDN-profiel is een verzameling van CDN-eindpunten van dezelfde prijscategorie. |
| **Prijscategorie** | Standard Akamai | De [prijscategorie](../cdn/cdn-overview.md#azure-cdn-features) geeft de provider en de beschikbare functies aan. In deze zelfstudie gebruiken we standaard Akamai. |
| **Naam van CDN-eindpunt** | Een unieke naam in het domein azureedge.net | U heeft toegang tot uw resources in de cache via het domein  *\<endpointname>.azureedge.net*.

Selecteer **Maken**.

Azure maakt het profiel en het eindpunt. Het nieuwe eindpunt wordt weergegeven in de lijst **Eindpunten** op dezelfde pagina, wanneer het de status **Actief** heeft.

![Nieuw eindpunt in lijst](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-the-cdn-endpoint"></a>Het CDN-eindpunt testen

Als u de prijscategorie Verizon hebt geselecteerd, duurt het doorgaans ongeveer 90 minuten voor het eindpunt is ingericht. In Akamai duurt het inrichten enkele minuten

De voorbeeld-app heeft een `index.html`-bestand en de mappen *css*, *img* en *js* met andere statische assets. De inhoudspaden voor al deze bestanden zijn op het CDN-eindpunt hetzelfde. De volgende URL's bieden bijvoorbeeld toegang tot het bestand *bootstrap.css* in de map *css*:

```
http://<appname>.azurewebsites.net/css/bootstrap.css
```

```
http://<endpointname>.azureedge.net/css/bootstrap.css
```

Navigeer in een browser naar de volgende URL:

```
http://<endpointname>.azureedge.net/index.html
```

![Startpagina van voorbeeldapp waarvoor de gegevens vanuit CDN worden geleverd](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page-cdn.png)

 U ziet dat dezelfde pagina die u eerder in een Azure-web-app hebt uitgevoerd. Azure CDN is opgehaald van de oorsprong web-app activa en is behoeve van het CDN-eindpunt

Vernieuw de pagina om ervoor te zorgen dat deze pagina is opgeslagen in de cache in het CDN. Er zijn soms twee aanvragen van hetzelfde asset vereist voordat het CDN de gevraagde inhoud in de cache opslaat.

Zie voor meer informatie over het maken van Azure CDN-profielen en -eindpunten [Aan de slag met Azure CDN](../cdn/cdn-create-new-endpoint.md).

## <a name="purge-the-cdn"></a>Het CDN leegmaken

Het CDN vernieuwt periodiek de behorende resources uit de oorspronkelijke web-app op basis van de configuratie van de time-to-live (TTL). De standaard TTL-waarde is zeven dagen.

Soms moet u wellicht het CDN vernieuwen voordat de TTL is verlopen. Bijvoorbeeld wanneer u bijgewerkte inhoud in de web-app implementeert. U kunt de resources in het CDN handmatig opschonen om te vernieuwen. 

In dit gedeelte van de zelfstudie implementeert u een wijziging in de web-app en schoont u het CDN op zodat de bijbehorende cache wordt vernieuwd.

### <a name="deploy-a-change-to-the-web-app"></a>Een wijziging implementeren in de web-app

Open het bestand `index.html` en voeg toe '- V2' toe aan de kop H1, zoals wordt weergegeven in het volgende voorbeeld: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

Voer uw wijziging door en implementeer deze naar de web-app.

```bash
git commit -am "version 2"
git push azure master
```

Als de implementatie is voltooid, ziet u de wijziging als u naar de URL vn de web-app bladert.

```
http://<appname>.azurewebsites.net/index.html
```

![V2 in de titel van de web-app](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Als u naar de CDN-eindpunt-URL van de startpagina bladert, wordt de wijziging niet weergegeven omdat de in de cache opgeslagen versie in het CDN nog niet is verlopen. 

```
http://<endpointname>.azureedge.net/index.html
```

![Geen V2 in de titel van het CDN](media/app-service-web-tutorial-content-delivery-network/no-v2-in-cdn-title.png)

### <a name="purge-the-cdn-in-the-portal"></a>Het CDN leegmaken in de portal

Schoon het CDN op zodat het de in de cache opgeslagen versie vernieuwt.

Selecteer in de linkernavigatie in de portal de optie **Resourcegroepen** en selecteer vervolgens de resourcegroep die u hebt gemaakt voor uw web-app (myResourceGroup).

![Resourcegroep selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-group.png)

Selecteer uw CDN-eindpunt in de lijst met resources.

![Eindpunt selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-endpoint.png)

Klik boven aan de pagina **Eindpunt** op **Leegmaken**.

![Leegmaken selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Typ u de inhoudspaden die u wilt leegmaken. Als u een afzonderlijk bestand wilt verwijderen, geeft u het volledige bestandspad door. Als u een map wilt leegmaken en vernieuwen, geeft u een padsegment door. Zorg ervoor dat `index.html` een van de paden is, gezien u deze hebt gewijzigd.

Klik onder aan de pagina op **Leegmaken**.

![Pagina leegmaken](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-the-cdn-is-updated"></a>Controleren of het CDN is bijgewerkt

Wacht totdat de aanvraag voor het leegmaken is verwerkt. Dit duurt doorgaans enkele minuten. Selecteer het belpictogram boven aan de pagina om de huidige status te bekijken. 

![Melding voor leegmaken](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

Blader naar de URL van het CDN-eindpunt voor `index.html`. U ziet nu de V2 die u hebt toegevoegd aan de titel op de startpagina. U ziet dat de CDN-cache is vernieuwd.

```
http://<endpointname>.azureedge.net/index.html
```

![V2 in de titel van het CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

Zie voor meer informatie [Een Azure CDN-eindpunt leegmaken](../cdn/cdn-purge-endpoint.md). 

## <a name="use-query-strings-to-version-content"></a>Queryreeksen gebruiken om inhoud een versie te geven

Het Azure CDN biedt de volgende opties voor cachegedrag:

* Queryreeksen negeren
* Queryreeksen in de cache opslaan overslaan
* Elke unieke URL in de cache opslaan 

De eerste hiervan is de standaard, wat betekent dat er slechts één versie van een asset ongeacht de queryreeks in de URL-cache. 

In dit gedeelte van de zelfstudie wijzigt u het cachegedrag zodat elke unieke URL in de cache wordt opgeslagen.

### <a name="change-the-cache-behavior"></a>Het gedrag van de cache wijzigen

Selecteer in de Azure Portal op de pagina **CDN-eindpunt** de optie **Cache**.

Selecteer **Elke unieke URL in de cache opslaan** in de vervolgkeuzelijst **Cachegedrag van queryreeks**.

Selecteer **Opslaan**.

![Selecteer cachegedrag van queryreeks](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Controleren of de unieke URL's afzonderlijk in de cache worden opgeslagen

Navigeer in een browser naar de startpagina op het CDN-eindpunt, maar de volgende queryreeks toe: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

Het CDN retourneert de huidige inhoud van de web-app, met 'V2' in de kop. 

Vernieuw de pagina om ervoor te zorgen dat deze pagina is opgeslagen in de cache in het CDN. 

Open `index.html`, wijzig 'V2' in 'V3' en implementeer de wijziging. 

```bash
git commit -am "version 3"
git push azure master
```

Ga in een browser naar de URL van het CDN-eindpunt met een nieuwe querytekenreeks zoals `q=2`. Het CDN haalt het huidige `index.html`-bestand op en 'V3' wordt weergegeven.  Als u echter naar het CDN-eindpunt navigeert met de querytekenreeks `q=1`, ziet u 'V2'.

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 in de titel in het CDN, queryreeks 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 in de titel in het CDN, queryreeks 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

Deze uitvoer toont elke queryreeks anders wordt behandeld:

* q = 1 werd gebruikt voorafgaand aan, zodat inhoud in cache (V2) worden geretourneerd.
* q = 2 is er nieuw is, zodat de meest recente inhoud voor web-app zijn opgehaald en (V3) geretourneerd.

Zie [Cachegedrag in Azure CDN bepalen met queryreeksen](../cdn/cdn-query-string.md) voor meer informatie.

## <a name="map-a-custom-domain-to-a-cdn-endpoint"></a>Een aangepast domein aan een CDN-eindpunt toewijzen

U wijst uw aangepaste domein toe aan uw CDN-eindpunt door een CNAME-record te maken. Een CNAME-record is een DNS-functie die een brondomein toewijst aan een doeldomein. U kunt bijvoorbeeld `cdn.contoso.com` of `static.contoso.com` toewijzen aan `contoso.azureedge.net`.

Als u geen aangepaste domeinnaam hebt, overweeg dan de [Zelfstudie over App Service-domeinen](custom-dns-web-site-buydomains-web-app.md) te volgen om een domein aan te schaffen met de Azure Portal. 

### <a name="find-the-hostname-to-use-with-the-cname"></a>De hostnaam voor gebruik met de CNAME vinden

Zorg er in de Azure Portal op de pagina **Eindpunt** voor dat **Overzicht** is geselecteerd in de navigatiebalk aan de linkerkant en selecteer vervolgens de knop **+ Aangepast domein** boven aan de pagina.

![Aangepast domein toevoegen selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

Op de pagina **Een aangepast domein toevoegen** ziet u de naam van de eindpunthost die u kunt gebruiken om een CNAME-record te maken. De hostnaam is afgeleid van de URL van uw CDN-eindpunt: **&lt;EndpointName>. azureedge.net**. 

![Een domeinpagina toevoegen](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-the-cname-with-your-domain-registrar"></a>De CNAME configureren met uw domeinregistrar

Navigeer naar de website van uw domeinregistrar en ga naar het gedeelte voor het maken van DNS-records. U vindt dit in een gedeelte zoals **Domeinnaam**, **DNS** of **Serverbeheernaam**.

Zoek het gedeelte voor het beheren van CNAME's. Mogelijk moet u naar een pagina met geavanceerde instellingen gaan en de woorden CNAME, Alias of Subdomeinen zoeken.

Een CNAME-record maken die uw gekozen subdomein toegewezen (bijvoorbeeld **statische** of **cdn**) naar de **eindpunt hostnaam** eerder in de portal weergegeven. 

### <a name="enter-the-custom-domain-in-azure"></a>Voer het aangepast domein in Azure in

Ga terug naar de pagina **Een aangepast domein toevoegen** en voer in het dialoogvenster uw aangepaste domein in, inclusief het subdomein. Geef bijvoorbeeld `cdn.contoso.com` op.   
   
Azure controleert of de CNAME-record voor de domeinnaam die u hebt ingevoerd bestaat. Als de CNAME juist is, wordt uw aangepaste domein gevalideerd.

Het even kan duren voordat de CNAME-record is doorgeven aan de naamservers op het internet. Als uw domein is niet onmiddellijk gevalideerd, wacht een paar minuten en probeer het opnieuw.

### <a name="test-the-custom-domain"></a>Het aangepaste domein testen

Ga in een browser naar het bestand `index.html` op uw aangepaste domein (bijvoorbeeld `cdn.contoso.com/index.html`) om te controleren of het resultaat hetzelfde is als wanneer u rechtstreeks naar `<endpointname>azureedge.net/index.html` gaat.

![Startpagina van voorbeeldapp met URL van aangepast domein](media/app-service-web-tutorial-content-delivery-network/home-page-custom-domain.png)

Zie voor meer informatie [Azure CDN-inhoud toewijzen aan een aangepast domein](../cdn/cdn-map-content-to-custom-domain.md).

[!INCLUDE [cli-samples-clean-up](../../includes/cli-samples-clean-up.md)]

## <a name="next-steps"></a>Volgende stappen

Wat u hebt geleerd:

> [!div class="checklist"]
> * Een CDN-eindpunt maken.
> * Assets in cache vernieuwen.
> * Queryreeksen gebruiken voor het beheren van versies in cache.
> * Een aangepast domein gebruiken voor het CDN-eindpunt.

Meer informatie over het optimaliseren van CDN in de volgende artikelen:

> [!div class="nextstepaction"]
> [De prestaties verbeteren door bestanden in Azure CDN te comprimeren](../cdn/cdn-improve-performance.md)

> [!div class="nextstepaction"]
> [Vooraf assets op een Azure CDN-eindpunt laden](../cdn/cdn-preload-endpoint.md)

---
title: 'Zelfstudie: Azure CDN toevoegen aan een Azure App Service-web-app | Microsoft Docs'
description: In deze zelfstudie wordt Azure Content Delivery Network (CDN) toegevoegd aan een Azure App Service-web-app, zodat uw statische bestanden altijd in de cache worden opgeslagen en geleverd vanaf een server dicht bij uw klanten, overal ter wereld.
services: cdn
documentationcenter: ''
author: dksimpson
manager: cfowler
editor: ''
ms.assetid: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 05/14/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: d782f5bf30160d85d7d9ef7a00190551f9a9843a
ms.sourcegitcommit: e14229bb94d61172046335972cfb1a708c8a97a5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2018
---
# <a name="tutorial-add-azure-cdn-to-an-azure-app-service-web-app"></a>Zelfstudie: Azure CDN toevoegen aan een Azure App Service-web-app

In deze zelfstudie leest u hoe u Azure CDN toevoegt aan een [web-app in de Azure App Service](../app-service/app-service-web-overview.md). [Azure Content Delivery Network (CDN)](cdn-overview.md) slaat op strategisch geplaatste locaties statische webinhoud in de cache op om een maximale doorvoer voor de levering van inhoud te waarborgen. Met Azure CDN wordt ook de serverbelasting voor uw web-app kleiner. 

Dit is de startpagina van de statische HTML-voorbeeldsite die u gaat gebruiken:

![Startpagina van voorbeeld-app](media/app-service-web-tutorial-content-delivery-network/sample-app-home-page.png)

Wat u leert:

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

Volg de [snelstart voor statische HTML](../app-service/app-service-web-get-started-html.md) om de web-app die u gaat gebruiken te maken tot en met de stap **Browse to the app** (Bladeren naar de app).

### <a name="have-a-custom-domain-ready"></a>Houd een aangepast domein bij de hand

U hebt een aangepast domein en toegang tot het DNS-register voor uw domeinprovider nodig om de stap voor het aangepaste domein van deze zelfstudie te voltooien. Als u bijvoorbeeld DNS-vermeldingen voor contoso.com en www.contoso.com wilt toevoegen, moet u toegang hebben om de DNS-instellingen voor het hoofddomein van contoso.com te configureren.

Als u nog geen domeinnaam hebt, overweeg dan de [Zelfstudie over App Service-domeinen](../app-service/custom-dns-web-site-buydomains-web-app.md) te volgen om een domein aan te schaffen met de Azure Portal. 

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Open een browser en navigeer naar de [Azure Portal](https://portal.azure.com).

## <a name="create-a-cdn-profile-and-endpoint"></a>Een CDN-profiel en -eindpunt maken

Selecteer in het navigatiedeelvenster links **App Services** en selecteer vervolgens de app die u hebt gemaakt in de [Quickstart voor statische HTML](../app-service/app-service-web-get-started-html.md).

![Een App Service-app in de portal selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-app-services.png)

Selecteer op de pagina **App Service** in het gedeelte **Instellingen** de optie **Netwerken > Azure CDN configureren voor uw app**.

![CDN selecteren in de portal](media/app-service-web-tutorial-content-delivery-network/portal-select-cdn.png)

Geef op de pagina **Azure Content Delivery Network** de instellingen voor **Nieuw eindpunt** op zoals aangegeven in de tabel.

![Een profiel en eindpunt maken in de portal](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint.png)

| Instelling | Voorgestelde waarde | Beschrijving |
| ------- | --------------- | ----------- |
| **CDN-profiel** | myCDNProfile | Selecteer **Nieuwe maken** om een CDN-profiel te maken. Een CDN-profiel is een verzameling van CDN-eindpunten van dezelfde prijscategorie. |
| **Prijscategorie** | Standard Akamai | De [prijscategorie](cdn-features.md) geeft de provider en de beschikbare functies aan. In deze zelfstudie wordt gebruikgemaakt van *Standard Akamai*. |
| **Naam van CDN-eindpunt** | Een unieke naam in het domein azureedge.net | U heeft toegang tot uw resources in de cache via het domein  *\<endpointname>.azureedge.net*.

Selecteer **Maken**.

Azure maakt het profiel en het eindpunt. Het nieuwe eindpunt wordt weergegeven in de lijst **Eindpunten**, en wanneer het eindpunt is ingericht, heeft het de status **Actief**.

![Nieuw eindpunt in lijst](media/app-service-web-tutorial-content-delivery-network/portal-new-endpoint-in-list.png)

### <a name="test-the-cdn-endpoint"></a>Het CDN-eindpunt testen

Als u de prijscategorie Verizon hebt geselecteerd, duurt het doorgaans ongeveer 90 minuten voor het eindpunt is ingericht. Voor Akamai duurt de doorgifte enkele minuten.

De voorbeeld-app heeft een *index.html*-bestand en de mappen *css*, *img* en *js* met andere statische assets. De inhoudspaden voor al deze bestanden zijn op het CDN-eindpunt hetzelfde. De volgende URL's bieden bijvoorbeeld toegang tot het bestand *bootstrap.css* in de map *css*:

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

 U ziet dezelfde pagina die u eerder in een Azure-web-app hebt uitgevoerd. Azure CDN heeft de assets van de oorspronkelijke web-app opgehaald en verstuurt gegevens vanaf het CDN-eindpunt.

Vernieuw de pagina om ervoor te zorgen dat deze pagina is opgeslagen in de cache in het CDN. Er zijn soms twee aanvragen van hetzelfde asset vereist voordat het CDN de gevraagde inhoud in de cache opslaat.

Zie voor meer informatie over het maken van Azure CDN-profielen en -eindpunten [Aan de slag met Azure CDN](cdn-create-new-endpoint.md).

## <a name="purge-the-cdn"></a>Het CDN leegmaken

Het CDN vernieuwt periodiek de behorende resources uit de oorspronkelijke web-app op basis van de configuratie van de time-to-live (TTL). De standaard-TTL is zeven dagen.

Soms moet u wellicht het CDN vernieuwen voordat de TTL is verlopen. Bijvoorbeeld wanneer u bijgewerkte inhoud in de web-app implementeert. Wis de resources in het CDN handmatig om te vernieuwen. 

In dit gedeelte van de zelfstudie implementeert u een wijziging in de web-app en schoont u het CDN op zodat de bijbehorende cache wordt vernieuwd.

### <a name="deploy-a-change-to-the-web-app"></a>Een wijziging implementeren in de web-app

Open het *index.html*-bestand en voeg *- V2* toe aan de kop H1, zoals wordt weergegeven in het volgende voorbeeld: 

```
<h1>Azure App Service - Sample Static HTML Site - V2</h1>
```

Voer uw wijziging door en implementeer deze naar de web-app.

```bash
git commit -am "version 2"
git push azure master
```

Als de implementatie is voltooid, ziet u de wijziging als u naar de URL van de web-app bladert.

```
http://<appname>.azurewebsites.net/index.html
```

![V2 in de titel van de web-app](media/app-service-web-tutorial-content-delivery-network/v2-in-web-app-title.png)

Als u naar de URL van het CDN-eindpunt voor de startpagina bladert, wordt de wijziging niet weergegeven omdat de in de cache opgeslagen versie in het CDN nog niet is verlopen. 

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

Selecteer boven aan de pagina **Eindpunt** de optie **Leegmaken**.

![Leegmaken selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-purge.png)

Voer de inhoudspaden in die u wilt leegmaken. Als u een afzonderlijk bestand wilt verwijderen, geeft u het volledige bestandspad door. Als u een map wilt leegmaken en vernieuwen, geeft u een padsegment door. Omdat u *index.html* hebt gewijzigd, moet u controleren of het bestand zich op een van de paden bevindt.

Klik onder aan de pagina op **Leegmaken**.

![Pagina leegmaken](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

### <a name="verify-that-the-cdn-is-updated"></a>Controleren of het CDN is bijgewerkt

Wacht totdat de aanvraag voor leegmaken is verwerkt. Dit duurt doorgaans enkele minuten. Selecteer het belpictogram boven aan de pagina om de huidige status te bekijken. 

![Melding voor leegmaken](media/app-service-web-tutorial-content-delivery-network/portal-purge-notification.png)

Wanneer u naar de URL van het CDN-eindpunt voor *index.html* bladert, ziet u de *V2* die u hebt toegevoegd aan de titel op de startpagina. Dit betekent dat de CDN-cache is vernieuwd.

```
http://<endpointname>.azureedge.net/index.html
```

![V2 in de titel van het CDN](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title.png)

Zie voor meer informatie [Een Azure CDN-eindpunt leegmaken](../cdn/cdn-purge-endpoint.md). 

## <a name="use-query-strings-to-version-content"></a>Queryreeksen gebruiken om inhoud een versie te geven

Azure CDN biedt de volgende opties voor cachegedrag:

* Queryreeksen negeren
* Queryreeksen in de cache opslaan overslaan
* Elke unieke URL in de cache opslaan 

De eerste optie is de standaardoptie. Hierbij bevat de cache slechts één versie van een asset, ongeacht de gebruikte queryreeks in de URL. 

In dit gedeelte van de zelfstudie wijzigt u het cachegedrag zodat elke unieke URL in de cache wordt opgeslagen.

### <a name="change-the-cache-behavior"></a>Het gedrag van de cache wijzigen

Selecteer in de Azure Portal op de pagina **CDN-eindpunt** de optie **Cache**.

Selecteer **Elke unieke URL in de cache opslaan** in de vervolgkeuzelijst **Cachegedrag van queryreeks**.

Selecteer **Opslaan**.

![Selecteer cachegedrag van queryreeks](media/app-service-web-tutorial-content-delivery-network/portal-select-caching-behavior.png)

### <a name="verify-that-unique-urls-are-cached-separately"></a>Controleren of de unieke URL's afzonderlijk in de cache worden opgeslagen

Navigeer in een browser naar de startpagina op het CDN-eindpunt, en voeg een queryreeks toe: 

```
http://<endpointname>.azureedge.net/index.html?q=1
```

Met Azure CDN wordt de huidige inhoud van de web-app geretourneerd, met *V2* in de kop. 

Vernieuw de pagina om ervoor te zorgen dat deze pagina is opgeslagen in de cache in het CDN. 

Open *index.html*, wijzig *V2* in *V3*, en implementeer vervolgens de wijziging. 

```bash
git commit -am "version 3"
git push azure master
```

Ga in een browser naar de URL van het CDN-eindpunt met een nieuwe queryreeks zoals `q=2`. Met Azure CDN wordt het huidige *index.html*-bestand opgehaald en *V3* weergegeven. Als u echter naar het CDN-eindpunt navigeert met de queryreeks `q=1`, ziet u *V2*.

```
http://<endpointname>.azureedge.net/index.html?q=2
```

![V3 in de titel in het CDN, queryreeks 2](media/app-service-web-tutorial-content-delivery-network/v3-in-cdn-title-qs2.png)

```
http://<endpointname>.azureedge.net/index.html?q=1
```

![V2 in de titel in het CDN, queryreeks 1](media/app-service-web-tutorial-content-delivery-network/v2-in-cdn-title-qs1.png)

Deze uitvoer laat zien dat elke queryreeks anders wordt behandeld:

* q = 1 werd eerder gebruikt, waardoor in cache opgeslagen inhoud wordt geretourneerd (V2).
* q = 2 is nieuw, waardoor de meest recente inhoud van de web-app wordt opgehaald en geretourneerd (V3).

Zie [Cachegedrag in Azure CDN bepalen met queryreeksen](../cdn/cdn-query-string.md) voor meer informatie.

## <a name="map-a-custom-domain-to-a-cdn-endpoint"></a>Een aangepast domein aan een CDN-eindpunt toewijzen

U wijst uw aangepaste domein toe aan het CDN-eindpunt door een CNAME-record te maken. Een CNAME-record is een DNS-functie die een brondomein toewijst aan een doeldomein. U kunt bijvoorbeeld cdn.contoso.com of static.contoso.com toewijzen aan contoso.azureedge.net.

Als u geen aangepaste domeinnaam hebt, overweeg dan de [Zelfstudie over App Service-domeinen](../app-service/custom-dns-web-site-buydomains-web-app.md) te volgen om een domein aan te schaffen met de Azure Portal. 

### <a name="find-the-hostname-to-use-with-the-cname"></a>De hostnaam voor gebruik met de CNAME vinden

Zorg er in Azure Portal op de pagina **Eindpunt** voor dat **Overzicht** is geselecteerd in de linkernavigatiebalk en selecteer vervolgens de knop **+ Aangepast domein** bovenaan de pagina.

![Aangepast domein toevoegen selecteren](media/app-service-web-tutorial-content-delivery-network/portal-select-add-domain.png)

Op de pagina **Een aangepast domein toevoegen** ziet u de naam van de eindpunthost die u kunt gebruiken om een CNAME-record te maken. De hostnaam is afgeleid van de URL van uw CDN-eindpunt: **&lt;EndpointName>. azureedge.net**. 

![Een domeinpagina toevoegen](media/app-service-web-tutorial-content-delivery-network/portal-add-domain.png)

### <a name="configure-the-cname-with-your-domain-registrar"></a>De CNAME configureren met uw domeinregistrar

Navigeer naar de website van uw domeinregistrar en ga naar het gedeelte voor het maken van DNS-records. Deze sectie kan bijvoorbeeld de naam **Domeinnaam**, **DNS** of **Beheerservernaam** hebben.

Zoek het gedeelte voor het beheren van CNAME's. Mogelijk moet u naar een pagina met geavanceerde instellingen gaan en de woorden CNAME, Alias of Subdomeinen zoeken.

Maak een CNAME-record die uw gekozen subdomein (bijvoorbeeld **statisch** of **cdn**) toewijst aan de **Hostnaam van eindpunt** die eerder in de portal werd weergegeven. 

### <a name="enter-the-custom-domain-in-azure"></a>Voer het aangepast domein in Azure in

Ga terug naar de pagina **Een aangepast domein toevoegen** en voer in het dialoogvenster uw aangepaste domein in, inclusief het subdomein. Voer bijvoorbeeld *cdn.contoso.com* in.   
   
Azure controleert of de CNAME-record voor de domeinnaam die u hebt ingevoerd bestaat. Als de CNAME juist is, wordt uw aangepaste domein gevalideerd.

Het even kan duren voordat de CNAME-record is doorgeven aan de naamservers op het internet. Als uw domein niet onmiddellijk wordt gevalideerd, wacht u een paar minuten en probeert u het opnieuw.

### <a name="test-the-custom-domain"></a>Het aangepaste domein testen

Ga in een browser naar het *index.html*-bestand via uw aangepaste domein (bijvoorbeeld cdn.contoso.com/index.html) om te controleren of het resultaat hetzelfde is als wanneer u rechtstreeks naar &lt;eindpuntnaam&gt;azureedge.net/index.html gaat.

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

In de volgende artikelen vindt u informatie over het optimaliseren van CDN:

> [!div class="nextstepaction"]
> [Zelfstudie: Een aangepast domein toevoegen aan uw Azure CDN-eindpunt](cdn-map-content-to-custom-domain.md)



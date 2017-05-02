---
title: Een netwerk voor contentlevering toevoegen aan een Azure App Service | Microsoft Docs
description: Voeg aan een Azure App Service een netwerk voor contentlevering toe om uw statische bestanden te leveren vanuit Edge-knooppunten.
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: 7ba3737566401152a3171e8926beca188045230c
ms.lasthandoff: 04/22/2017

---
# <a name="add-a-content-deliver-network-on-an-azure-app-service"></a>Een netwerk voor contentlevering toevoegen aan een Azure App Service

In deze zelfstudie voegt u een CDN (netwerk voor contentlevering) aan uw Azure App Service toe om de statische inhoud beschikbaar te stellen op een randserver. U gaat een CDN-profiel maken, een verzameling van maximaal 10 CDN-eindpunten.

Een Azure-netwerk voor contentlevering (CDN) slaat op strategisch geplaatste locaties statische webinhoud in de cache op om een maximale doorvoer voor de levering van inhoud te waarborgen. Enkele voordelen van het gebruik van de CDN om website-assets op te slaan in de cache:

* Betere prestaties en gebruikerservaring voor eindgebruikers wanneer er toepassingen worden gebruikt waarbij meerdere retouren zijn vereist om inhoud te laden.
* Grote schaalbaarheid zodat een korte hoge belasting, bijvoorbeeld wanneer een product wordt gestart, beter kan worden verwerkt.
* Door de gebruikersaanvragen te distribueren en de inhoud uit te voeren vanaf randservers wordt er minder verkeer naar de oorsprong verzonden.

> [!TIP]
> Controleer de bijgewerkte lijst van [Azure CDN POP-locaties](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations).
>

## <a name="deploy-the-sample"></a>Het voorbeeld implementeren

Om deze zelfstudie te kunnen volgen, moet u een toepassing in Web App hebben geïmplementeerd. Volg de [statische HTML-snelstartgids](app-service-web-get-started-html.md) om een basis voor deze zelfstudie te creëren.

## <a name="step-1---login-to-azure-portal"></a>Stap 1: Meld u aan bij Azure Portal

Eerst opent u uw browser en bladert u naar Azure [Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Stap 2: Maak een CDN-profiel

Klik op de knop `+ New` in de linkernavigatie en kies **Web en mobiel**. Selecteer in de categorie Web en mobiel de optie **CDN**.

Geef de volgende velden op:

| Veld | Voorbeeldwaarde | Beschrijving |
|---|---|---|
| Naam | myCDNProfile | Een naam voor het CDN-profiel. |
| Locatie | West-Europa | Dit is de Azure-locatie waar uw CDN-profielgegevens worden opgeslagen. Dit heeft geen invloed op de locaties van CDN-eindpunten. |
| Resourcegroep | myResourceGroup | Zie [Overzicht van Azure Resource Manager](../azure-resource-manager/resource-group-overview.md#resource-groups) voor meer informatie over resourcegroepen |
| Prijscategorie | Standard Akamai | Zie het [CDN Overview](../cdn/cdn-overview.md#azure-cdn-features) (CDN-overzicht) voor een vergelijking van prijscategorieën. |

Klik op **Create**.

Open de resourcegroephub vanuit de linkernavigatie en selecteer **myResourceGroup**. Selecteer in de lijst met resources **myCDNProfile**.

![azure-cdn-profile-gemaakt](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Stap 3: Maak een CDN-eindpunt

Klik op **+ Eindpunt** vanuit de opdrachten naast het zoekvak. Hiermee wordt de blade voor het maken van een eindpunt geopend.

Geef de volgende velden op:

| Veld | Voorbeeldwaarde | Beschrijving |
|---|---|
| Naam |  | Deze naam wordt gebruikt om toegang te krijgen tot uw resources in de cache in het domein `<endpointname>.azureedge.net` |
| Oorsprongtype | Web-app | Als u een oorsprongtype selecteert, kunt u voor de resterende velden beschikken over contextmenu's. Als u een aangepaste oorsprong selecteert, wordt er een tekstveld voor de hostnaam van de oorsprong weergegeven. |
| Hostnaam van oorsprong | |  De vervolgkeuzelijst bevat alle beschikbare oorsprongen van het oorsprongtype dat u hebt opgegeven. Als u Aangepaste oorsprong als Oorsprongtype hebt geselecteerd, voert u nu het domein in van de aangepaste oorsprong  |

Klik op **Add**.

Het eindpunt wordt gemaakt en nadat het Content Delivery Network-eindpunt is gemaakt, wordt de status bijgewerkt naar **actief**.

![azure-cdn-endpoint-gemaakt](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---serve-from-azure-cdn"></a>Stap 4: Uitvoeren vanuit Azure CDN

Wanneer het CDN-eindpunt **Actief** is, moet u vanuit het CDN-eindpunt inhoud kunnen openen.

Als u de [statische HTML-snelstartgids](app-service-web-get-started-html.md) als basis voor deze zelfstudie hebt gebruikt, zijn de volgende mappen beschikbaar op het CDN: `css`, `img` en `js`.

De inhoudspaden tussen de Web App-URL `http://<app_name>.azurewebsites.net/img/` en de URL van het CDN-eindpunt `http://<endpointname>.azureedge.net/img/` zijn hetzelfde. Dat betekent dat u statische inhoud gewoon kunt vervangen door het domein van het CDN-eindpunt, om deze inhoud vanuit het CDN te laten leveren.

U gaat nu een eerste afbeelding van het CDN-eindpunt ophalen door in uw webbrowser de volgende URL te volgen:

```bash
http://<endpointname>.azureedge.net/img/03-enterprise.png
```

Wanneer de statische inhoud beschikbaar is op het CDN, kunt u uw toepassing bijwerken om het CDN-eindpunt te gebruiken om de inhoud aan de eindgebruiker te leveren.

Afhankelijk van de taal waarin uw site is gebouwd, kunnen er vele structuren zijn die u helpen bij CDN-terugval. ASP.NET biedt bijvoorbeeld ondersteuning voor [bundeling en minificatie](https://docs.microsoft.com/en-us/aspnet/mvc/overview/performance/bundling-and-minification#using-a-cdn), waarmee ook CDN-terugvalmogelijkheden worden ingeschakeld.

Als uw taal geen ingebouwde CDN-terugvalondersteuning of bibliotheek voor CDN-terugval heeft, kunt u een JavaScript-structuur zoals [FallbackJS](http://fallback.io/) gebruiken. Deze biedt ondersteuning voor het laden van [scripts](https://github.com/dolox/fallback/tree/master/examples/loading-scripts), [opmaakmodellen](https://github.com/dolox/fallback/tree/master/examples/loading-stylesheets) en [afbeeldingen](https://github.com/dolox/fallback/tree/master/examples/loading-images).

## <a name="step-5---purge-the-cdn"></a>Stap 5: het CDN leegmaken

Soms moet het CDN worden leeggemaakt, bijvoorbeeld als u wilt dat inhoud vóór de Time to Live (TTL) vervalt.

U kunt het Azure CDN handmatig leegmaken vanaf de blade CDN-profiel of de blade CDN-eindpunt. Als u ervoor kiest om het CDN leeg te maken vanaf de profielpagina, moet u selecteren welk eindpunt u wilt leegmaken.

Als u inhoud wilt verwijderen, typt u de inhoudspaden die u wilt leegmaken. Als u een afzonderlijk bestand wilt verwijderen, geeft u het volledige bestandspad door. Als u een specifieke map wilt leegmaken en vernieuwen, geeft u een padsegment door.

Wanneer u alle inhoudspaden hebt opgegeven die u wilt leegmaken, klikt u op **Leegmaken**.

![app-service-web-opschonen-cdn](media/app-service-web-tutorial-content-delivery-network/app-service-web-purge-cdn.png)

## <a name="step-6---map-a-custom-domain"></a>Stap 6: Een aangepast domein toewijzen

Door een aangepast domein toe te wijzen aan uw CDN-eindpunt, voorziet u in een uniform domein voor uw webtoepassing.

Maak een CNAME-record bij uw domeinregistrar als u een aangepast domein aan uw CDN-eindpunt wilt toewijzen.

> [!NOTE]
> Een CNAME-record is een DNS-functie die een brondomein, bijvoorbeeld `www.contosocdn.com` of `static.contosocdn.com`, toewijst aan een doeldomein.

In dit geval voegt u het brondomein `static.contosocdn.com` toe, dat naar het doeldomein wijst dat het CDN-eindpunt is.

| brondomein | doeldomein |
|---|---|
| static.contosocdn.com | &lt;eindpuntnaam&gt;.azureedge.net |

Klik op de blade Overzicht van CDN-eindpunt op de knop `+ Custom domain`.

Ga in het dialoogvenster naar de blade Een aangepast domein toevoegen en voer daar uw aangepaste domein in, inclusief het subdomein. Voer bijvoorbeeld de domeinnaam in de notatie `static.contosocdn.com` in.

Klik op **Add**.

## <a name="step-7---version-content"></a>Stap 7: Versie-inhoud

In de linkernavigatie van het CDN-eindpunt selecteert u **Cache** onder de kop Instellingen.

Op de blade **Cache** kunt u configureren hoe het CDN queryreeksen in de aanvraag moet afhandelen.

> [!NOTE]
> Voor een beschrijving van de opties voor het cachegedrag van querytekenreeksen raadpleegt u het onderwerp [Control Azure CDN caching behavior with query strings](../cdn/cdn-query-string.md) (Het cachegedrag van queryreeksen in Azure CDN beheren).

Selecteer **Elke unieke URL in de cache opslaan** in de vervolgkeuzelijst Cachegedrag van queryreeks.

Klik op **Opslaan**.

## <a name="next-steps"></a>Volgende stappen

* [Wat is Azure CDN?](../best-practices-cdn.md?toc=%2fazure%2fcdn%2ftoc.json)
* [HTTPS op een aangepast Azure CDN-domein inschakelen](../cdn/cdn-custom-ssl.md)
* [De prestaties verbeteren door bestanden in Azure CDN te comprimeren](../cdn/cdn-improve-performance.md)
* [Vooraf assets op een Azure CDN-eindpunt laden](../cdn/cdn-preload-endpoint.md)


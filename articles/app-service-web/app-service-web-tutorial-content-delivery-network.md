---
title: Een web-app verbinden met een Content Delivery Network | Microsoft Docs
description: Verbind een web-app met een Content Delivery Network om uw statische bestanden te leveren vanaf Edge-knooppunten.
services: app-service
author: syntaxc4
ms.author: cfowler
ms.date: 04/03/2017
ms.topic: hero-article
ms.service: app-service-web
manager: erikre
translationtype: Human Translation
ms.sourcegitcommit: db7cb109a0131beee9beae4958232e1ec5a1d730
ms.openlocfilehash: 8194e669f943a6c47f02ae0d2a55e0e720420489
ms.lasthandoff: 04/18/2017

---
# <a name="connect-a-web-app-to-a-content-delivery-network"></a>Een web-app verbinden met een Content Delivery Network

In deze zelfstudie maakt u een Azure CDN-profiel en een Azure CDN-eindpunt om de statische bestanden van uw web-app te leveren via de Azure CDN POP-locaties.

> [!TIP]
> Controleer de bijgewerkte lijst van [Azure CDN POP-locaties](https://docs.microsoft.com/en-us/azure/cdn/cdn-pop-locations).
>

## <a name="step-1---login-to-azure-portal"></a>Stap 1: Meld u aan bij Azure Portal

Open eerst uw favoriete browser en blader naar Azure [Portal](https://portal.azure.com).

## <a name="step-2---create-a-cdn-profile"></a>Stap 2: Maak een CDN-profiel

Klik op de knop **+ Nieuw** in het linkernavigatiepaneel. Klik vervolgens op **Web + mobiel**. Selecteer onder de categorie Web + mobiel de optie **CDN**.

Geef de **Naam**, **Locatie**, **Resourcegroep** en **Prijscategorie** op en klik vervolgens op **Maken**.

Open de resourcegroephub vanuit de linkernavigatie en selecteer **myResourceGroup**. Selecteer in de lijst met resources **myCDNProfile**.

![azure-cdn-profile-gemaakt](media/app-service-web-tutorial-content-delivery-network/azure-cdn-profile-created.png)

## <a name="step-3---create-a-cdn-endpoint"></a>Stap 3: Maak een CDN-eindpunt

Klik op **+ Eindpunt** vanuit de opdrachten naast het zoekvak. Hiermee wordt de blade voor het maken van een eindpunt geopend.

Geef de **Naam**, het **Oorsprongtype** en de **Hostnaam van oorsprong** op en klik vervolgens op **Toevoegen**.

Het eindpunt wordt gemaakt en nadat het Content Delivery Network-eindpunt is gemaakt, wordt de status bijgewerkt naar **actief**.

![azure-cdn-endpoint-gemaakt](media/app-service-web-tutorial-content-delivery-network/azure-cdn-endpoint-created.png)

## <a name="step-4---leveraging-cdn"></a>Stap 4: Maak gebruik van CDN

Nu het eindpunt actief is, gaan we valideren of de inhoud beschikbaar is op de POP-server door te bladeren naar een statisch bestand op de webserver en vervolgens de hostnaam te wijzigen van `http://<app_name>.azurewebsites.net/path/to-static-file` in `http://<endpoint_name>.azureedge.net/path/to-static-file`.

![app-service-web-url-naar-cdn-eindpunt-url](media/app-service-web-tutorial-content-delivery-network/app-service-web-url-to-cdn-endpoint-url.png)

## <a name="next-steps"></a>Volgende stappen



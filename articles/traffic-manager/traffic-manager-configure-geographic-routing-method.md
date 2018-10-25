---
title: Configureren van de geografische verkeersrouteringsmethode met behulp van Azure Traffic Manager | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u de geografische verkeersrouteringsmethode met behulp van Azure Traffic Manager configureren
services: traffic-manager
documentationcenter: ''
author: kumudd
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 5346617c858ab3bc168781c76985729c2da6a768
ms.sourcegitcommit: c2c279cb2cbc0bc268b38fbd900f1bac2fd0e88f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/24/2018
ms.locfileid: "49984338"
---
# <a name="configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>De geografische verkeersrouteringsmethode met Traffic Manager configureren

De geografische verkeersrouteringsmethode kunt u verkeer naar specifieke eindpunten op basis van de geografische locatie waar de aanvragen afkomstig zijn. Deze zelfstudie leert u hoe u een Traffic Manager-profiel maken met deze routeringsmethode en configureren van de eindpunten voor het ontvangen van verkeer van specifieke Geografie.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/).
2. Klik op **Een resource maken** > **Netwerken** > **Traffic Manager-profiel** > **Maken**.
4. In de **Traffic Manager-profiel**:
    1. Geef een naam voor uw profiel. Deze naam moet uniek zijn binnen de zone trafficmanager.net. Voor toegang tot uw Traffic Manager-profiel, gebruikt u de DNS-naam <profilename>. trafficmanager.net.
    2. Selecteer de **geografisch** routeringsmethode.
    3. Selecteer het abonnement dat u wilt maken van dit profiel.
    4. Gebruik een bestaande resourcegroep of maak een nieuwe resourcegroep voor dit profiel. Als u ervoor kiest om een nieuwe resourcegroep te maken, gebruikt u de **resourcegroep locatie** vervolgkeuzelijst om op te geven van de locatie van de resourcegroep. Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.
    5. Nadat u op **maken**, uw Traffic Manager-profiel wordt gemaakt en wereldwijd geïmplementeerd.

![Een Traffic Manager-profiel maken](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Eindpunten toevoegen

1. Zoek de naam voor Traffic Manager-profiel dat u hebt gemaakt in de zoekbalk van de portal en klik op het resultaat wanneer deze wordt weergegeven.
2. Navigeer naar **instellingen** -> **eindpunten** in Traffic Manager.
3. Klik op **toevoegen** om weer te geven de **eindpunt toevoegen**.
3. Klik op **toevoegen** en in de **eindpunt toevoegen** die wordt weergegeven, als volgt uitvoeren:
4. Selecteer **Type** , afhankelijk van het type van het eindpunt dat u wilt toevoegen. Voor de geografische routering profielen die worden gebruikt in productie, wordt aangeraden met behulp van geneste eindpunttypen met een onderliggende-profiel met meer dan één eindpunt. Zie voor meer informatie, [Veelgestelde vragen over geografische verkeersrouteringsmethoden](traffic-manager-FAQs.md).
5. Geef een **Naam** op waarmee u dit eindpunt kunt herkennen.
6. Bepaalde velden op deze pagina is afhankelijk van het type van het eindpunt dat u wilt toevoegen:
    1. Als u een Azure-eindpunt toevoegen wilt, selecteert u de **Doelresourcetype** en de **doel** op basis van de resource die u wilt instellen dat verkeer naar
    2. Als u wilt toevoegen een **externe** eindpunt, bieden de **volledig gekwalificeerde domeinnaam (FQDN)** voor het eindpunt.
    3. Als u wilt toevoegen een **genest eindpunt**, selecteer de **Doelresource** die overeenkomt met het onderliggende profiel dat u wilt gebruiken en geef de **minimumaantal onderliggende eindpunten**.
7. In de sectie Geo-toewijzing gebruikt u de vervolgkeuzelijst omlaag naar de regio's van waar u het verkeer wordt verzonden naar dit eindpunt wilt toevoegen. U moet ten minste één regio toevoegt en u kunt meerdere regio's toegewezen hebben.
8. Herhaal dit voor alle eindpunten die u wilt toevoegen onder dit profiel

![Traffic Manager-eindpunt toevoegen](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Gebruik Traffic Manager-profiel
1.  Zoek in de zoekbalk van de portal, de **Traffic Manager-profiel** naam die u hebt gemaakt in de vorige sectie en klik op het traffic manager-profiel in de resultaten die de weergegeven.
2. Klik op **Overzicht**.
3. Het **Traffic Manager-profiel** geeft de DNS-naam weer van het Traffic Manager-profiel dat u zojuist hebt gemaakt. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het via een webbrowser) ophalen gerouteerd naar het juiste eindpunt als bepaald door het routeringstype.  In het geval van de geografische routering, Traffic Manager kijkt naar de bron-IP-adres van de inkomende aanvraag en bepaalt de regio van waaruit deze afkomstig is. Als deze regio is toegewezen aan een eindpunt, wordt verkeer doorgestuurd naar de locatie. Als deze regio niet aan een eindpunt toegewezen is, retourneert een GEENGEGEVENS query-antwoord met Traffic Manager.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [geografisch routeringsmethode voor verkeer](traffic-manager-routing-methods.md#geographic).
- Meer informatie over het [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

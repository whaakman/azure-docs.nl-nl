---
title: Routeringsmethode voor geografische verkeer met behulp van Azure Traffic Manager configureren | Microsoft Docs
description: In dit artikel wordt uitgelegd hoe u de geografische verkeersrouteringsmethode met behulp van Azure Traffic Manager configureren
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.openlocfilehash: 13190189074b24b2d28cd3ce46cf8571f3e1e1d1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="configure-the-geographic-traffic-routing-method-using-traffic-manager"></a>De geografische verkeersrouteringsmethode met Traffic Manager configureren

De geografische verkeersrouteringsmethode kunt u verkeer omleiden naar specifieke eindpunten op basis van de geografische locatie waar de verzoeken afkomstig zijn. Deze zelfstudie laat zien hoe u een Traffic Manager-profiel maken met deze methode voor het doorsturen en configureren van de eindpunten voor het ontvangen verkeer van specifieke locaties.

## <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog geen account hebt, kunt u zich registreren voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/).
2. Klik in het menu Hub op **nieuw** > **Networking** > **alle**, en klik vervolgens op **Traffic Manager-profiel**openen de **maken Traffic Manager-profiel** blade.
3. Op de **maken Traffic Manager-profiel** blade:
    1. Geef een naam voor uw profiel. Deze naam moet uniek zijn binnen de trafficmanager.net zone en leidt ertoe dat de DNS-naam <profilename>, trafficmanager.net die wordt gebruikt voor toegang tot uw Traffic Manager-profiel.
    2. Selecteer de **geografisch** routeringsmethode.
    3. Selecteer het abonnement dat u wilt maken van dit profiel onder.
    4. Gebruik een bestaande resourcegroep of maak een nieuwe resourcegroep die u wilt dit profiel onder plaatsen. Als u ervoor kiest om een nieuwe resourcegroep te maken, gebruikt u de **locatie van resourcegroep** vervolgkeuzelijst voor de locatie van de resourcegroep opgeven. Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat globaal wordt geïmplementeerd.
    5. Nadat u op **maken**, uw Traffic Manager-profiel is gemaakt en geïmplementeerd globaal.

![Een Traffic Manager-profiel maken](./media/traffic-manager-geographic-routing-method/create-traffic-manager-profile.png)

## <a name="add-endpoints"></a>Het toevoegen van eindpunten

1. De naam van het Traffic Manager-profiel die u zojuist hebt gemaakt in de portal zoekbalk zoeken en klik op het resultaat wanneer deze wordt weergegeven.
2. Navigeer naar **instellingen** -> **eindpunten** in de blade Traffic Manager.
3. Klik op **toevoegen** om weer te geven de **eindpunt toevoegen** blade.
3. In de **eindpunten** blade, klikt u op **toevoegen** en in de **eindpunt toevoegen** blade die wordt weergegeven, voltooi als volgt:
4. Selecteer **Type** afhankelijk van het type van het eindpunt dat u wilt toevoegen. Voor geografische routering profielen die worden gebruikt in productie, wordt aangeraden met behulp van geneste eindpunttypen met een onderliggende profiel met meer dan één eindpunt. Zie voor meer informatie [Veelgestelde vragen over geografische verkeersrouteringsmethoden](traffic-manager-FAQs.md).
5. Geef een **Naam** op waarmee u dit eindpunt kunt herkennen.
6. Bepaalde velden in deze blade is afhankelijk van het type van het eindpunt dat u wilt toevoegen:
    1. Als u een Azure-eindpunt toevoegt, selecteert u de **resource doeltype** en de **doel** op basis van de bron die u verkeer omleiden naar
    2. Als u wilt toevoegen een **externe** eindpunt, bieden de **FQDN-naam (Fully qualified domain name)** voor uw eindpunt.
    3. Als u wilt toevoegen een **geneste eindpunt**, selecteer de **Doelresource** die overeenkomt met het onderliggende profiel dat u wilt gebruiken en geef de **minimumaantal onderliggende eindpunten**.
7. Gebruik in de sectie Geo-toewijzing de vervolgkeuzelijst omlaag naar de regio's waar u verkeer wordt verzonden naar dit eindpunt wilt toevoegen. U moet ten minste één regio toevoegen en u kunt meerdere regio's die zijn toegewezen.
8. Herhaal deze stap voor alle eindpunten die u wilt toevoegen onder dit profiel

![Een Traffic Manager-eindpunt toevoegen](./media/traffic-manager-geographic-routing-method/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Gebruik het Traffic Manager-profiel
1.  Zoek in de portal zoekbalk, op de **Traffic Manager-profiel** naam die u hebt gemaakt in de voorgaande sectie en klik op het traffic manager-profiel in de resultaten die de weergegeven.
2. In de **Traffic Manager-profiel** blade, klikt u op **overzicht**.
3. De **Traffic Manager-profiel** blade bevat de DNS-naam van de zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het met een webbrowser) te verkrijgen gerouteerd naar het juiste eindpunt als bepaald door het type routering.  In het geval van een geografische routering Traffic Manager wordt gekeken naar de bron-IP van de binnenkomende aanvraag en bepaalt de regio van waaruit deze afkomstig is. Als deze regio is toegewezen aan een eindpunt, wordt verkeer gerouteerd naar daar. Als deze regio niet aan een eindpunt toegewezen is, retourneert Traffic Manager een GEENGEGEVENS query-antwoord.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [geografisch methode voor het doorsturen van verkeer](traffic-manager-routing-methods.md#geographic).
- Meer informatie over hoe [Traffic Manager-instellingen testen](traffic-manager-testing-settings.md).

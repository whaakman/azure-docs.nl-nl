---
title: Een Traffic Manager-profiel maken in Azure | Microsoft Docs
description: Dit artikel wordt beschreven hoe u een Traffic Manager-profiel maken
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
ms.date: 04/21/2017
ms.author: kumud
ms.openlocfilehash: e9ff7947e7801a9f352a7a947b09893b8f615d88
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken

Dit artikel wordt beschreven hoe u een profiel met **prioriteit** routeringstype voor gebruikers van de route voor eindpunten Azure Web Apps kan worden gemaakt. Met behulp van de **prioriteit** type routering, al het verkeer wordt doorgestuurd naar het eerste eindpunt terwijl de tweede wordt opgeslagen als een back-up. Als gevolg hiervan is dat de gebruikers kunnen worden doorgestuurd naar het tweede eindpunt als het eerste eindpunt slecht wordt.

In dit artikel zijn twee eerder gemaakte Azure-Web-App-eindpunten zijn gekoppeld aan deze nieuwe Traffic Manager-profiel. Voor meer informatie over het maken van Azure-Web-App-eindpunten, gaat u naar de [documentatiepagina voor Azure Web Apps](https://docs.microsoft.com/azure/app-service/). U kunt een willekeurig eindpunt dat een DNS-naam heeft en bereikbaar is via het openbare internet toevoegen en dat we Azure Web Apps eindpunten als voorbeeld gebruiken.

### <a name="create-a-traffic-manager-profile"></a>Een Traffic Manager-profiel maken
1. Meld u vanuit een browser aan bij [Azure Portal](http://portal.azure.com). Als u nog een account hebt, kunt u aanmelden voor een [gratis proefversie van één maand](https://azure.microsoft.com/free/). 
2. In het menu **Hub** klikt u op **Nieuw** > **Netwerken** > **Alles weergeven** en klikt u op het **Traffic Manager**-profiel om de blade **Traffic Manager-profiel** te openen. Klik vervolgens op **Maken**.
3. Op de blade **Traffic Manager-profiel maken** vult u het volgende in:
    1. In **Naam** geeft u een naam op voor het profiel. Deze naam moet uniek zijn binnen de zone trafficmanager.net en resulteert in de DNS-naam <name>, trafficmanager.net die wordt gebruikt voor toegang tot uw Traffic Manager-profiel.
    2. In **Routeringsmethode** selecteert u de routeringsmethode **Prioriteit**.
    3. In **Abonnement** selecteert u het abonnement waarvoor u dit profiel wilt maken
    4. In **Resourcegroep** maakt u een nieuwe resourcegroep om dit profiel voor te maken.
    5. In **Locatie van de resourcegroep** selecteert u de locatie van de resourcegroep. Deze instelling verwijst naar de locatie van de resourcegroep en heeft geen invloed op het Traffic Manager-profiel dat wereldwijd wordt geïmplementeerd.
    6. Klik op **Create**.
    7. Wanneer de wereldwijde implementatie van uw Traffic Manager-profiel is voltooid, wordt het in de bijbehorende resourcegroep vermeld als één van de resources.

    ![Een Traffic Manager-profiel maken](./media/traffic-manager-create-profile/Create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Traffic Manager-eindpunten toevoegen

1. Zoek in de portal zoekbalk, op de **Traffic Manager-profiel** naam die u hebt gemaakt in de voorgaande sectie en klik op het traffic manager-profiel in de resultaten die de weergegeven.
2. Klik op de blade **Traffic Manager-profiel** in de sectie **Instellingen** op **Eindpunten**.
3. Klik op de blade **Eindpunten** die wordt weergegeven, op **Toevoegen**.
4. Voltooi de blade **Eindpunt toevoegen** als volgt:
    1. Klik bij **Type** op **Azure-eindpunt**.
    2. Geef een **Naam** op waarmee u dit eindpunt kunt herkennen.
    3. Voor **resource doeltype**, klikt u op **App Service**.
    4. Voor **Doelresource**, klikt u op **kiest u een app service** om weer te geven van de vermelding van de Web-Apps in hetzelfde abonnement. In de **Resource** blade die wordt weergegeven, kies de App-service die u wilt toevoegen als het eerste eindpunt.
    5. Selecteer bij **Prioriteit** de optie **1**. Dit zorgt ervoor dat alle verkeer naar dit eindpunt wordt geleid, indien het eindpunt in orde is.
    6. Laat **Toevoegen als uitgeschakeld** uit staan.
    7. Klik op **OK**
5.  Herhaal stap 3 en 4 voor de volgende Azure Web Apps-eindpunt. Zorg ervoor dat dit eindpunt wordt toegevoegd met de waarde **2** als bijbehorende **Prioriteit**.
6.  Als beide eindpunten zijn toegevoegd, worden ze weergegeven op de blade **Traffic Manager-profiel**, samen met de controlestatus **Online**.

    ![Een Traffic Manager-eindpunt toevoegen](./media/traffic-manager-create-profile/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Gebruik het Traffic Manager-profiel
1.  Zoek in de portal zoekbalk, op de **Traffic Manager-profiel** naam die u in de voorgaande sectie hebt gemaakt. In de resultaten die worden weergegeven, klikt u op het traffic manager-profiel.
2. In de **Traffic Manager-profiel** blade, klikt u op **overzicht**.
3. De **Traffic Manager-profiel** blade bevat de DNS-naam van de zojuist gemaakte Traffic Manager-profiel. Dit kan worden gebruikt door clients (bijvoorbeeld door te navigeren naar het met een webbrowser) te verkrijgen gerouteerd naar het juiste eindpunt als bepaald door het type routering. In dit geval alle aanvragen worden doorgestuurd naar het eerste eindpunt en als Traffic Manager wordt gedetecteerd, worden niet in orde, het verkeer automatisch wordt overgenomen door het volgende eindpunt.

## <a name="delete-the-traffic-manager-profile"></a>Traffic Manager-profiel verwijderen
Wanneer deze niet langer nodig is, verwijder de resourcegroep en de Traffic Manager-profiel dat u hebt gemaakt. Om dit te doen, selecteert u de resourcegroep van de **Traffic Manager-profiel** blade en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [routering typen](traffic-manager-routing-methods.md).
- Meer informatie over eindpunttypen [eindpunttypen](traffic-manager-endpoint-types.md).
- Meer informatie over [eindpuntcontrole](traffic-manager-monitoring.md).




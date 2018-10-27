---
title: Real-User-metingen naar Azure Traffic Manager met webpagina's | Microsoft Docs
description: Instellen van uw webpagina's voor het verzenden van Real-User-metingen naar Traffic Manager
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: ''
ms.workload: infrastructure
ms.date: 03/16/2018
ms.author: kumud
ms.custom: ''
ms.openlocfilehash: 19a641ba86333047b90c36cef242d524ccad5fe1
ms.sourcegitcommit: 0f54b9dbcf82346417ad69cbef266bc7804a5f0e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/26/2018
ms.locfileid: "50138162"
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Over het verzenden van Real User Measurements op Azure Traffic Manager met behulp van webpagina 's

U kunt uw webpagina's voor het verzenden van Real-User-metingen naar Traffic Manager door het verkrijgen van een echte gebruiker metingen (RUM)-sleutel en insluiten van de gegenereerde code naar webpagina kunt configureren.

## <a name="obtain-a-real-user-measurements-key"></a>Ophalen van een sleutel Real User Measurements

De metingen die u ondernemen en het verzenden naar Traffic Manager van uw clienttoepassing worden geïdentificeerd door de service met behulp van een unieke tekenreeks, met de naam van de **echte gebruiker metingen (RUM) sleutel**. U kunt een uitvoeren met behulp van Azure portal, een REST-API-sleutel ophalen of met behulp van PowerShell of Azure CLI.

Als u de RUM-sleutel met behulp van Azure portal:
1. Vanuit een browser, moet u zich aanmelden bij de Azure-portal. Als u nog een account hebt, kunt u zich aanmelden voor een gratis proefversie van één maand.
2. Zoek in de zoekbalk van de portal, de naam van het Traffic Manager-profiel dat u wilt wijzigen en klik vervolgens op het Traffic Manager-profiel in de resultaten die de weergegeven.
3. Klik in de blade Traffic Manager-profiel op **Real User Measurements** onder **instellingen**.
4. Klik op **sleutel genereren** om een nieuwe RUM-sleutel te maken.
 
  ![Real User Measurements sleutel genereren](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Afbeelding 1: Real-User-metingen genereren van sleutels**

5. De blade worden nu de RUM-sleutel wordt gegenereerd en een JavaScript-codefragment die moeten worden ingesloten in de HTML-pagina weergegeven.
 
    ![JavaScript-code voor Real User Measurements sleutel](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Afbeelding 2: Real-User Meetsleutel en meting JavaScript**
 
6.  Klik op de **kopie** knop om de JavaScript-code te kopiëren. 

>[!IMPORTANT]
> De gegenereerde JavaScript voor Real User Measurements functie gebruiken om te laten functioneren. Wijzigingen in dit script of de scripts die worden gebruikt door Real User Measurements kunnen leiden tot onvoorspelbaar gedrag.

## <a name="embed-the-code-to-an-html-web-page"></a>De code in een HTML-pagina insluiten

Nadat u de uitvoeren code hebt ontvangen, is de volgende stap deze gekopieerde JavaScript insluiten in een HTML-pagina die uw gebruikers bezoeken. Bewerken HTML-code kan worden gedaan op tal van manieren en het gebruik van verschillende hulpprogramma's en werkstromen. In dit voorbeeld laat zien hoe een HTML-pagina om toe te voegen met dit script bijwerken. U kunt deze richtlijnen gebruiken om aan te passen aan uw HTML-bron management-werkstroom.

1.  De HTML-pagina in een teksteditor openen
2.  Plak de JavaScript-code die u in de vorige stap had gekopieerd naar de sectie instantie van de HTML-code (de gekopieerde code zich op regel 8 en 9, zie afbeelding 3).
 
    ![Het insluiten van Javascript-code in webpagina's voor Real-User-metingen](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **Afbeelding 3: Eenvoudige HTML-code met ingesloten JavaScript met echte metingen**

3.  Sla het HTML-bestand en de host op een webserver met het internet. 
4. Volgende keer dat deze pagina wordt weergegeven in een webbrowser, de JavaScript waarnaar wordt verwezen is gedownload en het script wordt uitgevoerd de meting en rapportage van bewerkingen.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [Real User Measurements](traffic-manager-rum-overview.md)
- Informatie over [hoe Traffic Manager werkt](traffic-manager-overview.md)
- Meer informatie over de [routeringsmethoden voor verkeer](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over het [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)


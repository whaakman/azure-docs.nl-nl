---
title: Echte gebruiker metingen Azure Traffic Manager met webpagina's | Microsoft Docs
description: Instellen van uw webpagina's voor het verzenden van echte metingen van de gebruiker aan Traffic Manager
services: traffic-manager
documentationcenter: traffic-manager
author: KumudD
manager: jeconnoc
editor: 
tags: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: 
ms.workload: infrastructure
ms.date: 09/19/2017
ms.author: kumud
ms.custom: 
ms.openlocfilehash: 7f4088cf4470b1f9fa22c4ec83a9f92657032734
ms.sourcegitcommit: 9cc3d9b9c36e4c973dd9c9028361af1ec5d29910
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="how-to-send-real-user-measurements-to-azure-traffic-manager-using-web-pages"></a>Het verzenden van echte metingen van de gebruiker naar Azure Traffic Manager via de webpagina 's

>[!NOTE]
>De functie echte gebruiker metingen in Traffic Manager kan is openbare preview-versie en geen hetzelfde niveau van beschikbaarheid en betrouwbaarheid zoals functies die in het algemeen beschikbaarheid release. De functie wordt niet ondersteund, kan hebben beperkte mogelijkheden en mogelijk niet beschikbaar in alle Azure-locaties. Voor de meest recente meldingen op beschikbaarheid en de status van deze functie, Controleer de [Azure Traffic Manager-updates](https://azure.microsoft.com/updates/?product=traffic-manager) pagina.

U kunt uw webpagina's voor het verzenden van echte metingen van de gebruiker aan Traffic Manager door het verkrijgen van een echte gebruiker metingen (RUM)-sleutel en de gegenereerde code naar webpagina insluiten configureren.

## <a name="obtain-a-real-user-measurements-key"></a>Een echte gebruiker metingen-sleutel ophalen

De metingen u duren en verzendt aan Traffic Manager van uw clienttoepassing worden geïdentificeerd door de service met een unieke tekenreeks met de naam de **echte gebruiker metingen (RUM) sleutel**. U krijgt een RUMSECTOR sleutel met de Azure portal, een REST-API of met behulp van PowerShell of Azure CLI.

Als u de RUM sleutel met Azure portal:
1. Vanuit een browser, moet u zich aanmelden bij de Azure-portal. Als u nog een account hebt, kunt u zich aanmelden voor een gratis proefversie van één maand.
2. In de portal zoekbalk, zoekt u de naam van het Traffic Manager-profiel dat u wilt wijzigen en klik vervolgens op het Traffic Manager-profiel in de resultaten die de weergegeven.
3. Klik in de blade Traffic Manager-profiel op **echte gebruiker metingen** onder **instellingen**.
4. Klik op **sleutel genereren** een nieuwe RUM sleutel te maken.
 
  ![Echte gebruiker metingen sleutel genereren](./media/traffic-manager-create-rum-visual-studio/generate-rum-key.png)

   **Afbeelding 1: Echte gebruiker metingen genereren van sleutels**

5. De blade worden nu de RUM-sleutel wordt gegenereerd en een JavaScript-codefragment die moet worden ingesloten in de HTML-pagina weergegeven.
 
    ![JavaScript-code voor de sleutel van echte metingen van de gebruiker](./media/traffic-manager-create-rum-web-pages/rum-javascript-code.png)

    **Afbeelding 2: Echte gebruikerssleutel van metingen en meting JavaScript**
 
6.  Klik op de **kopie** om de JavaScript-code te kopiëren. 

>[!IMPORTANT]
> Gebruik de gegenereerde JavaScript voor echte gebruiker metingen functie te laten functioneren. Wijzigingen in dit script of de scripts die worden gebruikt door de echte metingen van de gebruiker kunnen leiden tot onvoorspelbaar gedrag.

## <a name="embed-the-code-to-an-html-web-page"></a>De code naar een HTML-pagina insluiten

Nadat u de RUMSECTOR code hebt ontvangen, wordt de volgende stap is het deze gekopieerde JavaScript insluiten in een HTML-pagina die uw eindgebruikers bezoeken. HTML bewerken kan worden uitgevoerd op vele manieren en het gebruik van verschillende hulpprogramma's en werkstromen. Dit voorbeeld ziet het bijwerken van een HTML-pagina als dit script wilt toevoegen. U kunt deze richtlijnen gebruiken om aan te passen aan uw HTML-bron management-werkstroom.

1.  De HTML-pagina in een teksteditor openen
2.  Plak de JavaScript-code die u in de vorige stap had gekopieerd naar de sectie BODY van de HTML-code (de gekopieerde code is op de regel 8 en 9, zie afbeelding 3).
 
    ![Javascript-code insluiten in webpagina's voor echte metingen van de gebruiker](./media/traffic-manager-create-rum-web-pages/real-user-measurement-embed-script.png)  

    **Afbeelding 3: Eenvoudige HTML met ingesloten JavaScript met echte metingen**

3.  De HTML-bestand opslaan
4. Zodra die deze pagina wordt weergegeven in een webbrowser JavaScript waarnaar wordt verwezen is gedownload en het script wordt uitgevoerd voor de meting en rapportagebewerkingen.


## <a name="next-steps"></a>Volgende stappen
- Meer informatie over [echte metingen van de gebruiker](traffic-manager-rum-overview.md)
- Meer informatie over [de werking van Traffic Manager](traffic-manager-overview.md)
- Meer informatie over de [verkeersroutering methoden](traffic-manager-routing-methods.md) ondersteund door Traffic Manager
- Meer informatie over hoe [een Traffic Manager-profiel maken](traffic-manager-create-profile.md)


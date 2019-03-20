---
title: Vul het formulier technische informatie over hoe u | Microsoft Docs
description: Wordt uitgelegd hoe u de waarden opgeven voor de in het formulier technische informatie voor een nieuwe Dynamics 365 Business Central-app.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: pbutlerm
ms.openlocfilehash: 5aa118815146287d00a3bb8ee7d5fce57a6ad9ca
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58120292"
---
<a name="how-to-fill-out-the-technical-info-form"></a>Vul het formulier technische informatie over hoe u
===========================================

1.  In de **toepassingstype Kies** sectie, upload het bestand in uw extensie-pakket (.app) en alle extensie-pakketbestanden uw extensie heeft een afhankelijkheid van.

    ![Toepassingspakket informatie](./media/d365-financials/image015.png)

-   **Het pakketbestand extensies** --vereist: het pakketbestand extensie (.app).

-   **Het pakketbestand afhankelijkheid** --vereist als de app is afhankelijk van een andere app is gepubliceerd in AppSource. Dit bestand .app van een al gepubliceerde uitbreiding in AppSource, die de huidige app afhankelijk is. 

-   **Bibliotheek pakketbestand** -als de app is afhankelijk van een andere app die is vereist *niet* gepubliceerd in AppSource. Deze .app-bestand van een bestaande app, maar een die niet is en niet worden gepubliceerd in AppSource.

-   **App Test Automation** --vereist: het testpakket VS code dat u maken moet voor automatisch testen van extensies.

1. In de **aanvullende informatie voor de extensie** sectie, aanvullende informatie voor de extensie uploaden. Deze informatie wordt gebruikt tijdens de validatie.

   ![Aanvullende informatie voor het app-extensie-formulier](./media/d365-financials/image016.png)


-   **URL naar de productdocumentatie** --vereist - URL de documentatie voor de extensie.

-   **Sleutel gebruiksscenario's** --vereist: een document met een lijst met de stap voor stap installatie en gebruik de details voor de extensie. Een voorbeeld kunt u vinden in het artikel [Scenario gebruikersdocumentatie](https://docs.microsoft.com/dynamics-nav/compliance/apptest-userscenario/).

-   **Doel Release** --vereist: Selecteer de-versie waarop de app implementeren. Selecteer **huidige** op de huidige versie van de markt te implementeren. Selecteer **vervolgens secundaire** te implementeren met de volgende secundaire versie moeten worden vrijgegeven. Selecteer **naast belangrijke** te implementeren met de volgende primaire versie worden vrijgegeven.

-   **Premium-SKU vereist** --optioneel--Selecteer de Premium-knop als de app is vereist voor de Premium-SKU. Service Management en productie zijn alleen beschikbaar voor premium. Gedetailleerde informatie over essentiële Visual Studio Premium u in het artikel vinden kunt [wijzigen welke functies worden weergegeven](https://docs.microsoft.com/dynamics365/financials/ui-experiences).

-   **Uitleg voor analyse codefouten** --Nepovinné--Document dat een lijst met en code die niet voldoet aan de vereisten uitlijnen.

-   **Uitleg van Kernfunctionaliteit beïnvloed** --Nepovinné--Document geeft een lijst en wordt uitgelegd core functionaliteit die wordt beperkt door de extensie.

-   **Testaccounts** --Nepovinné--gebruikersaccounts voor externe services, websites, enz. die nodig is om uit te voeren van het gebruik van de end-to-test.

-   **UX vereisten uitzonderingen** --Nepovinné--Document dat een lijst met en zijn niet voldaan aan door de extensie vereisten voor de ervaring van een gebruiker.

De volgende stap is het toevoegen van storefront details voor uw aanbieding.

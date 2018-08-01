---
title: Geen werkende connectorgroep gevonden voor een toepassingsproxy-toepassing | Microsoft Docs
description: Oplossingen voor problemen die optreden wanneer er geen werkende Connector in een Connectorgroep voor uw toepassing met de Azure AD-toepassingsproxy
services: active-directory
documentationcenter: ''
author: barbkess
manager: mtillman
ms.assetid: ''
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: asteen
ms.openlocfilehash: 82014e1eaadc343ac320b0f4b3091d530228a8c5
ms.sourcegitcommit: f86e5d5b6cb5157f7bde6f4308a332bfff73ca0f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2018
ms.locfileid: "39366158"
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Geen werkende connector-groep voor een toepassing Application Proxy gevonden

Dit artikel helpt bij het oplossen van de veelvoorkomende problemen gehad als er geen een connector voor een geïntegreerd met Azure Active Directory Application Proxy-toepassing gedetecteerd.

## <a name="overview-of-steps"></a>Overzicht van stappen
Als er geen werkende Connector in een Connectorgroep voor uw toepassing, zijn er enkele manieren om het probleem te verhelpen:

-   Als u geen connectors in de groep hebt, kunt u het volgende doen:

    -   Een nieuwe Connector op de juiste on-premises server downloaden en deze toewijzen aan deze groep

    -   Een actieve Connector verplaatsen naar de groep

-   Als u geen actieve connectors in de groep hebt, kunt u het volgende doen:

    -   Identificeer de reden dat de Connector niet actief is en op te lossen

    -   Een actieve Connector verplaatsen naar de groep

Als u wilt weten het probleem, opent u het menu "Application Proxy" in uw toepassing en kijken naar het Connectorgroep waarschuwingsbericht staan aangegeven. Als er geen connectors in de groep, wordt de waarschuwing geeft aan dat de groep moet ten minste één Connector. Als u geen actieve Connectors hebt, wordt die door het waarschuwingsbericht staan aangegeven uitgelegd. Het is gebruikelijk dat niet-actieve Connectors. 

   ![Connector-groepsselectie in Azure portal](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Zie voor meer informatie over elk van deze opties, de bijbehorende sectie hieronder. De instructies wordt ervan uitgegaan dat u vanaf de beheerpagina van de Connector opstart. Als u de bovenstaande foutbericht bekijkt, kunt u naar deze pagina gaan door te klikken op het waarschuwingsbericht staan aangegeven. U kunt ook naar de pagina ophalen door te gaan naar **Azure Active Directory**, te klikken op **bedrijfstoepassingen**, klikt u vervolgens **Application Proxy.**

   ![Beheer van de connector-groepen in Azure portal](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Een nieuwe Connector downloaden

Gebruik de knop 'Downloaden Connector' aan de bovenkant van de pagina voor het downloaden van een nieuwe Connector.

De connector installeren op een computer met rechtstreekse verbinding naar de back endtoepassing. Normaal gesproken is de connector geïnstalleerd op dezelfde server als de toepassing. Nadat u hebt gedownload, moet de Connector worden weergegeven in dit menu. Klik op de Connector en de 'Connector groeperen' vervolgkeuzelijst gebruiken om ervoor te zorgen dat hoort bij de juiste groep. Sla de wijziging.

   ![Download de connector van de Azure-portal](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Verplaatsen van een actieve Connector

Hebt u een actieve Connector die moet deel uitmaken van de groep en peeren aan de doeltoepassing back-end heeft, kunt u de Connector verplaatsen naar de toegewezen groep. Om dit te doen, klikt u op de Connector. Gebruik de vervolgkeuzelijst de juiste groep selecteren en klik op opslaan in het veld 'Connectorgroep'.

## <a name="resolve-an-inactive-connector"></a>Een niet-actieve Connector oplossen

Als de enige Connectors in de groep niet actief zijn, is ze waarschijnlijk op een computer die beschikt niet over de vereiste poorten gedeblokkeerd.

Raadpleeg het document over poorten problemen oplossen voor meer informatie over dit probleem te onderzoeken.

## <a name="next-steps"></a>Volgende stappen
[Meer informatie over Azure AD Application Proxy connectors](manage-apps/application-proxy-connectors.md)



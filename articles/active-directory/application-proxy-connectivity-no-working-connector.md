---
title: Er is geen connector werkgroep gevonden voor een toepassing toepassingsproxy | Microsoft Docs
description: Oplossingen voor problemen die optreden mogelijk wanneer er geen be-Connector in een groep Connector voor uw toepassing met de Azure AD-toepassingsproxy
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
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.openlocfilehash: f6baea8c2702fc2a86c75d32c7b4ca958fd913b1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34591742"
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Er is geen werkgroep connector gevonden voor een toepassing toepassingsproxy

In dit artikel helpt bij het omzetten van de algemene problemen wanneer er geen een gedetecteerd voor een toepassing toepassingsproxy is geïntegreerd met Azure Active Directory-connector.

## <a name="overview-of-steps"></a>Overzicht van stappen
Als er geen be-Connector in een groep Connector voor uw toepassing, zijn er enkele manieren om het probleem te verhelpen:

-   Als u geen connectors in de groep hebt, kunt u:

    -   Een nieuwe Connector op de juiste on-premises server downloaden en deze toewijzen aan deze groep

    -   Een actieve Connector verplaatsen naar de groep

-   Als u geen actieve connectors in de groep hebt, kunt u:

    -   De reden dat de Connector niet actief is identificeren en oplossen

    -   Een actieve Connector verplaatsen naar de groep

Als u wilt nagaan wat het probleem, opent u het menu 'Application Proxy' in uw toepassing en bekijk de waarschuwing Connector groep. Als er geen connectors in de groep, wordt de waarschuwing geeft aan dat de groep moet ten minste één Connector. Als u geen actieve Connectors hebt, wordt die de waarschuwing uitgelegd. Het is gebruikelijk om inactieve Connectors. 

   ![De selectie van connector in Azure-portal](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Zie voor meer informatie over elk van deze opties, de overeenkomstige sectie hieronder. De instructies wordt ervan uitgegaan dat u op de pagina van de Connector-beheer begint. Als u de bovenstaande foutbericht bekijkt, kunt u naar deze pagina gaan door te klikken op het waarschuwingsbericht staan aangegeven. U kunt ook naar de pagina opvragen door te gaan naar **Azure Active Directory**, te klikken op **bedrijfstoepassingen**, klikt u vervolgens **Application Proxy.**

   ![Connector-groepsbeheer in Azure-portal](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Een nieuwe Connector downloaden

Gebruik de knop 'Connector downloaden' boven aan de pagina voor het downloaden van een nieuwe Connector.

De connector installeren op een machine met directe regel zicht naar de back-end-toepassing. Normaal gesproken is de connector geïnstalleerd op dezelfde server als de toepassing. Nadat u hebt gedownload, moet de Connector worden weergegeven in het menu. Klik op de Connector en de 'Connector groep' vervolgkeuzelijst gebruiken om ervoor te zorgen dat deze aan de juiste groep behoort. Sla de wijziging.

   ![De connector downloaden van de Azure-portal](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Verplaatsen van een actieve Connector

Als u een actieve Connector die u moet behoren tot de groep en heeft regel zicht naar de back-end-doeltoepassing hebt, kunt u de Connector kunt verplaatsen naar de groep toegewezen. Om dit te doen, klikt u op de Connector. Gebruik de vervolgkeuzelijst selecteert u de juiste groep en klik op opslaan in het veld 'Connector groep'.

## <a name="resolve-an-inactive-connector"></a>Een inactieve Connector oplossen

Als de enige Connectors in de groep niet actief zijn, zijn deze waarschijnlijk op een computer waarop u beschikt niet over de vereiste poorten gedeblokkeerd.

Zie het document poorten oplossen voor meer informatie over dit probleem te onderzoeken.

## <a name="next-steps"></a>Volgende stappen
[Azure AD-toepassingsproxy connectors begrijpen](manage-apps/application-proxy-connectors.md)



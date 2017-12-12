---
title: Er is geen connector werkgroep gevonden voor een toepassing toepassingsproxy | Microsoft Docs
description: Oplossingen voor problemen die optreden mogelijk wanneer er geen be-Connector in een groep Connector voor uw toepassing met de Azure AD-toepassingsproxy
services: active-directory
documentationcenter: 
author: ajamess
manager: mtillman
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/11/2017
ms.author: asteen
ms.openlocfilehash: 7aad43be4dc411215a4b54c21a16c4cbb949c7ec
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="no-working-connector-group-found-for-an-application-proxy-application"></a>Er is geen werkgroep connector gevonden voor een toepassing toepassingsproxy

Deze Help-artikel informatie die u bij het oplossen van de algemene problemen wanneer er geen een gedetecteerd voor een toepassing Application Proxy connector geïntegreerd met Azure Active Directory.

## <a name="overview-of-steps"></a>Overzicht van stappen
Als er geen be-Connector in een groep Connector voor uw toepassing, zijn er enkele manieren om het probleem te verhelpen:

-   Als u geen connectors in de groep hebt, kunt u:

    -   Een nieuwe Connector op de juiste on-premises server downloaden en deze toewijzen aan deze groep

    -   Een actieve Connector verplaatsen naar de groep

-   Als u geen actieve connectors in de groep hebt, kunt u:

    -   De reden dat de Connector niet actief is identificeren en oplossen

    -   Een actieve Connector verplaatsen naar de groep

Als u wilt weten wat het probleem is, opent u het menu 'Application Proxy' in uw toepassing en bekijk de waarschuwing Connector groep. Het dat de groep moet ten minste één Connector (u hebt geen zijn in de groep) of het heeft geen actieve verbindingslijnen (Hoewel u waarschijnlijk inactieve Connectors hebt) opgeven.

   ![De selectie van connector in Azure Portal](./media/application-proxy-connectivity-no-working-connector/no-active-connector.png)

Zie voor meer informatie over elk van deze opties, de overeenkomstige sectie hieronder. Elk van deze wordt ervan uitgegaan dat u op de pagina van de Connector-beheer begint. Als u de bovenstaande foutbericht bekijkt, kunt u naar deze pagina gaan door te klikken op het waarschuwingsbericht staan aangegeven. Anders dit kunt u vinden door te gaan naar **Azure Active Directory**, te klikken op **bedrijfstoepassingen**, klikt u vervolgens **Application Proxy.**

   ![Connector-groepsbeheer in Azure Portal](./media/application-proxy-connectivity-no-working-connector/app-proxy.png)

## <a name="download-a-new-connector"></a>Een nieuwe Connector downloaden

Gebruik de knop 'Connector downloaden' boven aan de pagina voor het downloaden van een nieuwe Connector.

Opmerking van de Connector moet worden geïnstalleerd op een machine met directe regel zicht naar de back-end-toepassing en meestal op dezelfde server als de toepassing wordt geplaatst. Nadat u hebt gedownload, moet de Connector worden weergegeven in het menu. Klik op de Connector en de 'Connector groep' vervolgkeuzelijst gebruiken om ervoor te zorgen dat deze aan de juiste groep behoort. Sla de wijziging.

   ![De connector downloaden vanuit de Azure Portal](./media/application-proxy-connectivity-no-working-connector/download-connector.png)
   
## <a name="move-an-active-connector"></a>Verplaatsen van een actieve Connector

Als u een actieve Connector die u moet behoren tot de groep en heeft regel zicht naar de back-end-doeltoepassing hebt, kunt u de Connector kunt verplaatsen naar de groep toegewezen. Om dit te doen, klikt u op de Connector. Gebruik de vervolgkeuzelijst selecteert u de juiste groep en klik op opslaan in het veld 'Connector groep'.

## <a name="resolve-an-inactive-connector"></a>Een inactieve Connector oplossen

Als de enige Connectors in de groep niet actief zijn, zijn deze waarschijnlijk op een computer waarop u beschikt niet over de vereiste poorten gedeblokkeerd.

Zie het document poorten oplossen voor meer informatie over dit probleem te onderzoeken.

## <a name="next-steps"></a>Volgende stappen
[Azure AD-toepassingsproxy connectors begrijpen](application-proxy-understand-connectors.md)



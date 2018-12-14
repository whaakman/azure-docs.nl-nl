---
title: Statussen van waarschuwingen en slimme groepen beheren
description: De status van de waarschuwing en slimme-instanties beheren
author: anantr
services: monitoring
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 09/24/2018
ms.author: anantr
ms.component: alerts
ms.openlocfilehash: 88601383df5015f9ea23184d65266974bb0f35e1
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53346086"
---
# <a name="manage-alert-and-smart-group-states"></a>Statussen van waarschuwingen en slimme groepen beheren
Waarschuwingen in Azure Monitor beschikt nu over een [status en een controleconditie gewaarschuwd](https://aka.ms/azure-alerts-overview) en op dezelfde manier, slimme groepen hebben een [groepsstatus slimme](https://aka.ms/smart-groups). Wijzigingen in de staat worden nu in de geschiedenis die zijn gekoppeld aan de respectieve waarschuwing of smart-groep vastgelegd. In dit artikel begeleidt u bij het proces van het wijzigen van de status, voor zowel een waarschuwing als een slimme groep.

## <a name="change-the-state-of-an-alert"></a>De status van een waarschuwing wijzigen
1. U kunt de status van een waarschuwing wijzigen in de volgende manieren: 
    * Klikt u op het selectievakje naast de waarschuwingen die u wilt wijzigen van de status van alle waarschuwingen op de pagina en klikt u op status wijzigen.   
    ![Bewaking](./media/alerts-managing-alert-states/state-all-alerts.jpg)
    * In de pagina met Details van de waarschuwing voor een bepaalde instantie van de waarschuwing, klikt u op status wijzigen   
    ![Bewaking](./media/alerts-managing-alert-states/state-alert-details.jpg)
    * In de pagina met Details van de waarschuwing voor een specifiek exemplaar van de waarschuwing, in het deelvenster Smart-groep, kunt u het selectievakje naast de waarschuwingen die u wilt    
    ![Bewaking](./media/alerts-managing-alert-states/state-alert-details-sg.jpg)

    * Op de pagina slimme Groepsdetails in de lijst met waarschuwingen lid kunt u op het selectievakje naast de waarschuwingen die u wilt de status van wijzigen en klik op wijzigen Stateto wijzigen de status van en op status wijzigen.   
    ![Bewaking](./media/alerts-managing-alert-states/state-sg-details-alerts.jpg)
1. Op status wijzigen klikt, wordt een pop-upvenster geopend omhoog zodat u kunt de status (nieuw/bevestigd/gesloten) selecteren en voer een opmerking indien nodig.   
![Bewaking](./media/alerts-managing-alert-states/state-alert-change.jpg)
1. Zodra deze actie is uitgevoerd, wordt de wijziging in de status geregistreerd in de geschiedenis van de respectieve waarschuwing. Dit kan worden weergegeven door de bijbehorende detailpagina te openen en het controleren van de sectie Geschiedenis.    
![Bewaking](./media/alerts-managing-alert-states/state-alert-history.jpg)

## <a name="change-the-state-of-a-smart-group"></a>De status van een slimme groep wijzigen
1. U kunt de status van een slimme groep wijzigen in de volgende manieren:
    1. Op de pagina van de lijst met smartcards groep, kunt u het selectievakje naast de slimme groepen die u wilt de status van wijzigen en klik op status wijzigen  
    ![Bewaking](./media/alerts-managing-alert-states/state-sg-list.jpg)
    1. Pagina met Details van slimme groep, klikt u op status wijzigen        
    ![Bewaking](./media/alerts-managing-alert-states/state-sg-details.jpg)
1. Op status wijzigen klikt, wordt een pop-upvenster geopend omhoog zodat u kunt de status (nieuw/bevestigd/gesloten) selecteren en voer een opmerking indien nodig. 
![Controle](./media/alerts-managing-alert-states/state-sg-change.jpg)
   > [!NOTE]
   >  Als u de status van een slimme groep wijzigt, wordt de status van de waarschuwingen afzonderlijk lid niet gewijzigd.

1. Zodra deze actie is uitgevoerd, wordt de wijziging in de status geregistreerd in de geschiedenis van de betreffende slimme groep. Dit kan worden weergegeven door de bijbehorende detailpagina te openen en het controleren van de sectie Geschiedenis.     
![Bewaking](./media/alerts-managing-alert-states/state-sg-history.jpg)

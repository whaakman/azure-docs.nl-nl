---
title: Waarschuwingen met Azure Sentinel Preview onderzoeken | Microsoft Docs
description: Gebruik deze zelfstudie voor informatie over het onderzoeken van waarschuwingen met Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: b5fbc5ac-68b2-4024-9c1b-bd3cc41a66d0
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/20/2019
ms.author: rkarlin
ms.openlocfilehash: a0a16fbda662ee1d3718dbafc4231de67aab277e
ms.sourcegitcommit: 81fa781f907405c215073c4e0441f9952fe80fe5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/25/2019
ms.locfileid: "58400675"
---
# <a name="tutorial-detect-threats-with-azure-sentinel-preview"></a>Zelfstudie: Detectie van bedreigingen met Azure Sentinel Preview

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Nadat u [uw gegevensbronnen verbonden](quickstart-onboard.md) Azure Sentinel, u wilt worden gewaarschuwd wanneer er iets verdachte gebeurt. Zodat u kunt dit doen Sentinel van Azure kunt u geavanceerde regels voor waarschuwingen, die aanvragen die u kunt genereren en gebruiken voor het onderzoeken van diep afwijkingen en bedreigingen in uw omgeving. 

Deze zelfstudie helpt u bedreigingen in een met Azure Sentinel.
> [!div class="checklist"]
> * Detectieregels maken
> * Op bedreigingen reageren

## <a name="create-detection-rules"></a>Detectieregels maken

Voor het onderzoeken van aanvragen, moet u eerst detectieregels maken. 

> [!NOTE]
> Waarschuwingen die zijn gegenereerd in Azure Sentinel zijn beschikbaar via [Microsoft Graph Security](https://aka.ms/securitygraphdocs). Raadpleeg de [documentatie voor Microsoft Graph-beveiligingswaarschuwingen](https://aka.ms/graphsecurityreferencebetadocs) voor meer informatie en van integratiepartners.

Detectieregels zijn gebaseerd op de typen dreigingen en afwijkingen die gebruikt in uw omgeving die u weten worden kunnen wilt over meteen verdachte ervoor te zorgen dat ze zijn opgehaald, onderzocht, en hersteld. 

1. Selecteer in de Azure-portal onder Azure Sentinel **Analytics**.

   ![Analyse](./media/tutorial-detect-threats/alert-rules.png)

2. Klik in de bovenste menubalk op **+ toevoegen**.  

   ![Waarschuwingsregel maken](./media/tutorial-detect-threats/create-alert-rule.png)

3. Onder **waarschuwingsregel maken**, Geef een beschrijvende naam en stel de **ernst** indien nodig. 

4. De query is gemaakt in Log Analytics en plak deze in de **Set waarschuwingsregel** veld. Hier volgt een voorbeeldquery die u waarschuwen wilt wanneer een afwijkende aantal resources in Azure-activiteit wordt gemaakt.

        AzureActivity
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"
        | where ActivityStatus == "Succeeded"
        | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller

5. In de **entiteitstoewijzing** sectie, gebruikt u de velden onder **entiteitstype** toewijzen van de kolommen in uw query aan entiteitsvelden wordt herkend door het Azure-Sentinel. Voor elk veld de betreffende kolom in de query die u hebt gemaakt in Log Analytics, naar het entiteitveld juiste worden toegewezen. Selecteer de naam van de desbetreffende kolom onder de **eigenschap**. Elke entiteit bevat meerdere velden, zoals SID, GUID, enzovoort. U kunt de entiteit op basis van een van de velden, niet alleen de bovenste niveau entiteit toewijzen.

6. Voorwaarden voor de waarschuwingsactivering definiëren **waarschuwingsactivering**. Hiermee definieert u de voorwaarden die de waarschuwing. 

7. Stel de **frequentie** voor hoe vaak de query wordt uitgevoerd - als vaak als elke 5 minuten of weinig eenmaal per dag. 

8. Stel de **periode** voor het beheren van het tijdvenster voor de hoeveelheid gegevens die de query wordt uitgevoerd op - bijvoorbeeld het kunt uitvoeren om het uur over 60 minuten aan gegevens.

9. U kunt ook instellen de **onderdrukking**. Onderdrukking is handig als u stoppen van dubbele waarschuwingen wilt van hetzelfde incident wordt geactiveerd. Op deze manier kunt u waarschuwingen van die wordt geactiveerd tijdens een bepaalde periode stoppen. Dit kan helpen u bij het voorkomen van dubbele waarschuwingen voor hetzelfde incident en kunt u opeenvolgende waarschuwingen onderdrukken gedurende een bepaalde periode. Bijvoorbeeld, als de **waarschuwen planning** **frequentie** is ingesteld op 60 minuten en de **waarschuwen planning periode** is ingesteld op twee uur en resultaten van de query de gedefinieerde overschreden drempelwaarde, er wordt een waarschuwing geactiveerd tweemaal zodra wanneer het eerst wordt gedetecteerd in de afgelopen 60 minuten en het opnieuw wanneer het zich in de eerste 60 minuten van de 2-uren van gegevens, worden steekproeven genomen. U wordt aangeraden dat als een waarschuwing wordt geactiveerd, de onderdrukking voor de hoeveelheid tijd die is ingesteld in de waarschuwing periode moet zijn. In ons voorbeeld, kunt u om in te stellen onderdrukking gedurende 60 minuten zodat de worden alleen waarschuwingen gegenereerd voor gebeurtenissen die hebben plaatsgevonden in het meest recente uur.

8. Nadat u plakt u de query in de **Set waarschuwingsregel** veld, kunt u direct een simulatie van de waarschuwing onder zien **alert Logic-simulatie** zodat krijgt u inzicht in van hoeveel gegevens worden gegenereerd gedurende een bepaald tijdsinterval voor de waarschuwing die u hebt gemaakt. Dit is afhankelijk van wat u instelt voor **frequentie** en **drempelwaarde**. Als u ziet dat gemiddeld, de waarschuwing wordt geactiveerd te vaak, wilt u het aantal resultaten hoger instellen zodat deze hoger dan de gemiddelde basislijn is.

9. Klik op **maken** initialiseren van de waarschuwingsregel. Nadat de waarschuwing is gemaakt, wordt er een aanvraag gemaakt die de waarschuwing bevat. U kunt de gedefinieerde detectieregels zien als rijen in de **beveiligingsanalyses** tabblad. U ziet ook het aantal overeenkomsten voor elke regel - de waarschuwingen geactiveerd. In deze lijst die kunt u inschakelen, uitschakelen of verwijderen van elke regel. U kunt ook rechts: Selecteer het weglatingsteken (...) aan het einde van de rij voor elke waarschuwing te bewerken, uitschakelen, klonen, komt overeen met weergeven of verwijderen van een regel. De **Analytics** pagina is een galerie met alle actieve waarschuwingsregels, waaronder sjablonen u inschakelen en waarschuwingsregels u maken op basis van sjablonen.

1. De resultaten van de regels voor waarschuwingen kunnen worden weergegeven de **gevallen** pagina, waar u sorteren kunt, [onderzoeken gevallen](tutorial-investigate-cases.md), en de bedreigingen te herstellen.



## <a name="respond-to-threats"></a>Op bedreigingen reageren

Azure Sentinel kunt u twee primaire manieren voor het reageren op bedreigingen met behulp van playbooks. U kunt een playbook worden automatisch uitgevoerd wanneer een waarschuwing wordt geactiveerd, of u handmatig een playbook in reactie op een waarschuwing uitvoeren kunt instellen.

- Stel een playbook worden automatisch uitgevoerd wanneer een waarschuwing wordt geactiveerd wanneer u de playbook configureren. 

- Een playbook uit in de waarschuwing handmatig uitvoeren door te klikken op **playbooks weergeven** en selecteer vervolgens een playbook om uit te voeren.




## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u aan de slag voor het detecteren van bedreigingen met behulp van Azure Sentinel. 

Voor informatie over het automatiseren van uw antwoorden op bedreigingen, [het reageren op bedreigingen met behulp van geautomatiseerde playbooks](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Reageren op bedreigingen](tutorial-respond-threats-playbook.md) voor het automatiseren van uw antwoorden op bedreigingen.


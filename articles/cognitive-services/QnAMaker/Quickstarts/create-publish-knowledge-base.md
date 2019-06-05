---
title: Maken, trainen en publiceren van knowledge base - QnA Maker
titleSuffix: Azure Cognitive Services
description: U kunt een QnA Maker-knowledge base (KB) maken van uw eigen inhoud, zoals veelgestelde vragen of producthandleidingen. De QnA Maker knowledge base in dit voorbeeld is gemaakt op basis van een eenvoudige Veelgestelde vragen over webpagina om te beantwoorden vragen over herstel van BitLocker-sleutels.
author: diberry
manager: nitinme
services: cognitive-services
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: quickstart
ms.date: 05/10/2019
ms.author: diberry
ms.openlocfilehash: 609d71898d8db027f1cfee9e1b73039367ec94f4
ms.sourcegitcommit: 18a0d58358ec860c87961a45d10403079113164d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2019
ms.locfileid: "66693377"
---
# <a name="create-train-and-publish-your-qna-maker-knowledge-base"></a>Uw QnA Maker-knowledge base maken, trainen en publiceren

U kunt een QnA Maker-knowledge base (KB) maken van uw eigen inhoud, zoals veelgestelde vragen of producthandleidingen. Dit artikel bevat een voorbeeld van het maken van een kennisdatabase QnA Maker van een eenvoudige Veelgestelde vragen over webpagina, om te beantwoorden van vragen over herstel van BitLocker-sleutels.

## <a name="prerequisite"></a>Vereiste

> [!div class="checklist"]
> * Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-qna-maker-knowledge-base"></a>Een QnA Maker-knowledge base maken

1. Aanmelden bij de [QnAMaker.ai](https://QnAMaker.ai) portal met uw Azure-referenties.

1. Selecteer op de portal QnA Maker **maken van een kennisdatabase**.

   ![Schermafbeelding van de QnA Maker portal](../media/qna-maker-create-kb.png)

1. Selecteer op de pagina **Create** (Maken) in stap 1 de optie **Create a QnA service** (Een QnA-service maken). U wordt omgeleid naar de [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) om een QnA Maker-service in te stellen in uw abonnement. Als er een time-out optreedt in de Azure Portal, selecteert u **Opnieuw proberen** op de site. Wanneer de verbinding is gemaakt, wordt uw Azure-dashboard weergegeven.

1. Nadat u een nieuwe QnA Maker-service hebt gemaakt in Azure, gaat u terug naar qnamaker.ai/create. Selecteer de QnA Maker-service in de vervolgkeuzelijsten in stap 2. Als u een nieuwe QnA Maker-service hebt gemaakt, moet u de pagina te vernieuwen.

   ![Schermafbeelding van het selecteren van een kennisdatabase QnA Maker-service](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

1. In stap 3, naam op voor uw knowledge base **mijn voorbeeld QnA KB**.

1. Selecteer drie soorten gegevensbronnen om inhoud toe aan uw knowledge base. Voeg in stap 4, onder **Populate your KB** (De KB vullen), de URL [Veelgestelde vragen over BitLocker-herstel](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) toe in het vak **URL**.

   ![Schermafbeelding van het toevoegen van gegevensbronnen](../media/qnamaker-quickstart-kb/add-datasources.png)

1. Selecteer in stap 5 **Create your KB** (De KB maken).

1. Terwijl QnA Maker wordt gemaakt van de knowledge base, wordt er een pop-upvenster weergegeven. Tijdens het extractieproces, dat enkele minuten duurt, wordt de HTML-pagina gelezen en worden de vragen en antwoorden geïdentificeerd.

1. Nadat de QnA Maker is gemaakt de knowledge base, de **Knowledge base** pagina wordt geopend. U kunt de inhoud van de knowledge base op deze pagina kunt bewerken.

## <a name="edit-the-knowledge-base"></a>De knowledge base bewerken

1. In de QnA Maker-portal op de **bewerken** sectie, selecteer **QnA toevoegen paar** naar een nieuwe rij toevoegt aan de knowledge base. Typ onder **Question** (Vraag) **Hoi.** Typ onder **Answer** (Antwoord) **Hallo. Stel mij BitLocker vragen.**

    ![Schermafbeelding van de QnA Maker portal](../media/qnamaker-quickstart-kb/add-qna-pair.png)

1. Selecteer in de rechterbovenhoek **Save and train** (Opslaan en trainen) om de wijzigingen op te slaan en het QnA Maker-model te trainen. Bewerkingen worden alleen bewaard als ze worden opgeslagen.

## <a name="test-the-knowledge-base"></a>Testen van de knowledge base

1. Selecteer in de rechterbovenhoek van de portal QnA Maker **testen** om te testen of de wijzigingen van kracht. Typ `hi there` in het vak en selecteer Enter. Als het goed is, ziet u nu het antwoord dat u hebt gemaakt.

1. Selecteer **Inspect** (Inspecteren) om het antwoord gedetailleerder te onderzoeken. Het testvenster wordt gebruikt om uw wijzigingen in de knowledge base testen voordat ze worden gepubliceerd.

    ![Schermopname van het Configuratiescherm van de test](../media/qnamaker-quickstart-kb/inspect.png)

1. Selecteer **Test** (Testen) nogmaals om de pop-up **Test** te sluiten.

## <a name="publish-the-knowledge-base"></a>De knowledge base publiceren

Wanneer u een kennisdatabase publiceren, wordt de status van de vraag en antwoord inhoud van uw knowledge base verplaatst van de test-index naar een productie-index in Azure search.

![Schermafbeelding van het verplaatsen van de inhoud van uw knowledge base](../media/qnamaker-how-to-publish-kb/publish-prod-test.png)

1. In de portal QnA Maker, in het menu naast **bewerken**, selecteer **publiceren**. Selecteer vervolgens **Publish** (Publiceren) op de pagina om te bevestigen.

1. De QnA Maker-service is nu gepubliceerd. U kunt het eindpunt in uw toepassing of botcode gebruiken.

    ![Schermopname van geslaagde publiceren](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="create-a-bot"></a>Maken van een bot

Nadat deze is gepubliceerd, kunt u een bot uit de **publiceren** pagina: 

* U kunt verschillende bots snel, maken alle die verwijst naar de dezelfde knowledge base voor verschillende regio's of plannen voor de afzonderlijke bots prijzen. 
* Als u wilt dat slechts één bot voor de knowledge base, gebruikt u de **weergeven van alle uw bots op de Azure-portal** koppeling om een lijst met uw huidige bots weer te geven. 

Wanneer u wijzigingen in de knowledge base en opnieuw publiceren aanbrengt, moet u geen verdere actie met de bot te ondernemen. Er al geconfigureerd voor samenwerking met de knowledge base en werkt met alle toekomstige wijzigingen in de knowledge base. Telkens wanneer u een kennisdatabase publiceren, worden alle bots die zijn verbonden met het automatisch bijgewerkt.

1. In de QnA Maker-portal op de **publiceren** weergeeft, schakelt **bot maken**. Deze knop wordt weergegeven nadat u de knowledge base hebt gepubliceerd.

    ![Schermopname van het maken van een bot](../media/qnamaker-create-publish-knowledge-base/create-bot-from-published-knowledge-base-page.png)

1. Er wordt een nieuw browsertabblad geopend voor de Azure-portal, met de pagina voor het maken van de Azure Bot Service. De Azure botservice configureren. Zie voor meer informatie over deze configuratie-instellingen, [QnA Bot maken met Azure Bot Service v4](../tutorials/create-qna-bot.md).
    
    * De volgende instellingen in de Azure-portal niet te wijzigen bij het maken van de bot. Ze worden ingevuld voor uw bestaande knowledge base: 
        * QnA verificatiesleutel
        * App service-plan en de locatie
        * Azure Storage
    * De bot en de QnA Maker kunnen de web-app service-plan delen, maar de web-app niet delen. Dit betekent dat de **appnaam** moet verschillen van de naam van de app die u hebt gebruikt tijdens het maken van de QnA Maker-service. 


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase maken](../How-To/create-knowledge-base.md)

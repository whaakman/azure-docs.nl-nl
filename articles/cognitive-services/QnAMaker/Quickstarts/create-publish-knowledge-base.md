---
title: 'QuickStart: Maken van een KB - QnA Maker'
titleSuffix: Azure Cognitive Services
description: U kunt een QnA Maker knowledge base (KB) van uw eigen inhoud, zoals veelgestelde vragen over of producthandleidingen maken. De QnA Maker KB in dit voorbeeld is gemaakt op basis van een eenvoudige Veelgestelde vragen over webpagina om te beantwoorden vragen over herstel van BitLocker-sleutels.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: f7af86687a8a61fb7aed028d2868752faaa8045a
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47030251"
---
# <a name="create-train-and-publish-your-knowledge-base"></a>Maken, trainen en publiceren van uw knowledge base

U kunt een QnA Maker knowledge base (KB) van uw eigen inhoud, zoals veelgestelde vragen over of producthandleidingen maken. De QnA Maker KB in dit voorbeeld is gemaakt op basis van een eenvoudige Veelgestelde vragen over webpagina om te beantwoorden vragen over herstel van BitLocker-sleutels.

## <a name="prerequisite"></a>Vereiste

> [!div class="checklist"]
> * Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-qna-maker-knowledge-base"></a>Maken van een kennisdatabase QnA Maker

1. Meld u aan QnAMaker.ai met uw Azure-referenties.

2. Selecteer op de website QnA Maker **maken van een kennisdatabase**.

   ![Maken van nieuwe KB](../media/qna-maker-create-kb.png)

3. Op de **maken** pagina, klikt u in stap 1, selecteer **maken van een service QnA**. U bent omgeleid naar de [Azure-portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) voor het instellen van een QnA Maker-service in uw abonnement. Als er is een time-out opgetreden voor de Azure-portal, selecteert u **probeer het opnieuw** op de site. Nadat u verbinding maakt, wordt uw Azure-dashboard weergegeven.

4. Nadat u een nieuwe QnA Maker-service is gemaakt in Azure, kunt u terugkeren naar qnamaker.ai/create. Selecteer de QnA-service in de vervolgkeuzelijsten in stap 2. Als u een nieuwe Writer-service hebt gemaakt, moet u de pagina te vernieuwen.

   ![Selecteer een service QnA KB](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. In stap 3, naam op voor uw KB **mijn voorbeeld QnA KB**.

6. Inhoud toevoegen aan uw KB, selecteert u drie soorten gegevensbronnen. In stap 4, onder **vullen uw KB**, toevoegen de [Veelgestelde vragen over het BitLocker-herstel](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) in de URL van de **URL** vak.

   ![Selecteer een service QnA KB](../media/qnamaker-quickstart-kb/add-datasources.png)

7. Selecteer in stap 5 **maken uw KB**.

8. Terwijl de KB wordt gemaakt, wordt er een pop-upvenster weergegeven. De extractie-proces duurt enkele minuten voor het lezen van de HTML-pagina en identificeren van vragen en antwoorden.

9. Nadat de KB is gemaakt, de **Knowledge base** pagina wordt geopend. U kunt de inhoud van de KB op deze pagina kunt bewerken.

10. Selecteer in de rechterbovenhoek **QnA toevoegen paar** om toe te voegen een nieuwe rij in de **redactionele** sectie van de KB. Onder **vraag**, voer **Hallo.** Onder **antwoord**, voer **Hello. Stel mij bitlocker vragen.**

   ![Een set QnA toevoegen](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. Selecteer in de rechterbovenhoek **opslaan en trainen** en de wijzigingen opslaan voor de QnA Maker-model te trainen. Bewerkingen worden niet bewaard, tenzij ze worden opgeslagen.

   ![Opslaan en trainen](../media/qnamaker-quickstart-kb/add-qna-pair2.png)

12. Selecteer in de rechterbovenhoek **testen** om te testen of de wijzigingen van kracht. ENTER **Hallo er** in het vak en drukt u op Enter. U ziet het antwoord dat u hebt gemaakt als een antwoord.

13. Selecteer **inspecteren** om te controleren van het antwoord in meer detail. Het testvenster wordt gebruikt om uw wijzigingen in de KB testen voordat ze worden gepubliceerd.

   ![Test-Configuratiescherm](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Selecteer **Test** opnieuw uit te sluiten de **Test** pop-upvenster.

15. In het menu naast **bewerken**, selecteer **publiceren**. Selecteer vervolgens het volgende om te bevestigen, **publiceren** op de pagina.

16. De QnA Maker-service is nu heeft gepubliceerd. U kunt het eindpunt in uw toepassing of bot code kunt gebruiken.

   ![Publiceren](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase maken](../How-To/create-knowledge-base.md)

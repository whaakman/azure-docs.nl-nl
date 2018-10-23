---
title: 'Snelstart: Een KB maken - QnA Maker'
titleSuffix: Azure Cognitive Services
description: U kunt een QnA Maker-knowledge base (KB) maken van uw eigen inhoud, zoals veelgestelde vragen of producthandleidingen. De QnA Maker-KB in dit voorbeeld is gemaakt op basis van een eenvoudige webpagina met veelgestelde vragen waarop vragen worden beantwoord over het herstel van BitLocker-sleutels.
author: diberry
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: quickstart
ms.date: 09/12/2018
ms.author: diberry
ms.openlocfilehash: 85b4630492d5f5b3fb71d563b0947d8a72ae4e1d
ms.sourcegitcommit: 55952b90dc3935a8ea8baeaae9692dbb9bedb47f
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/09/2018
ms.locfileid: "48886530"
---
# <a name="quickstart-create-train-and-publish-your-knowledge-base"></a>Snelstart: Een knowledge base maken, trainen en publiceren

U kunt een QnA Maker-knowledge base (KB) maken van uw eigen inhoud, zoals veelgestelde vragen of producthandleidingen. De QnA Maker-KB in dit voorbeeld is gemaakt op basis van een eenvoudige webpagina met veelgestelde vragen waarop vragen worden beantwoord over het herstel van BitLocker-sleutels.

## <a name="prerequisite"></a>Vereiste

> [!div class="checklist"]
> * Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="create-a-qna-maker-knowledge-base"></a>Een QnA Maker-knowledge base maken

1. Meld u met uw Azure-referenties aan bij QnAMaker.ai.

2. Selecteer op de website van QnA Maker de optie **Een knowledge base maken**.

   ![Nieuwe KB maken](../media/qna-maker-create-kb.png)

3. Selecteer op de pagina **Create** (Maken) in stap 1 de optie **Create a QnA service** (Een QnA-service maken). U wordt omgeleid naar de [Azure Portal](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesQnAMaker) om een QnA Maker-service in te stellen in uw abonnement. Als er een time-out optreedt in de Azure Portal, selecteert u **Opnieuw proberen** op de site. Wanneer de verbinding is gemaakt, wordt uw Azure-dashboard weergegeven.

4. Nadat u een nieuwe QnA Maker-service hebt gemaakt in Azure, gaat u terug naar qnamaker.ai/create. Selecteer de QnA-service in de vervolgkeuzelijsten in stap 2. Als u een nieuwe QnA-service hebt gemaakt, moet u de pagina vernieuwen.

   ![Een QnA-service-KB selecteren](../media/qnamaker-quickstart-kb/qnaservice-selection.png)

5. Noem uw KB in stap 3**Mijn voorbeeld-QnA-KB**.

6. Selecteert drie soorten gegevensbronnen om inhoud toe te voegen aan de KB. Voeg in stap 4, onder **Populate your KB** (De KB vullen), de URL [Veelgestelde vragen over BitLocker-herstel](https://docs.microsoft.com/en-us/windows/security/information-protection/bitlocker/bitlocker-overview-and-requirements-faq) toe in het vak **URL**.

   ![Een QnA-service-KB selecteren](../media/qnamaker-quickstart-kb/add-datasources.png)

7. Selecteer in stap 5 **Create your KB** (De KB maken).

8. Terwijl de KB wordt gemaakt, wordt er een pop-upvenster weergegeven. Tijdens het extractieproces, dat enkele minuten duurt, wordt de HTML-pagina gelezen en worden de vragen en antwoorden geïdentificeerd.

9. Wanneer de KB is gemaakt, wordt de pagina **Knowledge base** geopend. U kunt de inhoud van de KB op deze pagina bewerken.

10. Selecteer in de rechterbovenhoek **Add QnA pair** (QnA-set toevoegen) om een nieuwe rij toe te voegen in de sectie **Editorial** (Redactioneel) van de KB. Typ onder **Question** (Vraag) **Hoi.** Typ onder **Answer** (Antwoord) **Hallo. Wat wil je weten over Bitlocker?**

   ![Een QnA-set toevoegen](../media/qnamaker-quickstart-kb/add-qna-pair.png)

11. Selecteer in de rechterbovenhoek **Save and train** (Opslaan en trainen) om de wijzigingen op te slaan en het QnA Maker-model te trainen. Bewerkingen worden alleen bewaard als ze worden opgeslagen.

   ![Opslaan en trainen](../media/qnamaker-quickstart-kb/add-qna-pair2.png)

12. Selecteer in de rechterbovenhoek **Test** (Testen) om te testen of de wijzigingen worden toegepast. Typ **Hoi** in het vak en druk op Enter. Als het goed is, ziet u nu het antwoord dat u hebt gemaakt.

13. Selecteer **Inspect** (Inspecteren) om het antwoord gedetailleerder te onderzoeken. Het testvenster wordt gebruikt om uw wijzigingen in de KB te testen voordat ze worden gepubliceerd.

   ![Testvenster](../media/qnamaker-quickstart-kb/inspect-panel.png)

14. Selecteer **Test** (Testen) nogmaals om de pop-up **Test** te sluiten.

15. Selecteer in het menu naast **Edit** (Bewerken) de optie **Publish** (Publiceren). Selecteer vervolgens **Publish** (Publiceren) op de pagina om te bevestigen.

16. De QnA Maker-service is nu gepubliceerd. U kunt het eindpunt in uw toepassing of botcode gebruiken.

   ![Publiceren](../media/qnamaker-quickstart-kb/publish-sucess.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Een kennisdatabase maken](../How-To/create-knowledge-base.md)

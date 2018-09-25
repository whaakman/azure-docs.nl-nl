---
title: Een kennisdatabase - QnA Maker bewerken
titleSuffix: Azure Cognitive Services
description: QnA Maker kunt u de inhoud van uw knowledge base beheren door op te geven van een bewerkingservaring eenvoudig te gebruiken.
services: cognitive-services
author: tulasim88
manager: cgronlun
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: ca35e13c502efadad07ef78077271923951ae4b1
ms.sourcegitcommit: 4ecc62198f299fc215c49e38bca81f7eb62cdef3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47038218"
---
# <a name="edit-a-knowledge-base"></a>Een knowledge base bewerken

QnA Maker kunt u de inhoud van uw knowledge base beheren door op te geven van een bewerkingservaring eenvoudig te gebruiken.

## <a name="edit-your-knowledge-base-content"></a>Uw knowledge base-inhoud bewerken

1.  Selecteer **mijn knowledge bases** in de bovenste navigatiebalk. 

    Ziet u alle services die u hebt gemaakt of gedeeld met u gesorteerd in aflopende volgorde van de **het laatst is gewijzigd** datum.

    ![Mijn Knowledge Bases](../media/qnamaker-how-to-edit-kb/my-kbs.png)

2. Selecteer een bepaalde kennisdatabase wijzigingen aanbrengen.

3. Als u klaar bent met het aanbrengen van wijzigingen in de Knowledge base, klikt u op **opslaan en trainen** in de rechterbovenhoek van de pagina om de wijzigingen te behouden.    

    ![Opslaan en trainen](../media/qnamaker-how-to-edit-kb/save-and-train.png)

    >[!NOTE]
    Verlaat de pagina voordat u op bij het opslaan en train, blijven de wijzigingen niet behouden.

## <a name="add-a-qna-pair"></a>Een set QnA toevoegen

Selecteer **QnA toevoegen paar** naar een nieuwe rij toevoegt aan de tabel knowledge base.

![QnA paar toevoegen](../media/qnamaker-how-to-edit-kb/add-qnapair.png)

## <a name="delete-a-qna-pair"></a>Een paar QnA verwijderen

Als u wilt een QnA verwijderen, klikt u op de **verwijderen** pictogram aan de rechterkant van de QnA-rij.

![QnA paar verwijderen](../media/qnamaker-how-to-edit-kb/delete-qnapair.png)

## <a name="add-alternate-questions"></a>Alternatieve vragen toevoegen

Alternatieve vragen toevoegen aan een bestaande QnA-paar voor het verbeteren van de kans op een overeenkomst aan een gebruikersquery.

![Alternatieve vragen toevoegen](../media/qnamaker-how-to-edit-kb/add-alternate-question.png)

## <a name="add-metadata"></a>metagegevens toevoegen


Metagegevens paren toevoegen door het filterpictogram selecteren

![Metagegevens toevoegen](../media/qnamaker-how-to-edit-kb/add-metadata.png)

> [!TIP]
> Zorg ervoor dat u regelmatig opslaan en de kennisdatabase trainen na het aanbrengen van wijzigingen om te voorkomen dat wijzigingen verloren gaan.

## <a name="manage-large-knowledge-bases"></a>Beheren van grote knowledge bases

1. De vragen en antwoorden supereenvoudig zijn **gegroepeerd** door de gegevensbron waaruit deze zijn geëxtraheerd. U kunt uitvouwen of samenvouwen van de gegevensbron.
2. U kunt **zoeken** de knowledge base door te typen in het tekstvak aan de bovenkant van de Knowledge Base-tabel. Klik op enter om te zoeken op de vraag, antwoord of de metagegevens van inhoud. Klik op het pictogram X om de search-filter te verwijderen.
3. **Paginering** kunt u voor het beheren van grote knowledge bases

    ![Zoeken, pagineren, groep](../media/qnamaker-how-to-edit-kb/search-paginate-group.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Samenwerken aan een kennisdatabase](./collaborate-knowledge-base.md)

---
title: Veelgestelde vragen over - cognitieve Services van Microsoft | Microsoft Docs
titleSuffix: Azure
description: Veelgestelde vragen
services: cognitive-services
author: nstulasi
manager: sangitap
ms.service: cognitive-services
ms.component: QnAMaker
ms.topic: article
ms.date: 04/21/2018
ms.author: saneppal
ms.openlocfilehash: a6bf32549715d0357771b3f3b0ff72f64788ec20
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35345307"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>Waarom wordt mijn URL('s) / vragen antwoorden paren is niet uitpakken van bestanden?

Het is mogelijk dat QnA Maker kan niet automatisch-extract vraag en antwoord (QnA) inhoud van geldige Veelgestelde vragen over de URL's. In dergelijke gevallen kunt u de inhoud QnA plakken in een txt-bestand en ziet als het hulpprogramma, het opnemen kan. U kunt ook redactioneel inhoud toevoegen aan uw knowledge base.

## <a name="how-large-a-knowledge-base-can-i-create"></a>Hoe groot mag de Knowledge Base zijn die ik maak?

De grootte van de knowledge base, is afhankelijk van de SKU van Azure search die u bij het maken van de service QnA Maker kiezen. Lees [hier](./Tutorials/choosing-capacity-qnamaker-deployment.md) voor meer informatie.

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>Waarom kan ik niet zien in de vervolgkeuzelijst voor niets wanneer ik probeer te maken van een nieuwe knowledge base?

U nog niet hebt nog QnA Maker-services in Azure gemaakt. Lees [hier](./How-To/set-up-qnamaker-service-azure.md) hoe u dat doet.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>Hoe kan ik een kennisdatabase met andere delen

Delen werkt op het niveau van een service QnA Maker, worden dat wil zeggen alle kennis basissen in de services gedeeld. Lees [hier](./How-To/collaborate-knowledge-base.md) hoe samenwerken aan een knowledge base.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Hoe kan ik het standaardbericht wijzigen wanneer er geen goede overeenkomst wordt gevonden?

Het standaardbericht is onderdeel van de instellingen in App service.
- Ga naar de uw App service-resource in de Azure-portal

![qnamaker appservice](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Klik op de **instellingen** optie

![qnamaker appservice-instellingen](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Wijzig de waarde van de **DefaultAnswer** instelling
- Uw App-service opnieuw starten

![qnamaker appservice opnieuw opstarten](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Waarom wordt Mijn SharePoint-koppeling niet ophalen van opgehaald?

Het hulpprogramma alleen openbare URL's parseert en biedt geen ondersteuning voor geverifieerde gegevensbronnen op dit moment. U kunt ook de optie-bestand uploaden en downloaden van het bestand uitpakken vragen en antwoorden.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>De updates die ik mijn kennisdatabase aangebracht worden niet doorgevoerd op publiceren. Waarom niet?

Elke bewerking moet in een tabel-update-, test- of -instellingen, worden opgeslagen voordat deze kan worden gepubliceerd. Moet u op het opslaan en trainen knop na elke bewerking.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>Wanneer moet ik mijn endpoint-sleutels vernieuwen?

Als u vermoedt dat ermee is geknoeid, moet u uw sleutels eindpunt vernieuwen.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Ondersteunt het knowledge base-ondersteuning voor uitgebreide gegevens of multimedia?

De Knowledge Base ondersteunt Markdown. De automatische-extractie van URL's is echter HTML-Markdown-conversie functionaliteit beperkt. Als u gebruiken Markdown met een volwaardig wilt, kunt u uw inhoud rechtstreeks in de tabel wijzigen of een kennisdatabase met de uitgebreide inhoud uploaden.

Multimedia, zoals afbeeldingen en video's, wordt niet ondersteund op dit moment.

## <a name="does-qna-maker-support-non-english-languages"></a>QnA Maker biedt ondersteuning voor niet-Engelse talen?

Meer details weergeven over [ondersteunde talen](./Overview/languages-supported.md).

Als u de inhoud van meerdere talen hebt, moet u een afzonderlijke service maken voor elke taal.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Moet ik Bot Framework gebruiken om QnA Maker te kunnen gebruiken?

Nee, u niet wilt gebruiken van het Framework Bot met QnA Maker. Echter wordt QnA Maker aangeboden als een van de verschillende sjablonen in Azure Bot-Service. Bot-Service biedt snelle intelligent bot ontwikkeling via Microsoft Bot Framework en deze wordt uitgevoerd in een server minder omgeving.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>Hoe kan ik een bot met QnA Maker maken?

Volg de instructies in [dit](./Tutorials/create-qna-bot.md) documentatie voor het maken van uw Bot met Azure Bot.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Hoe sluit ik de QnA Maker-service in op mijn website?

Volg deze stappen voor de service QnA Maker als webchat besturingselement insluiten in uw website:

1. De veelgestelde vragen over bot maken door de instructies volgen [hier](./Tutorials/create-qna-bot.md).
2. De chatten inschakelen door de stappen te volgen [hier](https://docs.microsoft.com/en-us/azure/bot-service/bot-service-channel-connect-webchat)



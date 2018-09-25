---
title: Veelgestelde vragen - QnA Maker
titleSuffix: Azure Cognitive Services
description: Lijst met veelgestelde vragen over QnA Maker-service
services: cognitive-services
author: tulasim88
manager: cjgronlund
ms.service: cognitive-services
ms.component: qna-maker
ms.topic: article
ms.date: 09/12/2018
ms.author: tulasim
ms.openlocfilehash: be1621bb461d491d4304474034a39a83c5293570
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46972632"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

## <a name="why-is-my-urlsfiles-is-not-extracting-question-answer-pairs"></a>Waarom wordt mijn URL('s) / paren met vraag-antwoord is niet uitpakken van bestanden?

Het is mogelijk dat QnA Maker kan niet automatisch-extract vraag en antwoord (QnA) inhoud van geldige Veelgestelde vragen over URL's. In dergelijke gevallen kunt u de QnA-inhoud plakken in een txt-bestand en ziet als het hulpprogramma kan voorverwerkingsstappen toe. U kunt ook redactioneel inhoud toevoegen aan uw knowledge base.

## <a name="how-large-a-knowledge-base-can-i-create"></a>Hoe groot mag de Knowledge Base zijn die ik maak?

De grootte van de knowledge base, is afhankelijk van de SKU van Azure search die u bij het maken van de QnA Maker-service. Lezen [hier](./Tutorials/choosing-capacity-qnamaker-deployment.md) voor meer informatie.

## <a name="why-do-i-not-see-anything-in-the-drop-down-for-when-i-try-to-create-a-new-knowledge-base"></a>Waarom zie ik geen alles in de vervolgkeuzelijst voor wanneer ik wil een nieuw kennisdatabase maken?

U kunt de QnA Maker-services nog niet hebt nog gemaakt in Azure. Lezen [hier](./How-To/set-up-qnamaker-service-azure.md) hoe u dat doet.

## <a name="how-do-i-share-a-knowledge-base-with-other"></a>Hoe moet ik een kennisdatabase delen met andere?

Delen werkt op het niveau van een QnA Maker-service, worden dat wil zeggen alle knowledge bases in de services gedeeld. Lezen [hier](./How-To/collaborate-knowledge-base.md) hoe u kunt samenwerken op een knowledge base.

## <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Hoe kan ik het standaardbericht wijzigen wanneer er geen goede overeenkomst is gevonden?

Het standaardbericht is onderdeel van de instellingen in uw appservice.
- Ga naar de uw App service-resource in Azure portal

![Azure App service qnamaker](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Klik op de **instellingen** optie

![Azure App service-instellingen voor qnamaker](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Wijzig de waarde van de **DefaultAnswer** instelling
- Uw App-service opnieuw starten

![qnamaker appservice-opnieuw opstarten](./media/qnamaker-faq/qnamaker-appservice-restart.png)

## <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Waarom wordt Mijn SharePoint-koppeling niet ophalen van opgehaald?

Het hulpprogramma alleen openbare URL's worden geparseerd en biedt geen ondersteuning voor geverifieerde gegevensbronnen op dit moment. U kunt ook het bestand te downloaden en het uploaden van bestand-optie gebruiken om aan te vragen en antwoorden extraheren.


## <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>De updates die ik heb in mijn knowledge base aangebracht worden niet weergegeven bij publicatie. Waarom niet?

Elke bewerking moet in een tabelupdate-, test- of -instellingen worden opgeslagen voordat deze kan worden gepubliceerd. Zorg dat op het opslaan en trainen knop na elke bewerking.

## <a name="when-should-i-refresh-my-endpoint-keys"></a>Wanneer moet ik mijn endpoint-sleutels vernieuwen?

Als u vermoedt dat ermee is geknoeid, moet u uw eindpunt-sleutels vernieuwen.

## <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Is de opgemaakte ondersteuningsgegevens in knowledge base of multimedia?

De Knowledge Base ondersteunt Markdown. De automatische extractie van URL's is echter HTML naar Markdown converteren functionaliteit beperkt. Als u gebruiken van de volledige Markdown wilt, kunt u uw inhoud rechtstreeks in de tabel wijzigen of een kennisdatabase met de opgemaakte inhoud uploaden.

Multimedia, zoals afbeeldingen en video's, wordt niet ondersteund op dit moment.

## <a name="does-qna-maker-support-non-english-languages"></a>Ondersteunt QnA Maker niet-Engelse talen?

Zie meer informatie over [ondersteunde talen](./Overview/languages-supported.md).

Als u de inhoud van meerdere talen hebt, maakt u een afzonderlijke service voor elke taal worden.

## <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Moet ik Bot Framework gebruiken om QnA Maker te kunnen gebruiken?

Nee, hoeft u niet de Bot Framework gebruiken met QnA Maker. QnA Maker wordt echter aangeboden als een van verschillende sjablonen in Azure Bot Service. Botservice kunt snelle, intelligente bot-ontwikkeling via Microsoft Bot Framework en deze wordt uitgevoerd op een server minder omgeving.

## <a name="how-can-i-create-a-bot-with-qna-maker"></a>Hoe maak ik een bot met QnA Maker?

Volg de instructies in [dit](./Tutorials/create-qna-bot.md) documentatie voor het maken van uw Bot met Azure Bot.

## <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Hoe sluit ik de QnA Maker-service in op mijn website?

Volg deze stappen voor het insluiten van de QnA Maker-service als een besturingselement voor web-chat in uw website:

1. Maken van uw bot Veelgestelde vragen over door de instructies te volgen [hier](./Tutorials/create-qna-bot.md).
2. De web-chat inschakelen met de volgende stappen [hier](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat)



---
title: Veelgestelde vragen - QnA Maker
titleSuffix: Azure Cognitive Services
description: De lijst currated van de meest gestelde vragen over de QnA Maker-service helpt u vaststellen van de service, sneller en met betere resultaten.
services: cognitive-services
author: tulasim88
manager: nitinme
ms.service: cognitive-services
ms.subservice: qna-maker
ms.topic: article
ms.date: 11/27/2018
ms.author: tulasim
ms.custom: seodec18
ms.openlocfilehash: 6399fe3e53a572c8aea6b804c0e82ffed41cab77
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55873951"
---
# <a name="frequently-asked-questions-for-qna-maker"></a>Veelgestelde vragen over QnA Maker

De lijst currated van de meest gestelde vragen over de QnA Maker-service helpt u vaststellen van de service, sneller en met betere resultaten.

## <a name="manage-the-knowledge-base"></a>De knowledge base beheren

### <a name="i-accidentally-deleted-a-part-of-my-qna-maker-what-should-i-do"></a>Ik heb een deel van mijn QnA Maker per ongeluk verwijderd, wat moet ik doen? 

Alle verwijderingen worden permanent, met inbegrip van paren van vragen en antwoorden, bestanden, URL's, aangepaste vragen en antwoorden, knowledge bases of Azure-resources. Zorg ervoor dat u uw knowledge base van exporteert de **instellingen** pagina voordat u verwijdert een deel van uw knowledge base. 

### <a name="why-is-my-urlsfiles-not-extracting-question-answer-pairs"></a>Waarom wordt mijn URL('s) / bestand(en) niet extraheren vraag-antwoord paren?

Het is mogelijk dat QnA Maker kan niet automatisch-extract vraag en antwoord (QnA) inhoud van geldige Veelgestelde vragen over URL's. In dergelijke gevallen kunt u de QnA-inhoud plakken in een txt-bestand en ziet als het hulpprogramma kan voorverwerkingsstappen toe. U kunt ook u redactioneel inhoud kunt toevoegen aan uw knowledge base via de [QnA Maker portal](https://qnamaker.ai).

### <a name="how-large-a-knowledge-base-can-i-create"></a>Hoe groot mag de Knowledge Base zijn die ik maak?

De grootte van de knowledge base, is afhankelijk van de SKU van Azure search die u bij het maken van de QnA Maker-service. Lezen [hier](./Tutorials/choosing-capacity-qnamaker-deployment.md) voor meer informatie.

### <a name="why-cant-i-see-anything-in-the-drop-down-when-i-try-to-create-a-new-knowledge-base"></a>Waarom zie ik niet alles in de vervolgkeuzelijst wanneer ik wil een nieuw kennisdatabase maken?

U kunt de QnA Maker-services nog niet hebt nog gemaakt in Azure. Lezen [hier](./How-To/set-up-qnamaker-service-azure.md) voor meer informatie over hoe u dat doet.

### <a name="how-do-i-share-a-knowledge-base-with-others"></a>Hoe kan ik een kennisdatabase met anderen delen

Delen werkt op het niveau van een QnA Maker-service, dat wil zeggen, wordt alle knowledge bases in de service gedeeld. Lezen [hier](./How-To/collaborate-knowledge-base.md) hoe u kunt samenwerken op een knowledge base.

### <a name="can-you-share-a-knowledge-base-with-a-contributor-that-is-not-in-the-same-aad-tenant-to-modify-a-knowledge-base"></a>Kunt u een kennisdatabase met een bijdrager die zich niet in dezelfde AAD-tenant, om te wijzigen van een kennisdatabase delen? 

Delen is gebaseerd op rollen gebaseerd toegangsbeheer in Azure (RBAC). Als u kunt delen _eventuele_ resource in Azure met een andere gebruiker, kunt u ook QnA Maker te delen.

### <a name="if-you-have-an-app-service-plan-with-5-qnamaker-knowledge-bases-can-you-assign-readwrite-rights-to-5-different-users-so-each-of-them-can-access-only-1-qnamaker-knowledge-base"></a>Als u een App Service-Plan met 5 QnAMaker knowledge bases. Kunt u lezen/schrijven-rechten aan 5 verschillende gebruikers toewijzen zodat elk van deze krijgt slechts 1 QnAMaker-kennisdatabase tot toegang?

U kunt een hele QnAMaker-service, niet voor afzonderlijke knowledge bases delen.

### <a name="how-can-i-change-the-default-message-when-no-good-match-is-found"></a>Hoe kan ik het standaardbericht wijzigen wanneer er geen goede overeenkomst is gevonden?

Het standaardbericht is onderdeel van de instellingen in uw appservice.
- Ga naar uw App service-resource in Azure portal

![Azure App service qnamaker](./media/qnamaker-faq/qnamaker-resource-list-appservice.png)
- Klik op de **instellingen** optie

![Azure App service-instellingen voor qnamaker](./media/qnamaker-faq/qnamaker-appservice-settings.png)
- Wijzig de waarde van de **DefaultAnswer** instelling
- Uw App-service opnieuw starten

![qnamaker appservice-opnieuw opstarten](./media/qnamaker-faq/qnamaker-appservice-restart.png)

### <a name="why-is-my-sharepoint-link-not-getting-extracted"></a>Waarom wordt Mijn SharePoint-koppeling niet ophalen van opgehaald?

Zie [gegevensbron locaties](./Concepts/data-sources-supported.md#data-source-locations) voor meer informatie.

### <a name="the-updates-that-i-made-to-my-knowledge-base-are-not-reflected-on-publish-why-not"></a>De updates die ik heb in mijn knowledge base aangebracht worden niet weergegeven bij publicatie. Waarom niet?

Elke bewerking moet in tabelupdate, test, of instellen, worden opgeslagen voordat deze kan worden gepubliceerd. Klik op de **opslaan en trainen** knop na elke bewerking.

### <a name="does-the-knowledge-base-support-rich-data-or-multimedia"></a>Is de opgemaakte ondersteuningsgegevens in knowledge base of multimedia?

De Knowledge Base ondersteunt Markdown. De automatische extractie van URL's is echter HTML naar Markdown converteren functionaliteit beperkt. Als u gebruiken van de volledige Markdown wilt, kunt u uw inhoud rechtstreeks in de tabel wijzigen of een kennisdatabase met de opgemaakte inhoud uploaden.

Multimedia, zoals afbeeldingen en video's, wordt niet ondersteund op dit moment.

### <a name="does-qna-maker-support-non-english-languages"></a>Ondersteunt QnA Maker niet-Engelse talen?

Zie meer informatie over [ondersteunde talen](./Overview/languages-supported.md).

Als u de inhoud van meerdere talen hebt, maakt u een afzonderlijke service voor elke taal worden.

## <a name="manage-service"></a>Services beheren

### <a name="when-should-i-restart-my-app-service"></a>Wanneer moet ik mijn appservice opnieuw starten? 

Uw appservice vernieuwen wanneer het waarschuwingspictogram naast de versiewaarde van de knowledge base in de **eindpunt sleutels** tabel op de **gebruikersinstellingen** [pagina](https://www.qnamaker.ai/UserSettings).

### <a name="when-should-i-refresh-my-endpoint-keys"></a>Wanneer moet ik mijn endpoint-sleutels vernieuwen?

Uw eindpunt-sleutels vernieuwen als u vermoedt dat ermee is geknoeid.

### <a name="can-i-use-the-same-azure-search-resource-for-knowledge-bases-using-multiple-languages"></a>Kan ik de dezelfde Azure Search-resource gebruiken voor knowledge bases met behulp van meerdere talen?

Als u meerdere talen en meerdere knowledge bases, wordt de gebruiker heeft om een QnA Maker-resource voor elke taal te maken. Hiermee maakt u een afzonderlijke Azure search-services per taal. Met een combinatie van andere taal knowledge bases in een enkele Azure search-service, leidt dit gedegradeerde relevantie van resultaten.

### <a name="how-can-i-change-the-name-of-the-azure-search-resource-used-by-qna-maker"></a>Hoe kan ik de naam van de Azure Search-resource die worden gebruikt door de QnA Maker wijzigen?

De naam van de Azure Search-resource is de naam van de QnA Maker-resource met een willekeurige letters toegevoegd aan het einde. Dit maakt het moeilijk te onderscheiden van meerdere resources zoeken voor QnA Maker. Een afzonderlijke Azure Search-service (vermeld de manier waarop u wilt) maken en verbinden met uw QnA-Service. De stappen zijn vergelijkbaar met de stappen die u doen wilt, tot [upgraden van een Azure Search](How-To/upgrade-qnamaker-service.md#upgrade-azure-search-service).

## <a name="integrate-with-other-services-including-bots"></a>Integreren met andere services zoals Bots

### <a name="do-i-need-to-use-bot-framework-in-order-to-use-qna-maker"></a>Moet ik Bot Framework gebruiken om QnA Maker te kunnen gebruiken?

Nee, hoeft u niet de Bot Framework gebruiken met QnA Maker. QnA Maker wordt echter aangeboden als een van verschillende sjablonen in Azure Bot Service. Botservice kunt snelle, intelligente bot-ontwikkeling via Microsoft Bot Framework en deze wordt uitgevoerd in een omgeving zonder server.

### <a name="how-can-i-create-a-bot-with-qna-maker"></a>Hoe maak ik een bot met QnA Maker?

Volg de instructies in [dit](./Tutorials/create-qna-bot.md) documentatie voor het maken van uw Bot met Azure Bot Service.

### <a name="how-do-i-embed-the-qna-maker-service-in-my-website"></a>Hoe sluit ik de QnA Maker-service in op mijn website?

Volg deze stappen voor het insluiten van de QnA Maker-service als een besturingselement voor web-chat in uw website:

1. Maken van uw bot Veelgestelde vragen over door de instructies te volgen [hier](./Tutorials/create-qna-bot.md).
2. De web-chat inschakelen met de volgende stappen [hier](https://docs.microsoft.com/azure/bot-service/bot-service-channel-connect-webchat)

## <a name="data-storage"></a>Gegevensopslag

### <a name="what-data-is-stored-and-where-is-it-stored"></a>Welke gegevens worden opgeslagen en waar wordt deze opgeslagen? 

Wanneer u uw QnA Maker-service maakt, kunt u een Azure-regio geselecteerd. Uw knowledge bases en logboekbestanden worden opgeslagen in deze regio. 

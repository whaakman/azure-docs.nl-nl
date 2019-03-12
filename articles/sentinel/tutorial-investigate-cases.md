---
title: Aanvragen met Azure Sentinel Preview onderzoeken | Microsoft Docs
description: Gebruik deze zelfstudie voor informatie over het onderzoeken van aanvragen met Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: 7c4994863f3c145c7095bcc12dd69ff02fab8455
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57540226"
---
# <a name="tutorial-investigate-cases-with-azure-sentinel-preview"></a>Zelfstudie: Aanvragen met Azure Sentinel Preview onderzoeken

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze zelfstudie helpt u bedreigingen in een met Azure Sentinel.

Nadat u [uw gegevensbronnen verbonden](quickstart-onboard.md) Azure Sentinel, u wilt worden gewaarschuwd wanneer er iets verdachte gebeurt. Zodat u kunt dit doen Sentinel van Azure kunt u geavanceerde regels voor waarschuwingen, die aanvragen die u kunt genereren en gebruiken voor het onderzoeken van diep afwijkingen en bedreigingen in uw omgeving. 

> [!div class="checklist"]
> * Aanvragen maken
> * Cases onderzoeken
> * Op bedreigingen reageren

## <a name="investigate-cases"></a>Cases onderzoeken

Een aanvraag kan meerdere waarschuwingen bevatten. Het is een samenloop van alle relevante gegevens voor een specifiek onderzoek. Een aanvraag is gemaakt op basis van waarschuwingen die u hebt gedefinieerd in de **Analytics** pagina. De eigenschappen die betrekking hebben op de waarschuwingen, zoals ernst en status worden ingesteld op het niveau van de aanvraag. Nadat u Azure Sentinel weten welke soorten bedreigingen die u zoekt en u deze kunt vinden laat, kunt u bedreigingen die zijn gedetecteerd door het onderzoeken van gevallen bewaken. 

1. Selecteer **gevallen**. De **gevallen** pagina laat u weten hoe vaak u hebt, hoeveel zijn geopend, hoeveel u hebt ingesteld op **Bezig**, en hoeveel worden gesloten. Voor elke aanvraag ziet u de tijd die het is opgetreden en de status van de aanvraag. Bekijk de ernst om te bepalen wat u moet eerst worden verwerkt. In de **gevallen** pagina, klikt u op de **waarschuwingen** tabblad om alle waarschuwingen die zijn gerelateerd aan een aanvraag te zien. Entiteiten die u hebt toegewezen eerder als onderdeel van de aanvraag kan worden weergegeven in de **entiteiten** tabblad.  U kunt de gevallen kunt filteren, indien nodig, bijvoorbeeld op status of ernst. Wanneer u kijkt de **gevallen** tabblad ziet u openstaande aanvragen die waarschuwingen geactiveerd door uw detectieregels die is gedefinieerd bevatten in **Analytics**. Aan de bovenzijde ziet u uw actieve aanvragen, nieuwe aanvragen en in gevallen wordt uitgevoerd. U ziet ook een overzicht van alle aanvragen op basis van ernst.

  ![Een waarschuwingendashboard](./media/tutorial-investigate-cases/cases.png)

2. Als u wilt een onderzoek, klikt u op een specifieke aanvraag. Aan de rechterkant ziet u gedetailleerde informatie voor de aanvraag met inbegrip van de ernst, overzicht van het aantal entiteiten die betrokken zijn (op basis van uw toewijzing). Elk geval heeft een unieke ID. De ernst van de aanvraag wordt bepaald op basis van de meest ernstige waarschuwing die is opgenomen in het geval is.  

1. Als u meer details over de waarschuwingen en de entiteiten in het geval, klik op **volledige details weergeven** in het geval pagina en de relevante tabbladen met samenvattingen van gegevens van de aanvraag bekijken.  Volledige case weergave consolideert alle gegevens in de waarschuwing, de bijbehorende waarschuwingen en entiteiten.

1. In de **waarschuwingen** tabblad, controleert u de zelf - waarschuwing wanneer deze is geactiveerd en hoeveel de drempels is overschreden. Hier ziet u alle relevante informatie over de waarschuwing: de query die de waarschuwing, het aantal resultaten per query en de mogelijkheid om uit te voeren playbooks op de waarschuwingen geactiveerd. Om in te zoomen omlaag nog verder in het geval is, klikt u op het aantal treffers. Hiermee opent u de query die gegenereerd van de resultaten en de resultaten die de waarschuwing in Log Analytics is geactiveerd.

3. In de **entiteiten** tabblad kunt u de entiteiten die u hebt toegewezen zien als onderdeel van de definitie van de waarschuwingsregel. 

4. Als u een aanvraag actief onderzoekt, is een goed idee om de status van een case ingesteld op **Bezig** totdat u deze sluiten. U kunt ook het geval is, sluit waar **gesloten opgelost** is de status voor aanvragen die aangeven dat een incident is verwerkt, terwijl **gesloten gesloten** is de status voor aanvragen die geen handelingen zijn vereist. Uitleg zijn vereist de redenering voor het sluiten van een aanvraag waarin wordt uitgelegd.

5. Gevallen kunnen worden toegewezen aan een specifieke gebruiker. Voor elk afzonderlijk geval kunt u eigenaar van een toewijzen door in te stellen van de aanvraag **eigenaar** veld. Alle gevallen start als niet-toegewezen. U kunt met ingang van de gevallen en filteren op uw naam om te zien van alle gevallen waarvan u eigenaar bent. 

5. Klik op **onderzoeken** de onderzoekskaart en het bereik van de schending met herstelstappen bekijken. 



## <a name="respond-to-threats"></a>Op bedreigingen reageren

Azure Sentinel kunt u twee primaire manieren voor het reageren op bedreigingen met behulp van playbooks. U kunt een playbook worden automatisch uitgevoerd wanneer een waarschuwing wordt geactiveerd, of u handmatig een playbook in reactie op een waarschuwing uitvoeren kunt instellen.

- U kunt instellen dat een playbook worden automatisch uitgevoerd wanneer een waarschuwing wordt geactiveerd wanneer u de playbook configureren. 

- U kunt een playbook uit in de waarschuwing handmatig uitvoeren door te klikken op **playbooks weergeven** en selecteer vervolgens een playbook om uit te voeren.




## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe u aan de slag met behulp van Azure Sentinel gevallen onderzoeken. Ga verder met de zelfstudie voor [het reageren op bedreigingen met behulp van geautomatiseerde playbooks](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Reageren op bedreigingen](tutorial-respond-threats-playbook.md) voor het automatiseren van uw antwoorden op bedreigingen.


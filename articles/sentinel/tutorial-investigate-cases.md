---
title: Incidenten onderzoeken met Azure Sentinel preview | Microsoft Docs
description: Gebruik deze zelf studie om te leren hoe u incidenten met Azure Sentinel kunt onderzoeken.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: a493cd67-dc70-4163-81b8-04a9bc0232ac
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 3/6/2019
ms.author: rkarlin
ms.openlocfilehash: bad3fddd6caf7e6eb455e59280f181c787b95a4e
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780384"
---
# <a name="tutorial-investigate-incidents-with-azure-sentinel-preview"></a>Zelfstudie: Incidenten onderzoeken met Azure Sentinel preview

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Deze zelf studie helpt u bij het detecteren van bedreigingen met Azure Sentinel.

Nadat u [uw gegevens bronnen hebt verbonden](quickstart-onboard.md) met Azure Sentinel, wilt u een melding ontvangen wanneer er iets verdacht is. Hiervoor kunt u met Azure Sentinel geavanceerde waarschuwings regels maken waarmee incidenten worden gegenereerd die u kunt toewijzen en gebruiken om afwijkingen en bedreigingen in uw omgeving nauw keurig te onderzoeken. 

> [!div class="checklist"]
> * Incidenten maken
> * Incidenten onderzoeken
> * Op bedreigingen reageren

## <a name="investigate-incidents"></a>Incidenten onderzoeken

een incident kan meerdere waarschuwingen bevatten. Het is een aggregatie van alle relevante bewijzen voor een specifiek onderzoek. Er wordt een incident gemaakt op basis van waarschuwingen die u hebt gedefinieerd op de **Analytics** -pagina. De eigenschappen die betrekking hebben op de waarschuwingen, zoals ernst en status, worden ingesteld op incident niveau. Nadat u Azure-Sentinel weet wat voor soort bedreigingen u zoekt en hoe u deze kunt vinden, controleert u de bedreigingen die worden gedetecteerd door incidenten te onderzoeken. 

1. Selecteer **incidenten**. Op de pagina incidenten kunt u zien hoeveel incidenten u hebt, hoeveel er zijn geopend, hoeveel er openstaan en hoeveel u hebt ingesteld op **in uitvoering**en hoeveel er zijn gesloten. Voor elk incident ziet u de tijd waarop het probleem is opgetreden en de status van het incident. Bekijk de ernst om te bepalen wat u eerst moet afhandelen. Klik op de pagina incidenten op het tabblad **waarschuwingen** om alle waarschuwingen weer te geven die betrekking hebben op een incident. Entiteiten die u eerder hebt toegewezen als onderdeel van het incident, kunnen worden weer gegeven op het tabblad **entiteiten** .  U kunt de incidenten zo nodig filteren, bijvoorbeeld op basis van de status of de ernst. Wanneer u het tabblad **incidenten** bekijkt, ziet u open incidenten die waarschuwingen bevatten die worden geactiveerd door de detectie regels die zijn gedefinieerd in de **analyse**. Bovenaan ziet u uw actieve incidenten, nieuwe incidenten en incidenten die worden uitgevoerd. U kunt ook een overzicht van al uw incidenten op Ernst bekijken.

   ![Waarschuwings dashboard](./media/tutorial-investigate-cases/cases.png)

2. Als u een onderzoek wilt starten, klikt u op een specifiek incident. Aan de rechter kant kunt u gedetailleerde informatie voor het incident bekijken, inclusief de ernst, samen vatting van het aantal betrokken entiteiten (op basis van uw toewijzing). Elk incident heeft een unieke ID. De ernst van het incident wordt bepaald aan de hand van de ernstigste waarschuwing die is opgenomen in het incident.  

1. Als u meer informatie over de waarschuwingen en entiteiten in het incident wilt weer geven, klikt u op **volledige details weer geven** op de pagina incident en bekijkt u de relevante tabbladen met een overzicht van de informatie over het incident.  Volledige incident weergave consolideert alle bewijzen in de waarschuwing, de bijbehorende waarschuwingen en entiteiten.

1. Bekijk op het tabblad **waarschuwingen** de waarschuwing zelf: wanneer deze werd geactiveerd en de drempel waarden die u hebt ingesteld, overschreden. U kunt alle relevante informatie over de waarschuwing zien: de query die de waarschuwing heeft geactiveerd, het aantal resultaten dat per query wordt geretourneerd en de mogelijkheid om playbooks uit te voeren op de waarschuwingen. Als u nog meer wilt inzoomen op het incident, klikt u op het aantal treffers. Hiermee opent u de query die de resultaten heeft gegenereerd en de resultaten die de waarschuwing hebben geactiveerd in Log Analytics.

3. Op het tabblad **entiteiten** ziet u alle entiteiten die u hebt toegewezen als onderdeel van de definitie van de waarschuwings regel. 

4. Als u een incident actief onderzoekt, is het een goed idee om de status van het incident in te stellen op **in behandeling** tot u het probleem sluit. U kunt ook het incident sluiten, waarbij **gesloten opgelost** de status is van incidenten die aangeven dat een incident is verwerkt, terwijl **gesloten is afgesloten** de status is van incidenten waarvoor geen verwerking is vereist. Uitleg is vereist uitleg over de reden voor het sluiten van een incident.

5. Incidenten kunnen worden toegewezen aan een specifieke gebruiker. U kunt voor elk incident een eigenaar toewijzen door het veld **eigenaar** van het incident in te stellen. Alle incidenten worden gestart als niet-toegewezen. U kunt de incidenten door lopen en filteren op uw naam om alle incidenten te zien waarvan u eigenaar bent. 

5. Klik op **onderzoeken** om de kaart van het onderzoek en het bereik van de schending met herstels tappen weer te geven. 



## <a name="respond-to-threats"></a>Op bedreigingen reageren

Azure Sentinel biedt u twee primaire opties voor het reageren op bedreigingen met behulp van playbooks. U kunt instellen dat een Playbook automatisch wordt uitgevoerd wanneer een waarschuwing wordt geactiveerd, of u kunt hand matig een Playbook uitvoeren als reactie op een waarschuwing.

- U kunt instellen dat een Playbook automatisch wordt uitgevoerd wanneer een waarschuwing wordt geactiveerd wanneer u de Playbook configureert. 

- U kunt hand matig een Playbook uitvoeren vanuit de waarschuwing door te klikken op **Playbooks weer geven** en vervolgens een Playbook te selecteren dat u wilt uitvoeren.




## <a name="next-steps"></a>Volgende stappen
In deze zelf studie hebt u geleerd hoe u aan de slag gaat met het onderzoeken van incidenten met behulp van Azure Sentinel. Ga verder met de zelf studie voor het [reageren op bedreigingen met behulp van automatische playbooks](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Reageer op bedreigingen](tutorial-respond-threats-playbook.md) om uw reacties op bedreigingen te automatiseren.


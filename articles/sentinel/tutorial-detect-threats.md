---
title: Waarschuwingen onderzoeken met Azure Sentinel preview | Microsoft Docs
description: Gebruik deze zelf studie om te leren hoe u waarschuwingen met Azure Sentinel kunt onderzoeken.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: b5fbc5ac-68b2-4024-9c1b-bd3cc41a66d0
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/20/2019
ms.author: rkarlin
ms.openlocfilehash: ad9c752898733286701db2d0f0b1fc40029b7521
ms.sourcegitcommit: c71306fb197b433f7b7d23662d013eaae269dc9c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/22/2019
ms.locfileid: "68370706"
---
# <a name="tutorial-detect-threats-with-azure-sentinel-preview"></a>Zelfstudie: Bedreigingen detecteren met de preview-versie van Azure Sentinel

> [!IMPORTANT]
> Azure Sentinel is momenteel beschikbaar als open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.

Nadat u [uw gegevens bronnen hebt verbonden](quickstart-onboard.md) met Azure Sentinel, wilt u een melding ontvangen wanneer er iets verdacht is. Als u dit wilt doen, kunt u met Azure Sentinel geavanceerde waarschuwings regels maken waarmee aanvragen worden gegenereerd die u kunt toewijzen en gebruiken om afwijkingen en bedreigingen in uw omgeving nauw keurig te onderzoeken. 

Deze zelf studie helpt u bij het detecteren van bedreigingen met Azure Sentinel.
> [!div class="checklist"]
> * Detectie regels maken
> * Bedreigings reacties automatiseren

## <a name="create-detection-rules"></a>Detectie regels maken

Als u cases wilt onderzoeken, moet u eerst detectie regels maken. 

> [!NOTE]
> Waarschuwingen die zijn gegenereerd in azure Sentinel zijn beschikbaar via [Microsoft Graph beveiliging](https://aka.ms/securitygraphdocs). Raadpleeg de [documentatie over Microsoft Graph Security Alerts](https://aka.ms/graphsecurityreferencebetadocs) voor meer informatie en integratie partners.

Detectie regels zijn gebaseerd op de typen bedreigingen en afwijkingen die in uw omgeving kunnen worden verdacht die u direct wilt weten, zodat ze worden opgehaald, onderzocht en opgelost. 

1. Selecteer in de Azure Portal onder Azure Sentinel **Analytics**.

   ![Analyse](./media/tutorial-detect-threats/alert-rules.png)

2. Klik in de bovenste menu balk op **+ toevoegen**.  

   ![Waarschuwingsregel maken](./media/tutorial-detect-threats/create-alert-rule.png)

3. Geef onder **waarschuwings regel maken**een beschrijvende naam op en stel de **Ernst** in als dat nodig is. 

4. Maak de query in Log Analytics en plak deze in het veld **waarschuwings regel instellen** . Hier volgt een voorbeeld query waarmee u wordt gewaarschuwd wanneer een afwijkend aantal resources wordt gemaakt in azure activity.

        AzureActivity
        | where OperationName == "Create or Update Virtual Machine" or OperationName == "Create Deployment"
        | where ActivityStatus == "Succeeded"
        | make-series dcount(ResourceId)  default=0 on EventSubmissionTimestamp in range(ago(7d), now(), 1d) by Caller

   > [!NOTE]
   > De lengte van de query moet tussen 1 en 10000 tekens lang zijn en mag niet ' Search * ' en ' Union * ' bevatten.


5. Gebruik in de sectie **entiteits toewijzing** de velden onder **entiteits type** om de kolommen in uw query toe te wijzen aan entiteits velden die worden herkend door Azure Sentinel. Wijs voor elk veld de relevante kolom toe aan de query die u in Log Analytics hebt gemaakt, naar het juiste entiteits veld. Selecteer de relevante kolom naam onder de **eigenschap**. Elke entiteit bevat meerdere velden, zoals SID, GUID, enzovoort. U kunt de entiteit toewijzen op basis van een van de velden, niet alleen de entiteit op het hoogste niveau.

6. Definieer de voor waarden voor waarschuwings Triggers onder **waarschuwings trigger**. Hiermee definieert u de voor waarden die de waarschuwing activeren. 

7. Stel de **frequentie** in voor hoe vaak de query wordt uitgevoerd, net zo vaak als elke 5 minuten of als een keer per dag. 

8. Stel de **periode** in voor het bepalen van het tijd venster voor het aantal gegevens dat door de query wordt uitgevoerd. Dit kan bijvoorbeeld elk uur over 60 minuten aan gegevens worden uitgevoerd.

9. U kunt ook de **onderdrukking**instellen. Onderdrukking is handig als u wilt voor komen dat dubbele waarschuwingen worden geactiveerd voor hetzelfde incident. Op deze manier kunt u stoppen met het activeren van waarschuwingen tijdens een bepaalde periode. Dit kan u helpen dubbele waarschuwingen voor hetzelfde incident te voor komen en u in staat te stellen opeenvolgende waarschuwingen voor een bepaalde tijd te onderdrukken. Als de **frequentie** van de **waarschuwings planning** bijvoorbeeld is ingesteld op 60 minuten en de **plannings periode** van de waarschuwing is ingesteld op twee uur, en de query resultaten de gedefinieerde drempel overschrijden, wordt een waarschuwing twee keer geactiveerd, eenmaal wanneer deze voor het eerst wordt gedetecteerd in de afgelopen 60 minuten en opnieuw wanneer deze zich in de eerste 60 minuten van de gegevens van 2 uur van de bemonsterde periode bevindt. Als er een waarschuwing wordt geactiveerd, moet de onderdrukking worden uitgevoerd voor de hoeveelheid tijd die in de waarschuwings periode is ingesteld. In ons voor beeld wilt u mogelijk onderdrukking instellen gedurende 60 minuten, zodat waarschuwingen alleen worden geactiveerd voor gebeurtenissen die zijn opgetreden tijdens het meest recente uur.

8. Nadat u de query hebt geplakt in het veld **waarschuwings regel instellen** , kunt u direct een simulatie van de waarschuwing bekijken onder **Logic alert simulatie** zodat u kunt zien hoeveel gegevens er worden gegenereerd over een bepaald tijds interval voor de waarschuwing u hebt gemaakt. Dit is afhankelijk van wat u hebt ingesteld voor **frequentie** en **drempel waarde**. Als u ziet dat uw waarschuwing gemiddeld te vaak wordt geactiveerd, moet u het aantal resultaten hoger instellen, zodat het boven uw gemiddelde basis lijn komt.

9. Klik op **maken** om uw waarschuwings regel te initialiseren. Nadat de waarschuwing is gemaakt, wordt er een aanvraag gemaakt die de waarschuwing bevat. U kunt de gedefinieerde detectie regels als rijen zien op het tabblad **Security Analytics** . U kunt ook het aantal overeenkomsten voor elke regel bekijken. de waarschuwingen worden geactiveerd. In deze lijst kunt u elke regel inschakelen, uitschakelen of verwijderen. U kunt ook met de rechter muisknop het weglatings teken (...) aan het einde van de rij selecteren om elke waarschuwing te bewerken, uit te scha kelen, te klonen, overeenkomsten weer te geven of een regel te verwijderen. De **Analytics** -pagina is een galerie met al uw actieve waarschuwings regels, waaronder sjablonen die u hebt ingeschakeld en waarschuwings regels die u maakt op basis van sjablonen.

1. De resultaten van de waarschuwings regels kunnen worden weer gegeven op  de pagina cases, waar u de bedreigingen kunt sorteren, [onderzoeken](tutorial-investigate-cases.md)en oplossen.



## <a name="automate-threat-responses"></a>Bedreigings reacties automatiseren

SIEM/SOC-teams kunnen regel matig worden overspoeld met beveiligings waarschuwingen. Het gegenereerde volume van waarschuwingen is zo zeer groot dat beschik bare beveiligings beheerders worden overspoeld. Dit resulteert al te vaak in situaties waarin veel waarschuwingen niet kunnen worden onderzocht, waardoor de organisatie kwetsbaar is voor aanvallen die niet meer worden opgemerkt. 

Veel, indien niet de meeste, van deze waarschuwingen voldoen aan terugkerende patronen die kunnen worden aangepakt door specifieke en gedefinieerde herstel acties. Met Azure Sentinel kunt u al uw herbemiddeling definiëren in playbooks. Het is ook mogelijk om realtime-automatisering in te stellen als onderdeel van de Playbook-definitie zodat u een bepaald antwoord op bepaalde beveiligings waarschuwingen volledig kunt automatiseren. Met behulp van realtime-automatisering kunnen antwoord teams hun werk belasting aanzienlijk verminderen door de routine reacties op terugkerende waarschuwingen volledig te automatiseren, waardoor u meer kunt concentreren op unieke waarschuwingen, patronen kunt analyseren, de jacht kunt automatiseren en meer.

Voor het automatiseren van antwoorden:

1. Kies de waarschuwing waarvoor u het antwoord wilt automatiseren.
1. Selecteer in het navigatie menu van de Azure-Sentinel-werk ruimte **Analytics**.
1. Selecteer de waarschuwing die u wilt automatiseren. 
1. Kies op de pagina **waarschuwings regel bewerken** onder **realtime automatisering**de **geactiveerde Playbook** die u wilt uitvoeren wanneer deze waarschuwings regel overeenkomt.
1. Selecteer **Opslaan**.

   ![realtime-automatisering](./media/tutorial-detect-threats/rt-configuration.png)


Daarnaast kunt u een waarschuwing hand matig herstellen door vanuit de waarschuwing een Playbook uit te voeren, door te klikken op **Playbooks weer geven** en vervolgens een Playbook te selecteren dat u wilt uitvoeren. Zie [werken met playbooks in azure Sentinel](tutorial-respond-threats-playbook.md)voor meer informatie over het maken van een nieuwe Playbook of het bewerken van een bestaande.



## <a name="next-steps"></a>Volgende stappen
In deze zelf studie hebt u geleerd hoe u aan de slag gaat met het detecteren van bedreigingen met behulp van Azure Sentinel. 

Voor meer informatie over hoe u uw reacties op bedreigingen kunt automatiseren, kunt u [reageren op bedreigingen met behulp van automatische playbooks](tutorial-respond-threats-playbook.md).
> [!div class="nextstepaction"]
> [Reageer op bedreigingen](tutorial-respond-threats-playbook.md) om uw reacties op bedreigingen te automatiseren.


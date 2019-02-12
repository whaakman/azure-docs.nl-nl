---
title: Verwerken van Azure-toepassing controleren feedback - Azure Marketplace | Microsoft Docs
description: Wordt uitgelegd hoe u Azure DevOps voor het afhandelen van revisie feedback voor Azure-toepassing biedt voor de Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: v-miclar
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 02/05/2019
ms.author: pbutlerm
ms.openlocfilehash: 0d3102a269d41fded112b0704c28ca2cf5df71a6
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56101149"
---
# <a name="handling-review-feedback"></a>Afhandeling van revisie feedback

In dit artikel wordt uitgelegd hoe u toegang tot het Azure DevOps-omgeving gebruikt door het team van Microsoft Azure Marketplace controleren.  Als er kritieke problemen worden gevonden in de aanbieding van uw Azure-toepassing tijdens de **Microsoft revisie** stap, kunt u zich in het systeem om gedetailleerde informatie over deze problemen (feedback bekijken) weer te geven.  Nadat u alle deze problemen oplossen, moet u uw aanbieding om door te gaan publiceren op Azure Marketplace naar opnieuw indienen.  Het volgende diagram illustreert hoe dit Feedbackproces is gekoppeld aan het publicatieproces.

![Publiceren stappen met VSTS feedback](./media/pub-flow-vsts-access.png)

Beoordeling problemen wordt meestal verwezen als pull-aanvraag (PR).  Elke pull-aanvraag is gekoppeld aan een online [Azure DevOps](https://azure.microsoft.com/services/devops/) (voorheen Visual Studio Team Services (VSTS) genoemd) artikel informatie over het probleem bevat.  De volgende afbeelding geeft een voorbeeld van een verwijzing naar een beoordeling pull-aanvraag.  Voor complexe situaties kunnen de controle en ondersteuning voor teams ook u e-mail. 

![Status van tabblad weergeven Lees feedback](./media/status-tab-ms-review.png)


## <a name="vsts-access"></a>VSTS-toegang

Als u wilt weergeven van de pull-aanvraag-items waarnaar wordt verwezen in lees feedback, moeten uitgevers eerst worden verleend juiste autorisatie.  Anders, nieuwe uitgevers ontvangen een `401 - Not Authorized` antwoordpagina bij het weergeven van pull-aanvragen.  Voor het aanvragen van toegang tot deze Azure DevOps-opslagplaats, moet u de volgende stappen uitvoeren:

1. De volgende informatie verzamelen:
    - De naam van de uitgever en -ID
    - Aanbiedingstype (Azure-app), bieden een naam en SKU-ID
    - De pull-aanvraag koppelen, bijvoorbeeld: `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/contoso/pullrequest/<number>`  Deze URL kan worden opgehaald uit de melding of het adres van de pagina 401-respons.
    - De e-mailadressen van personen van de uitgever die u toegang verleend wilt aan.  Deze lijst moet de eigenaar van adressen die u hebt opgegeven bij het registreren als een publisher op de Cloud Partner-Portal opnemen.
2. Maak een ondersteuningsincident.  Selecteer in de titelbalk van de Cloud Partner-Portal, de **Help** knop, en kies vervolgens **ondersteuning** in het menu.  U standaard web browser moet starten en navigeer naar de Microsoft nieuwe incidenten ondersteuningspagina.  (U hebt mogelijk eerst aanmelden.)
3. Geef de **probleemtype** als **marketplace onboarding** en **categorie** als **probleem met toegang tot**en selecteer vervolgens **starten aanvraag**.

    ![Ondersteuning voor ticket categorie](./media/support-incident1.png)

4. In **stap 1 van 2** pagina, Geef uw contactgegevens en selecteer **doorgaan**.
5. In **stap 2 van 2** pagina, de titel van een incident opgeven (bijvoorbeeld `Request VSTS access`) en geef de gegevens die u hebt verzameld in de eerste stap (hierboven).  Lees en accepteer de overeenkomst, en selecteer vervolgens **indienen**.

Als het incident maken voltooid is, wordt een bevestigingspagina weergegeven.  De Bevestigingsgegevens opslaan op deze pagina ter referentie.  Het ondersteuningsteam van Microsoft moet binnen een paar dagen antwoord op uw aanvraag voor toegang.


## <a name="reviewing-the-pull-request"></a>De pull-aanvraag beoordelen 

Gebruik de volgende procedure om problemen die worden beschreven in de pull-aanvraag te bekijken.

1. In de **Microsoft revisie** sectie van **publiceren stappen** vormen, klikt u op een koppeling pull-aanvraag naar Start uw browser en navigeer naar de **overzicht** (homepage) voor dit pull-aanvraag.  De volgende afbeelding ziet u een voorbeeld van kritiek probleem-startpagina voor de Contoso-voorbeeld-app-aanbieding.  Deze pagina bevat nuttige overzichtsinformatie over de beoordeling-problemen gevonden in de Azure-app.  

    [ ![Startpagina voor pull-aanvraag](./media/pr-home-page-thumb.png) ](./media/pr-home-page.png)
    <br/> *Klik op de afbeelding om uit te vouwen.*
    
2. (Optioneel) Aan de rechterkant van het venster in de sectie **beleid**, klikt u op het probleem-bericht (in dit voorbeeld: **Beleid voor validatie is mislukt**) voor het onderzoeken van de details op laag niveau van het probleem, met inbegrip van de bijbehorende logboekbestanden.  Fouten worden doorgaans weergegeven aan de onderkant van de logboekbestanden.

3. Selecteer in het menu aan de aan de linkerkant van de startpagina, **bestanden** om de lijst met bestanden die deel uitmaken van de technische activa voor deze aanbieding weer te geven.  De Microsoft-revisoren moeten opmerkingen met een beschrijving van de gedetecteerde kritieke problemen hebt toegevoegd.  In het volgende voorbeeld, zijn twee problemen gedetecteerd. 

    [ ![Startpagina voor pull-aanvraag](./media/pr-files-page-thumb.png) ](./media/pr-files-page.png)
    <br/> *Klik op de afbeelding om uit te vouwen.*

4. Klik op elk knooppunt van de opmerking in het linkerdeelvenster om te navigeren naar de opmerking in de context van de omringende code.  Corrigeer uw broncode in uw teamproject voor het probleem wordt beschreven in de opmerking.

> [!Note]
> U kan niet worden bewerkt technische activa van uw aanbieding binnen de van de beoordelingsteam Azure DevOps-omgeving.  Voor uitgevers is dit een alleen-lezen-omgeving voor de ingesloten broncode.  U kunt echter laten antwoorden op de opmerkingen voor de beoordeling-team van Microsoft.

   In het volgende voorbeeld heeft de uitgever gecontroleerd, gecorrigeerd en heeft gereageerd op het eerste probleem.

   ![Eerste oplossing en de opmerking beantwoord](./media/first-comment-reply.png)


## <a name="next-steps"></a>Volgende stappen

Nadat u de kritieke problemen beschreven in de revisie PR(s) corrigeert, moet u [opnieuw publiceren van uw Azure-app-aanbieding](./cpp-publish-offer.md).

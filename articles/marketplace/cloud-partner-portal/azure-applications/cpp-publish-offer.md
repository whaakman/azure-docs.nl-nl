---
title: Aanbieding voor Azure-toepassing - Azure Marketplace publiceren | Microsoft Docs
description: Beschrijft het proces en de stappen voor het publiceren van een Azure-toepassing-aanbieding op Azure Marketplace.
services: Azure, Marketplace, Cloud Partner Portal,
documentationcenter: ''
author: dan-wesley
manager: Patrick.Butler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: conceptual
ms.date: 01/10/2018
ms.author: pbutlerm
ms.openlocfilehash: cfcb33f9586033db9f4197a341227365d43601cc
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54232716"
---
# <a name="publish-azure-application-offer"></a>Aanbieding voor Azure-toepassing publiceren

Nadat u een aanbieding hebt gemaakt, dankzij de informatie over de **nieuwe aanbieding** pagina, kunt u de aanbieding publiceren. Selecteer **publiceren** het publicatieproces te starten.

Het volgende diagram toont de belangrijkste stappen bij het publicatieproces voor een aanbieding "om live te gaan'.

![Aanbieding publiceren stappen](./media/offer-publishing-steps.png)

## <a name="detailed-description-of-publishing-steps"></a>Gedetailleerde beschrijving van de publicatie van stappen

De volgende tabel geeft een lijst van en beschrijft elke stap publiceren en biedt een geschatte tijd voor het voltooien van elke stap.  Tijden naar schatting in 'days' worden gedefinieerd als werkdagen uitgesloten weekends en feestdagen.

|  **Publiceren stap**           | **tijd**    | **Beschrijving**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Valideer de vereisten         | < 15 minuten    | Biedt informatie en bieden instellingen worden gevalideerd.                        |
| Beïnvloed omzet instellingen valideren | < 15 minuten  |      |
| Certificering                  | < 1 dag     | Aanbieding wordt geanalyseerd door het Team van Azure-certificering. De aanbieding is gescand op virussen, malware, veiligheid, compatibiliteit en beveiligingsproblemen met zich mee. De aanbieding wordt gecontroleerd om te zien dat het voldoet aan alle criteria in de in aanmerking te komen. Zie voor meer informatie, [vereisten](./cpp-prerequisites.md). Feedback wordt gegeven als er een probleem is gevonden. |
| Test Drive niet valideren          | < 2 uur   | (Optioneel) Als een Test Drive aanwezig is, wordt Microsoft valideert dat kan worden gedistribueerd en gerepliceerd.  |
| Pakketten en lead generatie registratie | < 1 uur  | Technische activa van de aanbieding zijn verpakt voor gebruik door de klant en de lead-systemen worden geconfigureerd en geïmplementeerd. |
|  Goedkeuring van uitgever             |  Handmatig    | Laatste uitgever controleren en de bevestiging voordat de aanbieding live meteen. De aanbieding is nu beschikbaar als preview.  U kunt uw aanbieding in de geselecteerde abonnementen (in de aanbieding informatie stappen) implementeren om te controleren of het voldoet aan alle vereisten van uw.  Nadat u hebt gecontroleerd dat de aanbieding, selecteert u **Go Live** , zodat uw aanbieding naar de volgende stap verplaatsen kunt. |
| Controle van Microsoft                | 7 - 14 dagen * | Microsoft zuinigste beoordelingen van uw Azure-toepassing en u een e-mail stuurt als er problemen worden ontdekt.  * Zelden, deze stap duurt mogelijk langer dan 14 dagen, afhankelijk van de complexiteit van de toepassing en de problemen zijn gedetecteerd.  |
| Live                           | < 1 dag | Aanbieding is uitgebracht, gerepliceerd naar de opgegeven regio's en beschikbaar gesteld voor het publiek. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |
 
U kunt het publicatieproces in bewaken de **Status** tabblad voor uw aanbod in de Cloud Partner-Portal.

![Tabblad van de status voor een Azure-app-aanbieding](./media/offer-status-tab.png)

Nadat u klaar bent met het publicatieproces, uw aanbieding wordt weergegeven in de [Microsoft Azure Marketplace-toepassingscategorie](https://azuremarketplace.microsoft.com/marketplace/apps/).


## <a name="errors-and-review-feedback"></a>Fouten en lees feedback

Naast het weergeven van de publicatiestatus van uw aanbieding, de **Status** tabblad geeft ook foutberichten en feedback van de **Microsoft revisie** stap.  Beoordeling problemen wordt meestal verwezen als pull-aanvraag (PR).  Elke pull-aanvraag is gekoppeld aan een online Visual Studio Team Services (VSTS, gewijzigd in [Azure DevOps](https://azure.microsoft.com/services/devops/)) artikel informatie over het probleem bevat.  De volgende afbeelding geeft een voorbeeld van een verwijzing naar een beoordeling pull-aanvraag.  Voor meer complexe situaties kunnen de controle en ondersteuning voor teams u e-mail. 

![Status van tabblad weergeven Lees feedback](./media/status-tab-ms-review.png)

U kunt elk gerapporteerd probleem moet oplossen voordat de aanbieding tot en met het publicatieproces blijft.  Het volgende diagram illustreert hoe dit Feedbackproces is gekoppeld aan het publicatieproces.

![Publiceren stappen met VSTS feedback](./media/pub-flow-vsts-access.png)


### <a name="vsts-access"></a>VSTS-toegang

Als u wilt weergeven van de VSTS-items waarnaar wordt verwezen in lees feedback, moeten uitgevers juiste machtiging worden verleend.  Anders, nieuwe uitgevers ontvangen een `401 - Not Authorized` antwoordpagina.  Voor het aanvragen van toegang tot het voorstel controleren VSTS-systeem, moet u de volgende stappen uitvoeren:

1. De volgende informatie verzamelen:
    - De naam van de uitgever en -ID
    - Aanbiedingstype (Azure-app), bieden een naam en SKU-ID
    - De pull-aanvraag koppelen, bijvoorbeeld `https://solutiontemplates.visualstudio.com/marketplacesolutions/_git/jfrog/pullrequest/<number>`.  Deze URL kan worden opgehaald uit de melding of het adres van de pagina 401-respons.
    - De e-mailadressen van personen van de uitgever die u toegang verleend wilt aan.  Hierbij moet de eigenaar van adressen die u hebt opgegeven bij het registreren als een publisher op de Cloud Partner-Portal.
2. Maak een ondersteuningsincident.  Selecteer in de titelbalk van de Cloud Partner-Portal, de **Help** knop, en kies vervolgens **ondersteuning** in het menu.  U standaard web browser moet starten en navigeer naar de Microsoft nieuwe incidenten ondersteuningspagina.  (U hebt mogelijk eerst aanmelden.)
3. Geef de **probleemtype** als **marketplace onboarding** en **categorie** als **probleem met toegang tot**en selecteer vervolgens **starten aanvraag**.

    ![Ondersteuning voor ticket categorie](./media/support-incident1.png)

4. In **stap 1 van 2** pagina, Geef uw contactgegevens en selecteer **doorgaan**.
5. In **stap 2 van 2** pagina, geeft u de titel van een incident en geef de gegevens die u hebt verzameld in de eerste stap (hierboven).  Lees en accepteer de overeenkomst, en selecteer vervolgens **indienen**.

Als het incident maken voltooid is, wordt een bevestigingspagina weergegeven.  De Bevestigingsgegevens ter referentie niet opslaan.  Microsoft-ondersteuning moet binnen een paar dagen antwoord op uw aanvraag voor toegang.


## <a name="next-steps"></a>Volgende stappen

Zodra een Azure-app is gepubliceerd, kunt u [bestaande aanbieding bijwerken](./cpp-update-existing-offer.md) in overeenstemming met het wijzigen van zakelijke en technische vereisten. 

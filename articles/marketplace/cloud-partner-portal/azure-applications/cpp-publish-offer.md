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
ms.date: 01/25/2019
ms.author: pbutlerm
ms.openlocfilehash: cafda8a48f9160c80edb02c3452035f912958bc7
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56098218"
---
# <a name="publish-azure-application-offer"></a>Aanbieding voor Azure-toepassing publiceren

Nadat u een aanbieding hebt gemaakt, dankzij de informatie over de **nieuwe aanbieding** pagina, kunt u de aanbieding publiceren. Selecteer **publiceren** het publicatieproces te starten.

Het volgende diagram toont de belangrijkste stappen bij het publicatieproces voor een aanbieding "om live te gaan'.

![Aanbieding publiceren stappen](./media/offer-publishing-steps.png)


## <a name="detailed-description-of-publishing-steps"></a>Gedetailleerde beschrijving van de publicatie van stappen

De volgende tabel geeft een lijst van en beschrijft elke stap publiceren en biedt een geschatte tijd voor het voltooien van elke stap.  Tijden naar schatting in 'days' worden gedefinieerd als werkdagen uitgesloten weekends en feestdagen.

|  **Publiceren stap**           | **tijd**    | **Beschrijving**                                                            |
|  -------------------           | --------    | ---------------                                                            |
| Valideer de vereisten         | < 15 min    | Biedt informatie en bieden instellingen worden gevalideerd.                        |
| Beïnvloed omzet instellingen valideren | < 15 min  | Azure-resource gebruik attribution voor de aanbieding is ingeschakeld.             |
| Certificering                  | < 1 dag     | Aanbieding wordt geanalyseerd door het Team van Azure-certificering. De aanbieding is gescand op virussen, malware, veiligheid, compatibiliteit en beveiligingsproblemen met zich mee. De aanbieding wordt gecontroleerd om te zien dat het voldoet aan alle criteria in de in aanmerking te komen. Zie voor meer informatie, [vereisten](./cpp-prerequisites.md). Feedback wordt gegeven als er een probleem is gevonden. |
| Test Drive niet valideren          | < 2 uur   | (Optioneel) Als een Test Drive aanwezig is, wordt Microsoft valideert dat kan worden gedistribueerd en gerepliceerd.  |
| Pakketten en lead generatie registratie | < 1 uur  | Technische activa van de aanbieding zijn verpakt voor gebruik door de klant en de lead-systemen worden geconfigureerd en geïmplementeerd. |
|  Goedkeuring van uitgever             |  Handmatig    | Laatste uitgever controleren en de bevestiging voordat de aanbieding live meteen. De aanbieding is nu beschikbaar als preview.  U kunt uw aanbieding in de geselecteerde abonnementen (in de aanbieding informatie stappen) implementeren om te controleren of het voldoet aan alle vereisten van uw.  Nadat u hebt gecontroleerd dat de aanbieding, selecteert u **Go Live** , zodat uw aanbieding naar de volgende stap verplaatsen kunt. |
| Controle van Microsoft                | 7 - 14 dagen | Microsoft zuinigste beoordelingen van uw Azure-toepassing en u een e-mail stuurt als er problemen worden ontdekt.  De lengte van deze stap is afhankelijk van de complexiteit van de toepassing, de gevonden problemen en hoe snel u erop kan reageren.  |
| Live                           | < 1 dag | Aanbieding is uitgebracht, gerepliceerd naar de opgegeven regio's en beschikbaar gesteld voor het publiek. |
|   |&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|   |

U kunt het publicatieproces in bewaken de **Status** tabblad voor uw aanbod in de Cloud Partner-Portal.

![Tabblad van de status voor een Azure-app-aanbieding](./media/offer-status-tab.png)

Nadat u klaar bent met het publicatieproces, uw aanbieding wordt weergegeven in de [Microsoft Azure Marketplace-toepassingscategorie](https://azuremarketplace.microsoft.com/marketplace/apps/).


## <a name="errors-and-review-feedback"></a>Fouten en lees feedback

Naast het weergeven van de publicatiestatus van uw aanbieding, de **Status** tabblad geeft ook foutberichten en feedback van eventuele publishing stappen waar een probleem is opgetreden.  Als het probleem is van essentieel belang, is te publiceren geannuleerd.  Vervolgens moet u de gerapporteerde oorzaak van deze corrigeren en de aanbieding publiceren.  Omdat de **Microsoft revisie** stap vertegenwoordigt een uitgebreid overzicht van uw aanbieding en de bijbehorende technische activa (met name de Azure Resource Manager-sjabloon), problemen worden doorgaans weergegeven als pull-aanvraag (PR) koppelingen.  Een uitleg van hoe u kunt bekijken en reageren op deze pull-aanvragen, Zie [verwerking Lees feedback](./cpp-handling-review-feedback.md).


## <a name="next-steps"></a>Volgende stappen

Als u fouten in een of meer van de publicatie stappen optreden, moet u deze corrigeren en uw aanbod opnieuw publiceren.  Als er kritieke problemen zijn opgetreden de **Microsoft revisie** stap, moet u [verwerken van de beoordeling-feedback](./cpp-handling-review-feedback.md) bekijken door het openen van de Microsoft Azure DevOps-opslagplaats van team.

Als een Azure-app is gepubliceerd, kunt u [de bestaande aanbieding bijwerken](./cpp-update-existing-offer.md) in overeenstemming met het wijzigen van zakelijke en technische vereisten. 

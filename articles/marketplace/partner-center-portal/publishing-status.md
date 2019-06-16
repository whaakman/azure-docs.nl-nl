---
title: Controleer de publicatiestatus van uw commerciële Marketplace-aanbieding
description: Controleer de status van de validatie, certificering en Preview-versie stappen vereist voor het publiceren van een aanbieding via de commerciële Marketplace in de Microsoft Partner Center.
author: mattwojo
manager: evansma
ms.author: mattwoj
ms.service: marketplace
ms.topic: conceptual
ms.date: 05/30/2019
ms.openlocfilehash: 9bf3136969974abbe9a99a5632478e3cbb22307e
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66474523"
---
# <a name="check-the-publishing-status-of-your-commercial-marketplace-offer"></a>Controleer de publicatiestatus van uw commerciële Marketplace-aanbieding

U kunt uw huidige weergeven **publicatiestatus** op de **overzicht bieden** tabblad van de [commerciële Marketplace portal](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) in Partner Center.

Een van de volgende statusindicatoren moet worden weergegeven voor elke aanbieding.

| **Status**    | **Beschrijving**  |
| :---------- | :-------------------|
| **Concept** | Aanbieding is gemaakt, maar is niet worden gepubliceerd. |
| **Publiceren wordt uitgevoerd** | Abonnement/werkt eraan door de stappen van het publicatieproces. |
| **Aandacht vereist** | Een belangrijk probleem is gedetecteerd tijdens de certificering door Microsoft of een van de publicatie stappen. |
| **Preview** | Aanbieding is gecertificeerd door Microsoft en nu wacht op de laatste controle door de uitgever. Selecteer gaat u live om te maken van de aanbieding live. |
| **Live** | Aanbieding kan is gepubliceerd in de marketplace en worden bekeken en overgenomen door klanten. |
| **Verkopen in behandeling stoppen** | Uitgever "stop verkopen" geselecteerd op de aanbieding of van plan zijn, maar de actie is nog niet voltooid. |
| **Niet beschikbaar in de marketplace** | Een eerder gepubliceerde aanbieding/plan in de marketplace is verwijderd. |

## <a name="automated-validation"></a>Geautomatiseerde validatie

De eerste stap bij het publicatieproces is een reeks geautomatiseerde controles. Elke validatiestap komt overeen met een functie die u hebt gekozen om in te schakelen bij het maken van uw aanbieding. Als deze functie niet is ingeschakeld, slaat de validatie verder met de volgende stap in de publicatie. Elke validatiecontrole moet worden voltooid voordat de status van de publicatie is goedgekeurd.

- **Aankoop stroom instellingen bieden (< 10 minuten)**

Wij garanderen dat uw aanbieding kan worden voldaan die door klanten via de Azure-portal worden gekocht in deze stap maakt. Deze stap is alleen van toepassing voor die worden verkocht via Microsoft-aanbiedingen.

- **Test drive-gegevensvalidatie (ongeveer 5 min.)**

In deze stap valideren we de gegevens die u hebt opgegeven in de proefrit technische configuratiesectie van de aanbieding. Test drive-functionaliteit is getest en goedgekeurd. Deze stap is alleen van toepassing op aanbiedingen met een test drive is ingeschakeld.

- **Test drive inrichten (~ 30 min)**

In deze stap na de validatie van de gegevens en functionaliteit van uw test drive in de vorige stap, we implementeren en exemplaren van uw test drive repliceren, zodat ze gereed voor gebruik door de klant zijn.  Deze stap is alleen van toepassing op aanbiedingen met een test drive is ingeschakeld.

- **Validatie van beheer en registratie leiden (< 15 minuten)**

In deze stap bevestigen we dat uw beheersysteem potentiële klant leads op basis van de informatie in de installatie van de aanbieding kan ontvangen. Deze stap is alleen van toepassing op aanbiedingen met leadbeheer is ingeschakeld.

## <a name="certification"></a>Certificering

Voordat het werd gepubliceerd, moeten de aanbiedingen die zijn verzonden naar de commerciële Marketplace in de Partner Center worden gecertificeerd. Verzonden aanbiedingen ondergaan uitvoerig getest, sommige geautomatiseerde en andere handmatige, waaronder een controle op basis van de [beleidsregels voor Azure Marketplace-certificering](https://docs.microsoft.com/legal/marketplace/general-policies). Bieden inzendingen moeten zijn gemarkeerd in aanmerking komen voor certificering voordat ze verdergaan naar de volgende stap in de publicatie stroom.

### <a name="types-of-validation-that-take-place-during-certification"></a>Typen van de validatie van die tijdens de certificering plaatsvinden

Er zijn drie niveaus van validatie opgenomen in het certificeringsproces voor elke aanbieding die is verzonden.

- Uitgever bedrijven in aanmerking komt
- Validatie van inhoud
- Technische validatie

#### <a name="publisher-business-eligibility"></a>Uitgever bedrijven in aanmerking komt

Elk aanbiedingtype controleert of een set criteria basis in aanmerking te komen die moet voldoen aan de uitgever. Criteria voor in aanmerking komt, omvat mogelijk MPN-status van de uitgever, competenties die zijn ondergebracht, competentie niveaus, enzovoort.

#### <a name="content-validation"></a>Validatie van inhoud

Tijdens de validatie van inhoud, de informatie hebt ingevoerd tijdens het maken van uw aanbieding gecontroleerd op kwaliteit en de volgorde van relevantie. Deze controles controleert uw vermeldingen voor de marketplace-vermelding details, prijzen, beschikbaarheid, de bijbehorende plannen, enzovoort. Om te voldoen aan de Azure Marketplace en/of de AppSource criteria aanbieding, zullen we dit valideren dat uw aanbieding bevat:

- een titel die nauwkeuriger de aanbieding beschrijft;
- goed geschreven beschrijvingen die een grondige overzicht en een toegevoegde waarde;
- schermafbeeldingen van de kwaliteit en de bijbehorende video's; en
- een uitleg van hoe Microsoft-platforms en hulpprogramma's maakt gebruik van de aanbieding.

Meer informatie over de criteria voor de validatie van inhoud door te lezen die de [algemene beleidsregels aanbieding](https://docs.microsoft.com/legal/marketplace/general-policies#10-general-listing-policies).

#### <a name="technical-validation"></a>Technische validatie

De aanbieding (pakket of binair) ondergaat tijdens het valideren van technische, de volgende controles.
- Gescand op schadelijke software
- Netwerkaanroepen bewaakt
- Pakket is geanalyseerd
- Uitgebreide scannen van de werkelijke functionaliteit van de aanbieding

De aanbieding is getest op verschillende platforms en versies om ervoor te zorgen is krachtig.

Lees de specifieke configuratie-informatie die nodig zijn voor uw aanbod in de sectie met technische configuratie van dit document.

### <a name="certification-failure-report"></a>Rapport over fouten certificering

Na voltooiing van de beoordeling, als uw aanbieding is verstreken certificering vervolgens wordt verplaatst samen met de volgende stap in het publicatieproces. Als uw aanbieding is een van de aanbieding, technische of beleid controles zijn mislukt of als u niet in aanmerking voor het indienen van een aanbieding van dat type, wordt een rapport over certificering fouten gegenereerd en naar u verzonden.

Dit rapport bevat beschrijvingen van elk beleid dat is mislukt, samen met opmerkingen bij de beoordeling. Bekijk dit e-mailrapport, los eventuele problemen met updates aanbrengen in uw aanbieding indien nodig, en verzend opnieuw de aanbieding met behulp van de [commerciële Marketplace portal](https://partner.microsoft.com/dashboard/commercial-marketplace/offers) in Partnercentrum. (U kunt de aanbieding zo vaak als nodig is tot doorgeven certificering verzenden).

## <a name="preview-creation"></a>Maken van de Preview-versie

Tijdens de **Preview maken** stap maken we een versie van uw aanbieding toegankelijk is voor alleen de doelgroep die u hebt opgegeven in de Preview-sectie van uw aanbieding.

## <a name="publisher-approval"></a>Goedkeuring van de uitgever

In deze stap wordt u worden verzonden met een aanvraag voor u om te controleren en goedkeuren van uw aanbieding-voorbeeld vóór de laatste stap van de publicatie.

Als u hebt geselecteerd om te verkopen van uw aanbieding met behulp van Microsoft, kunt u zich voor het testen van de aanschaf en implementatie van uw aanbieding om ervoor te zorgen dat het voldoet aan uw vereisten tijdens deze fase van de goedkeuring Preview-versie. Uw aanbieding wordt nog niet beschikbaar in de symphysis marketplace. Nadat u testen en goedkeuren van deze Preview-versie, moet u selecteren **Go Live** op de [ **overzicht bieden** ](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) dashboard.

Als u wijzigingen aanbrengen in de aanbieding in deze previewfase wilt, kunt u deze kunt bewerken en opnieuw indienen voor het publiceren van een nieuwe Preview-versie. Zie het artikel [Update bestaande marketplace-aanbiedingen](#update-existing-marketplace-offers) voor meer informatie over meer wijzigingen aanbrengen.

Als uw aanbieding al live en beschikbaar zijn voor het publiek in de marketplace, eventuele wijzigingen die u aanbrengt live wordt niet gaan totdat u **Go live** op de [ **overzicht bieden** ](https://partner.microsoft.com/dashboard/commercial-marketplace/overview) dashboard.

### <a name="publish-offer-to-the-public"></a>Aanbieding publiceren in de openbare

Aanmelden bij de Partner Center en toegang tot de aanbieding. U wordt omgeleid naar de **overzicht bieden** pagina. Klik boven aan deze pagina ziet u een optie voor **live gaan**. Selecteer **live, gaan** en nadat is bevestigd dat, de aanbieding wordt gestart met het ophalen van gepubliceerd op het publiek. U ontvangt een e-mailmelding wanneer de aanbieding gepubliceerd is.

## <a name="publish"></a>Publiceren

Nu dat u hebt geselecteerd voor **live gaan** in uw aanbieding, zodat deze beschikbaar wordt in de marketplace, er zijn een reeks laatste validatie wordt gecontroleerd die zullen worden getrapte via om ervoor te zorgen dat de aanbieding voor live net als bij de Preview-versie is geconfigureerd de versie van het product.

- **Aankoop stroom instellingen bieden (> 10 minuten)**

Wij garanderen dat uw aanbieding kan worden voldaan die door klanten via de Azure-portal worden gekocht in deze stap maakt. Deze stap is alleen van toepassing voor die worden verkocht via Microsoft-aanbiedingen.

- **Test drive-gegevensvalidatie (ongeveer 5 min.)**

In deze stap valideren we de gegevens die u hebt opgegeven in de proefrit technische configuratiesectie van de aanbieding. Test drive-functionaliteit is getest en goedgekeurd. Deze stap is alleen van toepassing op aanbiedingen met een test drive is ingeschakeld.

- **Test drive inrichten (~ 30 min)**

In deze stap, we implementeren en exemplaren van uw test drive repliceren, zodat ze gereed voor gebruik door de klant zijn.  Deze stap is alleen van toepassing op aanbiedingen met een test drive is ingeschakeld.

- **Validatie van beheer en registratie leiden (> 15 minuten)**

In deze stap bevestigen we dat uw beheersysteem potentiële klant leads op basis van de informatie in de installatie van de aanbieding kan ontvangen. Deze stap is alleen van toepassing op aanbiedingen met leadbeheer is ingeschakeld.

- **Laatste publiceren (> 30 minuten)**

Wij garanderen dat uw aanbieding openbaar beschikbaar in de marketplace wordt in deze stap maakt.

## <a name="update-existing-marketplace-offers"></a>Bijwerken van bestaande marketplace-aanbiedingen

Als u wilt wijzigingen aanbrengen in een aanbieding die u al hebt gepubliceerd, moet u eerst de bestaande aanbieding bijwerken en opnieuw publiceren.

## <a name="next-steps"></a>Volgende stappen

- [Een bestaande aanbieding op Marketplace commercieel bijwerken](./update-existing-offer.md)

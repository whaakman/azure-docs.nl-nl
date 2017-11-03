---
title: Handleiding voor het maken van een Service van gegevens voor de Marketplace | Microsoft Docs
description: Gedetailleerde instructies voor het maken, certificeren en implementeren van een Service voor gegevens aanschaffen op Azure Marketplace.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: 96194198-6991-43b4-918e-ee337e250339
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: c0c9362f1c2e15c947aaaf7187f3383ad243140f
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="data-service-publishing-guide-for-the-azure-marketplace"></a>Handleiding voor Azure Marketplace publiceren gegevensservice
> [!IMPORTANT]
> **Op dit moment wordt niet langer zijn voorbereiden op alle nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet goedgekeurd voor de aanbieding.** Als u een SaaS business-toepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u beschikt over een IaaS-toepassingen of developer-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Na het voltooien van de stap 1, [-Account maken en registratie](marketplace-publishing-accounts-creation-registration.md), we begeleide u via de [algemene niet-technische](marketplace-publishing-pre-requisites.md) en [technische vereisten](marketplace-publishing-data-service-creation-prerequisites.md) van een Data Service-aanbieding voor Azure Marketplace. Nu we u stapsgewijs door de stappen begeleidt voor het maken van een aanbieding Data-Service op de [Publishing Portal] [ link-pubportal] voor Azure Marketplace.

## <a name="1----login-to-the-publishing-portal"></a>1.    Meld u aan bij de publicatie van de Portal.
Ga naar [https://publish.windowsazure.com](https://publish.windowsazure.com.)

**Voor de eerste keer aanmelden bij Publishing Portal, moet u hetzelfde account waaraan uw bedrijfsprofiel verkoper is geregistreerd in het Developer Center gebruiken.**  (Later kunt u toevoegen elke werknemer van uw bedrijf als een co-beheerder in de Portal voor publiceren).

Klik op de **publiceren van een Data Services** tegel als dit de eerste aanmelding bij de publishing portal.

## <a name="2----choose-data-services-in-the-navigation-menu-on-the-left-side"></a>2.    Kies **Data Services** in het navigatiemenu aan de linkerkant.
  ![tekenen](media/marketplace-publishing-data-service-creation/pubportal-main-nav.png)

## <a name="3----create-a-new-data-service"></a>3.    Een nieuwe Data-Service maken
Vul de titel in voor uw nieuwe gegevens Service bieden en klik op '+' aan de rechterkant.

  ![tekenen](media/marketplace-publishing-data-service-creation/step-3.png)

## <a name="4----review-the-sub-menu-under-the-newly-created-data-service-in-the-navigation-menu"></a>4.    Bekijk de submenu onder de zojuist gemaakte Data-Service in het navigatiemenu.
Klik op de **scenario** tabblad en Bekijk alle noodzakelijke stappen die nodig zijn voor de Data-Service op Azure Marketplace correct wordt gepubliceerd.

> [!TIP]
> U kunt altijd klikt u op de koppelingen in de pagina 'Overzicht' of tabbladen gebruiken op de Data Service-aanbieding submenu aan de linkerkant.
> 
> 

## <a name="5----create-a-new-plan"></a>5.    Maak een nieuw Plan.
### <a name="offers-plans-transactions"></a>Biedt, plannen, transacties.
Elke aanbieding moet meerdere abonnementen kan hebben, maar ten minste één (1) abonnement. Wanneer eindgebruikers zich op uw aanbieding abonneert abonneren ze voor een van de aanbieding Plan. Elk plan bepaalt hoe eindgebruikers worden uw service gebruiken.

Op dit moment Azure Marketplace alleen maandelijkse abonnement transactie op basis van model ondersteuning voor Data Services, dat wil zeggen eindgebruikers loont de maandelijkse kosten volgens de prijs van het specifieke plan dat ze geabonneerd op en kunnen gebruiken voor elke maand aantal transacties die zijn gedefinieerd door het plan.

Elke transactie gewoonlijk gedefinieerd als het aantal records dat uw Data-Service wordt geretourneerd, is afhankelijk van de query verzonden naar de Service. De standaardwaarde is 100. Het aantal transacties die zijn geretourneerd naar elke query worden aantal records gedeeld door 100 en op het dichtstbijzijnde gehele getal afgerond.

Het is Azure Marketplace Service laag verantwoordelijkheid voor het bewaken van het aantal transacties die zijn gebruikt door elke query (meter).

> [!IMPORTANT]
> Eindgebruikers die de transactie tijdens de maand bereikt wordt niet door kan gaan om de service gebruiken tot einde van de maandelijkse abonnement cyclus geblokkeerd.
> 
> Het abonnement of een van de abonnementen kunt (maar niet moet) omvatten onbeperkt aantal transacties.
> 
> 

### <a name="create-a-plan"></a>Een schema maken.
1. Klik op **'+'** naast de 'Add een nieuw plan'.
2. Kies een van de opties: **onbeperkt** of **beperkt** gebruik voor dit abonnement.  Als beperkt het aantal transactie geeft kunt het plan gebruiken in een maand.
   
    ![tekenen](media/marketplace-publishing-data-service-creation/step-5.1.png)  
   
    Publiceren van de Portal wordt ook gevraagd of 'Abonnement-id', die wordt gebruikt om te communiceren met de eindgebruikers de naam van het plan in de gebruikersinterface en ook gebruikt door de Service marktplaats ter identificatie van het Plan. U kunt de 'Plan-id' wijzigen als u wilt.
   
   > [!NOTE]
   > De 'Plan-id' moet uniek zijn binnen het bereik van elke aanbieding. Omdat veel andere-id wordt gebruikt in de Publishing Portal Plan-id wordt vergrendeld na de eerste publiceren naar productie en u niet kunt wijzigen van deze id.
   > 
   > 
3. Klik op voor het accepteren van uw keuze.
4. Wordt u gevraagd enkele aanvullende vragen met betrekking tot de zojuist gemaakte planning.
   
    ![tekenen](media/marketplace-publishing-data-service-creation/step-5.2.png)

| Vraag | Significante veelvoud |
| --- | --- |
| **Dit Plan is vrij en beschikbaar wereldwijd?** |U kunt een volledig vrij van-gratis-plan maken. Als dit het enige plan voor deze aanbieding – betekent dit dat u 'Gratis bieden' in de Marketplace publiceren wilt. Als deze alleen gebruikt voor een (van enkele wordt) abonnement, it biedt u een optie voor het bieden van eindgebruikers voor meer informatie over uw service met een relatief klein aantal transacties per maand.  Als het antwoord 'Ja' is, wordt geen verdere vragen worden gevraagd. |

> [!NOTE]
> Eindgebruikers kunnen altijd upgraden naar de betaalde abonnementen.
> 
> 

| Vraag | Significante veelvoud |
| --- | --- |
| **Gratis proefversie beschikbaar is?** |U kunt kiezen tussen 'Nee proefversie' helemaal of geeft een optie voor het gebruik van uw abonnement voor 'Één maand'. Uitgevers als deze optie wilt gebruiken waarmee eindgebruikers de mogelijkheid om te begrijpen wat de voordelen van de aanbieding voor één maand gratis. |

> [!IMPORTANT]
> Eindgebruikers alleen kan worden een gratis proefversie schaffen als ze betaalmiddel bijvoorbeeld creditcard-of enterprise-overeenkomst hebt vastgesteld.
> 
> Na één maand van de gratis proefversie van gaat Azure Marketplace opladen klanten de prijs op de datum van het abonnement, tenzij de klant de annulering abonnement gestart. Er is geen speciale melding worden aan de eindgebruikers verstrekt.
> 
> 

| Vraag | Significante veelvoud |
| --- | --- |
| **Dit abonnement is vereist om een promotiecode om aan te schaffen?** |Uitgevers hebben een optie voor het beperken van toegang tot hun Service-plannen door op te geven van een speciale code, 'A Promocode' genoemd aan specifieke klanten. Alleen eindgebruikers waarvoor deze Promocode zich abonneren op het Plan. Als u 'Nee' kiest, wordt u gaat ermee akkoord dat iedereen van de regio waar het beschikbaar is (Zie [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) voor meer informatie) kunnen zich abonneren op dit plan. Er is geen verdere vragen gevraagd. |
| **Dit plan van iemand die niet beschikt over een geldige promotiecode ook verbergen?** |Als het antwoord naar de vorige vraag 'Ja' heeft de uitgever van een optie voor het verwijderen van dit abonnement wordt weergegeven in de gebruikersinterface van de Marketplace. Dit houdt in dat klanten niet zichtbaar voor dit abonnement in de pagina details van de aanbieding. Eindgebruikers ontvangen een promocode om aan te schaffen, zich abonneren op met behulp van deze promocode. |

## <a name="6----create-your-marketplace-marketing-content"></a>6.    Uw Marketplace marketing inhoud maken
Voor informatie over het vereiste gegevens **Marketing, prijs, ondersteuning en categorieën** tabbladen gaat u naar [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) die geldt voor alle artefacten die zijn gepubliceerd in Azure Marketplace.  

## <a name="7----connect-your-offer-to-your-service-sql-azure-based-or-web-service-based"></a>7.    Verbinding maken met uw Service (SQL Azure op basis van of op basis van Web-Service) van uw aanbieding.
Klik op de **Data Services** vervolgmenu.

Op het bovenste gedeelte van de pagina wordt u gevraagd om de aanbieding **Namespace**.  

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.png)

De onderstaande vraag definieert hoe de uitgever gaat zichtbaar nieuw gemaakte aanbieding voor Azure Marketplace. (Zie voor meer informatie de [Data Services technische vereisten Guide](marketplace-publishing-data-service-creation-prerequisites.md)).

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.2.png)

**De Database op basis van service publiceren**

Klik op **Database**. De volgende pagina wordt weergegeven:

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.3.png)

Een CSDL-toewijzing voor de gegevensset op basis van de SQL Azure DB maken:

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.4.png)

En vervolgens voor elke tabel

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.5.png)

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.6.png)

Als webservice

  ![tekenen](media/marketplace-publishing-data-service-creation/step-7.7.png)

> [!IMPORTANT]
> Lees [toewijzing van een bestaande webservice voor OData via CSDL](marketplace-publishing-data-service-creation-odata-mapping.md) voor gedetailleerde instructies en voorbeelden voor het maken van een CSDL-webservice.
> 
> 

## <a name="next-steps"></a>Volgende stappen
Nu dat u uw aanbieding Data-Service hebt gemaakt, zorg ervoor dat u de instructies in de [Marketplace Marketing Content Guide](marketplace-publishing-push-to-staging.md) voordat u verder gaan naar [testen van uw Data-Service in fasering](marketplace-publishing-data-service-test-in-staging.md).

## <a name="see-also"></a>Zie ook
* [Aan de slag: Hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)
* Als u geïnteresseerd bent in het algehele proces voor OData-toewijzing en het doel te begrijpen, Lees dit artikel [gegevens Service OData-toewijzing](marketplace-publishing-data-service-creation-odata-mapping.md) bekijken definities, structuren en instructies.
* Als u geïnteresseerd bent in het leren van en inzicht in de specifieke knooppunten en de bijbehorende parameters, Lees dit artikel [Service OData toewijzing gegevensknooppunten](marketplace-publishing-data-service-creation-odata-mapping-nodes.md) voor definities en uitleg, voorbeelden en gebruik case-context.
* Als u geïnteresseerd bent in de voorbeelden controleren, Lees dit artikel [gegevens Service OData toewijzing voorbeelden](marketplace-publishing-data-service-creation-odata-mapping-examples.md) voorbeeldcode zien en begrijpen codesyntaxis en in het contextmenu.

[link-pubportal]:https://publish.windowsazure.com

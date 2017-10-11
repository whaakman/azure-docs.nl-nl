---
title: Testen van uw Data-Service-aanbieding voor de Marketplace | Microsoft Docs
description: Begrijpen hoe u voor het testen van uw Data-Service-aanbieding voor Azure Marketplace.
services: marketplace-publishing
documentationcenter: 
author: HannibalSII
manager: hascipio
editor: 
ms.assetid: e861bd11-f74d-4d77-b4b5-23fb463644ad
ms.service: marketplace
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/26/2016
ms.author: hascipio; avikova
ms.openlocfilehash: 56a8aad7484fed18b74200ffa7acf22363625a15
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="testing-your-data-service-offer-in-staging"></a>Testen van uw Data-Service-aanbieding in fasering
> [!IMPORTANT]
> **Op dit moment wordt niet langer zijn voorbereiden op alle nieuwe gegevensservice uitgevers. Nieuwe dataservices wordt niet goedgekeurd voor de aanbieding.** Als u een SaaS business-toepassing die u wilt publiceren op AppSource hebt u meer informatie vindt [hier](https://appsource.microsoft.com/partners). Als u beschikt over een IaaS-toepassingen of developer-service die u wilt publiceren op Azure Marketplace u meer informatie vindt [hier](https://azure.microsoft.com/marketplace/programs/certified/).
> 
> 

Na het voltooien van de eerste twee stappen van [maken van uw account Microsoft Developer](marketplace-publishing-accounts-creation-registration.md) en [maken van uw gegevens Service bieden in Publishing Portal](marketplace-publishing-data-service-creation.md) u klaar bent om uw aanbieding beschikbaar zijn in Azure Marketplace. In dit onderwerp helpt u bij de eerste, tussenliggende, naam 'Staging' stap

Fasering betekent het implementeren van uw aanbieding in een particulier 'sandbox"waar u kunt testen en de functionaliteit ervan controleren voordat u deze naar productie. De aanbieding wordt weergegeven in de faseringsmodus net zoals voor een klant die is geïmplementeerd.

## <a name="step-1-pushing-your-offer-to-staging"></a>Step 1. Uw aanbieding voor fasering pushen
Uw aanbieding voor fasering pushen, kunt u de aanbieding te testen voordat deze beschikbaar voor toekomstige abonnees.  U kunt zien hoe uw aanbieding wordt weergegeven en werken voor degenen abonneren op uw gegevens.  

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-1.1.png)

1. Aanmelden bij de [Portal publiceren](https://publish.windowsazure.com)
2. Selecteer **Data Services** in het venster linkernavigatiegedeelte
3. Selecteer uw aanbieding die u wilt pushen voor fasering. Hier ziet u de bovenstaande scherm.
4. Klik op **Push fasering** knop.  
5. Als er problemen met de aanbieding die worden voltooid zijn moeten voordat het pushen voor fasering, ziet u een lijst weergegeven.  Corrigeer deze items door te klikken op elk item in de lijst. Wanneer alle correcties zijn aangebracht, klikt u op **Push voor fasering** knop opnieuw.

Als er geen problemen met uw aanbieding zijn ziet u het onderstaande pop-upvenster.  

Als u bent niet plannen/niet goedgekeurd voor uw aanbieding in Azure Portal surface (momenteel heeft beperkte capaciteit), sluit u alleen het pop-upvenster.

Test uw Data-Service in Azure Portal (in aanvulling op de portal DataMarket), moet u een Azure-abonnements-ID om mee te testen.  Dit abonnement-ID wordt de account die is toegestaan voor het testen van uw aanbieding identificeren.  

Knippen en plakken van uw abonnements-ID en klik op het vinkje om door te gaan.

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-1.2.png)

> [!NOTE]
> Deze id Azure-abonnementen zijn alleen vereist voor het testen en fasering in de [Azure Management Portal](https://manage.windowsazure.com). Ze zijn niet vereist om te testen in Azure Marketplace.
> 
> 

Het volgende scherm wordt weergegeven ziet publicatie is plaatsvinden door het pictogram 'wordt uitgevoerd' weer te geven dat hieronder geel gemarkeerd. Pushen voor fasering duurt tussen 10 tot 15 minuten.  Als het duurt langer, moet u eerst uw browser (druk op F5 in Internet Explorer) vernieuwen.  Klik op de koppeling naar contact in het zeldzame gevallen waarin uw aanbieding nog steeds wordt gepusht voor fasering na een uur, laat ons weten dat er een probleem is.

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-1.3.png)

Wanneer de Staging-Push het pictogram 'wordt uitgevoerd' is voltooid wordt stop verplaatsen en de status wordt bijgewerkt naar 'Tijdelijke'.  U bent nu klaar voor het testen van uw aanbieding.  

## <a name="step-2-test-your-staged-offer-in-datamarket"></a>Stap 2. Testen van uw voorbereide aanbieding in DataMarket
Klik op de koppeling die na de tekst **' Zie uw service bieden op... '** om weer te geven in het scherm dat de abonnee ziet wanneer uw aanbieding overschakelt naar de productie en in DataMarket wordt weergegeven.

  ![tekenen](media/marketplace-publishing-data-service-test-in-staging/step-2.2.png)

Testen of Controleer of elk van de 12 items gemarkeerd hierboven om te controleren of alle logo's, prijzen/transacties, tekst, afbeeldingen, documentatie en koppelingen correct zijn en werken goed.  Dit is een goed moment om te controleren of een testwaarden die u hebt ingevoerd bij het maken van uw aanbieding zijn vervangen door feitelijke waarden.

1. Aanbieding-logo
2. De aanbiedingsnaam van de
3. Publisher naam/koppeling naar de website van uw bedrijf
4. Categorieën voor uw aanbieding
5. Uw aanbieding ondersteuning koppeling om te helpen abonnees
6. Contextafhankelijke beschrijving voor uw aanbieding
7. Plan factureringsgegevens van aanbieding
8. Koppelen aan implementatiecode
9. Voorbeeld-installatiekopieën die aangeven gebruiken van gegevens van aanbieding
10. I/o-metagegevens voor elke service binnen de aanbieding
11. De gebruiksvoorwaarden van aanbieding
12. Voorbeeld van gegevens van de aanbieding

Controleer ten slotte dat de service werkt via de Datamarket door te klikken op de koppeling 'Deze GEGEVENSSET VERKENNEN'.  Een nieuw venster wordt geopend in het hulpprogramma noemen we 'Service Explorer' zodat u de resultaten van een query bij uw service kunt bekijken.  U kunt hier de parameters invoeren en de resultaten van een query op uw service weergegeven.   Weergegeven is ook de URL voor uw Query.  

> [!NOTE]
> Zorg ervoor dat de tekstbeschrijving van de service die wordt weergegeven boven bekijken.  En als uw aanbieding bestaat uit meer dan één service aanroepen, klikt u op de tabbladen aan de onderkant overschakelen naar de volgende service om te controleren en te testen.
> 
> 

## <a name="next-step"></a>Volgende stap
Als u problemen ondervindt en hebt u hulp nodig fouten op te lossen neemt contact op met [ondersteuning van Azure Publisher](http://go.microsoft.com/fwlink/?LinkId=272975).

Als u tevreden en gereed voor het publiceren van uw aanbieding Lees bent de [goedkeuring aanvragen naar productie Push](marketplace-publishing-push-to-production.md) documentatie.

## <a name="see-also"></a>Zie ook
* [Aan de slag: Hoe een aanbieding publiceren in Azure Marketplace](marketplace-publishing-getting-started.md)


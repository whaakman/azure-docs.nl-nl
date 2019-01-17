---
title: Bijwerken van Marketplace-aanbiedingen - Azure Marketplace | Microsoft Docs
description: Update-aanbiedingen op Azure en AppSource marktplaatsen, met behulp van de Cloud Partner-Portal
services: Azure, AppSource, Marketplace, Cloud Partner Portal,
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
ms.date: 01/11/2019
ms.author: pbutlerm
ms.openlocfilehash: 690ce08aa15a9677b04931ed1965ef819614ee84
ms.sourcegitcommit: a1cf88246e230c1888b197fdb4514aec6f1a8de2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54355540"
---
# <a name="update-azure-marketplace-and-appsource-offers"></a>Aanbiedingen voor Azure Marketplace en AppSource bijwerken

Er zijn verschillende soorten updates die u op uw aanbieding toepassen kunt nadat deze gepubliceerd.  De [Cloud Partner-Portal](https://cloudpartner.azure.com/) helpt u bij het correct als kenmerken van een aanbieding, met inbegrip van:

-  Nieuwe installatiekopie of pakket versie van virtuele machine (VM) toevoegen aan een bestaande SKU
-  Een SKU is beschikbaar in regio's wijzigen
-  Toevoegen van nieuwe SKU 's
-  Bijwerken van marketplace-metagegevens voor aanbiedingen of SKU 's 
-  Bieden van een bijwerken prijs van betalen per gebruik

De portal heeft ook functies, zoals de mogelijkheid om te vergelijken van functies en een geschiedenis van functies voor een aanbieding die u helpen bij het beheren van wijzigingen weer te geven.  Nadat u een aanbieding of SKU hebt gewijzigd, moet worden gepubliceerd voordat de wijzigingen gaan 'live'.  Dit artikel begeleidt u bij de verschillende aspecten van uw marketplace-aanbieding wordt bijgewerkt.

## <a name="unpermitted-changes-to-an-offersku"></a>Ongeoorloofde wijzigingen aan een aanbieding/SKU

Er zijn enkele kenmerken van een aanbieding of de SKU die niet worden gewijzigd nadat deze is gepubliceerd in de marketplace.  De bijbehorende velden zijn uitgeschakeld in de **Editor** tabblad van de portal, bijvoorbeeld:  

- Aanbieding-ID en uitgevers-ID
- SKU-ID 
- Telling van bestaande SKU's van de gegevensschijf
- Wijzigingen in het gegevensmodel van bestaande SKU's facturering/licenties
- Versie-tags, bijvoorbeeld: `1.0.1`


## <a name="common-update-operations"></a>Algemene updatebewerkingen

In de volgende secties wordt uitgelegd hoe u enkele van de meeste updatebewerkingen uitvoert.  Deze bewerkingen zijn niet beschikbaar voor alle typen.  U moet zich in de Cloud Partner-Portal een van deze bewerkingen te starten.


### <a name="update-offer-contacts"></a>Aanbieding contactpersonen bijwerken

Gebruik de volgende stappen uit om bij te werken van de contactpersonen voor ondersteuning van uw aanbieding.
1. In de **alle biedt** pagina, selecteert u de aanbieding.
2. Selecteer de **contactpersonen** tabblad. Uw contactpersonen bijwerken.
3. Selecteer de knop **Opslaan**.
4. Selecteer **publiceren** het publicatieproces te starten.


### <a name="change-regions-an-offer-or-sku-is-available-in"></a>Wijzigen van een aanbieding of de SKU is beschikbaar in regio 's

Na verloop van tijd kunt u uw aanbieding/SKU beschikbaar maken in meer regio's.
U kunt ook stoppen met het ondersteunen van de aanbieding/SKU in een bepaalde regio.
Deze wijzigingen wilt implementeren, volgt u de volgende stappen uit.

1. In de **alle aanbiedingen** pagina, vinden de aanbieding die u wilt bijwerken.

Voor Azure Marketplace-aanbiedingen:

2. Selecteer de **SKU's** tabblad.  Selecteer de SKU te wijzigen.
3. Klik op de **landen selecteren** knop onder de **land/regio beschikbaarheid** veld.
4. In het dialoogvenster van de beschikbaarheid van regio toevoegen of verwijderen van de regio's voor deze SKU.

Voor AppSource biedt het volgende:

2. Selecteer de **Storefront Details** tabblad.
3. Naast de **landen/regio's ondersteund** label, klikt u op **landen/regio's ondersteund**. 
4. In het dialoogvenster ondersteunde landen/regio's toevoegen of verwijderen van de regio's voor deze aanbieding.

Voor beide marketplace:

5. Klik op **publiceren** het publicatieproces te starten. 

Als u een SKU wordt beschikbaar gesteld in een nieuwe regio, hebt u de mogelijkheid om op te geven van de prijzen voor die bepaalde regio via het **prijzen gegevens exporteren** functionaliteit. Als u een regio weer die eerder beschikbaar was toevoegt, kunt u de prijzen omdat prijswijzigingen zijn niet toegestaan niet bijwerken.


### <a name="add-a-new-sku"></a>Voeg een nieuwe SKU toe 

Als u een nieuwe SKU voor een bestaande aanbieding, gebruikt u de volgende stappen uit:

1. In de **alle aanbiedingen** pagina, de aanbieding te vinden.
3. Onder de **SKU's** vormen, klikt u op **toevoegen van nieuwe Voorraadeenheid** en bieden een **SKU-ID** in het pop-upvenster.
4. Volg de rest van de stappen die worden beschreven [publiceren van een virtuele machine-aanbieding](../virtual-machine/cpp-publish-offer.md).
5. Klik op **publiceren** het publicatieproces te starten.


### <a name="update-offer-marketplace-assets"></a>Aanbieding marketplace activa bijwerken

Mogelijk hebt u scenario's waarbij u moet bijwerken van de marketplace op basis van tekst en afbeelding activa, zoals uw bedrijfslogo, bieden beschrijving, enzovoort. Gebruik de volgende stappen uit om bij te werken van deze elementen.

1. In de **alle aanbiedingen** pagina, merken dat uw aanbieding. 
2. Selecteer de **Marketplace** tabblad en volg de instructies in van uw aanbieding *Marketplace tabblad* onderwerp.
3. Klik op **publiceren** het publicatieproces te starten.


### <a name="update-pricing-on-published-offers"></a>De prijzen van gepubliceerde aanbiedingen bijwerken

Zodra uw betalen per gebruik-aanbieding is gepubliceerd, kunt u de prijs van een bestaande SKU niet verhogen.  In plaats daarvan een SKU onder de dezelfde aanbieding maken, verwijderen van de oude SKU en vervolgens uw aanbod opnieuw publiceren. U kunt de prijs van de eerder gepubliceerde aanbiedingen verlagen. De aanbiedingsprijs van uw verminderen:

1. Selecteer de SKU die u wilt verkleinen prijzen.
2. U moet de lagere prijs ingesteld door de dezelfde mechanisme dat u oorspronkelijk hebt gebruikt: rechtstreeks in de gebruikersinterface van de portal of met het werkblad voor importeren/exporteren.
3. Klik op **Opslaan**.
4. Klik op **publiceren** het publicatieproces te starten.

De prijzen is zichtbaar voor nieuwe klanten zodra ze actief op de marketplace is en alle nieuwe klanten wordt de nieuwe lagere prijs te betalen.  Voor bestaande klanten, de afname van de prijs met terugwerkende kracht weergegeven aan het begin van de factureringscyclus gedurende welke de afname van de prijs werd van kracht.  Als ze al zijn gefactureerd voor de cyclus waarbij er een lagere prijs is opgetreden, ontvangt deze een restitutie tijdens de volgende factureringscyclus voor de lagere prijs.


## <a name="compare-feature"></a>Functie vergelijken

Als u wijzigingen in een gepubliceerde aanbieding aanbrengt, kunt u de *vergelijken* functie om te controleren van de wijzigingen. Gebruik deze functie:

1. Op elk gewenst moment tijdens het bewerken, klikt u op de **vergelijken** knop in de **Editor** tabblad voor uw aanbieding.
2. Een vergelijking-venster bevat side-by-side-versies van de opgeslagen wijzigingen op deze aanbieding in vergelijking met de marketplace-aanbieding. 

![Aanbieding-knop in het tabblad editor vergelijken](./media/offer-compare-button.png)


## <a name="history-of-publishing-actions"></a>Geschiedenis van de publicatie van acties

Als u wilt weergeven van historische publicatie-activiteit, selecteer de **geschiedenis** tabblad in de linker verticale menubalk van de Cloud Partner-Portal.  De pagina Geschiedenis biedt flexibele door verschillende kenmerken te filteren en ondersteunt volgorde van kolommen.  Elke publishing gebeurtenis is voorzien van een tijdstempel.  Zie voor meer informatie, [Audit geschiedenispagina](../portal-tour/cpp-history-page.md).


## <a name="next-steps"></a>Volgende stappen

U kunt ook de Cloud Partner-Portal te gebruiken [verwijderen van een gepubliceerde SKU of aanbieding](./cpp-delete-offer.md).

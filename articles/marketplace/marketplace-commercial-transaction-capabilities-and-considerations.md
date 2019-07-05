---
title: Marketplace commerciële transactiemogelijkheden en aandachtspunten | Azure
description: Dit artikel wordt beschreven in de overwegingen voor Transact-prijzen, facturering, facturering en payout voor een aanbiedingtype.
services: Azure, Marketplace, Compute, Storage, Networking, Transact Offer Type
author: yijenj
manager: nuno costa
ms.service: marketplace
ms.topic: article
ms.date: 10/29/2018
ms.author: yijenj
ms.openlocfilehash: d266b314f19979578b7e7b8de4e7a7090200c9d2
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67445444"
---
# <a name="commercial-marketplace-transaction-capabilities-and-considerations"></a>Commerciële marketplace transactiemogelijkheden en aandachtspunten

In dit artikel bevat informatie over de volgende onderwerpen met betrekking tot commerce voor commerciële marketplace

* Opties voor het publiceren van Marketplace
* Algemeen overzicht Transact
* Transact factureringsmodellen
* Transact-vereisten

## <a name="marketplace-publishing-options"></a>Opties voor het publiceren van Marketplace

De volgende publicatie opties zijn beschikbaar voor commerciële marketplace-uitgevers.

### <a name="list--trial-publishing-options"></a>Lijst & proefversie publicatieopties

Uitgevers kunnen gebruikmaken van de lijst, proefversie en BYOL opties voor het publiceren van aanbiedingen en overname ten behoeve van gebruiker. Met deze opties Microsoft niet deelnemen aan rechtstreeks van de uitgever software-licentie transacties en er zijn geen transactiekosten verbonden. Uitgevers zijn verantwoordelijk voor de ondersteuning van alle aspecten van de software-licentie transactie, inclusief maar niet beperkt tot: order, afhandeling, softwaremeter, facturering, facturering, betaling en verzameling. Met de lijst en publicatieopties proefversie houdt uitgevers 100% van de uitgever softwarelicentiekosten die worden verzameld van de klant. 

### <a name="transact-publishing-option"></a>Publicatieoptie Transact

Naast de lijst en de proefversie publicatieopties is de publicatieoptie transact beschikbaar voor uitgevers. Dit maakt gebruik van Microsoft commerce wereldwijd beschikbare mogelijkheden en host cloud-marketplace transacties namens de uitgever van Microsoft kunt.

## <a name="transact-general-overview"></a>Algemeen overzicht Transact

Wanneer u de optie publiceren transact, maakt Microsoft de verkoop van software van derden en implementatie van bepaalde typen Azure-abonnement van de klant. De uitgever moet rekening houden met de facturering van de kosten voor infrastructuur en de van uitgever softwarelicentiekosten, bij het selecteren van een facturering model en type aanbieding.

De Transact-publicatieoptie wordt momenteel ondersteund voor de volgende typen: Virtuele Machines, Azure-toepassingen en SaaS-Apps.


![[Transacties Enterprise overeenkomsten in Azure Marketplace]](./media/marketplace-publishers-guide/Transact-enterprise-deals.png)

### <a name="billing-infrastructure-costs"></a>Infrastructuurkosten voor facturering

**Voor virtuele Machines en Azure-toepassingen**

Voor virtuele Machines en Azure-toepassingen, de infrastructuur van Azure-gebruikskosten worden in rekening gebracht voor Azure-abonnement van de klant.  Infrastructuur gebruikskosten zijn geprijsd en afzonderlijk wordt aangeboden door de licentiekosten voor de software-provider op de factuur van de klant.

**Voor SaaS-apps**

Voor SaaS-Apps, moet de uitgever-account voor Azure-infrastructuur gebruikskosten en softwarelicentiekosten als een item eenmalige kosten.  Deze wordt weergegeven als een vast bedrag aan de klant. Het gebruik van Azure-infrastructuur wordt beheerd en rechtstreeks aan de partner gefactureerd.  Werkelijke infrastructuur gebruikskosten zijn niet zichtbaar voor de klant.  Uitgevers is doorgaans ervoor kiezen te bundelen gebruikskosten voor Azure-infrastructuur in de software-licentie prijzen.  Software licentiekosten worden niet gemeten of op basis van verbruik.

## <a name="transact-billing-models"></a>Transact factureringsmodellen

Afhankelijk van de transactieoptie gebruikt, kunnen de kosten voor de softwarelicentie van de uitgever als volgt worden weergegeven:  

* Gratis: Geen kosten voor softwarelicenties. 

* Breng uw eigen license (BYOL): Van toepassing worden kosten voor softwarelicenties worden rechtstreeks tussen de uitgever en de klant beheerd. Microsoft geeft alleen via gebruikskosten voor Azure-infrastructuur. (Virtuele Machines en Azure alleen-toepassingen.)

* Pay-as-you-go: Kosten voor de softwarelicentie worden weergegeven als een per uur, per kern (vCPU) dat is gebaseerd op de Azure-infrastructuur gebruikt prijzen. Dit is alleen van toepassing op virtuele Machines en Azure-toepassingen.

* • Prijzen voor abonnement: Kosten voor de softwarelicentie worden weergegeven als maandelijkse of jaarlijkse overeenkomsten terugkerende kosten in rekening gebracht als een vast bedrag of per seat. Dit geldt alleen voor SaaS-Apps en -toepassingen in Azure-beheerde Apps.

* Gratis proefperiode van de software: Geen kosten voor softwarelicenties voor 30 dagen of 90 dagen.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Gratis en bring-your-own-license (BYOL) prijzen

Wanneer u een gratis of bring-your-own-license transactie-aanbieding publiceert, is Microsoft speelt geen een rol in de verkoop transactie voor de kosten voor de softwarelicentie te vergemakkelijken. Net als de lijst en de proefversie publicatieopties blijft de uitgever van de 100% van de kosten voor de softwarelicentie. 

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Betalen per gebruik en de prijzen van abonnement (site-indeling)

WPay-als-u-go en abonnementsprijzen bij het publiceren van een aanbieding voor betalen per gebruik of een ander abonnement transactie, biedt Microsoft de technologie en services voor het verwerken van software-licentie koopt, retourneert en toerekeningen. In dit scenario wordt Microsoft om te fungeren als een agent voor deze doeleinden geautoriseerd door de uitgever. De uitgever kan Microsoft om de software licensing-transactie, behouden hun aanduiding als de verkoper, provider, distributeur en Licentiegever mogelijk te maken.

Microsoft kan klanten op volgorde licentie en software-uitgever gebruik, de onderwerpen aan de voorwaarden en bepalingen van de commerciële Marketplace van Microsoft en van de uitgever gebruiksrechtovereenkomst. Uitgevers moeten hun gebruiksrechtovereenkomst bieden of Selecteer de [Standard Contract](https://docs.microsoft.com/azure/marketplace/standard-contract) bij het maken van de aanbieding.


### <a name="free-software-trials"></a>Gratis proefversies

Voor transact publishing scenario's, de uitgever kunt een software licentie beschikbaar gratis gedurende 30 dagen of 90 dagen. Deze mogelijkheid kortingsprogramma omvat de kosten van het gebruik van Azure-infrastructuur die wordt aangedreven door gebruik van de partneroplossing.

### <a name="private-offers"></a>Persoonlijke aanbiedingen

Naast het gebruik van bieden typen en facturering van modellen en verkopen van een aanbieding, uitgevers een persoonlijke aanbieding, compleet met in de onderhandelde, deal-specifieke prijzen of aangepaste configuraties kunnen verwerken. Persoonlijke aanbiedingen worden ondersteund door alle 3 transact publicatieopties.

Deze optie kan hoger of lager dan de openbaar beschikbare aanbieding-prijzen. Persoonlijke aanbiedingen kunnen worden gebruikt om de korting of een premium voor een aanbieding toevoegen. Persoonlijke aanbiedingen kunnen worden beschikbaar gesteld aan een of meer klanten door technische met hun Azure-abonnement op het niveau van de aanbieding.


### <a name="examples"></a>Voorbeelden

**Pay-As-You-Go** 

* Als u de betalen naar gebruik-optie inschakelt, hebt u de volgende structuur kosten.

|Uw licentie kosten  | $1,00 per uur  |
|---------|---------|
|Gebruik van Azure-kosten (D1/1-Core)    |   $0,14 per uur     |
|*De klant wordt gefactureerd op basis van Microsoft*    |  *$1.14 per uur*       |

* In dit scenario factureert Microsoft $1.14 per uur voor het gebruik van uw gepubliceerde VM-installatiekopie.

|Microsoft facturen  | $1.14 per uur  |
|---------|---------|
|Microsoft betaalt u 80% van de kosten van uw licentie|   $0,80 per uur     |
|Microsoft houdt 20% van de kosten van uw licentie  |  $0.20 per uur       |
|Microsoft houdt 100% van de kosten voor het gebruik van Azure | $0,14 per uur |

**Bring Your Own License (BYOL)**

* Als u de BYOL-optie inschakelt, hebt u de volgende structuur kosten.

|Uw licentie kosten  | Licentietarief onderhandeld en wordt gefactureerd op basis van u  |
|---------|---------|
|Gebruik van Azure-kosten (D1/1-Core)    |   $0,14 per uur     |
|*De klant wordt gefactureerd op basis van Microsoft*    |  *$0,14 per uur*       |

* In dit scenario factureert Microsoft $0,14 per uur voor het gebruik van uw gepubliceerde VM-installatiekopie.

|Microsoft facturen  | $0,14 per uur  |
|---------|---------|
|Microsoft houdt de kosten voor het gebruik van Azure    |   $0,14 per uur     |
|Microsoft houdt 0% van de kosten van uw licentie   |  $0,00 per uur       |

**SaaS-App-abonnement**

Deze optie moet worden geconfigureerd om te mogen verkopen via Microsoft en een vast tarief of per gebruiker op basis van een maandelijkse of jaarlijkse kan zijn geprijsd.
• Als u de verkopen via Microsoft-optie voor een SaaS-aanbieding, inschakelt hebt u de volgende kostenstructuur.

|Uw licentie kosten       | $100,00 per maand  |
|--------------|---------|
|Gebruik van Azure-kosten (D1/1-Core)    | Rechtstreeks op de publisher, niet de klant in rekening gebracht |
|*De klant wordt gefactureerd op basis van Microsoft*    |  *$100,00 per maand (Opmerking: de kosten van een gemaakte of Pass Through-infrastructuur in het licentietarief voor uitgever moet rekening)*  |

* In dit scenario Microsoft $100,00 voor uw softwarelicentie kosten in rekening gebracht en betaalt van $80,00 met de publisher.
* Partners die voor de kosten verlaagd Marketplace-Service hebt ziet dat een lagere transactiekosten in rekening op de SaaS-aanbiedingen van mei 2019 tot en met juni 2020. In dit scenario Microsoft $100,00 voor uw softwarelicentie kosten in rekening gebracht en betaalt van $90,00 met de publisher.

|Microsoft facturen  | $100,00 per maand  |
|---------|---------|
|Microsoft betaalt u 80% van de kosten van uw licentie <br> \* Microsoft betaalt u 90% van de kosten van uw licentie voor een gekwalificeerde SaaS-apps   |   $80,00 per maand <br> \* $90,00 per maand    |
|Microsoft houdt 20% van de kosten van uw licentie <br> \* Microsoft houdt 10% van de kosten van uw licentie voor alle gekwalificeerde SaaS-apps.  |  $20,00 per maand <br> \* $10.00     |

* **Kosten van de lagere Marketplace-Service:** Voor bepaalde SaaS-producten die u op onze commerciële Marketplace publiceert, Microsoft beperkt u de servicevergoeding voor Marketplace van 20% (zoals beschreven in de Microsoft-overeenkomst voor uitgevers) tot 10%.  In de volgorde voor uw Product in aanmerking komt, ten minste één van uw producten worden aangewezen door Microsoft als IP gezamenlijke verkoop gereed of IP-gezamenlijke verkoop-prioriteit. Voor het ontvangen van deze lagere servicevergoeding voor Marketplace gedurende de maand, moet de in aanmerking komen worden voldaan aan ten minste vijf (5) werkdagen vóór het einde van deze maand. Marketplace-Service bedrag wordt niet van toepassing op virtuele machines, beheerde Apps of andere producten beschikbaar gesteld via onze commerciële Marketplace verminderd.  Deze kosten verminderd Marketplace-Service is beschikbaar voor gekwalificeerde aanbiedingen met verzameld door Microsoft tussen 1 mei 2019 en 30 juni 2020-licentiekosten.  Na deze periode wordt retourneren de servicevergoeding voor Marketplace aan de normale hoeveelheid.

### <a name="customer-invoicing-payment-billing-and-collections"></a>Klantfacturering, betaling en facturering en verzamelingen

**Facturering en betaling**

Uitgever kunt gebruiken van de klant facturering methode de voorkeur aan het abonnement of kosten voor de softwarelicentie PAYGO leveren.

**Enterprise agreement** 

Als de klant gewenste factureringsmethode de Microsoft Enterprise Agreement is, wordt de kosten voor de softwarelicentie gefactureerd met deze methode als een gespecificeerde kosten, los van alle Azure-specifieke gebruikskosten facturering.

**Creditcards te accepteren en maandelijkse factuur** 

Klanten kunnen ook betalen met creditcard is geregistreerd en een maandelijkse factuur. In dit geval wordt de kosten voor de softwarelicentie net als bij het scenario voor Enterprise Agreement, wordt gefactureerd als een gespecificeerde kosten, los van alle Azure-specifieke gebruikskosten.

Bijvoorbeeld, als de klant koopt via een creditcard:

|Description    |    Date  |
|----------|----------|
|Volgorde-periode   | 15 augustus 2018 - en met 30 augustus 2018 |
|Term beëindigen (maand)   | En met 30 augustus 2018 |
|Factuurdatum | Vanaf 1 september 2018 |
|Klant-betaaldatum | Vanaf 1 september 2018 |
|Zodat bewaard periode (creditcards te accepteren, 30 dagen) | 1 september 2018 – 30 september 2018 |
|Begintijd van de verzameling periode | Vanaf 1 september 2018 |
|Verzameling periode einde (maximaal 30 dagen) | En met 30 september 2018 |
|Payout berekeningsdatum (elke maand op de 15e) | Vanaf 1 oktober 2018 |
|Betalings-datum | 15 oktober 2018 |

Als de klant koopt met behulp van een Enterprise Agreement:

| Description |    Date  |
|----------|----------|
|Volgorde-periode | 15 augustus 2018 - en met 30 augustus 2018 |
|Term beëindigen (kwartaal) | En met 30 september 2018 |
|Factuurdatum | 15 oktober 2018 |
|Zodat bewaard periode (creditcards te accepteren, 30 dagen) | N.v.t. |
|Begintijd van de verzameling periode | 15 oktober 2018 |
|Verzameling periode einde (maximaal 90 dagen) | 15 januari 2019 |
|Klant-betaaldatum | En met 30 december 2018 |
|Payout berekeningsdatum (elke maand op de 15e) | 15 januari 2019 |
|Betalings-datum | 15 februari 2019 |

**Gratis tegoed en monetaire toezegging** 

Sommige klanten ervoor kiezen Vooruitbetalen van Azure met een monetaire toezegging in de Enterprise Agreement of gratis tegoed hebt opgegeven voor gebruik met Azure. Hoewel dit tegoed kunnen worden gebruikt om te betalen voor gebruik van Azure, worden niet ze gebruikt om te betalen voor de kosten voor de softwarelicentie uitgever.

**Facturering en verzamelingen** 

Publisher software-licentie facturering wordt weergegeven met behulp van de klant geselecteerd-methode van facturering en volgt de tijdlijn voor facturering. Klanten zonder Enterprise Agreement erin worden maandelijks een factuur voor marketplace-softwarelicenties. Klanten met een Enterprise Agreement worden maandelijks gefactureerd via een factuur die elk kwartaal wordt weergegeven.

Wanneer het abonnement of betalen per gebruik prijsmodellen zijn geselecteerd, wordt Microsoft fungeert als de agent van de uitgever en is verantwoordelijk voor alle aspecten van facturering, betaling en -verzameling.

### <a name="publisher-payout-and-reporting"></a>Uitgever betalings- en rapportage

* Softwarelicentiekosten die door Microsoft als een agent worden verzameld zijn onderhevig aan 20% transactiekosten in rekening, tenzij anders vermeld en wordt in mindering gebracht op het moment van de uitgever payout.

* Klanten kopen meestal met behulp van de Enterprise Agreement of een creditcard betalen per gebruik overeenkomst ingeschakeld. Het overeenkomsttype bepaalt facturering, facturering, verzameling en payout timing.

>[!NOTE] 
>Alle reporting en inzichten voor de optie publiceren transact zijn beschikbaar via de Insights-sectie van de Cloud Partner-Portal of Analytics sectie van de Partner Center.

#### <a name="billing-questions-and-support"></a>Vragen over facturering en ondersteuning

Zie voor meer informatie en juridische beleid, de [-overeenkomst voor uitgevers](https://cloudpartner.azure.com/Content/Unversioned/PublisherAgreement2.pdf) (beschikbaar in de Cloud Partner-Portal).

Hulp op vragen over facturering, neem contact op met [commerciële marketplace publisher ondersteuning](https://aka.ms/marketplacepublishersupport).

## <a name="transact-requirements"></a>Transact-vereisten

De transact-vereisten voor verschillende typen worden behandeld in deze sectie.

### <a name="requirements-for-all-offer-types"></a>Vereisten voor alle bieden typen

- Een Microsoft-account en financiële gegevens zijn vereist voor de optie, ongeacht de aanbieding publiceren transact het prijsmodel.
- Verplichte financiële informatie omvat betalings-account en btw-profiel.

Zie voor meer informatie over het instellen van deze accounts [beheren van uw Partner Center Account](https://docs.microsoft.com/azure/marketplace/partner-center-portal/manage-account#financial-details).


### <a name="requirements-for-specific-offer-types"></a>Vereisten voor specifieke aanbiedingstypen

De transact-publicatieoptie is alleen beschikbaar voor gebruik met de volgende typen van de marketplace-aanbieding: 

**Virtuele machine** 

Selecteer in de gratis, bring-your-own-license of betalen per-as-you-gaan-prijzen modellen en worden weergegeven als de SKU's die zijn gedefinieerd op het niveau van de aanbieding. Op de factuur van de klant van Azure geeft Microsoft de uitgever van de kosten voor de softwarelicentie los van de kosten voor de onderliggende Azure-infrastructuur. Kosten voor Azure-infrastructuur worden aangestuurd door het gebruik van de software-uitgever.

**Azure-toepassingen: Oplossingssjabloon of beheerde App** 

Een of meer virtuele machines en pulls via de som van de prijzen van virtuele machines moet worden voorzien. Voor beheerde apps op één abonnement kunnen een vast bedrag maandabonnement worden geselecteerd, omdat de prijzen in plaats daarvan model de prijzen van virtuele machines. In sommige gevallen worden gebruikskosten voor Azure-infrastructuur doorgegeven aan de klant afzonderlijk van de kosten voor de softwarelicentie, maar op de dezelfde factuuroverzicht. Echter als u een beheerde App configureren voor ISV-kosten voor infrastructuur, biedt de Azure-resources worden in rekening gebracht met de publisher en de klant ontvangt een vast bedrag in dat de kosten van infrastructuur, licenties voor software en services management bevat.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de vereisten voor geschiktheid in de opties voor publiceren door de sectie type aanbieding voor het voltooien van de selectie en configuratie van uw aanbieding.
* Bekijk de publishing patronen door webwinkel voor voorbeelden van hoe uw oplossing wordt toegewezen aan een aanbiedingtype en de configuratie.

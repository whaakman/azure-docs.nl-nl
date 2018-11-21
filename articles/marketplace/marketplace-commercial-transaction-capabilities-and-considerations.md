---
title: Marketplace commerciële transactiemogelijkheden en aandachtspunten | Azure
description: Dit artikel wordt beschreven in de overwegingen voor Transact-prijzen, facturering, facturering en payout voor een aanbiedingtype.
services: Azure, Marketplace, Compute, Storage, Networking, Transact Offer Type
documentationcenter: ''
author: yijenj
manager: nuno costa
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 10/29/2018
ms.author: yijenj
ms.openlocfilehash: 32198387be680fd01558e900e4c438cde2cdc5b1
ms.sourcegitcommit: fa758779501c8a11d98f8cacb15a3cc76e9d38ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52266820"
---
# <a name="azure-marketplace-commercial-transaction-capabilities-and-considerations"></a>Azure Marketplace commercieel transactiemogelijkheden en aandachtspunten

Opties voor Azure Marketplace publiceren bieden unieke manieren om de software en service cloudproviders verbinding met klanten te. In dit artikel bevat informatie over de volgende onderwerpen met betrekking tot commerce in de Azure Marketplace:

* Opties voor het publiceren van Marketplace
* Algemeen overzicht Transact
* Transact factureringsmodellen
* Transact-vereisten

## <a name="marketplace-publishing-options"></a>Opties voor het publiceren van Marketplace

De volgende publicatie opties zijn beschikbaar voor Azure Marketplace-uitgevers.

### <a name="list--trial-publishing-options"></a>Lijst & proefversie publicatieopties

In Azure Marketplace, uitgevers kunnen gebruikmaken van de lijst en de proefversie publicatieopties voor aanbiedingen en overname ten behoeve van gebruiker. Microsoft niet deelnemen aan rechtstreeks van de uitgever software-licentie transacties met de lijst of proefversie publicatieopties en er zijn geen transactiekosten verbonden. Uitgevers zijn verantwoordelijk voor de ondersteuning van alle aspecten van de software-licentie transactie, inclusief maar niet beperkt tot: order, afhandeling, softwaremeter, facturering, facturering, betaling en verzameling. Met de lijst en publicatieopties proefversie houdt uitgevers 100% van de uitgever softwarelicentiekosten die worden verzameld van de klant. 

### <a name="transact-publishing-option"></a>Publicatieoptie Transact

Naast de lijst en de proefversie publicatieopties is de publicatieoptie transact beschikbaar voor Azure Marketplace-uitgevers.   Het maakt gebruik van de Microsoft wereldwijd beschikbaar commerce-functionaliteit. Deze optie kunt Microsoft host cloud-marketplace transacties namens de uitgever.

## <a name="transact-general-overview"></a>Algemeen overzicht Transact

Wanneer u de publicatieoptie transact gebruikt, kunt u Microsoft verkoop en implementatie van software van derden met Azure-abonnement van de klant. De uitgever moet rekening houden met de facturering van Azure-infrastructuur-kosten en de van uitgever softwarelicentiekosten, bij het selecteren van een facturering model en type in Azure Marketplace aanbieden. 

Het Transact publicatieoptie in Azure Marketplace wordt momenteel ondersteund voor de volgende typen: virtuele Machines, Azure-toepassingen of SaaS-Apps.

![[Transacties Enterprise overeenkomsten in Azure Marketplace]](./media/marketplace-publishers-guide/Transact-enterprise-deals.png)

### <a name="billing-infrastructure-costs"></a>Infrastructuurkosten voor facturering

**Voor virtuele Machines en Azure-toepassingen**

Voor virtuele Machines en Azure-toepassingen, de infrastructuur van Azure-gebruikskosten worden in rekening gebracht voor Azure-abonnement van de klant.  Infrastructuur gebruikskosten zijn geprijsd en afzonderlijk wordt aangeboden door de licentiekosten voor de software-provider op de factuur van de klant.

**Voor SaaS-apps**

Voor SaaS-Apps, moet de uitgever-account voor Azure-infrastructuur gebruikskosten en softwarelicentiekosten als een item eenmalige kosten.  Deze wordt weergegeven als een vaste maandelijkse kosten voor de klant. Het gebruik van Azure-infrastructuur wordt beheerd en rechtstreeks aan de partner gefactureerd.  Werkelijke infrastructuur gebruikskosten zijn niet zichtbaar voor de klant.  Uitgevers is doorgaans ervoor kiezen te bundelen gebruikskosten voor Azure-infrastructuur in de software-licentie prijzen.  Software licentiekosten worden niet gemeten of op basis van verbruik.

## <a name="transact-billing-models"></a>Transact factureringsmodellen

Afhankelijk van de transactieoptie gebruikt, kunnen de kosten voor de softwarelicentie van de uitgever als volgt worden weergegeven:  

* Gratis: Er zijn geen kosten voor softwarelicenties. 

* Uw eigen licentie (BYOL): van toepassing worden kosten voor softwarelicenties rechtstreeks tussen de uitgever en de klant worden beheerd. Microsoft geeft alleen via gebruikskosten voor Azure-infrastructuur. (Virtuele Machines en Azure alleen-toepassingen.)

* Betalen naar gebruik: Kosten voor de softwarelicentie worden weergegeven als een per uur, per kern (vCPU) dat is gebaseerd op de Azure-infrastructuur gebruikt prijzen. Dit is alleen van toepassing op virtuele Machines en Azure-toepassingen.

* Abonnement prijzen (site-gebaseerde): als een bedrag per maand, terugkerende kosten voor de softwarelicentie worden weergegeven.  Dit geldt alleen voor SaaS-Apps en -toepassingen in Azure-beheerde Apps.

* Gratis software-evaluatie: geen kosten voor softwarelicenties voor 30 dagen of 90 dagen.

### <a name="free-and-bring-your-own-license-byol-pricing"></a>Gratis en bring-your-own-license (BYOL) prijzen

Wanneer u een gratis of bring-your-own-license transactie-aanbieding publiceert, is Microsoft speelt geen een rol in de verkoop transactie voor de kosten voor de softwarelicentie te vergemakkelijken. Net als de lijst en de proefversie publicatieopties blijft de uitgever van de 100% van de kosten voor de softwarelicentie. 

### <a name="pay-as-you-go-and-subscription-site-based-pricing"></a>Betalen per gebruik en de prijzen van abonnement (site-indeling)

Bij het publiceren van een aanbieding voor betalen per gebruik of een ander abonnement transactie, biedt Microsoft de technologie en services voor aankopen van licenties voor software proces, retourneert en toerekeningen. In dit scenario wordt Microsoft om te fungeren als een agent voor deze doeleinden geautoriseerd door de uitgever. De uitgever kan Microsoft om de software licensing-transactie, behouden hun aanduiding als de verkoper, provider, distributeur en Licentiegever mogelijk te maken.

Microsoft biedt klanten de mogelijkheid om te rangschikken, licentie en software-uitgever, onderwerpen aan de voorwaarden en bepalingen van de Azure Marketplace en de volumelicentieovereenkomst van de eindgebruiker van de uitgever (Zie Cloud Partner-Portal). Uitgevers moeten hun gebruiksrechtovereenkomst van de aanbieding van marketplace leveren.

Orders verwerken via marketplace worden in rekening gebracht aan van de klant Azure-abonnement in een enkele factuur, facturering dezelfde methode als de kosten van de Azure-infrastructuur van de klant. Klanten kunnen de voorkeur facturering methode- en betalingsfouten instrument gebruikt voor de facturering van hun Azure-abonnement gebruiken.

### <a name="free-software-trials"></a>Gratis proefversies

Voor transact publishing scenario's, de uitgever kunt een software licentie beschikbaar gratis gedurende 30 dagen of 90 dagen. Deze mogelijkheid kortingsprogramma omvat de kosten van het gebruik van Azure-infrastructuur die wordt aangedreven door gebruik van de partneroplossing.

### <a name="private-offers"></a>Persoonlijke aanbiedingen

Naast het gebruik van bieden typen en facturering van modellen en verkopen van een aanbieding, uitgevers een privé-versie van de oplossing bieden, met in de onderhandelde, deal-specifieke prijzen, volledig en aangepaste configuraties met behulp van een aangepaste installatiekopie kunnen verwerken. Persoonlijke aanbiedingen worden ondersteund door alle 3 transact publicatieopties.

Deze prijsoptie kan hoger of lager is dan de openbaar weergegeven prijzen zijn.  Persoonlijke aanbiedingen kunnen worden gebruikt om de korting of een premium voor een aanbieding toevoegen. Persoonlijke aanbiedingen kunnen worden beschikbaar gesteld aan een of meer klanten door technische met hun Azure-abonnement op het niveau van de aanbieding.

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

**SaaS-App-abonnement (verkopen via Azure)**

Deze optie moet worden geconfigureerd om te mogen verkopen via Microsoft en kan zijn geprijsd met behulp van een of meer proportionele maandelijkse abonnementen gedefinieerd op het niveau van de aanbieding.

* Als u de verkopen via Azure optie inschakelt, hebt u de volgende structuur kosten.

|Uw licentie kosten       | $100,00 per maand  |
|--------------|---------|
|Gebruik van Azure-kosten (D1/1-Core)    | Rechtstreeks op de publisher, niet de klant in rekening gebracht |
|*De klant wordt gefactureerd op basis van Microsoft*    |  *$100,00 per maand (Opmerking: de kosten van een gemaakte of Pass Through-infrastructuur in het licentietarief voor uitgever moet rekening)*  |

* In dit scenario Microsoft $100,00 voor uw softwarelicentie kosten in rekening gebracht en betaalt van $80,00 met de publisher.

|Microsoft facturen  | $100,00 per maand  |
|---------|---------|
|Microsoft betaalt u 80% van de kosten van uw licentie    |   $80,00 per maand     |
|Microsoft houdt 20% van de kosten van uw licentie   |  $20,00 per maand       |

### <a name="customer-invoicing-payment-billing-and-collections"></a>Klantfacturering, betaling en facturering en verzamelingen

**Facturering en betaling**

Uitgever kunt gebruiken van de klant facturering methode de voorkeur aan het abonnement of kosten voor de softwarelicentie PAYGO leveren.

**Enterprise agreement** 

Als de klant gewenste factureringsmethode de Microsoft Enterprise Agreement is, wordt de kosten voor de softwarelicentie gefactureerd met deze methode als een gespecificeerde kosten, los van alle Azure-specifieke gebruikskosten facturering.

**Creditcards te accepteren en maandelijkse factuur** 

Klanten kunnen ook betalen met creditcard is geregistreerd en een maandelijkse factuur. In dit geval wordt de kosten voor de softwarelicentie net als bij het scenario voor Enterprise Agreement, wordt gefactureerd als een gespecificeerde kosten, los van alle Azure-specifieke gebruikskosten.

Bijvoorbeeld, als de klant koopt via een creditcard:

|Beschrijving    |    Date  |
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
|Beschrijving    |    Date  |
|----------|----------|
|Volgorde-periode | 15 augustus 2018 - en met 30 augustus 2018 |
|Term beëindigen (kwartaal) | En met 30 september 2018 |
|Factuurdatum | 15 oktober 2018 |
|Zodat bewaard periode (creditcards te accepteren, 30 dagen) | N.v.t. |
|Begintijd van de verzameling periode | 15 oktober 2018 |
|Verzameling periode einde (maximaal 90 dagen) | 15 januari 2018 |
|Klant-betaaldatum | En met 30 december 2018 |
|Payout berekeningsdatum (elke maand op de 15e) | 15 januari 2018 |
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
>Alle reporting en inzichten voor de optie publiceren transact zijn beschikbaar via de Insights-sectie van de Cloud Partner-Portal.

#### <a name="billing-questions-and-support"></a>Vragen over facturering en ondersteuning

Zie voor meer informatie en juridische beleid, de [-overeenkomst voor uitgevers](https://cloudpartner.azure.com/Content/Unversioned/PublisherAgreement2.pdf) (beschikbaar in de Cloud Partner-Portal).

Om hulp te krijgen op vragen over facturering, [maken van een ondersteuningsincident](https://support.microsoft.com/getsupport?wf=0&tenant=classiccommercial&oaspworkflow=start_1.0.0.0&locale=en-us&supportregion=en-us&pesid=16230&forceorigin=esmc&ccsid=636764613233453423) en kiest u virtuele Machines of Web-Apps (ook wel een SaaS-Apps), afhankelijk van het aanbod dat wordt gebruikt.

## <a name="transact-requirements"></a>Transact-vereisten

De transact-vereisten voor verschillende typen worden behandeld in deze sectie.

### <a name="requirements-for-all-offer-types"></a>Vereisten voor alle bieden typen

**Dev Center en Microsoft-account** 

* Zowel een Dev Center en een Microsoft-account zijn vereist voor de optie, ongeacht de aanbieding publiceren transact het prijsmodel.
* Het Dev Center-account bevat alle relevante financiële gegevens nodig is voor Microsoft voor het verzamelen van de kosten van de klant voor andere gebruikers van de uitgever en betalen van de uitgever.
* Hoewel u de dezelfde organisatie of details van aanmelding bij Microsoft voor beide accounts gebruiken mogelijk, is Dev Center een afzonderlijk account van de Publicatieportal van Cloud-account. Voor het gebruik van de transact-publicatieoptie, moet de uitgever van het Dev Center-account aanmeldingsproces geleid, naast het registreren voor toegang tot de Cloud Partner-Portal voltooien.

*Zie voor meer informatie over het instellen van deze accounts [geworden van een Cloud-Marketplace-uitgever](https://docs.microsoft.com/azure/marketplace/become-publisher).*

### <a name="requirements-for-specific-offer-types"></a>Vereisten voor specifieke aanbiedingstypen

De transact-publicatieoptie is alleen beschikbaar voor gebruik met de volgende typen van de marketplace-aanbieding: 

**Virtuele machine** 

Selecteer in de gratis, bring-your-own-license of betalen per-as-you-gaan-prijzen modellen en worden weergegeven als de SKU's die zijn gedefinieerd op het niveau van de aanbieding. Op de factuur van de klant van Azure geeft Microsoft de uitgever van de kosten voor de softwarelicentie los van de kosten voor de onderliggende Azure-infrastructuur. Kosten voor Azure-infrastructuur worden aangestuurd door het gebruik van de software-uitgever.

**Azure-toepassingen: De oplossingssjabloon of de beheerde App** 

Een of meer virtuele machines en pulls via de som van de prijzen van virtuele machines moet worden voorzien. Voor beheerde apps op één abonnement kunnen een vast bedrag maandabonnement worden geselecteerd, omdat de prijzen in plaats daarvan model de prijzen van virtuele machines. In beide gevallen worden gebruikskosten voor Azure-infrastructuur doorgegeven aan de klant afzonderlijk van de kosten voor de softwarelicentie, maar op de dezelfde factuuroverzicht.

## <a name="next-steps"></a>Volgende stappen

* Bekijk de vereisten voor geschiktheid in de opties voor publiceren door de sectie type aanbieding voor het voltooien van de selectie en configuratie van uw aanbieding.
* Bekijk de publishing patronen door webwinkel voor voorbeelden van hoe uw oplossing wordt toegewezen aan een aanbiedingtype en de configuratie.
* Een Marketplace-uitgever geworden en aanmelden bij de [Cloud Partner-Portal](https://cloudpartner.azure.com) maken en configureren uw aanbieding.

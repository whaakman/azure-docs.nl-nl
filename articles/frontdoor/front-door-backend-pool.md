---
title: Back-ends en back-end-pools in Azure-Service voordeur | Microsoft Docs
description: Dit artikel wordt beschreven welke back-ends en back-end-pools zijn voor de deur van configuratie.
services: front-door
documentationcenter: ''
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/10/2018
ms.author: sharadag
ms.openlocfilehash: 2372f49c7280ee5c817f3d2f98cc80a196dae5f5
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58879196"
---
# <a name="backends-and-backend-pools-in-azure-front-door-service"></a>Back-ends en back-end-pools in Azure voordeur Service
Dit artikel beschrijft de concepten over het toewijzen van de implementatie van uw app met Azure voordeur Service. Hierin wordt ook uitgelegd de verschillende termen in de configuratie van de voordeur over app-back-ends.

## <a name="backends"></a>Back-ends
Een back-end is gelijk aan het exemplaar van de implementatie van een app in een regio. Voordeur Service ondersteunt zowel Azure als niet-Azure-back-ends, zodat de regio is niet alleen tot Azure-regio's beperkt. Bovendien kan het zijn uw on-premises datacenter of een app-exemplaar in een andere cloud.

Back-ends voor voordeur Service verwijzen naar de hostnaam of het openbare IP-adres van uw app, die aanvragen van clients kan fungeren. Back-ends mag niet worden verward met de databaselaag, storage-laag, enzovoort. Back-ends moet worden weergegeven als het openbare eindpunt van de back-end van uw app. Wanneer u een back-end in een pool van de back-end voordeur toevoegt, moet u ook het volgende toevoegen:

- **Back-end hosttype**. Het type resource dat u wilt toevoegen. Voordeur Service biedt ondersteuning voor automatische detectie van back-ends voor uw app in appservice, cloudservice of opslag. Als u een andere resource in Azure of zelfs een niet-Azure back-end wilt, selecteer **aangepaste host**.

    >[!IMPORTANT]
    >Niet tijdens de configuratie valideren API's als de back-end niet toegankelijk is vanaf de voordeur omgevingen is. Zorg ervoor dat voordeur uw back-end kunnen bereiken.

- **Abonnement en de back-end hostnaam**. Als u dit nog niet hebt geselecteerd **aangepaste host** voor type back-end-host, selecteert u uw back-end door het kiezen van het juiste abonnement en de naam van de bijbehorende back-end in de gebruikersinterface.

- **Host-header van back-end**. De waarde van de host-header is verzonden naar de back-end voor elke aanvraag. Zie voor meer informatie, [host-header van back-end](#hostheader).

- **Prioriteit**. Als u wilt een back-end van de primaire service gebruiken voor al het verkeer, kunt u prioriteiten toewijzen aan uw verschillende back-ends. Geef ook back-ups als de primaire of de back-ends voor back-up niet beschikbaar zijn. Zie voor meer informatie, [prioriteit](front-door-routing-methods.md#priority).

- **Weight**. Het gewicht toewijzen aan uw verschillende back-ends om verkeer te verdelen over een set back-ends, gelijkmatig of op basis van gewicht coëfficiënten. Zie voor meer informatie, [gewichten](front-door-routing-methods.md#weighted).

### <a name = "hostheader"></a>Back-end host-header

Aanvragen die door de voordeur doorgestuurd naar een back-end bevatten een veld van de host-header die de back-end gebruikt voor het ophalen van de betreffende resource. De waarde voor dit veld wordt meestal afkomstig is van de back-end van de URI en heeft de host en poort.

Bijvoorbeeld: een verzoek voor www\.contoso.com heeft de host-header www\.contoso.com. Als u Azure portal gebruiken voor het configureren van uw back-end, is de standaardwaarde voor dit veld de hostnaam van de back-end. Als uw back-end contoso-westus.azurewebsites.net in Azure portal is, worden de waarde automatisch ingevuld voor back-end van de host-header contoso-westus.azurewebsites.net. Echter, als u Azure Resource Manager-sjablonen of een andere methode gebruiken zonder dat u dit veld expliciet instelt, voordeur Service stuurt de naam van de binnenkomende host de waarde voor de host-header. Als de aanvraag is gedaan voor www\.contoso.com en uw back-end is contoso-westus.azurewebsites.net waarvoor een veld leeg header, voordeur Service wordt de host-header ingesteld als www\.contoso.com.

De meeste back-ends Apps (Azure Web Apps, Blob storage en Cloud Services) vereisen dat de hostheader overeenkomt met het domein van de back-end. De frontend-host die u routes naar uw back-end gebruikt echter een andere hostnaam, zoals www\.contoso.azurefd.net.

Als uw back-end nodig is voor de hostheader overeenkomt met de naam van de back-end, ervoor zorgen dat de host-header van het back-end van de back-end naam host bevat.

#### <a name="configuring-the-backend-host-header-for-the-backend"></a>Configureren van de back-end-host-header voor de back-end

Het configureren van de **host-header van back-end** veld voor een back-end in de sectie van de back-end-toepassingen:

1. Open uw resource voordeur en selecteert u de back endadresgroep met de back-end te configureren.

2. Als u dit nog niet hebt gedaan, of een bestaand incident bewerkt, kunt u een back-end toevoegen.

3. De back-end-host header-veld ingesteld op een aangepaste waarde of laat dit veld leeg. De hostnaam voor de inkomende aanvraag wordt gebruikt als de waarde van de host-header.

## <a name="backend-pools"></a>Back-end-pools
Een back-endpool voor de deur van Service verwijst naar de set met back-ends die soortgelijke verkeer voor de app wordt ontvangen. Er is een logische groepering van uw app-instanties met andere woorden, over de hele wereld die hetzelfde verkeer ontvangen en hierop reageren met het verwachte gedrag. Deze back-ends zijn geïmplementeerd in verschillende regio's of binnen dezelfde regio. Alle back-ends kunnen zich in de modus actief/actief-implementatie of wat is gedefinieerd als de actief/passief-configuratie.

Een back-endpool wordt gedefinieerd hoe de verschillende back-ends via statustests moet worden geëvalueerd. Het definieert ook hoe u taakverdeling tussen de twee plaatsvindt.

### <a name="health-probes"></a>Statuscontroles
Voordeur Service verzendt aanvragen van de test periodieke HTTP/HTTPS naar elk van de geconfigureerde back-ends. Aanvragen van de WebTest bepalen de nabijheid en de status van elke back-end laden saldo ontvangen aanvragen van uw eindgebruikers. Instellingen van de Health-test voor een back endpool definiëren hoe we de status van back-ends Apps controleren. De volgende instellingen zijn beschikbaar voor loadbalancer-configuratie:

- **Path**. De URL die wordt gebruikt voor test-aanvragen voor alle de back-ends via de back-endpool. Bijvoorbeeld, als een van uw back-ends voor contoso-westus.azurewebsites.net is en het pad is ingesteld op /probe/test.aspx, klikt u vervolgens voordeur Service-omgevingen, ervan uitgaande dat het protocol is ingesteld op HTTP, wordt health test aanvragen verzenden naar http\:/ / contoso-westus.azurewebsites.net/probe/test.aspx.

- **Protocol**. Bepaalt of u de health test om aanvragen te verzenden vanaf de voordeur Service uw back-ends met HTTP of HTTPS-protocol.

- **Interval (seconden)**. Definieert de frequentie van statuscontroles voor uw back-ends, of de intervallen waarin elk van de voordeur omgevingen een test wordt verzonden.

    >[!NOTE]
    >Voor snellere failover, moet u het interval instellen op een lagere waarde. Hoe lager de waarde, hoe hoger het health test volume ontvangen uw back-ends. Bijvoorbeeld, als het interval is ingesteld op 30 seconden met 90 voordeur omgevingen of POP's wereldwijd, elke back-end ontvangt over 3 tot 5 test aanvragen per seconde.

Zie voor meer informatie, [statuscontroles](front-door-health-probes.md).

### <a name="load-balancing-settings"></a>Instellingen voor taakverdeling
Load balancing-instellingen voor de back endpool definiëren hoe we statuscontroles beoordelen. Deze instellingen bepalen of de back-end in orde is. Ze ook controleren hoe verdelen verkeer tussen verschillende back-ends via de back-endpool. De volgende instellingen zijn beschikbaar voor loadbalancer-configuratie:

- **Voorbeeld van een grootte**. Hiermee geeft u het aantal voorbeelden van statuscontroles we moet rekening houden met voor back-end evalueren.

- **Geslaagde samplegrootte**. Hiermee definieert u de samplegrootte zoals eerder vermeld, het aantal geslaagde voorbeelden die nodig zijn voor het aanroepen van de back-end in orde. Stel bijvoorbeeld een voordeur health test-interval is 30 seconden, samplegrootte is 5 seconden en geslaagde samplegrootte is 3 seconden. Telkens wanneer we de status evalueren de tests voor uw back-end, kijken we naar de laatste vijf voorbeelden meer dan 150 seconden (5 x 30). Ten minste drie geslaagde tests zijn vereist om aan te geven van de back-end als in orde.

- **Latentie gevoeligheid (extra latentie)**. Hiermee definieert u of u wilt dat de voordeur aan de aanvraag verzenden naar de back-ends binnen het bereik van latentie meting gevoeligheid of de aanvraag doorsturen naar de dichtstbijzijnde back-end.

Zie voor meer informatie, [minimale latentie op basis van de routeringsmethode](front-door-routing-methods.md#latency).

## <a name="next-steps"></a>Volgende stappen

- [Maak een profiel van de voordeur](quickstart-create-front-door.md)
- [De werking van de voordeur](front-door-routing-architecture.md)
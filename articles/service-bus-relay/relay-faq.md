---
title: Veelgestelde vragen over Azure Relay | Microsoft Docs
description: Vind antwoorden op enkele veelgestelde vragen over Azure Relay.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/23/2017
ms.author: sethm
ms.openlocfilehash: e8c146f4b6d02449be6ad9e991e52db8dfd58e04
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-relay-faqs"></a>Veelgestelde vragen over Azure Relay

In dit artikel worden enkele veelgestelde vragen (FAQ's) over [Azure Relay](https://azure.microsoft.com/services/service-bus/). Raadpleeg voor algemene Azure-prijzen en -ondersteuning informatie, [Azure ondersteunen Veelgestelde vragen over](http://go.microsoft.com/fwlink/?LinkID=185083).

## <a name="general-questions"></a>Algemene vragen
### <a name="what-is-azure-relay"></a>Wat is Azure Relay?
De [Azure Relay-service](relay-what-is-it.md) vereenvoudigt uw hybride toepassingen doordat u meer services die zich in een bedrijfsnetwerk naar de openbare cloud bevinden, veilig kunt blootstellen. De services kan worden blootgesteld zonder de firewallverbinding van een te openen en zonder wijzigingen in de infrastructuur van een bedrijfsnetwerk.

### <a name="what-is-a-relay-namespace"></a>Wat is een Relay-naamruimte?
Een [naamruimte](relay-create-namespace-portal.md) is een scoping container die u in uw toepassing Relay om resources te adresseren kunt gebruiken. U moet een naamruimte voor het gebruik van de Relay maken. Dit is een van de eerste stappen aan de slag.

### <a name="what-happened-to-service-bus-relay-service"></a>Wat is er gebeurd met Service Bus Relay-service?
De eerder genoemde Service Bus Relay-service wordt nu WCF Relay genoemd. U kunt blijven gewoon gebruik van deze service. De functie hybride verbindingen is een bijgewerkte versie van een service die wordt is getransplanteerd van Azure BizTalk Services. WCF Relay en hybride verbindingen nog steeds worden ondersteund.

## <a name="pricing"></a>Prijzen
Deze sectie worden enkele veelgestelde vragen over de prijzen structuur Relay. U kunt ook zien [Veelgestelde vragen over Azure-ondersteuning](http://go.microsoft.com/fwlink/?LinkID=185083) voor algemene Azure prijsinformatie. Zie voor meer informatie over prijzen voor Relay [Service Bus prijsinformatie][Pricing overview].

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Hoe u kosten in rekening gebracht voor hybride verbindingen en WCF Relay?
Zie voor meer informatie over prijzen voor Relay de [hybride verbindingen en WCF-Relays] [ Pricing overview] tabel op de Service Bus detailpagina met prijzen. Naast de prijzen genoteerd op die pagina, u in rekening worden gebracht voor de bijbehorende gegevensoverdracht voor uitgaande buiten het datacenter waarin de toepassing is ingericht.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Hoe ben ik in rekening gebracht voor hybride verbindingen?
Hier volgen drie facturering voorbeeldscenario's voor hybride verbindingen:

*   Scenario 1:
    *   U hebt één listener, zoals een exemplaar van de hybride verbindingen Manager geïnstalleerd en continu uitgevoerd voor de hele maand.
    *   U verzenden 3 GB aan gegevens via de verbinding in de maand. 
    *   De totale kosten is $5.
*   Scenario 2:
    *   U hebt één listener, zoals een exemplaar van de hybride verbindingen Manager geïnstalleerd en continu uitgevoerd voor de hele maand.
    *   U verzenden 10 GB aan gegevens via de verbinding in de maand.
    *   De totale kosten is $7,50. Dat is $5 voor de verbinding en de eerste 5 GB + $2,50 voor de aanvullende 5 GB aan gegevens.
*   Scenario 3:
    *   U hebt twee instanties, A en B, van de hybride verbindingen Manager geïnstalleerd en continu uitgevoerd voor de hele maand.
    *   U verzenden 3 GB aan gegevens via een verbinding in de maand.
    *   U verzenden 6 GB aan gegevens via verbinding B tijdens de maand.
    *   De totale kosten is $10,50. Dat is $5 voor de verbinding een + $5 voor verbinding B + $0,50 (voor de zesde gigabyte op verbinding B).

Houd er rekening mee dat de prijzen gebruikt in de voorbeelden van toepassing alleen tijdens de evaluatieperiode van hybride verbindingen zijn. Prijzen nog worden gewijzigd bij algemene beschikbaarheid van hybride verbindingen.

### <a name="how-are-hours-calculated-for-relay"></a>Hoe worden uren berekend voor Relay?

Relay WCF is alleen beschikbaar in Standard-laag naamruimten. Prijzen en [verbinding quota](../service-bus-messaging/service-bus-quotas.md) voor anders relays zijn niet gewijzigd. Dit betekent dat relays blijven in rekening gebracht op basis van het aantal berichten (geen bewerkingen) en de doorgifte-uren. Zie voor meer informatie de ['Hybride verbindingen en WCF Relays'](https://azure.microsoft.com/pricing/details/service-bus/) tabel op de prijspagina voor meer informatie.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-specific-relay"></a>Wat gebeurt er als ik heb meer dan één listener die zijn verbonden met een specifieke relay?
In sommige gevallen kan een enkele relay meerdere verbonden listeners hebben. Een relay wordt beschouwd als open wanneer ten minste één relay-listener is verbonden met het. Listeners worden toegevoegd aan een open relay resulteert in extra doorgifte-uren. Het aantal relay afzenders (clients die invoke of berichten verzenden naar relays) die zijn aangesloten op een relay heeft geen invloed op de berekening van doorgifte-uren.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Hoe wordt de meter berichten berekend voor WCF-Relays?
(**Dit geldt alleen voor WCF-relays. Berichten zijn geen kosten voor hybride verbindingen.** )

In het algemeen zijn factureerbare berichten voor relays berekend met behulp van de methode die wordt gebruikt voor brokered entiteiten (wachtrijen, onderwerpen en abonnementen) die eerder zijn beschreven. Er zijn echter enkele belangrijke verschillen.

Een bericht verzenden naar een Service Bus relay wordt behandeld als een 'volledige via' verzenden naar de relay-listener die het bericht ontvangt. Dit wordt niet beschouwd als een verzendbewerking Service Bus relay, gevolgd door een levering aan de relay-listener. Een aanvraag-antwoord stijl service aanroepen (van maximaal 64 KB) op basis van een relay listener resulteert in twee factureerbare berichten: één factureerbare bericht voor de aanvraag en één factureerbare bericht voor het antwoord (ervan uitgaande dat het antwoord is ook 64 KB of kleiner). Dit is anders dan het gebruik van een wachtrij te staan tussen een client en een service. Als u een wachtrij om te worden tussen een client en een service, is dit patroon request-reply vereist een aanvraag verzenden naar de wachtrij, gevolgd door een wachtrij halen/levering uit de wachtrij met de service. Dit wordt gevolgd door een antwoord verzenden naar een andere wachtrij en een wachtrij halen/levering van die wachtrij naar de client. Met de dezelfde grootte veronderstellingen gedurende (maximaal 64 KB), resulteert het patroon via wachtrij in 4 factureerbare berichten. U wordt gefactureerd voor twee keer het aantal berichten voor het implementeren van hetzelfde patroon dat u doen met behulp van de relay. Er zijn natuurlijk voordelen voor wachtrijen gebruiken voor het bereiken van dit patroon, zoals duurzaamheid en herverdeling van taken. Deze voordelen mogelijk extra kosten rechtvaardigen.

Relays die worden geopend met behulp van de **netTCPRelay** WCF binding berichten behandelen niet als afzonderlijke berichten, maar als een stream met gegevens die door het systeem. Wanneer u deze binding gebruikt, hebben alleen de afzender en de listener inzicht in de verwoording van de afzonderlijke berichten worden verzonden en ontvangen. Voor doorstuurt die gebruikmaken van de **netTCPRelay** binding, alle gegevens wordt behandeld als een stroom voor het berekenen van factureerbare berichten. Service Bus berekent in dit geval wordt de totale hoeveelheid gegevens verzonden of ontvangen via elke afzonderlijke relay op basis van de 5 minuten. Vervolgens wordt dat totale hoeveelheid gegevens verdeeld met 64 KB om te bepalen het aantal factureerbare berichten voor die relay in die periode.

## <a name="quotas"></a>Quota
| De naam van de quota | Bereik | Type | Gedrag wanneer overschreden | Waarde |
| --- | --- | --- | --- | --- |
| Gelijktijdige listeners op een relay |Entiteit |Statisch |De volgende aanvragen voor aanvullende verbindingen worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |25 |
| Gelijktijdige relay-listeners |Hele systeem |Statisch |De volgende aanvragen voor aanvullende verbindingen worden geweigerd en een uitzondering wordt ontvangen door de aanroepende code. |2,000 |
| Gelijktijdige relay-verbindingen per alle relay-eindpunten in de naamruimte van een service |Hele systeem |Statisch |- |5,000 |
| Relay-eindpunten per Servicenaamruimte |Hele systeem |Statisch |- |10.000 |
| Berichtgrootte voor [NetOnewayRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.netonewayrelaybinding.aspx) en [NetEventRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.neteventrelaybinding.aspx) doorstuurt |Hele systeem |Statisch |Binnenkomende berichten die groter is dan deze quota worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |64 kB |
| Berichtgrootte voor [HttpRelayTransportBindingElement](https://msdn.microsoft.com/library/microsoft.servicebus.httprelaytransportbindingelement.aspx) en [NetTcpRelayBinding](https://msdn.microsoft.com/library/microsoft.servicebus.nettcprelaybinding.aspx) doorstuurt |Hele systeem |Statisch |- |Onbeperkt |

### <a name="does-relay-have-any-usage-quotas"></a>Heeft Relay quota voor gebruik?
Microsoft stelt voor elke cloudservice wordt standaard een cumulatieve maandelijkse gebruiksgegevens van die is berekend voor alle abonnementen voor een klant. We begrijpen dat op tijdstippen uw behoeften, deze limieten overschrijdt mogelijk. U kunt contact opnemen met customer support op elk gewenst moment zodat we kunnen uw behoeften te begrijpen en pas deze limieten op de juiste wijze. Service bus zijn de quota voor cumulatieve gebruik als volgt uit:

* 5 miljard berichten
* 2 miljoen relayuren

Hoewel behoudt zich het recht om uit te schakelen van een account dat groter is dan de maandelijkse quota voor gebruik, bieden we een e-mailmelding en we meerdere maken probeert de klant contact alvorens een actie te ondernemen. Klanten die groter is dan deze quota zijn nog steeds zelf verantwoordelijk voor extra kosten.

### <a name="naming-restrictions"></a>Beperkingen voor naamgeving
De naam van een Relay-naamruimte moet tussen 6 en 50 tekens lang zijn.

## <a name="subscription-and-namespace-management"></a>Abonnement en de naamruimte management
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Hoe Migreer ik een naamruimte met een andere Azure-abonnement

Als u wilt een naamruimte van één Azure-abonnement verplaatst naar een ander abonnement, kunt u ofwel de [Azure-portal](https://portal.azure.com) of PowerShell-opdrachten gebruiken. Als u wilt een naamruimte naar een ander abonnement verplaatst, moet de naamruimte al actief zijn. De gebruiker de opdrachten uit te voeren, moet een beheerder op de bron- en doel-abonnementen.

#### <a name="azure-portal"></a>Azure Portal

Zie voor het gebruik van de Azure-portal één abonnement Azure Relay naamruimten migreren naar een ander abonnement, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

U kunt PowerShell verplaatsen van een naamruimte van één Azure-abonnement naar een ander abonnement, gebruiken de volgende reeks opdrachten. Om deze bewerking niet uitvoeren, de naamruimte moet al actief zijn en de gebruiker die de PowerShell-opdrachten moet een beheerder op de bron- en doel-abonnementen.

```powershell
# Create a new resource group in the target subscription.
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move the namespace from the source subscription to the target subscription.
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Wat zijn enkele van de uitzonderingen die worden gegenereerd door Azure Relay-API's en voorgestelde acties die u kunt nemen?
Zie voor een beschrijving van de algemene uitzonderingen en voorgestelde acties die u kunt uitvoeren, [Relay-uitzonderingen][Relay exceptions].

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-to-generate-a-signature"></a>Wat is er een shared access signature en welke talen kan ik gebruiken om een handtekening te genereren?
Shared Access Signatures (SAS) zijn een verificatiemethode op basis van beveiligde SHA-256-hashes of URI's. Zie voor meer informatie over het genereren van uw eigen handtekeningen in het knooppunt, PHP, Java, C en C# [Service Bus-verificatie met handtekeningen voor gedeelde toegang][Shared Access Signatures].

### <a name="is-it-possible-to-whitelist-relay-endpoints"></a>Is het mogelijk om geaccepteerde relay eindpunten?
Ja. De relay-client maakt verbinding met de Azure-Relay-service met behulp van de volledig gekwalificeerde domeinnamen. Klanten kunnen Voeg een vermelding voor `*.servicebus.windows.net` op firewalls die ondersteuning bieden voor DNS-whitelisting.

## <a name="next-steps"></a>Volgende stappen
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared access signatures]: ../service-bus-messaging/service-bus-sas.md
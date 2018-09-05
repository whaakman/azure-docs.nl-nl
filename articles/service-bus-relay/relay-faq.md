---
title: Veelgestelde vragen over Azure Relay | Microsoft Docs
description: Krijg antwoorden op enkele veelgestelde vragen over Azure Relay.
services: service-bus-relay
documentationcenter: na
author: spelluru
manager: timlt
editor: ''
ms.assetid: 886d2c7f-838f-4938-bd23-466662fb1c8e
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/21/2018
ms.author: spelluru
ms.openlocfilehash: e30e8c94547ac0f9106a69f1e99cf9a7c03abea5
ms.sourcegitcommit: cb61439cf0ae2a3f4b07a98da4df258bfb479845
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/05/2018
ms.locfileid: "43695894"
---
# <a name="azure-relay-faqs"></a>Veelgestelde vragen over Azure Relay

In dit artikel worden enkele veelgestelde vragen over [Azure Relay](https://azure.microsoft.com/services/service-bus/). Voor algemene Azure-prijzen en ondersteunende informatie, Zie de [ondersteuning voor veelgestelde vragen over Azure](https://azure.microsoft.com/support/faq/).

## <a name="general-questions"></a>Algemene vragen
### <a name="what-is-azure-relay"></a>Wat is Azure Relay?
De [Azure Relay-service](relay-what-is-it.md) vereenvoudigt het uitvoeren van uw hybride toepassingen doordat u meer services die zich in een bedrijfsnetwerk naar de openbare cloud bevinden, veilig kunt blootstellen. U kunt de services beschikbaar maken zonder een firewallverbinding te openen en zonder Tussenkomende wijzigingen in de infrastructuur van een bedrijfsnetwerk.

### <a name="what-is-a-relay-namespace"></a>Wat is een Relay-naamruimte?
Een [naamruimte](relay-create-namespace-portal.md) is een scoping container die u Relay om resources te adresseren binnen uw toepassing gebruiken kunt. U moet een naamruimte voor het gebruik van de Relay te maken. Dit is een van de eerste stappen aan de slag.

### <a name="what-happened-to-service-bus-relay-service"></a>Wat is er gebeurd met Service Bus Relay-service?
De eerder benoemde Service Bus Relay-service heet nu [WCF Relay](relay-wcf-dotnet-get-started.md). U kunt echter ook doorgaan met deze service zoals gebruikelijk. De functie Hybrid Connections is een bijgewerkte versie van een service die wordt is getransplanteerd van Azure BizTalk Services. WCF-Relay en hybride verbindingen nog steeds ondersteund.

## <a name="pricing"></a>Prijzen
In deze sectie vindt u antwoorden op enkele veelgestelde vragen over de prijsstructuur Relay. U kunt ook zien de [Veelgestelde vragen over Azure-ondersteuning](https://azure.microsoft.com/support/faq/) voor algemene Azure informatie over prijzen. Zie voor meer informatie over de prijzen voor Relay [Service Bus prijsinformatie][Pricing overview].

### <a name="how-do-you-charge-for-hybrid-connections-and-wcf-relay"></a>Hoe u kosten in rekening gebracht voor hybride verbindingen en WCF-Relay?
Zie voor meer informatie over de prijzen voor Relay, de [Hybrid Connections en WCF-Relays] [ Pricing overview] tabel op de pagina met prijsinformatie Service Bus. Naast de op deze pagina vermelde prijzen in rekening gebracht voor gegevensoverdracht van de bijbehorende gegevens voor uitgaande gegevens buiten het datacenter waarin uw toepassing is ingericht.

### <a name="how-am-i-billed-for-hybrid-connections"></a>Hoe Word ik gefactureerd voor hybride verbindingen?
Hier volgen drie facturering voorbeeldscenario's voor hybride verbindingen:

*   Scenario 1:
    *   U één listener hebt, zoals een instantie van Hybrid Connections Manager geïnstalleerd en continu wordt uitgevoerd voor de hele maand.
    *   U verzendt 3 GB gegevens over de verbinding gedurende de maand. 
    *   De totale kosten is $5.
*   Scenario 2:
    *   U één listener hebt, zoals een instantie van Hybrid Connections Manager geïnstalleerd en continu wordt uitgevoerd voor de hele maand.
    *   U verzendt 10 GB gegevens over de verbinding gedurende de maand.
    *   De totale kosten is $7,50. Dat is $5 voor de verbinding en eerste 5 GB + $2,50 voor de extra 5 GB aan gegevens.
*   Scenario 3:
    *   Hebt u twee instanties, A en B, hebt van Hybrid Connections Manager geïnstalleerd en continu wordt uitgevoerd voor de hele maand.
    *   U verzendt 3 GB gegevens over verbinding A per maand.
    *   U verzendt 6 GB aan gegevens over verbinding B per maand.
    *   De totale kosten is $10,50. Dat is $5 voor verbinding A + $5 voor verbinding B + $0,50 (voor de zesde GB op verbinding B).

Houd er rekening mee dat de prijzen in de voorbeelden van toepassing alleen tijdens de preview-periode van hybride verbindingen zijn. Prijzen zijn onderhevig aan wijzigingen bij algemene beschikbaarheid van Hybrid Connections.

### <a name="how-are-hours-calculated-for-relay"></a>Hoe worden de uren berekend voor omleiden?

WCF-Relay is alleen beschikbaar in Standard-laag-naamruimten. Prijzen en [verbinding quota](../service-bus-messaging/service-bus-quotas.md) voor relays anders zijn niet gewijzigd. Dit betekent dat relays worden gefactureerd op basis van het aantal berichten (niet-bewerkingen) en doorgifte-uren. Zie voor meer informatie de ["Hybride verbindingen en WCF-Relays"](https://azure.microsoft.com/pricing/details/service-bus/) tabel op de pagina met prijsinformatie.

### <a name="what-if-i-have-more-than-one-listener-connected-to-a-specific-relay"></a>Wat gebeurt er als ik meer dan één listener die zijn verbonden met een specifieke relay heb?
In sommige gevallen kan een enkele relay meerdere gekoppelde listeners hebben. Een relay wordt beschouwd als open wanneer ten minste één relay-listener is verbonden met het. Toe te voegen listeners met een open relay, resulteert in extra doorgifte-uren. Het aantal relay afzenders (clients die aanroepen of berichten verzenden naar relais) die zijn verbonden met een relay heeft geen invloed op de berekening van doorgifte-uren.

### <a name="how-is-the-messages-meter-calculated-for-wcf-relays"></a>Hoe wordt de meter voor berichten berekend voor de WCF-Relays?
(**Dit geldt alleen voor de WCF-relays. Berichten zijn niet de kosten voor hybride verbindingen.** )

In het algemeen worden factureerbare berichten voor relays berekend met behulp van dezelfde methode die wordt gebruikt voor verwerkte entiteiten (wachtrijen, onderwerpen en abonnementen) die eerder zijn beschreven. Er zijn echter enkele belangrijke verschillen.

Een bericht verzenden naar een Service Bus relay wordt behandeld als een 'volledige via' verzenden naar de relay-listener die het bericht ontvangt. Dit wordt niet beschouwd als een bewerking voor het verzenden naar de Service Bus relay, gevolgd door een levering aan de relay-listener. Een aanvraag / antwoord stijl Serviceaanroep (van maximaal 64 KB) op basis van een relay listener resulteert in twee factureerbare berichten: een factureerbaar bericht voor de aanvraag en een factureerbaar bericht voor het antwoord (ervan uitgaande dat het antwoord is ook 64 KB of kleiner is). Dit is anders dan het gebruik van een wachtrij dat overbrengt tussen een client en een service. Als u een wachtrij gebruiken dat overbrengt tussen een client en een service, moet hetzelfde patroon aanvraag / antwoord een aanvraag verzenden naar de wachtrij, gevolgd door een wachtrij halen/levering van de wachtrij met de service. Dit wordt gevolgd door een antwoord verzenden naar een andere wachtrij en een wachtrij halen/levering van die wachtrij naar de client. Met behulp van de dezelfde grootte veronderstellingen gedurende (maximaal 64 KB), resulteert het patroon wachtrij via in 4 factureerbare berichten. Krijgt u een factuur voor twee keer het aantal berichten voor het implementeren van hetzelfde patroon die u ook uitvoeren met behulp van doorsturen. Er zijn natuurlijk voordelen voor wachtrijen gebruiken voor het bereiken van dit patroon, zoals duurzaamheid en herverdeling van taken. Deze voordelen kunnen de extra kosten rechtvaardigen.

Relays die worden geopend met behulp van de **netTCPRelay** WCF binding behandelen berichten niet als afzonderlijke berichten, maar als een stroom van gegevens die binnenkomen via het systeem. Wanneer u deze binding gebruikt, hebben alleen de afzender en de listener inzicht in de verwoording van de afzonderlijke berichten verzonden en ontvangen. Voor doorgeeft die gebruikmaken van de **netTCPRelay** verbinding maakt, alle gegevens worden vervolgens beschouwd als een stroom voor het berekenen van factureerbare berichten. In dit geval, berekent Service Bus de totale hoeveelheid gegevens verzonden of ontvangen via een afzonderlijke relay op basis van 5 minuten. Vervolgens wordt dat totale hoeveelheid gegevens verdeeld met 64 KB om te bepalen het aantal factureerbare berichten voor de desbetreffende relay in die periode.

## <a name="quotas"></a>Quota
| Quotumnaam | Bereik |  Opmerkingen | Waarde |
| --- | --- | --- | --- |
| Gelijktijdige listeners op een relay |Entiteit |De volgende aanvragen voor extra verbindingen worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |25 |
| Gelijktijdige relay-verbindingen per alle relay-eindpunten in een service-naamruimte |Naamruimte |- |5.000 |
| Omleidingseindpunten per service-naamruimte |Naamruimte |- |10.000 |
| Berichtgrootte voor [NetOnewayRelayBinding](/dotnet/api/microsoft.servicebus.netonewayrelaybinding) en [NetEventRelayBinding](/dotnet/api/microsoft.servicebus.neteventrelaybinding) relays |Naamruimte |Binnenkomende berichten die groter zijn dan deze quota worden geweigerd en een uitzondering is ontvangen door de aanroepende code. |64 kB |
| Berichtgrootte voor [HttpRelayTransportBindingElement](/dotnet/api/microsoft.servicebus.httprelaytransportbindingelement) en [NetTcpRelayBinding](/dotnet/api/microsoft.servicebus.nettcprelaybinding) relays |Naamruimte |Geen limiet voor grootte van het bericht. |Onbeperkt |

### <a name="does-relay-have-any-usage-quotas"></a>Beschikt over Relay quota voor gebruik?
Microsoft stelt voor elke cloudservice wordt standaard een cumulatieve gebruiksquotum voor het maken van maandelijkse dat wordt berekend voor alle abonnementen van de klant. We begrijpen dat soms uw behoeften kunnen deze limiet overschrijden. U kunt contact opnemen met klantenservice op elk gewenst moment, zodat we kunnen uw behoeften te begrijpen en deze limieten op de juiste wijze aanpassen. Voor Service Bus zijn de cumulatieve gebruiksquota als volgt:

* 5 miljard berichten
* 2 miljoen relayuren

Hoewel we behouden ons het recht om uit te schakelen van een account dat groter is dan de maandelijkse gebruiksquota, we e-mailmelding bieden en we meerdere maken probeert contact opnemen met de klant voordat een actie wordt ondernomen. Klanten die groter zijn dan deze quota zijn nog steeds verantwoordelijk is voor de overtollige kosten in rekening gebracht.

### <a name="naming-restrictions"></a>Beperkingen voor naamgeving
De naam van een Relay-naamruimte moet tussen 6 en 50 tekens lang zijn.

## <a name="subscription-and-namespace-management"></a>Beheer van abonnementen en -naamruimte
### <a name="how-do-i-migrate-a-namespace-to-another-azure-subscription"></a>Hoe Migreer ik een naamruimte met een andere Azure-abonnement?

Om een naamruimte uit een Azure-abonnement naar een ander abonnement verplaatsen, kunt u ofwel de [Azure-portal](https://portal.azure.com) of PowerShell-opdrachten gebruiken. Als u wilt een naamruimte niet verplaatsen naar een ander abonnement, moet de naamruimte al actief zijn. De gebruiker die de opdrachten moet een beheerder op de bron- en doel-abonnementen.

#### <a name="azure-portal"></a>Azure Portal

Zie voor het gebruik van de Azure-portal naar Azure Relay-naamruimten van één abonnement naar een ander abonnement migreren, [resources verplaatsen naar een nieuwe resourcegroep of abonnement](../azure-resource-manager/resource-group-move-resources.md#use-portal). 

#### <a name="powershell"></a>PowerShell

Als u PowerShell wilt verplaatsen van een naamruimte van één Azure-abonnement naar een ander abonnement, gebruikt u de volgende reeks opdrachten. Voor het uitvoeren van deze bewerking, de naamruimte moet al actief zijn en de gebruiker die de PowerShell-opdrachten moet een beheerder op de bron- en doel-abonnementen.

```azurepowershell-interactive
# Create a new resource group in the target subscription.
Select-AzureRmSubscription -SubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff'
New-AzureRmResourceGroup -Name 'targetRG' -Location 'East US'

# Move the namespace from the source subscription to the target subscription.
Select-AzureRmSubscription -SubscriptionId 'aaaaaaaa-aaaa-aaaa-aaaa-aaaaaaaaaaaa'
$res = Find-AzureRmResource -ResourceNameContains mynamespace -ResourceType 'Microsoft.ServiceBus/namespaces'
Move-AzureRmResource -DestinationResourceGroupName 'targetRG' -DestinationSubscriptionId 'ffffffff-ffff-ffff-ffff-ffffffffffff' -ResourceId $res.ResourceId
```

## <a name="troubleshooting"></a>Problemen oplossen
### <a name="what-are-some-of-the-exceptions-generated-by-azure-relay-apis-and-suggested-actions-you-can-take"></a>Wat zijn enkele van de uitzonderingen die worden gegenereerd door Azure Relay-API's en aanbevolen acties die u kunt uitvoeren?
Zie voor een beschrijving van de algemene uitzonderingen en voorgestelde acties die u kunt uitvoeren, [Relay-uitzonderingen][Relay exceptions].

### <a name="what-is-a-shared-access-signature-and-which-languages-can-i-use-to-generate-a-signature"></a>Wat is er een shared access signature en welke talen kan ik gebruiken om een handtekening te genereren?
Shared Access Signatures (SAS) zijn een verificatiemechanisme voor op basis van beveiligde SHA-256-hashes of URI's. Zie voor meer informatie over het genereren van uw eigen handtekeningen in Node, PHP, Java, C en C# [Service Bus-verificatie met shared access signatures][Shared Access Signatures].

### <a name="is-it-possible-to-whitelist-relay-endpoints"></a>Is het mogelijk om de lijst met toegestane adressen omleidingseindpunten?
Ja. De relay-client maakt verbinding met de Azure Relay-service met behulp van de volledig gekwalificeerde domeinnamen. Klanten kunnen Voeg een vermelding voor `*.servicebus.windows.net` op firewalls die ondersteuning bieden voor DNS-opname in de whitelist.

## <a name="next-steps"></a>Volgende stappen
* [Een naamruimte maken](relay-create-namespace-portal.md)
* [Aan de slag met .NET](relay-hybrid-connections-dotnet-get-started.md)
* [Aan de slag met knooppunten](relay-hybrid-connections-node-get-started.md)

[Pricing overview]: https://azure.microsoft.com/pricing/details/service-bus/
[Relay exceptions]: relay-exceptions.md
[Shared Access Signatures]: ../service-bus-messaging/service-bus-sas.md

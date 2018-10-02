---
title: Overzicht van statusbewaking voor Azure Application Gateway
description: Meer informatie over de mogelijkheden voor bewaking in Azure Application Gateway
services: application-gateway
author: vhorne
manager: jpconnock
ms.service: application-gateway
ms.topic: article
ms.date: 8/6/2018
ms.author: victorh
ms.openlocfilehash: f9bd0288d4009af536bdc8f45cbaed4b3f1eee18
ms.sourcegitcommit: 7bc4a872c170e3416052c87287391bc7adbf84ff
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48018705"
---
# <a name="application-gateway-health-monitoring-overview"></a>Application Gateway overzicht van statuscontrole

Azure Application Gateway standaard controleert de status van alle resources in de back-end-pool en worden een resource als slecht beschouwd uit de groep automatisch verwijderd. Application Gateway blijft voor het bewaken van de exemplaren en terug naar de goede back-end-adrespool toegevoegd zodra ze beschikbaar komen en op statuscontroles reageren. Application gateway verzendt dat de statuscontroles met dezelfde poort die is gedefinieerd in de back-end-HTTP-instellingen. Deze configuratie zorgt ervoor dat de test dezelfde poort die klanten zouden worden gebruikt om te verbinden met de back-end wordt getest.

![Voorbeeld van Application gateway-test][1]

Naast het gebruik van standaard health test controle, kunt u ook de statustest aan de behoeften van de vereisten van uw toepassing aanpassen. In dit artikel worden zowel standaard als aangepaste statustests besproken.

## <a name="default-health-probe"></a>Standaard-statustest

Een application gateway wordt een statustest standaard automatisch geconfigureerd wanneer u een aangepaste test-configuratie niet instellen. Het gedrag van de controle werkt door een HTTP-aanvraag naar de IP-adressen geconfigureerd voor de back-endpool. Voor standaard tests als de back-end-http-instellingen zijn geconfigureerd voor HTTPS, de test maakt gebruik van HTTPS ook voor het testen van de status van de back-ends.

Bijvoorbeeld: configureren van uw application gateway voor het gebruik van back-endservers A, B en C voor het ontvangen van HTTP-verkeer op poort 80. De standaard-statuscontrole test de drie servers elke 30 seconden voor een goede HTTP-antwoord. Een gezonde HTTP-antwoord is een [statuscode](https://msdn.microsoft.com/library/aa287675.aspx) tussen 200 en 399.

Als de test standaard controle voor server A mislukt, de application gateway wordt deze verwijderd uit de back-end-pool en het netwerkverkeer stopt doorgestuurd naar deze server. De standaard-test blijft nog steeds controleren voor de server een elke 30 seconden. Als server A is op een aanvraag van een standaard-statustest reageert, deze weer als in orde wordt toegevoegd aan de back-end-pool en verkeer wordt doorgestuurd naar de server opnieuw.

### <a name="probe-matching"></a>Overeenkomen Probe

Standaard wordt een antwoord HTTP (S) met de statuscode 200 als in orde beschouwd. Daarnaast ondersteuning voor aangepaste statustests twee overeenkomende criteria. Die overeenkomen met criteria kan worden gebruikt om te wijzigen (optioneel) de standaardinterpretatie van wat wordt verstaan onder een reactie in orde.

De volgende zijn die overeenkomen met criteria: 

- **HTTP-antwoord status code overeenkomst** - test die overeenkomt met het criterium voor het accepteren van de gebruiker opgegeven bereiken uit de HTTP-antwoord code of antwoord-code. Afzonderlijke door komma's gescheiden antwoordstatuscodes of een bereik van de statuscode wordt ondersteund.
- **HTTP-antwoord hoofdtekst van de overeenkomst** - test die overeenkomen met criteria die er ongeveer zo uitziet op HTTP-antwoordtekst en overeenkomende gegevens met een gebruiker in de opgegeven tekenreeks. De overeenkomst alleen zoekt aanwezigheid van de gebruiker die is opgegeven tekenreeks in de hoofdtekst van antwoord en is niet een komt overeen met de volledige reguliere expressie.

Overeenkomen met criteria die kunnen worden opgegeven met behulp van de `New-AzureRmApplicationGatewayProbeHealthResponseMatch` cmdlet.

Bijvoorbeeld:

```
$match = New-AzureRmApplicationGatewayProbeHealthResponseMatch -StatusCode 200-399
$match = New-AzureRmApplicationGatewayProbeHealthResponseMatch -Body "Healthy"
```
Zodra de overeenkomen met criteria die is opgegeven, wordt deze kan worden gekoppeld als u wilt testen met behulp van configuratie van een `-Match` parameter in PowerShell.

### <a name="default-health-probe-settings"></a>Standaardinstellingen voor health test

| Test-eigenschap | Waarde | Beschrijving |
| --- | --- | --- |
| Test-URL |http://127.0.0.1:\<port\>/ |URL-pad |
| Interval |30 |De hoeveelheid tijd in seconden dat moet worden gewacht voordat de volgende statustest wordt verzonden.|
| Time-out |30 |De hoeveelheid tijd in seconden de toepassingsgateway moet wachten op een reactie test voordat u markeert de test is beschadigd. Als een test worden geretourneerd als in orde is, kunnen de bijbehorende back-end wordt onmiddellijk gemarkeerd als in orde.|
| Drempelwaarde voor onjuiste status |3 |Bepaalt hoeveel tests om te verzenden als er een storing van de reguliere statustest. Deze aanvullende tests snel achter elkaar worden verzonden om de status van de back-end snel te bepalen en niet wachten tot het testinterval. De back-endserver is gemarkeerd omlaag nadat het aantal opeenvolgende fouten de drempelwaarde voor onjuiste status heeft bereikt. |

> [!NOTE]
> De poort is dezelfde poort als de back-end-HTTP-instellingen.

De standaard-test kijkt naar alleen http://127.0.0.1:\<port\> om de van gezondheidsstatus te bepalen. Als u configureren met de statustest gaat u naar een aangepaste URL of andere instellingen wijzigen wilt, moet u aangepaste tests gebruiken.

### <a name="probe-intervals"></a>Testintervallen

Alle exemplaren van Application Gateway-test de back-end onafhankelijk van elkaar. De configuratie van de test hetzelfde geldt voor elk toepassingsgateway-exemplaar. Bijvoorbeeld, als de configuratie van de test is voor het verzenden van statuscontroles elke 30 seconden en de application gateway twee instanties biedt, verzenden vervolgens beide exemplaren met de statustest elke 30 seconden.

Ook als er meerdere listeners, controleert klikt u vervolgens elke listener de back-end onafhankelijk van elkaar. Bijvoorbeeld, als er twee listeners die verwijst naar dezelfde back-endadresgroep op twee verschillende poorten (geconfigureerd met twee back-end-http-instellingen) controleert klikt u vervolgens elke listener de dezelfde back-end onafhankelijk van elkaar. In dit geval zijn er twee tests van elk toepassingsgateway-exemplaar voor de twee listeners. Als er twee exemplaren van de application gateway in dit scenario, ziet u de back-end virtuele machine vier tests per testinterval voor de geconfigureerde.

## <a name="custom-health-probe"></a>Aangepaste statustest

Aangepaste tests kunnen u een meer nauwkeurige controle over de statuscontrole. Wanneer u aangepaste tests, kunt u de testinterval, de URL en pad om te testen en hoeveel mislukte reacties op accepteren voordat u markeert het exemplaar van de back-end-pool is beschadigd.

### <a name="custom-health-probe-settings"></a>Instellingen voor aangepaste health test

De volgende tabel bevat de definities voor de eigenschappen van een aangepaste statustest.

| Test-eigenschap | Beschrijving |
| --- | --- |
| Naam |De naam van de test. Deze naam wordt gebruikt om te verwijzen naar de test in de back-end-HTTP-instellingen. |
| Protocol |Het protocol dat wordt gebruikt voor het verzenden van de test. De test wordt gebruikt voor het protocol dat is gedefinieerd in de back-end-HTTP-instellingen |
| Host |De naam van de host voor het verzenden van de test. Van toepassing alleen als er meerdere sites is geconfigureerd in Application Gateway, anders gebruiken '127.0.0.1'. Deze waarde verschilt van de VM-hostnaam. |
| Pad |Relatief pad van de test. Ongeldig pad begint met '/'. |
| Interval |Testinterval in seconden. Deze waarde is het tijdsinterval tussen twee opeenvolgende tests. |
| Time-out |Test time-out in seconden. Als een geldige reactie niet binnen deze time-outperiode ontvangen is is, wordt de test is gemarkeerd als mislukt.  |
| Drempelwaarde voor onjuiste status |Aantal nieuwe pogingen-test. De back-endserver is gemarkeerd omlaag nadat het aantal opeenvolgende fouten de drempelwaarde voor onjuiste status heeft bereikt. |

> [!IMPORTANT]
> Als Application Gateway is geconfigureerd voor een enkele site, wordt standaard de Host moet naam worden opgegeven als '127.0.0.1', tenzij anders is geconfigureerd in aangepaste test.
> Ter referentie een aangepaste test wordt verzonden naar \<protocol\>://\<host\>:\<poort\>\<pad\>. De poort die wordt gebruikt is dezelfde poort zoals gedefinieerd in de back-end-HTTP-instellingen.

## <a name="nsg-considerations"></a>Overwegingen voor NSG

Als er een netwerkbeveiligingsgroep (NSG) op het subnet van een toepassingsgateway, moeten poortbereiken 65503 65534 worden geopend op het subnet van de gateway voor binnenkomend verkeer. Deze poorten zijn vereist voor de status van de back-end-API voor het werken.

Bovendien uitgaande verbinding met Internet kan niet worden geblokkeerd en binnenkomend verkeer die afkomstig zijn van de tag AzureLoadBalancer moet worden toegestaan.

## <a name="next-steps"></a>Volgende stappen
Nadat u meer over Application Gateway-statuscontrole, kunt u een [aangepaste statustest](application-gateway-create-probe-portal.md) in Azure portal of een [aangepaste statustest](application-gateway-create-probe-ps.md) met behulp van PowerShell en Azure Resource Manager implementatiemodel.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png

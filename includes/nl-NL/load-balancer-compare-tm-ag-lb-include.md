## <a name="load-balancer-differences"></a>Verschillen in Load Balancer

Er zijn verschillende opties voor het distribueren van netwerkverkeer met behulp van Microsoft Azure. Deze opties werken verschillend. Ze hebben een eigen functieset en ondersteunen verschillende scenario's. Ze kunnen elk afzonderlijk worden gebruikt, maar ook worden gecombineerd.

* **Azure Load Balancer** werkt via de transportlaag (laag 4 in de referentiestack van het OSI-netwerk). Deze service biedt distributie van verkeer op netwerkniveau tussen exemplaren van een toepassing die in hetzelfde datacenter van Azure wordt uitgevoerd.
* **Application Gateway** werkt via de toepassingslaag (laag 7 in de referentiestack van het OSI-netwerk). Deze service fungeert als een omgekeerde proxyservice, beëindigt de clientverbinding en stuurt aanvragen door naar back-endeindpunten.
* **Traffic Manager** werkt op DNS-niveau.  Er wordt gebruikgemaakt van DNS-antwoorden om verkeer van eindgebruikers door te sturen naar wereldwijd gedistribueerde eindpunten. Clients maken vervolgens rechtstreeks verbinding met deze eindpunten.

In de volgende tabel ziet u een overzicht van de functies die elke service biedt:

| Service | Azure Load Balancer | Application Gateway | Traffic Manager |
| --- | --- | --- | --- |
| Technologie |Transportniveau (laag 4) |Toepassingsniveau (laag 7) |DNS-niveau |
| Ondersteunde toepassingsprotocollen |Alle |HTTP, HTTPS en WebSockets |Alle (voor eindpuntbewaking is een HTTP-eindpunt vereist) |
| Eindpunten |Azure-VM's en Cloud Services-rolinstanties |Intern Azure-IP-adres, openbaar internet-IP-adres, Azure-VM of Azure Cloud Service |Azure-VM's, Cloud Services, Azure-web-apps en externe eindpunten |
| VNet-ondersteuning |Kan worden gebruikt voor internetgerichte en interne (VNet-)toepassingen |Kan worden gebruikt voor internetgerichte en interne (VNet-)toepassingen |Ondersteunt alleen internetgerichte toepassingen |
| Eindpuntbewaking |Ondersteund via tests |Ondersteund via tests |Ondersteund via HTTP/HTTPS GET |

Azure Load Balancer en Application Gateway routeren beide netwerkverkeer naar eindpunten, maar ze hebben verschillende gebruiksscenario's voor hoe het verkeer moet worden afgehandeld. In de volgende tabel wordt het verschil tussen de twee load balancers weergegeven:

| Type | Azure Load Balancer | Application Gateway |
| --- | --- | --- |
| Protocollen |UDP/TCP |HTTP, HTTPS en WebSockets |
| IP-reservering |Ondersteund |Niet ondersteund |
| Load balancing-modus |5-tuple (bron-IP, bronpoort, doel-IP, doelpoort, protocoltype) |Round robin<br>Routering op basis van URL |
| Load balancing-modus (bron-IP/vergrendelde sessies) |2-tuple (bron-IP en doel-IP), 3-tuple (bron-IP, doel-IP en poort). Kan omhoog of omlaag worden geschaald op basis van het aantal virtuele machines |Affiniteit op basis van cookies<br>Routering op basis van URL |
| Statuscontroles |Standaard: testinterval - 15 seconden. Uit rotatie gehaald: 2 doorlopende fouten. Ondersteunt door de gebruiker gedefinieerde tests |Inactieve testinterval - 30 seconden. Eruit gehaald na 5 opeenvolgende live verkeersfouten of het mislukken van één test in de inactieve modus. Ondersteunt door de gebruiker gedefinieerde tests |
| SSL-offloading |Niet ondersteund |Ondersteund |
| URL-gebaseerde routering | Niet ondersteund | Ondersteund|
| SSL-beleid | Niet ondersteund | Ondersteund|

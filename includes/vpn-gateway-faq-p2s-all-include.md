### <a name="supportedclientos"></a>Welke clientbesturingssystemen kan ik met point-to-site gebruiken?

De volgende clientbesturingssystemen worden ondersteund:

* Windows 7 (32-bits en 64-bits)
* Windows Server 2008 R2 (alleen 64-bits)
* Windows 8 (32-bits en 64-bits)
* Windows 8.1 (32-bits en 64-bits)
* Windows Server 2012 (alleen 64-bits)
* Windows Server 2012 R2 (alleen 64-bits)
* Windows 10
* OSX versie 10.11 voor Mac (El Capitan)
* macOS version 10.12 voor Mac (Sierra)

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hoeveel VPN-clienteindpunten kan mijn punt-naar-site-configuratie hebben?

Er kunnen maximaal 128 VPN-clients tegelijk met een virtueel netwerk worden verbonden.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan ik met punt-naar-site-functionaliteit proxy's en firewalls passeren?

Azure ondersteunt twee soorten point-to-site-VPN-opties:

* Secure Socket Tunneling Protocol (SSTP). SSTP is een bedrijfseigen, op SSL gebaseerde oplossing van Microsoft die firewalls kan passeren, omdat de meeste firewalls de TCP-poort 443 openen die door SSL wordt gebruikt.

* IKEv2 VPN. IKEv2 VPN is een op standaarden gebaseerde IPsec VPN-oplossing die gebruikmaakt van UDP-poort 500 en 4500 en IP-protocol nr. 50. Firewalls openen deze poorten niet altijd, zodat de kans bestaat dat een IKEv2 VPN proxy's en firewalls niet kan passeren.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Als ik een clientcomputer die is geconfigureerd voor punt-naar-site opnieuw start, wordt de VPN-verbinding dan automatisch opnieuw tot stand gebracht?

Standaard wordt de VPN-verbinding niet automatisch op de clientcomputer hersteld.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Biedt punt-naar-site ondersteuning voor automatisch opnieuw verbinding maken en DDNS op de VPN-clients?

Automatisch opnieuw verbinding maken en DDNS worden momenteel niet ondersteund in VPN’s met punt-naar-site-verbinding.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kunnen site-naar-site- en punt-naar-site-configuraties naast elkaar worden gebruikt in hetzelfde virtuele netwerk?

Ja. Voor het Resource Manager-implementatiemodel moet u een RouteBased VPN-type hebben voor uw gateway. Voor het klassieke implementatiemodel hebt u een dynamische gateway nodig. Point-to-site wordt niet ondersteund voor VPN-gateways met statische routering of PolicyBased VPN-gateways.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan ik een punt-naar-site-client configureren om verbinding te maken met meerdere virtuele netwerken tegelijk?

Nee. Een point-to-site-client kan alleen verbinding maken met resources in het VNet waarin de virtuele netwerkgateway zich bevindt.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hoeveel doorvoer kan ik verwachten via site-naar-site- of punt-naar-site-verbindingen?

Het is moeilijk om de exacte doorvoer van de VPN-tunnels te onderhouden. IPSec en SSTP zijn cryptografisch zware VPN-protocollen. Doorvoer wordt ook beperkt door de latentie en bandbreedte tussen uw locatie en het internet. Voor een VPN-gateway met alleen IKEv2 point-to-site-VPN-verbindingen is de totale doorvoer die u kunt verwachten, afhankelijk van de Gateway-SKU. Zie [Gateway-SKU's](../articles/vpn-gateway/vpn-gateway-about-vpngateways.md#gwsku) voor meer informatie over doorvoer.

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp-andor-ikev2"></a>Kan ik voor point-to-site elke VPN-softwareclient gebruiken die SSTP en/of IKEv2 ondersteunt?

Nee. U kunt alleen de systeemeigen VPN-client van Windows voor SSTP en de systeemeigen VPN-client van Mac voor IKEv2 gebruiken. Raadpleeg de lijst met ondersteunde clientbesturingssystemen.

### <a name="does-azure-support-ikev2-vpn-with-windows"></a>Biedt Azure ondersteuning voor IKEv2-VPN met Windows?

Gebruikers kunnen verbinding maken met Azure met behulp van de ingebouwde Windows VPN-client, die ondersteuning biedt voor IKEv2. Maar IKEv2-verbindingen vanaf een Windows-apparaat werken niet in het volgende scenario:

  Als het apparaat van de gebruiker een groot aantal vertrouwde basiscertificaten bevat, is de nettolading van het bericht tijdens IKE-uitwisseling groot en kan IP-laagfragmentatie optreden. De fragmenten worden aan de Azure-kant afgewezen, met negatieve gevolgen voor de verbinding. Het exacte aantal certificaten waarbij dit probleem optreedt, is moeilijk te schatten. Als gevolg hiervan zijn IKEv2-verbindingen vanaf Windows-apparaten niet gegarandeerd. Wanneer u zowel SSTP als IKEv2 configureert in een gemengde omgeving (bestaande uit Windows- en Mac-apparaten), probeert het Windows VPN-profiel altijd eerst de IKEv2-tunnel. Als dit mislukt als gevolg van het probleem dat hier wordt beschreven, wordt gebruikgemaakt van SSTP.

### <a name="other-than-windows-and-mac-which-other-platforms-does-azure-support-for-p2s-vpn"></a>Welke platformen, naast Windows en Mac, worden door Azure ondersteund voor P25-VPN?

Azure ondersteunt alleen Windows en Mac voor P2S-VPN.

### <a name="i-already-have-an-azure-vpn-gateway-deployed-can-i-enabled-radius-andor-ikev2-vpn-on-it"></a>Ik heb al een Azure VPN-gateway geïmplementeerd. Kan ik er RADIUS en/of IKEv2 VPN voor inschakelen?

Ja, u kunt deze nieuwe functies inschakelen op gateways die al zijn geïmplementeerd, zowel via PowerShell als Azure Portal.

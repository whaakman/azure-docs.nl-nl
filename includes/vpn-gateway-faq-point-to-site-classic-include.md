Deze veelgestelde vragen zijn van toepassing op P2S-verbindingen met het klassieke implementatiemodel.

### <a name="what-client-operating-systems-can-i-use-with-point-to-site"></a>Welke clientbesturingssystemen kan ik met punt-naar-site gebruiken?

De volgende clientbesturingssystemen worden ondersteund:

* Windows 7 (32-bits en 64-bits)
* Windows Server 2008 R2 (alleen 64-bits)
* Windows 8 (32-bits en 64-bits)
* Windows 8.1 (32-bits en 64-bits)
* Windows Server 2012 (alleen 64-bits)
* Windows Server 2012 R2 (alleen 64-bits)
* Windows 10

### <a name="can-i-use-any-software-vpn-client-for-point-to-site-that-supports-sstp"></a>Kan ik voor punt-naar-site elke VPN-softwareclient gebruiken die SSTP ondersteunt?

Nee. Ondersteuning is beperkt tot de hierboven vermelde versies van Windows-besturingssystemen.

### <a name="how-many-vpn-client-endpoints-can-i-have-in-my-point-to-site-configuration"></a>Hoeveel VPN-clienteindpunten kan mijn punt-naar-site-configuratie hebben?

Er kunnen maximaal 128 VPN-clients tegelijk met een virtueel netwerk worden verbonden.

### <a name="can-i-use-my-own-internal-pki-root-ca-for-point-to-site-connectivity"></a>Kan ik mijn eigen interne PKI basis-CA voor een punt-naar-site-verbinding gebruiken?

Ja. Voorheen konen alleen zelfondertekende basiscertificaten worden gebruikt. U kunt nog steeds 20 basiscertificaten uploaden.

### <a name="can-i-traverse-proxies-and-firewalls-using-point-to-site-capability"></a>Kan ik met punt-naar-site-functionaliteit proxy's en firewalls passeren?

Ja. SSTP (Secure Socket Tunneling Protocol) wordt gebruikt om firewalls te passeren. Deze tunnel wordt weergegeven als een HTTPs-verbinding.

### <a name="if-i-restart-a-client-computer-configured-for-point-to-site-will-the-vpn-automatically-reconnect"></a>Als ik een clientcomputer die is geconfigureerd voor punt-naar-site opnieuw start, wordt de VPN-verbinding dan automatisch opnieuw tot stand gebracht?

Standaard wordt de VPN-verbinding niet automatisch op de clientcomputer hersteld.

### <a name="does-point-to-site-support-auto-reconnect-and-ddns-on-the-vpn-clients"></a>Biedt punt-naar-site ondersteuning voor automatisch opnieuw verbinding maken en DDNS op de VPN-clients?

Automatisch opnieuw verbinding maken en DDNS worden momenteel niet ondersteund in VPN’s met punt-naar-site-verbinding.

### <a name="can-i-have-site-to-site-and-point-to-site-configurations-coexist-for-the-same-virtual-network"></a>Kunnen site-naar-site- en punt-naar-site-configuraties naast elkaar worden gebruikt in hetzelfde virtuele netwerk?

Ja. Beide oplossingen werken als de gateway een op RouteBased-type VPN heeft. Voor het klassieke implementatiemodel hebt u een dynamische gateway nodig. Punt-naar-site wordt niet ondersteund voor VPN-gateways met statische routering of gateways die gebruikmaken van de cmdlet `-VpnType PolicyBased`.

### <a name="can-i-configure-a-point-to-site-client-to-connect-to-multiple-virtual-networks-at-the-same-time"></a>Kan ik een punt-naar-site-client configureren om verbinding te maken met meerdere virtuele netwerken tegelijk?

Ja, dat is mogelijk. De virtuele netwerken kunnen echter geen overlappende IP-voorvoegsels hebben en de punt-naar-site-adresruimten tussen de virtuele netwerken mogen elkaar niet overlappen.

### <a name="how-much-throughput-can-i-expect-through-site-to-site-or-point-to-site-connections"></a>Hoeveel doorvoer kan ik verwachten via site-naar-site- of punt-naar-site-verbindingen?

Het is moeilijk om de exacte doorvoer van de VPN-tunnels te onderhouden. IPSec en SSTP zijn cryptografisch zware VPN-protocollen. Doorvoer wordt ook beperkt door de latentie en bandbreedte tussen uw locatie en het internet.

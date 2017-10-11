U opent een poort of een eindpunt met een virtuele machine (VM) in Azure maken met het maken van een netwerk-filter op een subnet of een VM-netwerkinterface. U kunt deze filters die binnenkomend en uitgaand verkeer worden beheerd, plaatsen op een Netwerkbeveiligingsgroep gekoppeld aan de resource die het verkeer ontvangt.

We gebruiken een voorbeeld van webverkeer op poort 80. Zodra u hebt een virtuele machine die is geconfigureerd om te fungeren webaanvragen op de standaard TCP-poort 80 (Vergeet niet de juiste services starten en alle firewallregels besturingssysteem op de virtuele machine ook openen), u:

1. Een Netwerkbeveiligingsgroep maken.
2. Maak een regel voor inkomend verkeer met toestaat:
   * het doelpoortbereik '80'
   * het bron-poortbereik van ' * ' (zodat alle bronpoorten)
   * de prioriteitswaarde minder 65,500 (om te worden geïnstalleerd met een hogere prioriteit dan de standaard catch all-regel voor weigeren van inkomende)
3. De Netwerkbeveiligingsgroep koppelen aan het VM-netwerkinterface of subnet.

U kunt complexe netwerkconfiguraties als u wilt beveiligen van uw omgeving met behulp van Netwerkbeveiligingsgroepen en regels kunt maken. Dit voorbeeld wordt slechts één of twee regels waarmee HTTP-verkeer of extern beheer. Zie voor meer informatie de volgende ["Meer informatie"](#more-information-on-network-security-groups) sectie of [wat is er een Netwerkbeveiligingsgroep?](../articles/virtual-network/virtual-networks-nsg.md)


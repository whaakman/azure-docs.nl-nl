- De virtuele netwerken kunnen zich in dezelfde of verschillende Azure-regio's (locaties) bevinden.

- Een cloudservice of een load balancing-eindpunt kan GEEN virtuele netwerken overbruggen, zelfs niet als ze met elkaar zijn verbonden.

- Om meerdere virtuele netwerken van Azure met elkaar te verbinden, hebt u geen on-premises VPN-gateways nodig, tenzij cross-premises connectiviteit vereist is.

- VNet naar VNet ondersteunt het verbinden van virtuele netwerken. Het biedt geen ondersteuning voor het verbinden van virtuele machines of cloudservices die GEEN deel uitmaken van een virtueel netwerk.

- VNet-naar-VNet vereist Azure VPN-gateways met op route gebaseerde VPN-typen (voorheen dynamische routering genoemd). 

- Virtuele-netwerkconnectiviteit kan tegelijkertijd worden gebruikt met multi-site-VPN's, met maximaal 10 (standaard/standaardgateways) of 30 (gateways met hoge prestaties) VPN-tunnels voor een VPN-gateway van een virtueel netwerk die verbinding maakt met andere virtuele netwerken of on-premises-sites.

- De adresruimten van de virtuele netwerken en on-premises lokale netwerksites mogen elkaar niet overlappen. Als adresruimten elkaar overlappen, kunnen de VNet-naar-VNet-verbindingen niet worden gemaakt.

- Redundante tunnels tussen een paar virtuele netwerken worden niet ondersteund.

- Alle VPN-tunnels van het virtuele netwerk delen de beschikbare bandbreedte op de Azure VPN-gateway en dezelfde SLA voor VPN-gatewaybedrijfstijd in Azure.

- VNet-naar-VNet-verkeer verplaatst zich via het Microsoft-netwerk, niet via internet.

- VNet-naar-VNet-verkeer in dezelfde regio is gratis voor beide richtingen; voor uitgaand VNet-naar-VNet-verkeer tussen regio's gelden de tarieven voor uitgaande gegevensoverdracht tussen VNets op basis van de bronregio's. Raadpleeg de [pagina met prijzen](https://azure.microsoft.com/pricing/details/vpn-gateway/) voor meer informatie.


<!--HONumber=ago16_HO4-->



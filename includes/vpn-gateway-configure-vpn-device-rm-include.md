Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. Er zijn veel verschillende VPN-apparaten die compatibel zijn met Azure. Zie [VPN-apparaten](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) voor meer informatie over VPN-apparaten en configuratie-instellingen. Controleer voordat u uw VPN-apparaat configureert of er [bekende compatibiliteitsproblemen](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) zijn met het VPN-apparaat dat u wilt gebruiken. Neem contact op met de fabrikant van uw apparaat voor specifieke configuratiegegevens voor het VPN-apparaat.

Bij de configuratie van uw VPN-apparaat hebt u het volgende nodig:

- **Het openbare IP-adres** van de gateway van uw virtuele netwerk.

    -  Navigeer naar **Virtuele netwerkgateways** en klik op de naam van uw gateway om het openbare IP-adres dat gebruikmaakt van Azure Portal, te achterhalen. 

    - Gebruik het volgende voorbeeld om het openbare IP-adres van uw virtuele netwerkgateway te vinden met behulp van PowerShell en vervang de waarden door uw eigen waarden.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- **Een gedeelde sleutel**. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site VPN-verbinding maakt. In onze voorbeelden gebruiken we een zeer eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te gebruiken.





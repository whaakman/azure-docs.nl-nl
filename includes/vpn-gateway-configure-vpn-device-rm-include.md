Voor site-naar-site-verbindingen met een on-premises netwerk is een VPN-apparaat vereist. Hoewel we niet voor alle VPN-apparaten de configuratiestappen geven, kan de informatie van de volgende koppelingen toch nuttig zijn:

- Zie [Over VPN-apparaten](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md) voor meer informatie over compatibele VPN-apparaten. 
- Zie [Gevalideerde VPN-apparaten](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#devicetable) voor koppelingen naar configuratie-instellingen. De koppelingen over apparaatconfiguratie worden naar beste vermogen geleverd. Het is altijd verstandig om de actuele configuratie-informatie op te vragen bij de fabrikant van uw apparaat.
- Zie [Bewerkingsvoorbeelden](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#editing) voor voorbeelden van het bewerken van de apparaatconfiguratie.
- Zie [Parameters](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#ipsec) voor de IPsec/IKE-parameters.
- Controleer voordat u uw VPN-apparaat configureert of er [bekende compatibiliteitsproblemen](../articles/vpn-gateway/vpn-gateway-about-vpn-devices.md#known) zijn met het VPN-apparaat dat u wilt gebruiken.

Bij de configuratie van uw VPN-apparaat hebt u het volgende nodig:

- Het openbare IP-adres van de gateway van uw virtuele netwerk.

    -  Navigeer naar **Virtuele netwerkgateways** en klik op de naam van uw gateway om het openbare IP-adres dat gebruikmaakt van Azure Portal, te achterhalen. 
    - Gebruik het volgende voorbeeld om het openbare IP-adres van uw virtuele netwerkgateway te vinden met behulp van PowerShell en vervang de waarden door uw eigen waarden.

            Get-AzureRmPublicIpAddress -Name GW1PublicIP -ResourceGroupName TestRG
- Een gedeelde sleutel. Dit is dezelfde gedeelde sleutel die u opgeeft wanneer u uw site-naar-site-VPN-verbinding maakt. In onze voorbeelden gebruiken we een zeer eenvoudige gedeelde sleutel. We raden u aan een complexere sleutel te gebruiken.





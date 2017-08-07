> [!NOTE]
> * Het openbare IP-adres van de VPN-gateway verandert wanneer u deze van een oude SKU naar een nieuwe SKU migreert.
> * U kunt geen klassieke VPN-gateways migreren naar de nieuwe SKU's. Klassieke VPN-gateways kunnen alleen de verouderde (oude) SKU's gebruiken.
> 

U kunt het formaat van uw Azure VPN-gateways niet wijzigen tussen de oude SKU's en de nieuwe SKU-families. Als u VPN-gateways in het Resource Manager-implementatiemodel hebt die de oudere versie van de SKU's gebruiken, kunt u naar de nieuwe SKU's migreren. Als u wilt migreren, verwijdert u de bestaande VPN-gateway voor het virtuele netwerk en maakt u een nieuwe.

Migratiewerkstroom:

1. Verwijder verbindingen van de gateway voor het virtuele netwerk.
2. Verwijder de oude VPN-gateway.
3. Maak de nieuwe VPN-gateway.
4. Werk uw on-premises VPN-apparaten bij met het nieuwe IP-adres van de VPN-gateway (voor site-naar-site-verbindingen).
5. Werk de waarde van het IP-adres voor de gateway bij voor alle lokale VNet-naar-VNet-netwerkgateways die verbinding met deze gateway maken.
6. Download nieuwe client-VPN-configuratiepakketten voor P2S-clients die verbinding maken met het virtuele netwerk via deze VPN-gateway.
7. Herstel de verbindingen met de gateway voor het virtuele netwerk.
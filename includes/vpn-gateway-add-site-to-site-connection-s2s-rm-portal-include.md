1. Zoek de gateway van het virtuele netwerk.
2. Klik op **Verbindingen**. Klik boven aan de blade Verbindingen op **+ Toevoegen** om de blade **Verbinding toevoegen** te openen.
   
    ![Site-naar-Site-verbinding maken](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connection.png)
3. Op de blade **Verbinding toevoegen** geeft u uw verbinding een **Naam**. 
4. Voor **Verbindingstype** selecteert u **Site-naar-site (IPSec)**.
5. Voor **Virtuele netwerkgateway** geldt een vaste waarde, omdat u verbinding maakt vanaf deze gateway.
6. Voor **Lokale netwerkgateway** klikt u op **Een lokale netwerkgateway kiezen** en selecteert u de lokale netwerkgateway die u wilt gebruiken. 
7. Voor **Gedeelde sleutel** moet de hier gebruikte waarde overeenkomen met de waarde die u voor uw lokale on-premises VPN-apparaat gebruikt. In het voorbeeld wordt abc123 gebruikt, maar u kunt een ingewikkeldere waarde gebruiken (aanbevolen). Het is van belang dat de waarde die u hier opgeeft, dezelfde waarde is die u hebt opgegeven bij het configureren van het VPN-apparaat.
8. De resterende waarden voor **Abonnement**, **Resourcegroep**, en **Locatie** zijn vast.
9. Klik op **OK** om uw verbinding te maken. U ziet *Verbinding maken* op het scherm knipperen.
10. Wanneer de verbinding is voltooid, wordt deze weergegeven op de blade **Verbindingen** voor uw virtuele netwerkgateway.
    
    ![Site-naar-Site-verbinding maken](./media/vpn-gateway-add-site-to-site-connection-s2s-rm-portal-include/connectionstatus450.png)


1. In de portal klikt u via **Alle resources** op **+Toevoegen**. In de **Alles** blade zoekvak, type **lokale netwerkgateway**, klikt u om te retourneren van een lijst met bronnen. Klik op **Lokale netwerkgateway** om de blade te openen en klik op **Maken** om de blade **Lokale netwerkgateway maken** te openen.
   
    ![maak een lokale netwerkgateway](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)

2. Op de **blade Lokale netwerkgateway maken**, voert u een **Naam** in voor uw lokale netwerkgateway-object. Gebruik indien mogelijk iets intuïtieve zoals **ClassicVNetLocal** of **TestVNet1Local**. Dit maakt het gemakkelijker kunt identificeren van de lokale netwerkgateway in de portal.
3. Geef een geldige openbare **IP-adres** voor de VPN-apparaat of de virtuele netwerkgateway waarmee u verbinding wilt maken.<br>**Als deze lokaal netwerk een on-premises-locatie vertegenwoordigt:** het openbare IP-adres van het VPN-apparaat dat u verbinding wilt maken. Het mag zich niet achter NAT bevinden en moet bereikbaar zijn voor Azure.<br>**Als u deze lokaal netwerk vertegenwoordigt een andere VNet:** het openbare IP-adres dat is toegewezen aan de virtuele netwerkgateway voor dit VNet opgeven.<br>**Als u het IP-adres nog geen hebt:** u kunt een geldige tijdelijke aanduiding voor IP-adres, gezamenlijk en keert u terug en wijzig deze instelling voordat u verbinding maakt.
4. **Adresruimte** verwijst naar de adresbereiken voor het netwerk dat dit lokale netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de bereiken die u hier opgeeft niet overlappen met adresbereiken van andere netwerken waarmee u verbinding maken.
5. Voor **Abonnement** controleert u of het juiste abonnement wordt weergegeven.
6. Voor **Resourcegroep** selecteert u de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een resourcegroep selecteren die u al hebt gemaakt.
7. Voor **locatie**, selecteer de locatie waarin deze bron wordt gemaakt. U kunt dezelfde locatie selecteren als die van uw VNet, maar dat is niet vereist.
8. Klik op **Maken** om de lokale netwerkgateway te maken.


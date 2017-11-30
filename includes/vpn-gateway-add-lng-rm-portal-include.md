1. In de portal klikt u via **Alle resources** op **+Toevoegen**.
2. In de **Alles** pagina zoekvak, type **lokale netwerkgateway**, klikt u om te retourneren van een lijst met bronnen. Klik op **Lokale netwerkgateway** om de pagina te openen en klik op **Maken** om de pagina **Lokale netwerkgateway maken** te openen.

  ![maak een lokale netwerkgateway](./media/vpn-gateway-add-lng-rm-portal-include/lng.png)
3. Op de **pagina Lokale netwerkgateway maken** geeft u de waarden voor uw lokale netwerkgateway op.

  - **Naam:** geef een naam op voor uw lokale netwerkgateway. Gebruik indien mogelijk iets intuïtieve zoals **ClassicVNetLocal** of **TestVNet1Local**. Dit maakt het gemakkelijker kunt identificeren van de lokale netwerkgateway in de portal.
  - **IP-adres:** Geef een geldige openbare **IP-adres** voor de VPN-apparaat of de virtuele netwerkgateway waarmee u verbinding wilt maken.

    * **Als deze lokaal netwerk een on-premises-locatie vertegenwoordigt:** het openbare IP-adres van het VPN-apparaat dat u verbinding wilt maken. Het mag zich niet achter NAT bevinden en moet bereikbaar zijn voor Azure.
    * **Als u deze lokaal netwerk vertegenwoordigt een andere VNet:** het openbare IP-adres dat is toegewezen aan de virtuele netwerkgateway voor dit VNet opgeven.
    * **Als u het IP-adres nog geen hebt:** u kunt een geldige tijdelijke aanduiding voor IP-adres, gezamenlijk en keert u terug en wijzig deze instelling voordat u verbinding maakt.
  - **Adresruimte** verwijst naar de adresbereiken voor het netwerk dat dit lokale netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de bereiken die u hier opgeeft niet overlappen met adresbereiken van andere netwerken waarmee u verbinding maken.
  - **BGP-instellingen configureren:** gebruik deze alleen bij het configureren van BGP. Selecteer deze anders niet.
  - **Abonnement:** controleer of het juiste abonnement wordt weergegeven.
  - **Resourcegroep:** selecteer de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een resourcegroep selecteren die u al hebt gemaakt.
  - **Locatie:** selecteer de locatie waarin dit object wordt gemaakt. U kunt dezelfde locatie selecteren als die van uw VNet, maar dat is niet vereist.
4. Klik op **Maken** om de lokale netwerkgateway te maken.
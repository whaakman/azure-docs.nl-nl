1. In de portal klikt u via **Alle resources** op **+Toevoegen**. 
2. In het zoekvak van de pagina **Alles** typt u **Lokale netwerkgateway**. Klik vervolgens om te zoeken. Er wordt dan een lijst geretourneerd. Klik op **Lokale netwerkgateway** om de pagina te openen en klik op **Maken** om de pagina **Lokale netwerkgateway maken** te openen.

  ![maak een lokale netwerkgateway](./media/vpn-gateway-add-lng-s2s-rm-portal-include/createlng.png)

3. Op de **pagina Lokale netwerkgateway maken** geeft u de waarden voor uw lokale netwerkgateway op.

  - **Naam:** geef een naam op voor uw lokale netwerkgateway.
  - **IP-adres:** dit is het openbare IP-adres van het VPN-apparaat waarmee u via Azure verbinding wilt maken. Geef een geldig openbaar IP-adres op. Het IP-adres mag zich niet achter NAT bevinden en moet bereikbaar zijn voor Azure. Als u op dit moment niet beschikt over het IP-adres, kunt u de waarden uit de schermafbeelding gebruiken. U moet dan wel later terugkeren om uw tijdelijke IP-adres te vervangen door het openbare IP-adres van uw VPN-apparaat. Anders kan er geen verbinding worden gemaakt vanuit Azure.
  - **Adresruimte** verwijst naar de adresbereiken voor het netwerk dat dit lokale netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de bereiken die u hier opgeeft, niet overlappen met bereiken van andere netwerken waarmee u verbinding wilt maken. Azure stuurt het adresbereik dat u opgeeft, door naar het IP-adres van het on-premises VPN-apparaat. *Gebruik hier uw eigen waarden, niet de waarden die worden weergegeven in de schermopname*.
  - **BGP-instellingen configureren:** gebruik deze alleen bij het configureren van BGP. Selecteer deze anders niet.
  - **Abonnement:** controleer of het juiste abonnement wordt weergegeven.
  - **Resourcegroep:** selecteer de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een resourcegroep selecteren die u al hebt gemaakt.
  - **Locatie:** selecteer de locatie waarin dit object wordt gemaakt. U kunt dezelfde locatie selecteren als die van uw VNet, maar dat is niet vereist.

4. Wanneer u klaar bent met het opgeven van de waarden klikt u op **Maken** onder aan de pagina om de lokale netwerkgateway te maken.
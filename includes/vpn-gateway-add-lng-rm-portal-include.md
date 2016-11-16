1. In de portal klikt u via **Alle resources** op **+Toevoegen**. In het zoekvak van de blade **Alles** typt u **Lokale netwerkgateway**. Klik vervolgens om te zoeken. Er wordt dan een lijst geretourneerd. Klik op **Lokale netwerkgateway** om de blade te openen en klik op **Maken** om de blade **Lokale netwerkgateway maken** te openen.
   
    ![maak een lokale netwerkgateway](./media/vpn-gateway-add-lng-rm-portal-include/addlng250.png)
2. Op de **blade Lokale netwerkgateway maken**, voert u een **Naam** in voor uw lokale netwerkgateway-object.
3. Geef een geldig openbaar **IP-adres** op voor het VPN-apparaat of de gateway van het virtuele netwerk waarmee u verbinding wilt maken.<br>Als dit lokale netwerk een on-premises locatie vertegenwoordigt, is dit het openbare IP-adres van het VPN-apparaat waarmee u verbinding wilt maken. Het mag zich niet achter NAT bevinden en moet bereikbaar zijn voor Azure.<br>Als dit lokale netwerk een ander VNet vertegenwoordigt, geeft u het openbare IP-adres op dat is toegewezen aan de gateway van het virtuele netwerk voor dat VNet.<br>
4. **Adresruimte** verwijst naar de adresbereiken voor het netwerk dat dit lokale netwerk vertegenwoordigt. U kunt meerdere adresruimtebereiken toevoegen. Zorg ervoor dat de bereiken die u hier opgeeft, niet overlappen met bereiken van andere netwerken waarmee u verbinding wilt maken.
5. Voor **Abonnement** controleert u of het juiste abonnement wordt weergegeven.
6. Voor **Resourcegroep** selecteert u de resourcegroep die u wilt gebruiken. U kunt een nieuwe resourcegroep maken of een resourcegroep selecteren die u al hebt gemaakt.
7. Selecteer voor **Locatie** de locatie waarin dit object wordt gemaakt. U kunt dezelfde locatie selecteren als die van uw VNet, maar dat is niet vereist.
8. Klik op **Maken** om de lokale netwerkgateway te maken.



<!--HONumber=Nov16_HO2-->



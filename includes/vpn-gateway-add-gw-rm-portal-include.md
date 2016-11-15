1. In de portal gaat u naar **Nieuw**. Typ 'Gateway van het virtuele netwerk' in het zoekvak. Klik op het zoekresultaat **Gateway van het virtuele netwerk**. Hiermee opent u de blade **Gateway van het virtuele netwerk maken**.
2. Klik op **Maken** onderaan de blade **Gateway van het virtuele netwerk**. De blade **Gateway van het virtuele netwerk maken** wordt geopend. Vul de waarden in voor de gateway van het virtuele netwerk.
   
    ![Velden van de blade Gateway van het virtuele netwerk maken](./media/vpn-gateway-add-gw-rm-portal-include/createvnetgw300.png "Create virtual network gateway blade fields")
3. **Naam**: naam van uw gateway. Deze mag niet gelijk zijn aan de naam van het gateway-subnet. Het is de naam van het gateway-object dat u maakt.
4. **Gatewaytype**: selecteer **VPN**. VPN-gateways maken gebruik van een gateway van het virtuele netwerk van het type **VPN**. 
5. **VPN-type**selecteer het VPN-type dat wordt opgegeven voor uw configuratie. De meeste configuraties vereisen een op route gebaseerd VPN-type.
6. **SKU**: selecteer de gateway-SKU in de vervolgkeuzelijst. Welke SKU's worden weergegeven in de vervolgkeuzelijst, is afhankelijk van het VPN-type dat u selecteert.
7. **Locatie**: wijzig de **locatie** om naar de locatie van het virtuele netwerk te verwijzen.
8. Kies het virtuele netwerk waaraan u deze gateway wilt toevoegen. Klik op **Virtueel netwerk** om de blade **Een virtueel netwerk kiezen** te openen. Selecteer het VNet. Als u uw VNet niet ziet, moet u controleren of het veld **Locatie** verwijst naar de regio waarin het virtuele netwerk zich bevindt.
9. Kies een openbaar IP-adres. Klik op **Openbaar IP-adres** om de blade **Openbaar IP-adres kiezen** te openen. Klik op **+Nieuwe maken** om de blade **Openbaar IP-adres maken** te openen. Geef een naam op voor uw openbare IP-adres. Met deze blade maakt u een openbaar IP-adresobject waaraan een openbaar IP-adres dynamisch wordt toegewezen.<br>Klik op **OK** om uw wijzigingen in deze blade op te slaan.
10. **Abonnement**: controleer of het juiste abonnement is geselecteerd.
11. **Resourcegroep**: deze instelling wordt bepaald door het virtuele netwerk dat u selecteert. 
12. Wijzig de **locatie** niet nadat u de voorgaande instellingen hebt opgegeven.
13. Controleer de instellingen. U kunt onder aan de blade **Vastmaken aan dashboard** selecteren, als u wilt dat uw gateway op het dashboard wordt weergegeven.
14. Klik op **Aanmaken** om de gateway aan te maken. De instellingen worden gevalideerd en op het dashboard wordt de tegel 'De gateway van het virtuele netwerk implementeren' weergegeven. Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.
    
    ![De gateway van een virtueel netwerk implementeren](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png "Deploying Virtual network gateway")
15. Nadat de gateway is aangemaakt, kunt u het IP-adres dat eraan is toegewezen bekijken door naar het virtuele netwerk in de portal te kijken. De gateway wordt weergegeven als aangesloten apparaat. U kunt op het aangesloten apparaat klikken (uw gateway van het virtuele netwerk) om meer informatie te laten weergeven.



<!--HONumber=Nov16_HO2-->



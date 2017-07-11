1. Klik links op de portalpagina op **+** en typ 'Virtuele netwerkgateway' in het zoekvak. Zoek in **Resultaten** naar **Virtuele netwerkgateway** en klik hierop.
2. Klik onder aan de blade Virtuele netwerkgateway op **Maken**. Hiermee opent u de blade **Gateway van het virtuele netwerk maken**.

    ![Velden van de blade Virtuele netwerkgateway maken](./media/vpn-gateway-add-gw-s2s-rm-portal-include/vnet_gw.png "Nieuwe gateway")

3. Vul op de blade **Gateway van het virtuele netwerk maken** de waarden in voor de gateway van het virtuele netwerk.

  - **Naam**: naam van uw gateway. Deze mag niet gelijk zijn aan de naam van het gateway-subnet. Het is de naam van het gateway-object dat u maakt.
  - **Gatewaytype**: selecteer **VPN**. VPN-gateways maken gebruik van een gateway van het virtuele netwerk van het type **VPN**. 
  - **VPN-type**selecteer het VPN-type dat wordt opgegeven voor uw configuratie. De meeste configuraties vereisen een op route gebaseerd VPN-type.
  - **SKU**: selecteer de gateway-SKU in de vervolgkeuzelijst. Welke SKU's worden weergegeven in de vervolgkeuzelijst, is afhankelijk van het VPN-type dat u selecteert. Zie [Gateway-SKU's](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor informatie over gateway-SKU's.
  - **Locatie**: u moet mogelijk omlaag schuiven om Locatie te zien. Wijzig het veld **Locatie** om naar de locatie van het virtuele netwerk te verwijzen. Als de locatie niet verwijst naar de regio waarin het virtuele netwerk zich bevindt, wordt het virtuele netwerk niet weergegeven in de vervolgkeuzelijst Een virtueel netwerk kiezen in de volgende stap.
  - **Virtueel netwerk**: kies het virtuele netwerk waaraan u deze gateway wilt toevoegen. Klik op **Virtueel netwerk** om de blade Een virtueel netwerk kiezen te openen. Selecteer het VNet. Als u uw VNet niet ziet, moet u controleren of het veld Locatie verwijst naar de regio waarin het virtuele netwerk zich bevindt.
  - **Openbaar IP-adres**: op de blade Openbaar IP-adres maken maakt u een openbaar-IP-adres-object. Het openbare IP-adres wordt dynamisch toegewezen wanneer de VPN-gateway wordt gemaakt. VPN Gateway ondersteunt momenteel alleen *dynamische* toewijzing van openbare IP-adressen. Dit betekent echter niet dat het IP-adres wordt gewijzigd nadat het aan uw VPN Gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

    - Klik eerst op **Openbaar IP-adres** om de blade Openbaar IP-adres kiezen te openen. Klik daarna op **+Nieuw** om de blade Openbaar IP-adres maken te openen.
    - Voer vervolgens een **naam** in voor uw openbare IP-adres en klik op **OK** onder aan de blade uw wijzigingen op te slaan.

      ![Openbaar IP maken](./media/vpn-gateway-add-gw-s2s-rm-portal-include/pip.png "PIP maken")

4. Controleer de instellingen. U kunt onder aan de blade **Vastmaken aan dashboard** selecteren, als u wilt dat uw gateway op het dashboard wordt weergegeven. 
5. Klik op **Maken** om de VPN-gateway aan te maken. De instellingen worden gevalideerd en op het dashboard wordt de tegel 'De gateway van het virtuele netwerk implementeren' weergegeven. Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

Nadat de gateway is gemaakt, gaat u na welk IP-adres eraan is toegewezen. Bekijk hiervoor het virtuele netwerk in de portal. De gateway wordt weergegeven als aangesloten apparaat. U kunt op het aangesloten apparaat klikken (uw gateway van het virtuele netwerk) om meer informatie te laten weergeven.
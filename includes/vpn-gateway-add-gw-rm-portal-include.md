1. Klik in de portal aan de linkerkant op **+** en typ in het zoekvak: 'virtuele netwerkgateway'. Klik op het zoekresultaat **Gateway van het virtuele netwerk**. Klik onder aan de pagina **Virtuele netwerkgateway** op **Maken** om de pagina **Virtuele netwerkgateway maken** te openen.
2. Vul op de pagina **Virtuele netwerkgateway maken** de waarden in voor uw virtuele netwerkgateway.

  ![Velden van de pagina Virtuele netwerkgateway maken](./media/vpn-gateway-add-gw-rm-portal-include/gw.png "Velden van de pagina Virtuele netwerkgateway maken")
3. Op de **virtuele netwerkgateway aanmaken** pagina, geeft u de waarden voor uw virtuele netwerkgateway.

  - **Naam**: naam van uw gateway. Deze mag niet gelijk zijn aan de naam van het gateway-subnet. Het is de naam van het gateway-object dat u maakt.
  - **Gatewaytype**: selecteer **VPN**. VPN-gateways maken gebruik van een gateway van het virtuele netwerk van het type **VPN**. 
  - **VPN-type**selecteer het VPN-type dat wordt opgegeven voor uw configuratie. De meeste configuraties vereisen een op route gebaseerd VPN-type.
  - **SKU**: selecteer de gateway-SKU in de vervolgkeuzelijst. Welke SKU's worden weergegeven in de vervolgkeuzelijst, is afhankelijk van het VPN-type dat u selecteert. Zie [Gateway-SKU's](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku) voor informatie over gateway-SKU's.
  - **Locatie**: u moet mogelijk omlaag schuiven om Locatie te zien. Wijzig het veld **Locatie** om naar de locatie van het virtuele netwerk te verwijzen. Als de locatie niet naar de regio waarin het virtuele netwerk zich bevindt verwijst, wanneer u een virtueel netwerk in de volgende stap selecteert, wordt het niet weergegeven in de vervolgkeuzelijst.
  - **Virtueel netwerk**: kies het virtuele netwerk waaraan u deze gateway wilt toevoegen. Klik op **virtueel netwerk** om de pagina 'Kiezen een virtueel netwerk' te openen. Selecteer het VNet. Als u uw VNet niet ziet, moet u controleren of het veld Locatie verwijst naar de regio waarin het virtuele netwerk zich bevindt.
  - **Adresbereik van gatewaysubnet**: alleen ziet u deze instelling als u niet eerder hebt gemaakt een gatewaysubnet voor het virtuele netwerk. Als u een geldig gatewaysubnet eerder hebt gemaakt, wordt deze instelling wordt niet weergegeven.
  - **Eerste IP-configuratie**: de pagina 'Openbare IP-adres kiezen' maakt een openbare IP-adres-object, dat gekoppeld aan de VPN-gateway wordt. Wanneer de VPN-gateway is gemaakt, wordt het openbare IP-adres dynamisch toegewezen aan dit object. VPN Gateway ondersteunt momenteel alleen *dynamische* toewijzing van openbare IP-adressen. Dit betekent echter niet dat het IP-adres wordt gewijzigd nadat het aan uw VPN Gateway is toegewezen. Het openbare IP-adres verandert alleen wanneer de gateway wordt verwijderd en opnieuw wordt gemaakt. Het verandert niet wanneer de grootte van uw VPN Gateway verandert, wanneer deze gateway opnieuw wordt ingesteld of wanneer andere interne onderhoudswerkzaamheden of upgrades worden uitgevoerd.

    - Klik eerst **configuratie van IP-gateway maken** naar de pagina 'Openbare IP-adres kiezen' openen en klik vervolgens op **+ nieuw** om de pagina 'Maak openbaar IP-adres' te openen.
    - Voer vervolgens een **naam** voor uw openbare IP-adres. Laat de SKU als **Basic** tenzij er een specifieke reden om dit te wijzigen in iets anders, klikt u vervolgens op **OK** onder aan deze pagina uw wijzigingen op te slaan.

      ![Openbaar IP maken](./media/vpn-gateway-add-gw-s2s-rm-portal-include/gwip.png "PIP maken")

4. Controleer de instellingen. U kunt selecteren **vastmaken aan dashboard** onder aan de pagina als u wilt dat uw gateway worden weergegeven op het dashboard. 
5. Klik op **Maken** om de VPN-gateway aan te maken. De instellingen worden gevalideerd en ziet u de 'virtuele netwerkgateway implementeren' tegel op het dashboard. Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

Nadat de gateway is gemaakt, gaat u na welk IP-adres eraan is toegewezen. Bekijk hiervoor het virtuele netwerk in de portal. De gateway wordt weergegeven als verbonden apparaat. U kunt op het verbonden apparaat klikken (uw gateway van het virtuele netwerk) om meer informatie te laten weergeven.

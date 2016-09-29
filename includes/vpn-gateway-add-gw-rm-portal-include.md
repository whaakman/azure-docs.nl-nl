1. Ga in de portal naar **Nieuw** > **Netwerken** > **Virtuele netwerkgateway**. Hiermee opent u de blade **Gateway van het virtuele netwerk maken**.

    ![Gateway](./media/vpn-gateway-add-gw-rm-portal-include/creategw250.png)

2. Voer op de blade **Gateway van het virtuele netwerk maken** in het veld **Naam** de naam van de gateway in. Deze mag niet gelijk zijn aan de naam van het gateway-subnet. De naam is de naam van het gateway-object dat u maakt.

3. Wijzig het veld **Locatie** om naar de locatie van het virtuele netwerk te verwijzen. Als u dit niet doet, wordt het virtuele netwerk niet in de VNet-lijst weergegeven.
 
4. Kies vervolgens het virtuele netwerk waaraan u deze gateway wilt toevoegen. Klik op **Virtueel netwerk** om de blade **Een virtueel netwerk kiezen** te openen. Selecteer het VNet. Het VNet moet al een geldig gateway-subnet hebben om in de lijst te worden weergegeven.

5. Kies een openbaar IP-adres. Klik op **Openbaar IP-adres** om de blade **Openbaar IP-adres kiezen** te openen. Klik op **+Nieuwe maken** om de blade **Openbaar IP-adres maken** te openen. Geef een naam op voor uw openbare IP-adres. Hiermee maakt u een openbaar IP-adresobject waaraan een openbaar IP-adres dynamisch worden toegewezen. <br>Klik op **OK** om uw wijzigingen op te slaan.

5. Bij **Gatewaytype** selecteert u het gatewaytype dat wordt opgegeven voor uw configuratie.

6. Bij **VPN-type** selecteert u het VPN-type dat wordt opgegeven voor uw configuratie.

7. Bij **Abonnement** controleert u of het juiste abonnement is geselecteerd.

8. De **resourcegroep** wordt bepaald door het virtuele netwerk dat u selecteert. 

9. Wijzig de **locatie** niet nadat u de bovenstaande instellingen hebt opgegeven. 

10. Op dit moment lijkt de blade op de grafiek in stap 1. Controleer of de instellingen overeenkomen met die van uw eigen configuratie. U kunt onder aan de blade **Vastmaken aan dashboard** selecteren, als u wilt dat uw gateway op het dashboard wordt weergegeven.

11. Klik op **Aanmaken** om de gateway aan te maken. De instellingen worden gevalideerd en op het dashboard wordt de tegel 'De gateway van het virtuele netwerk implementeren' weergegeven. Het aanmaken van een gateway kan tot 45 minuten duren. U moet mogelijk uw portal-pagina vernieuwen om de voltooide status te kunnen zien.

    ![Gateway](./media/vpn-gateway-add-gw-rm-portal-include/deployvnetgw150.png)

11. Nadat de gateway is aangemaakt, kunt u het IP-adres dat eraan is toegewezen bekijken door naar het virtuele netwerk in de portal te kijken. De gateway wordt weergegeven als aangesloten apparaat. U kunt op het aangesloten apparaat klikken (uw virtuele netwerkgateway) om meer informatie te laten weergeven.





<!--HONumber=Sep16_HO3-->



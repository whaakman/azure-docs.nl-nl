Volg onderstaande stappen als u in het Resource Manager-implementatiemodel een VNet wilt maken met behulp van Azure Portal. De schermafbeeldingen dienen alleen als voorbeeld. Zorg dat u de waarden vervangt door die van uzelf. Bekijk het [Virtual Network-overzicht](../articles/virtual-network/virtual-networks-overview.md) voor meer informatie over het gebruik van virtuele netwerken.

1. Navigeer via een browser naar de [Azure Portal](http://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op **+**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **Virtual Network** in de lijst met resultaten en klik erop om de pagina **Virtual Network** te openen.

  ![Pagina voor zoeken van Virtual Network](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/newvnetportal700.png "Pagina voor zoeken van Virtual Network")
3. Ga onder aan de pagina Virtual Network naar de lijst **Selecteer een implementatiemodel**, selecteer de optie **Resource Manager** in de lijst en klik vervolgens op **Maken**.

  ![Resource Manager selecteren](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/resourcemanager250.png "Resource Manager selecteren")
4. Configureer de VNet-instellingen op de pagina **Virtueel netwerk maken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die zijn ingevoerd in het veld, geldig zijn. Mogelijk zijn er waarden die automatisch worden ingevuld. Als dat het geval is, vervangt u die waarden door die van uzelf. De pagina **Virtueel netwerk maken** ziet er ongeveer uit zoals in het volgende voorbeeld:

  ![Veldvalidatie](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/createp2sgvnet.png "Veldvalidatie")
5. **Naam**: geef de naam op voor het virtuele netwerk.
6. **Adresruimte**: voer de adresruimte. Als er meerdere adresruimten moeten worden toegevoegd, voegt u de eerste adresruimte toe. U kunt later, nadat u het VNet hebt gemaakt, extra adresruimten toevoegen.
7. **Subnetnaam**: voeg de naam en het adresbereik van het subnet toe. U kunt later, nadat u het VNet hebt gemaakt, extra subnetten toevoegen.
8. **Abonnement**: controleer of het weergegeven abonnement het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
9. **Resourcegroep**: selecteer een bestaande resourcegroep of maak een nieuwe door een naam te typen voor uw nieuwe resourcegroep. Geef de resourcegroep een naam die aansluit bij de geplande configuratiewaarden wanneer u een nieuwe groep aanmaakt. Zie [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
10. **Locatie**: selecteer de locatie voor uw VNet. De locatie bepaalt waar de resources die u naar dit VNet implementeert, worden opgeslagen.
11. Selecteer **Vastmaken aan dashboard** als u uw VNet gemakkelijk wilt terugvinden op het dashboard en klik vervolgens op **Aanmaken**.

 ![Vastmaken aan dashboard](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/pintodashboard150.png "Vastmaken aan dashboard")
12. Nadat u op **Maken** klikt, ziet u een tegel op uw dashboard die de voortgang van uw VNet aangeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.

  ![Tegel Virtueel netwerk maken](./media/vpn-gateway-basic-p2s-vnet-rm-portal-include/deploying150.png "Tegel Virtueel netwerk maken")
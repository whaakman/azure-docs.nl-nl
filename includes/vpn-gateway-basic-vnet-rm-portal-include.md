Volg de onderstaande stappen om een VNet aan te maken met behulp van de Azure Portal. De schermafbeeldingen dienen alleen als voorbeeld. Zorg dat u de waarden vervangt door die van uzelf. Bekijk het [Virtual Network-overzicht](../articles/virtual-network/virtual-networks-overview.md) voor meer informatie over het gebruik van virtuele netwerken.

1. Navigeer via een browser naar de [Azure Portal](http://portal.azure.com) en log, indien nodig, in met uw Azure-account.
2. Klik op **Nieuw**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **virtueel netwerk** in de geretourneerde lijst en klik hierop om de resourceblade **Virtueel netwerk** te openen.
   
    ![Naar de resourceblade Virtueel netwerk zoeken](./media/vpn-gateway-basic-vnet-rm-portal-include/newvnetportal700.png "Locate virtual network resource blade")
3. Kies, uit de lijst **Selecteer een implementatiemodel** aan de onderzijde van de blade virtueel netwerk, de optie **Resource Manager** en klik vervolgens op **Aanmaken**.

    ![Selecteer Resource Manager](./media/vpn-gateway-basic-vnet-rm-portal-include/resourcemanager250.png "Select Resource Manager")

1. Configureer de VNet-instellingen op de blade **virtueel netwerk aanmaken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die worden ingevoerd in het veld, geldig zijn.
   
    ![Velden valideren](./media/vpn-gateway-basic-vnet-rm-portal-include/checkmark300.png "Field validation")
2. De blade **Virtueel netwerk maken** ziet er ongeveer uit zoals in het volgende voorbeeld. Mogelijk zijn er waarden die automatisch worden ingevuld. Als dat het geval is, vervangt u die waarden door die van uzelf.
   
    ![Een virtuele netwerkblade aanmaken](./media/vpn-gateway-basic-vnet-rm-portal-include/createvnet300.png "Create virtual network blade")
3. **Naam**: geef de naam op voor het virtuele netwerk.
4. **Adresruimte**: voer de adresruimte. Als er meerdere adresruimten moeten worden toegevoegd, voegt u de eerste adresruimte toe. U kunt later, nadat u het VNet hebt gemaakt, extra adresruimten toevoegen.
5. **Subnetnaam**: voeg de naam en het adresbereik van het subnet toe. U kunt later, nadat u het VNet hebt gemaakt, extra subnetten toevoegen.
6. **Abonnement**: controleer of het weergegeven abonnement het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
7. **Resourcegroep**: selecteer een bestaande resourcegroep of maak een nieuwe door een naam te typen voor uw nieuwe resourcegroep. Geef de resourcegroep een naam die aansluit bij de geplande configuratiewaarden wanneer u een nieuwe groep aanmaakt. Zie [Azure Resource Manager Overview](../articles/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
8. **Locatie**: selecteer de locatie voor uw VNet. De locatie bepaalt waar de resources die u naar dit VNet implementeert, worden opgeslagen.
9. Selecteer **Vastmaken aan dashboard** als u uw VNet gemakkelijk wilt terugvinden op het dashboard en klik vervolgens op **Aanmaken**.
   
   ![Vastmaken aan dashboard](./media/vpn-gateway-basic-vnet-rm-portal-include/pintodashboard150.png "pin to dashboard")
10. Nadat u op **Maken** klikt, ziet u een tegel op uw dashboard die de voortgang van uw VNet aangeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.
    
    ![Tegel Virtueel netwerk aanmaken](./media/vpn-gateway-basic-vnet-rm-portal-include/deploying150.png "Creating virtual network tile")

<!--HONumber=Oct16_HO1-->



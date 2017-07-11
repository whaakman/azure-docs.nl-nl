Volg onderstaande stappen als u in het Resource Manager-implementatiemodel een VNet wilt maken met behulp van Azure Portal. Gebruik de [voorbeeldwaarden](#values) als u deze stappen uitvoert als onderdeel van een zelfstudie. Als u deze stappen niet uitvoert als onderdeel van een zelfstudie, vervangt u de waarden door die van uzelf. Bekijk het [Virtual Network-overzicht](../articles/virtual-network/virtual-networks-overview.md) voor meer informatie over het gebruik van virtuele netwerken.

1. Open een browser, ga naar [Azure Portal](http://portal.azure.com) en meld u aan met uw Azure-account.
2. Klik op **Nieuw**. In het veld **Marketplace doorzoeken** typt u 'virtueel netwerk'. Zoek **virtueel netwerk** in de geretourneerde lijst en klik hierop om de resourceblade **Virtueel netwerk** te openen.
3. Kies, uit de lijst **Selecteer een implementatiemodel** aan de onderzijde van de blade virtueel netwerk, de optie **Resource Manager** en klik vervolgens op **Aanmaken**. Hiermee opent u de blade Virtueel netwerk maken.

    ![Blade Virtueel netwerk maken](./media/vpn-gateway-basic-vnet-s2s-rm-portal-include/createvnet.png "Blade Virtueel netwerk maken")
4. Configureer de VNet-instellingen op de blade **virtueel netwerk aanmaken**. Wanneer u de velden invult, verandert het rode uitroepteken in een groen vinkje als de tekens die zijn ingevoerd in het veld, geldig zijn.

  - **Naam**: geef de naam op voor het virtuele netwerk. In dit voorbeeld wordt de naam TestVNet1 gebruikt.
  - **Adresruimte**: voer de adresruimte. Als er meerdere adresruimten moeten worden toegevoegd, voegt u de eerste adresruimte toe. U kunt later, nadat u het VNet hebt gemaakt, extra adresruimten toevoegen. Zorg ervoor dat de adresruimte die u opgeeft, niet overlapt met de adresruimte voor uw on-premises locatie.
  - **Subnetnaam**: voeg de naam en het adresbereik van het eerste subnet toe. U kunt extra subnetten en het gatewaysubnet later toevoegen, na het maken van dit VNet. 
  - **Abonnement**: controleer of het weergegeven abonnement het juiste is. U kunt abonnementen wijzigen met behulp van de vervolgkeuzelijst.
  - **Resourcegroep**: selecteer een bestaande resourcegroep of maak een nieuwe door een naam te typen voor uw nieuwe resourcegroep. Geef de resourcegroep een naam die aansluit bij de geplande configuratiewaarden wanneer u een nieuwe groep aanmaakt. Zie [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
  - **Locatie**: selecteer de locatie voor uw VNet. De locatie bepaalt waar de resources die u naar dit VNet implementeert, worden opgeslagen.

5. Selecteer **Vastmaken aan dashboard** als u uw VNet gemakkelijk wilt terugvinden op het dashboard en klik vervolgens op **Aanmaken**. Nadat u op **Maken** klikt, ziet u een tegel op uw dashboard die de voortgang van uw VNet aangeeft. De tegel wordt gewijzigd wanneer het VNet wordt gemaakt.
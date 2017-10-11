## <a name="how-to-create-a-classic-vnet-in-the-azure-portal"></a>Het maken van een klassiek VNet en in de Azure portal
Volg de onderstaande stappen voor het maken van een klassiek VNet en op basis van de bovenstaande scenario.

1. Navigeer via een browser naar http://portal.azure.com en log, indien nodig, in met uw Azure-account.
2. Klik op **nieuw** > **Networking** > **virtueel netwerk**, zoals u ziet de **een implementatiemodel selecteren** lijst al staat **klassieke**, en klik vervolgens op **maken**, zoals in de afbeelding hieronder.
   
    ![VNet aanmaken in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure1.gif)
3. Op de **virtueel netwerk** blade, typ de **naam** van het VNet en klik op **adresruimte**. Uw adresruimte instellingen configureren voor het VNet en het eerste subnet en klik vervolgens op **OK**. De onderstaande afbeelding ziet u de CIDR-blok-instellingen voor ons scenario.
   
    ![Blade adresruimte](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure2.png)
4. Klik op **resourcegroep** en selecteer een resourcegroep om toe te voegen van het VNet of klik op **nieuwe resourcegroep maken** het VNet toevoegen aan een nieuwe resourcegroep. In de onderstaande afbeelding worden de instellingen voor de resourcegroep genaamd **TestRG** weergegeven. Zie [Azure Resource Manager Overview](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (Overzicht van Azure Resource Manager) voor meer informatie over resourcegroepen.
   
    ![Resourcegroepblade maken](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure3.png)
5. Wijzig zo nodig de instellingen van het **Abonnement** en de **Locatie** voor uw VNet. 
6. Als u VNet niet als tegel wilt laten weergeven in het **Startboard** schakelt u **Vastmaken aan Startboard** uit. 
7. Klik op **Aanmaken** en let op de tegel met de naam **Virtueel netwerk aanmaken** die in de onderstaande afbeelding wordt weergegeven.
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure4.png)
8. Wacht tot het VNet worden gemaakt en wanneer u de tegel hieronder ziet, klikt u op om toe te voegen meer subnetten.
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure5.png)
9. U ziet de **configuratie** voor uw VNet zoals hieronder wordt weergegeven. 
   
    ![VNet aanmaken in portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure6.png)
10. Klik op **subnetten** > **toevoegen**, typ een **naam** en geef een **-adresbereik (CIDR-blok)** voor uw subnet, en vervolgens Klik op **OK**. De afbeelding hieronder ziet de instellingen voor onze huidige scenario.
    
    ![VNet aanmaken in Azure-portal](./media/virtual-networks-create-vnet-classic-pportal-include/vnet-create-pportal-figure7.gif)


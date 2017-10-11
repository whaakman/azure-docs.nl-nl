


## <a name="tagging-a-virtual-machine-through-templates"></a>Labels van een virtuele Machine via sjablonen
Eerst gaan we kijken tagging via sjablonen. [Deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags) plaatst labels op de volgende bronnen: Compute (virtuele Machine) en opslag (Storage-Account) netwerk (openbare IP-adres, virtuele netwerk en Network Interface). Deze sjabloon is voor een virtuele machine van Windows, maar kan worden aangepast voor virtuele Linux-machines.

Klik op de **implementeren in Azure** knop van de [sjabloonkoppeling](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-tags). Dit, gaat u naar de [Azure-portal](https://portal.azure.com/) waarin u deze sjabloon kunt implementeren.

![Eenvoudige implementatie met labels](./media/virtual-machines-common-tag/deploy-to-azure-tags.png)

Deze sjabloon bevat de volgende codes: *afdeling*, *toepassing*, en *gemaakt door*. U kunt toevoegen/bewerken deze tags rechtstreeks in de sjabloon indien andere tagnaam gewenst.

![Azure labels in een sjabloon](./media/virtual-machines-common-tag/azure-tags-in-a-template.png)

Zoals u ziet, worden de labels worden gedefinieerd als sleutel-waardeparen, gescheiden door een dubbele punt (:). De labels moeten worden gedefinieerd in deze indeling:

        “tags”: {
            “Key1” : ”Value1”,
            “Key2” : “Value2”
        }

Sla het sjabloonbestand nadat u deze bewerken met de labels van uw keuze.

Vervolgens gaat u naar de **Parameters bewerken** sectie, kunt u de waarden invullen voor de labels.

![Labels bewerken in Azure-portal](./media/virtual-machines-common-tag/edit-tags-in-azure-portal.png)

Klik op **maken** om deze sjabloon met de waarden van uw code te implementeren.

## <a name="tagging-through-the-portal"></a>Via de Portal-tagging
Na het maken van uw resources met labels, kunt u weergeven, toevoegen en verwijderen van de labels in de portal.

Selecteer het pictogram labels de labels weergeven:

![Pictogram van de labels in Azure-portal](./media/virtual-machines-common-tag/azure-portal-tags-icon.png)

Een nieuwe code via de portal door te definiëren van uw eigen sleutel-waardepaar toevoegt en sla het.

![Nieuw label toevoegen in Azure-portal](./media/virtual-machines-common-tag/azure-portal-add-new-tag.png)

Uw nieuwe code wordt nu weergegeven in de lijst met labels voor uw resource.

![Nieuwe code opgeslagen in Azure-portal](./media/virtual-machines-common-tag/azure-portal-saved-new-tag.png)


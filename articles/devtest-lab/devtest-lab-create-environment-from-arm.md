---
title: Maken van meerdere VM-omgevingen en PaaS-resources met Azure Resource Manager-sjablonen | Microsoft Docs
description: Meer informatie over het maken van meerdere VM-omgevingen en PaaS-resources in Azure DevTest Labs van een Azure Resource Manager-sjabloon
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: craigcaseyMSFT
manager: douge
editor: 
ms.assetid: 
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/31/2017
ms.author: v-craic
ms.openlocfilehash: 55a6c5cd5a0544b297bb68841c5ff0314f48dcc9
ms.sourcegitcommit: 562a537ed9b96c9116c504738414e5d8c0fd53b1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Meerdere VM-omgevingen en PaaS-resources met Azure Resource Manager-sjablonen maken

De [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) kunt u eenvoudig [maken en een virtuele machine toevoegen aan een lab](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm). Dit werkt goed bij het maken van een virtuele machine tegelijk. Als de omgeving meerdere virtuele machines bevat, moet elke virtuele machine echter worden afzonderlijk gemaakt. Voor scenario's zoals een meerlaagse Web-app of een SharePoint-farm is een mechanisme om toe te staan voor het maken van meerdere virtuele machines in één stap nodig. Met behulp van Azure Resource Manager-sjablonen, kunt u nu de infrastructuur en configuratie van uw Azure-oplossing definiëren en herhaaldelijk implementeren die meerdere virtuele machines in een consistente status. Deze functie biedt de volgende voordelen:

- Azure Resource Manager-sjablonen zijn geladen rechtstreeks vanuit uw resourcebeheerbibliotheek (GitHub of Team Services Git).
- Zodra geconfigureerd, uw gebruikers een omgeving maken door het verzamelen van een Azure Resource Manager-sjabloon uit de Azure portal, net als bij andere soorten [VM basissen](./devtest-lab-comparing-vm-base-image-types.md).
- Azure PaaS-resources kunnen worden ingericht in een omgeving met een Azure Resource Manager-sjabloon naast IaaS VM's.
- De kosten van omgevingen kan worden bijgehouden in de testomgeving naast afzonderlijke virtuele machines die zijn gemaakt door andere soorten basissen.
- PaaS-resources zijn gemaakt en wordt weergegeven in kosten bijhouden; virtuele machine automatisch afsluiten geldt echter niet voor PaaS-resources.
- Gebruikers hebben de dezelfde VM beleidscontrole voor omgevingen zoals ze voor één lab virtuele machines hebben.

Meer informatie over de vele [voordelen van het gebruik van Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) om te implementeren, bijwerken of verwijderen van al uw lab-resources in één bewerking.

> [!NOTE]
> Wanneer u een Resource Manager-sjabloon als basis gebruikt meer lab VM's maken, zijn er enkele verschillen rekening moet houden of meerdere virtuele machines of één voor virtuele machines te maken. [Gebruik Azure Resource Manager-sjabloon voor een virtuele machine](devtest-lab-use-resource-manager-template.md) deze verschillen in meer detail uitgelegd.
>
>

## <a name="configure-azure-resource-manager-template-repositories"></a>Azure Resource Manager-sjabloon opslagplaatsen configureren

Als een van de aanbevolen procedures met infrastructuur als code en configuratie als code, moeten de omgeving sjablonen worden beheerd in broncodebeheer. Azure DevTest Labs volgt deze praktijk en laadt alle sjablonen voor Azure Resource Manager rechtstreeks vanuit GitHub of VSTS Git-opslagplaatsen. Als gevolg hiervan kunnen Resource Manager-sjablonen worden gebruikt in de releasecyclus van een volledige, vanuit de testomgeving naar de productieomgeving.

Er zijn een paar regels volgen om u te ordenen van uw Azure Resource Manager-sjablonen in een opslagplaats:

- Het master sjabloonbestand moet de naam `azuredeploy.json`. 

    ![De sjabloonbestanden sleutel Azure Resource Manager](./media/devtest-lab-create-environment-from-arm/master-template.png)

- Als u gebruiken in een parameterbestand gedefinieerde parameterwaarden wilt, het parameterbestand moet worden benoemd `azuredeploy.parameters.json`.
- U kunt de parameters `_artifactsLocation` en `_artifactsLocationSasToken` kan de waarde van de URI parametersLink, zodat u DevTest Labs automatisch geneste om sjablonen te beheren. Zie [hoe Azure DevTest Labs gemakkelijker geneste Resource Manager sjabloonimplementaties te maken voor testomgevingen](https://blogs.msdn.microsoft.com/devtestlab/2017/05/23/how-azure-devtest-labs-makes-nested-arm-template-deployments-easier-for-testing-environments/) voor meer informatie.
- Geef de weergavenaam van sjabloon en de beschrijving kan metagegevens worden gedefinieerd. Deze metagegevens moet zich in een bestand met de naam `metadata.json`. Het metagegevensbestand van de volgende voorbeeld ziet u hoe de weergavenaam en beschrijving opgeven: 

```json
{
 
"itemDisplayName": "<your template name>",
 
"description": "<description of the template>"
 
}
```

De volgende stappen helpen u bij een opslagplaats toe te voegen aan uw testomgeving met behulp van de Azure-portal. 

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer de gewenste testomgeving uit de lijst van labs.   
1. Op de testomgeving **overzicht** deelvenster **configuratie en het beleid**.

    ![Configuratie en het beleid](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Van de **configuratie en het beleid** lijst met instellingen, selecteer **opslagplaatsen**. De **opslagplaatsen** deelvenster toont de opslagplaatsen die zijn toegevoegd aan het lab. Een opslagplaats met de naam `Public Repo` automatisch is gegenereerd voor alle labs en verbinding maakt met de [DevTest Labs GitHub-repo-](https://github.com/Azure/azure-devtestlab) die bevat verschillende VM artefacten voor het gebruik.

    ![Openbare opslagplaats](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Selecteer **toevoegen +** uw opslagplaats Azure Resource Manager-sjabloon toevoegen.
1. Wanneer de tweede **opslagplaatsen** deelvenster geopend, voer de benodigde gegevens als volgt:
    - **Naam** -Voer de naam van de opslagplaats die wordt gebruikt in de testomgeving.
    - **GIT kloon-URL** -GIT HTTPS-kloon-URL invoeren in GitHub of Visual Studio Team Services.  
    - **Vertakking** -Voer de naam van de filialen voor toegang tot uw Azure Resource Manager-Sjabloondefinities. 
    - **Persoonlijke toegangstoken** -het persoonlijke toegangstoken veilig toegang krijgen tot uw opslagplaats wordt gebruikt. Als u uw token vanuit Visual Studio Team Services, selecteer  **&lt;uwnaam >> Mijn profiel > Beveiliging > openbare toegangstoken**. Als u uw token vanuit GitHub, selecteer uw avatar gevolgd door het selecteren van **instellingen > openbare toegangstoken**. 
    - **Paden voor mappen** -met een van de twee invoervelden, geeft u het mappad die begint met een slash - / - en is ten opzichte van de Git-kloon-URI bij uw artefactdefinities (eerste invoerveld) of de Sjabloondefinities van uw Azure Resource Manager-.   
    
        ![Openbare opslagplaats](./media/devtest-lab-create-environment-from-arm/repo-values.png)


1. Zodra de vereiste velden zijn ingevoerd, en de zijn gevalideerd, selecteert u **opslaan**.

De volgende sectie helpt u bij het maken van omgevingen van een Azure Resource Manager-sjabloon.

## <a name="create-an-environment-from-a-resource-manager-template-using-the-azure-portal"></a>Maken van een omgeving met een Resource Manager-sjabloon met de Azure portal

Als een opslagplaats Azure Resource Manager-sjabloon is geconfigureerd in de testomgeving, kunnen uw lab-gebruikers een omgeving met behulp van Azure portal met de volgende stappen maken:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer de gewenste testomgeving uit de lijst van labs.   
1. Selecteer op de testomgeving deelvenster **toevoegen +**.
1. De **kiezen een base** deelvenster ziet u de basisinstallatiekopieën kunt u met de Azure Resource Manager-sjablonen eerst weergegeven. Selecteer de gewenste Azure Resource Manager-sjabloon.

    ![Kies een base](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Op de **toevoegen** deelvenster, voer de **omgevingsnaam** waarde. Naam van de omgeving is wat aan uw gebruikers in de testomgeving wordt weergegeven. De resterende invoervelden zijn gedefinieerd in de Azure Resource Manager-sjabloon. Als de standaardwaarden zijn gedefinieerd in de sjabloon of het `azuredeploy.parameter.json` bestand aanwezig is, de standaardwaarden worden weergegeven in de invoervelden. Voor de parameters van het type *tekenreeks secure*, kunt u de geheimen die zijn opgeslagen in de testomgeving [persoonlijke archief van de geheime](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Deelvenster toevoegen](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Er zijn verschillende parameterwaarden die - zelfs als opgegeven - als lege waarden worden weergegeven. Daarom als deze waarden gebruikers aan parameters in een Azure Resource Manager-sjabloon toewijzen, DevTest Labs heeft niet de waarden weergeven. In plaats daarvan leeg invoervelden weergegeven waar lab gebruikers een waarde moeten opgeven bij het maken van de omgeving.
    > 
    > - GEN UNIEKE
    > - -UNIEKE - GEN [N]
    > - GEN-SSH-PUB-KEY
    > - GEN-WACHTWOORD 
 
1. Selecteer **toevoegen** voor het maken van de omgeving. De omgeving wordt gestart onmiddellijk inrichting met het weergeven van de status in de **mijn virtuele machines** lijst. Een nieuwe resourcegroep wordt automatisch gemaakt door de testomgeving voor het inrichten van de resources die zijn gedefinieerd in de Azure Resource Manager-sjabloon.
1. Zodra de omgeving is gemaakt, selecteer de omgeving in de **mijn virtuele machines** lijst opent u het deelvenster van de groep resource en bladert u alle resources in de omgeving is ingericht.
    
    ![De lijst met virtuele machines](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   U kunt ook de omgeving zodanig dat alleen de lijst met virtuele machines die zijn ingericht in de omgeving weergeven uitbreiden.
    
    ![De lijst met virtuele machines](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Klik op een van de omgevingen om weer te geven van de beschikbare acties - zoals het toepassen van artefacten, gegevensschijven veranderende automatisch afsluiten en tijd meer koppelen.

    ![Omgeving acties](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="deploy-a-resource-manager-template-to-create-a-vm"></a>Een Resource Manager-sjabloon voor het maken van een virtuele machine implementeren
Nadat u hebt opgeslagen van Resource Manager-sjabloon en het is afgestemd op uw behoeften, kunt u het maken van VM automatiseren. 
- [Implementeren van resources met Resource Manager-sjablonen en Azure PowerShell](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy) wordt beschreven hoe u Azure PowerShell gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources in Azure. 
- [Implementeren van resources met Resource Manager-sjablonen en Azure CLI](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-template-deploy-cli) wordt beschreven hoe u Azure CLI gebruiken met Resource Manager-sjablonen voor het implementeren van uw resources in Azure.

> [!NOTE]
> Alleen een gebruiker met machtigingen van de eigenaar lab kunt virtuele machines maken van een Resource Manager-sjabloon met behulp van Azure PowerShell. Als u wilt automatiseren met behulp van een Resource Manager-sjabloon maken van VM en u alleen gebruikersmachtigingen hebt, kunt u de [ **az lab vm maken** opdracht in de CLI](https://docs.microsoft.com/cli/azure/lab/vm#az_lab_vm_create).

### <a name="general-limitations"></a>Algemene beperkingen 

Overweeg deze beperkingen bij het gebruik van Resource Manager-sjabloon in DevTest Labs:

- Een Resource Manager-sjabloon die u maakt mag niet verwijzen naar bestaande resources; het kan alleen verwijzen naar nieuwe resources. Worden bovendien, als u een bestaande Resource Manager-sjabloon die u doorgaans buiten DevTest Labs implementeert hebt en deze verwijzingen naar bestaande bronnen bevat, deze kan niet gebruikt in de testomgeving.

   De enige uitzondering hierop is dat u **kunt** verwijzen naar een bestaand virtueel netwerk. 

- Formules kunnen niet worden gemaakt van lab virtuele machines die zijn gemaakt op basis van een Resource Manager-sjabloon. 

- Aangepaste installatiekopieën kunnen niet worden gemaakt van lab virtuele machines die zijn gemaakt op basis van een Resource Manager-sjabloon. 

- De meeste beleidsregels zijn niet bij het implementeren van Resource Manager-sjablonen geëvalueerd.

   U kunt bijvoorbeeld wellicht een lab beleid opgeven dat een gebruiker alleen vijf virtuele machines kunt maken. Echter, als de gebruiker met een Resource Manager-sjabloon die wordt gemaakt van talloze van virtuele machines implementeert, die is toegestaan. Beleidsregels die niet zijn geëvalueerd zijn onder andere:

   - Aantal virtuele machines per gebruiker
   - Aantal premium virtuele machines per lab-gebruiker
   - Aantal premium-schijven per lab-gebruiker


### <a name="configure-environment-resource-group-access-rights-for-lab-users"></a>Omgeving resource group-toegangsrechten voor gebruikers lab configureren

Lab gebruikers kunnen Resource Manager-sjabloon implementeren. Maar standaard beschikken over leestoegang, wat betekent dat ze de resources in een omgeving resourcegroep niet wijzigen. Bijvoorbeeld, mag niet stoppen of starten van hun bronnen.

Volg deze stappen zodat gebruikers op uw lab Inzender toegangsrechten. Vervolgens, wanneer ze een Resource Manager-sjabloon implementeren, ze zich voor het bewerken van de resources in deze omgeving. 


1. Op uw lab **overzicht** deelvenster **configuratie en het beleid**.
1. Selecteer **Lab instellingen**.
1. Selecteer in het deelvenster Lab instellingen **Inzender** schrijven om machtigingen te verlenen aan gebruikers van de testomgeving.

    ![Toegangsrechten lab-gebruiker configureren](./media/devtest-lab-create-environment-from-arm/configure-access-rights.png)

1. Selecteer **Opslaan**.

## <a name="next-steps"></a>Volgende stappen
* Wanneer een virtuele machine is gemaakt, kunt u verbinden met de virtuele machine door te selecteren **Connect** op de VM management deelvenster.
* Weergeven en beheren van resources in een omgeving met het selecteren van de omgeving in de **mijn virtuele machines** lijst in uw testomgeving. 
* Verken de [Azure Resource Manager-sjablonen uit Azure sjabloon snelstartgalerie](https://github.com/Azure/azure-quickstart-templates).

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
ms.openlocfilehash: 3fd0f15c695bcd22a51233846ace8711a4fcd635
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
# <a name="create-multi-vm-environments-and-paas-resources-with-azure-resource-manager-templates"></a>Meerdere VM-omgevingen en PaaS-resources met Azure Resource Manager-sjablonen maken

De [Azure-portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) kunt u eenvoudig [maken en een virtuele machine toevoegen aan een lab](https://docs.microsoft.com/azure/devtest-lab/devtest-lab-add-vm). Dit werkt goed bij het maken van een virtuele machine tegelijk. Als de omgeving meerdere virtuele machines bevat, heeft elke VM echter afzonderlijk worden gemaakt. Voor scenario's zoals een meerlaagse Web-app of een SharePoint-farm is een mechanisme om toe te staan voor het maken van meerdere virtuele machines in één stap nodig. Met behulp van Azure Resource Manager-sjablonen, kunt u nu de infrastructuur en configuratie van uw Azure-oplossing definiëren en herhaaldelijk implementeren die meerdere virtuele machines in een consistente status. Deze functie biedt de volgende voordelen:

- Azure Resource Manager-sjablonen zijn geladen rechtstreeks vanuit uw resourcebeheerbibliotheek (GitHub of Team Services Git).
- Na de configuratie, uw gebruikers een omgeving maken door het verzamelen van een Azure Resource Manager-sjabloon uit de Azure portal als wat ze met andere typen doen kunnen [VM basissen](./devtest-lab-comparing-vm-base-image-types.md).
- Azure PaaS-resources kunnen worden ingericht in een omgeving met een Azure Resource Manager-sjabloon naast IaaS VM's.
- De kosten van omgevingen kan worden bijgehouden in de testomgeving naast afzonderlijke virtuele machines die zijn gemaakt door andere soorten basissen.
- PaaS-resources zijn gemaakt en wordt weergegeven in kosten bijhouden; virtuele machine automatisch afsluiten geldt echter niet voor PaaS-resources.
- Gebruikers hebben de dezelfde VM beleidscontrole voor omgevingen zoals ze voor één lab virtuele machines hebben.

Meer informatie over de vele [voordelen van het gebruik van Resource Manager-sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#the-benefits-of-using-resource-manager) om te implementeren, bijwerken of verwijderen van al uw lab-resources in één bewerking.

> [!NOTE]
> Wanneer u een Resource Manager-sjabloon als basis gebruikt meer lab VM's maken, zijn er enkele verschillen rekening moet houden of meerdere virtuele machines of één voor virtuele machines te maken. Een virtuele machine gebruiken Azure Resource Manager-sjabloon wordt deze verschillen in meer detail uitgelegd.
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
1. Selecteer op de labblade **configuratie en het beleid**.

    ![Configuratie en het beleid](./media/devtest-lab-create-environment-from-arm/configuration-and-policies-menu.png)

1. Van de **configuratie en het beleid** lijst met instellingen, selecteer **opslagplaatsen**. De **opslagplaatsen** blade bevat de opslagplaatsen die zijn toegevoegd aan het lab. Een opslagplaats met de naam `Public Repo` automatisch is gegenereerd voor alle labs en verbinding maakt met de [DevTest Labs GitHub-repo-](https://github.com/Azure/azure-devtestlab) die bevat verschillende VM artefacten voor het gebruik.

    ![Openbare opslagplaats](./media/devtest-lab-create-environment-from-arm/public-repo.png)

1. Selecteer **toevoegen +** uw opslagplaats Azure Resource Manager-sjabloon toevoegen.
1. Wanneer de tweede **opslagplaatsen** blade wordt geopend, voer de benodigde gegevens als volgt:
    - **Naam** -Voer de naam van de opslagplaats die wordt gebruikt in de testomgeving.
    - **GIT kloon-URL** -GIT HTTPS-kloon-URL invoeren in GitHub of Visual Studio Team Services.  
    - **Vertakking** -Voer de naam van de filialen voor toegang tot uw Azure Resource Manager-Sjabloondefinities. 
    - **Persoonlijke toegangstoken** -het persoonlijke toegangstoken veilig toegang krijgen tot uw opslagplaats wordt gebruikt. Als u uw token vanuit Visual Studio Team Services, selecteer  **&lt;uwnaam >> Mijn profiel > Beveiliging > openbare toegangstoken**. Als u uw token vanuit GitHub, selecteer uw avatar gevolgd door het selecteren van **instellingen > openbare toegangstoken**. 
    - **Paden voor mappen** -met een van de twee invoervelden, geeft u het mappad die begint met een slash - / - en is ten opzichte van de Git-kloon-URI bij uw artefactdefinities (eerste invoerveld) of de Sjabloondefinities van uw Azure Resource Manager-.   
    
        ![Openbare opslagplaats](./media/devtest-lab-create-environment-from-arm/repo-values.png)

1. Zodra de vereiste velden zijn ingevoerd, en de zijn gevalideerd, selecteert u **opslaan**.

De volgende sectie helpt u bij het maken van omgevingen van een Azure Resource Manager-sjabloon.

## <a name="create-an-environment-from-an-azure-resource-manager-template"></a>Een omgeving met een Azure Resource Manager-sjabloon maken

Als een opslagplaats Azure Resource Manager-sjabloon is geconfigureerd in de testomgeving, kunnen uw lab-gebruikers een omgeving met behulp van Azure portal met de volgende stappen maken:

1. Meld u aan bij [Azure Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040).
1. Selecteer **meer Services**, en selecteer vervolgens **DevTest Labs** uit de lijst.
1. Selecteer de gewenste testomgeving uit de lijst van labs.   
1. Selecteer op de labblade **toevoegen +**.
1. De **kiezen een base** blade geeft de basisinstallatiekopieën kunt u met de Azure Resource Manager-sjablonen eerst weergegeven. Selecteer de gewenste Azure Resource Manager-sjabloon.

    ![Kies een base](./media/devtest-lab-create-environment-from-arm/choose-a-base.png)
  
1. Op de **toevoegen** blade, voer de **omgevingsnaam** waarde. Naam van de omgeving is wat aan uw gebruikers in de testomgeving wordt weergegeven. De resterende invoervelden zijn gedefinieerd in de Azure Resource Manager-sjabloon. Als de standaardwaarden zijn gedefinieerd in de sjabloon of het `azuredeploy.parameter.json` bestand aanwezig is, de standaardwaarden worden weergegeven in de invoervelden. Voor de parameters van het type *tekenreeks secure*, kunt u de geheimen die zijn opgeslagen in de testomgeving [persoonlijke archief van de geheime](https://azure.microsoft.com/en-us/updates/azure-devtest-labs-keep-your-secrets-safe-and-easy-to-use-with-the-new-personal-secret-store).

    ![Blade toevoegen](./media/devtest-lab-create-environment-from-arm/add.png)

    > [!NOTE]
    > Er zijn verschillende parameterwaarden die - zelfs als opgegeven - als lege waarden worden weergegeven. Daarom als deze waarden gebruikers aan parameters in een Azure Resource Manager-sjabloon toewijzen, worden DevTest Labs niet weergegeven de waarden; in plaats daarvan weergegeven leeg invoervelden waar de lab-gebruikers moeten een waarde invoeren bij het maken van de omgeving.
    > 
    > - GEN UNIEKE
    > - -UNIEKE - GEN [N]
    > - GEN-SSH-PUB-KEY
    > - GEN-WACHTWOORD 
 
1. Selecteer **toevoegen** voor het maken van de omgeving. De omgeving wordt gestart onmiddellijk inrichting met het weergeven van de status in de **mijn virtuele machines** lijst. Een nieuwe resourcegroep wordt automatisch gemaakt door de testomgeving voor het inrichten van de resources die zijn gedefinieerd in de Azure Resource Manager-sjabloon.
1. Zodra de omgeving is gemaakt, selecteer de omgeving in de **mijn virtuele machines** lijst open de resourceblade van de groep en bladert u alle resources in de omgeving is ingericht.
    
    ![De lijst met virtuele machines](./media/devtest-lab-create-environment-from-arm/all-environment-resources.png)
   
   U kunt ook de omgeving zodanig dat alleen de lijst met virtuele machines die zijn ingericht in de omgeving weergeven uitbreiden.
    
    ![De lijst met virtuele machines](./media/devtest-lab-create-environment-from-arm/my-vm-list.png)

1. Klik op een van de omgevingen om weer te geven van de beschikbare acties - zoals het toepassen van artefacten, gegevensschijven veranderende automatisch afsluiten en tijd meer koppelen.

    ![Omgeving acties](./media/devtest-lab-create-environment-from-arm/environment-actions.png)

## <a name="next-steps"></a>Volgende stappen
* Wanneer een virtuele machine is gemaakt, kunt u verbinden met de virtuele machine door te selecteren **Connect** op de blade van de VM.
* Weergeven en beheren van resources in een omgeving met het selecteren van de omgeving in de **mijn virtuele machines** lijst in uw testomgeving. 
* Verken de [Azure Resource Manager-sjablonen uit Azure sjabloon snelstartgalerie](https://github.com/Azure/azure-quickstart-templates)

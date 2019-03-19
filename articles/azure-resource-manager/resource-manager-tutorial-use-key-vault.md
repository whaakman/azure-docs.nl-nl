---
title: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie | Microsoft Docs
description: Ontdek hoe u Azure Key Vault gebruikt om veilig parameterwaarden door te geven tijdens het implementeren van Resource Manager-sjablonen
services: azure-resource-manager
documentationcenter: ''
author: mumian
manager: dougeby
editor: tysonn
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.date: 03/04/2019
ms.topic: tutorial
ms.author: jgao
ms.custom: seodec18
ms.openlocfilehash: 50882fc383d4dfab85b5afb919b24c7940eb5ee0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58122738"
---
# <a name="tutorial-integrate-azure-key-vault-in-resource-manager-template-deployment"></a>Zelfstudie: Azure Key Vault integreren in de Resource Manager-sjabloonimplementatie

Ontdek hoe u geheime waarden uit Azure Key Vault ophaalt en deze als parameters doorgeeft tijdens de implementatie van Resource Manager. De waarde zelf wordt nooit getoond, omdat u alleen verwijst naar de sleutelkluis-id. Zie [Azure Key Vault gebruiken om veilig parameterwaarden door te geven tijdens de implementatie](./resource-manager-keyvault-parameter.md) voor meer informatie

In de zelfstudie [Resource-implementatievolgorde instellen](./resource-manager-tutorial-create-templates-with-dependent-resources.md) maakt u een virtuele machine, een virtueel netwerk en enkele andere afhankelijke resources. In deze zelfstudie past u de sjabloon zodanig aan dat het beheerderswachtwoord van de virtuele machine wordt opgehaald uit eens sleutelkluis.

![Diagram van Resource Manager sjabloon Key Vault-integratie](./media/resource-manager-tutorial-use-key-vault/resource-manager-template-key-vault-diagram.png)

Deze zelfstudie bestaat uit de volgende taken:

> [!div class="checklist"]
> * Een sleutelkluis voorbereiden
> * Een quickstartsjabloon openen
> * Het parameterbestand bewerken
> * De sjabloon implementeren
> * De implementatie valideren
> * Resources opschonen

Als u geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="prerequisites"></a>Vereisten

Als u dit artikel wilt voltooien, hebt u het volgende nodig:

* [Visual Studio Code](https://code.visualstudio.com/) met de [extensie Resource Manager Tools](./resource-manager-quickstart-create-templates-use-visual-studio-code.md#prerequisites).
* Gebruik een gegenereerd wachtwoord voor het beheerdersaccount van de virtuele machine om de beveiliging te verhogen. Hier volgt een voorbeeld voor het genereren van een wachtwoord:

    ```azurecli-interactive
    openssl rand -base64 32
    ```
    Controleer of het gegenereerde wachtwoord voldoet aan de wachtwoordvereisten voor virtuele machines. Elke Azure-service heeft eigen wachtwoordvereisten. Zie [Wat zijn de wachtwoordvereisten bij het maken van een virtuele machine?](../virtual-machines/windows/faq.md#what-are-the-password-requirements-when-creating-a-vm) voor meer informatie over VM-wachtwoordvereisten.

## <a name="prepare-a-key-vault"></a>Een sleutelkluis voorbereiden

In deze sectie gebruikt u een Resource Manager-sjabloon om een sleutelkluis en een geheim te maken. Dit sjabloon doet het volgende:

* Een sleutelkluis maken waarvoor de eigenschap `enabledForTemplateDeployment` is ingeschakeld. Deze eigenschap moet de waarde 'true' hebben, anders kan tijdens het sjabloonimplementatieproces geen toegang worden verkregen tot de geheimen die in deze sleutelkluis zijn gedefinieerd.
* Voeg een geheim toe aan de sleutelkluis.  In het geheim wordt het beheerderswachtwoord van de virtuele machine opgeslagen.

Als u (als de gebruiker die de virtuele-machinesjabloon gaat implementeren) niet de eigenaar of inzender bent van de sleutelkluis, moet de eigenaar of inzender van de sleutelkluis u toegang verlenen tot de machtiging Microsoft.KeyVault/vaults/deploy/action voor de sleutelkluis. Zie [Azure Key Vault gebruiken om veilig parameterwaarden door te geven tijdens de implementatie](./resource-manager-keyvault-parameter.md) voor meer informatie

Uw Azure AD-gebruikersobject-id is in de sjabloon nodig om machtigingen te kunnen configureren. Met de volgende procedure wordt de object-id (GUID) opgehaald.

1. Voer de volgende Azure PowerShell- of Azure CLI-opdracht uit.  

    # <a name="clitabcli"></a>[CLI](#tab/CLI)
    ```azurecli-interactive
    echo "Enter your email address that is associated with your Azure subscription):" &&
    read upn &&
    az ad user show --upn-or-object-id $upn --query "objectId" &&
    ```   
    # <a name="powershelltabpowershell"></a>[PowerShell](#tab/PowerShell)
    ```azurepowershell-interactive
    $upn = Read-Host -Prompt "Enter your user principal name (email address) used to sign in to Azure"
    (Get-AzADUser -UserPrincipalName $upn).Id
    ```
    of
    ```azurepowershell-interactive
    $displayName = Read-Host -Prompt "Enter your user display name (i.e. John Dole, see the upper right corner of the Azure portal)"
    (Get-AzADUser -DisplayName $displayName).Id
    ```
    ---
2. Noteer de object-id. U hebt deze verderop in de zelfstudie nodig.

U maakt als volgt een sleutelkluis:

1. Selecteer de volgende afbeelding om u aan te melden bij Azure en een sjabloon te openen. Met de sjabloon worden een sleutelkluis en een geheim gemaakt.

    <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Farmtutorials.blob.core.windows.net%2Fcreatekeyvault%2FCreateKeyVault.json"><img src="./media/resource-manager-tutorial-use-key-vault/deploy-to-azure.png" alt="deploy to azure"/></a>

2. Typ of selecteer de volgende waarden.  Selecteer **Aankoop** niet nadat u de waarden hebt ingevoerd.

    ![Resource Manager-sjabloon - sleutelkluisintegratie - portalimplementatie](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-key-vault-portal.png)

    * **Abonnement**: selecteer een Azure-abonnement.
    * **Resourcegroep**: wijs een unieke naam toe. Schrijf deze naam op. U gebruikt deze resourcegroep om in de volgende sessie de virtuele machine te implementeren. Als u zowel de sleutelkluis als de virtuele machine in dezelfde resourcegroep plaatst, wordt het eenvoudiger om de resource na voltooiing van de zelfstudie op te schonen.
    * **Locatie**: selecteer een locatie.  De standaardlocatie is **US - centraal**.
    * **Naam voor Key Vault**: wijs een unieke naam toe. 
    * **Tenant-id**: met de sjabloonfunctie wordt uw tenant-id automatisch opgehaald.  Wijzig de standaardwaarde niet
    * **AD-gebruikers-id**: voer de gebruikersobject-id voor Azure AD in die u hebt opgehaald tijdens de laatste procedure.
    * **Geheime naam**: de standaardnaam is **vmAdminPassword**. Als u de naam van het geheim hier wijzigt, moet u de naam van het geheim bijwerken wanneer u de virtuele machine implementeert.
    * **Geheime waarde**: voer het geheim in.  Het geheim is het wachtwoord dat u gebruikt om zich aan te melden bij de virtuele machine. Het wordt aanbevolen om het wachtwoord dat tijdens de afgelopen procedure is gemaakt, te gebruiken.
    * **Ik ga akkoord met de bovenstaande voorwaarden**: Selecteren.
3. Selecteer **Parameters bewerken** bovenin om de sjabloon te bekijken.
4. Blader naar regel 28 van het JSON-sjabloonbestand. Dit is de resourcedefinitie van de sleutelkluis.
5. Blader naar regel 35:

    ```json
    "enabledForTemplateDeployment": true,
    ```
    `enabledForTemplateDeployment` is een Key Vault-eigenschap. Deze eigenschap moet de waarde 'true' hebben, anders kunt u tijdens de implementatie geen geheimen ophalen uit deze sleutelkluis.
6. Blader naar regel 89. Dit is de Key Vault-geheimdefinitie.
7. Selecteer **Negeren** aan de onderkant van de pagina. U hebt geen wijzigingen aangebracht.
8. Controleer of u alle waarden hebt opgegeven, zoals in de vorige schermafbeelding te zien is. Klik dan onder aan de pagina op **Kopen**.
9. Selecteer het belpictogram (voor meldingen) bovenaan op de pagina om het deelvenster **Meldingen** te openen. Wacht tot de resource is geïmplementeerd.
10. Selecteer **Ga naar de resourcegroep** in het deelvenster **Meldingen**. 
11. Selecteer de naam van de sleutelkluis om deze te openen.
12. Selecteer **Geheimen** in het linkerdeelvenster. Daar wordt **vmAdminPassword** vermeld.
13. Selecteer **Toegangsbeleid** in het linkerdeelvenster. Uw naam (Active Directory) wordt vermeld, want anders hebt u het recht niet om de sleutelkluis te openen.
14. Selecteer **Klik hierop om geavanceerde beleidsregels weer te geven**. U ziet dat **Toegang tot Azure Resource Manager inschakelen voor sjabloonimplementatie** is geselecteerd. Deze instelling is nog een voorwaarde voor een goede werking van de Key Vault-integratie.

    ![Resource Manager-sjabloon - sleutelkluisintegratie - toegangsbeleid](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-key-vault-access-policies.png)
15. Selecteer **Eigenschappen** in het linkerdeelvenster.
16. Kopieer de **resource-id**. U hebt deze id nodig bij het implementeren van de virtuele machine.  De indeling van de resource-id is:

    ```json
    /subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>
    ```

## <a name="open-a-quickstart-template"></a>Een snelstartsjabloon openen

Azure-snelstartsjablonen is een opslagplaats voor Resource Manager-sjablonen. In plaats van een sjabloon helemaal vanaf de basis te maken, kunt u een voorbeeldsjabloon zoeken en aanpassen. De sjabloon die in deze zelfstudie wordt gebruikt, heet [Deploy a simple Windows VM](https://azure.microsoft.com/resources/templates/101-vm-simple-windows/) (Een eenvoudige Windows-VM implementeren).

1. Selecteer in Visual Studio Code **Bestand**>**Bestand openen**.
2. Plak de volgende URL in **Bestandsnaam**:

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.json
    ```
3. Selecteer **Openen** om het bestand te openen. Dit is hetzelfde scenario als in [Zelfstudie: Azure Resource Manager-sjablonen met afhankelijke resources maken](./resource-manager-tutorial-create-templates-with-dependent-resources.md).
4. Er worden vijf resources gedefinieerd met de sjabloon:

   * `Microsoft.Storage/storageAccounts`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/Microsoft.Storage/storageAccounts).
   * `Microsoft.Network/publicIPAddresses`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/publicipaddresses).
   * `Microsoft.Network/virtualNetworks`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/virtualnetworks).
   * `Microsoft.Network/networkInterfaces`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.network/networkinterfaces).
   * `Microsoft.Compute/virtualMachines`. Zie de [sjabloonverwijzing](https://docs.microsoft.com/azure/templates/microsoft.compute/virtualmachines).

     Het is handig om enige basiskennis te hebben van de sjabloon voordat u deze gaat aanpassen.
5. Selecteer **Bestand**>**Opslaan als** om het bestand op uw lokale computer op te slaan als **azuredeploy.json**.
6. Herhaal stap 1-4 om de volgende URL te openen en sla het bestand dan op als **azuredeploy.parameters.json**.

    ```url
    https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-simple-windows/azuredeploy.parameters.json
    ```

## <a name="edit-the-parameters-file"></a>Het parameterbestand bewerken

U hoeft het sjabloonbestand niet te wijzigen.

1. Open **azuredeploy.parameters.json** in Visual Studio Code als het bestand nog niet is geopend.
2. Werk de parameter **adminPassword** bij naar:

    ```json
    "adminPassword": {
        "reference": {
            "keyVault": {
            "id": "/subscriptions/<SubscriptionID>/resourceGroups/mykeyvaultdeploymentrg/providers/Microsoft.KeyVault/vaults/<KeyVaultName>"
            },
            "secretName": "vmAdminPassword"
        }
    },
    ```

    Vervang de **id** door de resource-id van de sleutelkluis die u in de afgelopen procedure hebt gemaakt.  

    ![Key Vault en Resource Manager-sjabloon integreren - virtuele-machine-implementatie - parameterbestand](./media/resource-manager-tutorial-use-key-vault/resource-manager-tutorial-create-vm-parameters-file.png)
3. Geef waarden op voor:

    * **adminUsername**: geef het beheerdersaccount van de virtuele machine een naam.
    * **dnsLabelPrefix**: geef dnsLabelPrefix een naam.
4. Sla de wijzigingen op.

## <a name="deploy-the-template"></a>De sjabloon implementeren

Volg de instructies in [De sjabloon implementeren](./resource-manager-tutorial-create-templates-with-dependent-resources.md#deploy-the-template) om de sjabloon te implementeren. U moet zowel **azuredeploy.json** als **azuredeploy.parameters.json** uploaden naar de cloudshell. Gebruik dan het volgende PowerShell-script om de sjabloon te implementeren:

```azurepowershell
$resourceGroupName = Read-Host -Prompt "Enter the Resource Group name"
$location = Read-Host -Prompt "Enter the location (i.e. centralus)"

New-AzResourceGroup -Name $resourceGroupName -Location $location
New-AzResourceGroupDeployment `
    -ResourceGroupName $resourceGroupName `
    -TemplateFile "$HOME/azuredeploy.json" `
    -TemplateParameterFile "$HOME/azuredeploy.parameters.json"
```

Wanneer u de sjabloon implementeert, gebruikt u dezelfde resourcegroep als die van de sleutelkluis. Dit is handiger bij het opschonen van de resources. U hoeft dan slechts één resourcegroep te verwijderen in plaats van twee.

## <a name="valid-the-deployment"></a>De implementatie controleren

Wanneer u de virtuele machine hebt geïmplementeerd, test u of u zich kunt aanmelden met het wachtwoord dat in de sleutelkluis is opgeslagen.

1. Open de [Azure Portal](https://portal.azure.com).
2. Selecteer **Resourcegroepen**/**Naam van uw resourcegroep>**/**simpleWinVM**
3. Selecteer bovenaan de optie **Verbinding maken**.
4. Selecteer **RDP-bestand downloaden** en volg de instructies voor het aanmelden bij de virtuele machine, met het wachtwoord dat is opgeslagen in de sleutelkluis.

## <a name="clean-up-resources"></a>Resources opschonen

Schoon de geïmplementeerd Azure-resources, wanneer u deze niet meer nodig hebt, op door de resourcegroep te verwijderen.

1. Selecteer **Resourcegroep** in het linkermenu van Azure Portal.
2. Voer de naam van de resourcegroep in het veld **Filter by name** in.
3. Selecteer de naam van de resourcegroep.  U ziet in totaal zes resources in de resourcegroep.
4. Selecteer **Resourcegroep verwijderen** in het bovenste menu.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een geheim opgehaald uit Azure Key Vault en hebt u dat geheim gebruikt in uw sjabloonimplementatie.  Voor meer informatie over het maken van gekoppelde sjablonen ziet u:

> [!div class="nextstepaction"]
> [Gekoppelde sjablonen maken](./resource-manager-tutorial-create-linked-templates.md)

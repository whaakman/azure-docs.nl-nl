---
title: Snelstart - Webverkeer omleiden met Azure Application Gateway - Azure Portal | Microsoft Docs
description: Lees hoe u de Azure-portal kunt gebruiken om een Azure Application Gateway te maken die webverkeer omleidt naar virtuele machines in een back-endpool.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 1/8/2019
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 16e23f77509d2402f765981b39a30e08a2309f68
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54156524"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Snelstart: Webverkeer omleiden met Azure Application Gateway - Azure Portal

In deze snelstart wordt beschreven hoe u Azure Portal kunt gebruiken om snel de toepassingsgateway te maken met twee virtuele machines in zijn back-endpool. Vervolgens test u de gateway om er zeker van te zijn dat deze correct werkt. Met Azure Application Gateway kunt u webverkeer van uw toepassing omleiden naar specifieke resources door listeners toe te wijzen aan poorten, regels te maken en resources toe te voegen aan een back-endpool.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Er is een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. In dit voorbeeld maakt u twee subnetten: één voor de toepassingsgateway en één voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Selecteer **Een resource maken** in het linkermenu van de Azure-portal. Het venster **Nieuw** wordt weergegeven.

2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst **Aanbevolen**.

### <a name="basics-page"></a>De pagina Basisinformatie

1. Op de pagina **Basisinformatie** voert u deze waarden in voor de volgende toepassingsgateway-instellingen:

    - **Naam**: Typ *myAppGateway* als naam voor de toepassingsgateway.
    - **Resourcegroep**: Selecteer **myResourceGroupAG** als de resourcegroep. Als deze nog niet bestaat, selecteert u **Nieuwe maken** om deze te maken.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**.

### <a name="settings-page"></a>De pagina Instellingen

1. Op de pagina **Instellingen**, onder **Subnetconfiguratie**, selecteert u **Een virtueel netwerk kiezen**.

2. Op de pagina **Virtueel netwerk kiezen** selecteert u **Nieuwe maken** en voert u waarden in voor de volgende instellingen voor het virtuele netwerk:

    - **Naam**: Typ *myVnet* als naam voor het virtuele netwerk.

    - **Adresruimte**: Voer *10.0.0.0/16* in als de adresruimte van het virtuele netwerk.

    - **Subnetnaam**: Typ *myAGSubnet* als naam voor het subnet.<br>Het subnet van de toepassingsgateway kan alleen bestaan uit toepassingsgateways. Andere resources zijn niet toegestaan.

    - **Subnetadresbereik**: Voer *10.0.0.0/24* in als het adresbereik van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

3. Selecteer **OK** om terug te gaan naar de pagina **Instellingen**.

4. Controleer onder **Frontend-IP-configuratie** of **Type IP-adres** is ingesteld op **Openbaar**. Controleer onder **Openbaar IP-adres** of **Nieuw** is geselecteerd. 

5. Typ *myAGPublicIPAddress* als naam voor het openbare IP-adres. 

6. Accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**.

### <a name="summary-page"></a>Overzichtspagina

Controleer de instellingen op de **overzichtspagina** en selecteer **OK** om het virtuele netwerk, het openbare IP-adres en de toepassingsgateway te maken. Het kan enkele minuten duren om de toepassingsgateway te maken in Azure. Wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

## <a name="add-a-subnet"></a>Een subnet toevoegen

Voeg een subnet toe aan het virtuele netwerk dat u zojuist hebt gemaakt door de volgende stappen uit te voeren:

1. Selecteer **Alle resources** in het linkermenu van de Azure-portal, zoek *myVNet* in het zoekvak en selecteer vervolgens **myVNet** in de lijst met zoekresultaten.

2. Selecteer **Subnetten** in het linkermenu en selecteer **+ Subnet**. 

    ![Subnet maken](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Typ op de pagina **Subnet toevoegen** *myBackendSubnet* in het vak **Naam** voor het subnet en selecteer **OK**.

## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines die in Azure worden gebruikt als back-endservers voor de toepassingsgateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway in Azure is gemaakt.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Selecteer **Een resource maken** in de Azure-portal. Het venster **Nieuw** wordt weergegeven.

2. Selecteer **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst **Aanbevolen**. De pagina **Een virtuele machine maken** wordt weergegeven.

3. Voer deze waarden in op het tabblad **Basisinformatie** voor de volgende instellingen voor de virtuele machine:

    - **Resourcegroep**: Selecteer **myResourceGroupAG** als naam van de resourcegroep.
    - **Naam van virtuele machine**: Typ *myVM* als naam voor de virtuele machine.
    - **Gebruikersnaam**: Typ *azureuser* als gebruikersnaam van de beheerder.
    - **Wachtwoord**: Typ *Azure123456!* als beheerderswachtwoord.

4. Accepteer de overige standaardwaarden en klik op **Volgende: Schijven**.  

5. Accepteer de standaardwaarden op het tabblad **Schijven** en selecteer **Volgende: Netwerken**.

6. Zorg ervoor dat, op het tabblad **Netwerken**, **myVNet** is geselecteerd bij **Virtueel netwerk** en dat **Subnet** is ingesteld op **myBackendSubnet**. Accepteer de overige standaardwaarden en klik op **Volgende: Beheer**.

8. Op het tabblad **Beheer** stelt u **Diagnostische gegevens over opstarten** in op **Uit**. Accepteer de overige standaardwaarden en selecteer **Beoordelen en maken**.

9. Controleer de instellingen op het tabblad **Beoordelen en maken**, corrigeer eventuele validatiefouten en selecteer vervolgens **Maken**.

10. Wacht tot de virtuele machine is gemaakt voordat u verder gaat.

### <a name="install-iis"></a>IIS installeren

1. Open [Azure PowerShell](https://docs.microsoft.com/azure/cloud-shell/quickstart-powershell). Hiertoe selecteert u **Cloud Shell** in de bovenste navigatiebalk van de Azure-portal en vervolgens **PowerShell** in de vervolgkeuzelijst. 

    ![Aangepaste extensie installeren](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Voer de volgende opdracht uit om IIS op de virtuele machine te installeren: 

    ```azurepowershell-interactive
    Set-AzureRmVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Maak een tweede virtuele machine en installeer IIS met behulp van de stappen die u zojuist hebt voltooid. Gebruik *myVM2* als naam voor de virtuele machine en voor de instelling **VMName** van de cmdlet **Set-AzureRmVMExtension**.

### <a name="add-backend-servers"></a>Back-endservers toevoegen

1. Selecteer **Alle resources** en vervolgens **myAppGateway**.

2. Selecteer **Back-endpools** in het linkermenu. Bij het maken van de toepassingsgateway wordt automatisch de standaardpool **appGatewayBackendPool** gemaakt. 

3. Selecteer **appGatewayBackendPool**.

4. Onder **Doelen** selecteert u **Virtuele machine** in de vervolgkeuzelijst.

5. Onder **VIRTUELE MACHINE** en **NETWERKINTERFACES** selecteert u de virtuele machines **myVM** en **myVM2** en de bijbehorende netwerkinterfaces in de vervolgkeuzelijsten.

    ![Back-endservers toevoegen](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Selecteer **Opslaan**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Zoek op de pagina **Overzicht** het openbare IP-adres voor de toepassingsgateway.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

    U kunt ook **Alle resources** selecteren, *myAGPublicIPAddress* invoeren in het zoekvak en het openbare IP-adres vervolgens selecteren in de lijst met zoekresultaten. Het openbare IP-adres wordt weergegeven op de pagina **Overzicht**.

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

    ![Toepassingsgateway testen](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de bij de toepassingsgateway gemaakte resources niet meer nodig hebt, verwijdert u de resourcegroep. Als u de resourcegroep verwijdert, worden ook de toepassingsgateway en alle gerelateerde resources verwijderd. 

Ga als volgt te werk om de resourcegroep te verwijderen:
1. Selecteer **Resourcegroepen** in het linkermenu van de Azure-portal.
2. Zoek en selecteer **myResourceGroupAG** in de lijst op de pagina **Resourcegroepen**.
3. Selecteer **Resourcegroep verwijderen** op de **pagina van de resourcegroep**.
4. Voer *myResourceGroupAG* in bij **TYP DE RESOURCEGROEPNAAM** en selecteer vervolgens **Verwijderen**

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Webverkeer met een toepassingsgateway beheren met behulp van Azure CLI](./tutorial-manage-web-traffic-cli.md)

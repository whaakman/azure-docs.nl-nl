---
title: Snelstart - Webverkeer omleiden met Azure Application Gateway - Azure Portal | Microsoft Docs
description: Lees hoe u de Azure-portal kunt gebruiken om een Azure Application Gateway te maken die webverkeer omleidt naar virtuele machines in een back-endpool.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: quickstart
ms.date: 11/15/2018
ms.author: victorh
ms.custom: mvc
ms.openlocfilehash: 6ad839b9cf1179e282b9163df5a38e13417408e2
ms.sourcegitcommit: 275eb46107b16bfb9cf34c36cd1cfb000331fbff
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51706225"
---
# <a name="quickstart-direct-web-traffic-with-azure-application-gateway---azure-portal"></a>Snelstart: Webverkeer omleiden met Azure Application Gateway - Azure Portal

Met Azure Application Gateway kunt u het webverkeer van uw toepassing omleiden naar specifieke resources door listeners toe te wijzen aan poorten, regels te maken en resources toe te voegen aan een back-endpool.

In deze snelstart wordt beschreven hoe u Azure Portal kunt gebruiken om snel de toepassingsgateway te maken met twee virtuele machines in zijn back-endpool. Vervolgens test u de gateway om er zeker van te zijn dat deze correct werkt.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

U hebt een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Selecteer **Netwerken** en klik op **Application Gateway** in de lijst Aanbevolen.

### <a name="basics"></a>Basisbeginselen

1. Voer deze waarden in voor de toepassingsgateway:

    - *myAppGateway* als de naam van de toepassingsgateway.
    - *myResourceGroupAG* als de nieuwe resourcegroep.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.

### <a name="settings"></a>Instellingen

1. Klik op **Een virtueel netwerk kiezen**, klik op **Nieuw maken** en voer deze waarden in voor het virtuele netwerk:

    - *myVnet* als de naam van het virtuele netwerk.
    - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
    - *myAGSubnet* als de naam van het subnet.
    - *10.0.0.0/24* als het adresbereik van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Klik op **OK** om terug te gaan naar de pagina Instellingen.
7. Controleer onder **Front-end-IP-configuratie** of **Type IP-adres** is ingesteld op **Openbaar**, en controleer onder **Openbaar IP-adres** of **Nieuw maken** is geselecteerd. Typ *myAGPublicIPAddress* als naam van het openbaar IP-adres. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.

### <a name="summary"></a>Samenvatting

Controleer de instellingen op de overzichtspagina en klik op **OK** om het virtuele netwerk, het openbare IP-adres en de toepassingsgateway te maken. Het kan enkele minuten duren voordat de toepassingsgateway is gemaakt. Wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

## <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik in het menu links op **Alle resources** en vervolgens in de lijst met resources op **myVNet**.
2. Klik op **Subnetten** en vervolgens op **+ Subnet**.

    ![Subnet maken](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* in als naam van het subnet en klik op **OK**.

## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines die worden gebruikt als back-endservers voor de toepassingsgateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik in Azure Portal op **Een resource maken**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *myResourceGroupAG* als de resourcegroep.
    - *myVM* als naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* als het wachtwoord.

   Accepteer de overige standaardwaarden en klik op **Next: Disks**.
4. Accepteer de overige standaardwaarden en klik op **Next: Networking**.
5. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en dat het subnet **myBackendSubnet** is.
6. Accepteer de overige standaardwaarden en klik op **Next: Management**.
7. Klik op **Uit** om diagnostische gegevens over opstarten uit te schakelen. Accepteer de overige standaardwaarden en klik op **Review + create**.
8. Controleer de instellingen op de overzichtspagina en klik op **Maken**.
9. Wacht tot de virtuele machine is gemaakt voordat u verder gaat.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en controleer of deze is ingesteld op **PowerShell**.

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

3. Maak een tweede virtuele machine en installeer IIS met behulp van de stappen die u zojuist hebt voltooid. Voer *myVM2* in als de naam ervan en als VMName in Set-AzureRmVMExtension.

### <a name="add-backend-servers"></a>Back-endservers toevoegen

1. Klik op **Alle resources** en vervolgens op **myAppGateway**.
4. Klik op **Back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. Klik op **appGatewayBackendPool**.
5. Klik onder **Doelen** op **IP-adres of FQDN**. Selecteer **Virtuele machine**.
6. Voeg onder **Virtuele machine** de virtuele machines myVM en myVM2 en hun bijbehorende netwerkinterfaces toe.

    ![Back-endservers toevoegen](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Klik op **Opslaan**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Zoek op het scherm Overzicht het openbare IP-adres voor de toepassingsgateway. Klik op **Alle resources** en vervolgens op **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

    ![Toepassingsgateway testen](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Resources opschonen

U kunt de resourcegroep, de toepassingsgateway en alle gerelateerde resources verwijderen als u deze niet meer nodig hebt. Selecteer hiertoe de resourcegroep met de toepassingsgateway en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Webverkeer met een toepassingsgateway beheren met behulp van Azure CLI](./tutorial-manage-web-traffic-cli.md)

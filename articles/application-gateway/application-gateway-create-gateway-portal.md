---
title: Een Application Gateway maken - Azure Portal
description: Informatie over het maken van een toepassingsgateway met behulp van de Azure-portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2018
ms.author: victorh
ms.openlocfilehash: d8ab49bc988060533bc5ff65d414dcba6245be48
ms.sourcegitcommit: 0b7fc82f23f0aa105afb1c5fadb74aecf9a7015b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2018
ms.locfileid: "51631906"
---
# <a name="create-an-application-gateway-using-the-azure-portal"></a>Een toepassingsgateway met behulp van de Azure portal maken

U kunt de Azure-portal te maken of beheren van Toepassingsgateways. Dit artikel ziet u hoe u netwerkbronnen, back-endservers en een application gateway maakt.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor communicatie tussen de bronnen die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.
2. Klik op **netwerken** en klik vervolgens op **Application Gateway** in de lijst met aanbevolen.

### <a name="basics"></a>Basisbeginselen

1. Voer deze waarden in voor de toepassingsgateway:

    - *myAppGateway* als de naam van de toepassingsgateway.
    - *myResourceGroupAG* als de nieuwe resourcegroep.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

2. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.

### <a name="settings"></a>Instellingen

1. Klik op **een virtueel netwerk kiezen**, klikt u op **nieuw**, en voer vervolgens deze waarden voor het virtuele netwerk:

    - *myVnet* als de naam van het virtuele netwerk.
    - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
    - *myAGSubnet* als de naam van het subnet.
    - *10.0.0.0/24* : voor het adresbereik van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Klik op **OK** om terug te gaan naar de pagina instellingen.
7. Onder **Frontend-IP-configuratie** zorgen **type IP-adres** is ingesteld op **openbare**, en klikt u onder **openbaar IP-adres**, zorg ervoor dat **Nieuw** is geselecteerd. Type *myAGPublicIPAddress* voor de naam van het openbare IP-adres. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.

### <a name="summary"></a>Samenvatting

Controleer de instellingen op de overzichtspagina en klik op **OK** om het virtuele netwerk, het openbare IP-adres en de toepassingsgateway te maken. Het kan enkele minuten duren voordat de toepassingsgateway is gemaakt. Wacht totdat de implementatie voltooid is voordat u doorgaat met de volgende sectie.

## <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik in het menu links op **Alle resources** en vervolgens in de lijst met resources op **myVNet**.
2. Klik op **subnetten**, en klik vervolgens op **+ Subnet**.

    ![Subnet maken](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* in als naam van het subnet en klik op **OK**.

## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines die worden gebruikt als back-endservers voor application gateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik in Azure Portal op **Een resource maken**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst met aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *myResourceGroupAG* voor de resourcegroep.
    - *myVM* als naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* als het wachtwoord.

   Accepteer de overige standaardwaarden en klik op **volgende: schijven**.
4. Accepteer de standaardwaarden van de schijf en klikt u op **volgende: netwerken**.
5. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en dat het subnet **myBackendSubnet** is.
6. Accepteer de overige standaardwaarden en klik op **volgende: Management**.
7. Klik op **uit** diagnostische gegevens over opstarten uitschakelen. Accepteer de overige standaardwaarden en klik op **revisie + maken**.
8. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **maken**.
9. Wachten op voor het maken van de virtuele machine om te voltooien voordat u doorgaat.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze ingesteld op **PowerShell**.

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

1. Klik op **alle resources**, en klik vervolgens op **myAppGateway**.
4. Klik op **Back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. Klik op **appGatewayBackendPool**.
5. Onder **doelen**, klikt u op **IP-adres of FQDN-naam** Selecteer **virtuele machine**.
6. Onder **virtuele Machine**, myVM en myVM2 virtuele machines en hun bijbehorende netwerkinterfaces toevoegen.

    ![Back-endservers toevoegen](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Klik op **Opslaan**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Zoek op het scherm Overzicht het openbare IP-adres voor de toepassingsgateway. Klik op **alle resources** en klik vervolgens op **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

    ![Toepassingsgateway testen](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u niet meer nodig hebt, verwijdert u de resourcegroep, application gateway en alle gerelateerde resources. Selecteer hiertoe de resourcegroep met de toepassingsgateway en klik op **Verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een resourcegroep, netwerkbronnen en backend-servers gemaakt. Vervolgens gebruikt u deze resources om een toepassingsgateway te maken. Zie voor meer informatie over Toepassingsgateways en de bijbehorende resources, [wat is Azure Application Gateway?](overview.md)

---
title: Maken van een toepassingsgateway - Azure-Portal | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met behulp van de Azure-portal.
services: application-gateway
author: davidmu1
manager: timlt
editor: 
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/25/2018
ms.author: davidmu
ms.openlocfilehash: df9235bc7ff61943de52a0bcc4064bf9fab6636a
ms.sourcegitcommit: ded74961ef7d1df2ef8ffbcd13eeea0f4aaa3219
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2018
---
# <a name="create-an-application-gateway-using-the-azure-portal"></a>Een toepassingsgateway met Azure portal maken

U kunt de Azure portal maken of beheren van Toepassingsgateways. Deze snelstartgids wordt beschreven hoe u netwerkbronnen, back-endservers en een toepassingsgateway maken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij de Azure portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor de communicatie tussen de bronnen die u maakt. Twee subnetten worden gemaakt in dit voorbeeld: één voor de toepassingsgateway, en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Networking** en selecteer vervolgens **Application Gateway** in de lijst met aanbevolen.
3. Voer deze waarden voor de toepassingsgateway:

    - *myAppGateway* - voor de naam van de toepassingsgateway.
    - *myResourceGroupAG* - voor de nieuwe resourcegroep.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-create-gateway-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
5. Klik op **Kies een virtueel netwerk**, klikt u op **nieuw**, en voer deze waarden voor het virtuele netwerk:

    - *myVNet* - voor de naam van het virtuele netwerk.
    - *10.0.0.0/16* - voor de adresruimte van het virtuele netwerk.
    - *myAGSubnet* - voor de subnetnaam.
    - *10.0.0.0/24* - voor de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-create-gateway-portal/application-gateway-vnet.png)

6. Klik op **OK** voor het maken van het virtuele netwerk en subnet.
6. Klik op **Kies een openbaar IP-adres**, klikt u op **nieuw**, en voer vervolgens de naam van het openbare IP-adres. In dit voorbeeld wordt het openbare IP-adres met de naam *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om het virtuele netwerk, het openbare IP-adres en de toepassingsgateway te maken. Duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie wordt voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik op **alle resources** in het menu links en klik vervolgens op **myVNet** uit de lijst met resources.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/application-gateway-create-gateway-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* voor de naam van het subnet en klik vervolgens op **OK**.

## <a name="create-backend-servers"></a>Maken van back-endservers

In dit voorbeeld maakt u twee virtuele machines moet worden gebruikt als back-endservers voor de toepassingsgateway. U kunt ook IIS installeren op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst met aanbevolen.
3. Voer deze waarden voor de virtuele machine:

    - *myVM* - voor de naam van de virtuele machine.
    - *azureuser* - voor de gebruikersnaam van de beheerder.
    - *Azure123456!* voor het wachtwoord.
    - Selecteer **gebruik bestaande**, en selecteer vervolgens *myResourceGroupAG*.

4. Klik op **OK**.
5. Selecteer **DS1_V2** voor de grootte van de virtuele machine en klik op **Selecteer**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en het subnet is **myBackendSubnet**. 
7. Klik op **uitgeschakelde** diagnostische gegevens over opstarten uitschakelen.
8. Klik op **OK**, Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze is ingesteld op **PowerShell**.

    ![Aangepaste extensie installeren](./media/application-gateway-create-gateway-portal/application-gateway-extension.png)

2. Voer de volgende opdracht voor het installeren van IIS op de virtuele machine: 

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

3. Maak een tweede virtuele machine en IIS installeren met behulp van de stappen die u zojuist hebt voltooid. Voer *myVM2* voor de naam en VMName in Set AzureRmVMExtension.

### <a name="add-backend-servers"></a>Back-endservers toevoegen

3. Klik op **alle resources**, en klik vervolgens op **myAppGateway**.
4. Klik op **back-endpools**. Een standaardgroep is automatisch gemaakt met de toepassingsgateway. Klik op **appGatewayBackendPool**.
5. Klik op **toevoegen doel** elke virtuele machine die u hebt gemaakt toevoegen aan de back-endpool.

    ![Back-endservers toevoegen](./media/application-gateway-create-gateway-portal/application-gateway-backend.png)

6. Klik op **Opslaan**.

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

1. Het openbare IP-adres vinden voor de toepassingsgateway op het scherm overzicht. Klik op **alle resources** en klik vervolgens op **myAGPublicIPAddress**.

    ![Record application gateway openbaar IP-adres](./media/application-gateway-create-gateway-portal/application-gateway-record-ag-address.png)

2. Het openbare IP-adres Kopieer en plak deze in de adresbalk van uw browser.

    ![Test toepassingsgateway](./media/application-gateway-create-gateway-portal/application-gateway-iistest.png)


## <a name="clean-up-resources"></a>Resources opschonen

Wanneer deze niet langer nodig is, de resourcegroep, toepassingsgateway en alle bijbehorende resources verwijderd. Om dit te doen, selecteert u de resourcegroep met de toepassingsgateway en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids gemaakt u een resourcegroep, netwerkbronnen en back-endservers. Vervolgens gebruikt u deze resources om een toepassingsgateway te maken. Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.

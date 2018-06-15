---
title: Een toepassingsgateway maken met een web application firewall - Azure-portal | Microsoft Docs
description: Informatie over het maken van een toepassingsgateway met web application firewall via de Azure-portal.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.workload: infrastructure-services
ms.date: 01/26/2018
ms.author: victorh
ms.openlocfilehash: 9967813b193159b68aa0f008dae4440aa6e533dc
ms.sourcegitcommit: c47ef7899572bf6441627f76eb4c4ac15e487aec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/04/2018
ms.locfileid: "33207388"
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Een toepassingsgateway maken met een web application firewall met de Azure portal

U kunt de Azure portal maken een [toepassingsgateway](application-gateway-introduction.md) met een [web application firewall](application-gateway-web-application-firewall-overview.md) (WAF). Het gebruik van WAF [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project) regels voor het beveiligen van uw toepassing. Deze regels omvatten bescherming tegen aanvallen, zoals SQL-injectie, cross-site scripting aanvallen en sessie hijacks.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken met WAF ingeschakeld
> * De virtuele machines gebruikt als back-endservers maken
> * Een opslagaccount maken en configureren van diagnostische gegevens

![Web application firewall voorbeeld](./media/application-gateway-web-application-firewall-portal/scenario-waf.png)

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij de Azure portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor de communicatie tussen de bronnen die u maakt. Twee subnetten worden gemaakt in dit voorbeeld: één voor de toepassingsgateway, en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Networking** en selecteer vervolgens **Application Gateway** in de lijst met aanbevolen.
3. Voer deze waarden voor de toepassingsgateway:

    - *myAppGateway* - voor de naam van de toepassingsgateway.
    - *myResourceGroupAG* - voor de nieuwe resourcegroep.
    - Selecteer *WAF* voor de laag van de toepassingsgateway.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-web-application-firewall-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
5. Klik op **Kies een virtueel netwerk**, klikt u op **nieuw**, en voer deze waarden voor het virtuele netwerk:

    - *myVNet* - voor de naam van het virtuele netwerk.
    - *10.0.0.0/16* - voor de adresruimte van het virtuele netwerk.
    - *myAGSubnet* - voor de subnetnaam.
    - *10.0.0.0/24* - voor de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-web-application-firewall-portal/application-gateway-vnet.png)

6. Klik op **OK** voor het maken van het virtuele netwerk en subnet.
7. Klik op **Kies een openbaar IP-adres**, klikt u op **nieuw**, en voer vervolgens de naam van het openbare IP-adres. In dit voorbeeld wordt het openbare IP-adres met de naam *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om netwerkbronnen en de toepassingsgateway te maken. Duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie wordt voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik op **alle resources** in het menu links en klik vervolgens op **myVNet** uit de lijst met resources.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/application-gateway-web-application-firewall-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* voor de naam van het subnet en klik vervolgens op **OK**.

## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines moet worden gebruikt als back-endservers voor de toepassingsgateway. U kunt ook IIS installeren op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst met aanbevolen.
3. Voer deze waarden voor de virtuele machine:

    - *myVM* - voor de naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* voor het wachtwoord.
    - Selecteer **gebruik bestaande**, en selecteer vervolgens *myResourceGroupAG*.

4. Klik op **OK**.
5. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en het subnet is **myBackendSubnet**. 
7. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
8. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze is ingesteld op **PowerShell**.

    ![Aangepaste extensie installeren](./media/application-gateway-web-application-firewall-portal/application-gateway-extension.png)

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

1. Klik op **alle resources**, en klik vervolgens op **myAppGateway**.
2. Klik op **back-endpools**. Een standaardgroep is automatisch gemaakt met de toepassingsgateway. Klik op **appGateayBackendPool**.
3. Klik op **toevoegen doel** elke virtuele machine die u hebt gemaakt toevoegen aan de back-endpool.

    ![Back-endservers toevoegen](./media/application-gateway-web-application-firewall-portal/application-gateway-backend.png)

4. Klik op **Opslaan**.

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Een opslagaccount maken en configureren van diagnostische gegevens

## <a name="create-a-storage-account"></a>Create a storage account

In deze zelfstudie gebruikt de toepassingsgateway een opslagaccount voor het opslaan van gegevens voor detectie en preventie doeleinden. U kunt ook logboekanalyse of Event Hub gebruiken om gegevens te noteren.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Storage** en selecteer vervolgens **Opslagaccount - blob, bestand, tabel, wachtrij**.
3. Voer de naam van het opslagaccount, selecteer **gebruik bestaande** voor de resourcegroep en selecteer vervolgens **myResourceGroupAG**. In dit voorbeeld wordt de naam van het opslagaccount is *myagstore1*. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **maken**.

## <a name="configure-diagnostics"></a>Diagnostische gegevens configureren

Diagnostische gegevens te registreren in de logboeken ApplicationGatewayAccessLog ApplicationGatewayPerformanceLog en ApplicationGatewayFirewallLog configureert.

1. Klik in het menu links op **alle resources**, en selecteer vervolgens *myAppGateway*.
2. Bewaking, klik op **diagnostische logboeken**.
3. Klik op **diagnostics-instelling toevoegen**.
4. Voer *myDiagnosticsSettings* als de naam voor de diagnostische instellingen.
5. Selecteer **archiveren naar een opslagaccount**, en klik vervolgens op **configureren** selecteren de *myagstore1* storage-account dat u eerder hebt gemaakt.
6. Selecteer de gateway-logboeken te verzamelen en bewaren.
7. Klik op **Opslaan**.

    ![Diagnostische gegevens configureren](./media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

1. Het openbare IP-adres vinden voor de toepassingsgateway op het scherm overzicht. Klik op **alle resources** en klik vervolgens op **myAGPublicIPAddress**.

    ![Record application gateway openbaar IP-adres](./media/application-gateway-web-application-firewall-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

    ![Test toepassingsgateway](./media/application-gateway-web-application-firewall-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe:

> [!div class="checklist"]
> * Een toepassingsgateway maken met WAF ingeschakeld
> * De virtuele machines gebruikt als back-endservers maken
> * Een opslagaccount maken en configureren van diagnostische gegevens

Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.
---
title: Een toepassingsgateway maken met een web application firewall - Azure portal | Microsoft Docs
description: Meer informatie over het maken van een toepassingsgateway met een firewall voor webtoepassingen met behulp van de Azure-portal.
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
ms.openlocfilehash: b368ef3b5503d90b0eb928113e8154aabca9c04a
ms.sourcegitcommit: 33091f0ecf6d79d434fa90e76d11af48fd7ed16d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54157136"
---
# <a name="create-an-application-gateway-with-a-web-application-firewall-using-the-azure-portal"></a>Een toepassingsgateway maken met een firewall voor webtoepassingen met behulp van de Azure portal

U kunt de Azure-portal gebruiken om te maken een [toepassingsgateway](application-gateway-introduction.md) met een [web application firewall](application-gateway-web-application-firewall-overview.md) (WAF). De WAF gebruikt [OWASP](https://www.owasp.org/index.php/Category:OWASP_ModSecurity_Core_Rule_Set_Project)-regels om uw toepassing te beveiligen. Deze regels omvatten bescherming tegen aanvallen als SQL-injectie, XSS-aanvallen (cross-site scripting) en sessiekapingen.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een toepassingsgateway maken met WAF ingeschakeld
> * De virtuele machines die worden gebruikt als back-endservers maken
> * Een opslagaccount maken en diagnostische gegevens configureren

![Voorbeeld van een WAF (Web Application Firewall)](./media/application-gateway-web-application-firewall-portal/scenario-waf.png)

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure Portal op [https://portal.azure.com](https://portal.azure.com)

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

U hebt een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer deze waarden in voor de toepassingsgateway:

    - *myAppGateway* als de naam van de toepassingsgateway.
    - *myResourceGroupAG* als de nieuwe resourcegroep.
    - Selecteer *WAF* voor de laag van de toepassingsgateway.

    ![Nieuwe toepassingsgateway maken](./media/application-gateway-web-application-firewall-portal/application-gateway-create.png)

4. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
5. Klik op **Een virtueel netwerk kiezen**, klik op **Nieuw maken** en voer deze waarden in voor het virtuele netwerk:

    - *myVnet* als de naam van het virtuele netwerk.
    - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
    - *myAGSubnet* als de naam van het subnet.
    - *10.0.0.0/24* als de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/application-gateway-web-application-firewall-portal/application-gateway-vnet.png)

6. Klik op **OK** om het virtuele netwerk en subnet te maken.
7. Klik op **een openbaar IP-adres kiezen**, klikt u op **nieuw**, en voer de naam van het openbare IP-adres. In dit voorbeeld is de naam van het openbare IP-adres *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
8. Accepteer de standaardwaarden voor de configuratie van de Listener, laat u de Web application firewall is uitgeschakeld en klik vervolgens op **OK**.
9. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om netwerkbronnen en de application gateway te maken. Het duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik in het menu links op **Alle resources** en vervolgens in de lijst met resources op **myVNet**.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/application-gateway-web-application-firewall-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* in als naam van het subnet en klik op **OK**.

## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines die worden gebruikt als back-endservers voor de toepassingsgateway. U installeert ook IIS op de virtuele machines om te controleren of de toepassingsgateway is gemaakt.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Klik op **Nieuw**.
2. Klik op **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *myVM* als naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* als het wachtwoord.
    - Selecteer **Bestaande gebruiken** en selecteer *myResourceGroupAG*.

4. Klik op **OK**.
5. Selecteer **DS1_V2** als grootte van de virtuele machine en klik op **Selecteren**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en dat het subnet **myBackendSubnet** is. 
7. Klik op **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
8. Klik op **OK**, controleer de instellingen op de overzichtspagina en klik op **Maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en zorg ervoor dat deze is ingesteld op **PowerShell**.

    ![Aangepaste extensie installeren](./media/application-gateway-web-application-firewall-portal/application-gateway-extension.png)

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
2. Klik op **Back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. Klik op **appGatewayBackendPool**.
3. Klik op **toevoegen doel** elke virtuele machine die u hebt gemaakt aan de back-endpool toevoegen.

    ![Back-endservers toevoegen](./media/application-gateway-web-application-firewall-portal/application-gateway-backend.png)

4. Klik op **Opslaan**.

## <a name="create-a-storage-account-and-configure-diagnostics"></a>Een opslagaccount maken en diagnostische gegevens configureren

## <a name="create-a-storage-account"></a>Create a storage account

In deze zelfstudie gebruikt de toepassingsgateway een opslagaccount voor het opslaan van gegevens voor detectie- en preventiedoeleinden. U kunt ook Log Analytics of Event Hub gebruiken om gegevens vast te leggen.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Storage** en selecteer vervolgens **Opslagaccount - blob, bestand, tabel, wachtrij**.
3. Voer de naam van het opslagaccount, selecteer **Use existing** voor de resourcegroep en selecteer vervolgens **myResourceGroupAG**. In dit voorbeeld wordt de naam van het opslagaccount is *myagstore1*. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **maken**.

## <a name="configure-diagnostics"></a>Diagnostische gegevens configureren

Configureer diagnostische gegevens om gegevens vast te leggen in de logboeken ApplicationGatewayAccessLog, ApplicationGatewayPerformanceLog en ApplicationGatewayFirewallLog.

1. Klik in het menu links op **alle resources**, en selecteer vervolgens *myAppGateway*.
2. Onder controleren, klikt u op **diagnoselogboeken**.
3. Klik op **diagnostische instelling toevoegen**.
4. Voer *myDiagnosticsSettings* als de naam voor de diagnostische instellingen.
5. Selecteer **archiveren naar een opslagaccount**, en klik vervolgens op **configureren** selecteren de *myagstore1* storage-account dat u eerder hebt gemaakt.
6. Selecteer de application gateway-logboeken te verzamelen en bewaren.
7. Klik op **Opslaan**.

    ![Diagnostische gegevens configureren](./media/application-gateway-web-application-firewall-portal/application-gateway-diagnostics.png)

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Zoek op het scherm Overzicht het openbare IP-adres voor de toepassingsgateway. Klik op **Alle resources** en vervolgens op **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/application-gateway-web-application-firewall-portal/application-gateway-record-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser.

    ![Toepassingsgateway testen](./media/application-gateway-web-application-firewall-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u:

> [!div class="checklist"]
> * Een toepassingsgateway maken met WAF ingeschakeld
> * De virtuele machines die worden gebruikt als back-endservers maken
> * Een opslagaccount maken en diagnostische gegevens configureren

Voor meer informatie over Toepassingsgateways en de bijbehorende resources, gaat u naar de artikelen met procedures.
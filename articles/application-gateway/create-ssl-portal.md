---
title: Een toepassingsgateway met SSL-beëindiging - Azure-portal configureren | Microsoft Docs
description: Informatie over het configureren van een application gateway en het toevoegen van een certificaat voor SSL-beëindiging met de Azure portal.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.date: 5/15/2018
ms.author: victorh
ms.openlocfilehash: c64754595ef67b7c083ee8d47da5b412467c191b
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="configure-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>Een toepassingsgateway met SSL-beëindiging met de Azure portal configureren

U kunt de Azure portal gebruiken voor het configureren van een [toepassingsgateway](overview.md) met een certificaat voor SSL-beëindiging die virtuele machines worden gebruikt voor back-endservers.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een toepassingsgateway maken met het certificaat
> * De virtuele machines gebruikt als back-endservers maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij de Azure portal op [http://portal.azure.com](http://portal.azure.com)

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

In deze sectie gebruikt u [nieuw SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) voor het maken van een zelfondertekend certificaat dat u naar de Azure portal uploadt als u de listener voor de toepassingsgateway maakt.

Open een Windows PowerShell-venster als beheerder op de lokale computer. Voer de volgende opdracht om het certificaat te maken:

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

U ziet er ongeveer zo dit antwoord:

```
PSParentPath: Microsoft.PowerShell.Security\Certificate::LocalMachine\my

Thumbprint                                Subject
----------                                -------
E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630  CN=www.contoso.com

Use [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) with the Thumbprint that was returned to export a pfx file from the certificate:
```

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate \
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 \
  -FilePath c:\appgwcert.pfx \
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

Een virtueel netwerk is vereist voor de communicatie tussen de bronnen die u maakt. Twee subnetten worden gemaakt in dit voorbeeld: één voor de toepassingsgateway, en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Networking** en selecteer vervolgens **Application Gateway** in de lijst met aanbevolen.
3. Voer *myAppGateway* voor de naam van de toepassingsgateway en *myResourceGroupAG* voor de nieuwe resourcegroep.
4. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
5. Klik op **Kies een virtueel netwerk**, klikt u op **nieuw**, en voer deze waarden voor het virtuele netwerk:

    - *myVNet* - voor de naam van het virtuele netwerk.
    - *10.0.0.0/16* - voor de adresruimte van het virtuele netwerk.
    - *myAGSubnet* - voor de subnetnaam.
    - *10.0.0.0/24* - voor de adresruimte van het subnet.

    ![Virtueel netwerk maken](./media/create-ssl-portal/application-gateway-vnet.png)

6. Klik op **OK** voor het maken van het virtuele netwerk en subnet.
7. Klik op **Kies een openbaar IP-adres**, klikt u op **nieuw**, en voer vervolgens de naam van het openbare IP-adres. In dit voorbeeld wordt het openbare IP-adres met de naam *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik vervolgens op **OK**.
8. Klik op **HTTPS** voor het protocol van de listener en zorg ervoor dat de poort is gedefinieerd als **443**.
9. Klik op het mappictogram en blader naar de *appgwcert.pfx* certificaat dat u eerder hebt gemaakt om te uploaden.
10. Voer *mycert1* voor de naam van het certificaat en *Azure123456!* voor het wachtwoord en klik vervolgens op **OK**.

    ![Nieuwe toepassingsgateway maken](./media/create-ssl-portal/application-gateway-create.png)

11. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de toepassingsgateway te maken. Duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie wordt voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik op **alle resources** in het menu links en klik vervolgens op **myVNet** uit de lijst met resources.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/create-ssl-portal/application-gateway-subnet.png)

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

    ![Aangepaste extensie installeren](./media/create-ssl-portal/application-gateway-extension.png)

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
4. Klik op **back-endpools**. Een standaardgroep is automatisch gemaakt met de toepassingsgateway. Klik op **appGateayBackendPool**.
5. Klik op **toevoegen doel** elke virtuele machine die u hebt gemaakt toevoegen aan de back-endpool.

    ![Back-endservers toevoegen](./media/create-ssl-portal/application-gateway-backend.png)

6. Klik op **Opslaan**.

## <a name="test-the-application-gateway"></a>Testen van de toepassingsgateway

1. Klik op **alle resources**, en klik vervolgens op **myAGPublicIPAddress**.

    ![Record application gateway openbaar IP-adres](./media/create-ssl-portal/application-gateway-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De beveiligingswaarschuwing accepteren als u een zelfondertekend certificaat gebruikt, selecteert u Details en ga vervolgens naar de webpagina:

    ![Waarschuwing beveiligen](./media/create-ssl-portal/application-gateway-secure.png)

    Uw beveiligde IIS-website wordt vervolgens weergegeven zoals in het volgende voorbeeld:

    ![Basis-URL te testen in de toepassingsgateway](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een toepassingsgateway maken met het certificaat
> * De virtuele machines gebruikt als back-endservers maken

Blijven de artikelen voor meer informatie over Toepassingsgateways en de bijbehorende resources.
---
title: Een toepassingsgateway configureren met SSL-beëindiging - Azure portal | Microsoft Docs
description: Informatie over het configureren van een application gateway en het toevoegen van een certificaat voor SSL-beëindiging met Azure portal.
services: application-gateway
author: vhorne
manager: jpconnock
editor: tysonn
tags: azure-resource-manager
ms.service: application-gateway
ms.topic: article
ms.date: 5/15/2018
ms.author: victorh
ms.openlocfilehash: 92db27aa486936d53c2e2e1c92db7d728b7d99c5
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58091831"
---
# <a name="configure-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>Een toepassingsgateway met SSL-beëindiging met Azure portal configureren

U kunt de Azure-portal gebruiken om te configureren een [toepassingsgateway](overview.md) met een certificaat voor SSL-beëindiging die gebruikmaakt van virtuele machines voor back-endservers.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een toepassingsgateway maken met behulp van het certificaat
> * De virtuele machines die worden gebruikt als back-endservers maken

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.

Aanmelden bij Azure Portal op [https://portal.azure.com](https://portal.azure.com)

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

In deze sectie maakt u [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) te maken van een zelfondertekend certificaat dat u naar Azure portal uploadt wanneer u de listener voor de toepassingsgateway maakt.

Open een Windows PowerShell-venster als beheerder op de lokale computer. Voer de volgende opdracht om het certificaat te maken:

```powershell
New-SelfSignedCertificate \
  -certstorelocation cert:\localmachine\my \
  -dnsname www.contoso.com
```

U ziet er ongeveer als deze reactie:

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

U hebt een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Klik op **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer *myAppGateway* voor de naam van de toepassingsgateway en *myResourceGroupAG* voor de nieuwe resourcegroep.
4. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
5. Klik op **Een virtueel netwerk kiezen**, klik op **Nieuw maken** en voer deze waarden in voor het virtuele netwerk:

   - *myVnet* als de naam van het virtuele netwerk.
   - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
   - *myAGSubnet* als de naam van het subnet.
   - *10.0.0.0/24* als de adresruimte van het subnet.

     ![Virtueel netwerk maken](./media/create-ssl-portal/application-gateway-vnet.png)

6. Klik op **OK** om het virtuele netwerk en subnet te maken.
7. Klik op **een openbaar IP-adres kiezen**, klikt u op **nieuw**, en voer de naam van het openbare IP-adres. In dit voorbeeld is de naam van het openbare IP-adres *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en klik op **OK**.
8. Klik op **HTTPS** voor het protocol van de listener en zorg ervoor dat de poort is gedefinieerd als **443**.
9. Klik op het mappictogram en blader naar de *appgwcert.pfx* certificaat dat u eerder hebt gemaakt om te uploaden.
10. Voer *mycert1* voor de naam van het certificaat en *Azure123456!* voor het wachtwoord en klik vervolgens op **OK**.

    ![Nieuwe toepassingsgateway maken](./media/create-ssl-portal/application-gateway-create.png)

11. Controleer de instellingen op de pagina Samenvatting en klik vervolgens op **OK** om de netwerkbronnen en de application gateway te maken. Het duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Klik in het menu links op **Alle resources** en vervolgens in de lijst met resources op **myVNet**.
2. Klik op **subnetten**, en klik vervolgens op **Subnet**.

    ![Subnet maken](./media/create-ssl-portal/application-gateway-subnet.png)

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

    ![Aangepaste extensie installeren](./media/create-ssl-portal/application-gateway-extension.png)

2. Voer de volgende opdracht uit om IIS op de virtuele machine te installeren: 

    ```azurepowershell-interactive
    Set-AzVMExtension `
      -ResourceGroupName myResourceGroupAG `
      -ExtensionName IIS `
      -VMName myVM `
      -Publisher Microsoft.Compute `
      -ExtensionType CustomScriptExtension `
      -TypeHandlerVersion 1.4 `
      -SettingString '{"commandToExecute":"powershell Add-WindowsFeature Web-Server; powershell Add-Content -Path \"C:\\inetpub\\wwwroot\\Default.htm\" -Value $($env:computername)"}' `
      -Location EastUS
    ```

3. Maak een tweede virtuele machine en installeer IIS met behulp van de stappen die u zojuist hebt voltooid. Voer *myVM2* voor de naam en VMName in Set-AzVMExtension.

### <a name="add-backend-servers"></a>Back-endservers toevoegen

1. Klik op **Alle resources** en vervolgens op **myAppGateway**.
1. Klik op **Back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. Klik op **appGatewayBackendPool**.
1. Klik op **toevoegen doel** elke virtuele machine die u hebt gemaakt aan de back-endpool toevoegen.

    ![Back-endservers toevoegen](./media/create-ssl-portal/application-gateway-backend.png)

1. Klik op **Opslaan**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Klik op **alle resources**, en klik vervolgens op **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/create-ssl-portal/application-gateway-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De beveiligingswaarschuwing accepteren als u een zelfondertekend certificaat gebruikt, selecteert u Details en ga vervolgens naar de webpagina:

    ![Beveiligingswaarschuwing](./media/create-ssl-portal/application-gateway-secure.png)

    Uw beveiligde IIS-website wordt vervolgens weergegeven zoals in het volgende voorbeeld:

    ![Basis-URL testen in de toepassingsgateway](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een toepassingsgateway maken met behulp van het certificaat
> * De virtuele machines die worden gebruikt als back-endservers maken

Voor meer informatie over Toepassingsgateways en de bijbehorende resources, gaat u naar de artikelen met procedures.

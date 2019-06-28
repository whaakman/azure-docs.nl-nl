---
title: 'Zelfstudie: een toepassingsgateway met SSL-beëindiging - Azure-portal configureren'
description: In deze zelfstudie leert u hoe u een toepassingsgateway configureren en het toevoegen van een certificaat voor SSL-beëindiging met Azure portal.
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: tutorial
ms.date: 4/17/2019
ms.author: victorh
ms.openlocfilehash: ed4230969e81eee0d77b7e4b69eac3a264068388
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67449170"
---
# <a name="tutorial-configure-an-application-gateway-with-ssl-termination-using-the-azure-portal"></a>Zelfstudie: Een toepassingsgateway met SSL-beëindiging met Azure portal configureren

U kunt de Azure-portal gebruiken om te configureren een [toepassingsgateway](overview.md) met een certificaat voor SSL-beëindiging die gebruikmaakt van virtuele machines voor back-endservers.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een zelfondertekend certificaat maken
> * Een toepassingsgateway maken met behulp van het certificaat
> * De virtuele machines die worden gebruikt als back-endservers maken
> * De toepassingsgateway testen

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com)

## <a name="create-a-self-signed-certificate"></a>Een zelfondertekend certificaat maken

In deze sectie maakt u [New-SelfSignedCertificate](https://docs.microsoft.com/powershell/module/pkiclient/new-selfsignedcertificate) een zelf-ondertekend certificaat maken. Upload het certificaat naar de Azure-portal wanneer u de listener voor de toepassingsgateway maakt.

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
```

Gebruik [Export-PfxCertificate](https://docs.microsoft.com/powershell/module/pkiclient/export-pfxcertificate) met de vingerafdruk die is geretourneerd naar een pfx-bestand van het certificaat te exporteren:

```powershell
$pwd = ConvertTo-SecureString -String "Azure123456!" -Force -AsPlainText
Export-PfxCertificate \
  -cert cert:\localMachine\my\E1E81C23B3AD33F9B4D1717B20AB65DBB91AC630 \
  -FilePath c:\appgwcert.pfx \
  -Password $pwd
```

## <a name="create-an-application-gateway"></a>Een toepassingsgateway maken

U hebt een virtueel netwerk nodig voor communicatie tussen de resources die u maakt. In dit voorbeeld worden twee subnetten gemaakt: één voor de toepassingsgateway en de andere voor de back-endservers. U kunt een virtueel netwerk maken op hetzelfde moment dat u de toepassingsgateway maakt.

1. Selecteer **nieuw** gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Netwerken** en vervolgens **Application Gateway** in de lijst Aanbevolen.
3. Voer *myAppGateway* voor de naam van de toepassingsgateway en *myResourceGroupAG* voor de nieuwe resourcegroep.
4. Accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**.
5. Selecteer **een virtueel netwerk kiezen**, selecteer **nieuw**, en voer vervolgens deze waarden voor het virtuele netwerk:

   - *myVnet* als de naam van het virtuele netwerk.
   - *10.0.0.0/16* als de adresruimte van het virtuele netwerk.
   - *myAGSubnet* als de naam van het subnet.
   - *10.0.0.0/24* als de adresruimte van het subnet.

     ![Virtueel netwerk maken](./media/create-ssl-portal/application-gateway-vnet.png)

6. Selecteer **OK** te maken van het virtuele netwerk en subnet.
7. Selecteer **een openbaar IP-adres kiezen**, selecteer **nieuw**, en voer de naam van het openbare IP-adres. In dit voorbeeld is de naam van het openbare IP-adres *myAGPublicIPAddress*. Accepteer de standaardwaarden voor de overige instellingen en selecteer **OK**.
8. Selecteer **HTTPS** voor het protocol van de listener en zorg ervoor dat de poort is gedefinieerd als **443**.
9. Selecteer het mappictogram en blader naar de *appgwcert.pfx* certificaat dat u eerder hebt gemaakt om te uploaden.
10. Voer *mycert1* voor de naam van het certificaat en *Azure123456!* voor het wachtwoord en selecteer vervolgens **OK**.

    ![Nieuwe toepassingsgateway maken](./media/create-ssl-portal/application-gateway-create.png)

11. Controleer de instellingen op de pagina Samenvatting en selecteer vervolgens **OK** om de netwerkbronnen en de application gateway te maken. Het duurt enkele minuten voor de toepassingsgateway worden gemaakt, wacht totdat de implementatie is voltooid voordat u doorgaat met de volgende sectie.

### <a name="add-a-subnet"></a>Een subnet toevoegen

1. Selecteer **alle resources** in het menu links, en selecteer vervolgens **myVNet** in de lijst met resources.
2. Selecteer **subnetten**, en selecteer vervolgens **Subnet**.

    ![Subnet maken](./media/create-ssl-portal/application-gateway-subnet.png)

3. Voer *myBackendSubnet* voor de naam van het subnet en selecteer vervolgens **OK**.

## <a name="create-backend-servers"></a>Back-endservers maken

In dit voorbeeld maakt u twee virtuele machines die worden gebruikt als back-endservers voor application gateway. U kunt ook IIS installeren op de virtuele machines om te controleren of dat de application gateway is gemaakt.

### <a name="create-a-virtual-machine"></a>Een virtuele machine maken

1. Selecteer **Nieuw**.
2. Selecteer **Compute** en selecteer vervolgens **Windows Server 2016 Datacenter** in de lijst Aanbevolen.
3. Voer deze waarden in voor de virtuele machine:

    - *myVM* als naam van de virtuele machine.
    - *azureuser* als gebruikersnaam van de beheerder.
    - *Azure123456!* als het wachtwoord.
    - Selecteer **Bestaande gebruiken** en selecteer *myResourceGroupAG*.

4. Selecteer **OK**.
5. Selecteer **DS1_V2** voor de grootte van de virtuele machine en selecteer **Selecteer**.
6. Zorg ervoor dat **myVNet** is geselecteerd voor het virtuele netwerk en dat het subnet **myBackendSubnet** is. 
7. Selecteer **Uitgeschakeld** om diagnostische gegevens over opstarten uit te schakelen.
8. Selecteer **OK**, controleer de instellingen op de overzichtspagina en selecteer **Maken**.

### <a name="install-iis"></a>IIS installeren

1. Open de interactieve shell en controleer of deze is ingesteld op **PowerShell**.

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

1. Selecteer **Alle resources** en vervolgens **myAppGateway**.
1. Selecteer **back-endpools**. Er is automatisch een standaardpool gemaakt met de toepassingsgateway. select **appGatewayBackendPool**.
1. Selecteer **toevoegen doel** elke virtuele machine die u hebt gemaakt aan de back-endpool toevoegen.

    ![Back-endservers toevoegen](./media/create-ssl-portal/application-gateway-backend.png)

1. Selecteer **Opslaan**.

## <a name="test-the-application-gateway"></a>De toepassingsgateway testen

1. Selecteer **alle resources**, en selecteer vervolgens **myAGPublicIPAddress**.

    ![Registreer het openbare IP-adres van de toepassingsgateway](./media/create-ssl-portal/application-gateway-ag-address.png)

2. Kopieer het openbare IP-adres en plak het in de adresbalk van de browser. De beveiligingswaarschuwing accepteren als u een zelfondertekend certificaat gebruikt, selecteert u Details en ga vervolgens naar de webpagina:

    ![Beveiligingswaarschuwing](./media/create-ssl-portal/application-gateway-secure.png)

    Uw beveiligde IIS-website wordt vervolgens weergegeven zoals in het volgende voorbeeld:

    ![Basis-URL testen in de toepassingsgateway](./media/create-ssl-portal/application-gateway-iistest.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Meer informatie over wat u met Azure Application Gateway doen kunt](application-gateway-introduction.md)

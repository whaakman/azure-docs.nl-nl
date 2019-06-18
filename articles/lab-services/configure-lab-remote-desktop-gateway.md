---
title: Configureren van een lab voor het gebruik van extern bureaublad-Gateway in Azure DevTest Labs | Microsoft Docs
description: Informatie over het configureren van een lab in Azure DevTest Labs met een extern bureaublad-gateway om te controleren of beveiligde toegang tot het lab-virtuele machines zonder te hoeven de RDP-poort.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: 430734878c01d10a4e7dd385dc75d8d502a2d82c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079000"
---
# <a name="configure-your-lab-in-azure-devtest-labs-to-use-a-remote-desktop-gateway"></a>Uw lab in Azure DevTest Labs te gebruiken van een extern bureaublad-gateway configureren
U kunt een extern bureaublad-gateway voor uw lab voor beveiligde toegang tot de lab-virtuele machines (VM's) zonder dat om beschikbaar te stellen de RDP-poort configureren in Azure DevTest Labs. De testomgeving biedt een centrale locatie voor uw labgebruikers kunnen zien en verbinding maken met alle virtuele machines die ze toegang hebben. De **Connect** knop op de **virtuele Machine** pagina maakt een machine-specifieke RDP-bestand dat u openen kunt om verbinding maken met de machine. U kunt verder aanpassen en de RDP-verbinding beveiligen door verbinding te maken van uw testomgeving met een extern bureaublad-gateway. 

Deze methode is veiliger omdat de lab-gebruiker rechtstreeks geverifieerd op de gatewaycomputer of bedrijfsreferenties op een domein gatewaycomputer verbinding maken met hun machines kunt gebruiken. De testomgeving ondersteunt ook het gebruik van token verificatie bij de gatewaycomputer waarmee gebruikers verbinding maken met hun virtuele machines zonder de RDP-poort blootgesteld aan internet. In dit artikel leidt u door een voorbeeld over het instellen van een testomgeving die gebruikmaakt van tokenverificatie verbinding maken met lab-machines.

## <a name="architecture-of-the-solution"></a>Architectuur van de oplossing

![Architectuur van de oplossing](./media/configure-lab-remote-desktop-gateway/architecture.png)

1. De [bestandsinhoud ophalen RDP](/rest/api/dtl/virtualmachines/getrdpfilecontents) actie wordt aangeroepen wanneer u selecteert de **Connect** button.1. 
1. De actie ophalen RDP-bestand inhoud roept `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` om aan te vragen een verificatietoken.
    1. `{gateway-hostname}` de gateway-hostnaam die is opgegeven op de **Lab instellingen** pagina voor uw lab in Azure portal. 
    1. `{lab-machine-name}` is de naam van de computer waarop u verbinding wilt maken.
    1. `{port-number}` is de poort waarop de verbinding moet worden gemaakt. Meestal is deze poort 3389. Als het lab VM maakt gebruik van de [gedeelde IP](devtest-lab-shared-ip.md) functie in DevTest Labs, de poort zijn verschillend.
1. De extern bureaublad-gateway wordt uitgesteld de aanroep van `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` naar een Azure-functie om de verificatietoken te genereren. De DevTest Labs-service omvat automatisch de functie-sleutel in de aanvraagheader. De functie-sleutel is om te worden opgeslagen in de sleutelkluis van het lab. De naam van die geheim moeten worden weergegeven als **Gateway-tokengeheim** op de **Lab instellingen** pagina voor de testomgeving.
1. De Azure-functie wordt verwacht om terug te keren een token voor token verificatie op basis van certificaten bij de gatewaycomputer.  
1. Het ophalen van RDP-bestand actie-inhoud en geeft vervolgens het volledige RDP-bestand, met inbegrip van de verificatie-informatie.
1. U opent het RDP-bestand met behulp van uw voorkeur programma voor de RDP-verbinding. Houd er rekening mee dat niet alle programma's voor RDP-verbinding tokenverificatie ondersteunen. Het verificatietoken beschikt over een vervaldatum, ingesteld door de functie-app. Controleer de verbinding met de virtuele machine van de testomgeving voordat het token is verlopen.
1. Zodra de computer extern bureaublad-gateway het token in het RDP-bestand verifieert, wordt de verbinding wordt doorgestuurd naar uw lab-machine.

### <a name="solution-requirements"></a>Oplossingsvereisten
Als u wilt werken met de functie voor DevTest Labs-tokenverificatie, zijn er een paar configuratievereisten voor de gatewaymachines, functies en domeinnaamservices (DNS).

### <a name="requirements-for-remote-desktop-gateway-machines"></a>Vereisten voor extern bureaublad-gateway-machines
- SSL-certificaat moet worden geïnstalleerd op de gatewaycomputer om af te handelen HTTPS-verkeer. Het certificaat moet overeenkomen met de volledig gekwalificeerde domeinnaam (FQDN) van de load balancer voor de gateway-farm of de FQDN-naam van de machine zelf als er slechts één machine. Jokerteken SSL-certificaten werken niet.  
- Een handtekeningcertificaat op gateway computer (s) is geïnstalleerd. Een ondertekend certificaat maken met behulp van [maken SigningCertificate.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1) script.
- Installeer de [Pluggable verificatie](https://code.msdn.microsoft.com/windowsdesktop/Remote-Desktop-Gateway-517d6273) module die ondersteuning biedt voor tokenverificatie voor extern bureaublad-gateway. Een voorbeeld van een dergelijke module is `RDGatewayFedAuth.msi` die wordt geleverd met [System Center Virtual Machine Manager (VMM)-installatiekopieën](/system-center/vmm/install-console?view=sc-vmm-1807). Zie voor meer informatie over System Center, [documentatie voor System Center](https://docs.microsoft.com/system-center/) en [prijsinformatie](https://www.microsoft.com/cloud-platform/system-center-pricing).  
- De gateway-server kan verwerken aanvragen voor `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}`.

    De hostnaam van de gateway is de FQDN-naam van de load balancer van de gateway-farm of de FQDN-naam van de computer zelf als er slechts één machine. De `{lab-machine-name}` is de naam van het lab-machine die u probeert om verbinding te maken en de `{port-number}` is poort waarop de verbinding wordt gemaakt.  Standaard is deze poort 3389.  Echter, als de virtuele machine wordt met behulp van de [gedeelde IP](devtest-lab-shared-ip.md) onderdeel in DevTest Labs, de poort kan worden verschillende.
- De [routering toepassingsaanvraag](/iis/extensions/planning-for-arr/using-the-application-request-routing-module) module voor Internet Information Server (IIS) kan worden gebruikt om te leiden `https://{gateway-hostname}/api/host/{lab-machine-name}/port/{port-number}` aanvragen naar de azure-functie, waar de aanvraag voor het ophalen van een token voor verificatie.


## <a name="requirements-for-azure-function"></a>Vereisten voor Azure-functie
Azure-functie verwerkt de aanvraag met de indeling van `https://{function-app-uri}/app/host/{lab-machine-name}/port/{port-number}` en retourneert het verificatietoken op basis van hetzelfde certificaat geïnstalleerd op de gatewaymachines. De `{function-app-uri}` is de uri die wordt gebruikt voor toegang tot de functie. De functie-sleutel wordt automatisch in de koptekst van de aanvraag worden doorgegeven. Zie voor een voorbeeld van de functie, [ https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs ](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/src/RDGatewayAPI/Functions/CreateToken.cs). 


## <a name="requirements-for-network"></a>Vereisten voor netwerk

- DNS voor de FQDN-naam die is gekoppeld aan het SSL-certificaat geïnstalleerd op de gatewaymachines moet verkeer naar het gateway-apparaat of de load balancer van de gateway-machine-farm te regelen.
- Als de machine lab maakt gebruik van privé-IP's, moet er een netwerkpad van het gateway-apparaat naar de machine lab, hetzij door middel van delen van hetzelfde virtuele netwerk of met behulp van gekoppelde virtuele netwerken.

## <a name="configure-the-lab-to-use-token-authentication"></a>De testomgeving voor het gebruik van tokenverificatie configureren 
Deze sectie wordt beschreven hoe u een lab voor het gebruik van een extern bureaublad-gateway-machine die ondersteuning biedt voor token-verificatie configureren. In deze sectie wordt niet uitgelegd hoe u voor het instellen van een farm met extern bureaublad-gateway zelf. Voor die informatie, Zie de [voorbeeld om te maken van een extern bureaublad-gateway](#sample-to-create-a-remote-desktop-gateway) sectie aan het einde van dit artikel. 

Voordat u de instellingen voor lab bijwerkt, slaat u de sleutel die nodig zijn voor de functie als u wilt een verificatietoken in de sleutelkluis van de testomgeving is uitgevoerd. U kunt de waarde van de functie sleutel krijgen in de **beheren** pagina voor de functie in Azure portal. Zie voor meer informatie over hoe u een geheim opslaan in een key vault [een geheim toevoegen aan Key Vault](../key-vault/quick-create-portal.md#add-a-secret-to-key-vault). De naam van de geheime sleutel voor later gebruik opslaan.

Voer de volgende Azure CLI-opdracht om de ID van de sleutelkluis van het lab te: 

```azurecli
az resource show --name {lab-name} --resource-type 'Microsoft.DevTestLab/labs' --resource-group {lab-resource-group-name} --query properties.vaultName
```

Configureer de testomgeving voor het gebruik van de token verificatie met behulp van deze stappen:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **alle Services**, en selecteer vervolgens **DevTest Labs** in de lijst.
1. Selecteer in de lijst met labs, uw **lab**.
1. Selecteer op de pagina van het lab **configuratie en het beleid**.
1. In het menu links in de **instellingen** sectie, selecteer **Lab instellingen**.
1. In de **extern bureaublad** sectie voert u de volledig gekwalificeerde domeinnaam (FQDN) of IP-adres van de gatewaycomputer extern bureaublad-services of farm voor de **Gateway hostnaam** veld. Deze waarde moet overeenkomen met de FQDN-naam van het SSL-certificaat op de gatewaymachines gebruikt.

    ![Extern bureaublad-opties in het lab-instellingen](./media/configure-lab-remote-desktop-gateway/remote-desktop-options-in-lab-settings.png)
1. In de **extern bureaublad** sectie voor **gatewaytoken** geheim, voer de naam van het geheim dat eerder hebt gemaakt. Deze waarde is niet de functie sleutel zelf, maar de naam van het geheim van de testomgeving sleutelkluis waarin de functie-sleutel.

    ![Gateway-tokengeheim in lab-instellingen](./media/configure-lab-remote-desktop-gateway/gateway-token-secret.png)
1. **Sla** wijzigingen.

    > [!NOTE] 
    > Door te klikken op **opslaan**, gaat u akkoord met [licentievoorwaarden van extern bureaublad-Gateway](https://www.microsoft.com/licensing/product-licensing/products). Zie voor meer informatie over externe gateway [Welkom bij de extern bureaublad-Services](https://aka.ms/rds) en [uw extern bureaublad-omgeving implementeren](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure).


Als het configureren van het lab via automation heeft de voorkeur, Zie [Set DevTestLabGateway.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Set-DevTestLabGateway.ps1) voor een PowerShell-voorbeeldscript om in te stellen **gateway hostnaam** en **gateway-tokengeheim**instellingen. De [Azure DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab) biedt ook een Azure Resource Manager-sjabloon maakt of updatet een testomgeving met de **gateway hostnaam** en **gateway-tokengeheim**instellingen.

## <a name="configure-network-security-group"></a>Configureren van netwerkbeveiligingsgroep
Als u wilt beveiligen verder in het lab, kan een netwerkbeveiligingsgroep (NSG) worden toegevoegd aan het virtuele netwerk die worden gebruikt door de virtuele machines. Voor instructies over het instellen van een NSG, Zie [maken, wijzigen of verwijderen van een netwerkbeveiligingsgroep](../virtual-network/manage-network-security-group.md).

Hier volgt een voorbeeld NSG waarmee alleen verkeer dat eerst wordt verstuurd via de gateway te bereiken testcomputers. De bron in deze regel is het IP-adres van de machine één gateway, of het IP-adres van de load balancer voor de gatewaymachines.

![Netwerkbeveiligingsgroep - regels](./media/configure-lab-remote-desktop-gateway/network-security-group-rules.png)

## <a name="sample-to-create-a-remote-desktop-gateway"></a>Voorbeeld van een extern bureaublad-gateway maken

> [!NOTE] 
> Met behulp van de voorbeeldsjablonen, gaat u akkoord met [licentievoorwaarden van extern bureaublad-Gateway](https://www.microsoft.com/licensing/product-licensing/products). Zie voor meer informatie over externe gateway [Welkom bij de extern bureaublad-Services](https://aka.ms/rds) en [uw extern bureaublad-omgeving implementeren](/windows-server/remote/remote-desktop-services/rds-deploy-infrastructure).

De [Azure DevTest Labs GitHub-opslagplaats](https://github.com/Azure/azure-devtestlab) bevat enkele voorbeelden om te zorgen ervoor dat setup de resources die nodig zijn voor gebruik van tokenverificatie en extern bureaublad-gateway met DevTest Labs. Deze voorbeelden bevatten Azure Resource Manager-sjablonen voor gatewaymachines, lab-instellingen en functie-app.

Volg deze stappen voor het instellen van een oplossing voor de extern bureaublad-gateway-farm.

1. Maak een certificaat voor ondertekening.  Voer [maken SigningCertificate.ps1](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/tools/Create-SigningCertificate.ps1). Sla de vingerafdruk, wachtwoord en Base64-codering van het gemaakte certificaat.
2. Een SSL-certificaat ophalen. FQDN-naam die is gekoppeld aan het SSL-certificaat moet voor het domein dat u beheert. Sla de vingerafdruk, wachtwoord en Base64-codering voor dit certificaat. Als u de vingerafdruk met behulp van PowerShell, gebruikt u de volgende opdrachten.

    ```powershell
    $cer = New-Object System.Security.Cryptography.X509Certificates.X509Certificate;
    $cer.Import(‘path-to-certificate’);
    $hash = $cer.GetCertHashString()
    ```

    Als u de Base64-codering met behulp van PowerShell, gebruikt u de volgende opdracht uit.

    ```powershell
    [System.Convert]::ToBase64String([System.IO.File]::ReadAllBytes(‘path-to-certificate’))
    ```
3. Bestanden downloaden [ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway).

    De sjabloon vereist toegang tot een paar andere Resource Manager-sjablonen en gerelateerde bronnen op de dezelfde basis-URI. Kopieer alle bestanden uit de [ https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/blob/master/samples/DevTestLabs/GatewaySample/arm/gateway) en RDGatewayFedAuth.msi naar een blobcontainer in een storage-account.  
4. Implementeer **azuredeploy.json** van [ https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway ](https://github.com/Azure/azure-devtestlab/tree/master/samples/DevTestLabs/GatewaySample/arm/gateway). De sjabloon bevat de volgende parameters:
    - adminUsername – vereist.  Gebruikersnaam van beheerder voor de gatewaymachines.
    - adminPassword – vereist. Wachtwoord voor het beheerdersaccount voor de gatewaymachines.
    - instanceCount: aantal gatewaymachines te maken.  
    - alwaysOn: geeft aan of u wilt voorkomen dat u de gemaakte Azure Functions-app in een warme staat of niet. De Azure Functions-app houden voorkomen vertragingen wanneer gebruikers eerst proberen verbinding maken met hun lab-VM, maar deze kosten gevolgen heeft.  
    - tokenLifetime – de hoeveelheid tijd die het gemaakte token ongeldig is. Notatie is UU: mm:.
    - sslCertificate – de met Base64-codering van het SSL-certificaat voor de gatewaycomputer.
    - sslCertificatePassword: het wachtwoord van het SSL-certificaat voor de gatewaycomputer.
    - sslCertificateThumbprint - de vingerafdruk van het certificaat voor de identificatie in het lokale certificaatarchief van de SSL-certificaat.
    - signCertificate – de met Base64 coderen voor het ondertekenen van certificaat voor de gatewaycomputer.
    - signCertificatePassword: het wachtwoord voor het ondertekenen van certificaat voor de gatewaycomputer.
    - signCertificateThumbprint - de vingerafdruk van het certificaat voor de identificatie in het lokale certificaatarchief van het certificaat voor ondertekening.
    - _artifactsLocation-URI-locatie waar alle ondersteunende resources kunnen worden gevonden. Deze waarde moet een volledig gekwalificeerde UIR, niet een relatief pad zijn.
    - _artifactsLocationSasToken – de Shared Access Signature (SAS)-token gebruikt voor toegang tot ondersteunende resources, als de locatie een Azure storage-account is.

    De sjabloon kan worden geïmplementeerd met behulp van de Azure CLI met behulp van de volgende opdracht uit:

    ```azurecli
    az group deployment create --resource-group {resource-group} --template-file azuredeploy.json --parameters @azuredeploy.parameters.json -–parameters _artifactsLocation=”{storage-account-endpoint}/{container-name}” -–parameters _artifactsLocationSasToken = “?{sas-token}”
    ```

    Hier volgen de beschrijvingen van de parameters:

    - Het {storage-account-eindpunt} kan worden verkregen door het uitvoeren van `az storage account show --name {storage-acct-name} --query primaryEndpoints.blob`.  De {opslag-acct-name} is de naam van het opslagaccount waarin de bestanden die u hebt geüpload.  
    - De {container-name} is de naam van de container in de {opslag-acct-naam} met bestanden die u hebt geüpload.  
    - De {sas-token} kan worden verkregen door het uitvoeren van `az storage container generate-sas --name {container-name} --account-name {storage-acct-name} --https-only –permissions drlw –expiry {utc-expiration-date}`. 
        - De {opslag-acct-name} is de naam van het opslagaccount waarin de bestanden die u hebt geüpload.  
        - De {container-name} is de naam van de container in de {opslag-acct-naam} met bestanden die u hebt geüpload.  
        - De {utc--vervaldatum} is de datum, in UTC, waarmee het SAS-token verloopt en de SAS-token kan niet meer worden gebruikt voor toegang tot het opslagaccount.

    Noteer de waarden voor gatewayFQDN en gatewayIP uit de uitvoer van de implementatie van de sjabloon. U moet ook om op te slaan de waarde van de functie-sleutel voor de zojuist gemaakte functie, kunt u vinden in de [functie app-instellingen](../azure-functions/functions-how-to-use-azure-function-app-settings.md) tabblad.
5. DNS configureren zodat deze FQDN-naam van een SSL-certificaat naar de IP-adres van gatewayIP uit de vorige stap verwijst.

    Nadat de extern bureaublad-Gateway-farm is gemaakt en de juiste DNS-updates zijn aangebracht, is het klaar om te worden gebruikt door een lab in DevTest Labs. De **gateway hostnaam** en **gateway-tokengeheim** instellingen moeten worden geconfigureerd voor het gebruik van de gateway computer (s) u hebt geïmplementeerd. 

    > [!NOTE]
    > Als de machine lab maakt gebruik van privé-IP's, moet er een netwerkpad van het gateway-apparaat naar de machine lab, hetzij door middel van delen van hetzelfde virtuele netwerk of met behulp van een gekoppeld virtueel netwerk.

    Wanneer zowel de gateway en de lab zijn geconfigureerd, het verbindingsbestand gemaakt wanneer de gebruiker van de testomgeving op de **Connect** bevatten informatie die nodig is om te verbinden via tokenverificatie automatisch.     

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel voor meer informatie over extern bureaublad-Services: [Documentatie voor extern bureaublad-Services](/windows-server/remote/remote-desktop-services/Welcome-to-rds)



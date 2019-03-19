---
title: Pakketopname gebruiken voor proactieve netwerkbewaking met waarschuwingen en Azure Functions | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een waarschuwing geactiveerd pakketopname maken met Azure Network Watcher
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: ''
ms.assetid: 75e6e7c4-b3ba-4173-8815-b00d7d824e11
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.openlocfilehash: 71e71b417f12b58fc03c581826c0e5c2412e684b
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57876643"
---
# <a name="use-packet-capture-for-proactive-network-monitoring-with-alerts-and-azure-functions"></a>Pakketopname gebruiken voor proactieve netwerkbewaking met waarschuwingen en Azure Functions

Network Watcher packet-capture maakt vastleggen sessies om bij te houden van verkeer voor virtuele machines. De capture-bestand hebben een filter dat is gedefinieerd voor het volgen van alleen het verkeer die u wilt bewaken. Deze gegevens worden vervolgens opgeslagen in een opslag-blob of lokaal op de gastmachine.

Deze mogelijkheid kan op afstand worden gestart vanuit andere automation-scenario's zoals Azure Functions. Pakketopname biedt u de mogelijkheid om uit te voeren proactieve opnamen op basis van gedefinieerde netwerk afwijkingen. Andere toepassingen zijn onder andere de netwerkstatistieken, het ophalen van gegevens over network hackers, foutopsporing client-servercommunicatie en informatie te verzamelen.

Resources die zijn geïmplementeerd in Azure 24/7 worden uitgevoerd. U en uw personeel, kan niet de status van alle resources 24/7 actief bewaken. Wat gebeurt bijvoorbeeld als er een probleem optreedt om 2 uur?

Met behulp van Network Watcher, waarschuwingen en functies van binnen het Azure-ecosysteem, kunt u proactief reageren met de gegevens en hulpprogramma's voor het oplossen van problemen in uw netwerk.

![Scenario][scenario]

## <a name="prerequisites"></a>Vereisten

* De nieuwste versie van [Azure PowerShell](/powershell/azure/azurerm/install-azurerm-ps).
* Een bestaand exemplaar van Network Watcher. Als u er nog geen hebt, [Maak een instantie van Network Watcher](network-watcher-create.md).
* Een bestaande virtuele machine in dezelfde regio als Network Watcher met de [Windows extensie](../virtual-machines/windows/extensions-nwa.md) of [Linux virtuele machine-extensie](../virtual-machines/linux/extensions-nwa.md).

## <a name="scenario"></a>Scenario

In dit voorbeeld wordt de virtuele machine verzendt meer TCP-segmenten dan normaal en u wilt worden gewaarschuwd. TCP-segmenten worden gebruikt als hier een voorbeeld, maar kunt u de voorwaarde voor elke waarschuwing.

Wanneer u wordt gewaarschuwd, die u wilt ontvangen op pakketniveau gegevens om te begrijpen waarom communicatie is toegenomen. Vervolgens kunt u stappen om te retourneren van de virtuele machine tot normale communicatie uitvoeren.

In dit scenario wordt ervan uitgegaan dat u een bestaand exemplaar van Network Watcher en een resourcegroep met een geldige virtuele machine hebt.

Hieronder volgt een overzicht van de werkstroom die uitgevoerd wordt:

1. Een waarschuwing wordt geactiveerd op de virtuele machine.
1. De waarschuwing aanroept uw Azure-functie via een webhook.
1. Uw Azure-functie verwerkt de waarschuwing en wordt een Network Watcher packet capture-sessie gestart.
1. De pakketopname wordt uitgevoerd op de virtuele machine en verkeer verzamelt.
1. De packet capture-bestand wordt geüpload naar een opslagaccount voor controle en diagnose.

Als u wilt dit proces automatiseren, we maken en verbinding maken met een waarschuwing op de virtuele machine om te activeren wanneer het incident voordoet. We maken ook een functie aan te roepen Network Watcher.

In dit scenario doet het volgende:

* Hiermee maakt u een Azure-functie die een pakketopname begint.
* Een waarschuwingsregel op een virtuele machine maakt en configureert u de waarschuwingsregel voor het aanroepen van de Azure-functie.

## <a name="create-an-azure-function"></a>Een Azure-functie maken

De eerste stap is het maken van een Azure-functie voor het verwerken van de waarschuwing en maken van een pakketopname.

1. In de [Azure-portal](https://portal.azure.com), selecteer **een resource maken** > **Compute** > **functie-App**.

    ![Het maken van een functie-app][1-1]

2. Op de **functie-App** blade, voer de volgende waarden in en selecteer vervolgens **OK** om de app te maken:

    |**Instelling** | **Waarde** | **Details** |
    |---|---|---|
    |**Naam van app**|PacketCaptureExample|De naam van de functie-app.|
    |**Abonnement**|[Uw abonnement] Het abonnement waarvoor u wilt maken van de functie-app.||
    |**Resourcegroep**|PacketCaptureRG|De resourcegroep bevat de functie-app.|
    |**Hostingabonnement**|Verbruiksabonnement| Het type van plan bent uw functie-app gebruikt. Opties zijn verbruik of Azure App Service-plan. |
    |**Locatie**|US - centraal| De regio waarin u wilt maken van de functie-app.|
    |**Opslagaccount**|{automatisch gegenereerde}| Het opslagaccount dat Azure Functions nodig voor de opslag voor algemeen gebruik heeft.|

3. Op de **PacketCaptureExample functie-Apps** Selecteer **functies** > **aangepaste functie**  >  **+**.

4. Selecteer **HttpTrigger-Powershell**, en voer vervolgens de resterende gegevens. Selecteer ten slotte voor het maken van de functie **maken**.

    |**Instelling** | **Waarde** | **Details** |
    |---|---|---|
    |**Scenario**|Experimenteel|Type scenario|
    |**Een naam voor de functie opgeven**|AlertPacketCapturePowerShell|Naam van de functie|
    |**Verificatieniveau**|Function|Machtigingsniveau voor de functie|

![Voorbeeld van de functies][functions1]

> [!NOTE]
> De PowerShell-sjabloon is experimenteel en heeft geen volledige ondersteuning.

Aanpassingen zijn vereist voor dit voorbeeld en worden beschreven in de volgende stappen.

### <a name="add-modules"></a>Modules toevoegen

Network Watcher PowerShell-cmdlets wilt gebruiken, de meest recente PowerShell-module naar de functie-app te uploaden.

1. Voer de volgende PowerShell-opdracht op uw lokale computer met de nieuwste Azure PowerShell-modules geïnstalleerd:

    ```powershell
    (Get-Module AzureRM.Network).Path
    ```

    In dit voorbeeld geeft u het lokale pad van de Azure PowerShell-modules. Deze mappen worden gebruikt in een latere stap. De modules die worden gebruikt in dit scenario zijn:

   * AzureRM.Network

   * AzureRM.Profile

   * AzureRM.Resources

     ![PowerShell-mappen][functions5]

1. Selecteer **functie app-instellingen** > **gaat u naar App Service-Editor**.

    ![Instellingen voor functie-app][functions2]

1. Met de rechtermuisknop op de **AlertPacketCapturePowershell** map, en maak vervolgens een map met de naam **azuremodules**. 

4. Maak een submap voor elke module die u nodig hebt.

    ![Map en submappen][functions3]

    * AzureRM.Network

    * AzureRM.Profile

    * AzureRM.Resources

1. Met de rechtermuisknop op de **AzureRM.Network** submap en selecteer vervolgens **bestanden uploaden**. 

6. Ga naar uw Azure-modules. In de lokale **AzureRM.Network** map, selecteert u alle bestanden in de map. Selecteer vervolgens **OK**. 

7. Herhaal deze stappen voor **AzureRM.Profile** en **AzureRM.Resources**.

    ![Bestanden uploaden][functions6]

1. Wanneer u klaar bent, elke map moet de PowerShell-module-bestanden van uw lokale computer.

    ![PowerShell-bestanden][functions7]

### <a name="authentication"></a>Authentication

De PowerShell-cmdlets wilt gebruiken, moet u verifiëren. U configureren verificatie in de functie-app. Om verificatie te configureren, moet u omgevingsvariabelen configureren en uploaden van een bestand met een versleutelde sleutel naar de functie-app.

> [!NOTE]
> In dit scenario biedt slechts één voorbeeld van hoe u verificatie met Azure Functions implementeren. Er zijn andere manieren om dit te doen.

#### <a name="encrypted-credentials"></a>Versleutelde referenties

De volgende PowerShell-script maakt een bestand met een sleutel met de naam **PassEncryptKey.key**. Het biedt ook een versleutelde versie van het wachtwoord dat opgegeven. Dit wachtwoord is het wachtwoord dat is gedefinieerd voor de Azure Active Directory-toepassing die wordt gebruikt voor verificatie.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

In de App Service-Editor van de functie-app, maak een map genaamd **sleutels** onder **AlertPacketCapturePowerShell**. Upload de **PassEncryptKey.key** -bestand dat u hebt gemaakt in de vorige PowerShell-voorbeeld.

![Functions-sleutel][functions8]

### <a name="retrieve-values-for-environment-variables"></a>Waarden voor omgevingsvariabelen ophalen

De laatste vereiste is voor het instellen van de omgevingsvariabelen die nodig zijn voor toegang tot de waarden voor de verificatie. De volgende lijst bevat de omgevingsvariabelen die zijn gemaakt:

* AzureClientID

* AzureTenant

* AzureCredPassword


#### <a name="azureclientid"></a>AzureClientID

De client-ID is de toepassings-ID van een toepassing in Azure Active Directory.

1. Als u geen al een toepassing te gebruiken, moet u het volgende voorbeeld voor het maken van een toepassing uitvoert.

    ```powershell
    $app = New-AzureRmADApplication -DisplayName "ExampleAutomationAccount_MF" -HomePage "https://exampleapp.com" -IdentifierUris "https://exampleapp1.com/ExampleFunctionsAccount" -Password "<same password as defined earlier>"
    New-AzureRmADServicePrincipal -ApplicationId $app.ApplicationId
    Start-Sleep 15
    New-AzureRmRoleAssignment -RoleDefinitionName Contributor -ServicePrincipalName $app.ApplicationId
    ```

   > [!NOTE]
   > Het wachtwoord dat u gebruikt bij het maken van de toepassing moet hetzelfde wachtwoord dat u eerder hebt gemaakt bij het opslaan van het sleutelbestand.

1. Selecteer in de Azure portal, **abonnementen**. Selecteer het abonnement wilt gebruiken, en selecteer vervolgens **toegangsbeheer (IAM)**.

    ![Functions IAM][functions9]

1. Kiezen welk account wilt gebruiken, en selecteer vervolgens **eigenschappen**. Kopieer de toepassings-ID.

    ![Functies toepassings-ID][functions10]

#### <a name="azuretenant"></a>AzureTenant

De tenant-ID ophalen door het uitvoeren van de volgende PowerShell-voorbeeld:

```powershell
(Get-AzureRmSubscription -SubscriptionName "<subscriptionName>").TenantId
```

#### <a name="azurecredpassword"></a>AzureCredPassword

De waarde van de omgevingsvariabele AzureCredPassword is de waarde die u krijgt bij het uitvoeren van de volgende PowerShell-voorbeeld. In dit voorbeeld wordt hetzelfde account dat wordt weergegeven in de voorgaande **versleutelde referenties** sectie. De waarde die nodig is, is de uitvoer van de `$Encryptedpassword` variabele.  Dit is de service principal wachtwoord dat u versleuteld met behulp van het PowerShell-script.

```powershell
#Variables
$keypath = "C:\temp\PassEncryptKey.key"
$AESKey = New-Object Byte[] 32
$Password = "<insert a password here>"

#Keys
[Security.Cryptography.RNGCryptoServiceProvider]::Create().GetBytes($AESKey) 
Set-Content $keypath $AESKey

#Get encrypted password
$secPw = ConvertTo-SecureString -AsPlainText $Password -Force
$AESKey = Get-content $KeyPath
$Encryptedpassword = $secPw | ConvertFrom-SecureString -Key $AESKey
$Encryptedpassword
```

### <a name="store-the-environment-variables"></a>De omgevingsvariabelen Store

1. Ga naar de functie-app. Selecteer vervolgens **functie app-instellingen** > **app-instellingen configureren**.

    ![App-instellingen configureren][functions11]

1. De omgevingsvariabelen en hun waarden toevoegen aan de app-instellingen en selecteer vervolgens **opslaan**.

    ![App-instellingen][functions12]

### <a name="add-powershell-to-the-function"></a>PowerShell voor de functie toevoegen

Het is nu tijd om aanroepen in Network Watcher uit binnen de Azure-functie. Afhankelijk van de vereisten, kan de implementatie van deze functie variëren. Echter, de algemene stroom van de code is als volgt:

1. Proces-invoerparameters.
2. Bestaande query-pakket bevat om te controleren limieten en de naam van conflicten.
3. Een pakketopname maken met de juiste parameters.
4. Poll-pakket vastleggen periodiek totdat deze bewerking is voltooid.
5. De gebruiker waarschuwen dat de opnamesessie pakket voltooid is.

Het volgende voorbeeld wordt PowerShell-code die kan worden gebruikt in de functie. Er zijn waarden die worden vervangen moeten voor **subscriptionId**, **resourceGroupName**, en **storageAccountName**.

```powershell
            #Import Azure PowerShell modules required to make calls to Network Watcher
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Profile\AzureRM.Profile.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Network\AzureRM.Network.psd1" -Global
            Import-Module "D:\home\site\wwwroot\AlertPacketCapturePowerShell\azuremodules\AzureRM.Resources\AzureRM.Resources.psd1" -Global

            #Process alert request body
            $requestBody = Get-Content $req -Raw | ConvertFrom-Json

            #Storage account ID to save captures in
            $storageaccountid = "/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.Storage/storageAccounts/{storageAccountName}"

            #Packet capture vars
            $packetcapturename = "PSAzureFunction"
            $packetCaptureLimit = 10
            $packetCaptureDuration = 10

            #Credentials
            $tenant = $env:AzureTenant
            $pw = $env:AzureCredPassword
            $clientid = $env:AzureClientId
            $keypath = "D:\home\site\wwwroot\AlertPacketCapturePowerShell\keys\PassEncryptKey.key"

            #Authentication
            $secpassword = $pw | ConvertTo-SecureString -Key (Get-Content $keypath)
            $credential = New-Object System.Management.Automation.PSCredential ($clientid, $secpassword)
            Connect-AzureRmAccount -ServicePrincipal -Tenant $tenant -Credential $credential #-WarningAction SilentlyContinue | out-null


            #Get the VM that fired the alert
            if($requestBody.context.resourceType -eq "Microsoft.Compute/virtualMachines")
            {
                Write-Output ("Subscription ID: {0}" -f $requestBody.context.subscriptionId)
                Write-Output ("Resource Group:  {0}" -f $requestBody.context.resourceGroupName)
                Write-Output ("Resource Name:  {0}" -f $requestBody.context.resourceName)
                Write-Output ("Resource Type:  {0}" -f $requestBody.context.resourceType)

                #Get the Network Watcher in the VM's region
                $nw = Get-AzurermResource | Where {$_.ResourceType -eq "Microsoft.Network/networkWatchers" -and $_.Location -eq $requestBody.context.resourceRegion}
                $networkWatcher = Get-AzureRmNetworkWatcher -Name $nw.Name -ResourceGroupName $nw.ResourceGroupName

                #Get existing packetCaptures
                $packetCaptures = Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher

                #Remove existing packet capture created by the function (if it exists)
                $packetCaptures | %{if($_.Name -eq $packetCaptureName)
                { 
                    Remove-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -PacketCaptureName $packetCaptureName
                }}

                #Initiate packet capture on the VM that fired the alert
                if ((Get-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher).Count -lt $packetCaptureLimit){
                    echo "Initiating Packet Capture"
                    New-AzureRmNetworkWatcherPacketCapture -NetworkWatcher $networkWatcher -TargetVirtualMachineId $requestBody.context.resourceId -PacketCaptureName $packetCaptureName -StorageAccountId $storageaccountid -TimeLimitInSeconds $packetCaptureDuration
                    Out-File -Encoding Ascii -FilePath $res -inputObject "Packet Capture created on ${requestBody.context.resourceID}"
                }
            } 
 ``` 
#### <a name="retrieve-the-function-url"></a>De functie-URL niet ophalen 
1. Nadat u de functie hebt gemaakt, configureert u de waarschuwing voor het aanroepen van de URL die is gekoppeld aan de functie. Als u deze waarde, de functie-URL van uw functie-app te kopiëren.

    ![Zoeken naar de functie-URL][functions13]

2. Kopieer de URL van de functie voor uw functie-app.

    ![De functie-URL kopiëren][2]

Als u aangepaste eigenschappen in de nettolading van de webhook POST-aanvraag nodig hebt, raadpleegt u [een webhook configureren voor een Azure metrische waarschuwing](../azure-monitor/platform/alerts-webhooks.md).

## <a name="configure-an-alert-on-a-vm"></a>Een waarschuwing configureren op een virtuele machine

Waarschuwingen kunnen worden geconfigureerd om te melden personen wanneer een specifieke metriek een drempelwaarde die toegewezen. In dit voorbeeld wordt de waarschuwing is op de TCP-segmenten die worden verzonden, maar de waarschuwing kan worden geactiveerd voor veel andere metrische gegevens. In dit voorbeeld wordt is een waarschuwing geconfigureerd voor het aanroepen van een webhook voor het aanroepen van de functie.

### <a name="create-the-alert-rule"></a>De waarschuwingsregel maken

Ga naar een bestaande virtuele machine en vervolgens een waarschuwingsregel toevoegen. Meer gedetailleerde informatie over het configureren van waarschuwingen kan worden gevonden op [waarschuwingen maken in Azure Monitor voor Azure-services - Azure-portal](../monitoring-and-diagnostics/insights-alerts-portal.md). Voer de volgende waarden in de **waarschuwingsregel** blade, en selecteer vervolgens **OK**.

  |**Instelling** | **Waarde** | **Details** |
  |---|---|---|
  |**Naam**|TCP_Segments_Sent_Exceeded|De naam van de waarschuwingsregel.|
  |**Beschrijving**|TCP-segmenten verzonden heeft de drempel|De beschrijving voor de waarschuwingsregel.|
  |**Gegevens**|TCP-segmenten verzonden| De metrische gegevens te gebruiken om de waarschuwing te activeren. |
  |**voorwaarde**|Groter dan| De voorwaarde om te gebruiken bij het evalueren van de metrische gegevens.|
  |**Drempelwaarde**|100| De waarde van de metrische gegevens die de waarschuwing wordt geactiveerd. Deze waarde moet worden ingesteld op een geldige waarde voor uw omgeving.|
  |**Period**|In de afgelopen vijf minuten| Bepaalt de periode waarin om te zoeken naar de drempelwaarde voor de metrische gegevens.|
  |**Webhook**|[webhook-URL van de functie-app]| De webhook-URL van de functie-app die in de vorige stappen is gemaakt.|

> [!NOTE]
> De TCP-segmenten metrische gegevens is niet standaard ingeschakeld. Meer informatie over het inschakelen van aanvullende metrische gegevens door naar de pagina [controle en diagnose inschakelen](../monitoring-and-diagnostics/insights-how-to-use-diagnostics.md).

## <a name="review-the-results"></a>De resultaten bekijken

Nadat de criteria voor de waarschuwing triggers, is een pakketopname gemaakt. Ga naar Network Watcher, en selecteer vervolgens **pakketopname**. Op deze pagina kunt u de koppeling packet capture-bestand voor het downloaden van de pakketopname.

![Pakketopname weergeven][functions14]

Als het bestand vastleggen lokaal is opgeslagen, kunt u deze ophalen door het aanmelden bij de virtuele machine.

Zie voor instructies over het downloaden van bestanden vanuit Azure storage-accounts [aan de slag met Azure Blob storage met .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md). Een ander hulpprogramma, kunt u is [Opslagverkenner](https://storageexplorer.com/).

Nadat uw vastleggen is gedownload, kunt u deze weergeven met behulp van een hulpmiddel dat vindt u een **.cap** bestand. Hieronder vindt u koppelingen naar twee van deze hulpprogramma's:

- [Microsoft Message Analyzer](https://technet.microsoft.com/library/jj649776.aspx)
- [WireShark](https://www.wireshark.org/)

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het weergeven van uw pakketopnamen recentst [pakket netwerkopname-analyse met Wireshark](network-watcher-deep-packet-inspection.md).


[1]: ./media/network-watcher-alert-triggered-packet-capture/figure1.png
[1-1]: ./media/network-watcher-alert-triggered-packet-capture/figure1-1.png
[2]: ./media/network-watcher-alert-triggered-packet-capture/figure2.png
[3]: ./media/network-watcher-alert-triggered-packet-capture/figure3.png
[functions1]:./media/network-watcher-alert-triggered-packet-capture/functions1.png
[functions2]:./media/network-watcher-alert-triggered-packet-capture/functions2.png
[functions3]:./media/network-watcher-alert-triggered-packet-capture/functions3.png
[functions4]:./media/network-watcher-alert-triggered-packet-capture/functions4.png
[functions5]:./media/network-watcher-alert-triggered-packet-capture/functions5.png
[functions6]:./media/network-watcher-alert-triggered-packet-capture/functions6.png
[functions7]:./media/network-watcher-alert-triggered-packet-capture/functions7.png
[functions8]:./media/network-watcher-alert-triggered-packet-capture/functions8.png
[functions9]:./media/network-watcher-alert-triggered-packet-capture/functions9.png
[functions10]:./media/network-watcher-alert-triggered-packet-capture/functions10.png
[functions11]:./media/network-watcher-alert-triggered-packet-capture/functions11.png
[functions12]:./media/network-watcher-alert-triggered-packet-capture/functions12.png
[functions13]:./media/network-watcher-alert-triggered-packet-capture/functions13.png
[functions14]:./media/network-watcher-alert-triggered-packet-capture/functions14.png
[scenario]:./media/network-watcher-alert-triggered-packet-capture/scenario.png

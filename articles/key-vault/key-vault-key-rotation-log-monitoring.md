---
title: Azure Key Vault instellen met end-to-end sleutelrotatie en controle | Microsoft Docs
description: Met deze gebruiksaanwijzing kunt u rouleren van de sleutel instellen en controleren van de logboeken van key vault.
services: key-vault
documentationcenter: ''
author: barclayn
manager: barbkess
tags: ''
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 01/07/2019
ms.author: barclayn
ms.openlocfilehash: 68fd33dc3e9def11f72b7aec14f83f86b8bb74d0
ms.sourcegitcommit: e88188bc015525d5bead239ed562067d3fae9822
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/24/2019
ms.locfileid: "56749701"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>Azure Key Vault instellen met sleutelrotatie en controle

## <a name="introduction"></a>Inleiding

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

Nadat u een key vault hebt, kunt u gaan gebruiken voor het opslaan van sleutels en geheimen. Uw toepassingen niet langer nodig hebt om vast te leggen van de sleutels of geheimen, maar u kunt deze aanvragen op de kluis naar behoefte. Een key vault kunt u sleutels en geheimen bijwerken zonder het gedrag van uw toepassing, u een breed scala aan mogelijkheden voor uw sleutel en geheime beheer opent.

>[!IMPORTANT]
> De voorbeelden in dit artikel vindt u alleen ter illustratie. Ze zijn niet bedoeld voor gebruik in productieomgevingen. 

Dit artikel helpt bij:

- Een voorbeeld van het gebruik van Azure Key Vault voor het opslaan van een geheim. Het geheim opgeslagen is in dit artikel wordt de Azure storage-accountsleutel die toegankelijk is voor een toepassing. 
- Het implementeren van een geplande rotatie van die sleutel van het opslagaccount.
- Het bewaken van de sleutel kluis de logboeken voor controle en waarschuwingen genereren wanneer onverwachte aanvragen worden gedaan.

> [!NOTE]
> In dit artikel niet de eerste installatie van uw key vault in detail uitgelegd. Voor deze informatie, Zie [wat is Azure Key Vault?](key-vault-overview.md). Zie voor instructies voor platformoverschrijdende opdrachtregelinterface [Key Vault beheren met behulp van de Azure CLI](key-vault-manage-with-cli2.md).

## <a name="set-up-key-vault"></a>Key Vault instellen

Om in te schakelen van een toepassing op te halen van een geheim uit Key Vault, moet u eerst het geheim maken en uploaden naar uw kluis.

Start een Azure PowerShell-sessie en gebruik de volgende opdracht om u aan te melden bij uw Azure-account:

```powershell
Connect-AzAccount
```

Voer de gebruikersnaam en het wachtwoord voor uw Azure-account in het pop-browservenster. PowerShell haalt alle abonnementen die gekoppeld aan dit account zijn. PowerShell maakt gebruik van de eerste regel standaard.

Als u meerdere abonnementen hebt, hebt u mogelijk om op te geven die is gebruikt voor het maken van uw key vault. Voer de volgende als u wilt zien van de abonnementen voor uw account:

```powershell
Get-AzSubscription
```

Geef het abonnement dat is gekoppeld aan de sleutelkluis die u zult worden logboekregistratie op te voeren:

```powershell
Set-AzContext -SubscriptionId <subscriptionID>
```

Omdat in dit artikel ziet u het opslaan van een opslagaccountsleutel als een geheim, moet u die sleutel van het opslagaccount ophalen.

```powershell
Get-AzStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Bij het ophalen van het geheim (in dit geval uw opslagaccountsleutel), moet u deze sleutel converteren naar een beveiligde tekenreeks en vervolgens een geheim maken met de waarde in uw key vault.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

Vervolgens krijgt u de URI voor de geheime sleutel die u hebt gemaakt. U moet deze URI in een latere stap voor het aanroepen van de key vault en het geheim ophalen. Voer de volgende PowerShell-opdracht en noteer de ID-waarde van het geheim URI is:

```powershell
Get-AzKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>De toepassing instellen

Nu dat u een geheim opgeslagen hebt, kunt u code ophalen en deze gebruiken na het uitvoeren van enkele extra stappen.

U moet uw toepassing eerst registreren bij Azure Active Directory. Vertel Key Vault de toepassingsgegevens van uw zodat het aanvragen van uw toepassing.

> [!NOTE]
> Uw toepassing moet worden gemaakt op dezelfde Azure Active Directory-tenant als uw key vault.

1. Open **Azure Active Directory**.
2. Selecteer **App-registraties**. 
3. Selecteer **nieuwe toepassing registreren** aan een toepassing toevoegen aan Azure Active Directory.

    ![Geopende toepassingen in Azure Active Directory](./media/keyvault-keyrotation/azure-ad-application.png)

4. Onder **maken**, laat u het toepassingstype als **Web-app / API** en geef een naam op voor uw toepassing. Geef uw toepassing een **aanmeldings-URL**. Deze URL is alles wat die u wilt gebruiken voor deze demo.

    ![Toepassingsregistratie maken](./media/keyvault-keyrotation/create-app.png)

5. Nadat de toepassing is toegevoegd aan Azure Active Directory, wordt de toepassingspagina wordt geopend. Selecteer **instellingen**, en selecteer vervolgens **eigenschappen**. Kopieer de **toepassings-ID** waarde. U hebt deze nodig in latere stappen.

Genereer een sleutel voor uw toepassing vervolgens, zodat deze met Azure Active Directory communiceren kan. Voor het maken van een sleutel selecteert **sleutels** onder **instellingen**. Noteer de zojuist gegenereerde sleutel voor uw Azure Active Directory-toepassing. U hebt deze in een latere stap nodig. De sleutel zijn niet beschikbaar nadat u deze sectie hebt verlaten. 

![Azure Active Directory-app-sleutels](./media/keyvault-keyrotation/create-key.png)

Voordat u de aanroepen vanuit uw toepassing in de key vault maken, moet u de key vault Vertel over uw toepassing en de bijbehorende machtigingen. De volgende opdracht maakt gebruik van de naam van de kluis en de toepassings-ID van uw app in Azure Active Directory voor het verlenen van de toepassing **ophalen** toegang tot uw key vault.

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

U bent nu klaar om te beginnen het bouwen van uw toepassing aanroepen. In uw toepassing, moet u de NuGet-pakketten die nodig zijn om te communiceren met Azure Key Vault en Azure Active Directory installeren. Voer de volgende opdrachten vanuit de Visual Studio Package Manager-console. Bij het schrijven van dit artikel, de huidige versie van de Azure Active Directory-clientpakket is 3.10.305231913, dus controleer of de meest recente versie en bijwerken, indien nodig.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

In de code van uw toepassing, maakt u een klasse voor het opslaan van de methode voor uw Azure Active Directory-verificatie. In dit voorbeeld deze klasse wordt aangeroepen **Utils**. Voeg de volgende `using` instructie:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Voeg vervolgens de volgende methode om op te halen van de JWT-token uit Azure Active Directory. Voor onderhoud, is het raadzaam om te verplaatsen van de vastgelegde tekenreekswaarden in de configuratie van uw web- of toepassing.

```csharp
public async static Task<string> GetToken(string authority, string resource, string scope)
{
    var authContext = new AuthenticationContext(authority);

    ClientCredential clientCred = new ClientCredential("<AzureADApplicationClientID>","<AzureADApplicationClientKey>");

    AuthenticationResult result = await authContext.AcquireTokenAsync(resource, clientCred);

    if (result == null)

    throw new InvalidOperationException("Failed to obtain the JWT token");

    return result.AccessToken;
}
```

Voeg de code die nodig zijn voor het aanroepen van Key Vault en de geheime waarde op te halen. Eerst moet u het volgende toevoegen `using` instructie:

```csharp
using Microsoft.Azure.KeyVault;
```

Voeg de methodeaanroepen voor het aanroepen van Key Vault en het geheim ophalen. In deze methode bieden u de geheime URI die u hebt opgeslagen in de vorige stap. Let op het gebruik van de **GetToken** methode van de **Utils** klasse die u eerder hebt gemaakt.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Wanneer u uw toepassing uitvoert, moet u nu worden geverifieerd bij Azure Active Directory en vervolgens het ophalen van de geheime waarde van Azure Key Vault.

## <a name="key-rotation-using-azure-automation"></a>Sleutelroulatie met behulp van Azure Automation

U bent nu klaar voor het instellen van een strategie rotatie van de waarden die u als Key Vault-geheimen opslaat. Geheimen kunnen op verschillende manieren worden gedraaid:

- Als onderdeel van een handmatig proces
- Programmatisch met behulp van API-aanroepen
- Via een Azure Automation-script

Voor de doeleinden van dit artikel gebruikt u PowerShell in combinatie met Azure Automation om een Azure storage-account toegangssleutel te wijzigen. U gaat vervolgens een key vault-geheim bijwerken met deze nieuwe sleutel.

Als u wilt toestaan dat Azure Automation geheime waarden instellen in uw key vault, moet u de client-ID ophalen voor de verbinding met de naam **AzureRunAsConnection**. Deze verbinding is gemaakt toen u uw Azure Automation-exemplaar tot stand gebracht. Als u wilt deze ID vinden, selecteer **activa** van uw Azure Automation-instantie. Selecteer **verbindingen**, en selecteer vervolgens de **AzureRunAsConnection** service-principal. Noteer de **ApplicationId** waarde.

![Azure Automation client ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

In **activa**, selecteer **Modules**. Selecteer **galerie**, en zoek vervolgens naar en bijgewerkte versies van elk van de volgende modules importeren:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage

> [!NOTE]
> Bij het schrijven van dit artikel, wordt alleen de hierboven vermelde modules nodig om te worden bijgewerkt in verband met het volgende script. Als uw automation-taak is mislukt, controleert u of u alle modules weer die nodig zijn en de bijbehorende afhankelijkheden hebt geïmporteerd.

Nadat u hebt de toepassings-ID opgehaald voor de verbinding van uw Azure Automation, moet u uw key vault vertellen dat deze toepassing is gemachtigd om bij te werken van geheimen in uw kluis aan te geven. Gebruik de volgende PowerShell-opdracht:

```powershell
Set-AzKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Selecteer vervolgens **Runbooks** onder uw Azure Automation-instantie en selecteer vervolgens **Runbook toevoegen**. Selecteer **Snelle invoer**. Naam van uw runbook en selecteer **PowerShell** als het runbooktype. U kunt een beschrijving toevoegen. Selecteer ten slotte **maken**.

![Runbook maken](./media/keyvault-keyrotation/Create_Runbook.png)

Plak het volgende PowerShell-script in het editorvenster voor uw nieuwe runbook:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection"
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Connect-AzAccount `
        -ServicePrincipal `
        -TenantId $servicePrincipalConnection.TenantId `
        -ApplicationId $servicePrincipalConnection.ApplicationId `
        -CertificateThumbprint $servicePrincipalConnection.CertificateThumbprint
    "Login complete."
}
catch {
    if (!$servicePrincipalConnection)
    {
        $ErrorMessage = "Connection $connectionName not found."
        throw $ErrorMessage
    } else{
        Write-Error -Message $_.Exception
        throw $_.Exception
    }
}

# Optionally you can set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Selecteer in het deelvenster van editor **testvenster** voor het testen van uw script. Nadat het script wordt uitgevoerd zonder fouten, selecteert u **publiceren**, en vervolgens kunt u een schema voor het runbook in het deelvenster runbook configuratie toepassen.

## <a name="key-vault-auditing-pipeline"></a>Key Vault controle pijplijn

Bij het instellen van een key vault, kunt u de controle voor het verzamelen van Logboeken op toegangsaanvragen tot de key vault. Deze logboeken worden opgeslagen in een opgegeven Azure storage-account en kunnen worden opgehaald, bewaakt en geanalyseerd. De volgende scenario maakt gebruik van Azure functions, Azure logic apps en logboeken voor controle van de sleutelkluis om een pijplijn waarmee een e-mailbericht wordt verzonden wanneer een app die niet overeenkomt met de app-ID van de web-app geheimen opgehaald uit de kluis te maken.

Eerst moet u zich aanmelden op uw key vault inschakelen. Gebruik de volgende PowerShell-opdrachten. (Ziet u de volledige details in [in dit artikel over de logboekregistratie van key vault](key-vault-logging.md).)

```powershell
$sa = New-AzStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzKeyVault -VaultName '<vaultName>'
Set-AzDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Category AuditEvent
```

Nadat de logboekregistratie is ingeschakeld, start u auditlogboeken worden opgeslagen in het opgegeven opslagaccount. Deze logboeken bevatten gebeurtenissen over hoe en wanneer uw sleutelkluizen toegankelijk zijn, en door wie.

> [!NOTE]
> U kunt toegang tot uw logboekgegevens 10 minuten na de sleutelkluis-bewerking. Het wordt vaak zijn beschikbaar sneller.

De volgende stap is het [maken van een Azure Service Bus-wachtrij](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Deze wachtrij is waar de logboeken voor sleutelkluis controle worden gepusht. Als de berichten logboek in de wachtrij, wordt de logische app worden ze opgepikt en hierop fungeert. Een Service Bus-exemplaar maken met de volgende stappen uit:

1. Een Service Bus-naamruimte maken (als u al die u wilt gebruiken hebt, gaat u verder met stap 2).
2. Blader naar het Service Bus-exemplaar in de Azure-portal en selecteer de naamruimte die u wilt maken van de wachtrij in.
3. Selecteer **een resource maken** > **bedrijfsintegratie** > **Service Bus**, en voer vervolgens de vereiste gegevens.
4. De Service Bus-verbindingsinformatie vinden door te selecteren van de naamruimte **verbindingsgegevens**. U hebt deze informatie voor de volgende sectie.

Volgende [maken van een Azure-functie](../azure-functions/functions-create-first-azure-function.md) pollen van de logboeken van key vault binnen het opslagaccount en nieuwe gebeurtenissen te kiezen. Deze functie wordt geactiveerd volgens een schema.

Voor het maken van een Azure-functie-app selecteert **een resource maken**, zoek in marketplace naar **functie-App**, en selecteer vervolgens **maken**. U kunt tijdens het maken van een bestaande hostingabonnement gebruiken of een nieuwe maken. U kunt ook kiezen voor dynamisch hosten. Zie voor meer informatie over de hostingopties voor Azure Functions, [Azure Functions schalen](../azure-functions/functions-scale.md).

Nadat de Azure-functie-app is gemaakt, gaat u naar deze en selecteer de **Timer** scenario en **C\#**  voor de taal. Selecteer vervolgens **maken van deze functie**.

![Azure Functions Start-blade](./media/keyvault-keyrotation/Azure_Functions_Start.png)

Op de **ontwikkelen** tabblad, vervang de code run.csx door het volgende:

```csharp
#r "Newtonsoft.Json"

using System;
using Microsoft.WindowsAzure.Storage;
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;
using Microsoft.ServiceBus.Messaging;
using System.Text;

public static void Run(TimerInfo myTimer, TextReader inputBlob, TextWriter outputBlob, TraceWriter log)
{
    log.Info("Starting");

    CloudStorageAccount sourceStorageAccount = new CloudStorageAccount(new StorageCredentials("<STORAGE_ACCOUNT_NAME>", "<STORAGE_ACCOUNT_KEY>"), true);

    CloudBlobClient sourceCloudBlobClient = sourceStorageAccount.CreateCloudBlobClient();

    var connectionString = "<SERVICE_BUS_CONNECTION_STRING>";
    var queueName = "<SERVICE_BUS_QUEUE_NAME>";

    var sbClient = QueueClient.CreateFromConnectionString(connectionString, queueName);

    DateTime dtPrev = DateTime.UtcNow;
    if(inputBlob != null)
    {
        var txt = inputBlob.ReadToEnd();

        if(!string.IsNullOrEmpty(txt))
        {
            dtPrev = DateTime.Parse(txt);
            log.Verbose($"SyncPoint: {dtPrev.ToString("O")}");
        }
        else
        {
            dtPrev = DateTime.UtcNow;
            log.Verbose($"Sync point file didnt have a date. Setting to now.");
        }
    }

    var now = DateTime.UtcNow;

    string blobPrefix = "insights-logs-auditevent/resourceId=/SUBSCRIPTIONS/<SUBSCRIPTION_ID>/RESOURCEGROUPS/<RESOURCE_GROUP_NAME>/PROVIDERS/MICROSOFT.KEYVAULT/VAULTS/<KEY_VAULT_NAME>/y=" + now.Year +"/m="+now.Month.ToString("D2")+"/d="+ (now.Day).ToString("D2")+"/h="+(now.Hour).ToString("D2")+"/m=00/";

    log.Info($"Scanning:  {blobPrefix}");

    IEnumerable<IListBlobItem> blobs = sourceCloudBlobClient.ListBlobs(blobPrefix, true);

    log.Info($"found {blobs.Count()} blobs");

    foreach(var item in blobs)
    {
        if (item is CloudBlockBlob)
        {
            CloudBlockBlob blockBlob = (CloudBlockBlob)item;

            log.Info($"Syncing: {item.Uri}");

            string sharedAccessUri = GetContainerSasUri(blockBlob);

            CloudBlockBlob sourceBlob = new CloudBlockBlob(new Uri(sharedAccessUri));

            string text;
            using (var memoryStream = new MemoryStream())
            {
                sourceBlob.DownloadToStream(memoryStream);
                text = System.Text.Encoding.UTF8.GetString(memoryStream.ToArray());
            }

            dynamic dynJson = JsonConvert.DeserializeObject(text);

            //Required to order by time as they might not be in the file
            var results = ((IEnumerable<dynamic>) dynJson.records).OrderBy(p => p.time);

            foreach (var jsonItem in results)
            {
                DateTime dt = Convert.ToDateTime(jsonItem.time);

                if(dt>dtPrev){
                    log.Info($"{jsonItem.ToString()}");

                    var payloadStream = new MemoryStream(Encoding.UTF8.GetBytes(jsonItem.ToString()));
                    //When sending to ServiceBus, use the payloadStream and set keeporiginal to true
                    var message = new BrokeredMessage(payloadStream, true);
                    sbClient.Send(message);
                    dtPrev = dt;
                }
            }
        }
    }
    outputBlob.Write(dtPrev.ToString("o"));
}

static string GetContainerSasUri(CloudBlockBlob blob)
{
    SharedAccessBlobPolicy sasConstraints = new SharedAccessBlobPolicy();

    sasConstraints.SharedAccessStartTime = DateTime.UtcNow.AddMinutes(-5);
    sasConstraints.SharedAccessExpiryTime = DateTime.UtcNow.AddHours(24);
    sasConstraints.Permissions = SharedAccessBlobPermissions.Read;

    //Generate the shared access signature on the container, setting the constraints directly on the signature.
    string sasBlobToken = blob.GetSharedAccessSignature(sasConstraints);

    //Return the URI string for the container, including the SAS token.
    return blob.Uri + sasBlobToken;
}
```

> [!NOTE]
> Wijzig de variabelen in de bovenstaande code om te verwijzen naar uw opslagaccount waar de sleutelkluis-logboeken worden geschreven, naar het Service Bus-exemplaar dat u eerder hebt gemaakt en naar het specifieke pad naar de van de sleutelkluis opslaglogboeken.

De functie neemt de meest recente logboekbestand uit het opslagaccount waar de sleutelkluis-logboeken worden geschreven, worden de meest recente gebeurtenissen uit het bestand en ze naar een Service Bus-wachtrij worden gepusht. 

Omdat een enkel bestand meerdere gebeurtenissen hebben kan, moet u een sync.txt-bestand dat de functie ook gekeken naar om te bepalen van de tijdstempel van de laatste gebeurtenis die is opgehaald maken. Dit bestand gebruikt, zorgt u ervoor dat u geen dezelfde gebeurtenis meerdere keren pushen. 

Het bestand sync.txt bevat een tijdstempel voor de gebeurtenis laatste aangetroffen. Wanneer de logboeken worden geladen, moeten ze worden gesorteerd op basis van hun tijdstempels om ervoor te zorgen dat ze correct worden geordend.

Voor deze functie, verwijzen we naar een aantal aanvullende bibliotheken die niet beschikbaar zijn gebruiksklaar in Azure Functions. Als u wilt opnemen in deze bibliotheken, moet u Azure Functions om op te halen ze met behulp van NuGet. Onder de **Code** Schakel **bestanden weergeven**.

![Optie "Bestanden weergeven"](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

Voeg een bestand met de naam van project.json met de volgende inhoud:

```json
    {
      "frameworks": {
        "net46":{
          "dependencies": {
                "WindowsAzure.Storage": "7.0.0",
                "WindowsAzure.ServiceBus":"3.2.2"
          }
        }
       }
    }
```

Nadat u hebt geselecteerd **opslaan**, Azure Functions de vereiste binaire bestanden worden gedownload.

Schakel over naar de **integreren** tabblad en geef de parameter timer een beschrijvende naam op om te gebruiken binnen de functie. In de bovenstaande code wordt de functie wordt verwacht dat de timer te worden opgeroepen *myTimer*. Geef een [CRON-expressie](../app-service/webjobs-create.md#CreateScheduledCRON) voor de timer als volgt te werk: `0 * * * * *`. Deze expressie zorgt ervoor dat de functie één keer een minuut wordt uitgevoerd.

Op dezelfde **integreren** tabblad, voegt u een invoer van het type **Azure Blob-opslag**. Deze invoer verwijst naar het sync.txt-bestand met de tijdstempel van de laatste gebeurtenis van de functie bekeken. Deze invoer wordt gebruikt binnen de functie door met de parameternaam. In de vorige code de Azure Blob storage-invoer wordt verwacht dat de parameternaam moet *inputBlob*. Selecteer het opslagaccount waar het bestand sync.txt zich bevindt (het kan zijn hetzelfde of een ander opslagaccount). Geef het pad naar het bestand in de indeling in het padveld `{container-name}/path/to/sync.txt`.

Toevoegen van een uitvoer van het type **Azure Blob-opslag**. Deze uitvoer wordt verwijzen naar het sync.txt-bestand dat u hebt gedefinieerd in de invoer. Deze uitvoer wordt gebruikt door de functie om de tijdstempel van de laatste gebeurtenis bekeken schrijven. De bovenstaande code wordt verwacht dat deze parameter moet worden aangeroepen *outputBlob*.

De functie is nu gereed. Zorg ervoor dat u wilt terugkeren naar de **ontwikkelen** tabblad en sla de code. Het uitvoervenster voor elke compilatiefouten Controleer en corrigeer deze indien nodig. Als de code wordt gecompileerd, klikt u vervolgens de code moet nu worden elke minuut van de logboeken van key vault controleren en pushen van alle nieuwe gebeurtenissen in de opgegeven Service Bus-wachtrij. U ziet de logboekinformatie voor schrijven naar het logboekvenster telkens wanneer de functie wordt geactiveerd.

### <a name="azure-logic-app"></a>Logische app van Azure

Vervolgens moet u een logische app van Azure die de gebeurtenissen neemt die de functie is pushen naar de Service Bus-wachtrij, de inhoud parseert en verzendt een e-mailbericht op basis van een voorwaarde die is gekoppeld.

[Maak een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) hiervoor **een resource maken** > **integratie** > **logische App**.

Nadat de logische app is gemaakt, gaat u naar deze en selecteer **bewerken**. Selecteer in de logische app-editor **Service Bus-wachtrij** en voer uw Service Bus-referenties om te verbinden met de wachtrij.

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Selecteer **een voorwaarde toevoegen**. In de voorwaarde, Ga naar de geavanceerde editor en voer de volgende code. Vervang *APP_ID* met de werkelijke app-ID van uw web-app:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Deze expressie retourneert in feite **false** als de *appid* van de inkomende gebeurtenis (dit is de hoofdtekst van de Service Bus-bericht) is niet de *appid* van de app.

Maak nu een actie onder **indien Nee, niets doen**.

![Actie voor Azure Logic Apps kiezen](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Selecteer voor de actie **Office 365 - e-mail verzenden**. Vul de velden te maken van een e-mailbericht verzenden wanneer de gedefinieerde voorwaarde retourneert **false**. Als u geen Office 365, zoek alternatieven om de dezelfde resultaten te behalen.

U hebt nu een end-to-end-pijplijn waarmee nieuwe sleutelkluis auditlogboeken zoekt naar één keer een minuut. Deze verstuurd nieuwe logboeken gevonden naar een Service Bus-wachtrij. De logische app wordt geactiveerd wanneer een nieuw bericht in de wachtrij terechtkomt. Als de *appid* binnen de gebeurtenis komt niet overeen met de app-ID van de aanroepende toepassing, een e-mailbericht stuurt de stroom.

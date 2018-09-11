---
title: Azure Key Vault instellen met end-to-end sleutelrotatie en controle | Microsoft Docs
description: Met deze instructies kunt u instellen met sleutelrotatie en controle logboeken van key vault.
services: key-vault
documentationcenter: ''
author: barclayn
manager: mbaldwin
tags: ''
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/12/2018
ms.author: barclayn
ms.openlocfilehash: bf3aba431e7b417b2213bc3410fd7722d7888d15
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44302014"
---
# <a name="set-up-azure-key-vault-with-key-rotation-and-auditing"></a>Azure Key Vault instellen met sleutelrotatie en controle

## <a name="introduction"></a>Inleiding

Nadat u een key vault hebt, kunt u gaan gebruiken voor het opslaan van sleutels en geheimen. Uw toepassingen niet langer nodig hebt om vast te leggen van de sleutels of geheimen, maar u kunt deze aanvragen op de kluis naar behoefte. Hiermee kunt u sleutels en geheimen bijwerken zonder het gedrag van uw toepassing, u een breed scala aan mogelijkheden om uw sleutel en geheime beheer opent.

>[!IMPORTANT]
> De voorbeelden in dit artikel vindt u alleen ter illustratie. Ze zijn niet bedoeld voor gebruik in productieomgevingen. 

Dit artikel helpt bij:

- Een voorbeeld van het gebruik van Azure Key Vault voor het opslaan van een geheim. In deze zelfstudie is het geheim opgeslagen de Azure Storage-Account-sleutel geopend door een toepassing. 
- U ziet ook implementatie van een geplande rotatie van die sleutel van het opslagaccount.
- Hierin wordt beschreven hoe u controleert de sleutelkluis-logboeken voor controle en waarschuwingen genereren wanneer onverwachte aanvragen worden gedaan.

> [!NOTE]
> In deze zelfstudie is niet bedoeld voor de eerste installatie van uw key vault in detail uitgelegd. Zie [Aan de slag met Azure Key Vault](key-vault-get-started.md) voor meer informatie. Zie voor instructies voor platformoverschrijdende opdrachtregelinterface [Key Vault beheren met behulp van CLI](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>Key Vault instellen

Om in te schakelen van een toepassing op te halen van een geheim uit Key Vault, moet u eerst het geheim maken en uploaden naar uw kluis. Dit kan worden bereikt door een Azure PowerShell-sessie starten en meldt u zich bij uw Azure-account met de volgende opdracht:

```powershell
Connect-AzureRmAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. PowerShell haalt alle abonnementen die gekoppeld aan dit account zijn. PowerShell maakt gebruik van de eerste regel standaard.

Als u meerdere abonnementen hebt, hebt u mogelijk om op te geven die is gebruikt voor het maken van uw key vault. Voer de volgende als u wilt zien van de abonnementen voor uw account:

```powershell
Get-AzureRmSubscription
```

Geef het abonnement dat is gekoppeld aan de sleutelkluis die u logboekregistratie wilt inschakelen op te voeren:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Omdat in dit artikel ziet u het opslaan van een opslagaccountsleutel als een geheim, moet u die sleutel van het opslagaccount ophalen.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Bij het ophalen van het geheim (in dit geval uw opslagaccountsleutel), moet u die converteren naar een beveiligde tekenreeks en vervolgens een geheim maken met de waarde in uw key vault.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```

Vervolgens krijgt u de URI voor de geheime sleutel die u hebt gemaakt. Dit wordt gebruikt in een latere stap bij het aanroepen van de key vault om op te halen van het geheim. Voer de volgende PowerShell-opdracht en noteer de ID-waarde de geheime URI is:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>De toepassing instellen

Nu dat u een geheim opgeslagen hebt, kunt u code ophalen en deze gebruiken. Er zijn een paar stappen die nodig zijn om dit te bereiken. De eerste en belangrijkste stap is het registreren van uw toepassing met Azure Active Directory en vervolgens vertellen Key Vault de toepassingsgegevens van uw zodat het aanvragen van uw toepassing.

> [!NOTE]
> Uw toepassing moet worden gemaakt op dezelfde Azure Active Directory-tenant als uw key vault.
>
>

1. Ga naar Azure Active Directory.
2. Kies **App-registraties** 
3. Kies **nieuwe toepassing registreren** aan een toepassing toevoegen aan uw Azure Active Directory.

    ![Geopende toepassingen in Azure Active Directory](./media/keyvault-keyrotation/azure-ad-application.png)

4. In de **maken** sectie laat het toepassingstype als **WEBTOEPASSING en/of WEB-API** en geef een naam op voor uw toepassing. Geef uw toepassing een **AANMELDINGS-URL**. Dit is alles wat die u wilt gebruiken voor deze demo.

    ![Toepassingsregistratie maken](./media/keyvault-keyrotation/create-app.png)

5. Nadat de toepassing is toegevoegd aan Azure Active Directory, wordt u in de toepassingspagina worden gebracht. Selecteer **instellingen** en selecteer Eigenschappen. Kopieer de **toepassings-ID** waarde. In latere stappen wel vereist.

Genereer een sleutel voor uw toepassing vervolgens, zodat deze met uw Azure Active Directory communiceren kan. U kunt een sleutel onder maken door te navigeren naar de **sleutels** sectie onder **instellingen**. Let op het zojuist gegenereerde sleutel van uw Azure Active Directory-toepassing voor gebruik in een latere stap. U ziet dat de sleutel niet meer beschikbaar nadat u buiten deze sectie navigeren. 

![Sleutels voor Azure Active Directory-App](./media/keyvault-keyrotation/create-key.png)

Voordat er een aanroepen vanuit uw toepassing in de key vault, moet u de key vault Vertel over uw toepassing en de bijbehorende machtigingen. De volgende opdracht wordt de naam van de kluis en de toepassings-ID van uw Azure Active Directory-app en verleent **ophalen** toegang tot uw key vault voor de toepassing.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Op dit moment bent u klaar om te beginnen het bouwen van uw toepassing aanroepen. In uw toepassing, moet u de NuGet-pakketten zijn vereist om te communiceren met Azure Key Vault en Azure Active Directory installeren. Voer de volgende opdrachten vanuit de Visual Studio Package Manager-console. Bij het schrijven van dit artikel is de huidige versie van de Azure Active Directory-clientpakket 3.10.305231913, dus mogelijk om te bevestigen van de meest recente versie en dienovereenkomstig bijwerken.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

In de code van uw toepassing, maakt u een klasse voor het opslaan van de methode voor uw Azure Active Directory-verificatie. In dit voorbeeld deze klasse wordt aangeroepen **Utils**. Voeg het volgende toe met de instructie:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Voeg vervolgens de volgende methode om op te halen van de JWT-token uit Azure Active Directory. Voor onderhoud, kunt u de vastgelegde tekenreekswaarden verplaatsen naar de configuratie van uw web- of toepassing.

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

Voeg de code die nodig zijn voor het aanroepen van Key Vault en de geheime waarde op te halen. Eerst moet u het volgende toevoegen met de instructie:

```csharp
using Microsoft.Azure.KeyVault;
```

Voeg de methodeaanroepen voor het aanroepen van Key Vault en het geheim ophalen. In deze methode bieden u de geheime URI die u hebt opgeslagen in de vorige stap. Let op het gebruik van de **GetToken** methode van de **Utils** klasse eerder hebt gemaakt.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Wanneer u uw toepassing uitvoert, moet u nu worden geverifieerd bij Azure Active Directory en vervolgens het ophalen van de geheime waarde van Azure Key Vault.

## <a name="key-rotation-using-azure-automation"></a>Sleutelroulatie met behulp van Azure Automation

Er zijn verschillende opties voor het implementeren van een strategie rotatie voor waarden die u als Azure Key Vault-geheimen opslaan. Geheimen kunnen worden gedraaid als onderdeel van een handmatig proces, kan via een programma met behulp van API-aanroepen worden gedraaid of ze via een automatiseringsscript kunnen worden gedraaid. Voor de doeleinden van dit artikel wordt u Azure PowerShell in combinatie met Azure Automation gebruiken om een Azure Storage-Account toegang-sleutel te wijzigen. U wordt vervolgens een key vault-geheim bijwerken met deze nieuwe sleutel.

Als u wilt toestaan dat Azure Automation geheime waarden instellen in uw key vault, moet u de client-ID ophalen voor de verbinding met de naam AzureRunAsConnection, die is gemaakt wanneer u uw Azure Automation-exemplaar tot stand gebracht. U kunt deze ID vinden door te kiezen **activa** van uw Azure Automation-instantie. Van daaruit, kiest u **verbindingen** en selecteer vervolgens de **AzureRunAsConnection** service-principal. Noteer de **toepassings-ID**.

![Azure Automation-client-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

In **activa**, kiest u **Modules**. Van **Modules**, selecteer **galerie**, en zoek vervolgens en **importeren** bijgewerkte versies van elk van de volgende modules:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> Bij het schrijven van dit artikel, wordt alleen de hierboven vermelde modules nodig om te worden bijgewerkt in verband met het volgende script. Als u merkt dat uw automation-taak is mislukt, moet u bevestigen dat u alle modules weer die nodig zijn en de bijbehorende afhankelijkheden hebt geïmporteerd.
>
>

Nadat u hebt de toepassings-ID opgehaald voor de verbinding van uw Azure Automation, moet u uw key vault vertellen dat deze toepassing toegang heeft tot bijwerken van geheimen in uw kluis aan te geven. Dit kan worden bewerkstelligd met de volgende PowerShell-opdracht:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Selecteer vervolgens **Runbooks** onder uw Azure Automation-instantie en selecteer vervolgens **een Runbook toevoegen**. Selecteer **Snelle invoer**. De naam van uw runbook en selecteer **PowerShell** als het runbooktype. U hebt de mogelijkheid om toe te voegen, een beschrijving. Klik tot slot, **maken**.

![Runbook maken](./media/keyvault-keyrotation/Create_Runbook.png)

Plak het volgende PowerShell-script in het editorvenster voor uw nieuwe runbook:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Connect-AzureRmAccount `
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

#Optionally you may set the following as parameters
$StorageAccountName = <storageAccountName>
$RGName = <storageAccountResourceGroupName>
$VaultName = <keyVaultName>
$SecretName = <keyVaultSecretName>

#Key name. For example key1 or key2 for the storage account
New-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName -KeyName "key2" -Verbose
$SAKeys = Get-AzureRmStorageAccountKey -ResourceGroupName $RGName -Name $StorageAccountName

$secretvalue = ConvertTo-SecureString $SAKeys[1].Value -AsPlainText -Force

$secret = Set-AzureKeyVaultSecret -VaultName $VaultName -Name $SecretName -SecretValue $secretvalue
```

Kies in het deelvenster van editor **testvenster** voor het testen van uw script. Zodra het script wordt uitgevoerd zonder fouten, kunt u selecteren **publiceren**, en vervolgens kunt u een schema voor het runbook terug in het deelvenster voor runbook-configuratie toepassen.

## <a name="key-vault-auditing-pipeline"></a>Key Vault controle pijplijn
Bij het instellen van een key vault, kunt u de controle voor het verzamelen van Logboeken op toegangsaanvragen tot de key vault. Deze logboeken worden opgeslagen in een opgegeven Azure Storage-account en kunnen worden opgehaald, bewaakt en geanalyseerd. De volgende scenario maakt gebruik van Azure functions, Azure logic apps en logboeken van key vault-audit om een pijplijn voor het verzenden van een e-mailbericht wanneer een app die overeenkomt met de app-ID van de web-app geheimen uit de kluis ophaalt te maken.

Eerst moet u zich aanmelden op uw key vault inschakelen. Dit kan worden gedaan via de volgende PowerShell-opdrachten (volledige gegevens kunnen worden weergegeven op [logboekregistratie van key vault](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Nadat dit is ingeschakeld, wordt de auditlogboeken start verzamelen in het opgegeven opslagaccount. Deze logboeken bevatten gebeurtenissen over hoe en wanneer uw sleutelkluizen toegankelijk zijn, en door wie.

> [!NOTE]
> U kunt toegang tot uw logboekgegevens 10 minuten na de sleutelkluis-bewerking. Het wordt doorgaans sneller zijn.
>
>

De volgende stap is het [maken van een Azure Service Bus-wachtrij](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Dit is waar de logboeken van key vault-controle worden gepusht. Wanneer de berichten voor het controlelogboek zich in de wachtrij, wordt de logische app worden ze opgepikt en hierop fungeert. Maak een servicebus met de volgende stappen uit:

1. Een Service Bus-naamruimte maken (als u al die u wilt gebruiken voor deze, gaat u verder met stap 2 hebt).
2. Blader naar de servicebus in de Azure-portal en selecteer de naamruimte die u wilt maken van de wachtrij in.
3. Selecteer **een resource maken**, **bedrijfsintegratie**, **Service Bus**, en voer vervolgens de vereiste gegevens.
4. Selecteer de Service Bus-verbindingsinformatie door te klikken en het kiezen van de naamruimte **verbindingsgegevens**. U moet deze informatie voor de volgende sectie.

Volgende [maken van een Azure-functie](../azure-functions/functions-create-first-azure-function.md) pollen van Logboeken van key vault binnen het opslagaccount en nieuwe gebeurtenissen te kiezen. Dit is een functie die worden geactiveerd volgens een schema.

Voor het maken van een Azure-functie, kies **een resource maken**, zoek in marketplace naar _functie-App_, en klikt u op **maken**. U kunt tijdens het maken van een bestaande hostingabonnement gebruiken of een nieuwe maken. U kunt ook kiezen voor dynamisch hosten. Meer informatie over de functie hostingopties kunnen worden gevonden op [Azure Functions schalen](../azure-functions/functions-scale.md).

Wanneer de Azure-functie is gemaakt, gaat u naar het en kies een timer functie en C\#. Klik vervolgens op **maken van deze functie**.

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

            //required to order by time as they may not be in the file
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
> Zorg ervoor dat u de variabelen in de bovenstaande code om te verwijzen naar uw storage-account waar de sleutelkluis-logboeken worden geschreven, servicebus die u eerder hebt gemaakt en het specifieke pad naar de logboeken van key vault opslag vervangen.
>
>

De functie neemt de meest recente logboekbestand uit het opslagaccount waar de sleutelkluis-logboeken worden geschreven, worden de meest recente gebeurtenissen uit het bestand en ze naar een Service Bus-wachtrij worden gepusht. Aangezien een enkel bestand meerdere gebeurtenissen hebben kan, moet u een sync.txt-bestand dat de functie ook gekeken naar om te bepalen van de tijdstempel van de laatste gebeurtenis die is opgehaald maken. Dit zorgt ervoor dat u geen dezelfde gebeurtenis meerdere keren pushen. Dit bestand sync.txt bevat een tijdstempel voor de laatste gebeurtenis van opgetreden. De logboeken, wanneer het wordt geladen, moeten worden gesorteerd op basis van de tijdstempel om te controleren of dat ze correct zijn gerangschikt.

Voor deze functie, verwijzen we naar een aantal aanvullende bibliotheken die niet beschikbaar zijn gebruiksklaar in Azure Functions. Opgenomen, moeten we Azure Functions om op te halen ze met behulp van NuGet. Kies de **bestanden weergeven** optie.

![Bestanden weergeven](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

En voeg een bestand met de naam van project.json met de volgende inhoud:

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

Bij **opslaan**, Azure Functions de vereiste binaire bestanden worden gedownload.

Schakel over naar de **integreren** tabblad en geef de parameter timer een beschrijvende naam op om te gebruiken binnen de functie. In de bovenstaande code wordt de timer te worden opgeroepen verwacht *myTimer*. Geef een [CRON-expressie](../app-service/web-sites-create-web-jobs.md#CreateScheduledCRON) als volgt: 0 \* \* \* \* \* voor de timer die zorgt ervoor dat de functie één keer een minuut wordt uitgevoerd.

Op dezelfde **integreren** tabblad, voegt u een invoer van het type **Azure Blob Storage**. Dit verwijst naar het sync.txt-bestand met de tijdstempel van de laatste gebeurtenis van de functie bekeken. Dit is beschikbaar in de functie door de naam van de parameter. In de vorige code de Azure Blob Storage-invoer wordt verwacht dat de parameternaam moet *inputBlob*. Kies de opslagaccount waar het bestand sync.txt worden geplaatst (het kan zijn hetzelfde of een ander opslagaccount). In het padveld, geeft u het pad waar het bestand zich in de notatie {container-name}/path/to/sync.txt bevinden.

Toevoegen van een uitvoer van het type *Azure Blob Storage* uitvoer. Dit verwijst naar het sync.txt-bestand dat u hebt gedefinieerd in de invoer. Dit wordt gebruikt door de functie het schrijven van de tijdstempel van de laatste gebeurtenis bekeken. De bovenstaande code wordt verwacht dat deze parameter moet worden aangeroepen *outputBlob*.

De functie is op dit moment klaar. Zorg ervoor dat u wilt terugkeren naar de **ontwikkelen** tabblad en sla de code. Controleer het uitvoervenster weergegeven voor elke compilatiefouten en pas deze dienovereenkomstig aan. Als de code wordt gecompileerd, klikt u vervolgens de code moet nu worden elke minuut van de logboeken van key vault controleren en pushen van alle nieuwe gebeurtenissen naar de opgegeven Service Bus-wachtrij. U ziet de logboekinformatie voor schrijven naar het logboekvenster telkens wanneer de functie wordt geactiveerd.

### <a name="azure-logic-app"></a>Logische app van Azure

Vervolgens maakt u een logische app van Azure die de gebeurtenissen neemt die de functie is pushen naar de Service Bus-wachtrij, de inhoud parseert en verzendt een e-mailbericht op basis van een voorwaarde die is gekoppeld.

[Maak een logische app](../logic-apps/quickstart-create-first-logic-app-workflow.md) door te gaan naar **New > logische App**.

Nadat de logische app is gemaakt, gaat u naar het en kies **bewerken**. Kies in de logische app-editor **Service Bus-wachtrij** en voer uw Service Bus-referenties om te verbinden met de wachtrij.

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Kies vervolgens **een voorwaarde toevoegen**. In de voorwaarde, Ga naar de geavanceerde editor en voer de volgende code, APP_ID vervangen door de werkelijke APP_ID van uw web-app:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Deze expressie retourneert in feite **false** als de *appid* van de inkomende gebeurtenis (dit is de hoofdtekst van de Service Bus-bericht) is niet de *appid* van de app.

Maak nu een actie onder **indien Nee, niets doen**.

![Actie voor Azure Logic App kiezen](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Kies voor de actie **Office 365 - e-mail verzenden**. Vul de velden te maken van een e-mailbericht verzenden wanneer de gedefinieerde voorwaarde retourneert **false**. Als u geen Office 365, kunt u alternatieven voor de dezelfde resultaten bekijken.

Op dit moment hebt u een end-to-end-pijplijn waarmee nieuwe logboeken van key vault-audit zoekt naar één keer een minuut. Deze verstuurd nieuwe logboeken gevonden naar een service bus-wachtrij. De logische app wordt geactiveerd wanneer een nieuw bericht in de wachtrij terechtkomt. Als de *appid* binnen de gebeurtenis komt niet overeen met de app-ID van de aanroepende toepassing, een e-mailbericht stuurt de stroom.

---
title: Instellen van Azure Sleutelkluis met end-to-end sleutel worden gedraaid en controle | Microsoft Docs
description: Met deze procedures kunt u met de sleutel worden gedraaid en bewaking sleutelkluis-logboeken ophalen instellen.
services: key-vault
documentationcenter: 
author: swgriffith
manager: mbaldwin
tags: 
ms.assetid: 9cd7e15e-23b8-41c0-a10a-06e6207ed157
ms.service: key-vault
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/07/2017
ms.author: jodehavi;stgriffi
ms.openlocfilehash: 2de788fabcae501d1a388bcea6b7759c9ea269cc
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="set-up-azure-key-vault-with-end-to-end-key-rotation-and-auditing"></a>Azure Key Vault instellen met end-to-end sleutelrotatie en -controle
## <a name="introduction"></a>Inleiding
Na het maken van de sleutelkluis, kunt u zich kunt starten dat kluis gebruiken voor het opslaan van uw sleutels en geheimen. Uw toepassingen niet langer nodig hebt voor het persistent maken van uw sleutels of geheimen, maar in plaats daarvan wordt deze aanvragen op de sleutelkluis indien nodig. Hiermee kunt u sleutels en geheimen bijwerken zonder het gedrag van uw toepassing, wat een groot aantal mogelijkheden om uw sleutel en geheime management wordt geopend.

Dit artikel begeleidt bij een voorbeeld van het gebruik van Azure Key Vault voor het opslaan van een geheim in dit geval een Azure Storage-Account-sleutel die is geopend door een toepassing. U ziet ook uitvoering van een geplande rotatie van de sleutel van die opslagaccount. Ten slotte wordt begeleid bij een demonstratie van het bewaken van de controlelogboeken van de sleutelkluis en waarschuwingen genereren wanneer onverwachte aanvragen worden gedaan.

> [!NOTE]
> Deze zelfstudie is niet bedoeld om uit te leggen in detail de eerste installatie van de sleutelkluis. Zie [Aan de slag met Azure Key Vault](key-vault-get-started.md) voor meer informatie. Zie voor instructies platformoverschrijdende opdrachtregelinterface [Key Vault beheren met CLI](key-vault-manage-with-cli2.md).
>
>

## <a name="set-up-key-vault"></a>Key Vault instellen
Om een toepassing een geheim ophalen uit Sleutelkluis, moet u eerst het geheim maken en uploaden naar uw kluis. Dit kunt doen met Azure PowerShell-sessie starten en het aanmelden bij uw Azure-account met de volgende opdracht:

```powershell
Login-AzureRmAccount
```

Voer in het pop-upvenster in de browser uw gebruikersnaam en wachtwoord voor uw Azure-account in. PowerShell haalt alle abonnementen die gekoppeld aan dit account zijn. PowerShell gebruikt de eerste standaard.

Als u meerdere abonnementen hebt, moet u wellicht opgeven die werd gebruikt voor het maken van de sleutelkluis. Voer de volgende als u wilt zien van de abonnementen voor uw account:

```powershell
Get-AzureRmSubscription
```

Geef het abonnement dat is gekoppeld aan de sleutelkluis die u logboekregistratie wilt inschakelen, voeren:

```powershell
Set-AzureRmContext -SubscriptionId <subscriptionID>
```

Omdat dit artikel wordt beschreven voor het opslaan van de sleutel van een opslagaccount als een geheim, moet u die opslagaccountsleutel ophalen.

```powershell
Get-AzureRmStorageAccountKey -ResourceGroupName <resourceGroupName> -Name <storageAccountName>
```

Bij het ophalen van het geheim (in dit geval wordt een sleutel van uw opslagaccount), moet u die naar een veilige tekenreeks te converteren en vervolgens een geheim maken met de waarde in de sleutelkluis.

```powershell
$secretvalue = ConvertTo-SecureString <storageAccountKey> -AsPlainText -Force

Set-AzureKeyVaultSecret -VaultName <vaultName> -Name <secretName> -SecretValue $secretvalue
```
Haal vervolgens de URI voor het geheim die u hebt gemaakt. Dit wordt gebruikt in een latere stap bij het aanroepen van de sleutelkluis voor het ophalen van het geheim. Voer de volgende PowerShell-opdracht en noteer de ID-waarde de geheime URI is:

```powershell
Get-AzureKeyVaultSecret –VaultName <vaultName>
```

## <a name="set-up-the-application"></a>De toepassing instellen
Nu dat u een geheim dat is opgeslagen hebt, kunt u code ophalen en deze gebruiken. Er zijn een paar stappen vereist om dit te bereiken. De eerste en belangrijkste stap is het registreren van uw toepassing met Azure Active Directory en vervolgens melding Sleutelkluis de informatie over uw toepassing zodat deze aanvragen van uw toepassing.

> [!NOTE]
> Uw toepassing moet worden gemaakt op dezelfde Azure Active Directory-tenant als uw sleutelkluis.
>
>

Open het tabblad toepassingen van Azure Active Directory.

![Geopende toepassingen in Azure Active Directory](./media/keyvault-keyrotation/AzureAD_Header.png)

Kies **toevoegen** een toepassing toevoegen aan uw Azure Active Directory.

![Kies toevoegen](./media/keyvault-keyrotation/Azure_AD_AddApp.png)

Laat het toepassingstype als **WEBTOEPASSING en/of WEB-API** en geef een naam op voor uw toepassing.

![De naam van de toepassing](./media/keyvault-keyrotation/AzureAD_NewApp1.png)

Geef de toepassing een **AANMELDINGS-URL** en een **APP ID URI**. Dit kunnen van alles die voor deze demo gewenste zijn en ze kunnen later worden gewijzigd indien nodig.

![Geef de vereiste URI 's](./media/keyvault-keyrotation/AzureAD_NewApp2.png)

Nadat de toepassing is toegevoegd aan Azure Active Directory, wordt u naar de pagina voor toepassingen worden gezet. Klik op de **configureren** tabblad en klik vervolgens zoeken en kopieer de **Client-ID** waarde. Noteer de client-ID voor de volgende stappen.

Vervolgens wordt een sleutel genereren voor uw toepassing zodat deze met uw Azure Active Directory communiceren kan. U kunt dit onder maken de **sleutels** sectie het **configuratie** tabblad. Let op het zojuist gegenereerde sleutel van uw Azure Active Directory-toepassing voor gebruik in een latere stap.

![Sleutels voor Azure Active Directory-App](./media/keyvault-keyrotation/Azure_AD_AppKeys.png)

Voordat er een aanroepen vanuit uw App in de sleutelkluis, moet u de sleutelkluis Vertel over uw toepassing en de bijbehorende machtigingen. De volgende opdracht wordt de kluisnaam van de en de client-ID van uw Azure Active Directory-app en verleent **ophalen** toegang tot de sleutelkluis voor de toepassing.

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <clientIDfromAzureAD> -PermissionsToSecrets Get
```

Op dit moment bent u klaar om te beginnen met het bouwen van uw toepassing aanroepen. U moet de NuGet-pakketten dat is vereist voor interactie met Azure Key Vault en Azure Active Directory installeren in uw toepassing. Voer de volgende opdrachten vanuit de Visual Studio Package Manager-console. Bij het schrijven van dit artikel is de huidige versie van het pakket met Azure Active Directory 3.10.305231913, dus raadzaam om te bevestigen van de meest recente versie en dienovereenkomstig bijgewerkt.

```powershell
Install-Package Microsoft.IdentityModel.Clients.ActiveDirectory -Version 3.10.305231913

Install-Package Microsoft.Azure.KeyVault
```

Maak een klasse voor het opslaan van de methode voor uw Azure Active Directory-verificatie in uw toepassingscode. In dit voorbeeld die klasse wordt aangeroepen **Utils**. Voeg het volgende toe met de instructie:

```csharp
using Microsoft.IdentityModel.Clients.ActiveDirectory;
```

Voeg vervolgens de volgende methode voor het ophalen van de JWT-token van Azure Active Directory. Voor onderhoud wilt u mogelijk verplaatsen van de vastgelegde tekenreekswaarden in de configuratie van uw website of toepassing.

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

Voeg de benodigde code voor het aanroepen van de Sleutelkluis en uw geheime waarde wordt opgehaald. Eerst moet u het volgende toevoegen met de instructie:

```csharp
using Microsoft.Azure.KeyVault;
```

Het methodeaanroepen van de invoke Sleutelkluis en ophalen van het geheim toevoegen. Deze methode geeft u het geheim URI die u hebt opgeslagen in de vorige stap. Let op het gebruik van de **GetToken** methode van de **Utils** klasse eerder hebt gemaakt.

```csharp
var kv = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(Utils.GetToken));

var sec = kv.GetSecretAsync(<SecretID>).Result.Value;
```

Wanneer u uw toepassing uitvoert, nu moet u verificatie met Azure Active Directory en vervolgens uw geheime waarde ophalen van Azure Sleutelkluis.

## <a name="key-rotation-using-azure-automation"></a>Sleutel rotatie met behulp van Azure Automation
Er zijn diverse opties voor het implementeren van een strategie worden gedraaid voor waarden die u als Azure Key Vault geheimen opslaan. Geheimen kunnen worden gedraaid als onderdeel van een handmatig proces, kan programmatisch met behulp van API-aanroepen worden gedraaid of ze in een automatiseringsscript kunnen worden gedraaid. Voor de doeleinden van dit artikel gaat u Azure PowerShell in combinatie met Azure Automation gebruiken een toegangssleutel voor Azure Storage-Account wijzigen. U wordt vervolgens een geheim sleutelkluis bijwerken met deze nieuwe sleutel.

Als u Azure Automation geheime waarden instellen voor uw sleutelkluis, moet u de client-ID ophalen voor de verbinding met de naam AzureRunAsConnection die is gemaakt wanneer u uw exemplaar van Azure Automation tot stand gebracht. U kunt deze ID vinden door te kiezen **activa** van uw Azure Automation-exemplaar. Van daaruit, kiest u **verbindingen** en selecteer vervolgens de **AzureRunAsConnection** principe service. Noteer de **toepassings-ID**.

![Azure Automation-client-ID](./media/keyvault-keyrotation/Azure_Automation_ClientID.png)

In **activa**, kies **Modules**. Van **Modules**, selecteer **galerie**, en zoek vervolgens naar en **importeren** bijgewerkte versies van elk van de volgende modules:

    Azure
    Azure.Storage
    AzureRM.Profile
    AzureRM.KeyVault
    AzureRM.Automation
    AzureRM.Storage


> [!NOTE]
> Bij het schrijven van dit artikel wordt alleen de modules die eerder is vermeld nodig om te worden bijgewerkt in verband met het volgende script. Als u merkt dat uw automation-taak is mislukt, moet u bevestigen dat u alle benodigde modules en de bijbehorende afhankelijkheden hebt geïmporteerd.
>
>

Nadat u hebt de toepassings-ID opgehaald voor uw Azure Automation-verbinding, moet u de sleutelkluis zien dat deze toepassing toegang heeft tot bijwerken van geheimen in uw kluis aan te geven. Dit kan worden bewerkstelligd met de volgende PowerShell-opdracht:

```powershell
Set-AzureRmKeyVaultAccessPolicy -VaultName <vaultName> -ServicePrincipalName <applicationIDfromAzureAutomation> -PermissionsToSecrets Set
```

Selecteer vervolgens **Runbooks** onder uw Azure Automation-instantie en selecteer vervolgens **een Runbook toevoegen**. Selecteer **Snelle invoer**. De naam van uw runbook en selecteer **PowerShell** als het runbooktype. U hebt de optie voor het toevoegen van een beschrijving. Tot slot op **maken**.

![Runbook maken](./media/keyvault-keyrotation/Create_Runbook.png)

Plak het volgende PowerShell-script in het venster editor voor uw nieuwe runbook:

```powershell
$connectionName = "AzureRunAsConnection"
try
{
    # Get the connection "AzureRunAsConnection "
    $servicePrincipalConnection=Get-AutomationConnection -Name $connectionName         

    "Logging in to Azure..."
    Add-AzureRmAccount `
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

Kies in het deelvenster editor **testvenster** voor het testen van uw script. Wanneer het script wordt uitgevoerd zonder fouten, kunt u selecteren **publiceren**, en vervolgens kunt u een planning voor het runbook terug in het deelvenster runbook configuratie toepassen.

## <a name="key-vault-auditing-pipeline"></a>Controle Sleutelkluis-pipeline
Wanneer u een sleutelkluis hebt ingesteld, kunt u de controle voor het verzamelen van Logboeken op toegangsaanvragen voor de sleutelkluis. Deze logboeken worden opgeslagen in een aangewezen Azure Storage-account en kunnen worden opgevraagd, bewaakt en geanalyseerd. Het volgende scenario maakt gebruik van Azure functions, Azure logic apps en sleutelkluis controlelogboeken voor het maken van een pijplijn een e-mailbericht verzenden wanneer een app die overeenkomt met de app-ID van de web-app geheimen opgehaald uit de kluis.

Eerst moet u de sleutelkluis aanmelden inschakelen. U kunt dit doen via de volgende PowerShell-opdrachten (alle gegevens die kunnen worden weergegeven wanneer [sleutel kluis registreren](key-vault-logging.md)):

```powershell
$sa = New-AzureRmStorageAccount -ResourceGroupName <resourceGroupName> -Name <storageAccountName> -Type Standard\_LRS -Location 'East US'
$kv = Get-AzureRmKeyVault -VaultName '<vaultName>'
Set-AzureRmDiagnosticSetting -ResourceId $kv.ResourceId -StorageAccountId $sa.Id -Enabled $true -Categories AuditEvent
```

Nadat dit is ingeschakeld, wordt de auditlogboeken start verzamelen in het opgegeven opslagaccount. Deze logboeken bevatten gebeurtenissen over hoe en wanneer uw sleutelkluizen toegankelijk zijn en door wie.

> [!NOTE]
> U kunt toegang tot uw logboekgegevens 10 minuten nadat de sleutelkluis-bewerking. Dit wordt meestal sneller worden.
>
>

De volgende stap is het [maken van een Azure Service Bus-wachtrij](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md). Dit is waar sleutelkluis controlelogboeken worden gepusht. Wanneer de wachtrij de berichten voor het controlelogboek, wordt de logische app ze opneemt en uitgevoerd op deze. Een servicebus Maak met de volgende stappen uit:

1. Een Service Bus-naamruimte maken (als u al die u wilt gebruiken voor deze, gaat u verder met stap 2 hebt).
2. Blader naar de servicebus in de Azure portal en selecteer de naamruimte die u wilt maken van de wachtrij in.
3. Selecteer **nieuw** en kies **Service Bus > wachtrij** en voer de vereiste gegevens.
4. Selecteer de Service Bus-verbindingsinformatie door de naamruimte kiezen en op **verbindingsgegevens**. U moet deze informatie gebruiken voor de volgende sectie.

Vervolgens [maken van een Azure-functie](../azure-functions/functions-create-first-azure-function.md) om te pollen sleutelkluis-Logboeken in het opslagaccount en nieuwe gebeurtenissen kunnen worden opgepikt. Dit is een functie die volgens een planning wordt geactiveerd.

Kies voor het maken van een Azure-functie **Nieuw > functie-App** in de Azure portal. U kunt tijdens het maken van een bestaand abonnement hosting gebruiken of een nieuwe maken. U kunt ook kiezen voor het hosten van dynamische. Meer informatie over de functie opties host kunnen worden gevonden op [Azure Functions schalen](../azure-functions/functions-scale.md).

Wanneer de functie Azure is gemaakt, gaat u naar het en kies een timer functie en C\#. Klik vervolgens op **maken van deze functie**.

![Blade van Azure Functions starten](./media/keyvault-keyrotation/Azure_Functions_Start.png)

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
> Zorg ervoor dat u de variabelen in de bovenstaande code verwijzen naar uw opslagaccount waarin de sleutelkluis-Logboeken geschreven, de servicebus die u eerder hebt gemaakt en het specifieke pad naar de sleutelkluis-logboeken voor opslag.
>
>

De functie neemt over de meest recente logboekbestand vanuit het opslagaccount waarin de sleutelkluis-Logboeken geschreven, pakt de meest recente gebeurtenissen uit dat bestand en stuurt deze naar een Service Bus-wachtrij. Aangezien één enkel bestand meerdere gebeurtenissen hebben kan, moet u een sync.txt-bestand dat de functie wordt ook bekeken om te bepalen van de tijdstempel van de laatste gebeurtenis die is opgehaald. Dit zorgt ervoor dat u geen dezelfde gebeurtenis meerdere keren pushen. Dit bestand sync.txt bevat een tijdstempel voor laatste gebeurtenis aangetroffen. De logboeken als geladen, moeten worden gesorteerd op basis van het tijdstempel om te controleren of dat ze correct zijn gerangschikt.

Voor deze functie, verwijzen we naar een aantal aanvullende bibliotheken die niet beschikbaar gebruiksklaar in Azure Functions. Zodanig dat deze, moeten we Azure Functions om op te halen ze met NuGet. Kies de **bestanden weergeven** optie.

![Optie bestanden weergeven](./media/keyvault-keyrotation/Azure_Functions_ViewFiles.png)

En toevoegen van een bestand met de naam project.json met de volgende inhoud:

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
Bij **opslaan**, Azure Functions vereiste binaire bestanden worden gedownload.

Overschakelen naar de **integreren** tabblad en geef de parameter timer een betekenisvolle naam te gebruiken binnen de functie. In de voorgaande code wordt ervan uitgegaan dat de timer moet worden aangeroepen *myTimer*. Geef een [CRON expressie](../app-service/web-sites-create-web-jobs.md#CreateScheduledCRON) als volgt: 0 \* \* \* \* \* voor de timer waardoor de functie eenmaal een minuut uitvoeren.

Op dezelfde **integreren** tabblad, het toevoegen van een invoer van het type **Azure Blob Storage**. Dit verwijst naar het sync.txt-bestand dat de tijdstempel van de laatste gebeurtenis bekeken door de functie bevat. Dit zijn beschikbaar binnen de functie door de parameternaam van de. In de vorige code de Azure Blob Storage-invoer verwacht de parameternaam moet *inputBlob*. Kies het opslagaccount waarin het bestand sync.txt wordt opgeslagen (kan zijn hetzelfde of een ander opslagaccount). In het padveld geeft u het pad waar het bestand zich in de notatie {container-name}/path/to/sync.txt.

Toevoegen van de uitvoer van het type *Azure Blob Storage* uitvoer. Dit verwijst naar het sync.txt-bestand dat u hebt gedefinieerd in de invoer. Dit wordt gebruikt door de functie voor het schrijven van het tijdstempel van de laatste gebeurtenis bekeken. De bovenstaande code verwacht dat deze parameter moet worden aangeroepen *outputBlob*.

De functie is nu klaar. Zorg ervoor dat u gaat u terug naar de **ontwikkelen** tabblad en de code op te slaan. Controleer het uitvoervenster op compilatiefouten en corrigeer deze dienovereenkomstig. Als de code wordt gecompileerd, moet vervolgens de code nu worden elke minuut van de sleutelkluis-logboeken controleren en eventuele nieuwe gebeurtenissen naar de gedefinieerde Service Bus-wachtrij worden gepusht. U ziet logboekinformatie wegschrijven naar het logboekvenster telkens wanneer de functie wordt geactiveerd.

### <a name="azure-logic-app"></a>Azure logic app
Vervolgens moet u een Azure logic app die neemt over de gebeurtenissen met de functie pusht naar de Service Bus-wachtrij, parseert de inhoud en verzendt een e-mail op basis van een voorwaarde wordt aangepast maken.

[Een logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md) door te gaan naar **Nieuw > logische App**.

Zodra de logische app is gemaakt, gaat u naar het en kies **bewerken**. Kies in de editor logic app **Service Bus-wachtrij** en voer uw Service Bus-referenties om te verbinden met de wachtrij.

![Azure Logic App Service Bus](./media/keyvault-keyrotation/Azure_LogicApp_ServiceBus.png)

Kies vervolgens **een voorwaarde toevoegen**. Overschakelen naar de geavanceerde editor in de voorwaarde, en voer de volgende code, APP_ID vervangen door de werkelijke APP_ID van uw web-app:

```
@equals('<APP_ID>', json(decodeBase64(triggerBody()['ContentData']))['identity']['claim']['appid'])
```

Deze expressie retourneert in wezen **false** als de *appid* van de inkomende gebeurtenis (dit is de hoofdtekst van het bericht Service Bus) is niet de *appid* van de app.

Maak nu een actie onder **zo Nee, niets doen**.

![Azure Logic App kiezen actie](./media/keyvault-keyrotation/Azure_LogicApp_Condition.png)

Kies voor de actie **Office 365 - e-mailbericht verzenden**. Vul de velden voor het maken van een e-mailbericht verzenden wanneer de ingestelde voorwaarde retourneert **false**. Als u geen Office 365 hebt, kunt u alternatieven voor dezelfde resultaten bereiken bekijken.

U hebt op dit moment een end-to-end-pijplijn eens per minuut wordt gezocht naar de nieuwe sleutelkluis controlelogboeken. Deze verstuurd nieuwe logboeken vindt naar een service bus-wachtrij. De logische app wordt geactiveerd wanneer een nieuw bericht in de wachtrij terechtkomt. Als de *appid* binnen de gebeurtenis komt niet overeen met de app-ID van de oproepende toepassing, zendt het een e-mailbericht.

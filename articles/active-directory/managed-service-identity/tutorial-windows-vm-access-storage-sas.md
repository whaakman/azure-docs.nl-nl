---
title: Een Windows VM-MSI gebruiken voor toegang tot Azure Storage met behulp van een SAS-referenties
description: Een zelfstudie waarin wordt uitgelegd hoe u een Windows VM Managed Service Identity (MSI) gebruikt voor toegang tot Azure Storage, met behulp van een SAS-referenties in plaats van een toegangssleutel voor opslag.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 2648c2ea0e54a3713bb5d89bf5cf5fcd029a9f99
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39006441"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Zelfstudie: Een Windows VM beheerde Service-identiteit gebruiken voor toegang tot Azure Storage via SAS-referenties

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie leert u hoe u Managed Service Identity (MSI) inschakelen voor een Windows virtuele Machine vervolgens het MSI-bestand gebruiken om te verkrijgen van een Shared Access Signature (SAS) storage referentie. Met name een [referentie-Service-SAS](/azure/storage/common/storage-dotnet-shared-access-signature-part-1?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Een Service-SAS biedt de mogelijkheid beperkte toegang verlenen tot objecten in een storage-account gedurende beperkte tijd en een bepaalde service (in ons geval de blob-service), zonder dat een toegangssleutel. U kunt een SAS-referenties zoals gebruikelijk bij het maken van opslag-bewerkingen, bijvoorbeeld bij het gebruik van de Storage-SDK gebruiken. Voor deze zelfstudie ziet uploaden en downloaden van een blob met behulp van PowerShell voor Azure-opslag. U leert hoe u:


> [!div class="checklist"]
> * MSI-bestand op een virtuele Machine van Windows inschakelen 
> * Uw virtuele machine toegang verlenen tot een opslagaccount SAS in Resource Manager 
> * Een toegangstoken met behulp van de identiteit van de virtuele machine en deze gebruiken om op te halen van de SAS van Resource Manager 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows-VM. U kunt ook de MSI-bestand op een bestaande virtuele machine inschakelen.

1.  Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van Azure portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakt als volgt de referenties die u gebruiken voor aanmelding bij de virtuele machine.
4.  Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Selecteer een nieuwe **resourcegroep** u graag aan virtuele machine moet worden gemaakt in, kies **nieuw**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

    ![De installatiekopie van de ALT-tekst](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI-bestand op de virtuele machine inschakelen

Een MSI-bestand voor de virtuele Machine kunt u tokens voor toegang van Azure AD ophalen zonder dat u om referenties in uw code. Op de achtergrond inschakelen van MSI doet twee dingen: registreert uw virtuele machine met Azure Active Directory voor het maken van de beheerde identiteit, en configureert u de identiteit op de virtuele machine.

1. Navigeer naar de resourcegroep van uw nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Klik onder de virtuele machine 'Instellingen' in het linkerdeelvenster op **configuratie**.
3. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Controleer of u klikken op **opslaan** aan de configuratie op te slaan.

    ![De installatiekopie van de ALT-tekst](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

## <a name="create-a-storage-account"></a>Create a storage account 

Als u dit niet al hebt, maakt u nu een storage-account. Ook kunt u deze stap overslaan en uw VM MSI toegang verlenen tot de SAS-referenties van een bestaand opslagaccount. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van Azure portal.
2. Klik op **opslag**, klikt u vervolgens **Opslagaccount**, en een nieuw deelvenster voor 'Storage-account maken' wordt weergegeven.
3. Voer een naam voor de storage-account, die u later gaat gebruiken.  
4. **Implementatiemodel** en **soort Account** moet worden ingesteld op 'Resource manager' en 'Algemeen', respectievelijk. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven tijdens het maken van uw virtuele machine in de vorige stap.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Maak een blobcontainer in de storage-account

We zullen later uploaden en een bestand downloaden naar het nieuwe opslagaccount. Omdat bestanden blob-opslag vereist, moet het maken van een blob-container waarin het bestand op te slaan.

1. Ga terug naar de zojuist gemaakte opslagaccount.
2. Klik op de **Containers** koppeling in het linkerdeelvenster, onder 'blobservice'.
3. Klik op **+ Container** boven aan de pagina en een 'nieuwe 'container deelvenster schuift.
4. Geef een naam op voor de container, selecteert u een toegangsniveau en klik op **OK**. De naam die u hebt opgegeven, wordt verderop in de zelfstudie worden gebruikt. 

    ![Opslagcontainer maken](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Verlenen van de virtuele machine MSI toegang voor het gebruik van een SAS-opslag 

Azure Storage biedt geen systeemeigen ondersteuning voor Azure AD-verificatie.  U kunt echter een MSI gebruiken voor het ophalen van een SAS-opslag van Resource Manager en vervolgens de SAS gebruiken voor toegang tot opslag.  In deze stap maakt verlenen u uw VM MSI toegang aan uw storage-account-SAS.   

1. Ga terug naar de zojuist gemaakte opslagaccount.   
2. Klik op de **toegangsbeheer (IAM)** koppeling in het linkerdeelvenster.  
3. Klik op **+ toevoegen** boven op de pagina om een nieuwe roltoewijzing voor de virtuele machine
4. Stel **rol** naar 'Inzender voor Opslagaccounts", aan de rechterkant van de pagina.  
5. Stel in de volgende vervolgkeuzelijst **toegang toewijzen aan** de resource 'Virtuele Machine'.  
6. Controleer vervolgens of het juiste abonnement wordt weergegeven in **abonnement** vervolgkeuzelijst, stel **resourcegroep** op 'Alle resourcegroepen'.  
7. Ten slotte onder **Selecteer** kiest u uw Windows-Machine in de vervolgkeuzelijst en klik vervolgens op **opslaan**. 

    ![De installatiekopie van de ALT-tekst](../media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Een toegangstoken met behulp van de identiteit van de virtuele machine en gebruiken voor het aanroepen van Azure Resource Manager 

Voor de rest van de zelfstudie werken we van de virtuele machine die we eerder hebben gemaakt.

U moet het gebruik van de Azure Resource Manager PowerShell-cmdlets in dit gedeelte.  Als u deze nog niet hebt geïnstalleerd, [download de nieuwste versie](https://docs.microsoft.com/powershell/azure/overview) voordat u doorgaat.

1. In de Azure-portal, gaat u naar **virtuele Machines**, gaat u naar uw Windows virtuele machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant.
2. Geef in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd bij het maken van de Windows-VM. 
3. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** met de virtuele machine, opent u PowerShell in de externe sessie. 
4. Met behulp van Powershell Invoke-WebRequest, een aanvraag indienen op de lokale MSI-eindpunt op een toegangstoken ophalen voor Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fmanagement.azure.com%2F' -Method GET -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > De waarde van de parameter 'resource' moet een exacte overeenkomst voor wat er door Azure AD wordt verwacht. Wanneer u de resource-ID van Azure Resource Manager gebruikt, moet u de afsluitende slash bevatten in de URI bevatten.
    
    Het element 'Inhoud', dat wordt opgeslagen als een tekenreeks JavaScript Object Notation (JSON)-indeling in het object $response vervolgens uitpakken. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Haal het toegangstoken vervolgens uit het antwoord.
    
    ```powershell
    $ArmToken = $content.access_token
    ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>SAS-referenties ophalen van Azure Resource Manager voor opslag-aanroepen 

Nu PowerShell gebruiken voor het aanroepen van Resource Manager met behulp van het toegangstoken dat we opgehaald in de vorige sectie, een opslag-SAS-referentie maken. Zodra we de SAS-referenties hebt, kunnen we opslagbewerkingen aanroepen.

Voor deze aanvraag gebruiken we de aanvraagparameters van de volgende HTTP-de SAS-referentie maken:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite. Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Deze parameters zijn opgenomen in de hoofdtekst van bericht van de aanvraag voor de SAS-referenties. Zie voor meer informatie over de parameters voor het maken van een SAS-referenties, het [naslaginformatie over de lijst met Service-SAS REST](/rest/api/storagerp/storageaccounts/listservicesas).

Eerst de parameters niet converteren naar JSON, vervolgens de storage aanroepen `listServiceSas` referentie-eindpunt om de SAS te maken:

```powershell
$params = @{canonicalizedResource="/blob/<STORAGE-ACCOUNT-NAME>/<CONTAINER-NAME>";signedResource="c";signedPermission="rcw";signedProtocol="https";signedExpiry="2017-09-23T00:00:00Z"}
$jsonParams = $params | ConvertTo-Json
```

```powershell
$sasResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT-NAME>/listServiceSas/?api-version=2017-06-01 -Method POST -Body $jsonParams -Headers @{Authorization="Bearer $ArmToken"}
```
> [!NOTE] 
> De URL is hoofdlettergevoelig, dus zorg ervoor dat u precies hetzelfde hoofdlettergebruik eerder hebt, gebruikt toen u de naam van de resourcegroep, met inbegrip van de hoofdletters "G" in "resourceGroups." 

Nu kunnen we de SAS-referenties ophalen uit het antwoord:

```powershell
$sasContent = $sasResponse.Content | ConvertFrom-Json
$sasCred = $sasContent.serviceSasToken
```

Als u de SAS-referenties downloaden inspecteren ziet er ongeveer als volgt op:

```powershell
PS C:\> $sasCred
sv=2015-04-05&sr=c&spr=https&se=2017-09-23T00%3A00%3A00Z&sp=rcw&sig=JVhIWG48nmxqhTIuN0uiFBppdzhwHdehdYan1W%2F4O0E%3D
```

Vervolgens maken we een bestand met de naam 'test.txt'. Gebruik vervolgens de SAS-referenties om te verifiëren met de `New-AzureStorageContent` cmdlet, upload het bestand naar de blob-container en vervolgens het bestand te downloaden.

```bash
echo "This is a test text file." > test.txt
```

Zorg ervoor dat de Azure Storage-cmdlets eerst installeren met behulp van `Install-Module Azure.Storage`. Upload de blob die u zojuist hebt gemaakt, met behulp van de `Set-AzureStorageBlobContent` PowerShell-cmdlet:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT-NAME> -SasToken $sasCred
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Antwoord:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

U kunt ook downloaden met de blob die u net hebt geüpload, met behulp van de `Get-AzureStorageBlobContent` PowerShell-cmdlet:

```powershell
Get-AzureStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Antwoord:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/21/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd over het maken van een Service-identiteit beheerd voor toegang tot Azure Storage met behulp van een SAS-referenties.  Zie voor meer informatie over Azure Storage-SAS:

> [!div class="nextstepaction"]
>[Shared access signatures (SAS) gebruiken](/azure/storage/common/storage-dotnet-shared-access-signature-part-1)



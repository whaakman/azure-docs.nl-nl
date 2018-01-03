---
title: Gebruik van een Windows-VM-MSI voor toegang tot Azure Storage
description: Een zelfstudie die u bij het proces helpt van het gebruik van een Windows VM beheerde Service identiteit (MSI) voor toegang tot Azure Storage.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: e7f715acc3ab343a36e0968701894fd29ff8af71
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-access-key"></a>Een Windows VM beheerde Service-identiteit gebruiken voor toegang tot Azure Storage via toegangssleutel

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe u beheerde Service identiteit (MSI) inschakelen voor een virtuele Windows-Machine en vervolgens die identiteit gebruiken voor het ophalen van de toegangssleutels voor opslag-account. U kunt gewoon bij het uitvoeren van opslagbewerkingen, bijvoorbeeld bij het gebruik van de opslag-SDK toegangssleutels voor opslag gebruiken. Voor deze zelfstudie we uploaden en downloaden van blobs met Azure Storage PowerShell. U leert hoe:


> [!div class="checklist"]
> * MSI op een virtuele Machine van Windows inschakelen 
> * Uw VM toegang verlenen tot toegang toegangscodes voor opslag in Resource Manager 
> * Ophalen van een toegangstoken die met de identiteit van de VM, en deze gebruiken voor het ophalen van de toegangssleutels voor opslag van Resource Manager 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows VM. U kunt ook MSI op een bestaande virtuele machine inschakelen.

1.  Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakte Hier ziet u de referenties die u kunt aanmelden bij de virtuele machine.
4.  Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Selecteer een nieuwe **resourcegroep** u wilt virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen

De MSI van een virtuele Machine kunt u toegangstokens ophalen uit Azure AD zonder dat u referenties in uw code te plaatsen. Achter de MSI inschakelen biedt twee dingen: het installeren van de MSI-VM-extensie op uw virtuele machine en zorgt ervoor dat MSI voor de virtuele Machine.  

1. Navigeer naar de resourcegroep van de nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Klik in de virtuele machine 'Instellingen' aan de linkerkant op **configuratie**.
3. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren welke uitbreidingen zijn op de virtuele machine, klikt u op **extensies**. Als MSI is ingeschakeld, de **ManagedIdentityExtensionforWindows** wordt weergegeven in de lijst.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Een opslagaccount maken 

Als u dit niet al hebt, maakt u nu een opslagaccount. Ook kunt u deze stap overslaan en uw VM MSI toegang verlenen tot de sleutels van een bestaand opslagaccount. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Klik op **opslag**, klikt u vervolgens **Opslagaccount**, en een nieuw deelvenster voor 'Storage-account maken' wordt weergegeven.
3. Voer een naam voor de storage-account, gaat u later gebruiken.  
4. **Implementatiemodel** en **Account kind** moet worden ingesteld op 'Resource manager' en 'Algemene', respectievelijk. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven toen u uw virtuele machine in de vorige stap hebt gemaakt.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Een blob-container in het opslagaccount maken

Later wordt uploaden en downloaden van een bestand met de nieuwe opslagaccount. Omdat bestanden blob-opslag is vereist, moeten we maken van een blob-container waarin het bestand wilt opslaan.

1. Ga terug naar uw nieuwe opslagaccount.
2. Klik op de **Containers** koppeling aan de linkerkant onder 'Blob-service'.
3. Klik op **+ Container** boven aan de pagina en een 'nieuwe container' Configuratiescherm dia's uit.
4. Geef een naam op voor de container, selecteer een toegangsniveau en klik op **OK**. De opgegeven naam wordt verderop in de zelfstudie gebruikt. 

    ![Opslagcontainer maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-storage-account-access-keys"></a>Uw VM MSI-toegang voor het gebruik van sleutels van opslagaccount toegang verlenen 

Azure Storage biedt geen systeemeigen ondersteuning voor Azure AD-verificatie.  U kunt echter een MSI-bestand gebruiken voor het ophalen van opslagtoegangssleutels-account van de Resource Manager en vervolgens een sleutel wordt gebruikt voor toegang tot opslag.  In deze stap maakt u uw VM MSI toegang verlenen tot de sleutels naar uw opslagaccount.   

1. Ga terug naar uw nieuwe opslagaccount.  
2. Klik op de **toegangsbeheer (IAM)** koppeling in het linkerdeelvenster.  
3. Klik op **+ toevoegen** boven op de pagina om een nieuwe roltoewijzing voor de virtuele machine
4. Stel **rol** naar 'Storage Account sleutel Operator Service rol', aan de rechterkant van de pagina. 
5. Stel in de volgende dropdown **toewijzen van toegang tot** de resource 'Virtuele Machine'.  
6. Controleer vervolgens het juiste abonnement wordt vermeld in **abonnement** vervolgkeuzelijst Stel **resourcegroep** aan 'Alle resourcegroepen'.  
7. Ten slotte onder **Selecteer** kiest u uw Windows-Machine in de vervolgkeuzelijst en klik vervolgens op **opslaan**. 

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Ophalen van een toegangstoken die met de identiteit van de VM en deze gebruiken om aan te roepen Azure Resource Manager 

Voor het restant van de zelfstudie werken we van de virtuele machine die we eerder hebben gemaakt. 

U moet de Azure Resource Manager PowerShell-cmdlets in dit gedeelte gebruiken.  Als u niet geïnstalleerd hebt, [de nieuwste versie downloaden](https://docs.microsoft.com/powershell/azure/overview) voordat u doorgaat.

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw Windows-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. 
2. Voer in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd tijdens het maken van de virtuele machine van Windows. 
3. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** met de virtuele machine, opent u PowerShell in de externe sessie.
4. Maak met behulp van Powershell Invoke-WebRequest, een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > De waarde van de resourceparameter '' moet een exacte overeenkomst voor door Azure AD wordt verwacht. Wanneer u de resource-ID van Azure Resource Manager gebruikt, moet u de afsluitende slash op de URI opnemen.
    
    Pak vervolgens het element 'Inhoud', dat wordt opgeslagen als een tekenreeks in JSON JavaScript Object Notation () geformatteerd in de $response-object. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Haal het toegangstoken vervolgens uit het antwoord.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Toegangssleutels voor opslag-account ophalen van Azure Resource Manager storage aanroepen  

Nu PowerShell gebruiken om aan te roepen Resource Manager met behulp van het toegangstoken dat we opgehaald in de vorige sectie voor het ophalen van de toegangssleutel voor opslag. Wanneer we de toegangssleutel voor opslag hebt, noemen we opslagbewerkingen uploaden/downloaden.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> De URL is hoofdlettergevoelig, dus zorg ervoor dat u eerder gebruikt wanneer u de resourcegroep, met inbegrip van de hoofdletters 'G' in "resourceGroups." met de naam exact hetzelfde hoofdlettergebruik gebruiken 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Vervolgens maken we een bestand met de naam 'test.txt'. De toegangssleutel voor opslag vervolgens gebruiken voor verificatie met de `New-AzureStorageContent` cmdlet, upload het bestand naar de blob-container en vervolgens het bestand downloaden.

```bash
echo "This is a test text file." > test.txt
```

Moet u eerst de Azure Storage-cmdlets installeren met behulp van `Install-Module Azure.Storage`. Upload de blob die u zojuist hebt gemaakt, met behulp van de `Set-AzureStorageBlobContent` PowerShell-cmdlet:

```powershell
$ctx = New-AzureStorageContext -StorageAccountName <STORAGE-ACCOUNT> -StorageAccountKey $key
Set-AzureStorageBlobContent -File test.txt -Container <CONTAINER-NAME> -Blob testblob -Context $ctx
```

Antwoord:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```

U kunt ook downloaden de blob die u zojuist hebt geüpload, met behulp van de `Get-AzureStorageBlobContent` PowerShell-cmdlet:

```powershell
Get-AzureStorageBlobContent -Blob testblob -Container <CONTAINER-NAME> -Destination test2.txt -Context $ctx
```

Antwoord:

```powershell
ICloudBlob        : Microsoft.WindowsAzure.Storage.Blob.CloudBlockBlob
BlobType          : BlockBlob
Length            : 56
ContentType       : application/octet-stream
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```


## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Zie voor meer informatie over hoe u doet dit dezelfde zelfstudie gebruik van een SAS-referentie van opslag, [gebruiken van een Windows VM beheerde Service-identiteit voor toegang tot Azure Storage via een SAS-referentie](msi-tutorial-windows-vm-access-storage-sas.md)
- Zie voor meer informatie over de SAS-functie van Azure Storage-account:
  - [Met behulp van handtekeningen voor gedeelde toegang (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Maken van een Service-SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm



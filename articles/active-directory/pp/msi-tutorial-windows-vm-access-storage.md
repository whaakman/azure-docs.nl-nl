---
title: Een Windows VM-MSI gebruiken voor toegang tot Azure Storage
description: Een zelfstudie die u helpt bij het proces van het gebruik van een Windows VM Managed Service Identity (MSI) voor toegang tot Azure Storage.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: daveba
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: a195fa62efa7332f7268c195a3edfb0a8b1d0a7c
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007679"
---
# <a name="use-a-windows-vm-managed-service-identity-to-access-azure-storage-via-access-key"></a>Een Windows VM beheerde Service-identiteit gebruiken voor toegang tot Azure Storage via toegangssleutel

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie leert u hoe u Managed Service Identity (MSI) inschakelen voor een Windows-Machine en vervolgens die identiteit gebruiken om op te halen van opslagaccountsleutels. Hier kunt u toegangssleutels voor opslag zoals gebruikelijk bij het maken van opslag-bewerkingen, bijvoorbeeld bij het gebruik van de Storage-SDK. Voor deze zelfstudie, we blobs uploaden en downloaden met behulp van PowerShell voor Azure-opslag. U leert hoe u:


> [!div class="checklist"]
> * MSI-bestand op een virtuele Machine van Windows inschakelen 
> * Uw virtuele machine toegang verlenen tot opslagaccountsleutels in Resource Manager 
> * Een toegangstoken met behulp van de identiteit van de virtuele machine en deze gebruiken om op te halen van de toegangssleutels voor opslag van Resource Manager 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

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

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI-bestand op de virtuele machine inschakelen

Een MSI-bestand voor de virtuele Machine kunt u tokens voor toegang van Azure AD ophalen zonder dat u om referenties in uw code. Op de achtergrond inschakelen van MSI doet twee dingen: de MSI-VM-extensie worden geïnstalleerd op de virtuele machine en zorgt ervoor dat MSI-bestand voor de virtuele Machine.  

1. Navigeer naar de resourcegroep van uw nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Klik onder de virtuele machine 'Instellingen' aan de linkerkant op **configuratie**.
3. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Controleer of u klikken op **opslaan** aan de configuratie op te slaan.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren welke uitbreidingen zijn op de virtuele machine, klikt u op **extensies**. Als het MSI-bestand is ingeschakeld, de **ManagedIdentityExtensionforWindows** wordt weergegeven in de lijst.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Create a storage account 

Als u dit niet al hebt, maakt u nu een storage-account. Ook kunt u deze stap overslaan en uw VM MSI toegang verlenen tot de sleutels van een bestaand opslagaccount. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van Azure portal.
2. Klik op **opslag**, klikt u vervolgens **Opslagaccount**, en een nieuw deelvenster voor 'Storage-account maken' wordt weergegeven.
3. Voer een naam voor de storage-account, die u later gaat gebruiken.  
4. **Implementatiemodel** en **soort Account** moet worden ingesteld op 'Resource manager' en 'Algemeen', respectievelijk. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven tijdens het maken van uw virtuele machine in de vorige stap.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Maak een blobcontainer in de storage-account

We zullen later uploaden en een bestand downloaden naar het nieuwe opslagaccount. Omdat bestanden blob-opslag vereist, moet het maken van een blob-container waarin het bestand op te slaan.

1. Ga terug naar de zojuist gemaakte opslagaccount.
2. Klik op de **Containers** koppelen aan de linkerkant, onder 'blobservice'.
3. Klik op **+ Container** boven aan de pagina en een 'nieuwe 'container deelvenster schuift.
4. Geef een naam op voor de container, selecteert u een toegangsniveau en klik op **OK**. De naam die u hebt opgegeven, wordt verderop in de zelfstudie worden gebruikt. 

    ![Opslagcontainer maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-storage-account-access-keys"></a>MSI-toegang van de virtuele machine voor het gebruik van storage-accountsleutels toegang verlenen 

Azure Storage biedt geen systeemeigen ondersteuning voor Azure AD-verificatie.  U kunt echter een MSI gebruiken voor toegang tot opslagaccountsleutels ophalen van de Resource Manager en vervolgens een sleutel wordt gebruikt voor toegang tot opslag.  In deze stap maakt verlenen u uw VM MSI toegang tot de sleutels naar uw opslagaccount.   

1. Ga terug naar de zojuist gemaakte opslagaccount.  
2. Klik op de **toegangsbeheer (IAM)** koppeling in het linkerdeelvenster.  
3. Klik op **+ toevoegen** boven op de pagina om een nieuwe roltoewijzing voor de virtuele machine
4. Stel **rol** 'Storage Account Key Service rol Operator', aan de rechterkant van de pagina. 
5. Stel in de volgende vervolgkeuzelijst **toegang toewijzen aan** de resource 'Virtuele Machine'.  
6. Controleer vervolgens of het juiste abonnement wordt weergegeven in **abonnement** vervolgkeuzelijst, stel **resourcegroep** op 'Alle resourcegroepen'.  
7. Ten slotte onder **Selecteer** kiest u uw Windows-Machine in de vervolgkeuzelijst en klik vervolgens op **opslaan**. 

    ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Een toegangstoken met behulp van de identiteit van de virtuele machine en gebruiken voor het aanroepen van Azure Resource Manager 

Voor de rest van de zelfstudie werken we van de virtuele machine die we eerder hebben gemaakt. 

U moet het gebruik van de Azure Resource Manager PowerShell-cmdlets in dit gedeelte.  Als u deze nog niet hebt geïnstalleerd, [download de nieuwste versie](https://docs.microsoft.com/powershell/azure/overview) voordat u doorgaat.

1. In de Azure-portal, gaat u naar **virtuele Machines**, gaat u naar uw Windows virtuele machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. 
2. Geef in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd bij het maken van de Windows-VM. 
3. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** met de virtuele machine, opent u PowerShell in de externe sessie.
4. Met behulp van Powershell Invoke-WebRequest, een aanvraag indienen op de lokale MSI-eindpunt op een toegangstoken ophalen voor Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
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
 
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Opslagaccountsleutels ophalen van Azure Resource Manager voor opslag-aanroepen  

Nu PowerShell gebruiken voor het aanroepen van Resource Manager met behulp van het toegangstoken dat we opgehaald in de vorige sectie om op te halen van de toegangssleutel voor opslag. Nadat we de toegangssleutel voor opslag hebben, kunnen we uploaden/downloaden-opslagbewerkingen aanroepen.

```powershell
$keysResponse = Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION-ID>/resourceGroups/<RESOURCE-GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE-ACCOUNT>/listKeys/?api-version=2016-12-01 -Method POST -Headers @{Authorization="Bearer $ARMToken"}
```
> [!NOTE] 
> De URL is hoofdlettergevoelig, dus zorg ervoor dat u precies hetzelfde hoofdlettergebruik eerder hebt, gebruikt toen u de naam van de resourcegroep, met inbegrip van de hoofdletters "G" in "resourceGroups." 

```powershell
$keysContent = $keysResponse.Content | ConvertFrom-Json
$key = $keysContent.keys[0].value
```

Vervolgens maken we een bestand met de naam 'test.txt'. Gebruik vervolgens de toegangssleutel voor opslag om te verifiëren met de `New-AzureStorageContent` cmdlet, upload het bestand naar de blob-container en vervolgens het bestand te downloaden.

```bash
echo "This is a test text file." > test.txt
```

Zorg ervoor dat de Azure Storage-cmdlets eerst installeren met behulp van `Install-Module Azure.Storage`. Upload de blob die u zojuist hebt gemaakt, met behulp van de `Set-AzureStorageBlobContent` PowerShell-cmdlet:

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
LastModified      : 9/13/2017 6:14:25 PM +00:00
SnapshotTime      :
ContinuationToken :
Context           : Microsoft.WindowsAzure.Commands.Storage.AzureStorageContext
Name              : testblob
```


## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).
- Zie voor meer informatie over deze dezelfde zelfstudie met behulp van een SAS-referenties voor opslag, [gebruiken van een Windows VM beheerde Service-identiteit voor toegang tot Azure Storage via SAS-referenties](msi-tutorial-windows-vm-access-storage-sas.md)
- Zie voor meer informatie over de SAS-functie van de Azure Storage-account:
  - [Shared access signatures (SAS) gebruiken](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Maken van een Service-SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud



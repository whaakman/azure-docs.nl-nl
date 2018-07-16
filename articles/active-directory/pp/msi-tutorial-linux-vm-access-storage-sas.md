---
title: Een Linux VM-MSI gebruiken voor toegang tot Azure Storage met behulp van een SAS-referenties
description: Een zelfstudie waarin wordt uitgelegd hoe u een Linux VM Managed Service Identity (MSI) gebruikt voor toegang tot Azure Storage, met behulp van een SAS-referenties in plaats van een toegangssleutel voor opslag.
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
ms.openlocfilehash: ccf6b41edbd085abeb358cf4fe624956b54946b9
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39056111"
---
# <a name="use-a-linux-vm-managed-service-identity-to-access-azure-storage-via-a-sas-credential"></a>Een Linux VM beheerde Service-identiteit gebruiken voor toegang tot Azure Storage via SAS-referenties

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie leert u hoe u Managed Service Identity (MSI) inschakelen voor een virtuele Linux-Machine en vervolgens het MSI-bestand gebruiken om op te halen van een Shared Access Signature (SAS) storage referentie. Met name een [referentie-Service-SAS](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json#types-of-shared-access-signatures). 

Een Service-SAS biedt de mogelijkheid beperkte toegang verlenen tot objecten in een storage-account voor een beperkte periode en een bepaalde service (in ons geval de blob-service), zonder dat een toegangssleutel. U kunt een SAS-referenties zoals gebruikelijk bij het maken van opslag-bewerkingen, bijvoorbeeld bij het gebruik van de Storage-SDK gebruiken. Voor deze zelfstudie ziet uploaden en downloaden van een blob met behulp van CLI van Azure Storage. U leert hoe u:


> [!div class="checklist"]
> * MSI-bestand op een Linux-Machine inschakelen 
> * Uw virtuele machine toegang verlenen tot een opslagaccount SAS in Resource Manager 
> * Een toegangstoken met behulp van de identiteit van de virtuele machine en deze gebruiken om op te halen van de SAS van Resource Manager 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Linux-VM. U kunt ook de MSI-bestand op een bestaande virtuele machine inschakelen.

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van Azure portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** u wilt dat de virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte voor de virtuele machine. Meer groottes Selecteer **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="enable-msi-on-your-vm"></a>MSI-bestand op de virtuele machine inschakelen

Een MSI-bestand voor de virtuele Machine kunt u tokens voor toegang van Azure AD ophalen zonder dat u om referenties in uw code. Op de achtergrond inschakelen van MSI doet twee dingen: de MSI-VM-extensie worden geïnstalleerd op de virtuele machine en zorgt ervoor dat de beheerde Service-identiteit voor de virtuele machine.  

1. Navigeer naar de resourcegroep van uw nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Klik onder de virtuele machine 'Instellingen' aan de linkerkant op **configuratie**.
3. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Controleer of u klikken op **opslaan** aan de configuratie op te slaan.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren welke uitbreidingen zijn op de virtuele machine, klikt u op **extensies**. Als het MSI-bestand is ingeschakeld, de **ManagedIdentityExtensionforLinux** wordt weergegeven in de lijst.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Create a storage account 

Als u dit niet al hebt, maakt u nu een storage-account.  Ook kunt u deze stap overslaan en uw VM MSI toegang verlenen tot de sleutels van een bestaand opslagaccount. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van Azure portal.
2. Klik op **opslag**, klikt u vervolgens **Opslagaccount**, en een nieuw deelvenster voor 'Storage-account maken' wordt weergegeven.
3. Voer een **naam** voor de storage-account, die u later gaat gebruiken.  
4. **Implementatiemodel** en **soort Account** moet worden ingesteld op 'Resource manager' en 'Algemeen', respectievelijk. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven tijdens het maken van uw virtuele machine in de vorige stap.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Maak een blobcontainer in de storage-account

We zullen later uploaden en een bestand downloaden naar het nieuwe opslagaccount. Omdat bestanden blob-opslag vereist, moet het maken van een blob-container waarin het bestand op te slaan.

1. Ga terug naar de zojuist gemaakte opslagaccount.
2. Klik op de **Containers** koppeling in het linkerdeelvenster, onder 'blobservice'.
3. Klik op **+ Container** boven aan de pagina en een 'nieuwe 'container deelvenster schuift.
4. Geef een naam op voor de container, selecteert u een toegangsniveau en klik op **OK**. De naam die u hebt opgegeven, wordt verderop in de zelfstudie worden gebruikt. 

    ![Opslagcontainer maken](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-a-storage-sas"></a>Verlenen van de virtuele machine MSI toegang voor het gebruik van een SAS-opslag 

Azure Storage biedt geen systeemeigen ondersteuning voor Azure AD-verificatie.  U kunt echter een MSI gebruiken voor het ophalen van een SAS-opslag van de Resource Manager en vervolgens de SAS gebruiken voor toegang tot opslag.  In deze stap maakt verlenen u uw VM MSI toegang aan uw storage-account-SAS.   

1. Ga terug naar de zojuist gemaakte opslagaccount...   
2. Klik op de **toegangsbeheer (IAM)** koppeling in het linkerdeelvenster.  
3. Klik op **+ toevoegen** boven op de pagina om een nieuwe roltoewijzing voor de virtuele machine
4. Stel **rol** naar 'Inzender voor Opslagaccounts", aan de rechterkant van de pagina. 
5. Stel in de volgende vervolgkeuzelijst **toegang toewijzen aan** de resource 'Virtuele Machine'.  
6. Controleer vervolgens of het juiste abonnement wordt weergegeven in **abonnement** vervolgkeuzelijst, stel **resourcegroep** op 'Alle resourcegroepen'.  
7. Ten slotte onder **Selecteer** kiest u uw virtuele Linux-Machine in de vervolgkeuzelijst en klik vervolgens op **opslaan**.  

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-storage/msi-storage-role-sas.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Een toegangstoken met behulp van de identiteit van de virtuele machine en gebruiken voor het aanroepen van Azure Resource Manager

Voor de rest van de zelfstudie werken we van de virtuele machine die we eerder hebben gemaakt.

Als u wilt deze stappen hebt voltooid, moet u een SSH-client. Als u Windows gebruikt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [over het gebruik van SSH-sleutels met Windows op Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en gebruiken van de openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. In de Azure-portal, gaat u naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. Kopieer de tekenreeks die moet verbinding maken met uw virtuele machine. 
2. Verbinding maken met uw virtuele machine via de SSH-client.  
3. Vervolgens wordt u gevraagd om in te voeren uw **wachtwoord** u hebt toegevoegd bij het maken van de **Linux-VM**. U moet vervolgens worden aangemeld.  
4. Gebruik CURL om een toegangstoken ophalen voor Azure Resource Manager.  

    De CURL-aanvraag en respons voor het toegangstoken lager is dan:
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true    
    ```
    
    > [!NOTE]
    > In de vorige aanvraag moet de waarde van de parameter 'resource' een exacte overeenkomst voor wat er door Azure AD wordt verwacht. Wanneer u de resource-ID van Azure Resource Manager gebruikt, moet u de afsluitende slash bevatten in de URI bevatten.
    > In het volgende antwoord, het access_token element als beknopt alternatief ingekort.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```

## <a name="get-a-sas-credential-from-azure-resource-manager-to-make-storage-calls"></a>SAS-referenties ophalen van Azure Resource Manager voor opslag-aanroepen

Nu gebruiken we CURL om aan te roepen van Resource Manager met behulp van het toegangstoken dat we opgehaald in de vorige sectie, een opslag-SAS-referentie maken. Zodra we de SAS-referenties hebt, kunt uploaden/downloaden-opslagbewerkingen genoemd.

Voor deze aanvraag gebruiken we de aanvraagparameters van de volgende HTTP-de SAS-referentie maken:

```JSON
{
    "canonicalizedResource":"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>",
    "signedResource":"c",              // The kind of resource accessible with the SAS, in this case a container (c).
    "signedPermission":"rcw",          // Permissions for this SAS, in this case (r)ead, (c)reate, and (w)rite.  Order is important.
    "signedProtocol":"https",          // Require the SAS be used on https protocol.
    "signedExpiry":"<EXPIRATION TIME>" // UTC expiration time for SAS in ISO 8601 format, for example 2017-09-22T00:06:00Z.
}
```

Deze parameters zijn opgenomen in de hoofdtekst van bericht van de aanvraag voor de SAS-referenties. Zie voor meer informatie over de parameters voor het maken van een SAS-referenties, het [naslaginformatie over de lijst met Service-SAS REST](/rest/api/storagerp/storageaccounts/listservicesas).

Gebruik de volgende CURL-aanvraag om de SAS-referenties. Vervang de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, `<CONTAINER NAME>`, en `<EXPIRATION TIME>` parameterwaarden door uw eigen waarden. Vervang de `<ACCESS TOKEN>` waarde met het toegangstoken dat u eerder hebt opgehaald:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listServiceSas/?api-version=2017-06-01 -X POST -d "{\"canonicalizedResource\":\"/blob/<STORAGE ACCOUNT NAME>/<CONTAINER NAME>\",\"signedResource\":\"c\",\"signedPermission\":\"rcw\",\"signedProtocol\":\"https\",\"signedExpiry\":\"<EXPIRATION TIME>\"}" -H "Authorization: Bearer <ACCESS TOKEN>"
```

> [!NOTE]
> De tekst in de voorgaande URL is hoofdlettergevoelig, dus zorg ervoor als u hoofdletters-kleine voor uw resourcegroepen gebruikt in overeenstemming met deze dienovereenkomstig aan. Daarnaast is het belangrijk te weten dat dit een POST-aanvraag niet een GET-aanvraag is.

Het antwoord CURL retourneert de SAS-referenties:  

```bash 
{"serviceSasToken":"sv=2015-04-05&sr=c&spr=https&st=2017-09-22T00%3A10%3A00Z&se=2017-09-22T02%3A00%3A00Z&sp=rcw&sig=QcVwljccgWcNMbe9roAJbD8J5oEkYoq%2F0cUPlgriBn0%3D"} 
```

Maak een voorbeeldbestand van de blob te uploaden naar uw blob storage-container. Op een Linux VM kunt u dit doen met de volgende opdracht. 

```bash
echo "This is a test file." > test.txt
```

Vervolgens verifiëren met de CLI `az storage` opdracht met behulp van de SAS-referenties in en upload het bestand naar de blob-container. Voor deze stap moet u [installeren de nieuwste Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) op de virtuele machine, als u dat nog niet gedaan hebt.

```azurecli-interactive
 az storage blob upload --container-name 
                        --file 
                        --name
                        --account-name 
                        --sas-token
```

Antwoord: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-21T03:58:56+00:00"
}
```

Bovendien kunt u het bestand met behulp van de Azure CLI en verificatie met de SAS-referenties downloaden. 

Aanvraag: 

```azurecli-interactive
az storage blob download --container-name
                         --file 
                         --name 
                         --account-name
                         --sas-token
```

Antwoord: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "testblob",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 16,
    "contentRange": "bytes 0-15/16",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "Aryr///Rb+D8JQ8IytleDA==",
      "contentType": "text/plain"
    },
    "copy": {
      "completionTime": null,
      "id": null,
      "progress": null,
      "source": null,
      "status": null,
      "statusDescription": null
    },
    "etag": "\"0x8D4F9929765C139\"",
    "lastModified": "2017-09-21T03:58:56+00:00",
    "lease": {
      "duration": null,
      "state": "available",
      "status": "unlocked"
    },
    "pageBlobSequenceNumber": null,
    "serverEncrypted": false
  },
  "snapshot": null
}
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).
- Voor meer informatie over hoe u deze zelfstudie doet met behulp van een opslagaccountsleutel, Zie [gebruiken van een Linux VM beheerde Service-identiteit voor toegang tot Azure Storage](msi-tutorial-linux-vm-access-storage.md)
- Zie voor meer informatie over de SAS-functie van de Azure Storage-account:
  - [Shared access signatures (SAS) gebruiken](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Maken van een Service-SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

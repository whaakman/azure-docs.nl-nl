---
title: Gebruik van een Linux-VM-MSI voor toegang tot Azure Storage
description: Een zelfstudie die u bij het proces helpt van het gebruik van een Linux VM beheerde Service identiteit (MSI) voor toegang tot Azure Storage.
services: active-directory
documentationcenter: 
author: bryanla
manager: mtillman
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: bryanla
ms.openlocfilehash: 2b264f8e2227e410caba8370d58c824d469198e4
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="use-a-linux-vm-managed-service-identity-to-access-azure-storage-via-access-key"></a>Gebruik van een Linux VM beheerde Service-identiteit voor toegang tot Azure Storage via toegangssleutel

[!INCLUDE[preview-notice](../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie ziet u het inschakelen van beheerde Service identiteit (MSI) voor een virtuele Linux-Machine en vervolgens die identiteit gebruiken voor het ophalen van de toegangssleutels voor opslag-account. U kunt een toegangssleutel voor opslag gewoon bij het uitvoeren van opslagbewerkingen, bijvoorbeeld bij het gebruik van de opslag-SDK gebruiken. Voor deze zelfstudie we uploaden en downloaden van blobs met Azure CLI. U leert hoe:

> [!div class="checklist"]
> * MSI op een virtuele Linux-Machine inschakelen 
> * Uw VM toegang verlenen tot toegang toegangscodes voor opslag in Resource Manager 
> * Ophalen van een toegangstoken die met de identiteit van de VM, en deze gebruiken voor het ophalen van de toegangssleutels voor opslag van Resource Manager  

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).


## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Linux VM. U kunt ook MSI op een bestaande virtuele machine inschakelen.

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

    ![De installatiekopie van de alternatieve tekst](media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** u wilt dat de virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte van de virtuele machine. Selecteer om te zien meer grootten, **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen

De MSI van een virtuele Machine kunt u toegangstokens ophalen uit Azure AD zonder dat u referenties in uw code te plaatsen. Achter de MSI inschakelen biedt twee dingen: het installeren van de MSI-VM-extensie op uw virtuele machine en zorgt ervoor dat de Service-identiteit beheerd voor de virtuele machine.  

1. Navigeer naar de resourcegroep van de nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Klik in de virtuele machine 'Instellingen' aan de linkerkant op **configuratie**.
3. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.

    ![De installatiekopie van de alternatieve tekst](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren welke uitbreidingen zijn op de virtuele machine, klikt u op **extensies**. Als MSI is ingeschakeld, de **ManagedIdentityExtensionforLinux** wordt weergegeven in de lijst.

    ![De installatiekopie van de alternatieve tekst](media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="create-a-storage-account"></a>Een opslagaccount maken 

Als u dit niet al hebt, maakt u nu een opslagaccount.  Ook kunt u deze stap overslaan en uw VM MSI toegang verlenen tot de sleutels van een bestaand opslagaccount. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Klik op **opslag**, klikt u vervolgens **Opslagaccount**, en een nieuw deelvenster voor 'Storage-account maken' wordt weergegeven.
3. Voer een **naam** voor de storage-account, gaat u later gebruiken.  
4. **Implementatiemodel** en **Account kind** moet worden ingesteld op 'Resource manager' en 'Algemene', respectievelijk. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven toen u uw virtuele machine in de vorige stap hebt gemaakt.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Een blob-container in het opslagaccount maken

Later wordt uploaden en downloaden van een bestand met de nieuwe opslagaccount. Omdat bestanden blob-opslag is vereist, moeten we maken van een blob-container waarin het bestand wilt opslaan.

1. Ga terug naar uw nieuwe opslagaccount.
2. Klik op de **Containers** koppeling aan de linkerkant onder 'Blob-service'.
3. Klik op **+ Container** boven aan de pagina en een 'nieuwe container' Configuratiescherm dia's uit.
4. Geef een naam op voor de container, selecteer een toegangsniveau en klik op **OK**. De opgegeven naam wordt verderop in de zelfstudie gebruikt. 

    ![Opslagcontainer maken](media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

## <a name="grant-your-vms-msi-access-to-use-storage-account-access-keys"></a>Uw VM MSI-toegang voor het gebruik van sleutels van opslagaccount toegang verlenen

Azure Storage biedt geen systeemeigen ondersteuning voor Azure AD-verificatie.  U kunt echter een MSI-bestand gebruiken voor het ophalen van opslagtoegangssleutels-account van de Resource Manager en vervolgens een sleutel wordt gebruikt voor toegang tot opslag.  In deze stap maakt u uw VM MSI toegang verlenen tot de sleutels naar uw opslagaccount.   

1. Ga terug naar uw nieuwe opslagaccount.
2. Klik op de **toegangsbeheer (IAM)** koppeling in het linkerdeelvenster.  
3. Klik op **+ toevoegen** boven op de pagina om een nieuwe roltoewijzing voor de virtuele machine
4. Stel **rol** naar 'Storage Account sleutel Operator Service rol', aan de rechterkant van de pagina. 
5. Stel in de volgende dropdown **toewijzen van toegang tot** de resource 'Virtuele Machine'.  
6. Controleer vervolgens het juiste abonnement wordt vermeld in **abonnement** vervolgkeuzelijst Stel **resourcegroep** aan 'Alle resourcegroepen'.  
7. Ten slotte onder **Selecteer** Kies uw virtuele Linux-Machine in de vervolgkeuzelijst en klik vervolgens op **opslaan**. 

    ![De installatiekopie van de alternatieve tekst](media/msi-tutorial-linux-vm-access-storage/msi-storage-role.png)

## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-call-azure-resource-manager"></a>Ophalen van een toegangstoken die met de identiteit van de VM en deze gebruiken om aan te roepen Azure Resource Manager

Voor het restant van de zelfstudie werken we van de virtuele machine die we eerder hebben gemaakt.

Deze stappen uit te voeren, moet u een SSH-client. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/install_guide). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](../virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](../virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. Kopieer de tekenreeks verbinding maken met uw virtuele machine. 
2. Verbinding maken met uw virtuele machine met behulp van uw SSH-client.  
3. Vervolgens wordt u gevraagd om in te voeren uw **wachtwoord** u toegevoegd bij het maken van de **Linux VM**. U moet vervolgens worden aangemeld.  
4. Gebruik CURL om een toegangstoken ophalen voor Azure Resource Manager.  

    De CURL aanvraag en antwoord voor het toegangstoken lager is dan:
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://management.azure.com/" -H Metadata:true    
    ```
    
    > [!NOTE]
    > De waarde van de resourceparameter '' moet een exacte overeenkomst voor wat er wordt verwacht door Azure AD, in de vorige aanvraag. Wanneer u de resource-ID van Azure Resource Manager gebruikt, moet u de afsluitende slash op de URI opnemen.
    > In het volgende antwoord, het element access_token zoals ingekort als beknopt alternatief bevat.
    
    ```bash
    {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://management.azure.com",
    "token_type":"Bearer"} 
     ```
    
## <a name="get-storage-account-access-keys-from-azure-resource-manager-to-make-storage-calls"></a>Toegangssleutels voor opslag-account ophalen van Azure Resource Manager storage aanroepen  

CURL nu gebruiken om aan te roepen Resource Manager met behulp van het toegangstoken dat we opgehaald in de vorige sectie voor het ophalen van de toegangssleutel voor opslag. Wanneer we de toegangssleutel voor opslag hebt, noemen we opslagbewerkingen uploaden/downloaden. Zorg ervoor dat u de `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, en `<STORAGE ACCOUNT NAME>` parameterwaarden met uw eigen waarden. Vervang de `<ACCESS TOKEN>` waarde met het toegangstoken dat u eerder hebt opgehaald:

```bash 
curl https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/listKeys?api-version=2016-12-01 –-request POST -d "" -H "Authorization: Bearer <ACCESS TOKEN>" 
```

> [!NOTE]
> De tekst in de voorgaande URL is hoofdlettergevoelig, dus zorg ervoor dat als u upper-kleine voor uw resourcegroepen gebruikt in overeenstemming met het dienovereenkomstig. Bovendien is het belangrijk te weten dat dit een POST-aanvraag niet op een GET-aanvraag is en zorg ervoor dat u een waarde voor het vastleggen van een lengtelimiet met -d die NULL kunnen worden doorgegeven.  

Het antwoord CURL hebt u de lijst met sleutels:  

```bash 
{"keys":[{"keyName":"key1","permissions":"Full","value":"iqDPNt..."},{"keyName":"key2","permissions":"Full","value":"U+uI0B..."}]} 
```
Maak een voorbeeldbestand blob uploaden naar blob storage-container. Op een Linux-VM kunt u dit doen met de volgende opdracht. 

```bash
echo "This is a test file." > test.txt
```

Vervolgens verifiëren met de CLI `az storage` opdracht met behulp van de toegangssleutel voor opslag en upload het bestand naar de blob-container. Voor deze stap moet u [installeren de nieuwste Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli) op de virtuele machine als u dat nog niet gedaan hebt.
 

```azurecli-interactive
az storage blob upload -c <CONTAINER NAME> -n test.txt -f test.txt --account-name <STORAGE ACCOUNT NAME> --account-key <STORAGE ACCOUNT KEY>
```

Antwoord: 

```JSON
Finished[#############################################################]  100.0000%
{
  "etag": "\"0x8D4F9929765C139\"",
  "lastModified": "2017-09-12T03:58:56+00:00"
}
```

Bovendien kunt u downloaden van het bestand met behulp van de Azure CLI en verificatie met de toegangssleutel voor opslag. 

Aanvraag: 

```azurecli-interactive
az storage blob download -c <CONTAINER NAME> -n test.txt -f test-download.txt --account-name <STORAGE ACCOUNT NAME> --account-key <STORAGE ACCOUNT KEY>
```

Antwoord: 

```JSON
{
  "content": null,
  "metadata": {},
  "name": "test.txt",
  "properties": {
    "appendBlobCommittedBlockCount": null,
    "blobType": "BlockBlob",
    "contentLength": 21,
    "contentRange": "bytes 0-20/21",
    "contentSettings": {
      "cacheControl": null,
      "contentDisposition": null,
      "contentEncoding": null,
      "contentLanguage": null,
      "contentMd5": "LSghAvpnElYyfUdn7CO8aw==",
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
    "etag": "\"0x8D5067F30D0C283\"",
    "lastModified": "2017-09-28T14:42:49+00:00",
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

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](../active-directory/msi-overview.md).
- Zie voor meer informatie over hoe u doet dit dezelfde zelfstudie gebruik van een SAS-referentie van opslag, [gebruiken van een Linux VM beheerde Service-identiteit voor toegang tot Azure Storage via een SAS-referentie](msi-tutorial-linux-vm-access-storage-sas.md)
- Zie voor meer informatie over de SAS-functie van Azure Storage-account:
  - [Met behulp van handtekeningen voor gedeelde toegang (SAS)](/azure/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Maken van een Service-SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.

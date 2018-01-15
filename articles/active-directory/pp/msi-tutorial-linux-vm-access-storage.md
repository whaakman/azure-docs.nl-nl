---
title: Een gebruiker toegewezen MSI op een Linux-VM te gebruiken voor toegang tot Azure Storage
description: Een zelfstudie die u bij het proces helpt van het gebruik van een gebruiker toegewezen beheerde Service identiteit (MSI) op een Linux-VM voor toegang tot Azure Storage.
services: active-directory
documentationcenter: 
author: bryanLa
manager: mtillman
editor: arluca
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 91fe06825d1db586b715617241b0ca39115414c0
ms.sourcegitcommit: 48fce90a4ec357d2fb89183141610789003993d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/12/2018
---
# <a name="use-a-user-assigned-managed-service-identity-msi-on-a-linux-vm-to-access-azure-storage"></a>Een gebruiker toegewezen beheerde Service identiteit (MSI) op een Linux-VM te gebruiken voor toegang tot Azure Storage

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe maken en gebruiken van een gebruiker toegewezen beheerde Service identiteit (MSI) van een virtuele Linux-Machine en vervolgens gebruiken voor toegang tot Azure Storage. Procedures voor:

> [!div class="checklist"]
> * Maak een beheerde Service-identiteit (MSI) toegewezen gebruiker
> * De gebruiker toegewezen MSI toewijzen aan een virtuele Linux-Machine
> * De MSI toegang verlenen tot een exemplaar van Azure Storage
> * Ophalen van een toegangstoken met behulp van de identiteit van de gebruiker toegewezen MSI, en deze gebruiken voor toegang tot Azure Storage

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

Als u wilt uitvoeren in de CLI scriptvoorbeelden in deze zelfstudie, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit de Azure-portal of via de knop 'Deze probeer' zich in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of hoger) als u liever een lokale CLI-console te gebruiken.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-machine in een nieuwe resourcegroep maken

U maakt eerst een nieuwe Linux VM. U kunt ook een MSI op een bestaande virtuele machine inschakelen als u liever.

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** u wilt dat de virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte van de virtuele machine. Selecteer om te zien meer grootten, **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="create-a-user-assigned-msi"></a>Maken van een MSI gebruiker toegewezen

1. Als u gebruikmaakt van de console CLI (in plaats van een Azure-Cloud-Shell-sessie), eerst aanmelden bij Azure. Gebruik een account dat is gekoppeld aan het Azure-abonnement waarmee u wilt maken van de nieuwe MSI:

    ```azurecli
    az login
    ```

2. Maak een gebruiker toegewezen MSI met [az identiteit maken](/cli/azure/identity#az_identity_create). De `-g` parameter geeft u de resourcegroep waar het MSI-bestand is gemaakt, en de `-n` parameter geeft u de naam ervan. Zorg ervoor dat u de `<RESOURCE GROUP>` en `<MSI NAME>` parameterwaarden met uw eigen waarden:

    ```azurecli-interactive
    az identity create -g <RESOURCE GROUP> -n <MSI NAME>
    ```

    Het antwoord bevat de details voor de gebruiker toegewezen MSI gemaakte, vergelijkbaar met het volgende voorbeeld. Opmerking de `id` waarde voor uw MSI zoals deze wordt gebruikt in de volgende stap:

    ```json
    {
    "clientId": "73444643-8088-4d70-9532-c3a0fdc190fz",
    "clientSecretUrl": "https://control-westcentralus.identity.azure.net/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>/credentials?tid=5678&oid=9012&aid=123444643-8088-4d70-9532-c3a0fdc190fz",
    "id": "/subscriptions/<SUBSCRIPTON ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>",
    "location": "westcentralus",
    "name": "<MSI NAME>",
    "principalId": "9012",
    "resourceGroup": "<RESOURCE GROUP>",
    "tags": {},
    "tenantId": "733a8f0e-ec41-4e69-8ad8-971fc4b533bl",
    "type": "Microsoft.ManagedIdentity/userAssignedIdentities"
    }
    ```

## <a name="assign-your-user-assigned-msi-to-your-linux-vm"></a>De gebruiker toegewezen MSI toewijzen aan uw Linux-VM

In tegenstelling tot een MSI systeem toegewezen kan een gebruiker toegewezen MSI worden gebruikt door clients in meerdere Azure-resources. Voor deze zelfstudie maakt toewijzen u deze aan een enkele virtuele machine. U kunt deze ook toewijzen aan meer dan één virtuele machine.

De gebruiker toegewezen MSI toewijzen aan uw Linux-VM met [az vm toewijzen-identity](/cli/azure/vm#az_vm_assign_identity). Zorg ervoor dat u de `<RESOURCE GROUP>` en `<VM NAME>` parameterwaarden met uw eigen waarden. Gebruik de `id` eigenschap geretourneerd in de vorige stap voor de `--identities` parameterwaarde:

```azurecli-interactive
az vm assign-identity -g <RESOURCE GROUP> -n <VM NAME> --identities "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.ManagedIdentity/userAssignedIdentities/<MSI NAME>"
```

## <a name="create-a-storage-account"></a>Een opslagaccount maken 

Als u dit niet al hebt, moet u nu een opslagaccount maken. U kunt ook deze stap overslaan en een bestaand opslagaccount gebruiken als u liever. 

1. Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Klik op **opslag**, klikt u vervolgens **Opslagaccount**, en een nieuw deelvenster voor 'Storage-account maken' wordt weergegeven.
3. Voer een **naam** voor het opslagaccount dat u later gebruiken.  
4. **Implementatiemodel** en **Account kind** moet worden ingesteld op 'Resource manager' en 'Algemene', respectievelijk. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven toen u uw virtuele machine in de vorige stap hebt gemaakt.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-in-the-storage-account"></a>Een blob-container in het opslagaccount maken

Omdat bestanden blob-opslag is vereist, moet u een blobcontainer in om op te slaan van het bestand te maken. Vervolgens uploaden en downloaden van een bestand naar de blob-container in het nieuwe opslagaccount.

1. Ga terug naar uw nieuwe opslagaccount.
2. Klik op de **Containers** koppeling aan de linkerkant onder 'Blob-service'.
3. Klik op **+ Container** boven aan de pagina en een 'nieuwe container' Configuratiescherm dia's uit.
4. Geef een naam op voor de container, selecteer een toegangsniveau en klik op **OK**. De opgegeven naam wordt ook gebruikt verderop in de zelfstudie. 

    ![Opslagcontainer maken](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Een bestand te uploaden naar de zojuist gemaakte container door te klikken op de containernaam van de, klikt u vervolgens **uploaden**, vervolgens selecteert u een bestand en klik vervolgens op **uploaden**.

    ![Tekstbestand uploaden](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-user-assigned-msi-access-to-an-azure-storage-container"></a>De gebruiker toegewezen MSI toegang verlenen tot een Azure Storage-container

Met behulp van een MSI-bestand, krijgt uw code toegangstokens voor verificatie naar bronnen die ondersteuning bieden voor Azure AD-verificatie. In deze zelfstudie gebruikt u Azure Storage.

Eerst u de MSI identiteit toegang verlenen tot een Azure Storage-container. In dit geval gebruikt u de container die eerder hebt gemaakt. Werk de waarden voor `<MSI PRINCIPALID>`, `<SUBSCRIPTION ID>`, `<RESOURCE GROUP>`, `<STORAGE ACCOUNT NAME>`, en `<CONTAINER NAME>` afhankelijk van wat geschikt is voor uw omgeving. Vervang `<CLIENT ID>` met de `clientId` eigenschap die is geretourneerd door de `az identity create` opdracht in [maken van een gebruiker toegewezen MSI](#create-a-user-assigned-msi):

```azurecli-interactive
az role assignment create --assignee <MSI PRINCIPALID> --role ‘Reader’ --scope "/subscriptions/<SUBSCRIPTION ID>/resourcegroups/<RESOURCE GROUP>/providers/Microsoft.Storage/storageAccounts/<STORAGE ACCOUNT NAME>/blobServices/default/containers/<CONTAINER NAME>"
```

Het antwoord bevat de details voor de roltoewijzing gemaakt:

```
{
  "id": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.Authorization/roleAssignments/b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "name": "b402bd74-157f-425c-bf7d-zed3a3a581ll",
  "properties": {
    "principalId": "f5fdfdc1-ed84-4d48-8551-999fb9dedfbl",
    "roleDefinitionId": "/subscriptions/<SUBSCRIPTION ID>/providers/Microsoft.Authorization/roleDefinitions/acdd72a7-3385-48ef-bd42-f606fba81ae7",
    "scope": "/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>/providers/Microsoft.storage/storageAccounts/<STORAGE ACCOUNT NAME>/blogServices/default/<CONTAINER NAME>"
  },
  "resourceGroup": "<RESOURCE GROUP>",
  "type": "Microsoft.Authorization/roleAssignments"
}
```

## <a name="get-an-access-token-using-the-user-assigned-msis-identity-and-use-it-to-call-azure-storage"></a>Ophalen van een toegangstoken met behulp van de gebruiker toegewezen MSI-identiteit en het aanroepen van Azure Storage

Voor de rest van de zelfstudie moet u te laten werken vanuit de virtuele machine die u eerder hebt gemaakt.

Deze stappen uit te voeren, moet u een SSH-client. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de Azure-portal naar **virtuele Machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens vanuit de **overzicht** pagina op **Connect** aan de bovenkant. Kopieer de tekenreeks verbinding maken met uw virtuele machine.
2. **Verbinding maken met** naar de virtuele machine met de SSH-client van uw keuze. 
3. Het terminalvenster met CURL, zorg er in een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Storage.

   De aanvraag CURL te verkrijgen van een toegangstoken wordt weergegeven in het volgende voorbeeld. Zorg ervoor dat u `<CLIENT ID>` met de `clientId` eigenschap die is geretourneerd door de `az identity create` opdracht in [maken van een gebruiker toegewezen MSI](#create-a-user-assigned-msi):

   ```bash
   curl -H Metadata:true "http://localhost:50342/oauth2/token?resource=https%3A%2F%2Fstorage.azure.com/&client_id=<CLIENT ID>"
   ```

   > [!NOTE]
   > In de vorige aanvraag, de waarde van de `resource` parameter moet een exacte overeenkomst voor door Azure AD wordt verwacht. Wanneer u de resource-ID van Azure Storage, kunt u de afsluitende slash op de URI moet opnemen.
   > In het volgende antwoord, het element access_token zoals ingekort als beknopt alternatief bevat.

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
   "refresh_token":"",
   "expires_in":"3599",
   "expires_on":"1504130527",
   "not_before":"1504126627",
   "resource":"https://storage.azure.com",
   "token_type":"Bearer"}
   ```

4. Het toegangstoken nu gebruiken voor toegang tot Azure Storage, bijvoorbeeld lezen van de inhoud van het voorbeeldbestand die u eerder hebt geüpload naar de container. Vervang de waarden van `<STORAGE ACCOUNT>`, `<CONTAINER NAME>`, en `<FILE NAME>` met de waarden die u eerder hebt opgegeven en `<ACCESS TOKEN>` aan het token geretourneerd in de vorige stap.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME>?api-version=2017-11-09 -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Het antwoord bevat de inhoud van het bestand:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Zie voor meer informatie over hoe u doet dit dezelfde zelfstudie gebruik van een SAS-referentie van opslag, [gebruiken van een Linux VM beheerde Service-identiteit voor toegang tot Azure Storage via een SAS-referentie](msi-tutorial-linux-vm-access-storage-sas.md)
- Zie voor meer informatie over de SAS-functie van Azure Storage-account:
  - [Met behulp van handtekeningen voor gedeelde toegang (SAS)](~/articles/storage/common/storage-dotnet-shared-access-signature-part-1.md)
  - [Maken van een Service-SAS](/rest/api/storageservices/Constructing-a-Service-SAS.md)

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.






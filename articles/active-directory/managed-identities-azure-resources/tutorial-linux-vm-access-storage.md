---
title: Een door het Linux-VM-systeem toegewezen beheerde identiteit gebruiken voor toegang tot Azure Storage
description: Een zelfstudie die u helpt bij het gebruiken van een door het Linux-VM-systeem toegewezen beheerde identiteit voor toegang tot Azure Storage.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.service: active-directory
ms.subservice: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/09/2018
ms.author: markvi
ms.collection: M365-identity-device-management
ms.openlocfilehash: bb7de72a435faf100d6992815ef8d5ec00cb3581
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "66236163"
---
# <a name="tutorial-use-a-linux-vm-system-assigned-managed-identity-to-access-azure-storage"></a>Zelfstudie: Een door het Linux-VM-systeem toegewezen beheerde identiteit gebruiken voor toegang tot Azure Storage 

[!INCLUDE [preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u toegang krijgt tot Azure Storage met een door het systeem toegewezen beheerde identiteit voor een virtuele Linux-machine (VM). In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Create a storage account
> * Een blobcontainer in een opslagaccount maken
> * De beheerde identiteit van de Linux-VM toegang geven tot een Azure Storage-container
> * Een toegangstoken ophalen en daarmee Azure Storage aanroepen

> [!NOTE]
> Azure Active Directory-verificatie voor Azure Storage is beschikbaar als openbare preview.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

Als u de CLI-scriptvoorbeelden in deze zelfstudie wilt uitvoeren, hebt u twee opties:

- Gebruik [Azure Cloud Shell](~/articles/cloud-shell/overview.md) vanuit Azure Portal of via de knop **Uitproberen** in de rechterbovenhoek van elk codeblok.
- [Installeer de nieuwste versie van CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (2.0.23 of later) als u liever een lokale CLI-console gebruikt.

## <a name="create-a-storage-account"></a>Create a storage account 

In deze sectie maakt u een opslagaccount. 

1. Klik op de knop **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Storage** en vervolgens **Opslagaccount - blob, bestand, tabel, wachtrij**.
3. Geef onder **Naam** een naam voor het opslagaccount op.  
4. **Implementatiemodel** en **Soort account** moeten respectievelijk worden ingesteld op **Resource Manager** en **Storage (algemeen gebruik v1)** . 
5. Zorg ervoor dat de waarden van **Abonnement** en **Resourcegroep** overeenkomen met de waarden die u hebt opgegeven bij het maken van de virtuele machine in de vorige stap.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](./media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Een blobcontainer maken en een bestand naar het opslagaccount uploaden

Voor bestanden is blobopslag nodig, dus moeten we een blobcontainer maken waarin het bestand kan worden opgeslagen. Vervolgens uploadt u een bestand naar de blobcontainer in het nieuwe opslagaccount.

1. Navigeer terug naar het zojuist gemaakte opslagaccount.
2. Klik onder **Blob Service** op **Containers**.
3. Klik op **+ Container** boven aan de pagina.
4. Voer onder **Nieuwe container** een naam in voor de container en laat **Niveau openbare toegang** ingesteld staan op de standaardwaarde.

    ![Opslagcontainer maken](./media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Maak met behulp van een editor naar keuze een bestand met de naam *hello_world.txt* op uw lokale computer.  Open het bestand en voeg de tekst ‘Hello world! :)’ toe (zonder de aanhalingstekens) en sla het bestand vervolgens op. 

6. Upload het bestand naar de zojuist gemaakte container door op de naam van de container te klikken, en vervolgens op **Uploaden**
7. Klik in het deelvenster **Blob uploaden** onder **Bestanden** op het mappictogram en blader naar het bestand **hello_world.txt** op uw lokale computer, selecteer het bestand en klik vervolgens op **Uploaden**.

    ![Tekstbestand uploaden](./media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Uw virtuele machine toegang verlenen tot een Azure Storage-container 

U kunt de beheerde identiteit van de virtuele machine gebruiken om de gegevens in de Azure Storage-blob op te halen.   

1. Navigeer terug naar het zojuist gemaakte opslagaccount.  
2. Klik op de koppeling **Toegangsbeheer (IAM)** in het linkerpaneel.  
3. Klik op **+ Roltoewijzing toevoegen** boven aan de pagina om een nieuwe roltoewijzing voor de VM toe te voegen.
4. Onder **rol**, in de vervolgkeuzelijst, selecteer **gegevenslezer voor Opslagblob**. 
5. In de volgende vervolgkeuzelijst, onder **Toegang toewijzen aan**, kiest u **Virtuele machine**.  
6. Controleer vervolgens of het juiste abonnement wordt weergegeven in de vervolgkeuzelijst **Abonnement**, en stel **Resourcegroep** in op **Alle resourcegroepen**.  
7. Kies onder **Selecteren** uw virtuele machine en klik vervolgens op **Opslaan**.

    ![Machtigingen toewijzen](./media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Een toegangstoken ophalen en daarmee Azure Storage aanroepen

Azure Storage biedt systeemeigen ondersteuning voor Azure AD-verificatie, zodat toegangstokens die zijn verkregen met behulp van een beheerde identiteit direct kunnen worden geaccepteerd. Dit maakt deel uit van de integratie van Azure Storage met Azure AD en wijkt af van het opgeven van referenties in de verbindingsreeks.

Om de volgende stappen te voltooien, moet u werken met de eerder gemaakte VM, en u hebt een SSH-client nodig om daar verbinding mee te maken. Als u Windows gebruikt, kunt u de SSH-client in het [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about) gebruiken. Zie [De sleutels van uw SSH-client gebruiken onder Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md) of [Een sleutelpaar met een openbare SSH-sleutel en een privé-sleutel maken en gebruiken voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md) als u hulp nodig hebt bij het configureren van de sleutels van uw SSH-client.

1. In Azure Portal navigeert u naar **Virtuele machines**, gaat u naar uw virtuele Linux-machine, klikt u vervolgens op de pagina **Overzicht** op **Verbinden**. Kopieer de verbindingsreeks voor uw virtuele machine.
2. **Maak verbinding** met de virtuele machine met de SSH-client van uw keuze. 
3. Dien in het terminalvenster met behulp van CURL een aanvraag in op het lokale beheerde identiteit-eindpunt om een toegangstoken voor Azure Storage op te halen.
    
    ```bash
    curl 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fstorage.azure.com%2F' -H Metadata:true
    ```
4. Gebruik nu het toegangstoken voor toegang tot Azure Storage, bijvoorbeeld om de inhoud te lezen van het voorbeeldbestand dat u eerder naar de container hebt geüpload. Vervang de waarden van `<STORAGE ACCOUNT>`, `<CONTAINER NAME>` en `<FILE NAME>` door de waarden die u eerder hebt opgegeven en `<ACCESS TOKEN>` door het token dat in de vorige stap is geretourneerd.

   ```bash
   curl https://<STORAGE ACCOUNT>.blob.core.windows.net/<CONTAINER NAME>/<FILE NAME> -H "x-ms-version: 2017-11-09" -H "Authorization: Bearer <ACCESS TOKEN>"
   ```

   Het antwoord bevat de inhoud van het bestand:

   ```bash
   Hello world! :)
   ```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd een door het Linux-VM-systeem toegewezen beheerde identiteit in te schakelen om toegang tot Azure Storage te krijgen.  Zie voor meer informatie over Azure Storage:

> [!div class="nextstepaction"]
> [Azure Storage](/azure/storage/common/storage-introduction)

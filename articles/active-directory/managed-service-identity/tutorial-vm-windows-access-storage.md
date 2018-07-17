---
title: Toegang krijgen tot Azure Storage met een beheerde entiteit voor Windows-VM
description: Een zelfstudie die u helpt bij het doorlopen van het proces voor het krijgen van toegang tot Azure Storage met een Managed Service Identity (MSI) voor Windows-VM.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: daveba
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 04/12/2018
ms.author: daveba
ms.openlocfilehash: a9b0ddbd8d0a348d00e57526e6d248e59a8a1e79
ms.sourcegitcommit: d551ddf8d6c0fd3a884c9852bc4443c1a1485899
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/07/2018
ms.locfileid: "37903952"
---
# <a name="tutorial-use-a-windows-vm-managed-identity-to-access-azure-storage"></a>Zelfstudie: Toegang krijgen tot Azure Storage met een beheerde entiteit voor Windows-VM

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u een beheerde entiteit kunt inschakelen voor een virtuele Windows-machine, en daarmee toegang kunt krijgen tot Azure Storage.  In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een virtuele Windows-machine maken in een nieuwe resourcegroep 
> * Beheerde identiteit op een virtuele Windows-machine (VM) inschakelen
> * Een blobcontainer in een opslagaccount maken
> * Beheerde entiteit van de virtuele Windows-machine toegang verlenen tot een opslagaccount 
> * Een toegangstoken ophalen en daarmee Azure Storage aanroepen 

> [!NOTE]
> Azure Active Directory-verificatie voor Azure Storage is beschikbaar als openbare preview.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine maken in een nieuwe resourcegroep

In deze sectie maakt u een virtuele Windows-machine waaraan later een beheerde identiteit wordt verleend.

1.  Klik op de knop **+/Nieuwe service maken** in de linkerbovenhoek van Azure Portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De referenties (combinatie van **Gebruikersnaam** en **Wachtwoord**) die u hier opgeeft, zijn de referenties waarmee u zich aanmeldt bij de virtuele machine.
4.  Kies het juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Om een nieuwe **resourcegroep** te selecteren waarin de virtuele machine moet worden gemaakt, kiest u **Nieuwe maken**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

    ![Alt-tekst voor afbeelding](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-identity-on-your-vm"></a>Beheerde identiteit op de virtuele machine inschakelen

Met een beheerde identiteit op de virtuele machine kunt u toegangstokens uit Azure AD ophalen zonder referenties in uw code te hoeven opnemen. Er gebeuren twee dingen als u Beheerde identiteit op een virtuele machine via Azure Portal inschakelt: de virtuele machine wordt bij Azure AD geregistreerd om een beheerde identiteit te maken, en de identiteit wordt geconfigureerd op de virtuele machine. 

1. Navigeer naar de resourcegroep van de nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Klik onder de categorie **Instellingen** op **Configuratie**.
3. Selecteer **Ja** om de beheerde entiteit in te schakelen.
4. Klik op **Opslaan** om de configuratie toe te passen. 

## <a name="create-a-storage-account"></a>Create a storage account 

In deze sectie maakt u een opslagaccount. 

1. Klik op de knop **+ Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Storage** en vervolgens **Opslagaccount - blob, bestand, tabel, wachtrij**.
3. Geef onder **Naam** een naam voor het opslagaccount op.  
4. **Implementatiemodel** en **Soort account** moeten respectievelijk worden ingesteld op **Resource Manager** en **Storage (algemeen gebruik v1)**. 
5. Zorg ervoor dat de waarden van **Abonnement** en **Resourcegroep** overeenkomen met de waarden die u hebt opgegeven bij het maken van de virtuele machine in de vorige stap.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Een blobcontainer maken en een bestand naar het opslagaccount uploaden

Voor bestanden is blobopslag nodig, dus moeten we een blobcontainer maken waarin het bestand kan worden opgeslagen. Vervolgens uploadt u een bestand naar de blobcontainer in het nieuwe opslagaccount.

1. Navigeer terug naar het zojuist gemaakte opslagaccount.
2. Klik onder **Blob Service** op **Containers**.
3. Klik op **+ Container** boven aan de pagina.
4. Voer onder **Nieuwe container** een naam in voor de container en laat **Niveau openbare toegang** ingesteld staan op de standaardwaarde.

    ![Opslagcontainer maken](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Maak met behulp van een editor naar keuze een bestand met de naam *hello_world.txt* op uw lokale computer.  Open het bestand en voeg de tekst (zonder de aanhalingstekens) 'Hello world! :)' toe en sla het bestand vervolgens op. 
6. Upload het bestand naar de zojuist gemaakte container door op de naam van de container te klikken, en vervolgens op **Uploaden**
7. Klik in het deelvenster **Blob uploaden** onder **Bestanden** op het mappictogram en blader naar het bestand **hello_world.txt** op uw lokale computer, selecteer het bestand en klik vervolgens op **Uploaden**.
    ![Tekstbestand uploaden](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Uw virtuele machine toegang verlenen tot een Azure Storage-container 

U kunt de beheerde identiteit van de virtuele machine gebruiken om de gegevens in de Azure storage-blob op te halen.   

1. Navigeer terug naar het zojuist gemaakte opslagaccount.  
2. Klik op de koppeling **Toegangsbeheer (IAM)** in het linkerpaneel.  
3. Klik op **+ Toevoegen** boven aan de pagina om een nieuwe roltoewijzing voor de virtuele machine toe te voegen.
4. In de vervolgkeuzelijst onder **Rol** selecteert u **Gegevenslezer voor opslagblob (preview-versie)**. 
5. In de volgende vervolgkeuzelijst, onder **Toegang toewijzen aan**, kiest u **Virtuele machine**.  
6. Controleer vervolgens of het juiste abonnement wordt weergegeven in de vervolgkeuzelijst **Abonnement**, en stel **Resourcegroep** in op **Alle resourcegroepen**.  
7. Kies onder **Selecteren** uw virtuele machine en klik vervolgens op **Opslaan**. 

    ![Machtigingen toewijzen](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Een toegangstoken ophalen en daarmee Azure Storage aanroepen 

Azure Storage biedt systeemeigen ondersteuning voor Azure AD-verificatie, zodat toegangstokens die zijn verkregen met behulp van een beheerde identiteit direct kunnen worden geaccepteerd. Dit maakt deel uit van de integratie van Azure Storage met Azure AD en wijkt af van het opgeven van referenties in de verbindingsreeks.

Hier volgt een voorbeeld van .NET-code voor het openen van een verbinding met Azure Storage met behulp van een toegangstoken en het lezen van de inhoud van het bestand dat u eerder hebt gemaakt. Deze code moet worden uitgevoerd op de virtuele machine om toegang te krijgen tot het eindpunt van de beheerde identiteit van de virtuele machine. .NET framework 4.6 of hoger is vereist voor het gebruik van de toegangsmethode met een toegangstoken. Vervang de waarde van `<URI to blob file>` dienovereenkomstig. U kunt deze waarde verkrijgen door te navigeren naar het bestand dat u hebt gemaakt en geüpload naar de blobopslag en de **URL** onder **Eigenschappen** op de pagina **Overzicht** te kopiëren.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.IO;
using System.Net;
using System.Web.Script.Serialization; 
using Microsoft.WindowsAzure.Storage.Auth;
using Microsoft.WindowsAzure.Storage.Blob;

namespace StorageOAuthToken
{
    class Program
    {
        static void Main(string[] args)
        {
            //get token
            string accessToken = GetMSIToken("https://storage.azure.com/");
           
            //create token credential
            TokenCredential tokenCredential = new TokenCredential(accessToken);

            //create storage credentials
            StorageCredentials storageCredentials = new StorageCredentials(tokenCredential);

            Uri blobAddress = new Uri("<URI to blob file>");

            //create block blob using storage credentials
            CloudBlockBlob blob = new CloudBlockBlob(blobAddress, storageCredentials);
        
            //retrieve blob contents
            Console.WriteLine(blob.DownloadText());
            Console.ReadLine();
        }

        static string GetMSIToken(string resourceID)
        {
            string accessToken = string.Empty;
            // Build request to acquire MSI token
            HttpWebRequest request = (HttpWebRequest)WebRequest.Create("http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=" + resourceID);
            request.Headers["Metadata"] = "true";
            request.Method = "GET";

            try
            {
                // Call /token endpoint
                HttpWebResponse response = (HttpWebResponse)request.GetResponse();

                // Pipe response Stream to a StreamReader, and extract access token
                StreamReader streamResponse = new StreamReader(response.GetResponseStream());
                string stringResponse = streamResponse.ReadToEnd();
                JavaScriptSerializer j = new JavaScriptSerializer();
                Dictionary<string, string> list = (Dictionary<string, string>)j.Deserialize(stringResponse, typeof(Dictionary<string, string>));
                accessToken = list["access_token"];
                return accessToken;
            }
            catch (Exception e)
            {
                string errorText = String.Format("{0} \n\n{1}", e.Message, e.InnerException != null ? e.InnerException.Message : "Acquire token failed");
                return accessToken;
            }
        }            
    }
}
```

Het antwoord bevat de inhoud van het bestand:

`Hello world! :)`

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd een beheerde identiteit van een virtuele Windows-machine in te schakelen om toegang tot Azure Storage te krijgen.  Zie voor meer informatie over Azure Storage:

> [!div class="nextstepaction"]
> [Azure Storage](/azure/storage/common/storage-introduction)




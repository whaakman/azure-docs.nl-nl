---
title: Gebruik van een Windows VM beheerd identiteit voor toegang tot Azure Storage
description: Een zelfstudie die u bij het proces helpt van het gebruik van een Windows-VM beheerd identiteit voor toegang tot Azure Storage.
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
ms.date: 04/12/2018
ms.author: daveba
ms.openlocfilehash: 9ccc94727a18fbcd77f00000531934be57b8e132
ms.sourcegitcommit: b7290b2cede85db346bb88fe3a5b3b316620808d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/05/2018
ms.locfileid: "34801361"
---
# <a name="tutorial-use-a-windows-vm-managed-identity-to-access-azure-storage"></a>Zelfstudie: Een Windows-VM beheerd identiteit gebruiken voor toegang tot Azure Storage

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u beheerde identiteit inschakelen voor een virtuele Windows-computer en die identiteit gebruiken voor toegang tot Azure Storage.  In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een virtuele Windows-machine in een nieuwe resourcegroep maken 
> * Beheerde identiteit op een Windows virtuele Machine (VM) inschakelen
> * Een blob-container in een opslagaccount maken
> * Uw Windows-VM identiteit beheerde toegang verlenen tot een opslagaccount 
> * Een toegang krijgen en deze gebruiken om aan te roepen Azure Storage 

> [!NOTE]
> Azure Active Directory-verificatie voor Azure Storage is openbare preview.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine in een nieuwe resourcegroep maken

In deze sectie maakt u een Windows-VM wordt een identiteit beheerd later verleend.

1.  Klik op de **+/ nieuwe service maken** knop gevonden in de linkerbovenhoek van de Azure portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakte Hier ziet u de referenties die u kunt aanmelden bij de virtuele machine.
4.  Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Selecteer een nieuwe **resourcegroep** u wilt virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

    ![De installatiekopie van de alternatieve tekst](../media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-managed-identity-on-your-vm"></a>Beheerde identiteit op de virtuele machine inschakelen

Een beheerde identiteit van de virtuele Machine kunt u toegangstokens ophalen uit Azure AD zonder referenties in uw code te plaatsen. Achter de inschakelen beheerd identiteit op een virtuele Machine via de Azure-portal biedt twee dingen: registreert uw virtuele machine met Azure AD voor het maken van een beheerde identiteit en configureert deze identiteit van de op de virtuele machine. 

1. Navigeer naar de resourcegroep van de nieuwe virtuele machine en selecteer de virtuele machine die u in de vorige stap hebt gemaakt.
2. Onder de **instellingen** categorie, klikt u op **configuratie**.
3. Als u de identiteit van de beheerde selecteert **Ja**.
4. Klik op **opslaan** toepassen van de configuratie. 

## <a name="create-a-storage-account"></a>Create a storage account 

In deze sectie maakt u een opslagaccount. 

1. Klik op de **+ maken van een resource** knop gevonden in de linkerbovenhoek van de Azure portal.
2. Klik op **opslag**, klikt u vervolgens **opslagaccount - blob, bestand, tabel, wachtrij**.
3. Onder **naam**, voer een naam voor het opslagaccount.  
4. **Implementatiemodel** en **Account kind** moet worden ingesteld op **resourcemanager** en **opslag (algemeen v1)**. 
5. Zorg ervoor dat de **abonnement** en **resourcegroep** overeenkomen met de gegevenstypen die u hebt opgegeven toen u uw virtuele machine in de vorige stap hebt gemaakt.
6. Klik op **Create**.

    ![Nieuw opslagaccount maken](../media/msi-tutorial-linux-vm-access-storage/msi-storage-create.png)

## <a name="create-a-blob-container-and-upload-a-file-to-the-storage-account"></a>Maken van een blob-container en een bestand uploaden naar het storage-account

Bestanden vereist blob-opslag, dus u moet maken van een blob-container waarin het bestand wilt opslaan. U kunt vervolgens een bestand uploaden naar de blob-container in het nieuwe opslagaccount.

1. Ga terug naar uw nieuwe opslagaccount.
2. Onder **Blob-Service**, klikt u op **Containers**.
3. Klik op **+ Container** boven aan de pagina.
4. Onder **nieuwe container**, Geef een naam voor de container en onder **openbare toegangsniveau** hou de standaardwaarde.

    ![Opslagcontainer maken](../media/msi-tutorial-linux-vm-access-storage/create-blob-container.png)

5. Met een editor naar keuze, maak een bestand met de titel *world.txt Hallo* op uw lokale machine.  Open het bestand en het toevoegen van de tekst (zonder aanhalingstekens) "Hello world! :) ' en vervolgens opslaan. 
6. Upload het bestand naar de zojuist gemaakte container door te klikken op de containernaam van de, klikt u vervolgens **uploaden**
7. In de **blob uploaden** deelvenster onder **bestanden**, klik op het pictogram van de map en blader naar het bestand **hello_world.txt** op uw lokale computer, selecteert u het bestand en klik op **Uploaden**.
    ![Tekstbestand uploaden](~/articles/active-directory/media/msi-tutorial-linux-vm-access-storage/upload-text-file.png)

## <a name="grant-your-vm-access-to-an-azure-storage-container"></a>Uw VM toegang verlenen tot een Azure Storage-container 

U kunt beheerde identiteit van de VM's gebruiken voor het ophalen van de gegevens in de Azure storage-blob.   

1. Ga terug naar uw nieuwe opslagaccount.  
2. Klik op de **toegangsbeheer (IAM)** koppeling in het linkerdeelvenster.  
3. Klik op **+ toevoegen** boven op de pagina om een nieuwe roltoewijzing voor de virtuele machine.
4. Onder **rol**, selecteer in de vervolgkeuzelijst **gegevenslezer Storage-Blob (Preview)**. 
5. In de volgende vervolgkeuzelijst onder **toewijzen van toegang tot**, kies **virtuele Machine**.  
6. Controleer vervolgens het juiste abonnement wordt vermeld in **abonnement** dropdown en stel vervolgens **resourcegroep** naar **alle resourcegroepen**.  
7. Onder **Selecteer**, kies uw virtuele machine en klik vervolgens op **opslaan**. 

    ![Machtigingen toewijzen](~/articles/active-directory/managed-service-identity/media/tutorial-linux-vm-access-storage/access-storage-perms.png)

## <a name="get-an-access-token-and-use-it-to-call-azure-storage"></a>Een toegangstoken ophalen en deze gebruiken om aan te roepen Azure Storage 

Azure Storage systeemeigen ondersteunt Azure AD-verificatie, zodat deze kan rechtstreeks toegangstokens accepteren verkregen met behulp van de identiteit van een beheerd. Dit maakt deel uit van de integratie van Azure Storage met Azure AD en verschilt van het verstrekken van referenties voor de verbindingsreeks.

Hier volgt een voorbeeld van een .net-code van een verbinding openen naar Azure Storage met behulp van een toegangstoken en vervolgens de inhoud van het bestand dat u eerder hebt gemaakt te lezen. Deze code moet worden uitgevoerd op de virtuele machine kunnen toegang tot beheerde Identity-eindpunt van de VM. .NET framework 4.6 of hoger is vereist voor het gebruik van de methode access token. Vervang de waarde van `<URI to blob file>` dienovereenkomstig. U kunt deze waarde verkrijgen door te navigeren naar het bestand dat u hebt gemaakt en geüpload naar blob storage en kopiëren van de **URL** onder **eigenschappen** de **overzicht** pagina.

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

In deze zelfstudie hebt u geleerd hoe een virtuele Windows-machine beheerde identiteit voor toegang tot Azure Storage inschakelen.  Zie voor meer informatie over Azure Storage:

> [!div class="nextstepaction"]
> [Azure Storage](/azure/storage/common/storage-introduction)




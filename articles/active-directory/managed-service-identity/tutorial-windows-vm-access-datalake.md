---
title: Toegang krijgen tot Azure Data Lake Storage met een Managed Service Identity (MSI) voor Windows-VM
description: Deze zelfstudie laat zien hoe u toegang krijgt tot Azure Data Lake Storage met een Managed Service Identity (MSI) voor Windows-VM.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: msi
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: a7935aa245239ed32527d2c22fd41845c6da2ae1
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39007964"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Zelfstudie: Toegang krijgen tot Azure Data Lake Storage met een Managed Service Identity (MSI) voor Windows-VM

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u toegang krijgt tot Azure Data Lake Storage met behulp van een Managed Service Identity (MSI) voor een virtuele Windows-machine (VM). Managed Service Identity's worden automatisch beheerd in Azure en stellen u in staat om te verifiëren bij services die Azure AD-verificatie ondersteunen, zonder referenties in code te hoeven invoegen. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * MSI inschakelen op een Windows-VM 
> * Uw virtuele machine toegang verlenen tot Azure Data Lake Storage
> * Een toegangstoken ophalen met behulp van de identiteit van de virtuele machine en dat token gebruiken om toegang te krijgen tot Azure Data Lake Storage

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine maken in een nieuwe resourcegroep

Voor deze zelfstudie maken we een nieuwe virtuele Windows-machine.  U kunt MSI ook inschakelen op een bestaande virtuele machine.

1. Klik op de knop **Een resource maken** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3. Geef de informatie van de virtuele machine op. De referenties (combinatie van **Gebruikersnaam** en **Wachtwoord**) die u hier opgeeft, zijn de referenties waarmee u zich aanmeldt bij de virtuele machine.
4. Kies het juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Om een nieuwe **resourcegroep** te selecteren waarin de virtuele machine moet worden gemaakt, kiest u **Nieuwe maken**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Handhaaf de standaardinstellingen op de pagina Instellingen en klik op **OK**.

   ![Alt-tekst voor afbeelding](media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen 

Met een MSI op de VM kunt u toegangstokens uit Azure AD ophalen zonder referenties in uw code te hoeven opnemen. Het inschakelen van MSI vertelt Azure dat er een beheerde identiteit voor uw VM moet worden gemaakt. Er gebeuren twee dingen als u MSI inschakelt: de virtuele machine wordt bij Azure Active Directory geregistreerd om de beheerde identiteit te maken, en de identiteit wordt geconfigureerd op de virtuele machine.

1. Selecteer de **virtuele machine** waarop u MSI wilt inschakelen.  
2. Klik op de linkernavigatiebalk op **Configuratie**. 
3. U ziet **Managed Service Identity**. Als u de MSI wilt registreren en inschakelen, selecteert u **Ja**. Als u de MSI wilt uitschakelen, kiest u Nee. 
4. Vergeet niet op **Opslaan** te klikken om de configuratie op te slaan.  
   ![Alt-tekst voor afbeelding](media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren en nagaan welke extensies aanwezig zijn op deze virtuele machine, klikt u op **Extensies**. Als MSI is ingeschakeld, wordt **ManagedIdentityExtensionforWindows** weergegeven in de lijst.

   ![Alt-tekst voor afbeelding](media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Uw virtuele machine toegang verlenen tot Azure Data Lake Storage

U kunt nu uw virtuele machine toegang tot bestanden en mappen in Azure Data Lake Storage verlenen.  Voor deze stap kunt u een bestaande Data Lake Storage gebruiken, of een nieuwe maken.  Als u een nieuwe Data Lake Storage wilt maken met behulp van Azure Portal, volgt u deze [snelstart voor Azure Data Lake Storage](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-get-started-portal). Er zijn ook snelstarts in de [documentatie over Azure Data Lake Storage](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-overview) voor het gebruik van Azure CLI en Azure Powershell.

Maak een nieuwe map in uw Data Lake Storage, en verleen de MSI van uw VM toestemming voor het lezen, schrijven en uitvoeren van bestanden in die map:

1. Klik in Azure Portal op **Data Lake Storage** in het linkernavigatievenster.
2. Klik op de Data Lake Storage die u wilt gebruiken voor deze zelfstudie.
3. Klik op **Data Explorer** in de opdrachtbalk.
4. De hoofdmap van de Data Lake Storage is geselecteerd.  Klik op **Toegang** in de opdrachtbalk.
5. Klik op **Add**.  Voer in het veld **Selecteren** de naam van uw virtuele machine in, bijvoorbeeld **DevTestVM**.  Klik op uw virtuele machine in de zoekresultaten om deze te selecteren, en klik vervolgens op **Selecteren**.
6. Klik op **Machtigingen selecteren**.  Selecteer **Lezen** en **Uitvoeren**, voeg deze toe aan **Deze map** als **Alleen een toegangsmachtiging**.  Klik op **OK**.  De machtiging wordt toegevoegd.
7. Sluit de blade **Toegang**.
8. Voor deze zelfstudie maakt u een nieuwe map.  Klik op **Nieuwe map** in de opdrachtbalk en geef een naam voor de nieuwe map op, bijvoorbeeld **TestFolder**.  Klik op **OK**.
9. Klik op de map die u hebt gemaakt en klik vervolgens op **Toegang** in de opdrachtbalk.
10. Klik, net zoals bij stap 5, op **Toevoegen**, voer de naam van uw virtuele machine in het veld **Selecteren** in, selecteer deze, en klik op **Selecteren**.
11. Klik, net zoals bij stap 6, op **Machtigingen selecteren**, selecteer **Lezen**, **Schrijven** en **Uitvoeren**, voeg deze toe aan **Deze map** als **Een vermelding van een toegangsmachtiging en een vermelding van een standaardmachtiging**.  Klik op **OK**.  De machtiging wordt toegevoegd.

De MSI van uw virtuele machine kan nu alle bewerkingen uitvoeren op bestanden in de map die u hebt gemaakt.  Lees dit artikel over [toegangsbeheer in Data Lake Storage](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control) voor meer informatie over het beheren van de toegang tot Data Lake Storage.

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Een toegangstoken ophalen met behulp van de MSI van de virtuele machine en dat token gebruiken om toegang te krijgen tot het bestandssysteem van Azure Data Lake Storage

Azure Data Lake Storage biedt systeemeigen ondersteuning voor Azure AD-verificatie, zodat toegangstokens die zijn verkregen met behulp van een MSI direct kunnen worden geaccepteerd.  Om te verifiëren bij het bestandssysteem van Data Lake Storage verzendt u een toegangstoken dat door Azure AD is uitgegeven aan het eindpunt van uw Data Lake Storage-bestandssysteem, in een autorisatie-header in de indeling 'Bearer <WAARDE_VAN_TOEGANGSTOKEN>'.  Lees [Verificatie bij Data Lake Storage met Azure Active Directory](https://docs.microsoft.com/azure/data-lake-store/data-lakes-store-authentication-using-azure-active-directory) voor meer informatie over ondersteuning voor Azure AD-verificatie in Data Lake Storage

> [!NOTE]
> Managed Service Identity wordt nog niet ondersteund voor de SDK's voor clients van het Data Lake Storage-bestandssysteem.  Deze zelfstudie wordt bijgewerkt zodra ondersteuning voor MSI wordt toegevoegd aan de SDK.

In deze zelfstudie gebruikt u PowerShell voor het maken van REST-aanvragen om te verifiëren bij de REST-API van het Data Lake Storage-bestandssysteem. Als u de MSI van de virtuele machine wilt gebruiken voor verificatie, moet u de aanvragen verzenden vanaf de virtuele machine.

1. Navigeer in Azure Portal naar **Virtuele machines**, ga naar uw virtuele Windows-machine, en klik op de pagina **Overzicht** op **Verbinden**.
2. Voer uw referenties (**gebruikersnaam** en **wachtwoord**) in die u hebt toegevoegd bij het maken van de virtuele Windows-machine. 
3. Nu u een **Verbinding met extern bureaublad** met de virtuele machine hebt gemaakt, opent u **PowerShell** in de externe sessie. 
4. Verzend met `Invoke-WebRequest` van Powershell een aanvraag naar op het lokale MSI-eindpunt om een toegangstoken voor Azure Data Lake Storage op te halen.  De resource-id voor Data Lake Storage is ‘https://datalake.azure.net/’.  Voor Data Lake moet u een exact overeenkomende resource-id opgeven, met de schuine streep op het einde.

   ```powershell
   $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fdatalake.azure.net%2F' -Method GET -Headers @{Metadata="true"}
   ```
    
   Converteer de reactie van een JSON-object naar een PowerShell-object. 
    
   ```powershell
   $content = $response.Content | ConvertFrom-Json
   ```

   Extraheer het toegangstoken uit de reactie.
    
   ```powershell
   $AccessToken = $content.access_token
   ```

5. Gebruik in PowerShell de Invoke-WebRequest-opdracht om een aanvraag te verzenden naar het REST-eindpunt van Data Lake Storage om de mappen in de hoofdmap weer te geven.  Dit is een eenvoudige manier om te controleren of alles juist is geconfigureerd.  Het is belangrijk dat de tekenreeks 'Bearer' in de autorisatie-header begint met de hoofdletter 'B'.  U vindt de naam van uw Data Lake Storage in de sectie **Overzicht** van de Data Lake Storage-blade in Azure Portal.

   ```powershell
   Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -Headers @{Authorization="Bearer $AccessToken"}
   ```

   Een geslaagde reactie ziet er als volgt uit:

   ```powershell
   StatusCode        : 200
   StatusDescription : OK
   Content           : {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY", "blockSize":0,"accessTime":1507934941392, "modificationTime":1507944835699,"replication":0, "permission":"770","ow..."
   RawContent        : HTTP/1.1 200 OK
                       Pragma: no-cache
                       x-ms-request-id: b4b31e16-e968-46a1-879a-3474aa7d4528
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict-Transport-Security: ma...
   Forms             : {}
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b4b31e16-e968-46a1-879a-3474aa7d4528],
                       [x-ms-webhdfs-version, 17.04.22.00], [Status, 0x0]...}
   Images            : {}
   InputFields       : {}
   Links             : {}
   ParsedHtml        : System.__ComObject
   RawContentLength  : 556
   ```

6. Nu kunt u proberen een bestand te uploaden naar uw Data Lake Storage.  Maak eerst een bestand om te uploaden.

   ```powershell
   echo "Test file." > Test1.txt
   ```

7. Verzend met behulp van `Invoke-WebRequest` in PowerShell een aanvraag naar het REST-eindpunt van uw Data Lake Storage voor het uploaden van het bestand naar de map die u eerder hebt gemaakt.  Deze aanvraag bestaat uit twee stappen.  In de eerste stap verzendt u een aanvraag haalt u omleidingsgegevens op zodat u weet waarnaar het bestand moet worden geüpload.  In de tweede stap uploadt u het bestand.  Vergeet niet de naam van de map en het bestand aan te passen als u andere waarden gebruikt dan die in deze zelfstudie. 

   ```powershell
   $HdfsRedirectResponse = Invoke-WebRequest -Uri https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Als u de waarde van `$HdfsRedirectResponse` inspecteert, ziet er ongeveer als volgt uit:

   ```powershell
   PS C:\> $HdfsRedirectResponse

   StatusCode        : 307
   StatusDescription : Temporary Redirect
   Content           : {}
   RawContent        : HTTP/1.1 307 Temporary Redirect
                       Pragma: no-cache
                       x-ms-request-id: b7ab492f-b514-4483-aada-4aa0611d12b3
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosn...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, b7ab492f-b514-4483-aada-4aa0611d12b3], 
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

   Voltooi het uploaden door een aanvraag te verzenden naar het omleidingseindpunt:

   ```powershell
   Invoke-WebRequest -Uri $HdfsRedirectResponse.Headers.Location -Method PUT -Headers @{Authorization="Bearer $AccessToken"} -Infile Test1.txt -MaximumRedirection 0
   ```

   Een geslaagde reactie ziet er ongeveer als volgt uit:

   ```powershell
   StatusCode        : 201
   StatusDescription : Created
   Content           : {}
   RawContent        : HTTP/1.1 201 Created
                       Pragma: no-cache
                       x-ms-request-id: 1e70f36f-ead1-4566-acfa-d0c3ec1e2307
                       ContentLength: 0
                       x-ms-webhdfs-version: 17.04.22.00
                       Status: 0x0
                       X-Content-Type-Options: nosniff
                       Strict...
   Headers           : {[Pragma, no-cache], [x-ms-request-id, 1e70f36f-ead1-4566-acfa-d0c3ec1e2307],
                       [ContentLength, 0], [x-ms-webhdfs-version, 17.04.22.00]...}
   RawContentLength  : 0
   ```

Met andere API's van het Data Lake Storage-bestandssysteem kunt u toevoegen aan bestanden, bestanden downloaden, en nog veel meer.

Gefeliciteerd!  U hebt geverifieerd bij het Data Lake Storage-bestandssysteem met behulp van een MSI voor een virtuele machine.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd hoe u toegang krijgt tot Azure Data Lake Storage met behulp van een Managed Service Identity voor een virtuele Windows-machine. Zie voor meer informatie over Azure Data Lake Storage:

> [!div class="nextstepaction"]
>[Azure Data Lake Store](/azure/data-lake-store/data-lake-store-overview)

---
title: Het gebruik van een Linux VM beheerde Service identiteit (MSI) voor toegang tot Azure Data Lake Store
description: Een zelfstudie waarin wordt getoond hoe u een Linux VM beheerde Service identiteit (MSI) wordt gebruikt voor toegang tot Azure Data Lake Store.
services: active-directory
documentationcenter: 
author: skwan
manager: mbaldwin
editor: 
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 4593046ec4d39c874d7ac5849cc9d8e7f30cf2ef
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Een Linux VM beheerde Service identiteit (MSI) gebruiken voor toegang tot Azure Data Lake Store

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe u een beheerde Service identiteit (MSI) voor virtuele Linux-machine (VM) voor toegang tot een Azure Data Lake Store. Beheerde Service-identiteiten worden automatisch beheerd door Azure en u te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. Procedures voor:

> [!div class="checklist"]
> * MSI op een virtuele Linux-machine inschakelen 
> * Uw VM-toegang verlenen aan een Azure Data Lake Store
> * Ophalen van een toegangstoken met behulp van de identiteit van de VM en deze gebruiken voor toegang tot een Azure Data Lake Store

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-Machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Linux VM. U kunt ook MSI op een bestaande virtuele machine inschakelen.

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

   ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** u wilt dat de virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte van de virtuele machine. Selecteer om te zien meer grootten, **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen

De MSI van een virtuele Machine kunt u toegangstokens ophalen uit Azure AD zonder dat u referenties in uw code te plaatsen. Achter de MSI inschakelen biedt twee dingen: het installeren van de MSI-VM-extensie op uw virtuele machine en zorgt ervoor dat MSI in Azure Resource Manager.  

1. Selecteer de **virtuele Machine** dat u inschakelen van MSI wilt op.
2. Klik op de linkernavigatiebalk **configuratie**.
3. U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.

   ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren welke uitbreidingen zijn op deze **Linux VM**, klikt u op **extensies**. Als MSI is ingeschakeld, de **ManagedIdentityExtensionforLinux** verschijnt in de lijst.

   ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Uw VM-toegang verlenen tot Azure Data Lake Store

U kunt nu uw VM-toegang tot bestanden en mappen in een Azure Data Lake Store verlenen.  U kunt voor deze stap maakt gebruik van een bestaande Data Lake Store of een nieuwe maken.  Voor het maken van een nieuwe Data Lake Store met de Azure portal, voert u [Azure Data Lake Store Quick Start](~/articles/data-lake-store/data-lake-store-get-started-portal.md). Er zijn ook snelstartgidsen die gebruikmaken van de Azure CLI en Azure PowerShell in de [documentatie Azure Data Lake Store](~/articles/data-lake-store/data-lake-store-overview.md).

Maak een nieuwe map in uw Data Lake Store en uw VM MSI machtiging verlenen om te lezen, schrijven en uitvoeren van bestanden in de map:

1. Klik in de Azure-portal op **Data Lake Store** in de linkernavigatiebalk.
2. Klik op de Data Lake Store die u wilt gebruiken voor deze zelfstudie.
3. Klik op **Data Explorer** in de opdrachtbalk.
4. De hoofdmap van de Data Lake Store is geselecteerd.  Klik op **toegang** in de opdrachtbalk.
5. Klik op **Add**.  In de **Selecteer** en voer de naam van uw virtuele machine, bijvoorbeeld **DevTestVM**.  Selecteer uw virtuele machine in de zoekresultaten en klik vervolgens op **Selecteer**.
6. Klik op **machtigingen selecteren**.  Selecteer **lezen** en **Execute**, toevoegen aan **deze map**, en toevoegen als **alleen een toegangsmachtiging**.  Klik op **OK**.  De machtiging moet worden toegevoegd.
7. Sluit de **toegang** blade.
8. Maak een nieuwe map voor deze zelfstudie.  Klik op **nieuwe map** in de opdrachtbalk en verleen de nieuwe map een naam, bijvoorbeeld **TestFolder**.  Klik op **OK**.
9. Klik op de map die u hebt gemaakt en klik vervolgens op **toegang** in de opdrachtbalk.
10. Vergelijkbaar is met stap 5, klikt u op **toevoegen**, in de **Selecteer** veld Geef de naam van uw virtuele machine, selecteert u deze en klikt u op **Selecteer**.
11. Vergelijkbaar is met stap 6, klikt u op **Selecteer machtigingen**, selecteer **lezen**, **schrijven**, en **Execute**, toevoegen aan **deze map**, en toevoegen als **een machtigingsvermelding toegang en een standaard machtigingsvermelding**.  Klik op **OK**.  De machtiging moet worden toegevoegd.

Uw VM MSI kunnen alle bewerkingen op bestanden nu uitvoeren in de map die u hebt gemaakt.  Lees voor meer informatie over het beheren van toegang tot Data Lake Store in dit artikel op [toegangsbeheer in Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Ophalen van een toegangstoken met behulp van de VM MSI en deze gebruiken om aan te roepen van het bestandssysteem van Azure Data Lake Store

Azure Data Lake Store systeemeigen ondersteunt Azure AD-verificatie, zodat deze kan rechtstreeks toegangstokens accepteren verkregen met behulp van MSI.  Om het Data Lake Store-bestandssysteem te verifiëren die u verzendt een toegangstoken dat is uitgegeven door Azure AD aan uw Data Lake Store filesystem-eindpunt in een autorisatie-header in de indeling ' Bearer \<ACCESS_TOKEN_VALUE\>'.  Lees voor meer informatie over Data Lake Store-ondersteuning voor verificatie met Azure AD [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

In deze zelfstudie maakt verifiëren u voor het Data Lake Store-bestandssysteem REST-API met CURL aanbrengen REST-aanvragen.

> [!NOTE]
> De client-SDK's van de bestandssysteem in Data Lake Store-Service-identiteit beheerd niet ondersteunen.  Deze zelfstudie wordt bijgewerkt wanneer ondersteuning wordt toegevoegd aan de SDK.

Deze stappen uit te voeren, moet u een SSH-client. Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de portal voor uw Linux-VM en in de **overzicht**, klikt u op **Connect**.  
2. **Verbinding maken met** naar de virtuele machine met de SSH-client van uw keuze. 
3. Het terminalvenster met CURL, zorg er in een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor het Data Lake Store-bestandssysteem.  De resource-id voor de Data Lake Store is 'https://datalake.azure.net/'.  Het is belangrijk dat u de afsluitende slash bevatten in de resource-id.
    
   ```bash
   curl http://localhost:50342/oauth2/token --data "resource=https://datalake.azure.net/" -H Metadata:true   
   ```
    
   Een geslaagde reactie retourneert het toegangstoken dat u gebruiken om te verifiëren naar Data Lake Store:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. Een aanvraag met CURL, worden aanbrengen in uw Data Lake Store bestandssysteem REST-eindpunt voor het weergeven van de mappen in de hoofdmap.  Dit is een eenvoudige manier om dat alles juist is geconfigureerd.  Kopieer de waarde van het toegangstoken van de vorige stap.  Het is belangrijk dat de tekenreeks 'Bearer' in de autorisatie-header is een hoofdletter "B".  U vindt de naam van uw Data Lake Store in de **overzicht** sectie van de blade Data Lake Store in de Azure portal.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Een geslaagde reactie ziet eruit als:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Nu kunt u proberen een bestand te uploaden naar uw Data Lake Store.  Maak eerst een bestand te uploaden.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. Een aanvraag met CURL, worden aanbrengen in uw Data Lake Store REST-eindpunt voor filesystem het bestand te uploaden naar de map die u eerder hebt gemaakt.  Het uploaden omvat een omleiding en CURL volgt de omleiding automatisch. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Een geslaagde reactie ziet eruit als:

   ```bash
   HTTP/1.1 100 Continue
   HTTP/1.1 307 Temporary Redirect
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: 756f6b24-0cca-47ef-aa12-52c3b45b954c
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
       
   HTTP/1.1 100 Continue
       
   HTTP/1.1 201 Created
   Cache-Control: no-cache, no-cache, no-store, max-age=0
   Pragma: no-cache
   Expires: -1
   Location: https://mytestadls.azuredatalakestore.net/webhdfs/v1/TestFolder/Test1.txt?op=CREATE&write=true
   x-ms-request-id: af5baa07-3c79-43af-a01a-71d63d53e6c4
   ContentLength: 0
   x-ms-webhdfs-version: 17.04.22.00
   Status: 0x0
   X-Content-Type-Options: nosniff
   Strict-Transport-Security: max-age=15724800; includeSubDomains
   Date: Sun, 15 Oct 2017 22:10:30 GMT
   Content-Length: 0
   ```

Met andere bestandssysteem Data Lake Store downloaden API's die u kunt toevoegen aan bestanden, bestanden en meer.

Gefeliciteerd!  U hebt geverifieerd voor het Data Lake Store-bestandssysteem met behulp van een VM MSI.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).
- Bewerkingen Data Lake Store maakt gebruik van Azure Resource Manager voor beheer.  Lees voor meer informatie over het gebruik van een VM MSI om te verifiëren naar Resource Manager [een Linux VM beheerde Service identiteit (MSI) gebruiken voor toegang tot de Resource Manager](msi-tutorial-linux-vm-access-arm.md).
- Meer informatie over [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).
- Meer informatie over [Bestandssysteembewerkingen op Azure Data Lake Store met REST-API](~/articles/data-lake-store/data-lake-store-data-operations-rest-api.md) of de [WebHDFS FileSystem APIs](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis.md).
- Meer informatie over [toegangsbeheer in Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.
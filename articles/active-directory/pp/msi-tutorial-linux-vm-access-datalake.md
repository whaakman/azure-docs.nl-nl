---
title: Het gebruik van een Linux VM Managed Service Identity (MSI) voor toegang tot Azure Data Lake Store
description: Een zelfstudie waarin wordt uitgelegd hoe u een Linux VM Managed Service Identity (MSI) gebruikt voor toegang tot Azure Data Lake Store.
services: active-directory
documentationcenter: ''
author: daveba
manager: mtillman
editor: ''
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: skwan
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 358827722e8d77cd91410fae842ad2ba99967d98
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38610351"
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-data-lake-store"></a>Een Linux VM Managed Service Identity (MSI) gebruiken voor toegang tot Azure Data Lake Store

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie leert u hoe u een Managed Service Identity (MSI) voor een Linux-machine (VM) gebruikt voor toegang tot een Azure Data Lake Store. Beheerde Service-identiteiten automatisch worden beheerd door Azure en kunt u voor de verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * MSI-bestand op een virtuele Linux-machine inschakelen 
> * Uw VM-toegang verlenen aan een Azure Data Lake Store
> * Een toegangstoken met behulp van de identiteit van de virtuele machine en deze gebruiken voor toegang tot een Azure Data Lake Store

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-linux-virtual-machine-in-a-new-resource-group"></a>Een virtuele Linux-Machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Linux-VM. U kunt ook de MSI-bestand op een bestaande virtuele machine inschakelen.

1. Klik op **een resource maken** in de linkerbovenhoek van Azure portal.
2. Selecteer **Compute** en selecteer vervolgens **Ubuntu Server 16.04 LTS**.
3. Geef de informatie van de virtuele machine op. Voor **verificatietype**, selecteer **openbare SSH-sleutel** of **wachtwoord**. De gemaakte referenties kunnen u zich aanmelden bij de virtuele machine.

   ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-vm.png)

4. Kies een **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5. Selecteer een nieuwe **resourcegroep** u wilt dat de virtuele machine om te worden gemaakt in of kies **nieuw**. Na het voltooien klikt u op **OK**.
6. Selecteer de grootte voor de virtuele machine. Meer groottes Selecteer **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

## <a name="enable-msi-on-your-vm"></a>MSI-bestand op de virtuele machine inschakelen

Een MSI-bestand voor de virtuele Machine kunt u tokens voor toegang van Azure AD ophalen zonder dat u om referenties in uw code. Op de achtergrond inschakelen van MSI doet twee dingen: de MSI-VM-extensie worden geïnstalleerd op de virtuele machine en zorgt ervoor dat MSI in Azure Resource Manager.  

1. Selecteer de **virtuele Machine** dat u inschakelen van MSI-bestand wilt op.
2. Klik op de linker navigatiebalk op **configuratie**.
3. U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Controleer of u klikken op **opslaan** aan de configuratie op te slaan.

   ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren welke uitbreidingen zijn op deze **Linux-VM**, klikt u op **extensies**. Als het MSI-bestand is ingeschakeld, de **ManagedIdentityExtensionforLinux** wordt weergegeven in de lijst.

   ![De installatiekopie van de ALT-tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)

## <a name="grant-your-vm-access-to-azure-data-lake-store"></a>Uw VM-toegang verlenen tot Azure Data Lake Store

U kunt nu uw VM-toegang tot bestanden en mappen in een Azure Data Lake Store verlenen.  U kunt voor deze stap maakt gebruik van een bestaande Data Lake Store of een nieuwe maken.  Voor het maken van een nieuwe Data Lake Store met behulp van de Azure portal, volgt u deze [Azure Data Lake Store-snelstartgids](~/articles/data-lake-store/data-lake-store-get-started-portal.md). Er zijn ook snelstartgidsen die gebruikmaken van de Azure CLI en Azure PowerShell in de [documentatie Azure Data Lake Store](~/articles/data-lake-store/data-lake-store-overview.md).

Maak een nieuwe map in uw Data Lake Store, en uw VM MSI toestemming geven om te lezen, schrijven en uitvoeren van bestanden in die map:

1. Klik in de Azure-portal op **Data Lake Store** in de navigatiebalk links.
2. Klik op het Data Lake Store die u wilt gebruiken voor deze zelfstudie.
3. Klik op **Data Explorer** in de opdrachtbalk.
4. De hoofdmap van de Data Lake Store is geselecteerd.  Klik op **toegang** in de opdrachtbalk.
5. Klik op **Add**.  In de **Selecteer** en voer de naam van uw virtuele machine, bijvoorbeeld **DevTestVM**.  Klik om te selecteren van de virtuele machine in de zoekresultaten en klik vervolgens op **Selecteer**.
6. Klik op **machtigingen selecteren**.  Selecteer **lezen** en **Execute**, toevoegen aan **deze map**, en toevoegen als **alleen een toegangsmachtiging**.  Klik op **OK**.  De machtiging moet worden toegevoegd.
7. Sluit de **toegang** blade.
8. Voor deze zelfstudie, moet u een nieuwe map maken.  Klik op **nieuwe map** in de opdrachtbalk en geef de nieuwe map een naam, bijvoorbeeld **TestFolder**.  Klik op **OK**.
9. Klik op de map die u hebt gemaakt en klik vervolgens op **toegang** in de opdrachtbalk.
10. Vergelijkbaar met stap 5, klikt u op **toevoegen**, in de **Selecteer** veld Voer de naam van uw virtuele machine, selecteert u deze en klikt u op **Selecteer**.
11. Vergelijkbaar met stap 6, klikt u op **Selecteer machtigingen**, selecteer **lezen**, **schrijven**, en **Execute**, toevoegen aan **deze map**, en toevoegen als **vermelding van een toegangsmachtiging en een standaardmachtiging**.  Klik op **OK**.  De machtiging moet worden toegevoegd.

Uw VM MSI kunnen alle bewerkingen op bestanden nu uitvoeren in de map die u hebt gemaakt.  Voor meer informatie over het beheren van toegang tot Data Lake Store, in dit artikel leest u over [Access Control in Data Lake Store](https://docs.microsoft.com/azure/data-lake-store/data-lake-store-access-control).

## <a name="get-an-access-token-using-the-vm-msi-and-use-it-to-call-the-azure-data-lake-store-filesystem"></a>Een toegangstoken met behulp van de VM-MSI en deze gebruiken om aan te roepen van het Azure Data Lake Store-bestandssysteem

Azure Data Lake Store systeemeigen ondersteunt Azure AD-verificatie, zodat deze direct access tokens die zijn verkregen met behulp van MSI.  Om de Data Lake Store-bestandssysteem te verifiëren die u verzendt een toegangstoken dat is uitgegeven door Azure AD aan uw Data Lake Store-bestandssysteem-eindpunt, in een autorisatie-header in de indeling ' Bearer \<ACCESS_TOKEN_VALUE\>'.  Lees voor meer informatie over Data Lake Store ondersteuning voor Azure AD-verificatie, [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md)

In deze zelfstudie gaat verifiëren u naar het Data Lake Store-bestandssysteem-REST-API met CURL om de REST-aanvragen.

> [!NOTE]
> De Data Lake Store-bestandssysteemclient SDK's bieden nog geen ondersteuning voor beheerde Service-identiteit.  In deze zelfstudie wordt bijgewerkt wanneer ondersteuning wordt toegevoegd aan de SDK.

Als u wilt deze stappen hebt voltooid, moet u een SSH-client. Als u Windows gebruikt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [over het gebruik van SSH-sleutels met Windows op Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en gebruiken van de openbare en persoonlijke sleutelpaar voor een SSH voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).

1. Navigeer in de portal voor uw Linux-VM en klik in de **overzicht**, klikt u op **Connect**.  
2. **Verbinding maken met** aan de virtuele machine met de SSH-client van uw keuze. 
3. In het terminalvenster met CURL, een aanvraag indienen op het lokale eindpunt van de MSI-bestand op een toegangstoken verkrijgen voor het Data Lake Store-bestandssysteem.  De resource-id voor Data Lake Store is 'https://datalake.azure.net/. "  Het is belangrijk om de afsluitende slash bevatten in de resource-id.
    
   ```bash
   curl http://localhost:50342/oauth2/token --data "resource=https://datalake.azure.net/" -H Metadata:true   
   ```
    
   Een geslaagde respons retourneert het toegangstoken dat u gebruikt voor verificatie naar Data Lake Store:

   ```bash
   {"access_token":"eyJ0eXAiOiJ...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1508119757",
    "not_before":"1508115857",
    "resource":"https://datalake.azure.net/",
    "token_type":"Bearer"}
   ```

4. Met CURL, een aanvraag indienen met uw Data Lake Store REST-eindpunt voor bestandssysteem om de mappen in de hoofdmap weer te geven.  Dit is een eenvoudige manier om te controleren of dat alles juist is geconfigureerd.  Kopieer de waarde van het toegangstoken in de vorige stap.  Het is belangrijk dat de tekenreeks "Bearer" in de autorisatie-header is een hoofdletter "B".  U vindt de naam van uw Data Lake Store in de **overzicht** sectie van de Data Lake Store-blade in Azure portal.

   ```bash
   curl https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/?op=LISTSTATUS -H "Authorization: Bearer <ACCESS_TOKEN>"
   ```
    
   Een geslaagde respons ziet eruit zoals:

   ```bash
   {"FileStatuses":{"FileStatus":[{"length":0,"pathSuffix":"TestFolder","type":"DIRECTORY","blockSize":0,"accessTime":1507934941392,"modificationTime":1508105430590,"replication":0,"permission":"770","owner":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071","group":"bd0e76d8-ad45-4fe1-8941-04a7bf27f071"}]}}
   ```

5. Nu kunt u proberen een bestand uploaden naar uw Data Lake Store.  Maak eerst een bestand te uploaden.

   ```bash
   echo "Test file." > Test1.txt
   ```

6. Met CURL, een aanvraag indienen met uw Data Lake Store REST-eindpunt voor bestandssysteem voor het uploaden van het bestand naar de map die u eerder hebt gemaakt.  Het uploaden omvat een omleiding en CURL volgt de omleiding automatisch. 

   ```bash
   curl -i -X PUT -L -T Test1.txt -H "Authorization: Bearer <ACCESS_TOKEN>" 'https://<YOUR_ADLS_NAME>.azuredatalakestore.net/webhdfs/v1/<FOLDER_NAME>/Test1.txt?op=CREATE' 
   ```

    Een geslaagde respons ziet eruit zoals:

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

Andere Data Lake Store-bestandssysteem met de API's die u kunt toevoegen aan bestanden downloaden en meer.

Gefeliciteerd!  U hebt geverifieerd voor de Data Lake Store-bestandssysteem met behulp van een VM MSI.

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).
- Bewerkingen Data Lake Store gebruikt voor het beheer van Azure Resource Manager.  Lees voor meer informatie over het gebruik van een VM MSI om te verifiëren naar Resource Manager [gebruiken van een Linux VM Managed Service Identity (MSI) voor toegang tot Resource Manager](../managed-service-identity/msi-tutorial-linux-vm-access-arm.md).
- Meer informatie over [verificatie met Data Lake Store met Azure Active Directory](~/articles/data-lake-store/data-lakes-store-authentication-using-azure-active-directory.md).
- Meer informatie over [bestandssysteembewerkingen in Azure Data Lake Store met REST-API](~/articles/data-lake-store/data-lake-store-data-operations-rest-api.md) of de [WebHDFS FileSystem APIs](https://docs.microsoft.com/rest/api/datalakestore/webhdfs-filesystem-apis).
- Meer informatie over [Access Control in Data Lake Store](~/articles/data-lake-store/data-lake-store-access-control.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.
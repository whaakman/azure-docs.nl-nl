---
title: Gebruik van een Linux-VM-MSI voor toegang tot Azure Sleutelkluis
description: Een zelfstudie die u bij het proces helpt van het gebruik van een Linux VM beheerde Service identiteit (MSI) voor toegang tot Azure Resource Manager.
services: active-directory
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: bryanla
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/15/2017
ms.author: bryanla
ROBOTS: NOINDEX,NOFOLLOW
ms.openlocfilehash: 3531d6e406e7cf44daaaf03c73c93ec3662f108c
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-linux-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Een Linux VM beheerde Service identiteit (MSI) gebruiken voor toegang tot Azure Sleutelkluis 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe u beheerde Service identiteit (MSI) inschakelen voor een virtuele Linux-Machine en vervolgens die identiteit gebruiken voor toegang tot Azure Sleutelkluis. Fungeren als een bootstrap, maakt Sleutelkluis het mogelijk voor de clienttoepassing vervolgens geheim voor toegang tot bronnen die niet beveiligd door Azure Active Directory (AD) gebruiken. Beheerde Service-identiteiten worden automatisch beheerd door Azure en u te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. 

Procedures voor:

> [!div class="checklist"]
> * MSI op een virtuele Linux-Machine inschakelen 
> * Uw VM toegang verlenen tot een geheim dat is opgeslagen in een Sleutelkluis 
> * Ophalen van een toegangstoken met behulp van de identiteit van de VM en deze gebruiken voor het ophalen van het geheim uit de Sleutelkluis 
 
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
6. Selecteer de grootte van de virtuele machine. Selecteer om te zien meer grootten, **weergeven van alle** of wijzigen van de ondersteunde schijf type filter. Behoud de standaardinstellingen op de instellingenpagina en klik op **OK**.

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen

De MSI van een virtuele Machine kunt u toegangstokens ophalen uit Azure AD zonder dat u referenties in uw code te plaatsen. Achter de MSI inschakelen biedt twee dingen: het installeren van de MSI-VM-extensie op uw virtuele machine en zorgt ervoor dat MSI voor de virtuele machine.  

1. Selecteer de **virtuele Machine** dat u inschakelen van MSI wilt op.
2. Klik op de linkernavigatiebalk **configuratie**.
3. U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee.
4. Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren welke uitbreidingen zijn op deze **Linux VM**, klikt u op **extensies**. Als MSI is ingeschakeld, de **ManagedIdentityExtensionforLinux** verschijnt in de lijst.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-extension-value.png)


## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Uw VM-toegang verlenen aan een geheim dat is opgeslagen in een Sleutelkluis  

Met behulp van MSI Vind uw code toegangstokens voor verificatie naar bronnen die ondersteuning bieden voor Azure Active Directory-verificatie. Niet alle Azure-services ondersteunen echter Azure AD-verificatie. Opslaan van referenties voor de service in Azure Key Vault voor het gebruik van MSI aan deze services, en het gebruik van MSI voor toegang tot de Sleutelkluis voor het ophalen van de referenties. 

Eerst moet er een Sleutelkluis maken en onze VM identiteit toegang verlenen aan de Sleutelkluis.   

1. Selecteer boven aan de linkernavigatiebalk **+ nieuw** vervolgens **beveiliging en identiteit** vervolgens **Sleutelkluis**.  
2. Geef een **naam** voor de nieuwe Sleutelkluis. 
3. De Sleutelkluis niet vinden in de hetzelfde abonnement en de resource-groep als de virtuele machine die u eerder hebt gemaakt. 
4. Selecteer **toegangsbeleid** en klik op **nieuwe toevoegen**. 
5. Selecteer in het configureren van sjabloon **geheim Management**. 
6. Kies **Principal selecteren**, en voer de naam van de virtuele machine die u eerder hebt gemaakt in het zoekveld.  Selecteer de virtuele machine in de lijst met resultaten en klik **Selecteer**. 
7. Klik op **OK** naar het nieuwe toegangsbeleid toevoegen is voltooid en **OK** toegang beleid selectie te voltooien. 
8. Klik op **maken** voltooid met het maken van de Sleutelkluis. 

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)

Vervolgens een geheim toevoegen aan de Sleutelkluis, zodat u later kunt u de code die wordt uitgevoerd in uw virtuele machine met geheim ophalen: 

1. Selecteer **alle Resources**, en zoek en selecteer de Sleutelkluis die u hebt gemaakt. 
2. Selecteer **geheimen**, en klik op **toevoegen**. 
3. Selecteer **handmatige**, van **uploaden opties**. 
4. Voer een naam en waarde voor het geheim.  De waarde kan elke gewenste zijn. 
5. Laat de activering en vervaldatum wissen, en laat **ingeschakeld** als **Ja**. 
6. Klik op **maken** voor het maken van het geheim. 
 
## <a name="get-an-access-token-using-the-vms-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Ophalen van een toegangstoken die met de identiteit van de VM en deze gebruiken voor het ophalen van het geheim uit de Sleutelkluis  

Deze stappen uit te voeren, moet u een SSH-client.  Als u van Windows gebruikmaakt, kunt u de SSH-client in de [Windows-subsysteem voor Linux](https://msdn.microsoft.com/commandline/wsl/about). Als u informatie over het configureren van de client van uw SSH-sleutels nodig hebt, raadpleegt u [het gebruik van SSH-sleutels met Windows in Azure](~/articles/virtual-machines/linux/ssh-from-windows.md), of [maken en de openbare en persoonlijke sleutelpaar voor een SSH gebruiken voor virtuele Linux-machines in Azure](~/articles/virtual-machines/linux/mac-create-ssh-keys.md).
 
1. Navigeer in de portal voor uw Linux-VM en in de **overzicht**, klikt u op **Connect**. 
2. **Verbinding maken met** naar de virtuele machine met de SSH-client van uw keuze. 
3. Het terminalvenster met CURL, zorg er in een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Sleutelkluis.  
 
    De aanvraag CURL voor het toegangstoken is lager dan.  
    
    ```bash
    curl http://localhost:50342/oauth2/token --data "resource=https://vault.azure.net" -H Metadata:true  
    ```
    Het antwoord bevat het toegangstoken dat u nodig hebt voor toegang tot de Resource Manager. 
    
    Antwoord:  
    
    ```bash
    {"access_token":"eyJ0eXAi...",
    "refresh_token":"",
    "expires_in":"3599",
    "expires_on":"1504130527",
    "not_before":"1504126627",
    "resource":"https://vault.azure.net",
    "token_type":"Bearer"} 
    ```
    
    U kunt deze toegangstoken gebruiken om te verifiëren voor Azure Sleutelkluis.  De volgende CURL-aanvraag ziet u hoe een geheim van de Sleutelkluis met CURL en de Sleutelkluis REST-API.  U moet de URL van uw Sleutelkluis, dat zich bevindt in de **Essentials** sectie van de **overzicht** pagina van de Sleutelkluis.  U moet ook het toegangstoken dat u hebt verkregen in de vorige aanroep. 
        
    ```bash
    curl https://<YOUR-KEY-VAULT-URL>/secrets/<secret-name>?api-version=2016-10-01 -H "Authorization: Bearer <ACCESS TOKEN>" 
    ```
    
    Het antwoord ziet er als volgt: 
    
    ```bash
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Zodra u het geheim hebt opgehaald uit de Sleutelkluis, kunt u deze kunt gebruiken voor verificatie bij een service waarvoor een gebruikersnaam en wachtwoord.


## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.





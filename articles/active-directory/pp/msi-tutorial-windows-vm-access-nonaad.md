---
title: Een Windows VM-MSI gebruiken voor toegang tot Azure Key Vault
description: Een zelfstudie die u helpt bij het proces van het gebruik van een Windows VM Managed Service Identity (MSI) voor toegang tot Azure Key Vault.
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
ms.openlocfilehash: 20f8ac8f301302f3121880752ea698425c194623
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/14/2018
ms.locfileid: "39049432"
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-azure-key-vault"></a>Een Windows VM Managed Service Identity (MSI) gebruiken voor toegang tot Azure Key Vault 

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie leert u hoe u Managed Service Identity (MSI) inschakelen voor een Windows-Machine en vervolgens die identiteit gebruiken voor toegang tot Azure Key Vault. Fungeren als een bootstrap, maakt Key Vault het mogelijk voor de clienttoepassing gebruikt u het geheim voor toegang tot resources die niet beveiligd door Azure Active Directory (AD). Beheerde Service-identiteiten automatisch worden beheerd door Azure en kunt u voor de verificatie bij services die ondersteuning bieden voor Azure AD-verificatie, zonder referenties invoegen in uw code. 

In deze zelfstudie leert u procedures om het volgende te doen:


> [!div class="checklist"]
> * Beheerde Service-identiteit op een virtuele Machine van Windows inschakelen 
> * Uw virtuele machine toegang verlenen tot een geheim opgeslagen in een Key Vault 
> * Een toegangstoken met behulp van de identiteit van de virtuele machine en deze gebruiken om op te halen van het geheim uit Key Vault 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij de Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows-VM. U kunt ook de MSI-bestand op een bestaande virtuele machine inschakelen.

1.  Klik op **een resource maken** in de linkerbovenhoek van Azure portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakt als volgt de referenties die u gebruiken voor aanmelding bij de virtuele machine.
4.  Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Selecteer een nieuwe **resourcegroep** u graag aan virtuele machine moet worden gemaakt in, kies **nieuw**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte voor de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Handhaaf op de blade Instellingen de standaardwaarden en klik op **OK**.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI-bestand op de virtuele machine inschakelen 

Een MSI-bestand voor de virtuele Machine kunt u tokens voor toegang van Azure AD ophalen zonder dat u om referenties in uw code. Inschakelen van MSI weet Azure te maken van een beheerde identiteit voor uw virtuele Machine. Op de achtergrond inschakelen van MSI doet twee dingen: de MSI-VM-extensie worden geïnstalleerd op de virtuele machine en Hiermee MSI in Azure Resource Manager.

1.  Selecteer de **virtuele Machine** dat u inschakelen van MSI-bestand wilt op.  
2.  Klik op de linker navigatiebalk op **configuratie**. 
3.  U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee. 
4.  Controleer of u klikken op **opslaan** aan de configuratie op te slaan.  

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren en nagaan welke uitbreidingen zijn op deze virtuele machine, klikt u op **extensies**. Als het MSI-bestand is ingeschakeld, klikt u vervolgens **ManagedIdentityExtensionforWindows** wordt weergegeven in de lijst.

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Uw VM-toegang verlenen tot een geheim opgeslagen in een Key Vault 
 
Met behulp van MSI krijgt uw code toegangstokens voor verificatie bij de resources die ondersteuning bieden voor Azure AD-verificatie.  Niet alle Azure-services bieden echter ondersteuning voor Azure AD-verificatie. Voor het gebruik van MSI-bestand met deze services, referenties voor de service in Azure Key Vault opslaan en MSI gebruiken voor toegang tot Key Vault om op te halen van de referenties. 

Eerst moet er voor het maken van een Key Vault en van onze VM identiteit toegang verlenen tot de Key Vault.   

1. Selecteer boven aan de linker navigatiebalk **+ nieuw** vervolgens **beveiliging en identiteit** vervolgens **Key Vault**.  
2. Geef een **naam** voor de nieuwe Key Vault. 
3. Ga naar de Key Vault in het hetzelfde abonnement en resourcegroep als de virtuele machine die u eerder hebt gemaakt. 
4. Selecteer **toegangsbeleid** en klikt u op **nieuwe toevoegen**. 
5. Selecteer in het configureren van sjabloon **geheim Management**. 
6. Kies **Principal selecteren**, en voer in het zoekveld de naam van de virtuele machine die u eerder hebt gemaakt.  Selecteer de virtuele machine in de lijst met resultaten en klikt u op **Selecteer**. 
7. Klik op **OK** naar de nieuw toegangsbeleid toevoegen is voltooid en **OK** voltooid toegang-beleid te selecteren. 
8. Klik op **maken** te maken van de Key Vault voltooien. 

    ![De installatiekopie van de ALT-tekst](../managed-service-identity/media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Vervolgens een geheim toevoegen aan de Key Vault, zodat u later kunt u het geheim met behulp van code die wordt uitgevoerd in uw virtuele machine ophalen: 

1. Selecteer **alle Resources**, en zoek en selecteer de Sleutelkluis die u hebt gemaakt. 
2. Selecteer **geheimen**, en klikt u op **toevoegen**. 
3. Selecteer **handmatig**, van **uploadopties**. 
4. Voer een naam en waarde voor de geheime sleutel.  De waarde mag elke gewenste. 
5. Laat de activeringsdatum en datum van afloop voor wissen en laat **ingeschakeld** als **Ja**. 
6. Klik op **maken** om het geheim te maken. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Een toegangstoken met behulp van de identiteit van de virtuele machine en deze gebruiken om op te halen van het geheim van de Key Vault  

Als u geen PowerShell 4.3.1 of hoger zijn geïnstalleerd, moet u naar [download en installeer de meest recente versie](https://docs.microsoft.com/powershell/azure/overview).

Eerst wordt van de VM-MSI gebruiken voor een toegangstoken om te verifiëren naar Key Vault:
 
1. Navigeer in de portal naar **virtuele Machines** en gaat u naar uw Windows-machine en klik in de **overzicht**, klikt u op **Connect**.
2. Geef in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd bij het maken van de **Windows VM**.  
3. Nu dat u hebt gemaakt een **verbinding met extern bureaublad** met de virtuele machine, opent u PowerShell in de externe sessie.  
4. In PowerShell, aanroepen van de webaanvraag voor de tenant om op te halen van het token voor de lokale host in de specifieke poort voor de virtuele machine.  

    De PowerShell-aanvraag:
    
    ```powershell
    PS C:\> $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://vault.azure.net"} -Headers @{Metadata="true"} 
    ```
    
    Het volledige antwoord dat is opgeslagen als een tekenreeks JavaScript Object Notation (JSON)-indeling in het object $response vervolgens uitpakken.  
    
    ```powershell
    PS C:\> $content = $response.Content | ConvertFrom-Json 
    ```
    
    Haal het toegangstoken vervolgens uit het antwoord.  
    
    ```powershell
    PS C:\> $KeyVaultToken = $content.access_token 
    ```
    
    Ten slotte van de PowerShell-opdracht voor Invoke-WebRequest gebruiken om op te halen van de geheime sleutel die u eerder in de Key Vault, doorgeven van het toegangstoken in de autorisatie-header gemaakt.  U moet de URL van uw Key Vault, die zich in de **Essentials** sectie van de **overzicht** pagina van de Key Vault.  
    
    ```powershell
    PS C:\> (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    Het antwoord er als volgt: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Zodra u het geheim hebt opgehaald uit de Key Vault, kunt u deze kunt gebruiken om te verifiëren bij een service die een gebruikersnaam en wachtwoord vereist. 

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de beheerde Service-identiteit](msi-overview.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en vorm van onze inhoud.

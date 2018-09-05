---
title: Toegang krijgen tot Azure Key Vault met Managed Service Identity voor Windows-VM
description: Een zelfstudie die u helpt bij het doorlopen van het proces voor het krijgen van toegang tot Azure Key Vault met Managed Service Identity voor Windows-VM.
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
ms.date: 11/20/2017
ms.author: daveba
ms.openlocfilehash: 314e3ba608f41ff83565ddaa32e9cdeed2205211
ms.sourcegitcommit: f1e6e61807634bce56a64c00447bf819438db1b8
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/24/2018
ms.locfileid: "42886540"
---
# <a name="tutorial-use-a-windows-vm-managed-service-identity-to-access-azure-key-vault"></a>Zelfstudie: Toegang krijgen tot Azure Key Vault met Managed Service Identity voor Windows-VM 

[!INCLUDE[preview-notice](../../../includes/active-directory-msi-preview-notice.md)]

Deze zelfstudie laat zien hoe u toegang krijgt tot Azure Key Vault met een door het systeem toegewezen identiteit voor een virtuele Windows-machine (VM). Key Vault fungeert als een bootstrap en maakt het mogelijk voor uw clienttoepassing om met het geheim toegang te krijgen tot resources die niet zijn beveiligd met Azure Active Directory (AD). Managed Service Identity's worden automatisch beheerd in Azure en stellen u in staat om te verifiëren bij services die Azure AD-verificatie ondersteunen, zonder referenties in code te hoeven invoegen. 

In deze zelfstudie leert u procedures om het volgende te doen:


> [!div class="checklist"]
> * Uw virtuele machine toegang verlenen tot een geheim dat is opgeslagen in een sleutelkluis 
> * Een toegangstoken ophalen met behulp van de identiteit van de virtuele machine en daarmee het geheim uit Key Vault ophalen 

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-qs-configure-prereqs](../../../includes/active-directory-msi-qs-configure-prereqs.md)]

[!INCLUDE [msi-tut-prereqs](../../../includes/active-directory-msi-tut-prereqs.md)]

- [Aanmelden bij de Azure-portal](https://portal.azure.com)

- [Een virtuele Windows-machine maken](/azure/virtual-machines/windows/quick-create-portal)

- [Door het systeem toegewezen identiteit op uw virtuele machine inschakelen](/azure/active-directory/managed-service-identity/qs-configure-portal-windows-vm#enable-system-assigned-identity-on-an-existing-vm)

## <a name="grant-your-vm-access-to-a-secret-stored-in-a-key-vault"></a>Uw virtuele machine toegang verlenen tot een geheim dat is opgeslagen in een sleutelkluis 
 
Met behulp van Managed Service Identity kan uw code toegangstokens ophalen voor verificatie bij resources die ondersteuning bieden voor Azure AD-verificatie.  Niet alle Azure-services bieden echter ondersteuning voor Azure AD-verificatie. Als u Managed Service Identity wilt gebruiken voor deze services, slaat u de referenties voor de service op in Azure Key Vault en haalt u de referenties op uit Key Vault met behulp van Managed Service Identity. 

Eerst moeten we een sleutelkluis maken en de identiteit van onze virtuele machine toegang tot de sleutelkluis verlenen.   

1. Selecteer boven aan de linkernavigatiebalk **Een resource maken** > **Beveiliging en identiteit** > **Key Vault**.  
2. Geef een **naam** op voor de nieuwe sleutelkluis. 
3. Zoek de sleutelkluis in hetzelfde abonnement en dezelfde resourcegroep als de virtuele machine die u eerder hebt gemaakt. 
4. Selecteer **Toegangsbeleid** en klik op de knop **Nieuwe toevoegen**. 
5. Selecteer **Geheimenbeheer** in Configureren met sjabloon. 
6. Kies **Principal selecteren**, en voer in het zoekveld de naam in van de virtuele machine die u eerder hebt gemaakt.  Selecteer de virtuele machine in de lijst met resultaten en klik op **Selecteren**. 
7. Klik op **OK** om het nieuwe toegangsbeleid toe te voegen en klik op **OK** om de selectie van het toegangsbeleid te voltooien. 
8. Klik op **Maken** om het maken van de sleutelkluis te voltooien. 

    ![Alt-tekst voor afbeelding](../managed-service-identity/media/msi-tutorial-windows-vm-access-nonaad/msi-blade.png)


Voeg vervolgens een geheim toe aan de sleutelkluis, zodat u het geheim later kunt ophalen met behulp van code die wordt uitgevoerd op uw virtuele machine: 

1. Selecteer **Alle resources** en zoek en selecteer de sleutelkluis die u hebt gemaakt. 
2. Selecteer **Geheimen** en klik op **Toevoegen**. 
3. Selecteer **Handmatig** in **Opties uploaden**. 
4. Voer een naam en een waarde in voor de geheime sleutel.  De waarde mag elke gewenste waarde zijn. 
5. Laat de activeringsdatum en vervaldatum leeg en laat **Ingeschakeld** ingesteld staan op **Ja**. 
6. Klik op **Maken** om het geheim te maken. 
 
## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-retrieve-the-secret-from-the-key-vault"></a>Een toegangstoken ophalen met behulp van de identiteit van de virtuele machine en daarmee het geheim uit de sleutelkluis ophalen  

Als PowerShell 4.3.1 of hoger niet is geïnstalleerd, moet u [de meest recente versie downloaden en installeren](https://docs.microsoft.com/powershell/azure/overview).

Eerst gebruiken we de Managed Service Identity van de virtuele machine voor het ophalen van een toegangstoken om te verifiëren bij Key Vault:
 
1. Navigeer in Azure Portal naar **Virtuele machines**, ga naar uw virtuele Windows-machine en klik op de pagina **Overzicht** op **Verbinden**.
2. Voer uw referenties (**gebruikersnaam** en **wachtwoord**) in die u hebt toegevoegd bij het maken van de **virtuele Windows-machine**.  
3. Nu u een **Verbinding met extern bureaublad** met de virtuele machine hebt gemaakt, opent u PowerShell in de externe sessie.  
4. Voer in PowerShell een Invoke-WebRequest-opdracht uit op de tenant om het token voor de lokale host in de specifieke poort voor de virtuele machine op te halen.  

    De PowerShell-aanvraag:
    
    ```powershell
    $response = Invoke-WebRequest -Uri 'http://169.254.169.254/metadata/identity/oauth2/token?api-version=2018-02-01&resource=https%3A%2F%2Fvault.azure.net' -Method GET -Headers @{Metadata="true"} 
    ```
    
    Extraheer vervolgens de volledige reactie, die is opgeslagen als een tekenreeks in JSON-indeling (JavaScript Object Notation) in het object $response.  
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json 
    ```
    
    Extraheer vervolgens het toegangstoken uit het antwoord.  
    
    ```powershell
    $KeyVaultToken = $content.access_token 
    ```
    
    Gebruik ten slotte de Invoke-WebRequest-opdracht van Powershell om het geheim op te halen dat u eerder in de sleutelkluis hebt gemaakt, waarbij u het toegangstoken doorgeeft in de autorisatie-header.  U hebt de URL van uw sleutelkluis nodig. Deze vindt u in de sectie **Essentials** van de pagina **Overzicht** van de sleutelkluis.  
    
    ```powershell
    (Invoke-WebRequest -Uri https://<your-key-vault-URL>/secrets/<secret-name>?api-version=2016-10-01 -Method GET -Headers @{Authorization="Bearer $KeyVaultToken"}).content 
    ```
    
    De reactie ziet er ongeveer als volgt uit: 
    
    ```powershell
    {"value":"p@ssw0rd!","id":"https://mytestkeyvault.vault.azure.net/secrets/MyTestSecret/7c2204c6093c4d859bc5b9eff8f29050","attributes":{"enabled":true,"created":1505088747,"updated":1505088747,"recoveryLevel":"Purgeable"}} 
    ```
    
Zodra u het geheim hebt opgehaald uit de sleutelkluis, kunt u deze gebruiken om te verifiëren bij een service die een gebruikersnaam en wachtwoord vereist. 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u geleerd een Managed Service Identity te maken om toegang tot Azure Key Vault te krijgen.  Zie voor meer informatie over Azure Key Vault:

> [!div class="nextstepaction"]
>[Azure Key Vault](/azure/key-vault/key-vault-whatis)

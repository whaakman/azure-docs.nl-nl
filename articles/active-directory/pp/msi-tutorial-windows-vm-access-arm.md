---
title: Gebruik van een Windows-VM-MSI voor toegang tot Azure Resource Manager
description: Een zelfstudie die u bij het proces helpt van het gebruik van een Windows VM beheerde Service identiteit (MSI) voor toegang tot Azure Resource Manager.
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
ms.openlocfilehash: 8a717943239594886b2132e88efbd1792389309f
ms.sourcegitcommit: a648f9d7a502bfbab4cd89c9e25aa03d1a0c412b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2017
---
# <a name="use-a-windows-vm-managed-service-identity-msi-to-access-resource-manager"></a>Een Windows VM beheerde Service identiteit (MSI) gebruiken voor toegang tot de Resource Manager

[!INCLUDE[preview-notice](~/includes/active-directory-msi-preview-notice-ua.md)]

Deze zelfstudie laat zien hoe u beheerde Service identiteit (MSI) inschakelen voor virtuele Windows-machine (VM). U kunt vervolgens die identiteit gebruiken voor toegang tot de Azure Resource Manager-API. Beheerde Service-identiteiten worden automatisch beheerd door Azure en u te verifiëren bij services die ondersteuning bieden voor Azure AD-verificatie zonder referenties invoegen in uw code. Procedures voor:

> [!div class="checklist"]
> * Inschakelen van MSI op een Windows VM 
> * Uw VM-toegang verlenen aan een resourcegroep in Azure Resource Manager 
> * Ophalen van een toegangstoken met behulp van de identiteit van de virtuele machine en het aanroepen van Azure Resource Manager

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [msi-core-prereqs](~/includes/active-directory-msi-core-prereqs-ua.md)]

[!INCLUDE [msi-tut-prereqs](~/includes/active-directory-msi-tut-prereqs.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure
Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-a-windows-virtual-machine-in-a-new-resource-group"></a>Een virtuele Windows-machine in een nieuwe resourcegroep maken

Voor deze zelfstudie maken we een nieuwe Windows VM.  U kunt ook MSI op een bestaande virtuele machine inschakelen.

1.  Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.
2.  Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 
3.  Geef de informatie van de virtuele machine op. De **gebruikersnaam** en **wachtwoord** gemaakte Hier ziet u de referenties die u kunt aanmelden bij de virtuele machine.
4.  Kies de juiste **abonnement** voor de virtuele machine in de vervolgkeuzelijst.
5.  Selecteer een nieuwe **resourcegroep** waarin u uw virtuele machine maakt, kiest u **nieuw**. Na het voltooien klikt u op **OK**.
6.  Selecteer de grootte van de virtuele machine. Kies om meer groottes weer te geven de optie **Alle weergeven** of wijzig het filter **Ondersteund schijftype**. Behoud de standaardinstellingen op de pagina instellingen en klik op **OK**.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-vm.png)

## <a name="enable-msi-on-your-vm"></a>MSI op de virtuele machine inschakelen 

Een VM MSI kunt u toegangstokens ophalen uit Azure AD zonder dat u referenties in uw code te plaatsen. Inschakelen van MSI vertelt Azure maken van een beheerde identiteit voor uw virtuele machine. Achter de MSI inschakelen biedt twee dingen: het installeren van de MSI-VM-extensie op uw virtuele machine en zorgt ervoor dat MSI in Azure Resource Manager.

1.  Selecteer de **virtuele Machine** dat u inschakelen van MSI wilt op.  
2.  Klik op de linkernavigatiebalk **configuratie**. 
3.  U ziet **beheerde Service-identiteit**. Als u wilt registreren en inschakelen van het MSI-bestand, selecteer **Ja**, als u wilt uitschakelen, kiest u Nee. 
4.  Zorg ervoor dat u klikt op **opslaan** aan de configuratie op te slaan.  
    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-linux-vm-access-arm/msi-linux-extension.png)

5. Als u wilt controleren en nagaan welke uitbreidingen zijn op deze virtuele machine, klikt u op **extensies**. Als MSI is ingeschakeld, klikt u vervolgens **ManagedIdentityExtensionforWindows** worden weergegeven in de lijst.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-extension.png)

## <a name="grant-your-vm-access-to-a-resource-group-in-resource-manager"></a>Uw VM-toegang verlenen aan een resourcegroep in Resource Manager
Met behulp van MSI Vind uw code toegangstokens voor verificatie naar bronnen die ondersteuning bieden voor Azure AD-verificatie.  De Azure Resource Manager biedt ondersteuning voor Azure AD-verificatie.  Eerst moet deze VM identiteit toegang verlenen tot een resource in Resource Manager in dit geval de resourcegroep waarin de virtuele machine is opgenomen.  

1.  Navigeer naar het tabblad voor **resourcegroepen**. 
2.  Selecteer de specifieke **resourcegroep** u hebt gemaakt voor uw **Windows VM**. 
3.  Ga naar **toegangsbeheer (IAM)** in het linkerdeelvenster. 
4.  Vervolgens **toevoegen** een nieuwe roltoewijzing voor uw **Windows VM**.  Kies **rol** als **lezer**. 
5.  In de volgende vervolgkeuzelijst **toewijzen van toegang tot** de resource **virtuele Machine**. 
6.  Controleer vervolgens het juiste abonnement wordt vermeld in de **abonnement** vervolgkeuzelijst. En voor **resourcegroep**, selecteer **alle resourcegroepen**. 
7.  Ten slotte in **Selecteer** Kies uw virtuele machine van Windows in de vervolgkeuzelijst en klik op **opslaan**.

    ![De installatiekopie van de alternatieve tekst](~/articles/active-directory/media/msi-tutorial-windows-vm-access-arm/msi-windows-permissions.png)

## <a name="get-an-access-token-using-the-vm-identity-and-use-it-to-call-azure-resource-manager"></a>Ophalen van een toegangstoken met behulp van de identiteit van de virtuele machine en het aanroepen van Azure Resource Manager 

U moet gebruiken **PowerShell** in dit gedeelte.  Als u niet hebt geïnstalleerd, downloadt u dit [hier](https://docs.microsoft.com/powershell/azure/overview?view=azurermps-4.3.1). 

1.  Navigeer in de portal naar **virtuele Machines** en gaat u naar uw Windows-machine en in de **overzicht**, klikt u op **Connect**. 
2.  Voer in uw **gebruikersnaam** en **wachtwoord** voor die u hebt toegevoegd tijdens het maken van de virtuele machine van Windows. 
3.  Nu dat u hebt gemaakt een **verbinding met extern bureaublad** openen met de virtuele machine, **PowerShell** in de externe sessie. 
4.  Maak met behulp van Powershell Invoke-WebRequest, een aanvraag naar het lokale eindpunt MSI een access-token ophalen voor Azure Resource Manager.

    ```powershell
       $response = Invoke-WebRequest -Uri http://localhost:50342/oauth2/token -Method GET -Body @{resource="https://management.azure.com/"} -Headers @{Metadata="true"}
    ```
    
    > [!NOTE]
    > De waarde van de resourceparameter '' moet een exacte overeenkomst voor door Azure AD wordt verwacht. Wanneer u de resource-ID van Azure Resource Manager gebruikt, moet u de afsluitende slash op de URI opnemen.
    
    Pak vervolgens het volledige antwoord, die wordt opgeslagen als een tekenreeks in JSON JavaScript Object Notation () geformatteerd in de $response-object. 
    
    ```powershell
    $content = $response.Content | ConvertFrom-Json
    ```
    Haal het toegangstoken vervolgens uit het antwoord.
    
    ```powershell
    $ArmToken = $content.access_token
    ```
    
    Ten slotte Azure Resource Manager met behulp van het toegangstoken aanroepen. In dit voorbeeld we ook maken gebruik van de PowerShell Invoke-WebRequest te maken van de aanroep naar Azure Resource Manager en het toegangstoken opnemen in de autorisatie-header.
    
    ```powershell
    (Invoke-WebRequest -Uri https://management.azure.com/subscriptions/<SUBSCRIPTION ID>/resourceGroups/<RESOURCE GROUP>?api-version=2016-06-01 -Method GET -ContentType "application/json" -Headers @{ Authorization ="Bearer $ArmToken"}).content
    ```
    > [!NOTE] 
    > De URL is hoofdlettergevoelig, dus zorg ervoor als u de dezelfde hoofdletters gebruikt als u eerder gebruikt wanneer u met de naam de resourcegroep en de hoofdletters 'G' in "resourceGroups."
        
    De volgende opdracht retourneert de details van de resourcegroep:

    ```powershell
    {"id":"/subscriptions/98f51385-2edc-4b79-bed9-7718de4cb861/resourceGroups/DevTest","name":"DevTest","location":"westus","properties":{"provisioningState":"Succeeded"}}
    ```

## <a name="related-content"></a>Gerelateerde inhoud

- Zie voor een overzicht van MSI [overzicht van de Service-identiteit beheerd](msi-overview.md).

Gebruik de volgende sectie met opmerkingen uw feedback en help ons verfijnen en onze content vorm.


---
title: 'Zelfstudie voor het bulksgewijs uitnodigen van gebruikers van B2B-samenwerking: Azure Active Directory | Microsoft Docs'
description: In deze zelfstudie leert u hoe u PowerShell en een CSV-bestand gebruikt voor het verzenden van bulk-uitnodigingen naar externe gebruikers van de Microsoft Azure AD B2B-samenwerking.
services: active-directory
ms.service: active-directory
ms.subservice: B2B
ms.topic: tutorial
ms.date: 08/14/2018
ms.author: mimart
author: msmimart
manager: celestedg
ms.reviewer: mal
ms.collection: M365-identity-device-management
ms.openlocfilehash: 1f91d51bda4c41a314c7cc1c2cc895e36bde0500
ms.sourcegitcommit: 36c50860e75d86f0d0e2be9e3213ffa9a06f4150
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2019
ms.locfileid: "65768212"
---
# <a name="tutorial-bulk-invite-azure-ad-b2b-collaboration-users"></a>Zelfstudie: Bulksgewijs gebruikers uitnodigen voor Microsoft Azure AD B2B-samenwerking

Als u Azure Active Directory (Azure AD) B2B-samenwerking gebruikt om te werken met externe partners, kunt u tegelijkertijd meerdere gastgebruikers uitnodigen voor uw organisatie. In deze zelfstudie leert u hoe u PowerShell gebruikt voor het verzenden van bulk-uitnodigingen naar externe gebruikers. Ga als volgt te werk:

> [!div class="checklist"]
> * Bereid een bestand met door komma's gescheiden waarden (.csv) voor waarin u de gebruikersinformatie opneemt
> * Voer een PowerShell-script uit voor het verzenden van uitnodigingen
> * Controleer of de gebruikers zijn toegevoegd aan de map

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint. 

## <a name="prerequisites"></a>Vereisten

### <a name="install-the-latest-azureadpreview-module"></a>Installeer de nieuwste versie van de AzureADPreview-module
Zorg ervoor dat u de nieuwste versie van de Microsoft Azure AD PowerShell voor Graph-module (AzureADPreview) installeert. 

Controleer eerst welke modules die u hebt geïnstalleerd. Open PowerShell als een gebruiker met verhoogde bevoegdheid (Uitvoeren als administrator) en voer de volgende opdracht uit:
 
```powershell  
Get-Module -ListAvailable AzureAD*
```

Voer, op basis van de uitvoer, een van de volgende handelingen uit:

- Als er geen resultaten worden geretourneerd, voert u de volgende opdracht uit om de AzureADPreview-module te installeren:
  
   ```powershell  
   Install-Module AzureADPreview
   ```
- Als alleen de AzureAD-module wordt weergegeven in de resultaten, voert u de volgende opdrachten uit om de AzureADPreview-module te installeren: 

   ```powershell 
   Uninstall-Module AzureAD 
   Install-Module AzureADPreview 
   ```
- Als alleen de AzureADPreview-module wordt weergegeven in de resultaten, maar u een bericht ontvangt dat aangeeft dat er een latere versie is, voert u de volgende opdrachten uit om de module bij te werken: 

   ```powershell 
   Uninstall-Module AzureADPreview 
   Install-Module AzureADPreview 
  ```

Mogelijk krijgt u een opdrachtprompt dat u de module vanuit een niet-vertrouwde opslagplaats installeert. Dit gebeurt als u de opslagplaats PSGallery eerder niet hebt ingesteld als een vertrouwde opslagplaats. Druk op **Y** om de module te installeren.

### <a name="get-test-email-accounts"></a>Test-e-mailaccounts ophalen

U moet twee of meer test e-mailaccounts hebben waarnaar u uitnodigingen kunt verzenden. De accounts moet van buiten uw organisatie zijn. U kunt elk type account gebruiken, met inbegrip van sociale accounts, bijvoorbeeld met een adres van gmail.com of outlook.com.

## <a name="prepare-the-csv-file"></a>Het CSV-bestand voorbereiden

Maak in Microsoft Excel een CSV-bestand met de lijst van gebruikersnamen en e-mailadressen van de mensen die u wilt uitnodigen. Vergeet niet de kolomkoppen **Naam** en **InvitedUserEmailAddress** op te nemen. 

Maak bijvoorbeeld een werkblad in de volgende indeling:


![PowerShell-uitvoer die wacht op acceptatie door de gebruiker](media/tutorial-bulk-invite/AddUsersExcel.png)

Sla het bestand op als **C:\BulkInvite\Invitations.csv**. 

Als u niet beschikt over Excel, kunt u een CSV-bestand maken in een teksteditor zoals Kladblok. Scheid de waarden met een komma, en plaats elke rij op een nieuwe regel. 

## <a name="sign-in-to-your-tenant"></a>Aanmelden bij uw tenant

Voer de volgende opdracht uit om verbinding te maken met het tenantdomein:

```powershell
Connect-AzureAD -TenantDomain "<Tenant_Domain_Name>"
```
Bijvoorbeeld `Connect-AzureAD -TenantDomain "contoso.onmicrosoft.com"`.

Voer uw referenties in wanneer dit wordt gevraagd.

## <a name="send-bulk-invitations"></a>Bulk-uitnodigingen verzenden

Verzend de uitnodigingen met het volgende PowerShell-script (waarbij **c:\bulkinvite\invitations.csv** het pad van het CSV-bestandis): 

```powershell
$invitations = import-csv c:\bulkinvite\invitations.csv
   
$messageInfo = New-Object Microsoft.Open.MSGraph.Model.InvitedUserMessageInfo
   
$messageInfo.customizedMessageBody = "Hello. You are invited to the Contoso organization."
   
foreach ($email in $invitations) 
   {New-AzureADMSInvitation `
      -InvitedUserEmailAddress $email.InvitedUserEmailAddress `
      -InvitedUserDisplayName $email.Name `
      -InviteRedirectUrl https://myapps.microsoft.com `
      -InvitedUserMessageInfo $messageInfo `
      -SendInvitationMessage $true
   }
```
Dit script stuurt een uitnodiging naar de e-mailadressen in het bestand Invitations.csv. De uitvoer ziet er ongeveer als volgt uit voor elke gebruiker:

![PowerShell-uitvoer die wacht op acceptatie door de gebruiker](media/tutorial-bulk-invite/B2BBulkImport.png)

## <a name="verify-users-exist-in-the-directory"></a>Controleer of de gebruikers bestaan in de map

Controleer of de uitgenodigde gebruikers zijn toegevoegd aan Microsoft Azure AD door de volgende opdracht uit te voeren:
```powershell
 Get-AzureADUser -Filter "UserType eq 'Guest'"
```
U ziet dat de gebruikers die u hebt uitgenodigd weergegeven, met een user principal name (UPN) in de indeling *emailaddress*EXT #\@*domein*. Bijvoorbeeld, *lstokes_fabrikam.com#EXT#\@contoso.onmicrosoft.com*, waarbij contoso.onmicrosoft.com staat voor de organisatie waarvan u de uitnodigingen verzonden.

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u de test-gebruikersaccounts niet meer nodig hebt, kunt u deze uit de map verwijderen. Voer de volgende opdracht uit om een gebruikersaccount te verwijderen:

```powershell
 Remove-AzureADUser -ObjectId "<UPN>"
```
Bijvoorbeeld: `Remove-AzureADUser -ObjectId "lstokes_fabrikam.com#EXT#@contoso.onmicrosoft.com"`


## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u bulk-uitnodigingen naar gastgebruikers buiten uw organisatie verzonden. Vervolgens leert u hoe het proces voor uitnodiging inwisselen werkt.

> [!div class="nextstepaction"]
> [Meer informatie over het proces voor uitnodiging inwisselen in Microsoft Azure AD B2B-samenwerking](redemption-experience.md)


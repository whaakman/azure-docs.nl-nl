---
title: Richtlijnen voor Azure Marketplace en AppSource publisher | Azure
description: Richtlijnen voor Azure Marketplace en AppSource voor uitgevers van de app en service
services: Azure, Marketplace, Compute, Storage, Networking, Blockchain, Security
documentationcenter: ''
author: jm-aditi-ms
manager: pabutler
editor: ''
ms.assetid: ''
ms.service: marketplace
ms.workload: ''
ms.tgt_pltfrm: ''
ms.devlang: ''
ms.topic: article
ms.date: 06/13/2018
ms.author: ellacroi
ms.openlocfilehash: 4da6f5c4513477d1adbf4d7645a66de112eeab23
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36307482"
---
# <a name="guidelines"></a>Richtlijnen  

<!--
## Guidelines for AppSource  
-->
---  

## <a name="guidelines-for-azure-marketplace"></a>Richtlijnen voor Azure Marketplace  

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-a-marketplace-account"></a>Richtlijnen voor het maken van een Microsoft-ID voor het beheren van een marketplace-account  
Als meer dan één persoon toegang tot de Microsoft-ID gebruikt om uw marketplace-account te maken vereist, moet u deze richtlijnen om u bij het maken van een bedrijfsaccount te volgen. 

>[!IMPORTANT]
>Om meerdere gebruikers toegang tot uw account Microsoft Developer Center (Dev Center), wordt aangeraden dat u Azure Active Directory (Azure AD) voor het toewijzen van rollen naar afzonderlijke gebruikers. Elke gebruiker toegang moet hebben tot het account door aanmelden met afzonderlijke Azure AD-referenties. Uw Microsoft-ID maken met behulp van een e-mailadres in een domein geregistreerd bij uw bedrijf Microsoft raadt het e-mailbericht niet worden toegewezen aan een persoon. Een voorbeeld is `windowsapps@fabrikam.com`.  
>*   Voor meer informatie gaat u naar de [probleem: Microsoft-ID in een Azure AD domain federatieve](#issue-microsoft-id-in-an-azure-ad-federated-domain) sectie.  

*   Beperk de toegang op de Microsoft-ID op het kleinste aantal ontwikkelaars. 
*   Instellen van een zakelijke e-mail distributielijst (DL) met iedereen die toegang moet hebben tot uw account Dev Center. Het e-mailadres DL toevoegen aan je beveiligingsgegevens. De DL kan alle werknemers in de lijst voor het ontvangen van beveiligingscodes wanneer dit wordt aangevraagd en voor het beheren van de beveiligingsgegevens voor uw Microsoft-ID. Als het instellen van een distributielijst, is niet haalbaar, moet de eigenaar van de afzonderlijke e-mailaccount voor toegang tot en het delen van de beveiligingscode desgevraagd beschikbaar zijn.  
    *   De eigenaar wordt bijvoorbeeld gevraagd wanneer nieuwe beveiligingsgegevens is toegevoegd aan de Microsoft-ID of wanneer de Microsoft-ID wordt geopend vanaf een nieuw apparaat.  
*   Het telefoonnummer van een bedrijf die geen extensie vereist en toegankelijk is voor de sleutel teamleden toevoegen.  
*   In het algemeen moet u ontwikkelaars vertrouwde apparaten te gebruiken om het aanmelden bij uw Developer Center-account instellen. Alle belangrijke teamleden moeten toegang hebben tot de vertrouwde apparaten. Vertrouwde apparaten gebruiken voor toegang tot beperkt de vereiste voor het verzenden van beveiligingscodes wanneer iemand toegang heeft tot de Dev Center-account.  
*   Als u vereist zijn om toegang te verlenen aan het Dev Center-account van een niet-vertrouwde computer, moet u toegang tot niet meer dan vijf ontwikkelaars beperken. De ontwikkelaars moeten het account in het ideale geval openen vanaf computers die delen dezelfde geografische en netwerklocatie.  
*   Regelmatig controleren en je beveiligingsgegevens controleren.  
    *   Je beveiligingsgegevens, Ga naar de instellingenpagina zich bevindt op beveiliging [account.live.com/proofs/Manage](https://account.live.com/proofs/Manage).

Dev Center-account moet voornamelijk van vertrouwde computers worden benaderd. Het is essentieel dat u toegang vertrouwde computers tot, omdat er een limiet aan het aantal codes die worden gegenereerd per Dev Center-account per week. Met behulp van vertrouwde computers, kunt ook de veiligste en consistente-aanmeldingservaring aanpast. 
*   Voor meer informatie over aanvullende richtlijnen voor Dev Center-account en beveiliging, gaat u naar het openen een pagina developer-account zich bevindt op [docs.microsoft.com/windows/uwp/publish/opening-a-developer-account](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account). 

---  

#### <a name="issue-microsoft-id-in-an-azure-ad-federated-domain"></a>Probleem: Microsoft-ID in een federatieve Azure AD-domein  
Uw bedrijfsaccount kan worden gefedereerd via Azure Active Directory (Azure AD). Als u probeert te maken van een Microsoft-ID met behulp van een zakelijke e-mailadres dat is gefedereerd met Azure AD, ontvangt u een fout opgetreden. Als u een foutbericht ontvangt, moet klikt u vervolgens u controleren met uw IT-team om te bevestigen dat je account via Azure AD is gefedereerd. Azure AD federatieve e-mailbericht is een bekend probleem en voor het oplossen van het Microsoft werkt.  
*   Voor meer informatie over Azure AD, gaat u naar de Azure Active Directory-documentatie pagina zich bevindt op [docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory).

Microsoft raadt u aan een tijdelijke oplossing. Volg deze stappen voor het maken van een nieuw e-mailadres in de `outlook.com` domein en maakt u een regel voor het doorsturen van uw communicatie.  
1.  Ga naar de pagina voor het account maken en klik op te halen van een nieuwe koppeling van e-mailadres. 
    *   Als u wilt zich registreert voor uw Microsoft-ID, gaat u naar de pagina van de maken zich bevindt op [signup.live.com/signup](https://signup.live.com/signup).  
2.  Maak het nieuwe e-mailadres en een wachtwoord invoeren. Een nieuwe Microsoft-ID en een postvak in de `outlook.com` domein wordt gemaakt. Het registratieproces blijven totdat het account is gemaakt.  

    >[!IMPORTANT]
    >U moet een lijst met e-mailadres of distributie die is geregistreerd als een Microsoft-ID registreren in Dev Center. Microsoft raadt aan dat u een distributielijst met de afhankelijkheid van personen te verwijderen. Als uw e-mailadres of distributie lijst niet is geregistreerd, moet vervolgens u registreren nu.    

    >[!Important]
    >Als je een e-mailadres bevindt zich in de `Microsoft` domein, de bedrijfsportal en u zich niet kunt gebruiken voor registratie in Dev Center.  

3.  Nadat u de Microsoft-ID hebt gemaakt met de Outlook-e-mailadres, meld u aan bij uw Outlook-postvak. Maak een regel voor doorsturen via e-mail. Het e-mailbericht doorsturen regel moet alle e-mailberichten die worden ontvangen in de Outlook-postvak naar e-mailadres in uw domein die u hebt gemaakt voor het beheren van uw marketplace-account te verplaatsen.  
    *   Als u wilt aanmelden bij uw Outlook-postvak, gaat u naar de Outlook-pagina zich bevindt op [outlook.live.com/owa](https://outlook.live.com/owa).  
    *   Voor meer informatie over het doorsturen van regels, gaat u naar de regels gebruiken in Outlook Web App voor het automatisch doorsturen van berichten naar een ander account zich bevindt op [support.office.com/article/ Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed](https://support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed).  

1.  De regel voor doorsturen verzendt alle e-mail en communicatie ontvangen in de Outlook-e-mailaccount naar e-mailadres in een domein dat is geregistreerd bij uw bedrijf. Uw `outlook.com` e-mailadres moet worden gebruikt voor verificatie in zowel Dev Center en Cloud Partner-Portal.  

## <a name="next-steps"></a>Volgende stappen
*   Ga naar de [AppSource Publisher handleiding en Azure Marketplace](./marketplace-publishers-guide.md) pagina.  
 
---  

---
title: Richtlijnen voor Azure Marketplace en AppSource publisher | Azure
description: Richtlijnen voor Azure Marketplace en AppSource voor uitgevers van app- en service
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
ms.openlocfilehash: a0ab88b1375f5178ca6f41689de8d2f30f8d7808
ms.sourcegitcommit: 90c6b63552f6b7f8efac7f5c375e77526841a678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2019
ms.locfileid: "56728128"
---
# <a name="guidelines"></a>Richtlijnen  

<!--
## Guidelines for AppSource  
-->
---

## <a name="guidelines-for-azure-marketplace"></a>Richtlijnen voor Azure Marketplace  

### <a name="guidelines-for-creating-a-microsoft-id-to-manage-a-marketplace-account"></a>Richtlijnen voor het maken van een Microsoft-ID voor het beheren van een marketplace-account  
Als meer dan één persoon toegang tot de Microsoft-ID gebruikt om uw marketplace-account te maken heeft, moet u deze richtlijnen voor het maken van een bedrijfsaccount volgen. 

>[!IMPORTANT]
>Als u wilt toestaan dat meerdere gebruikers toegang tot uw account Microsoft Developer Center (Dev Center), wordt aangeraden gebruik te maken van Azure Active Directory (Azure AD) het toewijzen van rollen naar afzonderlijke gebruikers. Elke gebruiker moet toegang tot het account door aanmelden met afzonderlijke Azure AD-referenties. Uw Microsoft-ID maken met behulp van een e-mailadres in een domein geregistreerd voor uw bedrijf Microsoft stelt dat het e-mailbericht niet worden toegewezen aan een persoon. Een voorbeeld is `windowsapps@fabrikam.com`.  
>*   Voor meer informatie gaat u naar de [probleem: Microsoft-ID in een Azure AD federatief domein](#issue-microsoft-id-in-an-azure-ad-federated-domain) sectie.  

*   Toegang beperken tot de Microsoft-ID voor het kleinste aantal ontwikkelaars. 
*   Instellen van een zakelijke e-mail distributielijst (DL) met iedereen die toegang moet hebben tot uw Dev Center-account. Het e-mailadres van uw Distributielijst toevoegen aan uw beveiligingsgegevens. De DL kan alle van de werknemers in de lijst voor het ontvangen van beveiligingscodes wanneer aangevraagd en voor het beheren van de beveiligingsgegevens voor je Microsoft-ID. Als het instellen van een distributielijst niet haalbaar is is, moet klikt u vervolgens de eigenaar van het afzonderlijke e-mailaccount beschikbaar zijn voor toegang tot en het delen van de beveiligingscode wanneer hierom wordt gevraagd.  
    *   Bijvoorbeeld, de eigenaar van de gevraagd wanneer nieuwe informatie over de beveiliging wordt toegevoegd aan de Microsoft-ID of wanneer de Microsoft-ID van een nieuw apparaat wordt geopend.  
*   Het telefoonnummer van een bedrijf dat niet hoeft een uitbreiding en toegankelijk is voor belangrijke teamleden toevoegen.  
*   In het algemeen moet u ontwikkelaars gebruikmaken van vertrouwde apparaten aan te melden bij uw Dev Center-account instellen. Alle belangrijke teamleden moeten hebben toegang tot de vertrouwde apparaten. Vertrouwde apparaten gebruiken voor toegang tot beperkt de vereiste voor het verzenden van beveiligingscodes wanneer iemand toegang heeft tot het Dev Center-account.  
*   Als u vereist zijn om toegang te verlenen aan het Dev Center-account van een niet-vertrouwde computer, moet u toegang tot niet meer dan vijf ontwikkelaars beperken. Uw ontwikkelaars moeten in het ideale geval het account kunt openen vanaf computers die delen dezelfde geografische locatie van het netwerk.  
*   Regelmatig controleren en verifiëren van uw beveiligingsgegevens.  
    *   Als u wilt uw beveiligingsgegevens weergeven, gaat u naar de instellingenpagina te vinden op beveiliging [account.live.com/proofs/Manage](https://account.live.com/proofs/Manage).

Uw Dev Center-account moet worden voornamelijk van vertrouwde computers zijn geopend. Het is essentieel dat u toegang krijgen vertrouwde computers tot, omdat er een limiet aan het aantal codes dat per Dev Center-account per week wordt gegenereerd geldt. Met behulp van vertrouwde computers, kunt ook de veiligste en consistente aanmeldingservaring. 
*   Voor meer informatie over aanvullende richtlijnen voor Dev Center-account en beveiliging, gaat u naar het openen een developer-account-pagina te vinden op [docs.microsoft.com/windows/uwp/publish/opening-a-developer-account](https://docs.microsoft.com/windows/uwp/publish/opening-a-developer-account). 

---

#### <a name="issue-microsoft-id-in-an-azure-ad-federated-domain"></a>Probleem: Microsoft-ID in een federatief domein van Azure AD  
Uw zakelijke account kan worden gefedereerd via Azure Active Directory (Azure AD). Als u probeert te maken van een Microsoft-ID met behulp van een zakelijke e-mailadres dat is gefedereerd met Azure AD, ontvangt u een fout. Als u een foutbericht ontvangt, neemt klikt u vervolgens u contact met uw IT-team om te bevestigen van dat uw account via Azure AD is gefedereerd. Azure AD federatieve e-mailadres is een bekend probleem en Microsoft is bezig met het oplossen van deze.  
*   Voor meer informatie over Azure AD, gaat u naar de Azure Active Directory-documentatie pagina zich bevindt op [docs.microsoft.com/azure/active-directory](https://docs.microsoft.com/azure/active-directory).

Microsoft raadt aan een tijdelijke oplossing. Volg deze stappen voor het maken van een nieuw e-mailadres in de `outlook.com` domein en maak een regel voor het doorsturen van uw communicatie.  
1.  Ga naar de pagina voor het account maken en klik op het ophalen van een nieuwe koppeling voor e-mailadres. 
    *   Als u zich voor uw Microsoft-ID, gaat u naar de pagina voor het account maken dat zich bevindt in [signup.live.com/signup](https://signup.live.com/signup).  
2.  Maak het nieuwe e-mailadres en een wachtwoord invoeren. Een nieuwe Microsoft-ID en een e-postvak in de `outlook.com` domein wordt gemaakt. Het registratieproces verder voordat het account is gemaakt.  

    >[!IMPORTANT]
    >U moet een lijst met e-adres of de distributie die is geregistreerd als een Microsoft-ID om in Dev Center te registreren. Microsoft raadt aan dat u een distributielijst gebruiken om te verwijderen van afhankelijkheid van personen. Als uw lijst met e-adres of de distributie niet is geregistreerd, moet klikt u vervolgens u registreren nu.    

    >[!Important]
    >Als je een e-mailadres bevindt zich in de `Microsoft` bedrijf domein, en vervolgens kunt u ze niet te gebruiken voor registratie in Dev Center.  

3.  Nadat u de Microsoft-ID met de Outlook-e-mailadres gemaakt, meld u aan bij uw Outlook-postvak. Maak een regel voor doorsturen via e-mail. Het e-mailbericht doorsturen van de regel moet alle e-mailberichten die worden ontvangen in de Outlook-postvak naar de e-mailadres in uw domein dat u hebt gemaakt voor het beheren van uw marketplace-account verplaatsen.  
    *   Als u wilt aanmelden bij uw Outlook-postvak, gaat u naar de Outlook-pagina te vinden op [outlook.live.com/owa](https://outlook.live.com/owa).  
    *   Voor meer informatie over de regels voor doorsturen, gaat u naar de regels gebruiken in Outlook Web-App automatisch doorsturen van berichten naar een andere accountpagina te vinden op [support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed](https://support.office.com/article/Use-rules-in-Outlook-Web-App-to-automatically-forward-messages-to-another-account-1433e3a0-7fb0-4999-b536-50e05cb67fed).  

1.  De regel voor doorsturen verzendt alle e-mail en -communicatie ontvangen in de Outlook-e-mailaccount op het e-mailadres in een domein dat is geregistreerd bij uw bedrijf. Uw `outlook.com` e-mailadres moet worden gebruikt om te verifiëren in zowel de Dev Center en de Cloud Partner-Portal.  

## <a name="next-steps"></a>Volgende stappen
*   Ga naar de [Azure Marketplace en AppSource Uitgevershandleiding voor](./marketplace-publishers-guide.md) pagina.  
 
---

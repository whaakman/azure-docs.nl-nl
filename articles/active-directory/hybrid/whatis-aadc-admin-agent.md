---
title: Wat is de Azure AD Connect-beheerder Agent - Azure AD Connect | Microsoft Docs
description: Hier worden de hulpprogramma's beschreven die worden gebruikt om uw on-premises omgeving met Azure AD te synchroniseren en bewaken.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 04/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 36ab3fff4294b4cda3d1554ef2761d3f4acaca35
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64687225"
---
# <a name="what-is-the-azure-ad-connect-admin-agent"></a>Wat is de Azure AD Connect-beheerderagent? 
De Azure AD Connect beheer-Agent is een nieuw onderdeel van Azure Active Directory Connect die kan worden geïnstalleerd op een Azure Active Directory Connect-server. Deze wordt gebruikt om specifieke gegevens verzamelen over uw Active Directory-omgeving waarmee u een Microsoft ondersteuningstechnicus kunt oplossen van problemen wanneer u een ondersteuningsaanvraag openen. 

>[!NOTE]
>De beheerder-agent is niet geïnstalleerd en is standaard ingeschakeld.  U moet de agent installeren om het verzamelen van gegevens om te helpen met kwesties.

Wanneer geïnstalleerd, ondersteuning de Azure AD Connect beheer Agent wacht op specifieke aanvragen voor gegevens uit Azure Active Directory, de aangevraagde gegevens worden opgehaald uit de synchronisatie-omgeving en verzendt ze naar Azure Active Directory, waar deze wordt weergegeven aan het Microsoft engineer. 

De informatie die de Azure AD Connect beheer-Agent wordt opgehaald uit uw omgeving wordt niet opgeslagen op geen enkele manier: dit wordt alleen weergegeven voor de ondersteuningstechnicus van Microsoft om u te helpen bij het onderzoeken en het oplossen van de Azure Active Directory Connect gerelateerde ondersteuningsaanvraag dat u de Azure AD Connect-beheer Agent geopend is niet standaard geïnstalleerd op de Azure AD Connect-Server. 

## <a name="install-the-azure-ad-connect-administration-agent-on-the-azure-ad-connect-server"></a>De Azure AD Connect beheer-Agent installeren op de Azure AD Connect-server 
De Azure AD Connect beheer Agent binaire bestanden worden geplaatst in de AAD Connect-server. Als u wilt de agent hebt geïnstalleerd, het volgende doen: 



1. Open powershell in de beheermodus 
2. Navigeer naar de map waarin de toepassing zich bevindt cd "C:\Program Files\Microsoft Azure Active Directory Connect\SetupFiles" 
3. Voer de toepassing AADConnectAdminAgentSetup.exe 
 
Voer uw Azure AD-hoofdbeheerder referenties in wanneer hierom wordt gevraagd. 

>[!NOTE]
>Er is een bekend probleem waarbij u wordt gevraagd uw referenties meerdere keren. Dit wordt opgelost in de volgende release.

Nadat de agent is geïnstalleerd, ziet u de volgende twee nieuwe programma's in de lijst 'Programma's toevoegen/verwijderen' in het Configuratiescherm van de server: 

![beheerder-agent](media/whatis-aadc-admin-agent/adminagent1.png)

## <a name="what-data-in-my-sync-service-is-shown-to-the-microsoft-service-engineer"></a>Welke gegevens in mijn Sync-service wordt weergegeven aan de Microsoft-technicus? 
Wanneer u een ondersteuningsaanvraag openen kunt de ondersteuningstechnicus van Microsoft zien, voor een bepaalde gebruiker, de relevante gegevens in Active Directory, de Active Directory-connectorgebied in de Azure Active Directory Connect-server, de Azure Active Directory-connectorgebied in Azure Active Directory Connect-server en de Metaverse in de Azure Active Directory Connect-server. 

De ondersteuningstechnicus van Microsoft kan gegevens niet wijzigen in uw systeem en de wachtwoorden niet kan zien. 

## <a name="what-if-i-dont-want-the-microsoft-support-engineer-to-access-my-data"></a>Wat gebeurt er als ik wil niet de ondersteuningstechnicus van Microsoft voor toegang tot mijn gegevens? 
Nadat de agent is geïnstalleerd, als u niet wilt dat de Microsoft service engineer toegang tot uw gegevens voor een telefoongesprek ondersteuning, kunt u de functionaliteit uitschakelen door het wijzigen van het configuratiebestand van de service, zoals hieronder wordt beschreven: 

1.  Open **C:\Program Files\Microsoft Azure AD Connect beheer Agent\AzureADConnectAdministrationAgentService.exe.config** in Kladblok.
2.  Uitschakelen **UserDataEnabled** instellen zoals hieronder wordt weergegeven. Als **UserDataEnabled** instelling bestaat en is ingesteld op true, wordt deze ingesteld op false. Als de instelling niet bestaat nog, voegt u de instelling zoals hieronder wordt weergegeven.    

    ```xml
    <appSettings>
      <add key="TraceFilename" value="ADAdministrationAgent.log" />
      <add key="UserDataEnabled" value="false" />
    </appSettings>
    ```

3.  Sla het configuratiebestand.
4.  Azure AD Connect beheer Agent-service opnieuw starten, zoals hieronder wordt weergegeven

![beheerder-agent](media/whatis-aadc-admin-agent/adminagent2.png)

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).

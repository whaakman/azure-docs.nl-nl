---
title: Verwijder persoonsgegevens - Azure Active Directory-toepassingsproxy | Microsoft Docs
description: Persoonlijke gegevens verwijderen van connectors die zijn geïnstalleerd op apparaten voor Azure Active Directory-toepassingsproxy.
documentationcenter: ''
author: barbkess
manager: mtillman
ms.service: active-directory
ms.component: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/21/2018
ms.author: barbkess
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 94cf0464aca3c46e0c6425b0fb3e24fcd767f95c
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34654903"
---
# <a name="remove-personal-data-for-azure-active-directory-application-proxy"></a>Persoonlijke gegevens voor Azure Active Directory-toepassingsproxy verwijderen  

Azure Active Directory-toepassingsproxy vereist dat u connectors op uw apparaten installeert, wat betekent dat er is mogelijk persoonlijke gegevens op uw apparaten. Dit artikel bevat stappen voor het verwijderen van die persoonlijke gegevens ter verbetering van de privacy. 


## <a name="where-is-the-personal-data"></a>Waar is de persoonlijke gegevens?
Het is mogelijk voor toepassingsproxy persoonlijke gegevens naar de volgende typen van logboek schrijven:

- Connector-gebeurtenislogboeken
- Windows-gebeurtenislogboeken

## <a name="remove-personal-data-from-windows-event-logs"></a>Persoonlijke gegevens verwijderen uit de Windows-gebeurtenislogboeken

Zie voor meer informatie over het configureren van het bewaren van gegevens van de Windows-gebeurtenislogboeken [instellingen voor gebeurtenislogboeken](https://technet.microsoft.com/library/cc952132.aspx). Zie voor meer informatie over Windows-gebeurtenislogboeken, [Windows-gebeurtenislogboek](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="remove-personal-data-from-connector-event-logs"></a>Persoonlijke gegevens van gebeurtenislogboeken Connector verwijderen

Om te controleren of de logboeken van de toepassingsproxy hebben geen persoonlijke gegevens, kunt u:

- Verwijder of inzien wanneer deze nodig is, of
- Logboekregistratie uitschakelen

Gebruik de volgende secties persoonlijke gegevens verwijderen uit de gebeurtenislogboeken van de connector. U moet de verwijdering voltooien voor alle apparaten waarop de connector is geïnstalleerd.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-or-export-specific-data"></a>Weergeven of specifieke gegevens exporteren

Als u wilt weergeven of specifieke gegevens exporteren, zoeken naar gerelateerde vermeldingen in elk van de gebeurtenislogboeken van de connector. De logboeken bevinden zich op `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. 

Aangezien de logboekbestanden tekstbestanden zijn, kunt u [findstr](https://docs.microsoft.com/windows-server/administration/windows-commands/findstr) om te zoeken naar tekstvermeldingen die zijn gerelateerd aan een gebruiker.  

Persoonlijke om gegevens te zoeken, zoekt u logboekbestanden UserID. 

Ga voor persoonlijke gegevens die zijn geregistreerd door een toepassing die gebruikmaakt van Kerberos-beperkte overdracht zoeken voor deze onderdelen van het type gebruikersnaam:

- On-premises user principal name
- Het deel van de user principal name dat de gebruikersnaam omvat
- Het deel van de on-premises user principal name dat de gebruikersnaam omvat
- Lokale security accounts manager (SAM)-accountnaam 


### <a name="delete-specific-data"></a>Specifieke gegevens verwijderen

Specifieke om gegevens te verwijderen:

1. Start de service Microsoft Azure AD Connector voor toepassingsproxy voor het genereren van een nieuw logboekbestand. Het nieuwe logboekbestand kunt u te verwijderen of wijzigen van de oude logboekbestanden. 
2. Ga als volgt de [weergave of exporteren specifieke gegevens](#view-or-export-specific-data) proces eerder beschreven informatie te vinden die moet worden verwijderd. Zoeken in alle logboeken van de connector.
3. Verwijder de relevante logboekbestanden of selectief verwijderen van de velden die persoonlijke gegevens bevatten. U kunt ook alle oude logboekbestanden verwijderen als u ze niet meer nodig.

### <a name="turn-off-connector-logs"></a>Logboeken van de connector uitschakelen

Een optie om ervoor te zorgen logboeken van de connector bevatten geen persoonlijke gegevens is het genereren van de logboekbestanden uitschakelen. Als u wilt stoppen met het genereren van Logboeken van de connector, verwijdert u de volgende gemarkeerde regel uit `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config`. 

![Configuratie](./media/application-proxy-remove-personal-data/01.png)


## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van Application Proxy [het verstrekken van veilige externe toegang tot on-premises toepassingen](application-proxy.md).


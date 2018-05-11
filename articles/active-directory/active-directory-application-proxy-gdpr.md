---
title: GDPR in de Azure Active Directory-toepassingsproxy | Microsoft Docs
description: Meer informatie over GDPR in de Azure Active Directory-toepassingsproxy.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: mtillman
ms.assetid: c7186f98-dd80-4910-92a4-a7b8ff6272b9
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/16/2018
ms.author: markvi
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a3df15743b4918d72fac5f8769a2d3ee721a452f
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="gdpr-in-the-azure-active-directory-application-proxy"></a>GDPR in de Azure Active Directory-toepassingsproxy  

Azure Active Directory (Azure AD)-toepassingsproxy is compatibel met GDPR samen met andere Microsoft-services en functies. Zie voor meer informatie over de ondersteuning van Microsoft GDPR, [licentievoorwaarden en documentatie](http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31).

Omdat deze functie connectors op uw computers omvat, zijn er enkele gebeurtenissen die u nodig hebt om te controleren om te blijven voldoen aan het beleid GDPR. Toepassingsproxy maakt de volgende typen van het logboek, die kunnen EUII bevatten:

- Connector-gebeurtenislogboeken
- Windows-gebeurtenislogboeken

Er zijn twee manieren om te blijven voldoen aan het beleid GDPR:

- Verwijderen en het exporteren van aanvragen wanneer deze zich voordoen

- Logboekregistratie uitschakelen

Voor:

- Zie voor meer informatie over het bewaren van gegevens voor de Windows-gebeurtenislogboeken configureert [instellingen voor gebeurtenislogboeken](https://technet.microsoft.com/library/cc952132.aspx). 
- Algemene informatie over de Windows-gebeurtenislogboek Zie [Windows-gebeurtenislogboek](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).


U vindt de connect-gebeurtenislogboeken op `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`. De volgende secties bieden u de bijbehorende stappen van de gebeurtenislogboeken van de connector. U moet deze stappen uit op alle computers van de connector voltooien.
 

## <a name="processing-requests"></a>Het verwerken van aanvragen

Er zijn drie soorten aanvragen die u aansprakelijk zijn voor: 

- Verwijderen
- Weergave
- Exporteren
 
Voor het verwerken van de weergave / aanvragen exporteren, moet u elk van de logboekbestanden om te zoeken naar gerelateerde vermeldingen doorlopen. 

Omdat de logboekbestanden tekstbestanden zijn, u kunt zoeken op, bijvoorbeeld met behulp van de `findstr` opdracht om te bepalen van de vermeldingen die betrekking hebben op de gebruiker. Zoeken naar de volgende velden, omdat ze mogelijk in de logboeken: 

- UserId
- Het type gebruikersnaam is geconfigureerd voor toepassingen die gebruikmaken van Kerberos-beperkte overdracht:
    - User principal name
    - On-premises user principal name
    - Het deel van de user principal name dat de gebruikersnaam omvat
    - Het deel van de on-premises user principal name dat de gebruikersnaam omvat
    - Naam van het on-premises SAM-account 

 
U kunt verzamelen van deze velden en ze delen met de gebruiker.
Voor het verwerken van aanvragen voor het verwijderen moet u de logboeken van de relevante verwijderen. U kunt de connector-service (Microsoft Azure AD Connector voor toepassingsproxy) voor het genereren van een nieuw logboekbestand opnieuw. Het nieuwe logboekbestand kunt u de oude logboekbestanden verwijderen. U kunt Volg het proces voor weergave / exporteren om te zoeken naar alle relevante logboeken en de velden of bestanden selectief te verwijderen. U kunt alle oude logbestanden ook altijd alleen verwijderen als u ze niet meer nodig.


## <a name="turn-off-connector-logs"></a>Logboeken van de connector uitschakelen

Als u wilt uitschakelen Logboeken van de connector, die u wilt aanpassen `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config` door het verwijderen van de gemarkeerde regel: 


![Configuratie](./media/active-directory-application-proxy-gdpr/01.png)


## <a name="next-steps"></a>Volgende stappen

Zie voor een overzicht van de Azure AD-toepassingsproxy [het verstrekken van veilige externe toegang tot on-premises toepassingen](manage-apps/application-proxy.md).


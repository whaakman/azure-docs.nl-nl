---
title: Over het integreren van Azure Active Directory-logboeken met ArcSight met behulp van Azure Monitor (preview) | Microsoft Docs
description: Meer informatie over het integreren van Azure Active Directory-logboeken met ArcSight met behulp van Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: b37bef0d-982e-4e28-86b2-6c61ca524ae1
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 12/03/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 88979d58c78f3bc44c7fa96c65fb60c0fb69726b
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56175257"
---
# <a name="integrate-azure-active-directory-logs-with-arcsight-using-azure-monitor-preview"></a>Logboeken van Azure Active Directory integreren met ArcSight met behulp van Azure Monitor (preview)

[Micro Focus ArcSight](https://software.microfocus.com/products/siem-security-information-event-management/overview) is een beveiliging en -gebeurtenissen (SIEM) beheeroplossing waarmee u kunt detecteren en direct reageren op bedreigingen in uw platform. Nu kunt u logboeken van Azure Active Directory (Azure AD) doorsturen naar ArcSight met behulp van Azure Monitor met behulp van de connector ArcSight voor Azure AD. Deze functie kunt u voor het bewaken van uw tenant voor inbreuk op de beveiliging met behulp van ArcSight.  

In dit artikel leert u hoe u logboeken van Azure AD omleidt naar ArcSight met behulp van Azure Monitor. 

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze functie te gebruiken:
* Een Azure event hub met Azure AD-activiteit zich aanmeldt. Meer informatie over het [uw activiteitenlogboeken streamen naar een event hub streamen](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* Een geconfigureerde exemplaar van ArcSight Syslog-NG-Daemon SmartConnector (SmartConnector) of ArcSight Load Balancer. Als de gebeurtenissen worden verzonden naar ArcSight Load Balancer, worden ze als gevolg daarvan naar de SmartConnector verzonden door de Load Balancer.

Downloaden en open de [configuratiehandleiding voor ArcSight SmartConnector voor Azure Monitor Event Hub](https://community.softwaregrp.com/dcvta86296/attachments/dcvta86296/connector-documentation/1232/2/Microsoft%20Azure%20Monitor%20Event%20Hub.pdf). Deze handleiding bevat de stappen die u wilt installeren en configureren van de ArcSight SmartConnector voor Azure Monitor. 

## <a name="integrate-azure-ad-logs-with-arcsight"></a>Azure AD-logboeken integreren met ArcSight

1. Voer eerst de stappen in de **vereisten** sectie van de configuratiehandleiding. Deze sectie bevat de volgende stappen uit:
    * Gebruikersmachtigingen instellen in Azure, om ervoor te zorgen dat er een gebruiker met de **eigenaar** rol die u wilt implementeren en configureren van de connector.
    * Poorten openen op de server met Syslog NG-Daemon SmartConnector, zodat het toegankelijk vanaf Azure is. 
    * De implementatie wordt uitgevoerd van een Windows PowerShell-script, daarom moet u PowerShell scripts uit te voeren op de computer waar u naar het implementeren van de connector wilt inschakelen.

2. Volg de stappen in de **-Connector implementeren** sectie van de configuratiehandleiding voor de implementatie van de connector. In deze sectie helpt u bij het downloaden en uitpakken van de connector, eigenschappen voor de toepassing configureren en uitvoeren van het script voor implementatie van de uitgepakte map. 

3. Gebruik de stappen in de **controleren van de implementatie in Azure** om te controleren of de connector is ingesteld en naar behoren werkt. Controleer het volgende:
    * De vereiste Azure-functies worden gemaakt in uw Azure-abonnement.
    * De Azure AD-logboeken worden gestreamd naar de juiste bestemming. 
    * De instellingen van uw implementatie zijn opgeslagen in de toepassingsinstellingen in Azure functie-Apps. 
    * Een nieuwe resourcegroep voor ArcSight wordt gemaakt in Azure, met een Azure AD-toepassing voor de connector ArcSight en storage-accounts met de gekoppelde bestanden in CEF-indeling.

4. Ten slotte voltooid na de implementatie de stappen in de **na de implementatieconfiguraties** van de handleiding. In deze sectie wordt uitgelegd hoe u aanvullende configuratie uitvoeren als u van een App Service-Plan om te voorkomen dat de functie-apps niet-actieve na een time-outperiode gebruikmaakt, streaming van diagnostische logboeken van de event hub configureren en bijwerken van de SysLog-NG-Daemon Te koppelen aan de zojuist gemaakte opslagaccount SmartConnector keystore-certificaat.

5. De handleiding wordt ook uitgelegd over het aanpassen van de eigenschappen van de connector in Azure en hoe u bijwerkt en verwijdert de connector. Er is ook een sectie voor verbeterde prestaties, met inbegrip van een upgrade naar een [Azure verbruiksabonnement](https://azure.microsoft.com/pricing/details/functions) en het configureren van een ArcSight Load Balancer als de gebeurtenisbelasting groter is dan wat een enkel Syslog-NG-Daemon SmartConnector kan de ingang.

## <a name="next-steps"></a>Volgende stappen

* [Configuratiehandleiding voor ArcSight SmartConnector voor Azure Monitor Event Hub](https://community.softwaregrp.com/dcvta86296/attachments/dcvta86296/connector-documentation/1232/2/Microsoft%20Azure%20Monitor%20Event%20Hub.pdf)

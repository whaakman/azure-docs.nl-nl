---
title: Azure Active Directory-activiteitenlogboeken in Azure Monitor (preview) | Microsoft Docs
description: Inleiding tot Azure Active Directory-activiteit wordt geregistreerd in Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4b18127b-d1d0-4bdc-8f9c-6a4c991c5f75
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6cf9131fa5eb82fdd3248d78b1e6965cdc69cd3d
ms.sourcegitcommit: 1516779f1baffaedcd24c674ccddd3e95de844de
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/26/2019
ms.locfileid: "56816144"
---
# <a name="azure-ad-activity-logs-in-azure-monitor-preview"></a>Azure AD-activiteitenlogboeken in Azure Monitor (preview)

U kunt nu activiteitenlogboeken van Azure Active Directory (Azure AD) naar verschillende eindpunten voor de lange termijn bewaren en gegevens inzichten versturen. De openbare preview van Azure AD-Logboeken in Azure Monitor kunt u:

* Activiteitenlogboeken archiveren Azure AD naar Azure storage-account, de gegevens behouden gedurende een lange periode.
* Activiteitenlogboeken naar een Azure event hub voor analyses, met behulp van populaire Security Information and Event Management (SIEM) hulpprogramma's, zoals Splunk en QRadar Stream Azure AD.
* Azure AD integreren met uw eigen aangepaste logboek-oplossingen door ze te streamen naar een event hub-activiteitenlogboeken.
* Verzenden van Azure AD-activiteitenlogboeken naar Logboeken van Azure Monitor om in te schakelen uitgebreide visualisaties, bewaking en waarschuwingen voor de verbonden gegevens.

> [!VIDEO https://www.youtube.com/embed/syT-9KNfug8]

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="supported-reports"></a>Ondersteunde rapporten

U kunt versturen Azure AD controleren logboeken en aanmelden logboeken naar uw Azure storage-account, de event hub, de logboeken van Azure Monitor of de aangepaste oplossing met behulp van deze functie. 

* **Auditlogboeken**: De [activiteit audittrailrapporten](concept-audit-logs.md) krijgt u toegang tot de geschiedenis van elke taak die wordt uitgevoerd in uw tenant.
* **Meld u in logboeken**: Met de [rapport van aanmeldingsactiviteiten](concept-sign-ins.md), kunt u bepalen wie de taken die worden gerapporteerd in de auditlogboeken uitgevoerd.

> [!NOTE]
> Auditlogboeken en aanmeldingslogboeken met betrekking tot B2C worden momenteel niet ondersteund.
>

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze functie te gebruiken:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/).
* Een [licentie](https://azure.microsoft.com/pricing/details/active-directory/) voor Azure AD Free, Basic, Premium 1 of Premium 2 voor toegang tot de Azure AD-controlelogboeken in Azure Portal. 
* Een Azure AD-tenant.
* Een gebruiker die een **globale beheerder** of **beveiligingsbeheerder** voor de Azure-tenant is.
* Een [licentie](https://azure.microsoft.com/pricing/details/active-directory/) voor Azure AD Premium 1 of Premium 2 voor toegang tot de Azure AD-aanmeldingslogboeken in Azure Portal. 

Afhankelijk van waarnaar u uw auditlogboekgegevens wilt doorsturen, hebt u het volgende nodig:

* Een Azure-opslagaccount waarop u *ListKeys*-machtigingen hebt. We raden u aan om een algemeen opslagaccount te gebruiken en geen Blob Storage-account. Raadpleeg de [Azure Storage-prijscalculator](https://azure.microsoft.com/pricing/calculator/?service=storage) voor opslagprijzen. 
* Een Azure Event Hubs-naamruimte om te integreren met oplossingen van derden.
* Een Azure Log Analytics-werkruimte om Logboeken te verzenden naar Azure Monitor-Logboeken.

## <a name="cost-considerations"></a>Kostenoverwegingen

Als u al een Azure AD-licentie hebt, moet u een Azure-abonnement hebben om het opslagaccount en de Event Hub op te zetten. Het Azure-abonnement is gratis, maar u moet betalen om Azure-resources te gebruiken, waaronder het opslagaccount dat u gebruikt om te archiveren en de Event Hub die u gebruikt om te streamen. De hoeveelheid gegevens, en dus de in rekening gebrachte kosten, variëren sterk, afhankelijk van de grootte van de tenant. 

### <a name="storage-size-for-activity-logs"></a>Opslaggrootte voor activiteitenlogboeken

Elke auditlogboekgebeurtenis neemt ongeveer 2 KB aan opslagruimte in beslag. Zo hebt u voor een tenant met 100.000 gebruikers, wat neerkomt op ongeveer 1,5 miljoen gebeurtenissen per dag, ongeveer 3 GB aan gegevensopslag per dag nodig. Aangezien schrijfbewerkingen in batches van ongeveer 5 minuten voorkomen, kunt u om en nabij 9000 schrijfbewerkingen per maand verwachten. 

De volgende tabel bevat een kostenraming, afhankelijk van de grootte van de tenant, voor een opslagaccount voor algemeen gebruik v2 in US - west met een bewaringstermijn van ten minste één jaar. Gebruik de [Azure Storage-prijscalculator](https://azure.microsoft.com/pricing/details/storage/blobs/) om een meer nauwkeurige inschatting te maken van het gegevensvolume dat u denkt te gebruiken voor uw toepassing. 

| Logboekcategorie | Aantal gebruikers | Gebeurtenissen per dag | Gegevensvolume per maand (geschat) | Kosten per maand (geschat) | Kosten per jaar (geschat) |
|--------------|-----------------|----------------------|--------------------------------------|----------------------------|---------------------------|
| Controleren | 100.000 | 1,5&nbsp;miljoen | 90 GB | $ 1,93 | $ 23,12 |
| Controleren | 1000 | 15.000 | 900 MB | $ 0,02 | $ 0,24 |
| Aanmeldingen | 1000 | 34.800 | 4 GB | $ 0,13 | $ 1,56 |
| Aanmeldingen | 100.000 | 15&nbsp;miljoen | 1,7 TB | $ 35,41 | $ 424,92 | 


### <a name="event-hub-messages-for-activity-logs"></a>Event Hub-berichten voor activiteitenlogboeken

Gebeurtenissen worden ongeveer vijf minuten lang gebatched en vervolgens als één bericht met alle gebeurtenissen in die vijf minuten verstuurd. Een bericht in de Event Hub heeft een maximale grootte van 256 kB en als de totale grootte van alle berichten binnen de vijf minuten die grootte overstijgt, worden er meerdere berichten gestuurd. 

Zo vinden er voor een grote tenant met meer dan 100.000 gebruikers bijvoorbeeld doorgaans 18 gebeurtenissen per seconde plaats, een aantal dat overeenkomt met 5400 gebeurtenissen per vijf minuten. Omdat auditlogboeken ongeveer 2 kB per gebeurtenis groot zijn, komt dit overeen met 10,8 MB aan gegevens. Dat is de reden waarom er met die interval van 5 minuten 43 berichten naar de Event Hub worden gestuurd. 

De volgende tabel bevat een raming van de maandelijkse kosten voor een eenvoudige Event Hub in US - west, afhankelijk van het volume van de gebeurtenisgegevens. Gebruik de [Event Hubs-prijscalculator](https://azure.microsoft.com/pricing/details/event-hubs/) om een nauwkeurige inschatting te berekenen voor het gegevensvolume dat u voor uw toepassing verwacht.

| Logboekcategorie | Aantal gebruikers | Gebeurtenissen per seconde | Gebeurtenissen met een interval van vijf minuten | Volume per interval | Berichten per interval | Berichten per maand | Kosten per maand (geschat) |
|--------------|-----------------|-------------------------|----------------------------------------|---------------------|---------------------------------|------------------------------|----------------------------|
| Controleren | 100.000 | 18 | 5400 | 10,8 MB | 43 | 371.520 | $ 10,83 |
| Controleren | 1000 | 0.1 | 52 | 104 kB | 1 | 8640 | $ 10,80 |
| Aanmeldingen | 1000 | 178 | 53.400 | 106,8&nbsp;MB | 418 | 3.611.520 | $ 11,06 |  

### <a name="azure-monitor-logs-cost-considerations"></a>Azure Monitor logboeken kosten overwegingen met betrekking tot

Kosten met betrekking tot het beheren van de Azure Monitor-Logboeken, Zie [kosten beheren door te beheren gegevensvolume en retentie in Logboeken van Azure Monitor](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-cost-storage).

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

Deze sectie bevat antwoorden op veelgestelde vragen en bekende problemen met betrekking tot Azure AD-logboeken in Azure Monitor.

**V: Welke logboeken zijn opgenomen?**

**A**: Aanmeldingsactiviteiten logboeken en auditlogboeken zijn beschikbaar voor de routering via deze functie, hoewel gebeurtenissen voor beveiligingscontrole met betrekking tot B2C momenteel niet opgenomen zijn. Raadpleeg het [auditlogboekschema](reference-azure-monitor-audit-log-schema.md) en het [aanmeldingslogboekschema](reference-azure-monitor-sign-ins-log-schema.md) om uit te vinden welke typen logboeken en welke op functie gebaseerde logboeken momenteel worden ondersteund. 

-----

**V: Hoe snel na een actie wordt de bijbehorende logboeken weergegeven in mijn event hub?**

**A**: De logboeken moeten weergegeven in uw event hub binnen twee tot vijf minuten nadat de actie wordt uitgevoerd. Raadpleeg [Wat is Azure Event Hubs?](../../event-hubs/event-hubs-about.md) voor meer informatie over Event Hubs.

-----

**V: Hoe snel na een actie wordt de bijbehorende logboeken weergegeven in mijn storage-account?**

**A**: Voor Azure storage-accounts is de latentie overal van 5 tot 15 minuten nadat de actie wordt uitgevoerd.

-----

**V: Wat gebeurt er als de bewaarperiode van een diagnostische instelling wordt gewijzigd door een beheerder?**

**A**: Het nieuwe retentiebeleid zal worden toegepast op Logboeken verzameld na de wijziging. Logboeken verzameld voordat de wijziging niet gewijzigd worden.

-----

**V: Hoeveel kost het om op te slaan van mijn gegevens?**

**A**: De kosten voor opslag zijn afhankelijk van de grootte van uw logboeken en de bewaarperiode liggen die u kiest. Raadpleeg de sectie [Opslaggrootte voor activiteitenlogboeken](#storage-size-for-activity-logs) voor een lijst van de geschatte kosten voor tenants. De kosten zijn afhankelijk van het aantal logboeken dat wordt gegenereerd.

-----

**V: Hoeveel kost het om mijn gegevens naar een event hub streamen?**

**A**: De kosten voor streaming is afhankelijk van het aantal berichten u per minuut ontvangt. In dit artikel wordt beschreven hoe de kosten worden berekend en vindt u een lijst met geschatte kosten, die zijn gebaseerd op het aantal berichten. 

-----

**V: Hoe Integreer ik Azure AD-activiteitenlogboeken met mijn SIEM-systeem?**

**A**: U kunt dit op twee manieren doen:

- Azure Monitor gebruiken met Event Hubs om logboeken naar uw SIEM-systeem te streamen. [Stream de logboeken naar een event hub](tutorial-azure-monitor-stream-logs-to-event-hub.md) en [stel vervolgens uw SIEM-hulpprogramma](tutorial-azure-monitor-stream-logs-to-event-hub.md#access-data-from-your-event-hub) in met de geconfigureerde event hub. 

- Gebruik de [Reporting Graph API](concept-reporting-api.md) voor toegang tot de gegevens. Push de gegevens daarna naar uw SIEM-systeem met uw eigen scripts.

-----

**V: Welke SIEM-hulpprogramma's worden momenteel ondersteund?** 

**A**: Azure Monitor wordt momenteel ondersteund door [Splunk](tutorial-integrate-activity-logs-with-splunk.md), QRadar, en [Sumo logische](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory). Raadpleeg [Azure-bewakingsgegevens streamen naar een Event Hub voor gebruik door een extern hulpprogramma](../../azure-monitor/platform/stream-monitoring-data-event-hubs.md) voor meer informatie over hoe de connectors werken.

-----

**V: Hoe Integreer ik Azure AD activiteitenlogboeken met mijn Splunk instantie?**

**A**: Eerste, [route de Azure AD-activiteitenlogboeken streamen naar een event hub](quickstart-azure-monitor-stream-logs-to-event-hub.md), volgt u de stappen voor het [activiteitenlogboeken integreren met Splunk](tutorial-integrate-activity-logs-with-splunk.md).

-----

**V: Hoe Integreer ik Azure AD activiteitenlogboeken Sumo logische?** 

**A**: Eerste, [route de Azure AD-activiteitenlogboeken streamen naar een event hub](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Collect_Logs_for_Azure_Active_Directory), volgt u de stappen voor het [installeert de Azure AD-toepassing en de dashboards weergeven in SumoLogic](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure_Active_Directory/Install_the_Azure_Active_Directory_App_and_View_the_Dashboards).

-----

**V: Krijg ik toegang tot de gegevens van een event hub zonder een externe SIEM-hulpprogramma gebruiken?** 

**A**: Ja. U kunt de [Event Hubs-API](../../event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph.md) gebruiken om de logboeken vanuit uw eigen aangepaste toepassing te bekijken. 

-----


## <a name="next-steps"></a>Volgende stappen

* [Activiteitenlogboeken in een opslagaccount archiveren](quickstart-azure-monitor-route-logs-to-storage-account.md)
* [Activiteitenlogboeken doorsturen naar een Event Hub](quickstart-azure-monitor-stream-logs-to-event-hub.md)
* [Activiteitenlogboeken integreren met Azure Monitor](howto-integrate-activity-logs-with-log-analytics.md)

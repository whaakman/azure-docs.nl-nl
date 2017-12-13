---
title: Instellen van de Cloud App Discovery-service in Azure Active Directory | Microsoft Docs
description: Zoeken en beheren van toepassingen met Cloud App Discovery bruikbare informatie te verstrekken over het cloudgebruik en de shadow IT.
services: active-directory
keywords: cloud app discovery, het beheren van toepassingen
documentationcenter: 
author: curtand
manager: mtillman
tags: ignite
ms.assetid: db968bf5-22ae-489f-9c3e-14df6e1fef0a
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/18/2017
ms.author: curtand
ms.reviewer: nigu
ms.openlocfilehash: 4a0cb1b7793c846f98ae4e89b99b4bda984cd5e4
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="set-up-cloud-app-discovery-in-azure-ad"></a>Cloud App Discovery instellen in Azure AD

Cloud App Discovery in Azure AD is nu gebaseerd op de integratie met gegevens die beschikbaar is via de Microsoft Cloud App Security. Als u wilt bieden continue informatie over het cloudgebruik en de shadow IT, vergelijkt Cloud App Discovery uw verkeerslogboeken naar de Cloud App Security-catalogus van meer dan 15.000 cloud-apps. Dit artikel beschrijft het instellingsproces en bevat koppelingen naar de gedetailleerde informatie voor elke stap. Hierin worden ook firewall en proxy-gegevens en logboekbestanden ondersteuning.

## <a name="prerequisites"></a>Vereisten

* Uw organisatie moet een Azure AD Premium P1-licentie voor het gebruik van het product hebben. Zie voor meer informatie [prijzen van Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).
* Als u Cloud App Discovery instelt, moet u een globale beheerder of een lezer beveiliging in Azure Active Directory.

## <a name="setup-steps"></a>Instellingsstappen

1. [Instellen van rapporten in momentopname](cloudappdiscovery-set-up-snapshots.md) Controleer uw logboekindeling Zorg ervoor dat uw logboeken bruikbare informatie verstrekken aan Cloud App Discovery. Ze kunnen ook ad-hoc inzicht verschaffen in logboeken over webverkeer die u handmatig van uw firewalls en proxyservers uploaden.

2. [Instellen van continue reporting](https://docs.microsoft.com/cloud-app-security/discovery-docker) voor het analyseren van alle logboeken die zijn doorgestuurd vanaf uw netwerk met behulp van de logboekverzamelaar Cloud App Security. U kunt deze gebruiken om nieuwe apps en trends in gebruik te identificeren.

3. Als uw logboeken worden momenteel niet ondersteund, [instellen van een aangepaste log parser](https://docs.microsoft.com/cloud-app-security/custom-log-parser) zodat ze door Cloud App Discovery kunt analyseren.
  
## <a name="log-processing-flow"></a>Verwerking van de stroom van de logboekbestanden

Het kan duren van enkele minuten tot enkele uren om rapporten afhankelijk van de hoeveelheid gegevens te genereren. Dit is wat is geanalyseerd:

* **Uploaden** logboeken over webverkeer van uw netwerk naar de portal worden geüpload.
* **Parseren** Cloud App Security parseert en extraheert verkeersgegevens uit de verkeerslogboeken met een specifieke parser voor elke gegevensbron.
* **Analyseren** verkeersgegevens worden vergeleken met de catalogus met Cloud App Security voor het identificeren van meer dan 15.000 cloud-apps. Actieve gebruikers en IP-adressen ook geïdentificeerd als onderdeel van de analyse.
* **Rapport genereren** Cloud App Security genereert een rapport van de gegevens opgehaald uit de logboekbestanden en is beschikbaar voor Cloud App Discovery.

> [!NOTE]
> * Continue rapportgegevens is twee keer per dag worden geanalyseerd.
> * De logboekverzamelaar comprimeren gegevens voordat deze wordt geüpload. Het uitgaande verkeer op de logboekverzamelaar is ongeveer 10% van de grootte van de ontvangen verkeerslogboeken.

## <a name="using-traffic-logs-for-cloud-app-discovery"></a>Met behulp van logboeken over webverkeer voor Cloud App Discovery

Cloud App Discovery gebruikt de gegevens in uw verkeerslogboeken. Meer details die u in het logboek, de betere zichtbaarheid die u kunt toevoegen. Cloud App Discovery vereist-webverkeer gegevens met de volgende kenmerken:

* Datum van de transactie
* IP-adres van bron
* Brongebruiker **aanbevolen**
* IP-doeladres
* Doel-URL **aanbevolen** (URL's bieden meer nauwkeurigheid voor detectie van de cloud-app dan IP-adressen)
* Totale hoeveelheid gegevens
* Hoeveelheid geüpload of waarin deze gedownloade gegevens om inzicht te krijgen over patronen van app-cloudgebruik
* Actie op die (toegestaan/geblokkeerd)

Cloud App Discovery kan weergeven of kenmerken die niet zijn opgenomen in de logboeken te analyseren. Bijvoorbeeld: **Cisco ASA-Firewall** standaard logboekindeling bevat niet de **hoeveelheid geüploade bytes per transactie**, **gebruikersnaam**, of **doel-URL**  , maar alleen het IP-doeladres. Daarom moet u wellicht minder inzicht in de cloud-apps van deze gegevensbron. Voor Cisco ASA firewalls, moet u de informatie-niveau instellen op 6.1.

Om een Cloud App Discovery-rapport is gegenereerd, moeten uw verkeerslogboeken voldoen aan de volgende voorwaarden:

1.  Gegevensbron [een ondersteunde firewall of proxyserver server](#supported-firewalls-and-proxies).
2.  Logboekindeling komt overeen met de verwachte indeling standaard. Deze optie is ingeschakeld tijdens het uploaden. Zie voor het optimaliseren van uour logboekindeling [momentopname Cloud App Discovery rapporten maken](cloudappdiscovery-set-up-snapshots.md).
3.  Gebeurtenissen zijn niet meer dan 90 dagen oud.
4.  Het logboekbestand is ongeldig en bevat informatie voor uitgaand verkeer.

## <a name="supported-firewalls-and-proxy-servers"></a>Ondersteunde firewalls en proxyservers

* Barracuda - Web-App-Firewall (W3C)
* Blue Coat Proxy SG - toegangslogboek (W3C)
* Check Point
* Cisco ASA vuurkracht
* Cisco ASA-Firewall (Cisco ASA voor firewalls, de informatie-niveau instellen op 6)
* Cisco IronPort WSA
* Cisco ScanSafe
* Cisco Meraki – URL-logbestanden
* Clavister NGFW (Syslog)
* Dell Sonicwall
* Fortinet Fortigate
* Juniper SRX
* Juniper SSG
* McAfee beveiligde Webgateway
* Microsoft Forefront Threat Management Gateway (W3C)
* Firewall Palo Alto-reeks
* Sophos SG
* Sophos Cyberoam
* SQUID (Algemeen)
* SQUID (systeemeigen)
* Rapport van Websense - oplossingen voor webbeveiliging - gedetailleerd onderzoeksrapport (CSV)
* Websense - oplossingen voor webbeveiliging - activiteitenlogboek van Internet (CEF)
* Zscaler

> [!NOTE]
> Cloud App Discovery ondersteunt zowel IPv4 als IPv6-adressen.

Als uw logboek niet wordt ondersteund, schakelt u **andere** als de **gegevensbron** en geeft u het apparaat en meld u probeert te uploaden. Uw logboek wordt gecontroleerd door het team van Cloud App Security cloud analist. Wanneer ondersteuning voor het logboektype van uw wordt toegevoegd we u een melding, maar in plaats daarvan kunt u een aangepaste parser die overeenkomt met uw logboekindeling. Zie voor meer informatie [gebruik van een aangepaste log parser](https://docs.microsoft.com/cloud-app-security/custom-log-parser).

## <a name="data-attributes-according-to-vendor-documentation"></a>Gegevenskenmerken (volgens de documentatie van de leverancier)

| Gegevensbron         | Bepaalde App-URL | Bepaalde App IP-adres | Gebruikersnaam | IP-adres van bron | Totale verkeer | Geüploade bytes |
|-----------------------------------------|----------------|---------------|----------|-----------|---------------|----------------|
| Barracuda                               | **Ja**        | **Ja**       | **Ja**  | **Ja**   | Nee            | Nee             |
| Blue Coat                               | **Ja**        | Nee            | **Ja**  | **Ja**   | **Ja**       | **Ja**        |
| Controlepunt                              | Nee             | **Ja**       | Nee       | **Ja**   | Nee            | Nee             |
| Cisco ASA                               | Nee             | **Ja**       | Nee       | **Ja**   | **Ja**       | Nee             |
| Cisco FWSM                              | Nee             | **Ja**       | Nee       | **Ja**   | **Ja**       | Nee             |
| Cisco Ironport WSA                      | **Ja**        | **Ja**       | **Ja**  | **Ja**   | **Ja**       | **Ja**        |
| Cisco Meraki                            | **Ja**        | **Ja**       | Nee       | **Ja**   | Nee            | Nee             |
| Clavister NGFW (Syslog)                 | **Ja**        | **Ja**       | **Ja**  | **Ja**   | **Ja**       | **Ja**        |
| Dell SonicWall                          | **Ja**        | **Ja**       | Nee       | **Ja**   | **Ja**       | **Ja**        |
| FortiGate                               | Nee             | **Ja**       | Nee       | **Ja**   | **Ja**       | **Ja**        |
| Juniper SRX                             | Nee             | **Ja**       | Nee       | **Ja**   | **Ja**       | **Ja**        |
| Juniper SSG                             | Nee             | **Ja**       | Nee       | **Ja**   | **Ja**       | **Ja**        |
| McAfee SWG                              | **Ja**        | Nee            | Nee       | **Ja**   | **Ja**       | **Ja**        |
| MS TMG                                  | **Ja**        | Nee            | **Ja**  | **Ja**   | **Ja**       | **Ja**        |
| Palo Alto Networks                      | **Ja**        | **Ja**       | **Ja**  | **Ja**   | **Ja**       | **Ja**        |
| Sophos                                  | **Ja**        | **Ja**       | **Ja**  | **Ja**   | **Ja**       | Nee             |
| SQUID (Algemeen)                          | **Ja**        | Nee            | **Ja**  | **Ja**   | Nee            | **Ja**        |
| SQUID (systeemeigen)                          | **Ja**        | Nee            | **Ja**  | **Ja**   | Nee            | **Ja**        |
| Websense - onderzoek rapport (CSV)   | **Ja**        | **Ja**       | **Ja**  | **Ja**   | **Ja**       | **Ja**        |
| Websense - activiteitenlogboek Internet (CEF)  | **Ja**        | **Ja**       | **Ja**  | **Ja**   | **Ja**       | **Ja**        |
| Zscaler                                 | **Ja**        | **Ja**       | **Ja**  | **Ja**   | **Ja**       | **Ja**        |


## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende koppelingen om door te gaan naar de Cloud App Discovery instellen in Azure AD.

* [Momentopnamerapporten maken](cloudappdiscovery-set-up-snapshots.md)
* [Continue rapportage configureren](https://docs.microsoft.com/cloud-app-security/discovery-docker)
* [Een aangepaste logboekparser gebruiken](https://docs.microsoft.comcommit/cloud-app-security/custom-log-parser)

---
title: Verbinding maken met gegevensbronnen Sentinel Preview van Azure? | Microsoft Docs
description: Leer hoe u verbinding maken met gegevensbronnen Sentinel van Azure.
services: sentinel
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: a3b63cfa-b5fe-4aff-b105-b22b424c418a
ms.service: sentinel
ms.devlang: na
ms.topic: overview
ms.custom: mvc
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: a1c74f0ed9fd9b9abccfb7c2762cadf948c8fce0
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57884792"
---
# <a name="connect-data-sources"></a>Verbinding maken met gegevensbronnen

> [!IMPORTANT]
> Azure Sentinel is momenteel in openbare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.



Aan het ingebouwde Azure-Sentinel moet u eerst verbinding maken met uw gegevensbronnen. Azure Sentinel wordt geleverd met een aantal connectors voor Microsoft-oplossingen, beschikbaar is buiten het vak en biedt realtime-integratie, met inbegrip van oplossingen van Microsoft Threat Protection en Microsoft 365-bronnen, zoals Office 365, Azure AD, Azure ATP, en Microsoft Cloud App Security, en meer. Er zijn bovendien ingebouwde connectors voor het complete ecosysteem van de beveiliging voor niet-Microsoft-oplossingen. U kunt ook de algemene indeling voor gebeurtenissen, Syslog of REST-API om uw gegevensbronnen ook verbinding met Azure Sentinel te gebruiken.  

![Gegevens collectors](./media/collect-data/collect-data-page.png)

## <a name="data-collection-methods"></a>Methoden voor het verzamelen van gegevens

De volgende gegevens verzameling methoden worden ondersteund door Azure Sentinel:

- **Microsoft-services**:<br> Microsoft-services zijn systeemeigen, aangesloten gebruik te maken van de basis voor out-en-klare integratie van Azure, de volgende oplossingen kunnen worden verbonden met slechts een paar klikken:
    - [Office 365](connect-office-365.md)
    - [Azure AD controleren, logboeken en -aanmeldingen](connect-azure-active-directory.md)
    - [Azure Activity](connect-azure-activity.md)
    - [Azure AD Identity Protection](connect-azure-ad-Identity-protection.md)
    - [Azure Security Center](connect-azure-security-center.md)
    - [Azure Information Protection](connect-azure-information-protection.md)
    - [Azure Advanced Threat Protection](connect-azure-atp.md)
    - [Cloud App Security](connect-cloud-app-security.md)
    - [Windows-beveiligingsgebeurtenissen](connect-windows-security-events.md) 
    - [Windows firewall](connect-windows-firewall.md)

- **Externe oplossingen via API**: Sommige gegevensbronnen zijn verbonden met behulp van API's die worden geleverd door de verbonden gegevensbron. De meeste beveiligingstechnologieën bieden doorgaans een reeks API's via welke gebeurtenislogboeken kunnen worden opgehaald. De API's verbinding maken met Azure Sentinel en specifieke gegevenstypen verzamelen en ze verzenden naar Azure Log Analytics. Apparaten die zijn verbonden via de API zijn onder andere:
    - [Barracuda](connect-barracuda.md)
    - Symantec
- **Externe oplossingen via agent**: Azure Sentinel kan worden verbonden met alle andere gegevensbronnen die real-time logboek streamen met de Syslog-protocol, via een agent kunnen uitvoeren. <br>De meeste apparaten maken gebruik van het Syslog-protocol om gebeurtenisberichten te verzenden met het logboek zelf en gegevens over het logboek. De indeling van de logboeken is, maar de meeste apparaten ondersteunen de standaard Common Event Format (CEF). <br>De agent Sentinel van Azure, die is gebaseerd op de OMS-agent, zet CEF opgemaakt logboeken naar een indeling die door Log Analytics kan worden opgenomen. Afhankelijk van het apparaattype, wordt de agent rechtstreeks op het apparaat, of op een eigen Linux-server geïnstalleerd. De agent voor Linux gebeurtenissen vanuit de Syslog-daemon ontvangt via UDP, maar in gevallen waarin een Linux-machine voor het verzamelen van een groot aantal Syslog-gebeurtenissen worden verwacht, ze via TCP worden verzonden vanuit de Syslog-daemon naar de agent en van daaruit naar Log Analytics.
    - Firewalls, proxy's en -eindpunten:
        - [F5](connect-f5.md)
        - [Check Point](connect-checkpoint.md)
        - [Cisco ASA](connect-cisco.md)
        - [De Fortinet](connect-fortinet.md)
        - [Palo Alto](connect-paloalto.md)
        - [Andere CEF-apparaten](connect-common-event-format.md)
        - [Andere apparaten Syslog](connect-syslog.md)
    - DLP-oplossingen
    - [Threat intelligence providers](connect-threat-intelligence.md)
    - [DNS-machines](connect-dns.md) -agent is geïnstalleerd rechtstreeks op de DNS-machine
    - Linux-servers
    - Andere clouds
    
## Opties voor de agent verbinding<a name="agent-options"></a>

Als u wilt verbinden met uw externe apparaat Azure Sentinel, de agent moet worden geïmplementeerd op een specifieke virtuele machine (VM of on-premises) om de communicatie tussen het apparaat en de Azure-Sentinel te ondersteunen. U kunt de agent automatisch of handmatig implementeren. Automatische implementatie is alleen beschikbaar als uw toegewezen machine is een nieuwe virtuele machine die u in Azure maken wilt. 


![CEF in Azure](./media/connect-cef/cef-syslog-azure.png)

U kunt ook kunt u de agent handmatig op een bestaande VM in Azure, op een virtuele machine in een andere cloud of op een on-premises machine implementeren.

![CEF on-premises](./media/connect-cef/cef-syslog-onprem.png)


## <a name="next-steps"></a>Volgende stappen

- Als u wilt aan de slag met Azure Sentinel, moet u een abonnement op Microsoft Azure. Als u geen abonnement hebt, kunt u zich aanmelden voor een [gratis proefversie](https://azure.microsoft.com/free/).
- Meer informatie over het [onboarding uw gegevens naar Azure Sentinel](quickstart-onboard.md), en [Krijg inzicht in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).

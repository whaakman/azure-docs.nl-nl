---
title: Verzamel gegevens van uw omgeving met Azure Log Analytics | Microsoft Docs
description: Dit onderwerp helpt u begrijpen hoe u gegevens verzamelen en controleren van computers die worden gehost in uw on-premises of andere cloudomgeving met logboekanalyse.
services: log-analytics
documentationcenter: 
author: MGoedtel
manager: carmonm
editor: 
ms.assetid: 
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/08/2017
ms.author: magoedte
ms.openlocfilehash: 513855084c8b89d97b049f1df2ec24d0f9789afe
ms.sourcegitcommit: d247d29b70bdb3044bff6a78443f275c4a943b11
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Verzamel gegevens van computers in uw omgeving met Log Analytics

Azure Log Analytics kunt verzamelen van en reageren op gegevens van Windows of Linux-computers die zich:

* [Virtuele machines in Azure](log-analytics-quick-collect-azurevm.md) met behulp van de VM-extensie van Log Analytics 
* Uw datacenter als fysieke servers of virtuele machines
* Virtuele machines in een cloud-gebaseerde service zoals Amazon Web Services (AWS)

Computers die worden gehost in uw omgeving kunnen rechtstreeks worden verbonden met logboekanalyse of als u al deze computers met System Center Operations Manager 2012 R2 of 2016 controleren wilt, kunt u uw beheergroep bewerkingen beheren integreren met logboekanalyse en blijven uw strategie voor serviceprocessen bewerkingen en onderhoud.  

## <a name="overview"></a>Overzicht

![log-Analytics-agent-Direct-Connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Voordat u analyseren en op de verzamelde gegevens fungeert, moet u eerst installeren en verbinding maken met agents voor alle computers die u wilt gegevens verzenden naar het Log Analytics-service. U kunt agents installeren op uw on-premises computers via Setup, opdrachtregel of met Desired State Configuration (DSC) in Azure Automation. 

De agent voor Linux en Windows uitgaande met de Log Analytics-service communiceert via TCP-poort 443, en als de computer verbinding maakt met een firewall of proxy-server om te communiceren via Internet, bekijkt u [configuratie van de agent voor gebruik met een proxyserver of OMS Gateway](#configuring-the-agent-for-use-with-a-proxy-server-or-oms-gateway) moet worden toegepast om te begrijpen welke configuratie wordt gewijzigd. Als u de computer met System Center 2016 - Operations Manager of Operations Manager 2012 R2, bewaakt kan zijn multihomed met de service voor logboekanalyse voor het verzamelen van gegevens en door te sturen naar de service en nog steeds worden bewaakt door [Operations Manager ](log-analytics-om-agents.md). Linux-computers worden bewaakt door een Operations Manager-beheergroep geïntegreerd met logboekanalyse ontvangen geen configuratie voor gegevensbronnen en voorwaarts verzamelde gegevens via de beheergroep. De Windows-agent kan rapporteren dat maximaal vier werkruimten, terwijl de Linux-agent biedt alleen ondersteuning voor rapportage aan een enkele werkruimte.  

De agent voor Linux en Windows niet alleen om verbinding te maken in Log Analytics, het ondersteunt ook verbinding maken met het bij Azure Automation host de functie Hybride Runbook worker oplossingen zoals bijhouden en updatebeheer.  Zie voor meer informatie over de hybride Runbook Worker-rol [Azure Automation Hybrid Runbook Worker](../automation/automation-offering-get-started.md#automation-architecture-overview).  

Als de beleidsregels van uw IT-beveiliging niet toestaan computers in uw netwerk verbinding maken met Internet dat, kan de agent kan worden geconfigureerd voor verbinding met de OMS-Gateway voor configuratie-informatie te ontvangen en verzenden van verzamelde gegevens afhankelijk van de oplossing die u hebt ingeschakeld. Zie voor meer informatie en stapsgewijze instructies voor het configureren van uw Linux- of Windows-agent te communiceren via een OMS-Gateway naar de Log Analytics-service [computers koppelen aan OMS met behulp van de Gateway OMS](log-analytics-oms-gateway.md). 

## <a name="prerequisites"></a>Vereisten
Voordat u begint, lees de volgende informatie om te controleren of u voldoet aan de minimale systeemvereisten.

### <a name="windows-operating-system"></a>Windows-besturingssysteem
De volgende versies van het Windows-besturingssysteem worden officieel ondersteund voor de Windows-agent:

* Windows Server 2008 Service Pack 1 (SP1) of hoger
* Windows 7 SP1 en hoger.

#### <a name="network-configuration"></a>Netwerkconfiguratie
Gegevens van de onderstaande lijst de vereist voor de Windows-agent om te communiceren met logboekanalyse proxy- en firewall-configuratie-informatie. Verkeer is uitgaand vanaf het netwerk naar het Log Analytics-service. 

| Agentresource | Poorten | HTTPS-controle overslaan|
|----------------|-------|------------------------|
|*.ods.opinsights.azure.com |443 | Ja |
|*.oms.opinsights.azure.com | 443 | Ja | 
|*.blob.core.windows.net | 443 | Ja | 
|*.azure-automation.net | 443 | Ja | 

### <a name="linux-operating-systems"></a>Linux-besturingssystemen
De volgende Linux-distributies worden officieel ondersteund.  De Linux-agent kan echter ook uitvoeren op andere distributies niet wordt vermeld.

* Amazon Linux 2012.09-2015.09 (x86/x64)
* CentOS Linux 5, 6 en 7 (x86/x64)
* Oracle Linux 5, 6 en 7 (x86/x64)
* Red Hat Enterprise Linux Server 5, 6 en 7 (x86/x64)
* Debian GNU/Linux 6, 7 en 8 (x86/x64)
* Ubuntu 12.04 TNS, 14.04 TNS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 en 12 (x86/x64)

#### <a name="network-configuration"></a>Netwerkconfiguratie
Gegevens van de onderstaande lijst de vereist voor de Linux-agent om te communiceren met logboekanalyse proxy- en firewall-configuratie-informatie. Verkeer is uitgaand vanaf het netwerk naar het Log Analytics-service. 

|Agentresource| Poorten |  
|------|---------|  
|*.ods.opinsights.azure.com | Poort 443|   
|*.oms.opinsights.azure.com | Poort 443|   
|*.blob.core.windows.net | Poort 443|   
|*.azure-automation.net | Poort 443|  

De Linux-agent ondersteunt communiceren via een proxy of Gateway naar de Log Analytics-service met behulp van het HTTPS-protocol OMS.  Zowel anonieme verificatie en basisverificatie (gebruikersnaam en wachtwoord) worden ondersteund.  De proxy-server kan worden opgegeven tijdens de installatie of door het wijzigen van het configuratiebestand proxy.conf na de installatie.  

De configuratiewaarde proxy heeft de volgende syntaxis:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Als de proxyserver niet vereist is om te verifiëren, vereist de Linux-agent nog steeds bieden een pseudo-gebruiker en wachtwoord. Dit is geen gebruikersnaam of wachtwoord.

|Eigenschap| Beschrijving |
|--------|-------------|
|Protocol | https |
|Gebruiker | Optioneel de gebruikersnaam voor proxyverificatie |
|wachtwoord | Optioneel wachtwoord voor proxy-authenticatie |
|proxyhost | Adres of FQDN-naam van de proxy-server/OMS-Gateway |
|poort | Optionele poortnummer op voor de proxy-server/OMS-Gateway |

Bijvoorbeeld: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Als u speciale tekens bevatten zoals ' @ ' in uw wachtwoord, wordt een verbindingsfout proxy omdat de waarde is niet correct geparseerd.  U kunt dit probleem omzeilen, coderen het wachtwoord in de URL met een hulpprogramma zoals [URLDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Installeren en configureren van agent 
Verbinding maken met uw lokale computers rechtstreeks met Log Analytics kunt u doen met verschillende methoden, afhankelijk van uw vereisten. De volgende tabel licht elke methode om te bepalen die het beste werkt in uw organisatie.

|Bron | Methode | Beschrijving|
|-------|-------------|-------------|
| Windows-computer|- [Handmatige installatie](log-analytics-agent-windows.md)<br>- [Azure Automation DSC](log-analytics-agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Resource Manager-sjabloon met de Azure-Stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |De Microsoft Monitoring agent installeren vanaf de opdrachtregel of met behulp van een automatische methode zoals Azure Automation DSC [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications), of met een Azure Resource Manager-sjabloon als u Microsoft hebt geïmplementeerd Azure-Stack in uw datacenter.| 
|Linux-computer| [Handmatige installatie](log-analytics-quick-collect-linux-computer.md)|Installeer de agent voor Linux aanroepen van een wrapper-script dat wordt gehost op GitHub. | 
| System Center Operations Manager|[Operations Manager integreren met logboekanalyse](log-analytics-om-agents.md) | Configureren van de integratie tussen Operations Manager en logboekanalyse voor het doorsturen van gegevens verzameld van Linux- en Windows-computers die rapporteren aan een beheergroep.|  

## <a name="next-steps"></a>Volgende stappen

* Bekijk [gegevensbronnen](log-analytics-data-sources.md) om te begrijpen van de gegevensbronnen die beschikbaar zijn voor het verzamelen van gegevens van uw Windows- of Linux-systeem. 

* Meer informatie over [Meld zoekopdrachten](log-analytics-log-searches.md) om de gegevens verzameld van gegevensbronnen en oplossingen te analyseren. 

* Meer informatie over [oplossingen](log-analytics-add-solutions.md) functionaliteit toevoegen aan Log Analytics en ook het verzamelen van gegevens in de OMS-opslagplaats.
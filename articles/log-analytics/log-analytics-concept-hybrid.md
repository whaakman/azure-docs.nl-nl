---
title: Verzamel gegevens van uw omgeving met Azure Log Analytics | Microsoft Docs
description: Dit onderwerp helpt u begrijpen hoe u gegevens verzamelen en controleren van computers die worden gehost in uw on-premises of andere cloudomgeving met logboekanalyse.
services: log-analytics
documentationcenter: ''
author: MGoedtel
manager: carmonm
editor: ''
ms.assetid: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2018
ms.author: magoedte
ms.openlocfilehash: 5bf1e12c958fef0cb20eaad8cece8cadb380c196
ms.sourcegitcommit: 4e36ef0edff463c1edc51bce7832e75760248f82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/08/2018
ms.locfileid: "35235937"
---
# <a name="collect-data-from-computers-in-your-environment-with-log-analytics"></a>Verzamel gegevens van computers in uw omgeving met Log Analytics

Azure Log Analytics kunt verzamelen van en reageren op gegevens van Windows of Linux-computers die zich:

* [Virtuele machines in Azure](log-analytics-quick-collect-azurevm.md) met behulp van de VM-extensie van Log Analytics 
* Uw datacenter als fysieke servers of virtuele machines
* Virtuele machines in een cloud-gebaseerde service zoals Amazon Web Services (AWS)

Computers die worden gehost in uw omgeving kunnen rechtstreeks worden verbonden met logboekanalyse, of als u al deze computers met System Center Operations Manager 2012 R2, 2016 controleert of versie 1801, kunt u uw beheergroep bewerkingen beheren met integreren Meld u Analytics en uw IT-bewerkingen serviceprocessen aanhouden.  

## <a name="overview"></a>Overzicht

![log-Analytics-agent-Direct-Connect-diagram](media/log-analytics-concept-hybrid/log-analytics-on-prem-comms.png)

Voordat u analyseren en op de verzamelde gegevens fungeert, moet u eerst installeren en verbinding maken met agents voor alle computers die u wilt gegevens verzenden naar het Log Analytics-service. U kunt agents installeren op uw on-premises computers via Setup, opdrachtregel of met Desired State Configuration (DSC) in Azure Automation. 

De agent voor Linux en Windows uitgaande met de Log Analytics-service communiceert via TCP-poort 443, en als de computer verbinding maakt met een firewall of proxy-server om te communiceren via Internet, bekijkt u [de sectie vereisten](#prerequisites) naar inzicht in de netwerkconfiguratie die is vereist.  Als de beleidsregels van uw IT-beveiliging niet toestaan computers in het netwerk verbinding maken met Internet dat, kunt u de installatie een [OMS Gateway](log-analytics-oms-gateway.md) en configureer vervolgens de agent verbinding maken via de gateway met logboekanalyse. De agent kunt configuratie-informatie te ontvangen en verzenden van gegevens die zijn verzameld, afhankelijk van welke regels voor het verzamelen van gegevens en de oplossingen die u hebt ingeschakeld. 

Als u de computer met System Center 2016 - Operations Manager of Operations Manager 2012 R2, bewaakt kan zijn multihomed met de service voor logboekanalyse voor het verzamelen van gegevens en door te sturen naar de service en nog steeds worden bewaakt door [Operations Manager ](log-analytics-om-agents.md). Linux-computers worden bewaakt door een Operations Manager-beheergroep geïntegreerd met logboekanalyse ontvangen geen configuratie voor gegevensbronnen en voorwaarts verzamelde gegevens via de beheergroep. De Windows-agent kan rapporteren dat maximaal vier werkruimten, terwijl de Linux-agent biedt alleen ondersteuning voor rapportage aan een enkele werkruimte.  

De agent voor Linux en Windows niet alleen voor het verbinden met Log Analytics, het ondersteunt ook Azure Automation host de hybride Runbook worker-rol en oplossingen zoals bijhouden en updatebeheer.  Zie voor meer informatie over de hybride Runbook Worker-rol [Azure Automation Hybrid Runbook Worker](../automation/automation-hybrid-runbook-worker.md).  

## <a name="supported-windows-operating-systems"></a>Ondersteunde Windows-besturingssystemen
De volgende versies van het Windows-besturingssysteem worden officieel ondersteund voor de Windows-agent:

* Windows Server 2008 Service Pack 1 (SP1) of hoger
* Windows 7 SP1 en hoger.

> [!NOTE]
> De agent voor Windows biedt alleen ondersteuning voor Transport Layer Security (TLS) 1.0 en 1.1.  

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen
De volgende Linux-distributies worden officieel ondersteund.  De Linux-agent kan echter ook uitvoeren op andere distributies niet wordt vermeld.  Tenzij anders vermeld, worden alle secundaire versies worden ondersteund voor elke primaire versie vermeld.  

* Amazon Linux 2012.09-2015.09 (x86/x64)
* CentOS Linux 5, 6 en 7 (x86/x64)  
* Oracle Linux 5, 6 en 7 (x86/x64) 
* Red Hat Enterprise Linux Server 5, 6 en 7 (x86/x64)
* Debian GNU/Linux 6, 7 en 8 (x86/x64)
* Ubuntu 12.04 TNS, 14.04 TNS, 16.04 LTS (x86/x64)
* SUSE Linux Enterprise Server 11 en 12 (x86/x64)

## <a name="network-firewall-requirements"></a>Firewall netwerkvereisten
Gegevens van de onderstaande lijst de vereist voor de Linux- en Windows-agent om te communiceren met logboekanalyse proxy- en firewall-configuratie-informatie.  

|Agentresource|Poorten |Richting |HTTPS-controle overslaan|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Poort 443 |Binnenkomend en uitgaand|Ja |  
|*.oms.opinsights.azure.com |Poort 443 |Binnenkomend en uitgaand|Ja |  
|*.blob.core.windows.net |Poort 443 |Binnenkomend en uitgaand|Ja |  
|*.azure-automation.net |Poort 443 |Binnenkomend en uitgaand|Ja |  


Als u van plan bent te gebruiken van Azure Automation Hybrid Runbook Worker verbinding maken met en registreren bij de service Automation runbooks gebruiken in uw omgeving, moet hebben toegang tot het poortnummer en de URL's die worden beschreven [het netwerk configureren voor de Hybride Runbook Worker](../automation/automation-hybrid-runbook-worker.md#network-planning). 

De Windows- en Linux-agent ondersteunt communiceren via een proxy of Gateway naar de Log Analytics-service met behulp van het HTTPS-protocol OMS.  Zowel anonieme verificatie en basisverificatie (gebruikersnaam en wachtwoord) worden ondersteund.  Voor de Windows-agent rechtstreeks verbonden met de service, de proxyconfiguratie is opgegeven tijdens de installatie of [na implementatie](log-analytics-agent-manage.md#update-proxy-settings) vanuit het Configuratiescherm of met PowerShell.  

Voor de Linux-agent de proxyserver is opgegeven tijdens de installatie of [na de installatie](/log-analytics-agent-manage.md#update-proxy-settings) door het wijzigen van het configuratiebestand proxy.conf.  De configuratiewaarde voor Linux-agent-proxy heeft de volgende syntaxis:

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
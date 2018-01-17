---
title: Azure Site Recovery-implementatieplanner voor VMware naar Azure | Microsoft Docs
description: Dit is de gebruikershandleiding voor de Azure Site Recovery-implementatieplanner.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 12/04/2017
ms.author: nisoneji
ms.openlocfilehash: 2985ed0b4bf5d9525bc2274d71b703922524f5a8
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="azure-site-recovery-deployment-planner-for-vmware-to-azure"></a>Azure Site Recovery Deployment Planner voor VMware naar Azure
Dit artikel is de gebruikershandleiding voor de Azure Site Recovery-implementatieplanner voor productie-installaties van het type VMware-naar-Azure.

## <a name="overview"></a>Overzicht

Voordat u een virtuele VMware-machine gaat beveiligen met behulp van Site Recovery, moet u voldoende bandbreedte toewijzen om de gewenste RPO (Recovery Point Objective) te kunnen halen. Bepaal de bandbreedte op basis van de dagelijkse veranderingssnelheid van gegevens. Zorg ervoor dat on-premises het juiste aantal configuratieservers en processervers wordt geïmplementeerd.

Daarnaast is het belangrijk dat u het juiste type en aantal opslagaccounts maakt in Azure. U kunt kiezen tussen Standard- of Premium Storage-accounts, waarbij u rekening moet houden met groei van uw productieservers door toegenomen gebruik na verloop van tijd. U bepaalt per virtuele machine het opslagtype, op basis van kenmerken van de werkbelasting (zoals I/O-bewerkingen voor lezen/schrijven per seconde [IOPS] of gegevensverloop) en limieten van Site Recovery.

De Azure Site Recovery-implementatieplanner is een opdrachtregelprogramma voor zowel Hyper-V naar Azure en VMware naar Azure herstel na noodgevallen. U kunt uw virtuele VMware-machines met dit hulpprogramma op afstand profileren (zonder enige invloed op de productie) om inzicht te krijgen in de vereisten voor bandbreedte en Azure Storage waaraan moet worden voldaan voor een succesvolle replicatie en testfailover. U kunt het hulpprogramma uitvoeren zonder dat u on-premises onderdelen van Site Recovery hoeft te installeren. Voor nauwkeurige doorvoerresultaten wordt echter aangeraden de planner uit te voeren op een Windows-server die voldoet aan de minimale vereisten van de Site Recovery-configuratieserver die u uiteindelijk gaat implementeren als een van de eerste stappen van de productie-implementatie.

Het hulpprogramma levert de volgende gegevens:

**Beoordeling van compatibiliteit**

* Beoordeling van geschiktheid van een VM op basis van het aantal schijven, schijfgrootte, IOPS, verloop, opstarttype (EFI/BIOS) en besturingssysteem
 
**Vereiste netwerkbandbreedte versus RPO-evaluatie**

* De geschatte vereiste bandbreedte in het netwerk voor replicatie van verschillen
* De doorvoer die Site Recovery kan verwerken van on-premises naar Azure
* Het aantal virtuele machines in een batch, op basis van de geschatte bandbreedte die nodig is om initiële replicatie in een bepaalde tijd te voltooien
* RPO die kan worden bereikt voor een bepaalde bandbreedte
* Impact op de gewenste RPO als er een lagere bandbreedte beschikbaar is.

**Vereisten voor Azure-infrastructuur**

* Het vereiste type opslag (Standard- of Premium Storage-account) voor elke virtuele machine
* Het totale aantal Standard- en Premium Storage-accounts dat moet worden ingericht voor replicatie
* Suggesties voor naamgeving van opslagaccounts op basis van Azure Storage-richtlijnen
* De plaatsing van het opslagaccount voor alle virtuele machines
* Het aantal Azure-kerngeheugens dat moet worden ingericht voor testfailover of failover van het abonnement
* De aanbevolen grootte voor virtuele machines van Azure voor elke on-premises VM

**On-premises infrastructuurvereisten**
* Het vereiste aantal configuratieservers en processervers dat on-premises moet worden geïmplementeerd

**Geschatte kosten DR naar Azure**
* Geschatte totaalkosten DR naar Azure: rekensnelheid, opslag, netwerk en Azure Site Recovery-licentiekosten
* Analyse detailkosten per VM


>[!IMPORTANT]
>
>Gezien de verwachte toename van het gebruik in de loop van de tijd, wordt bij alle hierboven genoemde berekeningen een groeifactor van 30 procent gehanteerd voor kenmerken van de werkbelasting en wordt de 95e-percentielwaarde van alle metrische profileringsgegevens (IOPS voor lezen/schrijven, verloop,enzovoort) gebruikt. Beide elementen (groeifactor en percentielberekening) kunnen worden geconfigureerd. Zie de sectie 'Overwegingen voor groeifactor' voor meer informatie over de groeifactor. Zie de sectie 'Percentielwaarde gebruikt voor de berekening' voor meer informatie over de percentielwaarde.
>

## <a name="support-matrix"></a>Ondersteuningsmatrix

| | **VMware naar Azure** |**Hyper-V naar Azure**|**Azure naar Azure**|**Hyper-V naar secundaire site**|**VMware naar secundaire site**
--|--|--|--|--|--
Ondersteunde scenario's |Ja|Ja|Nee|Ja*|Nee
Ondersteunde versie | vCenter 6.5, 6.0 of 5.5| WindowsServer 2016, Windows Server 2012 R2 | N.v.t. |WindowsServer 2016, Windows Server 2012 R2|N.v.t.
Ondersteunde configuratie|vCenter, ESXi| Hyper-V-cluster, Hyper-V-host|N.v.t.|Hyper-V-cluster, Hyper-V-host|N.v.t.|
Aantal servers dat kan worden geprofileerd per exemplaar van de Azure Site Recovery Deployment Planner |Eén (virtuele machines die horen bij een VMware vCenter Server of een ESXi-server kunnen worden geprofileerd op een tijdstip)|Meerdere (VM's op meerdere hosts of hostclusters kunnen tegelijk profiel zijn)| N.v.t. |Meerdere (VM's op meerdere hosts of hostclusters kunnen tegelijk profiel zijn)| N.v.t.

* Het hulpprogramma is voornamelijk bedoeld voor het Hyper-V naar Azure noodherstelscenario. Voor Hyper-V naar secundaire site noodherstel kan deze alleen worden gebruikt voor begrip van aanbevelingen aan de bronzijde, zoals vereiste netwerkbandbreedte, vrije opslagruimte op elk van de Hyper-V-bronservers en de initiële replicatie van batchnummers en batchdefinities.  De Azure-aanbevelingen en de kosten van het rapport negeren. De bewerking Ophalen doorvoer is ook niet van toepassing voor het Hyper-V naar secundaire site noodherstelscenario.

## <a name="prerequisites"></a>Vereisten
Het hulpprogramma heeft twee belangrijke fasen: profileren en rapporten genereren. Er is ook een derde optie, waarmee alleen doorvoer wordt berekend. Raadpleeg de volgende tabel voor de vereisten voor de server waarop de profilering en meting van de doorvoersnelheid worden uitgevoerd:

| Serververeiste | Beschrijving|
|---|---|
|Profileren en meten van doorvoer| <ul><li>Besturingssysteem: Microsoft Windows Server 2016 of Microsoft Windows Server 2012 R2<br>(bij voorkeur met minimaal de [aanbevolen waarden voor de configuratieserver](https://aka.ms/asr-v2a-on-prem-components))</li><li>Machineconfiguratie: 8 vCPU's, 16 GB RAM, 300 GB harde schijf</li><li>[Microsoft .NET Framework 4.5](https://aka.ms/dotnet-framework-45)</li><li>[VMware vSphere PowerCLI 6.0 R3](https://aka.ms/download_powercli)</li><li>[Microsoft Visual C++ Redistributable voor Visual Studio 2012](https://aka.ms/vcplusplus-redistributable)</li><li>Internettoegang tot Azure vanaf deze server</li><li>Azure Storage-account</li><li>Beheerderstoegang op de server</li><li>Minimaal 100 GB vrije schijfruimte (uitgaande van 1000 virtuele machines met een gemiddelde van elk drie schijven, geprofileerd voor 30 dagen)</li><li>Het niveau voor VMware vCenter-statistieken moet worden ingesteld op niveau 2 of hoger</li><li>Poort 443 toestaan: de ASR-implementatieplanner gebruikt deze poort voor verbinding met de vCenter-server/ESXi-host</ul></ul>|
| Rapporten genereren | Een Windows-pc of Windows-server met Microsoft Excel 2013 of hoger |
| Gebruikersmachtigingen | Alleen-lezen-machtiging voor het gebruikersaccount dat tijdens het profileren wordt gebruikt voor toegang tot de VMware vCenter-server of VMware vSphere ESXi-host |

> [!NOTE]
>
>Het hulpprogramma kan alleen worden gebruikt voor het profileren van virtuele machines met VMDK- en RDM-schijven. Profilering van virtuele machines met iSCSI- of NFS-schijven is niet mogelijk. Hoewel iSCSI- en NFS-schijven in Site Recovery wel worden ondersteund voor VMware-servers, kan het hulpprogramma deze schijftypen niet zien. Dit komt doordat de implementatieplanner zich niet in de gast bevindt en voor de profilering alleen prestatiemeteritems van vCenter worden gebruikt.
>

## <a name="download-and-extract-the-deployment-planner-tool"></a>Downloaden en uitpakken van de tool voor implementatieplanning
1. Download de nieuwste versie van de [Azure Site Recovery-implementatieplanner](https://aka.ms/asr-deployment-planner).  
Het hulpprogramma bevindt zich in een gecomprimeerde map. De huidige versie van het hulpprogramma ondersteunt alleen het scenario van VMware naar Azure.

2. Kopieer de gecomprimeerde map naar de Windows-server waarop u het hulpprogramma wilt uitvoeren.  
U kunt het hulpprogramma uitvoeren vanuit Windows Server 2012 R2 als de server netwerktoegang heeft voor verbinding met de vCenter-server of vSphere ESXi-host met de virtuele machines die u wilt profileren. We raden u echter aan het hulpprogramma uit te voeren op een server waarvan de hardwareconfiguratie voldoet aan de [aanbevelingen voor de configuratieserver](https://aka.ms/asr-v2a-on-prem-components). Als u al onderdelen van Site Recovery on-premises hebt geïmplementeerd, moet u het hulpprogramma uitvoeren vanaf de configuratieserver.

 Het is raadzaam om voor de server waarop u het hulpprogramma uitvoert, de hardwareconfiguratie te gebruiken die overeenkomt met de configuratie van de configuratieserver (die een ingebouwde processerver heeft). Een dergelijke configuratie zorgt ervoor dat de behaalde doorvoer die het hulpprogramma aangeeft, overeenkomt met de werkelijke doorvoer die Site Recovery kan bereiken tijdens replicatie. De berekening van de doorvoer is afhankelijk van de beschikbare netwerkbandbreedte op de server en de hardwareconfiguratie van de server (CPU, opslag, enzovoort). Als u het hulpprogramma vanaf een andere server uitvoert, wordt de doorvoer berekend vanaf die server naar Microsoft Azure. Aangezien de hardwareconfiguratie van de server kan verschillen van die van de configuratieserver, is het bovendien mogelijk dat de berekende doorvoer onjuist is.

3. Pak de gecomprimeerde map uit.  
De map bevat meerdere bestanden en submappen. Het uitvoerbare bestand is ASRDeploymentPlanner.exe in de map op het hoogste niveau.

    Voorbeeld:  
    Kopieer het ZIP-bestand naar station E:\ en pak het uit.
   E:\ASR Deployment Planner_v2.1zip

    E:\ASR Deployment Planner_v2.1\ASRDeploymentPlanner.exe

### <a name="updating-to-the-latest-version-of-deployment-planner"></a>Bijwerken naar de nieuwste versie van de implementatieplanner
Als u een eerdere versie van de implementatieplanner hebt, doe dan het volgende:
 * Als de nieuwste versie geen bijgewerkte profileringsvoorziening bevat en er al een profilering wordt uitgevoerd in uw huidige versie van de planner, laat u de profilering gewoon doorgaan.
 * Als de nieuwste versie wel een bijgewerkte profileringsvoorziening bevat, adviseren we om de profilering met de huidige versie te stoppen en opnieuw te starten met de nieuwe versie.


 >[!NOTE]
 >
 >Wanneer u gaat profileren met de nieuwe versie, geef dan dezelfde uitvoerdirectory op zodat het hulpprogramma profielgegevens kan toevoegen aan de bestaande bestanden. Er wordt een volledige set geprofileerde gegevens gebruikt om het rapport te genereren. Als u een andere uitvoerdirectory opgeeft, worden er nieuwe bestanden gemaakt en worden oude profileringsgegevens niet gebruikt bij het genereren van het rapport.
 >
 >Elke nieuwe implementatieplanner is een cumulatieve update van het ZIP-bestand. U hoeft dus niet de nieuwste bestanden naar de vorige map te kopiëren. U kunt een nieuwe map maken en deze gebruiken.


## <a name="version-history"></a>Versiegeschiedenis
De meest recente versie van het hulpprogramma ASR Deployment Planner is 2.1.
Raadpleeg de pagina [Versiegeschiedenis van ASR Deployment Planner](https://social.technet.microsoft.com/wiki/contents/articles/51049.asr-deployment-planner-version-history.aspx) voor de oplossingen die zijn toegevoegd in elke update.

## <a name="next-steps"></a>Volgende stappen
* [De implementatieplanner uitvoeren](site-recovery-vmware-deployment-planner-run.md).

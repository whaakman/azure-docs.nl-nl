---
title: Beveiliging bewaken in Azure Security Center | Microsoft Docs
description: Dit artikel helpt u aan de slag te gaan met de bewakingsmogelijkheden in Azure Security Center.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: 3bd5b122-1695-495f-ad9a-7c2a4cd1c808
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/26/2018
ms.author: terrylan
ms.openlocfilehash: 434b73d4625f86fab195dbda1fed9c841791f5b6
ms.sourcegitcommit: d7725f1f20c534c102021aa4feaea7fc0d257609
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37099456"
---
# <a name="security-health-monitoring-in-azure-security-center"></a>Beveiligingsstatus bewaken in Azure Security Center
Dit artikel bevat informatie over het gebruik van de bewakingsmogelijkheden in Azure Security Center om naleving van het beleid te controleren.

## <a name="what-is-security-health-monitoring"></a>Wat houdt de bewaking van de beveiligingsstatus in?
Bij het woord bewaking wordt misschien gesuggereerd dat er wordt gewacht tot een gebeurtenis plaatsvindt en dat er dan op de situatie wordt gereageerd. Bij beveiligingsbewaking is er echter sprake van een proactieve strategie waarbij uw resources worden gecontroleerd om systemen op te sporen die niet voldoen aan de standaarden of aanbevolen procedures van de organisatie.

## <a name="monitoring-security-health"></a>Beveiligingsstatus bewaken
Nadat u een [beveiligingsbeleid](security-center-policies.md) voor de resources van een abonnement hebt ingeschakeld, analyseert Security Center de beveiliging van uw resources om mogelijke beveiligingsproblemen op te sporen. Informatie over uw netwerkconfiguratie is onmiddellijk beschikbaar. Afhankelijk van het aantal virtuele machines en computers waarop de agent is geïnstalleerd, kan het een uur of langer duren voordat informatie is verzameld over configuratie van de VM's en de computer, zoals de status van de beveiligingsupdates en de configuratie van het besturingssysteem. U kunt de beveiligingsstatus van uw resources en eventuele problemen bekijken in het gedeelte **Preventie**. U kunt ook een overzicht van die problemen op de tegel **Aanbevelingen** bekijken.

Zie [Beveiligingsaanbevelingen implementeren in Azure Security Center](security-center-recommendations.md) voor meer informatie over het toepassen van aanbevelingen.

Onder **Beveiligingsstatus van de resource** kunt u de beveiligingsstatus van uw resources bewaken. In het volgende voorbeeld ziet u dat in de tegel van elke resource (Compute en apps, Netwerken, Gegevensbeveiliging, en Identiteit en toegang) het totale aantal geïdentificeerde problemen weergegeven.

![De tegel Beveiligingsstatus van de resource](./media/security-center-monitoring/security-center-monitoring-fig1-newUI-2017.png)


### <a name="monitor-compute--apps"></a>Compute en apps bewaken
Zie [Uw computers en toepassingen beveiligen in Azure Security Center](security-center-virtual-machine-recommendations.md) voor meer informatie.

### <a name="monitor-virtual-networks"></a>Virtuele netwerken bewaken
Wanneer u op de tegel **Netwerken** klikt, wordt de blade **Netwerken** geopend met meer details, zoals op de volgende schermafbeelding wordt weergegeven:

![De blade Netwerken](./media/security-center-monitoring/security-center-monitoring-fig9-new3.png)

#### <a name="networking-recommendations"></a>Aanbevelingen voor netwerken
Net als bij de informatie over de resourcestatus van virtuele machines ziet u hier bovenin een overzicht van de problemen en onderin een lijst met bewaakte netwerken.

In het gedeelte met een uitsplitsing van de netwerkstatussen vindt u de mogelijke beveiligingsproblemen en [aanbevelingen](security-center-network-recommendations.md). Mogelijke aandachtspunten zijn:

* NGFW (Next Generation Firewall) is niet geïnstalleerd
* De netwerkbeveiligingsgroepen op subnetten zijn niet ingeschakeld
* De netwerkbeveiligingsgroepen op virtuele machines zijn niet ingeschakeld
* Externe toegang via openbaar extern eindpunt beperken
* Status van internetgerichte eindpunten in orde

Wanneer u op een aanbeveling klikt, krijgt u meer informatie over de aanbeveling, zoals weergegeven in het volgende voorbeeld:

![Details voor een aanbeveling in Netwerken](./media/security-center-monitoring/security-center-monitoring-fig9-ga.png)

In dit voorbeeld bevat **Ontbrekende netwerkbeveiligingsgroepen voor subnetten configureren** een lijst met subnetten en virtuele machines zonder beveiliging van netwerkbeveiligingsgroepen. Als u op het subnet klikt waarop u de netwerkbeveiligingsgroep wilt toepassen, wordt **Een netwerkbeveiligingsgroep kiezen** geopend. Hier selecteert u de meest geschikte netwerkbeveiligingsgroep voor het subnet of maakt u een nieuwe netwerkbeveiligingsgroep.

#### <a name="internet-facing-endpoints-section"></a>Sectie Internetgerichte eindpunten
De sectie **Internetgerichte eindpunten** bevat de virtuele machines die momenteel zijn geconfigureerd met een internetgericht eindpunt en de huidige status.

![Virtuele machines die zijn geconfigureerd met een internetgericht eindpunt en status](./media/security-center-monitoring/security-center-monitoring-fig10-ga.png)

Deze tabel bevat de eindpuntnaam die de virtuele machine vertegenwoordigt, het internetgerichte IP-adres en de huidige status van de ernst van de netwerkbeveiligingsgroep en de NGFW. De tabel is gesorteerd op ernst:

* Rood (bovenaan): hoge prioriteit en moet onmiddellijk worden opgelost
* Oranje: gemiddelde prioriteit en moet zo snel mogelijk worden opgelost
* Groen (laatste): integriteitsstatus

#### <a name="networking-topology-section"></a>Sectie Netwerktopologie
In de sectie **Netwerktopologie** ziet u een hiërarchische weergave van de resources, zoals te zien is op de onderstaande schermafbeelding:

![Hiërarchische weergave van resources in de sectie Netwerktopologie](./media/security-center-monitoring/security-center-monitoring-fig121-new4.png)

Deze tabel is gesorteerd (virtuele machines en subnetten) op ernst:

* Rood (bovenaan): hoge prioriteit en moet onmiddellijk worden opgelost
* Oranje: gemiddelde prioriteit en moet zo snel mogelijk worden opgelost
* Groen (laatste): integriteitsstatus

In deze topologieweergave bevat het eerste niveau [virtuele netwerken](../virtual-network/virtual-networks-overview.md), [virtuele netwerkgateways](../vpn-gateway/vpn-gateway-howto-site-to-site-classic-portal.md) en [virtuele netwerken (klassiek)](../virtual-network/virtual-networks-create-vnet-classic-pportal.md). Het tweede niveau bevat subnetten en het derde niveau bevat de virtuele machines die horen bij deze subnetten. In de rechterkolom bevindt zich de huidige status van de netwerkbeveiligingsgroep voor deze resources, zoals in het volgende voorbeeld wordt weergegeven:

![Status van de netwerkbeveiligingsgroep in de sectie Netwerktopologie](./media/security-center-monitoring/security-center-monitoring-fig12-ga.png)

Het onderste gedeelte van deze blade bevat de aanbevelingen voor deze virtuele machine, vergelijkbaar met wat eerder is beschreven. U kunt op een aanbeveling klikken voor meer informatie of om het benodigde beveiligingsbeheer of de benodigde beveiligingsconfiguratie toe te passen.

### <a name="monitor-data-security"></a>Gegevensbeveiliging bewaken

Wanneer u klikt op **Gegevensbeveiliging** in de sectie **Preventie**, wordt **Gegevensbronnen** geopend met aanbevelingen voor SQL en opslag. Ook bevat de blade [aanbevelingen](security-center-sql-service-recommendations.md) voor de algemene integriteitsstatus van de database. Lees voor meer informatie over de versleuteling van opslag [Versleuteling inschakelen voor een Azure-opslagaccount in Azure Security Center](security-center-enable-encryption-for-storage-account.md).

![Gegevensbronnen](./media/security-center-monitoring/security-center-monitoring-fig13-newUI-2017.png)

U kunt onder **SQL-aanbevelingen** op elke aanbeveling klikken voor meer informatie over verdere acties die u kunt ondernemen om een probleem te verhelpen. In het volgende voorbeeld is de aanbeveling **Databasecontrole en detectie van bedreigingen in SQL-databases** uitgevouwen.

![Details over een SQL-aanbeveling](./media/security-center-monitoring/security-center-monitoring-fig14-ga-new.png)

**Controle en detectie van bedreigingen in SQL-databases inschakelen** bevat de volgende informatie:

* Een lijst met SQL-databases
* De server waarop deze zich bevinden
* Informatie over of deze instelling is overgenomen van de server of dat deze uniek is in deze database
* De huidige status
* De ernst van het probleem

Als u op de database klikt om deze aanbeveling op te volgen, wordt **Controle en detectie van bedreigingen** geopend, zoals wordt weergegeven in het volgende scherm.

![Controle en detectie van bedreigingen](./media/security-center-monitoring/security-center-monitoring-fig15-ga.png)

Als u controle wilt inschakelen, hoeft u alleen maar **AAN** te selecteren onder de optie **Controle**.

### <a name="monitor-identity--access"></a>Identiteit en toegang

Zie [Identiteit en toegang bewaken in Azure Security Center](security-center-identity-access.md) voor meer informatie.

## <a name="see-also"></a>Zie ook
In dit artikel hebt u kunnen lezen hoe u de bewakingsmogelijkheden in Azure Security Center gebruikt. Zie de volgende onderwerpen voor meer informatie over het Azure Beveiligingscentrum:

* [Beveiligingsbeleid instellen in Azure Security Center](security-center-policies.md): leer hoe u beveiligingsinstellingen configureert in Azure Security Center.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

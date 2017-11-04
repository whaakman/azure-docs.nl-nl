---
title: Azure Security Center en Windows virtuele machines in Azure | Microsoft Docs
description: Meer informatie over de beveiliging van uw Windows Azure virtuele machine met Azure Security Center.
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 05/01/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: adb00e28b0b204858a763f83836ee2ac96f8f9e4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="monitor-virtual-machine-security-by-using-azure-security-center"></a>Beveiliging van de virtuele machine bewaken met behulp van Azure Security Center

Azure Security Center kunt u meer inzicht verkrijgen in uw Azure-resource procedures voor beveiliging. Security Center biedt geïntegreerde beveiligingsbewaking. Bedreigingen die anders onopgemerkt kunnen worden gedetecteerd. In deze zelfstudie hebt u meer informatie over Azure Security Center en hoe:
 
> [!div class="checklist"]
> * Verzamelen van gegevens instellen
> * Een beveiligingsbeleid instellen
> * Weergeven en oplossen van problemen met configuratie-status
> * Bekijk de gedetecteerde bedreigingen  

## <a name="security-center-overview"></a>Security Center-overzicht

Beveiligingscentrum identificeert mogelijke configuratieproblemen van de virtuele machine (VM) en gerichte beveiligingsrisico's. Hierbij kunnen virtuele machines met netwerkbeveiligingsgroepen, niet-versleutelde schijven en beveiligingsaanvallen Remote Desktop Protocol (RDP ontbrekende). De informatie wordt weergegeven op het Security Center-dashboard in grafieken gemakkelijk te lezen.

Selecteer voor toegang tot het dashboard Security Center in de Azure portal, in het menu **Security Center**. Op het dashboard kunt u zien van de beveiligingsstatus van uw Azure-omgeving, een telling van de huidige aanbevelingen vinden en de huidige status van de threat waarschuwingen weergeven. U kunt elke grafiek op hoog niveau voor meer details uitbreiden.

![Security Center-dashboard](./media/tutorial-azure-security/asc-dash.png)

Security Center zich verder uitstrekt dan data detectie aanbevelingen voor problemen die worden gedetecteerd. Bijvoorbeeld, als een virtuele machine zonder een gekoppelde netwerkbeveiligingsgroep is geïmplementeerd, Security Center wordt weergegeven een aanbeveling, met herstelstappen die u kunt nemen. Automatisch doorvoeren krijgt u zonder de context van Security Center.  

![Aanbevelingen](./media/tutorial-azure-security/recommendations.png)

## <a name="set-up-data-collection"></a>Verzamelen van gegevens instellen

Voordat u VM-beveiligingsconfiguraties zichtbaarheid krijgen kunt, moet u voor het instellen van de gegevensverzameling Security Center. Dit omvat het inschakelen van gegevensverzameling en het maken van een Azure storage-account voor het opslaan van verzamelde gegevens. 

1. Klik op het dashboard Security Center **beveiligingsbeleid**, en selecteer vervolgens uw abonnement. 
2. Voor **gegevensverzameling**, selecteer **op**.
3. Voor het maken van een opslagaccount selecteert **een opslagaccount kiezen**. Selecteer **OK**.
4. Op de **beveiligingsbeleid** blade Selecteer **opslaan**. 

De Security Center-agent voor het verzamelen van gegevens is geïnstalleerd op alle virtuele machines en begint met het verzamelen van gegevens. 

## <a name="set-up-a-security-policy"></a>Een beveiligingsbeleid instellen

Beveiligingsbeleid worden gebruikt voor het definiëren van de items waarvoor Security Center worden gegevens verzameld en worden aanbevelingen gedaan. U kunt verschillende beveiligingsbeleid toepassen op verschillende sets van Azure-resources. Hoewel Azure-resources worden standaard geëvalueerd tegen alle beleidsitems, kunt u afzonderlijke beleidsitems voor alle Azure-resources of voor een resourcegroep uitschakelen. Zie voor gedetailleerde informatie over Security Center-beveiligingsbeleid [beveiligingsbeleid instellen in Azure Security Center](../../security-center/security-center-policies.md). 

Een beveiligingsbeleid voor alle Azure-resources instellen:

1. Selecteer op het dashboard Security Center **beveiligingsbeleid**, en selecteer vervolgens uw abonnement.
2. Selecteer **preventiebeleid**.
3. Inschakelen of uitschakelen beleidsitems die u wilt toepassen op alle Azure-resources.
4. Wanneer u klaar bent uw instellingen te selecteren, selecteert u **OK**.
5. Op de **beveiligingsbeleid** blade Selecteer **opslaan**. 

Voor het instellen van een beleid voor een specifieke resourcegroep:

1. Selecteer op het dashboard Security Center **beveiligingsbeleid**, en selecteer vervolgens een resourcegroep.
2. Selecteer **preventiebeleid**.
3. Inschakelen of uitschakelen beleidsitems die u wilt toepassen op de resourcegroep.
4. Onder **overname**, selecteer **unieke**.
5. Wanneer u klaar bent uw instellingen te selecteren, selecteert u **OK**.
6. Op de **beveiligingsbeleid** blade Selecteer **opslaan**.  

Ook kunt u uitschakelen gegevensverzameling voor een specifieke resourcegroep op deze pagina.

In het volgende voorbeeld wordt een uniek beleid is gemaakt voor een resourcegroep met de naam *myResoureGroup*. Schijf versleutelings- en web application firewall aanbevelingen zijn in dit beleid uitgeschakeld.

![Unieke beleidsnaam](./media/tutorial-azure-security/unique-policy.png)

## <a name="view-vm-configuration-health"></a>Configuratiestatus van weergave VM

Nadat u hebt ingeschakeld op het verzamelen van gegevens en een beveiligingsbeleid instellen, begint Security Center waarschuwingen en aanbevelingen biedt. Als u virtuele machines zijn geïmplementeerd, wordt de agent voor het verzamelen van gegevens is geïnstalleerd. Security Center wordt vervolgens gevuld met gegevens voor de nieuwe virtuele machines. Zie voor gedetailleerde informatie over de configuratiestatus VM [beveiligen van uw virtuele machines in Security Center](../../security-center/security-center-virtual-machine-recommendations.md). 

Als gegevens worden verzameld, wordt de resourcestatus voor elke virtuele machine en de gerelateerde Azure-resource geaggregeerd. De informatie wordt weergegeven in het diagram van een gemakkelijk te lezen. 

Resourcestatus weergeven:

1.  Op het Security Center-dashboard onder **resourcebeveiligingsstatus**, selecteer **Compute**. 
2.  Op de **Compute** blade Selecteer **virtuele machines**. Deze weergave bevat een samenvatting van de configuratiestatus van alle virtuele machines.

![Status berekenen](./media/tutorial-azure-security/compute-health.png)

Als u wilt zien alle aanbevelingen voor een VM, selecteer de virtuele machine. Aanbevelingen en het doorvoeren worden behandeld in meer detail in de volgende sectie van deze zelfstudie.

## <a name="remediate-configuration-issues"></a>Problemen met de configuratie herstellen

Nadat het Beveiligingscentrum gestart te vullen met configuratiegegevens, zijn aanbevelingen gemaakt op basis van het instellen van beveiligingsbeleid. Als een virtuele machine zonder een gekoppelde netwerkbeveiligingsgroep is ingesteld, wordt een aanbeveling te maken van een uitgevoerd. 

Om een lijst weer met alle aanbevelingen: 

1. Selecteer op het dashboard Security Center **aanbevelingen**.
2. Selecteer een specifieke aanbeveling. Een lijst van alle resources waarvan de aanbeveling is van toepassing wordt weergegeven.
3. Als u wilt toepassen op een aanbeveling, selecteer een specifieke bron. 
4. Volg de instructies voor de herstelstappen. 

In veel gevallen biedt Security Center bruikbare stappen die u uitvoeren kunt voor het oplossen van een aanbeveling zonder Security Center. Security Center detecteert in het volgende voorbeeld wordt een netwerkbeveiligingsgroep met een onbeperkte inkomende regel. Op de pagina aanbeveling kunt u de **binnenkomende regels bewerken** knop. De gebruikersinterface die nodig is voor het wijzigen van de regel wordt weergegeven. 

![Aanbevelingen](./media/tutorial-azure-security/remediation.png)

Als de aanbevelingen zijn hersteld, worden ze gemarkeerd als opgelost. 

## <a name="view-detected-threats"></a>Weergave van gedetecteerde bedreigingen

Security Center bevat naast de aanbevelingen voor de resource configuratie dagelijks geconstateerde waarschuwingen. De waarschuwingsfunctie beveiliging aggregeert gegevens verzameld van elke VM Azure VPN-logboeken en verbonden partneroplossingen bedreigingen van Azure-resources te detecteren. Zie voor gedetailleerde informatie over Security Center threat detectiemogelijkheden [Azure Security Center detectiemogelijkheden](../../security-center/security-center-detection-capabilities.md).

De waarschuwingsfunctie beveiliging vereist het Security Center prijscategorie worden verhoogd van *vrije* naar *standaard*. Een 30-daagse **gratis proefversie** is beschikbaar wanneer u naar deze hogere prijscategorie. 

De prijscategorie wijzigen:  

1. Klik op het dashboard Security Center **beveiligingsbeleid**, en selecteer vervolgens uw abonnement.
2. Selecteer **prijscategorie**.
3. Selecteer de nieuwe laag en selecteer vervolgens **Selecteer**.
4. Op de **beveiligingsbeleid** blade Selecteer **opslaan**. 

Nadat u de prijscategorie hebt gewijzigd, begint de beveiliging waarschuwingen grafiek te vullen beveiliging dreigingen worden gedetecteerd.

![Beveiligingswaarschuwingen](./media/tutorial-azure-security/security-alerts.png)

Selecteer een waarschuwing om informatie te bekijken. Bijvoorbeeld, ziet u een beschrijving van de bedreiging, de tijd van de detectie, alle threat pogingen en het aanbevolen herstel. In het volgende voorbeeld wordt is een RDP-brute-force-aanval gedetecteerd, met 294 mislukte pogingen voor RDP. Een aanbevolen oplossing is opgegeven.

![RDP-aanval](./media/tutorial-azure-security/rdp-attack.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie kunt u instellen in Azure Security Center en vervolgens geëvalueerd VM's in Security Center. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Verzamelen van gegevens instellen
> * Een beveiligingsbeleid instellen
> * Weergeven en oplossen van problemen met configuratie-status
> * Bekijk de gedetecteerde bedreigingen

Ga naar de volgende zelfstudie voor informatie over het maken van een CI/CD-pijplijn met Visual Studio Team Services en een Windows-VM waarop IIS wordt uitgevoerd.

> [!div class="nextstepaction"]
> [Visual Studio Team Services CI/CD-pipeline](./tutorial-vsts-iis-cicd.md)

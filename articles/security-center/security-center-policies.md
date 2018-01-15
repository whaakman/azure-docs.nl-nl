---
title: Beveiligingsbeleid instellen in Azure Security Center | Microsoft Docs
description: In dit artikel leest u hoe u beveiligingsbeleid configureert in Azure Security Center.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: 3b9e1c15-3cdb-4820-b678-157e455ceeba
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2018
ms.author: yurid
ms.openlocfilehash: 4d1a30b046c0c398d934291a907af891e9ac7fdf
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="set-security-policies-in-azure-security-center"></a>Beveiligingsbeleid instellen in Azure Security Center
In dit artikel leest u hoe u beveiligingsbeleid configureert in Security Center.

## <a name="how-security-policies-work"></a>Hoe beveiligingsbeleid werkt
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. In Security Center kunt u het beleid bewerken en controleren of het beleid wordt nageleefd.

> [!NOTE]
> U kunt Security Center-beleid nu uitbreiden met behulp van Azure-beleid, waarvan een beperkte preview beschikbaar is. Als u wilt deelnemen aan de preview, gaat u naar [Registreren voor Azure Policy](https://aka.ms/getpolicy). Zie [Integratie van Security Center-beveiligingsbeleid met Azure Policy](security-center-azure-policy.md) voor meer informatie.

De beveiligingsvereisten voor resources die worden gebruikt voor ontwikkeling of tests kunnen verschillen van de vereisten voor resources die worden gebruikt voor productietoepassingen. Voor toepassingen die gereglementeerde gegevens gebruiken, zoals persoonsgegevens, kan een hoger beveiligingsniveau vereist zijn. Beveiligingsbeleid dat in Azure Security Center is ingeschakeld, bepaalt de beveiligingsaanbevelingen en bewaking om u te helpen mogelijke beveiligingsproblemen te identificeren en bedreigingen te verhelpen. Lees de [Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) (Plannings- en bedieningsgids voor Azure Security Center) voor meer informatie over hoe u kunt bepalen welke optie het best is voor u.

## <a name="edit-security-policies"></a>Beveiligingsbeleid bewerken
U kunt het standaardbeveiligingsbeleid voor elk van uw Azure-abonnementen bewerken in Security Center. Het beveiligingsbeleid kan alleen worden gewijzigd door een eigenaar, bijdrager of beveiligingsbeheerder van het abonnement. Als u uw beveiligingsbeleid wilt configureren in Security Center, doet u het volgende:

1. Meld u aan bij Azure Portal.

2. Ga op het dashboard van **Security Center** naar **Algemeen** en selecteer **Beveiligingsbeleid**.

3. Selecteer het abonnement waarvoor u een beveiligingsbeleid wilt inschakelen.

4. Klik in het gedeelte **Beleidsonderdelen** op **Beveiligingsbeleid**.  
    Dit is het standaardbeleid dat door Security Center wordt toegewezen. U kunt de beschikbare beveiligingsaanbevelingen in- of uitschakelen.

5. Selecteer **Opslaan** als u klaar bent.

## <a name="available-security-policy-definitions"></a>Beschikbare beveiligingsbeleiddefinities

Bekijk de volgende tabel om inzicht te krijgen in de beleidsdefinities die beschikbaar zijn in het standaardbeveiligingsbeleid:

| Beleid | Betekenis van het beleid |
| --- | --- |
| Systeemupdates |Hiermee wordt via Windows Update of Windows Server Update Services een dagelijkse lijst opgehaald van beschikbare beveiligingsupdates en essentiële updates. De opgehaalde lijst varieert per service die is geconfigureerd voor uw virtuele machines, en bevat aanbevelingen voor het toepassen van ontbrekende updates. Voor Linux-systemen maakt het beleid gebruik van het door de distributie beschikbaar gestelde pakketbeheersysteem om te bepalen voor welke pakketten er updates beschikbaar zijn. Ook wordt bij [virtuele Azure Cloud Services-machines](../cloud-services/cloud-services-how-to-configure-portal.md) gecontroleerd of er beveiligingsupdates en essentiële updates zijn. |
| Beveiligingsconfiguraties |Hiermee worden dagelijks besturingssysteemconfiguraties gecontroleerd om te bepalen of er problemen zijn die de virtuele machine kwetsbaar kan maken voor aanvallen. Vanuit het beleid zal ook worden aangeraden om deze beveiligingslekken aan te pakken door wijzigingen in de configuratie aan te brengen. Zie de [lijst met aanbevolen basislijnen](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor meer informatie over de specifieke configuraties die worden bewaakt. (Op dit moment wordt Windows Server 2016 niet volledig ondersteund.) |
| Eindpuntbeveiliging |Hiermee wordt aanbevolen dat voor alle virtuele machines in Windows eindpuntbeveiliging wordt ingesteld om virussen, spyware en andere schadelijke software op te sporen en te verwijderen. |
| Schijfversleuteling |Hiermee wordt aanbevolen om schijfversleuteling in te schakelen in alle virtuele machines om de beveiliging van opgeslagen gegevens te verbeteren. |
| Netwerkbeveiligingsgroepen |Hiermee wordt aanbevolen om [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) te configureren om binnenkomend en uitgaand verkeer naar VM's met openbare eindpunten te beheren. Netwerkbeveiligingsgroepen die zijn geconfigureerd voor een subnet, worden overgenomen door alle netwerkinterfaces van virtuele machines, tenzij anders wordt aangegeven. Behalve dat wordt gecontroleerd of een netwerkbeveiligingsgroep is geconfigureerd, worden met deze beleidsregel ook de beveiligingsregels voor binnenkomend verkeer beoordeeld die binnenkomend verkeer toestaan. |
| Web Application Firewall |Hiermee wordt aanbevolen om een Web Application Firewall in te stellen op virtuele machines wanneer een van de volgende omstandigheden waar is: <ul><li>Er wordt een [openbaar IP op exemplaarniveau](../virtual-network/virtual-networks-instance-level-public-ip.md) gebruikt, en de beveiligingsregels voor binnenkomend verkeer van de gekoppelde netwerkbeveiligingsgroep worden geconfigureerd om toegang tot poort 80/443 toe te staan.</li><li>Er wordt een IP met taakverdeling gebruikt en de bijbehorende taakverdeling en de inkomende NAT-regels (Network Address Translation) worden geconfigureerd om toegang tot poort 80/443 toe te staan. Zie [Azure Resource Manager-ondersteuning voor load balancer](../load-balancer/load-balancer-arm.md) voor meer informatie.</li> |
| Next Generation Firewall |Hiermee wordt meer netwerkbeveiliging toegevoegd dan met de netwerkbeveiligingsgroepen die in Azure zijn ingebouwd. Security Center detecteert implementaties waarvoor een Next Generation Firewall wordt aanbevolen. Vervolgens kunt u een virtuele toepassing instellen. |
| Controleren voor SQL en bedreigingen detecteren |Hiermee wordt aanbevolen dat de controle van toegang tot de SQL-database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek. |
| SQL-versleuteling |Hiermee wordt aanbevolen dat versleuteling-at-rest wordt ingeschakeld voor uw SQL-database, gekoppelde back-ups en transactielogboekbestanden. Zelfs bij een inbreuk kunnen uw gegevens niet worden gelezen. |
| Evaluatie van beveiligingsproblemen |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| Storage-versleuteling |Deze functie is momenteel beschikbaar voor blobs en Azure Files. Nadat Storage-serviceversleuteling is ingeschakeld, worden alleen nieuwe gegevens versleuteld. Alle bestaande bestanden in dit opslagaccount zijn nog steeds niet-versleuteld. |
| JIT-netwerktoegang |Wanneer JIT-netwerktoegang is ingeschakeld, wordt binnenkomend verkeer naar de Azure-VM's vergrendeld via Security Center door een beveiligingsgroepregel te maken. U selecteert de poorten op de VM waarop binnenkomend verkeer moet worden vergrendeld. Zie [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (VM-toegang beheren met behulp van JIT) voor meer informatie. |


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center:

* [Gids voor de planning en werking van Azure Security Center](security-center-planning-and-operations-guide.md): leer de ontwerpoverwegingen kennen en leer hiervoor te plannen voor Azure Security Center.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

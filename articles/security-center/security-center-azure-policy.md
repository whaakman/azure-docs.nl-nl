---
title: Integratie van Azure Security Center-beveiligingsbeleid met Azure Policy | Microsoft Docs
description: In dit document leest u hoe u integratie van Azure Security Center-beveiligingsbeleid configureert met Azure Policy.
services: security-center
documentationcenter: na
author: YuriDio
manager: mbaldwin
editor: 
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/05/2018
ms.author: yurid
ms.openlocfilehash: 5dedad4fa3695d1b210e1174d8f29966d2259889
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="integrate-security-center-security-policies-with-azure-policy"></a>Integratie van Security Center-beveiligingsbeleid met Azure Policy
In dit artikel leest u hoe u Azure Security Center-beveiligingsbeleid configureert met Azure Policy.

## <a name="how-security-policies-work"></a>Hoe beveiligingsbeleid werkt
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. U kunt het beleid bewerken in Security Center of [Azure Policy](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) gebruiken om het volgende doen:
* Nieuwe beleidsdefinities maken.
* Beleid toepassen binnen beheergroepen, die de hele organisatie of een zakelijke eenheid binnen de organisatie kunnen vertegenwoordigen.
* Beleidsnaleving bewaken.

> [!NOTE]
> Azure Policy is alleen beschikbaar als beperkte preview-versie. Als u zich wilt registreren, gaat u naar [Registreren voor Azure Policy](https://aka.ms/getpolicy). Lees [Beleid maken en beheren om naleving af te dwingen](http://docs.microsoft.com/azure/azure-policy/create-manage-policy) voor meer informatie over Azure Policy.

## <a name="edit-security-policies"></a>Beveiligingsbeleid bewerken
U kunt het standaardbeveiligingsbeleid voor elk van uw Azure-abonnementen bewerken in Security Center. U kunt een beveiligingsbeleid alleen wijzigen als u een eigenaar, bijdrager of beveiligingsbeheerder van dat abonnement bent of van de bovenliggende beheergroep. Als u uw beveiligingsbeleid wilt weergeven in Security Center, doet u het volgende:

1. Meld u aan bij Azure Portal.

2. Ga op het dashboard van **Security Center** naar **Algemeen** en selecteer **Beveiligingsbeleid**.

    ![Het deelvenster Beleidsbeheer](./media/security-center-azure-policy/security-center-policies-fig10.png)

3. Selecteer het abonnement waarvoor u een beveiligingsbeleid wilt inschakelen.  

4. Klik in het gedeelte **Beleidsonderdelen** op **Beveiligingsbeleid**.  
    Het venster **Basisbeginselen** wordt geopend.

    ![Beleidsonderdelen](./media/security-center-azure-policy/security-center-policies-fig12.png)

5. Als u een beleidsdefinitie wilt verwijderen, selecteert u onder **Beleidsregels en parameters** de definitie die u wilt verwijderen. Selecteer vervolgens **Verwijderen**.

6. Klik op **Opslaan**.  
    Het venster **Beschikbare definities** wordt geopend en geeft het standaardbeleid weer dat is toegewezen aan Security Center via Azure Policy.

7. (Optioneel) In het venster **Beschikbare definities** voert u een van de volgende handelingen uit:

    * Selecteer het plusteken (+) naast de definitie om een beleidsdefinitie toe te voegen.

    ![Beschikbare beleidsdefinities](./media/security-center-azure-policy/security-center-policies-fig11.png)

    * Selecteer een beleid voor een gedetailleerde beschrijving ervan.  
    Het venster **Preview** van de definitie wordt geopend. Hier wordt een beschrijving van de definitie weergegeven en een koppeling naar de JSON-code waarmee de structuur van de [beleidsdefinitie](../azure-policy/policy-definition.md) wordt opgegeven.

    ![Het preview-venster van de definitie](./media/security-center-azure-policy/security-center-policies-fig14.png)

7. Selecteer **Opslaan** als u klaar bent.

## <a name="available-security-policy-definitions"></a>Beschikbare beveiligingsbeleiddefinities

Bekijk de volgende tabel om inzicht te krijgen in de beleidsdefinities die beschikbaar zijn in het standaardbeveiligingsbeleid:

| Beleid | Wat het ingeschakelde beleid doet |
| --- | --- |
| Systeemupdates |Hiermee wordt via Windows Update of Windows Server Update Services een dagelijkse lijst opgehaald van beschikbare beveiligingsupdates en essentiële updates. De opgehaalde lijst varieert per service die is geconfigureerd voor uw virtuele machines, en bevat aanbevelingen voor het toepassen van ontbrekende updates. Voor Linux-systemen maakt het beleid gebruik van het door de distributie beschikbaar gestelde pakketbeheersysteem om te bepalen voor welke pakketten er updates beschikbaar zijn. Ook wordt bij [virtuele Azure Cloud Services-machines](../cloud-services/cloud-services-how-to-configure-portal.md) gecontroleerd of er beveiligingsupdates en essentiële updates zijn. |
| Beveiligingsconfiguraties |Hiermee worden dagelijks besturingssysteemconfiguraties gecontroleerd om te bepalen of er problemen zijn die de virtuele machine kwetsbaar kan maken voor aanvallen. Vanuit het beleid zal ook worden aangeraden om deze beveiligingslekken aan te pakken door wijzigingen in de configuratie aan te brengen. Zie de [lijst met aanbevolen basislijnen](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor meer informatie over de specifieke configuraties die worden bewaakt. (Op dit moment wordt Windows Server 2016 niet volledig ondersteund.) |
| Eindpuntbeveiliging |Hiermee wordt aanbevolen dat voor alle virtuele machines in Windows eindpuntbeveiliging wordt ingesteld om virussen, spyware en andere schadelijke software op te sporen en te verwijderen. |
| Schijfversleuteling |Hiermee wordt aanbevolen om schijfversleuteling in te schakelen in alle virtuele machines om de beveiliging van opgeslagen gegevens te verbeteren. |
| Netwerkbeveiligingsgroepen |Hiermee wordt aanbevolen om [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) te configureren om binnenkomend en uitgaand verkeer naar VM's met openbare eindpunten te beheren. Netwerkbeveiligingsgroepen die zijn geconfigureerd voor een subnet, worden overgenomen door alle netwerkinterfaces van virtuele machines, tenzij anders wordt aangegeven. Behalve dat wordt gecontroleerd of een netwerkbeveiligingsgroep is geconfigureerd, worden met deze beleidsregel ook de beveiligingsregels voor binnenkomend verkeer beoordeeld die binnenkomend verkeer toestaan. |
| Web Application Firewall |Hiermee wordt aanbevolen om een Web Application Firewall in te stellen op virtuele machines wanneer een van de volgende omstandigheden waar is: <ul><li>Er wordt een [openbaar IP op exemplaarniveau](../virtual-network/virtual-networks-instance-level-public-ip.md) gebruikt, en de beveiligingsregels voor binnenkomend verkeer van de gekoppelde netwerkbeveiligingsgroep worden geconfigureerd om toegang tot poort 80/443 toe te staan.</li><li>Er wordt een IP met taakverdeling gebruikt en de bijbehorende taakverdeling en de inkomende NAT-regels (Network Address Translation) worden geconfigureerd om toegang tot poort 80/443 toe te staan. Zie [Azure Resource Manager-ondersteuning voor load balancer](../load-balancer/load-balancer-arm.md) voor meer informatie.</li> |
| Next Generation Firewall |Hiermee wordt meer netwerkbeveiliging toegevoegd dan met de netwerkbeveiligingsgroepen die in Azure zijn ingebouwd. Security Center detecteert implementaties waarvoor een Next Generation Firewall wordt aanbevolen. Vervolgens kunt u een virtuele toepassing instellen. |
| Controleren voor SQL en bedreigingen detecteren |Hiermee wordt aanbevolen dat de controle van toegang tot de Azure-database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek. |
| SQL-versleuteling |Hiermee wordt aanbevolen dat versleuteling-at-rest wordt ingeschakeld voor uw Azure SQL-databases, gekoppelde back-ups en transactielogboekbestanden. Zelfs bij een inbreuk kunnen uw gegevens niet worden gelezen. |
| Evaluatie van beveiligingsproblemen |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| Storage-versleuteling |Deze functie is momenteel beschikbaar voor Azure-blobopslag en Azure Files. Nadat Storage-serviceversleuteling is ingeschakeld, worden alleen nieuwe gegevens versleuteld. Alle bestaande bestanden in dit opslagaccount zijn nog steeds niet-versleuteld. |
| JIT-netwerktoegang |Wanneer JIT-netwerktoegang is ingeschakeld, wordt binnenkomend verkeer naar de Azure-VM's vergrendeld via Security Center door een beveiligingsgroepregel te maken. U selecteert de poorten op de VM waarop binnenkomend verkeer moet worden vergrendeld. Zie [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (VM-toegang beheren met behulp van JIT) voor meer informatie. |


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center:

* [Gids voor de planning en werking van Azure Security Center](security-center-planning-and-operations-guide.md): leer de ontwerpoverwegingen kennen en leer hiervoor te plannen voor Azure Security Center.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

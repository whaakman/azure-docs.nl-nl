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
ms.date: 10/13/2017
ms.author: yurid
ms.openlocfilehash: 5e07cd6891a5ab04012f819b5f6b9379312e530d
ms.sourcegitcommit: 5d772f6c5fd066b38396a7eb179751132c22b681
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/13/2017
---
# <a name="set-security-policies-in-security-center-powered-by-azure-policy"></a>Beveiligingsbeleid instellen in Security Center, mogelijk gemaakt door Azure Policy
Dit document helpt u om beveiligingsbeleid in Security Center te configureren, dat mogelijk wordt gemaakt door Azure Policy, door u te begeleiden bij de stappen die nodig zijn om deze taak uit te voeren. 


## <a name="how-security-policies-work"></a>Hoe werkt beveiligingsbeleid?
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. U kunt het beleid bewerken in Security Center of [Azure Policy](http://docs.microsoft.com/azure/azure-policy/azure-policy-introduction) gebruiken voor het maken van nieuwe beleidsdefinities, het toewijzen van beleidsregels binnen beheergroepen (die de hele organisatie, een bedrijfseenheid, enzovoort kan vertegenwoordigen) en het bewaken van naleving van deze beleidsregels.

> [!NOTE]
> Azure Policy is alleen beschikbaar als beperkte preview-versie. Klik [hier](https://aka.ms/getpolicy) om de preview te gebruiken. Lees [Beleid maken en beheren om naleving af te dwingen](http://docs.microsoft.com/en-us/azure/azure-policy/create-manage-policy) voor meer informatie over Azure-beleid.

## <a name="edit-security-policies"></a>Beveiligingsbeleid bewerken
U kunt het standaardbeveiligingsbeleid voor elk van uw Azure-abonnementen bewerken in Security Center. U kunt een beveiligingsbeleid alleen wijzigen als u een eigenaar, bijdrager of beveiligingsbeheerder van dat abonnement bent of van de bovenliggende beheergroep. Meld u aan bij Azure Portal en voer de volgende stappen uit om beveiligingsbeleid te bekijken in Security Center:

1. Ga op het dashboard van **Security Center** naar **Algemeen** en klik op **Beveiligingsbeleid**.
2. Selecteer het abonnement waarvoor u het beveiligingsbeleid wilt inschakelen.

    ![Beleidsbeheer](./media/security-center-policies/security-center-policies-fig10.png)

3. Klik in het gedeelte **BELEIDSONDERDELEN** op **Beveiligingsbeleid**.

    ![Beleidsonderdelen](./media/security-center-policies/security-center-policies-fig12.png)

4. Dit is het standaardbeleid dat via Azure Policy is toegewezen aan Security Center. U kunt items verwijderen die onder **BELEIDSREGELS EN PARAMETERS** staan, of u kunt andere beleidsdefinities toevoegen die onder **BESCHIKBARE OPTIES** staan. Klik hiervoor op het plusteken (+) naast de naam van de definitie.

    ![Beleidsdefinities](./media/security-center-policies/security-center-policies-fig11.png)

5. Als u meer gedetailleerde informatie over het beleid wilt, klikt u erop om een pagina te openen met de details, evenals de JSON-code met de structuur van de [beleidsdefinitie](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-policy/#policy-definition-structure):

    ![Json](./media/security-center-policies/security-center-policies-fig14.png)

6. Klik op **Opslaan** als u klaar bent.


## <a name="available-security-policy-definitions"></a>Beschikbare beveiligingsbeleiddefinities

Gebruik de volgende tabel ter referentie om inzicht te krijgen in de beleidsdefinities die beschikbaar zijn in het standaardbeveiligingsbeleid: 

| Beleid | Wanneer de status is ingesteld op aan |
| --- | --- |
| Systeemupdates |Hiermee wordt via Windows Update of Windows Server Update Services een dagelijkse lijst opgehaald van beschikbare beveiligingsupdates en essentiële updates. De opgehaalde lijst varieert per service die is geconfigureerd voor de desbetreffende virtuele machine, en bevat aanbevelingen voor het toepassen van ontbrekende updates. Voor Linux-systemen maakt het beleid gebruik van het door de distributie beschikbaar gestelde pakketbeheersysteem om te bepalen voor welke pakketten er updates beschikbaar zijn. Ook wordt bij [virtuele Azure Cloud Services-machines](../cloud-services/cloud-services-how-to-configure.md) gecontroleerd of er beveiligingsupdates en essentiële updates zijn. |
| Beveiligingsproblemen van besturingssystemen |Hiermee worden dagelijks besturingssysteemconfiguraties gecontroleerd om te bepalen of er problemen zijn die de virtuele machine kwetsbaar kan maken voor aanvallen. Vanuit het beleid zal ook worden aangeraden om deze beveiligingslekken aan te pakken door wijzigingen in de configuratie aan te brengen. Zie de [lijst met aanbevolen basislijnen](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor meer informatie over de specifieke configuraties die worden bewaakt. (Op dit moment wordt Windows Server 2016 niet volledig ondersteund.) |
| Eindpuntbeveiliging |Hiermee wordt aanbevolen dat voor alle virtuele machines in Windows eindpuntbeveiliging wordt ingericht om virussen, spyware en andere schadelijke software op te sporen en te verwijderen. |
| Schijfversleuteling |Hiermee wordt aanbevolen om schijfversleuteling in te schakelen in alle virtuele machines om de beveiliging van opgeslagen gegevens te verbeteren. |
| Netwerkbeveiligingsgroepen |Hiermee wordt aanbevolen om [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) te configureren om binnenkomend en uitgaand verkeer naar VM's met openbare eindpunten te beheren. Netwerkbeveiligingsgroepen die zijn geconfigureerd voor een subnet, worden overgenomen door alle netwerkinterfaces van virtuele machines, tenzij anders wordt aangegeven. Behalve dat wordt gecontroleerd of een netwerkbeveiligingsgroep is geconfigureerd, worden met deze beleidsregel ook de beveiligingsregels voor binnenkomend verkeer beoordeeld die binnenkomend verkeer toestaan. |
| Web Application Firewall |Hiermee wordt aanbevolen om een Web Application Firewall in te richten op virtuele machines wanneer een van de volgende omstandigheden waar is: </br></br>[Openbaar IP op exemplaarniveau](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) wordt gebruikt, en de beveiligingsregels voor binnenkomend verkeer van de gekoppelde netwerkbeveiligingsgroep worden geconfigureerd om toegang tot poort 80/443 toe te staan.</br></br>IP met taakverdeling wordt gebruikt en de bijbehorende taakverdeling en de inkomende NAT-regels (Network Address Translation worden geconfigureerd om toegang tot poort 80/443 toe te staan. (Zie [Azure Resource Manager-ondersteuning voor load balancer](../load-balancer/load-balancer-arm.md) voor meer informatie.) |
| Next Generation Firewall |Hiermee wordt meer netwerkbeveiliging toegevoegd dan met de netwerkbeveiligingsgroepen die in Azure zijn ingebouwd. Security Center detecteert implementaties waarvoor een Next Generation Firewall wordt aanbevolen en biedt u de mogelijkheid een virtuele toepassing in te richten. |
| Controleren voor SQL en bedreigingen detecteren |Hiermee wordt aanbevolen dat de controle van toegang tot de Azure-database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek. |
| SQL-versleuteling |Hiermee wordt aanbevolen dat versleuteling-at-rest wordt ingeschakeld voor uw Azure SQL-databases, gekoppelde back-ups en transactielogboekbestanden. Zelfs bij een inbreuk kunnen uw gegevens niet worden gelezen. |
| Beoordeling van beveiligingslekken |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| Storage-versleuteling |Deze functie is momenteel beschikbaar voor Azure-blobs en -bestanden. Nadat Storage-serviceversleuteling is ingeschakeld, worden alleen nieuwe gegevens versleuteld. Alle bestaande bestanden in dit opslagaccount zijn nog steeds niet-versleuteld. |
| JIT-netwerktoegang |Wanneer JIT is ingeschakeld, wordt binnenkomend verkeer naar de Azure-VM's vergrendeld via Security Center door een NSG-regel te maken. U selecteert de poorten op de VM waarop binnenkomend verkeer moet worden vergrendeld. Zie [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (VM-toegang beheren met behulp van JIT) voor meer informatie. |


## <a name="next-step"></a>Volgende stap
In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md). Leer de ontwerpoverwegingen kennen en leer deze in te plannen als u de overstap naar Azure Security Center wilt maken.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md). Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.

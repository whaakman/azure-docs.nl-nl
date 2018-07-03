---
title: Integratie van Azure Security Center-beveiligingsbeleid met Azure Policy | Microsoft Docs
description: In dit document leest u hoe u integratie van Azure Security Center-beveiligingsbeleid configureert met Azure Policy.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: mbaldwin
editor: ''
ms.assetid: cd906856-f4f9-4ddc-9249-c998386f4085
ms.service: security-center
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/21/2018
ms.author: terrylan
ms.openlocfilehash: b3d6d15d41fece613290deb2c77e980caa5dcfef
ms.sourcegitcommit: 0fa8b4622322b3d3003e760f364992f7f7e5d6a9
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/27/2018
ms.locfileid: "37018560"
---
# <a name="integrate-security-center-security-policies-with-azure-policy"></a>Integratie van Security Center-beveiligingsbeleid met Azure Policy
In dit artikel leest u hoe u Azure Security Center-beveiligingsbeleid configureert met [Azure Policy](../azure-policy/azure-policy-introduction.md).

## <a name="how-security-policies-work"></a>Hoe beveiligingsbeleid werkt
In Security Center wordt voor elk van uw Azure-abonnementen automatisch een standaardbeveiligingsbeleid gemaakt. U kunt het beleid bewerken in Security Center of Azure Policy gebruiken om het volgende doen:
- Nieuwe beleidsdefinities maken.
- Beleid toepassen binnen beheergroepen en abonnementen; deze kunnen de hele organisatie of een zakelijke eenheid binnen de organisatie vertegenwoordigen.
- Beleidsnaleving bewaken.

Lees [Beleid maken en beheren om naleving af te dwingen](../azure-policy/create-manage-policy.md) voor meer informatie over Azure Policy.

De Azure-beleid bestaat uit de volgende onderdelen:

- Een **beleid** is een regel
- Een **initiatief** is een verzameling beleidsregels
- Een **toewijzing** is een toepassing van een initiatief of een beleid op een bepaalde scope (beheergroep, abonnement of resourcegroep)

Een resource wordt geëvalueerd op basis van de beleidsregels die eraan zijn toegewezen en krijgt een waardering op basis van het aantal beleidsregels waaraan de resource voldoet.

## <a name="edit-security-policies"></a>Beveiligingsbeleid bewerken
U kunt het standaardbeveiligingsbeleid voor elk van uw Azure-abonnementen en beheergroepen bewerken in Security Center. U kunt een beveiligingsbeleid alleen wijzigen als u een eigenaar, bijdrager of beveiligingsbeheerder van dat abonnement bent of van de bovenliggende beheergroep. Ga als volgt te werk als u uw beveiligingsbeleidsregels wilt weergeven in Security Center:

1. Ga in het dashboard van **Security Center** naar **BELEID EN NALEVING** en selecteer **Beveiligingsbeleid**. **Beleidsbeheer** wordt geopend.

    ![Het deelvenster Beleidsbeheer](./media/security-center-azure-policy/security-center-policies-fig10.png)

  In Beleidsbeheer wordt het aantal beheergroepen, abonnementen en werkruimten weergegeven, evenals de structuur van uw managementgroep.

  > [!NOTE]
  > Het is mogelijk dat in het Security Center-dashboard een hoger aantal abonnementen onder **Abonnementsdekking** wordt weergegeven dan het aantal abonnementen dat wordt weergegeven onder **beleidsbeheer**. Abonnementsdekking toont het aantal Standaard-, gratis en 'Niet gedekt' abonnementen. Voor de 'Niet gedekt'-abonnementen is Security Center niet ingeschakeld. Deze worden niet weergegeven onder **Beleidsbeheer**.
  >
  >

  De kolommen in de tabel bevat de volgende gegevens:

 - Toewijzing beleidsinitiatieven: in Security Center ingebouwde beleidsregels en initiatieven die zijn toegewezen aan een abonnement of beheergroep.
 - Naleving: algemene nalevingsscore voor een beheergroep, abonnement of werkruimte. De score is het gewogen gemiddelde van de toewijzingen. Bij het gewogen gemiddelde wordt rekening gehouden met het aantal beleidsregels in één toewijzing en het aantal resources waarop de toewijzing van toepassing is.

 Als uw abonnement bijvoorbeeld twee virtuele machines heeft en een initiatief waaraan vijf beleidsregels zijn toegewezen, dan hebt u 10 beoordelingen in uw abonnement. Als een van de virtuele machines niet aan twee van de beleidsregels voldoet, is de algemene nalevingsscore van uw abonnementstoewijzing 80%.

 - Dekking: toont de prijscategorie, Gratis of Standaard, waarin de beheergroep, het abonnement of de werkruimte wordt uitgevoerd.  Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
 - Instellingen: abonnementen hebben een koppeling **Instellingen bewerken**. Wanneer u **Instellingen bewerken** selecteert, kunt u uw abonnementsinstellingen bewerken, instellingen voor het verzamelen van gegevens, de prijscategorie en e-mailmeldingen.

2. Selecteer het abonnement of de beheergroep waarvoor u een beveiligingsbeleid wilt inschakelen. **Beveiligingsbeleid** wordt geopend.

3.  Selecteer onder **Beveiligingsbeleid** de gewenste besturingselementen die u met Security Center wilt bewaken en waarvoor u aanbevelingen wilt krijgen, door **Aan** te selecteren.  Selecteer **Uit** als u niet wilt dat het besturingselement door Security Center wordt bewaakt.

    ![Beleidsonderdelen](./media/security-center-azure-policy/security-policy.png)

4. Selecteer **Opslaan**.

## <a name="available-security-policy-definitions"></a>Beschikbare beveiligingsbeleiddefinities

Bekijk de volgende tabel om inzicht te krijgen in de beleidsdefinities die beschikbaar zijn in het standaardbeveiligingsbeleid:

| Beleid | Wat het ingeschakelde beleid doet |
| --- | --- |
| Systeemupdates |Hiermee wordt via Windows Update of Windows Server Update Services een dagelijkse lijst opgehaald van beschikbare beveiligingsupdates en essentiële updates. De opgehaalde lijst varieert per service die is geconfigureerd voor uw virtuele machines, en bevat aanbevelingen voor het toepassen van ontbrekende updates. Voor Linux-systemen maakt het beleid gebruik van het door de distributie beschikbaar gestelde pakketbeheersysteem om te bepalen voor welke pakketten er updates beschikbaar zijn. Ook wordt bij [virtuele Azure Cloud Services-machines](../cloud-services/cloud-services-how-to-configure-portal.md) gecontroleerd of er beveiligingsupdates en essentiële updates zijn. |
| Beveiligingsconfiguraties |Hiermee worden dagelijks besturingssysteemconfiguraties gecontroleerd om te bepalen of er problemen zijn die de virtuele machine kwetsbaar kan maken voor aanvallen. Vanuit het beleid zal ook worden aangeraden om deze beveiligingslekken aan te pakken door wijzigingen in de configuratie aan te brengen. Zie de [lijst met aanbevolen basislijnen](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor meer informatie over de specifieke configuraties die worden bewaakt. (Op dit moment wordt Windows Server 2016 niet volledig ondersteund.) |
| Eindpuntbeveiliging |Hiermee wordt aanbevolen dat voor alle virtuele machines in Windows eindpuntbeveiliging wordt ingesteld om virussen, spyware en andere schadelijke software op te sporen en te verwijderen. |
| Schijfversleuteling |Hiermee wordt aanbevolen om schijfversleuteling in te schakelen in alle virtuele machines om de beveiliging van opgeslagen gegevens te verbeteren. |
| Netwerkbeveiligingsgroepen |Hiermee wordt aanbevolen om [netwerkbeveiligingsgroepen](../virtual-network/security-overview.md) te configureren om binnenkomend en uitgaand verkeer naar VM's met openbare eindpunten te beheren. Netwerkbeveiligingsgroepen die zijn geconfigureerd voor een subnet, worden overgenomen door alle netwerkinterfaces van virtuele machines, tenzij anders wordt aangegeven. Behalve dat wordt gecontroleerd of een netwerkbeveiligingsgroep is geconfigureerd, worden met deze beleidsregel ook de beveiligingsregels voor binnenkomend verkeer beoordeeld die binnenkomend verkeer toestaan. |
| Web Application Firewall |Hiermee wordt aanbevolen om een Web Application Firewall in te stellen op virtuele machines wanneer een van de volgende zaken waar is: <ul><li>Er wordt een [openbaar IP op exemplaarniveau](../virtual-network/virtual-networks-instance-level-public-ip.md) gebruikt, en de beveiligingsregels voor binnenkomend verkeer van de gekoppelde netwerkbeveiligingsgroep worden geconfigureerd om toegang tot poort 80/443 toe te staan.</li><li>Er wordt een IP met taakverdeling gebruikt en de bijbehorende taakverdeling en de inkomende NAT-regels (Network Address Translation) worden geconfigureerd om toegang tot poort 80/443 toe te staan. Zie [Azure Resource Manager-ondersteuning voor load balancer](../load-balancer/load-balancer-arm.md) voor meer informatie.</li> |
| Next Generation Firewall |Hiermee wordt meer netwerkbeveiliging toegevoegd dan met de netwerkbeveiligingsgroepen die in Azure zijn ingebouwd. Security Center detecteert implementaties waarvoor een Next Generation Firewall wordt aanbevolen. Vervolgens kunt u een virtuele toepassing instellen. |
| Controleren voor SQL en bedreigingen detecteren |Hiermee wordt aanbevolen dat de controle van toegang tot de Azure-database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek. |
| SQL-versleuteling |Hiermee wordt aanbevolen dat versleuteling-at-rest wordt ingeschakeld voor uw Azure SQL-databases, gekoppelde back-ups en transactielogboekbestanden. Zelfs bij een inbreuk kunnen uw gegevens niet worden gelezen. |
| Evaluatie van beveiligingsproblemen |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| Storage-versleuteling |Deze functie is momenteel beschikbaar voor Azure-blobopslag en Azure Files. Nadat Storage-serviceversleuteling is ingeschakeld, worden alleen nieuwe gegevens versleuteld. Alle bestaande bestanden in dit opslagaccount zijn nog steeds niet-versleuteld. |
| JIT-netwerktoegang |Wanneer JIT-netwerktoegang is ingeschakeld, wordt binnenkomend verkeer naar de Azure-VM's vergrendeld via Security Center door een beveiligingsgroepregel te maken. U selecteert de poorten op de VM waarop binnenkomend verkeer moet worden vergrendeld. Zie [Manage virtual machine access using just in time](https://docs.microsoft.com/azure/security-center/security-center-just-in-time) (VM-toegang beheren met behulp van JIT) voor meer informatie. |

## <a name="management-groups"></a>Beheergroepen
Als uw organisatie veel abonnementen heeft, moet u de toegang, beleidsregels en naleving voor deze abonnementen op een efficiënte manier kunnen beheren. Azure Beheergroepen biedt een scopeniveau boven abonnementen. U organiseert abonnementen in containers, zogenaamde 'beheergroepen', en past uw governance-beleid op de beheergroepen toe. Alle abonnementen in een beheergroep nemen automatisch het beleid over dat op de beheergroep is toegepast. Elke directory krijgt één beheergroep op het hoogste niveau, de 'hoofdbeheergroep'. Deze hoofdbeheergroep is zo in de hiërarchie ingebouwd dat alle beheergroepen en abonnementen hierin zijn opgevouwen. Met deze hoofdbeheergroep kunt u algemene beleidsregels en RBAC-toewijzingen op directoryniveau toepassen. Als u beheergroepen wilt instellen voor gebruik met Azure Security Center, volgt u de instructies in het artikel [Tenantbrede zichtbaarheid verkrijgen voor Azure Security Center](security-center-management-groups.md). 

> [!NOTE]
> Het is belangrijk dat u de hiërarchie van beheergroepen en abonnementen begrijpt. Zie [Organize your resources with Azure Management Groups](../azure-resource-manager/management-groups-overview.md#root-management-group-for-each-directory) (Resources organiseren met Azure Beheergroepen) voor meer informatie over beheergroepen, hoofdbeheer en toegang tot beheergroepen.
>
>


## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u kunnen lezen hoe u het beveiligingsbeleid in Security Center configureert. Zie de volgende artikelen voor meer informatie over Security Center:

* [Gids voor de planning en werking van Azure Security Center](security-center-planning-and-operations-guide.md): leer de ontwerpoverwegingen kennen en leer hiervoor te plannen voor Azure Security Center.
* [Beveiligingsstatus controleren in Azure Security Center](security-center-monitoring.md): meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md): leer hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen controleren met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt controleren.
* [Tenantbrede zichtbaarheid verkrijgen voor Azure Security Center](security-center-management-groups.md): informatie over het instellen van beheergroepen voor Azure Security Center. 
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/): lees blogberichten over de beveiliging en naleving van Azure.

Zie [Wat is Azure Policy?](../azure-policy/azure-policy-introduction.md) voor meer informatie over Azure Policy.

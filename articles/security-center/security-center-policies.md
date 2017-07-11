---
title: Beveiligingsbeleid instellen in Azure Security Center | Microsoft Docs
description: In dit document leest u hoe u beveiligingsbeleid configureert in Azure Security Center.
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
ms.date: 06/16/2017
ms.author: yurid
ms.translationtype: Human Translation
ms.sourcegitcommit: ff2fb126905d2a68c5888514262212010e108a3d
ms.openlocfilehash: 2593e6846c897644017083b49ad4ba8219696c6c
ms.contentlocale: nl-nl
ms.lasthandoff: 06/17/2017


---
<a id="set-security-policies-in-azure-security-center" class="xliff"></a>

# Beveiligingsbeleid instellen in Azure Security Center
Dit document helpt u om beveiligingsbeleid in Security Center te configureren door u te begeleiden bij de stappen die nodig zijn om deze taak uit te voeren.

>[!NOTE] 
>Vanaf begin juni 2017 zal Security Center de Microsoft Monitoring Agent gebruiken voor het verzamelen en opslaan van gegevens. Zie [Migratie van Azure Security Center-platform](security-center-platform-migration.md) voor meer informatie. De informatie in dit artikel beschrijft functionaliteit van Security Center na de overstap naar de Microsoft Monitoring Agent.
>

<a id="what-are-security-policies" class="xliff"></a>

## Wat is beveiligingsbeleid?
Een beveiligingsbeleid bepaalt welke set besturingselementen wordt aanbevolen voor resources binnen het opgegeven abonnement. In Security Center definieert u beleid voor uw Azure-abonnementen overeenkomstig de behoeften van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement.

Zo kunnen er voor resources die worden gebruikt voor ontwikkeling of tests, andere beveiligingsvereisten zijn dan voor resources die worden gebruikt voor productietoepassingen. Ook kan voor toepassingen met gereglementeerde gegevens, zoals persoonsgegevens, een hoger beveiligingsniveau vereist zijn. Beveiligingsbeleid dat in Azure Security Center is ingeschakeld, bepaalt de beveiligingsaanbevelingen en bewaking om u te helpen mogelijke beveiligingsproblemen te identificeren en bedreigingen te verhelpen. Lees de [Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) (Plannings- en bedieningsgids voor Azure Security Center) voor meer informatie over hoe u kunt bepalen welke optie het geschiktst is voor u.

<a id="set-security-policies" class="xliff"></a>

## Beveiligingsbeleid instellen
U kunt voor elk abonnement beveiligingsbeleid configureren. Het beveiligingsbeleid kan alleen worden gewijzigd door een eigenaar of bijdrager van het abonnement. Meld u aan bij de Azure-portal en voer de volgende stappen uit om beveiligingsbeleid te configureren in Security Center:

1. Klik op de tegel **Beleid** op het dashboard van Security Center.
2. Selecteer in de blade Beveiligingsbeleid die wordt weergegeven, het abonnement waarvoor u het beveiligingsbeleid wilt inschakelen.

    ![Beleid definiëren](./media/security-center-policies/security-center-policies-fig1-ga.png)
3. De blade **Beveiligingsbeleid** voor het geselecteerde abonnement wordt geopend met een reeks opties. De opties die beschikbaar zijn op deze blade:

   * **Preventiebeleid**: met deze optie kunt u beleidsregels per abonnement of resourcegroep configureren.  
   * **E-mailmelding**: gebruik deze optie als u wilt dat er een e-mailbericht wordt verzonden de eerste keer dat een waarschuwing op die dag plaatsvindt, en voor waarschuwingen met een hoge urgentie. E-mailvoorkeuren kunnen alleen worden geconfigureerd voor abonnementsbeleid. Lees [Contactgegevens voor beveiliging verstrekken in Azure Security Center](security-center-provide-security-contact-details.md) voor meer informatie over het configureren van een e-mailmelding.
   * **Prijscategorie**: gebruik deze optie om een upgrade van de geselecteerde prijscategorie uit te voeren. Zie de pagina [Security Center-prijzen](security-center-pricing.md) voor meer informatie over tariefopties.
4. Zorg ervoor dat de optie **Gegevens van virtuele machines verzamelen** is ingesteld op **Aan**. Met deze optie kunt u automatisch logboekgegevens verzamelen voor bestaande en nieuwe resources, met behulp van Microsoft Monitoring Agent. Dit is dezelfde agent die ook wordt gebruikt door de Operations Management Suite en Log Analytics-service. Gegevens die via deze agent worden verzameld, worden opgeslagen in een bestaande Log Analytics-werkruimte die is gekoppeld aan uw Azure-abonnement, of in nieuwe werkruimten, rekening houdend met de geografische locatie van de virtuele machine.

5. Klik in de blade **Beveiligingsbeleid** op **Preventiebeleid** om de beschikbare opties te zien. Klik op **Aan** om de beveiligingsaanbevelingen in te schakelen die relevant zijn voor dit abonnement.

    ![Het beveiligingsbeleid selecteren](./media/security-center-policies/security-center-policies-fig4-newUI.png)

Gebruik de volgende tabel als referentie om te begrijpen wat elke optie doet:

| Beleid | Wanneer de status is ingesteld op aan |
| --- | --- |
| Systeemupdates |Hiermee wordt via Windows Update of Windows Server Update Services een dagelijkse lijst opgehaald van beschikbare beveiligingsupdates en essentiële updates. De opgehaalde lijst varieert per service die is geconfigureerd voor de desbetreffende virtuele machine, en bevat aanbevelingen voor het toepassen van ontbrekende updates. Voor Linux-systemen maakt het beleid gebruik van het door de distributie beschikbaar gestelde pakketbeheersysteem om te bepalen voor welke pakketten er updates beschikbaar zijn. Ook wordt bij [virtuele Azure Cloud Services-machines](../cloud-services/cloud-services-how-to-configure.md) gecontroleerd of er beveiligingsupdates en essentiële updates zijn. |
| Beveiligingsproblemen van besturingssystemen |Hiermee worden dagelijks besturingssysteemconfiguraties gecontroleerd om te bepalen of er problemen zijn die de virtuele machine kwetsbaar kan maken voor aanvallen. Vanuit het beleid zal ook worden aangeraden om deze beveiligingslekken aan te pakken door wijzigingen in de configuratie aan te brengen. Zie de [lijst met aanbevolen basislijnen](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor meer informatie over de specifieke configuraties die worden bewaakt. (Op dit moment wordt Windows Server 2016 niet volledig ondersteund.) |
| Eindpuntbeveiliging |Hiermee wordt aanbevolen dat voor alle virtuele machines in Windows eindpuntbeveiliging wordt ingericht om virussen, spyware en andere schadelijke software op te sporen en te verwijderen. |
| Schijfversleuteling |Hiermee wordt aanbevolen om schijfversleuteling in te schakelen in alle virtuele machines om de beveiliging van opgeslagen gegevens te verbeteren. |
| Netwerkbeveiligingsgroepen |Hiermee wordt aanbevolen om [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) te configureren om binnenkomend en uitgaand verkeer naar VM's met openbare eindpunten te beheren. Netwerkbeveiligingsgroepen die worden geconfigureerd voor een subnet, worden overgenomen door alle netwerkinterfaces van virtuele machines, tenzij anders is aangegeven. Behalve dat wordt gecontroleerd of een netwerkbeveiligingsgroep is geconfigureerd, worden met deze beleidsregel ook de beveiligingsregels voor binnenkomend verkeer beoordeeld die binnenkomend verkeer toestaan. |
| Web Application Firewall |Hiermee wordt aanbevolen om een Web Application Firewall in te richten op virtuele machines wanneer een van de volgende omstandigheden waar is: </br></br>[Openbaar IP op exemplaarniveau](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) wordt gebruikt, en de beveiligingsregels voor binnenkomend verkeer van de gekoppelde netwerkbeveiligingsgroep worden geconfigureerd om toegang tot poort 80/443 toe te staan.</br></br>IP met taakverdeling wordt gebruikt en de bijbehorende taakverdeling en de inkomende NAT-regels (Network Address Translation worden geconfigureerd om toegang tot poort 80/443 toe te staan. (Zie [Azure Resource Manager-ondersteuning voor load balancer](../load-balancer/load-balancer-arm.md) voor meer informatie.) |
| Next Generation Firewall |Hiermee wordt meer netwerkbeveiliging toegevoegd dan met de netwerkbeveiligingsgroepen die in Azure zijn ingebouwd. Security Center detecteert implementaties waarvoor een Next Generation Firewall wordt aanbevolen en biedt u de mogelijkheid een virtuele toepassing in te richten. |
| Controleren voor SQL en bedreigingen detecteren |Hiermee wordt aanbevolen dat de controle van toegang tot de Azure-database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek. |
| SQL-versleuteling |Hiermee wordt aanbevolen dat versleuteling-at-rest wordt ingeschakeld voor uw Azure SQL-databases, gekoppelde back-ups en transactielogboekbestanden. Zelfs bij een inbreuk kunnen uw gegevens niet worden gelezen. |
| Beoordeling van beveiligingslekken |Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |
| Storage-versleuteling |Deze functie is momenteel beschikbaar voor Azure-blobs en -bestanden. Opmerking: Nadat de service Storage-versleuteling is ingeschakeld, worden alleen nieuwe gegevens versleuteld. Alle bestaande bestanden in dit Storage-account zijn nog steeds niet-versleuteld. |

Wanneer u klaar bent met het configureren van alle opties, klikt u op **OK** in de blade **Beveiligingsbeleid** met de aanbevelingen en klikt u vervolgens op **Opslaan** in de blade **Beveiligingsbeleid** met de oorspronkelijke instellingen.

> [!NOTE]
> De prijscategorie is nog steeds van toepassing op het niveau van de resourcegroep. Bezoek voor meer informatie de [pagina met prijzen](https://azure.microsoft.com/pricing/details/security-center/).
>
>

<a id="see-also" class="xliff"></a>

## Zie ook
In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid configureert in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

* [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md). Leer de ontwerpoverwegingen kennen en leer deze in te plannen als u de overstap naar Azure Security Center wilt maken.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
* [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md). Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
* [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.


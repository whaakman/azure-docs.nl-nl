<properties
   pageTitle="Beveiligingsbeleid instellen in Azure Security Center | Microsoft Azure"
   description="In dit document leest u hoe u beveiligingsbeleid configureert in Azure Security Center."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="09/22/2016"
   ms.author="yurid"/>


# Beveiligingsbeleid instellen in Azure Security Center
Dit document helpt u om beveiligingsbeleid in Security Center te configureren door u te begeleiden bij de stappen die nodig zijn om deze taak uit te voeren.

## Wat is beveiligingsbeleid?
Een beveiligingsbeleid bepaalt welke set besturingselementen wordt aanbevolen voor resources binnen het opgegeven abonnement of de opgegeven resourcegroep. In Security Center definieert u beleid voor uw Azure-abonnementen of -resourcegroep overeenkomstig de behoeften van uw bedrijf en het type toepassingen of de vertrouwelijkheid van de gegevens in elk abonnement.

Zo kunnen er voor resources die worden gebruikt voor ontwikkeling of tests, andere beveiligingsvereisten zijn dan voor resources die worden gebruikt voor productietoepassingen. Ook kan voor toepassingen met gereglementeerde gegevens, zoals persoonsgegevens, een hoger beveiligingsniveau vereist zijn. Beveiligingsbeleid dat in Azure Security Center is ingeschakeld, bepaalt de beveiligingsaanbevelingen en bewaking om u te helpen mogelijke beveiligingsproblemen te identificeren en bedreigingen te verhelpen. Lees de [Azure Security Center Planning and Operations Guide](security-center-planning-and-operations-guide.md) (Plannings- en bedieningsgids voor Azure Security Center) voor meer informatie over hoe u kunt bepalen welke optie het geschiktst is voor u.

## Beveiligingsbeleid instellen voor abonnementen

U kunt voor elk abonnement en elke resourcegroep beveiligingsbeleid configureren. Het beveiligingsbeleid kan alleen worden gewijzigd door een eigenaar of bijdrager van het abonnement. Meld u aan bij de Azure-portal en voer de volgende stappen uit om beveiligingsbeleid te configureren in Security Center:

1. Klik op de tegel **Beleid** op het dashboard van Security Center.

2. Selecteer in de blade **Beveiligingsbeleid - Het beleid per abonnement of resourcegroep definiëren** die wordt weergegeven, het abonnement waarvoor u het beveiligingsbeleid wilt inschakelen. Als u een beveiligingsbeleid voor een resourcegroep wilt inschakelen in plaats van voor het hele abonnement, bladert u omlaag naar de volgende sectie waar het instellen van beveiligingsbeleid voor resourcegroepen wordt besproken.

    ![Beleid definiëren](./media/security-center-policies/security-center-policies-fig1-ga.png)

3. De blade **Beveiligingsbeleid** voor het geselecteerde abonnement wordt geopend met een reeks opties die vergelijkbaar zijn met de opties in de volgende schermafbeelding:

    ![Gegevensverzameling inschakelen](./media/security-center-policies/security-center-policies-fig2-ga.png)

    De opties die beschikbaar zijn op deze blade:
    - **Preventiebeleid**: met deze optie kunt u beleidsregels per abonnement of resourcegroep configureren.  
    - **E-mailmelding**: gebruik deze optie als u wilt dat er een e-mailbericht wordt verzonden de eerste keer dat een waarschuwing op die dag plaatsvindt, en voor waarschuwingen met een hoge urgentie. E-mailvoorkeuren kunnen alleen worden geconfigureerd voor abonnementsbeleid. Lees [Contactgegevens voor beveiliging verstrekken in Azure Security Center](security-center-provide-security-contact-details.md) voor meer informatie over het configureren van een e-mailmelding.
    - **Prijscategorie**: gebruik deze optie om een upgrade van de geselecteerde prijscategorie uit te voeren. Zie de pagina [Security Center](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie over tariefopties.


4.  Zorg ervoor dat de optie **Gegevens van virtuele machines verzamelen** is ingesteld op **Aan**. Met deze optie worden er automatisch logboekgegevens verzameld voor bestaande en nieuwe resources.

    >[AZURE.NOTE] U kunt het beste gegevensverzameling inschakelen voor al uw abonnementen, omdat dit ervoor zorgt dat beveiligingsbewaking beschikbaar is voor alle bestaande en nieuwe VM's. Bij het inschakelen van gegevensverzameling wordt de bewakingsagent geïnstalleerd. Als u het verzamelen van gegevens nu niet vanaf deze locatie wilt inschakelen, kunt u dit later doen vanuit de weergaven voor de **status** en **aanbevelingen**. U kunt het verzamelen van gegevens ook alleen voor het abonnement of bepaalde VM's inschakelen. Zie [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center) voor meer informatie over de ondersteunde VM's.

5. Als uw opslagaccount nog niet is geconfigureerd, wordt mogelijk een waarschuwing weergegeven zoals die in de volgende schermafbeelding, wanneer u **Beveiligingsbeleid** opent:

    ![Opslagselectie](./media/security-center-policies/security-center-policies-fig2.png)

6. Als deze waarschuwing wordt weergegeven, klikt u op deze optie en selecteert u de regio zoals in de volgende schermafbeelding:

    ![Opslagselectie](./media/security-center-policies/security-center-policies-fig3-ga.png)

7. Voor elke regio waarin u virtuele machines uitvoert, kiest u het opslagaccount waarin de gegevens worden opgeslagen die zijn verzameld bij deze virtuele machines. Hierdoor wordt het eenvoudig om gegevens in hetzelfde geografische gebied te bewaren met het oog op privacy en ten behoeve van de onafhankelijkheid van de gegevens. Nadat u heb besloten welke regio u wilt gebruiken, selecteert u de regio en selecteert u vervolgens het opslagaccount.

8. Klik in de blade **Opslagaccounts kiezen** op **OK**.

    > [AZURE.NOTE] U kunt de gegevens van virtuele machines in verschillende regio's desgewenst samenvoegen in één centraal opslagaccount. Zie [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center) voor meer informatie.

9. Klik in de blade **Beveiligingsbeleid** op **Aan** om de beveiligingsaanbevelingen in te schakelen die u wilt gebruiken voor dit abonnement. Klik op **Preventiebeleid** om opties weer te geven zoals die in de volgende schermafbeelding:

    ![Het beveiligingsbeleid selecteren](./media/security-center-policies/security-center-policies-fig4-ga-new.png)

Gebruik de volgende tabel als referentie om te begrijpen wat elke optie doet:

| Beleid | Wanneer de status is ingesteld op aan |
|----- |-----|
| Systeemupdates | Hiermee wordt via Windows Update of Windows Server Update Services een dagelijkse lijst opgehaald van beschikbare beveiligingsupdates en essentiële updates. De opgehaalde lijst varieert per service die is geconfigureerd voor de desbetreffende virtuele machine, en bevat aanbevelingen voor het toepassen van ontbrekende updates. Voor Linux-systemen maakt het beleid gebruik van het door de distributie beschikbaar gestelde pakketbeheersysteem om te bepalen voor welke pakketten er updates beschikbaar zijn. Ook wordt bij [virtuele Azure Cloud Services-machines](./cloud-services/cloud-services-how-to-configure.md) gecontroleerd of er beveiligingsupdates en essentiële updates zijn. |
| Beveiligingsproblemen van besturingssystemen | Hiermee worden dagelijks besturingssysteemconfiguraties gecontroleerd om te bepalen of er problemen zijn die de virtuele machine kwetsbaar kan maken voor aanvallen. Vanuit het beleid zal ook worden aangeraden om deze beveiligingslekken aan te pakken door wijzigingen in de configuratie aan te brengen. Zie de [lijst met aanbevolen basislijnen](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) voor meer informatie over de specifieke configuraties die worden bewaakt. |
| Eindpuntbeveiliging | Hiermee wordt aanbevolen dat voor alle virtuele machines in Windows eindpuntbeveiliging wordt ingericht om virussen, spyware en andere schadelijke software op te sporen en te verwijderen.|
| Schijfversleuteling | Hiermee wordt aanbevolen om schijfversleuteling in te schakelen in alle virtuele machines om de beveiliging van opgeslagen gegevens te verbeteren.
| Netwerkbeveiligingsgroepen | Hiermee wordt aanbevolen om [netwerkbeveiligingsgroepen](../virtual-network/virtual-networks-nsg.md) te configureren om binnenkomend en uitgaand verkeer naar VM's met openbare eindpunten te beheren. Netwerkbeveiligingsgroepen die worden geconfigureerd voor een subnet, worden overgenomen door alle netwerkinterfaces van virtuele machines, tenzij anders is aangegeven. Behalve dat wordt gecontroleerd of een netwerkbeveiligingsgroep is geconfigureerd, worden met deze beleidsregel ook de beveiligingsregels voor binnenkomend verkeer beoordeeld die binnenkomend verkeer toestaan. |
| Web Application Firewall | Hiermee wordt aanbevolen om een Web Application Firewall in te richten op virtuele machines wanneer een van de volgende omstandigheden waar is:</br></br>[Openbaar IP op exemplaarniveau](../virtual-network/virtual-networks-instance-level-public-ip.md) (ILPIP) wordt gebruikt, en de beveiligingsregels voor binnenkomend verkeer van de gekoppelde netwerkbeveiligingsgroep worden geconfigureerd om toegang tot poort 80/443 toe te staan.</br></br>IP met taakverdeling wordt gebruikt en de bijbehorende taakverdeling en de inkomende NAT-regels (Network Address Translation worden geconfigureerd om toegang tot poort 80/443 toe te staan. (Zie [Azure Resource Manager-ondersteuning voor load balancer](../load-balancer/load-balancer-arm.md) voor meer informatie.) |
| Next Generation Firewall | Hiermee wordt meer netwerkbeveiliging toegevoegd dan met de netwerkbeveiligingsgroepen die in Azure zijn ingebouwd. Security Center detecteert implementaties waarvoor een Next Generation Firewall wordt aanbevolen en biedt u de mogelijkheid een virtuele toepassing in te richten. |
| Controleren voor SQL en bedreigingen detecteren | Hiermee wordt aanbevolen dat de controle van toegang tot de Azure-database wordt ingeschakeld voor naleving, geavanceerde detectie en onderzoek. |
| Transparent Data Encryption voor SQL | Hiermee wordt aanbevolen dat versleuteling-at-rest wordt ingeschakeld voor uw Azure SQL-databases, gekoppelde back-ups en transactielogboekbestanden. Zelfs bij een inbreuk kunnen uw gegevens niet worden gelezen. |
| Beoordeling van beveiligingslekken | Hiermee wordt aanbevolen dat een oplossing voor de beoordeling van beveiligingslekken wordt geïnstalleerd op de VM. |

Wanneer u klaar bent met het configureren van alle opties, klikt u op **OK** in de blade **Beveiligingsbeleid** met de aanbevelingen en klikt u vervolgens op **Opslaan** in de blade **Beveiligingsbeleid** met de oorspronkelijke instellingen.

## Beveiligingsbeleid instellen voor resourcegroepen

Als u liever per resourcegroep een beveiligingsbeleid configureert, zijn de stappen gelijk aan de stappen die u hebt gebruikt voor het instellen van beveiligingsbeleid voor abonnementen. Het belangrijkste verschil is dat u de naam van het abonnement moet uitvouwen en de resourcegroep moet selecteren waarvoor u het unieke beveiligingsbeleid wilt configureren:

![Resourcegroep selecteren](./media/security-center-policies/security-center-policies-fig5-ga.png)

Nadat u de resourcegroep hebt geselecteerd, wordt de blade **Beveiligingsbeleid** geopend. De optie **Overname** is standaard ingeschakeld. Dat betekent dat alle beveiligingsbeleidsregels voor deze resourcegroep worden overgenomen van het abonnementsniveau. U kunt deze configuratie wijzigen als u een aangepast beveiligingsbeleid voor een resourcegroep wilt. Als dat het geval is, moet u **Uniek** selecteren en de wijzigingen onder de optie **Preventiebeleid** aanbrengen.

![Beveiligingsbeleid per resourcegroep](./media/security-center-policies/security-center-policies-fig6-ga.png)

> [AZURE.NOTE] Als er een conflict is tussen het beleid op abonnementsniveau en het beleid op het niveau van de resourcegroep, heeft het beleid op het niveau van de resourcegroep voorrang.


## Zie ook

In dit document hebt u kunnen lezen hoe u het beveiligingsbeleid configureert in Azure Security Center. Zie de volgende onderwerpen voor meer informatie over Azure Security Center:

- [Plannings- en bedieningsgids voor Azure Security Center](security-center-planning-and-operations-guide.md). Leer de ontwerpoverwegingen kennen en leer deze in te plannen als u de overstap naar Azure Security Center wilt maken.
- [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md). Meer informatie over het controleren van de status van uw Azure-resources.
- [Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center](security-center-managing-and-responding-alerts.md). Informatie over het beheren van en reageren op beveiligingswaarschuwingen.
- [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md). Meer informatie over het bewaken van de status van uw partneroplossingen.
- [Veelgestelde vragen over Azure Security Center](security-center-faq.md). Raadpleeg de veelgestelde vragen over het gebruik van de service.
- [Azure-beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/). Raadpleeg de blogberichten over beveiliging en naleving in Azure.



<!--HONumber=Sep16_HO4-->



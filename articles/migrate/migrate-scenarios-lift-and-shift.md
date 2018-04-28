---
title: Voor lokale werkbelastingen migreren naar Azure met Azure DMS- en Site Recovery | Microsoft Docs
description: Informatie over het voorbereiden van Azure voor migratie van lokale-machines met de Azure-Service voor het migreren van Database en de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
ms.service: site-recovery
ms.topic: conceptual
ms.date: 04/21/2018
ms.author: raynew
ms.openlocfilehash: d6fc1af3c5a587ab62b86dcd4189165d96e6e3bc
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2018
---
# <a name="scenario-2-lift-and-shift-migration-to-azure"></a>Scenario 2: Lift en shift migratie naar Azure

Het bedrijf Contoso overweegt migratie naar Azure. Probeer af die ze willen beoordelen en een kleine lokale reizen app migreren naar Azure. Het is een app twee lagen reizen met een web-app uitgevoerd op één virtuele machine en een SQL Server-database op de tweede VM. De toepassing wordt geïmplementeerd in VMware en de omgeving wordt beheerd door een vCenter Server. 

In [Scenario 1: migratie naar Azure beoordelen](migrate-scenarios-assessment.md), ze de gegevens migratie-assistent (DMA) gebruikt voor het evalueren van de SQL Server-database voor de app. Bovendien ze de service Azure migreren gebruikt om de app VM's vast te stellen. Nu in dit scenario wordt willen na het voltooien van de beoordeling, is ze migreren van de database naar een beheerd van Azure SQL-exemplaar met de Azure-Database migratie Service (DMS) en lokale machines virtuele Azure-machines met de Azure Site Recovery-service.

Als u wilt uitproberen [Scenario 1](migrate-scenarios-assessment.md), en vervolgens dit scenario gebruik van deze app voorbeelden reizen, u kunt dit downloaden van [github](https://github.com/Microsoft/SmartHotel360).



**Service** | **Beschrijving** | **Kosten**
--- | --- | ---
[Database-Management-Service](https://docs.microsoft.com/azure/dms/dms-overview) | DMS kunt naadloze migraties uit meerdere databasebronnen naar Azure data-platforms, met minimale downtime. | De service is momenteel in de openbare preview (April 2018) en vrije kan worden gebruikt tijdens de preview kosteloos.<br/><br/> Houd er rekening mee dat voor de openbare preview DMS beperkt tot is een [aantal gebieden](https://docs.microsoft.com/azure/dms/dms-overview).
[Exemplaar van Azure SQL beheerd](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) | Beheerde exemplaar is een beheerde databaseservice die staat voor een volledig beheerde SQL Server-exemplaar in de Azure-cloud. Het dezelfde code deelt met de nieuwste versie van SQL Server Database Engine en heeft de nieuwste functies, verbeterde prestaties en beveiligingspatches. | Met Azure SQL Database beheerd instanties die wordt uitgevoerd in Azure worden kosten in rekening op basis van capaciteit. [Meer informatie](https://azure.microsoft.com/pricing/details/sql-database/managed/). 
[Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) | De service ingedeeld en beheert de migratie en herstel na noodgevallen voor Azure VM's en on-premises virtuele machines en fysieke servers.  | Tijdens de replicatie naar Azure, zijn Azure Storage kosten verbonden.  Virtuele machines in Azure worden gemaakt en kosten wanneer failover plaatsvindt. [Meer informatie](https://azure.microsoft.com/pricing/details/site-recovery/) over kosten en prijzen.

In dit scenario stellen we een site-naar-site VPN zodat DMS kan verbinding met de lokale-database maken, een Azure SQL beheerd-exemplaar in Azure maken en migreren van de database. Voor de Site Recovery we de on-premises VMware-omgeving voorbereiden, instellen van de replicatie, en de virtuele machines migreren naar Azure.  


> [!IMPORTANT]
> U moet zijn geregistreerd bij de SQL beheerd exemplaar beperkt openbare Preview. U moet een Azure-abonnement om [aanmelden](https://portal.azure.com#create/Microsoft.SQLManagedInstance). Registratie kan duren voordat een paar dagen om te voltooien, dus zorg ervoor dat u dit doen voordat u begint met het implementeren van dit scenario.

## <a name="architecture"></a>Architectuur

### <a name="site-recovery"></a>Site Recovery

![Site Recovery](media/migrate-scenarios-lift-and-shift/asr-architecture.png)  

### <a name="dms"></a>DMS
![DMS](media/migrate-scenarios-lift-and-shift/dms-architecture.png)  

In dit scenario geldt het volgende:

- Contoso is een fictious naam die de organisatie van een typische bedrijf vertegenwoordigt. Contoso wil om te beoordelen en migreren van de twee lagen lokale reizen app.
- Contoso heeft een on-premises datacentrum (contoso-datacenter), met een lokale domeincontroller (**contosodc1**).
- De interne reizen-app is gelaagd over twee virtuele machines, WEBVM en SQLVM en bevindt zich op VMware ESXi-host (**contosohost1.contoso.com**).
- De VMware-omgeving wordt beheerd door vCenter Server (**vcenter.contoso.com**) die wordt uitgevoerd op een virtuele machine.

## <a name="prerequisites"></a>Vereisten

Als u uitvoeren van dit scenario wilt, is wat u moet hebben.

Vereisten | Details
--- | ---
**Azure-abonnement** | Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan.<br/><br/> Als u een gratis account maakt, bent u de beheerder van uw abonnement en kunt u alle acties uitvoeren.<br/><br/> Als u een bestaand abonnement en u niet de beheerder bent, moet u samen met de beheerder u eigenaar of bijdrager machtigingen toewijzen.<br/><br/> Als u meer gedetailleerde machtigingen nodig hebt, raadpleegt u [in dit artikel](../site-recovery/site-recovery-role-based-linked-access-control.md). 
**Site recovery (op locatie)** | Een lokale vCenter-server versie 5.5, 6.0 of 6.5<br/><br/> Een ESXi-host waarop versie 5.5, 6.0 of 6.5<br/><br/> Een of meer virtuele VMware-machines uitgevoerd op de ESXi-host.<br/><br/> Virtuele machines moeten voldoen aan [Azure-vereisten](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#azure-vm-requirements).<br/><br/> Ondersteund [netwerk](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#network) en [opslag](https://docs.microsoft.com/azure/site-recovery/vmware-physical-azure-support-matrix#storage) configuratie.
**DMS** | DMS moet u een [compatibel on-premises VPN-apparaat](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-devices).<br/><br/> U moet mogelijk de on-premises VPN-apparaat configureren. Er moet een extern gericht openbaar IPv4-adres en het adres kan zich niet achter een NAT-apparaat.<br/><br/> Zorg ervoor dat u toegang hebt tot uw lokale SQL Server-database.<br/><br/> De Windows Firewall moet toegang kunnen krijgen tot de bron-database-engine. [Meer informatie](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-a-windows-firewall-for-database-engine-access).<br/><br/> Als er een firewall voor uw database-machine, kunt u regels voor toegang tot de database en bestanden via het SMB-poort 445 toevoegen.<br/><br/> De referenties waarmee verbinding maken met de bron van SQL Server en de doelinstantie worden beheerd, moeten lid zijn van de serverrol sysadmin.<br/><br/> U moet een netwerk delen in uw lokale database die DMS gebruiken kunt voor back-up van de brondatabase.<br/><br/> Zorg ervoor dat het serviceaccount met een SQL Server-exemplaar van bron schrijfrechten op de netwerkshare heeft.<br/><br/> Maak een notitie van een Windows-gebruiker (en wachtwoord) die de machtiging Volledig beheer op de netwerkshare heeft. De migratie van Azure databaseservice imiteert deze gebruikersreferenties voor het uploaden van back-upbestanden naar de Azure storage-container.<br/><br/> Het installatieproces van SQL Server Express het TCP/IP-protocol wordt ingesteld op **uitgeschakelde** standaard. Zorg ervoor dat deze ingeschakeld.


# <a name="scenario-overview"></a>Overzicht van scenario's

Hier ziet u wat we gaan doen:

> [!div class="checklist"]
> * **Stap 1: Een site-naar-site VPN-verbinding instellen**: DMS maakt verbinding met uw lokale-database via een site-naar-site VPN-verbinding. We maken een virtueel netwerk voor de VPN-verbinding en hoger, we dit netwerk gebruiken voor siteherstel. Het netwerk die u maakt moet zich bevinden in de regio waarin u een Recovery Services-kluis maken.
> * **Stap 2: Een SQL-exemplaar voor het beheren van Azure instellen**: U moet een vooraf gemaakte beheerde exemplaar dat de lokale SQL Server-database wilt migreren.
> * **Stap 3: Instellen van SA-URI voor DMS**: een shared access signature (SAS) Uniform Resource Identifier (URI) biedt gedelegeerde toegang tot bronnen in uw opslagaccount zodat u beperkte machtigingen voor opslagobjecten verlenen kunt. Een SAS-URI instellen zodat de DMS toegang heeft tot de storage-account-container waarnaar de service de SQL Server-back-up van bestanden geüpload.
> * **Stap 4: Voorbereiden DMS**: Registreer de provider van de Database wilt migreren, geen exemplaar maken en vervolgens een DMS-project maken.
> * **Stap 5: Voorbereiden op Azure Site Recovery**: maken van een Azure storage-account gerepliceerde gegevens kan bevatten.
> * **Stap 6: Lokale VMware voorbereiden voor de Site Recovery**: U accounts voorbereiden voor VM-detectie en agent-installatie en verbinding maken met virtuele Azure-machines na een failover voorbereiden. 
> * **Stap 7: Repliceren VMs**: configureert u de Site Recovery-bron en doel-omgeving, instellen van een beleid voor wachtwoordreplicatie en virtuele machines repliceren naar Azure-opslag.
> * **Stap 8: Migreren van de database met DMS**: Voer een databasemigratie uit.
> * **Stap 9: Migreer de virtuele machines met Site Recovery**: een failover voor de virtuele machines migreren naar Azure uitvoert.


## <a name="step-1-set-up-site-to-site-vpn-connection"></a>Stap 1: Site-naar-site VPN-verbinding instellen

Om voor te bereiden voor de site-naar-site VPN-verbinding, een virtueel netwerk en subnetten instellen, een VPN-gateway maken voor het virtuele netwerk en een lokale netwerkgateway instellen zodat deze Azure weet hoe u verbinding maken met uw lokale VPN. Vervolgens maken en controleer of de site-naar-site-verbinding.

### <a name="set-up-a-virtual-network"></a>Een virtueel netwerk instellen

Maak een Azure-netwerk om DMS met site-naar-site-verbinding met uw lokale SQL-Server opgeven.


1. In de [Azure-portal](https://portal.azure.com), klikt u op **maken van een resource**. Selecteer **Networking** > **virtueel netwerk**.
2. In **virtueel netwerk** > **een implementatiemodel selecteren**, selecteer **Resource Manager** > **maken**
3. In **virtueel netwerk maken** > **naam**, voer een unieke naam. In ons scenario **ContosoVNET**.
4. In **adresruimte**, het IP-adresbereik toevoegen. Het bereik overlappen niet met uw on-premises-adresruimte.
5. In **resourcegroep**, een nieuwe groep maken of een bestaande set selecteren. In dit scenario we maken gebruik van **ContosoRG**.
6. In **locatie**, selecteer de regio waarin het virtuele netwerk maken. We maken gebruik van **VS-Oost 2**.
7. In **Subnet**, het toevoegen van de subnetnaam van de eerste en -adresbereik. Er is gemaakt de **Apps** subnet.
8. Service-eindpunten uitschakelen.

    ![Een virtueel netwerk maken](media/migrate-scenarios-lift-and-shift/vnet-create-network.png)

9. Selecteer **vastmaken aan dashboard** zodat u gemakkelijk in de toekomst uw netwerk vinden en klik vervolgens op **maken**.
10. Het duurt een paar seconden voordat het virtuele netwerk is gemaakt. Nadat deze gemaakt, kunt u deze in de Azure-portaldashboard verifiëren.
11. Zorg ervoor dat deze communicatiepoorten in het virtuele netwerk zijn toegestaan: 443, 53, 9354, 445, 12000.


### <a name="set-up-subnets"></a>Subnetten instellen

Vier subnetten hebben we in ons Azure-netwerk:

![Subnetlijst met](media/migrate-scenarios-lift-and-shift/vnet-subnet-list.png)

Instellen van het eerste subnet (Apps) tijdens het maken van het netwerk. Maak nu de rest van de subnetten. Het gatewaysubnet wordt gebruikt door de VPN-gatewayverbinding voor Azure om verkeer te verzenden via een VPN-verbinding met uw on-premises site.

1. In het virtuele netwerk onder **instellingen**, klikt u op **Subnet** > **+ Subnet**.
2. Instellen van een subnet **Datamigration**dit adresbereik en klik vervolgens op **OK**.

    ![Gegevens subnet](media/migrate-scenarios-lift-and-shift/vnet-subnet-data.png)  


3. In **+ Subnet**, instellen van een subnet **identiteit**dit adresbereik en klik vervolgens op **OK**.
    ![Identiteit subnet](media/migrate-scenarios-lift-and-shift/vnet-subnet-identity.png)

4. In **Subnet**, klikt u op **gatewaysubnet**, en klik vervolgens op **OK**.
    - Dit subnet bevat de IP-adressen die door uw VPN-gateway wordt gebruikt voor VPN-verbindingen.
    - De naam van dit subnet wordt automatisch ingesteld zodat die Azure het kunt herkennen.
    - Pas het adresbereik automatisch ingevuld, zodat deze overeenkomen met uw lokale subnet. 

    ![Gatewaysubnet](media/migrate-scenarios-lift-and-shift/vnet-subnet-gateway.png)
    
### <a name="set-up-a-virtual-network-gateway"></a>Instellen van een virtuele netwerkgateway

1. Klik links op de portalpagina op **+** en typ 'Virtuele netwerkgateway' in het zoekvak. In **resultaten**, klikt u op **virtuele netwerkgateway**.
2. Klik onder aan de pagina Virtuele netwerkgateway op **Maken**.
3. In **virtuele netwerkgateway aanmaken** >  **naam**, Geef een naam voor het gateway-object.
4. In **Gatewaytype**, selecteer **VPN**.
5. In **VPN-type**, selecteer **op Route gebaseerde**.
6. In **SKU**, selecteer de gateway-SKU in de vervolgkeuzelijst. Welke SKU's worden weergegeven in de vervolgkeuzelijst, is afhankelijk van het VPN-type dat u selecteert. Geen actieve-actieve modus niet inschakelen. [Meer informatie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings#gwsku) over SKU's.
7. In **virtueel netwerk** > **Kies een virtueel netwerk**, toevoegen van het netwerk die u hebt gemaakt. De gateway wordt toegevoegd aan dit netwerk.
8. In **eerste IP-configuratie**, een openbare IP-adres opgeven.  Een VPN-gateway moet een openbaar IP-adres hebben. Dit adres wordt dynamisch toegewezen aan de bron wanneer de VPN-gateway is gemaakt. Alleen dynamische adressen worden momenteel ondersteund, maar het IP-adres niet wijzigen nadat deze toegewezen.
    - Klik op **configuratie van IP-gateway maken**. In **openbare IP-adres kiezen**, klikt u op **+ nieuw**.
    - In **openbare IP-adres maken**, Geef een naam voor uw openbare IP-adres (Contoso-Gateway). Laat de SKU als **Basic**, en klik op **OK** de wijzigingen wilt opslaan.
        
    ![Gateway van een virtueel netwerk](media/migrate-scenarios-lift-and-shift/vnet-create-vpn-gateway2.png) 

9. Klik op **Maken** om de VPN-gateway aan te maken. De instellingen worden gevalideerd en 'Virtuele netwerkgateway implementeren' wordt weergegeven op het dashboard.

    ![Virtuele netwerkgateway valideren](media/migrate-scenarios-lift-and-shift/vnet-vpn-gateway-validate.png)
    
Het aanmaken van een gateway kan tot 45 minuten duren. Mogelijk moet u uw portal-pagina om te zien van de voltooide status te vernieuwen.

### <a name="set-up-a-local-network-gateway"></a>Instellen van een lokale netwerkgateway

Een lokale netwerkgateway in Azure kunt instellen voor uw on-premises site.

1. Klik in de portal op **+Een resource maken**.
2. In het zoekvak typt u **Lokale netwerkgateway**. Druk vervolgens op **Enter** om te zoeken. Klik in de resultaten op **lokale netwerkgateway** > **maken**.
3. In **de lokale netwerkgateway maken** > **naam**, Geef een naam die gebruikmaakt van Azure voor uw lokale netwerkgateway-object.
4. **IP-adres**, geef het openbare IP-adres van de on-premises VPN-apparaat waarmee Azure verbinding maakt. Het IP-adres mag niet zijn NAT'ed en Azure verbinding moet worden.
5. In **adresruimte**, typt u het lokale netwerk-adresbereiken die worden doorgestuurd via de VPN-gateway naar de on-premises-apparaat. Zorg ervoor dat deze niet overlappen met adresbereiken op de virtuele Azure-netwerk.
6. **BGP-instellingen configureren** niet relevant zijn voor dit scenario.
7. In **abonnement**, controleert u of het abonnement klopt.
8. In **resourcegroep**: Selecteer de resourcegroep die u gebruikt voor het maken van het netwerk (ContosoRG).
9. In **locatie**, selecteer de regio waarin u het virtuele netwerk hebt gemaakt.

    ![Lokale gateway](media/migrate-scenarios-lift-and-shift/vnet-create-local-gateway.png)

4. Klik op **maken** de lokale gateway te maken.

### <a name="configure-your-on-premises-vpn-device"></a>Uw on-premises VPN-apparaat configureren

Dit is wat u moet configureren op uw on-premises VPN-apparaat:

- Het openbare IPv4-adres van de virtuele Azure-netwerk-gateway die u zojuist hebt gemaakt.
- De vooraf gedeelde sleutel (de sleutel die u hebt gebruikt bij het maken van de site-naar-site VPN-verbinding).  

Instellen van uw on-premises VPN-apparaat om u te helpen:

- Afhankelijk van de on-premises VPN-apparaat die u hebt, kunt u mogelijk een configuratie van VPN-apparaatscript downloaden. [Meer informatie](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-download-vpndevicescript).
- Bekijk [meer handige resources](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal#VPNDevice).

### <a name="create-the-vpn-connection"></a>De VPN-verbinding maken

Maak nu de site-naar-site VPN-verbinding tussen de virtuele netwerkgateway en uw on-premises VPN-apparaat.

1. Open de pagina virtuele netwerk in **virtuele netwerken** > **overzicht** > **verbonden apparaten**. 
2. Klik op **verbindingen** > **+ toevoegen**.
3. In **verbinding toevoegen** > **naam**, Geef een naam voor de verbinding.
4. In **verbindingstype**, selecteer **Site-naar-site (IPSec)**.
5. De **virtuele netwerkgateway** geldt een vaste waarde omdat u verbinding vanaf deze gateway maakt.
6. In **lokale netwerkgateway**, geef de lokale netwerkgateway die u hebt gemaakt.
7. In **gedeelde sleutel**, de sleutel die overeenkomt met de waarde die u voor uw lokale on-premises VPN-apparaat opgeven. In het voorbeeld wordt 'abc123' gebruikt, maar u kunt een ingewikkeldere waarde gebruiken (aanbevolen). Het is van belang dat de waarde die u hier opgeeft, dezelfde waarde is die u hebt opgegeven bij het configureren van het VPN-apparaat.
8. De waarden voor **abonnement**, **resourcegroep**, en **locatie** worden opgelost.

    ![VPN-verbinding](media/migrate-scenarios-lift-and-shift/vpn-connection.png) 

4. Klik op **OK** om de verbinding te maken. U ziet 'Verbinding maken' flash op het scherm.
5. Nadat de verbinding is gemaakt, weergeven op de **verbindingen** pagina van de virtuele netwerkgateway. De status van onbekende gaan > verbinden > is voltooid.

### <a name="verify-the-vpn-connection"></a>De VPN-verbinding controleren

U kunt de verbindingsstatus voor een Resource Manager VPN Gateway weergeven door in Azure Portal naar de verbinding te navigeren. 

1. Klik op **alle resources**, en navigeer naar uw virtuele netwerkgateway.
2. Klik op de pagina van de gateway virtuele netwerk op **verbindingen**. U ziet de status van elke verbinding.
3. Klik op de naam van de verbinding en meer informatie bekijken in **Essentials**. De status is 'Geslaagd' en 'Verbonden' wanneer u een verbinding hebt gemaakt.

Nu controleren of u verbinding met de SQL Server-virtuele machine via VPN maken kunt. Gebruik een extern bureaublad-verbinding en verbinding maken met de VM IP-adres.



## <a name="step-2-prepare-an-azure-sql-managed-instance"></a>Stap 2: Een beheerde Azure SQL-exemplaar voorbereiden

### <a name="set-up-a-virtual-network"></a>Een virtueel netwerk instellen

Voor dit scenario moet u een ander virtueel netwerk dat toegewezen aan de Azure SQL beheerd-exemplaar te maken.  Houd rekening met de volgende vereisten:

- U moet een toegewezen subnet voor het maken van het beheerde-exemplaar.
- Het subnet moet niet leeg zijn en mag niet een andere cloudservice mag niet, en het een gatewaysubnet. Nadat het beheerde-exemplaar is gemaakt, kunt u resources mag niet toevoegen aan het subnet.
- Het subnet mag niet beschikken over een NSG is gekoppeld.
- Het subnet moet een gebruiker Route-tabel (UDR) met de volgende Hop Internet 0.0.0.0/0 hebben als de enige route toegewezen. 
- Optionele aangepaste DNS: als aangepaste DNS is opgegeven in het VNet, Azure recursieve resolvers IP-adres (zoals 168.63.129.16) moet worden toegevoegd aan de lijst. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-custom-dns).
- Het subnet mag niet beschikken over een service-eindpunt gekoppeld (opslag of SQL). Service-eindpunten moeten worden uitgeschakeld op het virtuele netwerk.
- Het subnet moet minimaal 16 IP-adressen hebben. [Meer informatie](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-vnet-configuration#determine-the-size-of-subnet-for-managed-instances) over het formaat van het subnet beheerde exemplaar.

Stel het virtuele netwerk als volgt in:

1.  Klik in Azure Portal op **Een resource maken**. 
2. Selecteer **Networking** > **virtueel netwerk**.
3. In **virtueel netwerk** > **een implementatiemodel selecteren**, selecteer **Resource Manager** > **maken**.
4. In **virtueel netwerk maken** > **naam**, voer een unieke naam. In ons scenario **ContosoVNETSQLMI**.
5. In **adresruimte**, het IP-adresbereik dat hieronder is weergegeven. Het bereik overlappen niet met uw on-premises en ContosoVNET-adresruimte.
6. In **resourcegroep**, een nieuwe groep maken of een bestaande set selecteren. In dit scenario we maken gebruik van **ContosoRG**.
7. In **locatie**, selecteer de regio waarin het virtuele netwerk maken. We maken gebruik van **VS-Oost 2**.
8. In **Subnet**, het toevoegen van de subnetnaam van de eerste en -adresbereik. We hebben gemaakt de **ManagedInstances** subnet.
9. Service-eindpunten uitschakelen.

    ![Beheerde exemplaar netwerk](media/migrate-scenarios-lift-and-shift/network-mi.png)

10. Nadat het netwerk is geïmplementeerd, moet u de DNS-instellingen wijzigen. In dit scenario worden de DNS-server eerst verwijst naar een domeincontroller in het Identity-subnet met behulp van een statisch privé IP-adres (172.16.3.4) en de Azure DNS-resolver 168.63.129.16.

    ![Beheerde exemplaar netwerk](media/migrate-scenarios-lift-and-shift/network-mi2.png)


### <a name="set-up-routing"></a>Routering instellen

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**. Klik op **routetabel** > **maken voor de routetabel** pagina.
2. In **routetabel maken** > **naam**, Geef een naam voor de tabel.
3. Selecteer uw abonnement, resourcegroep en locatie. Laat BGP uitgeschakeld en klik op **maken**.
    ![Routetabel](media/migrate-scenarios-lift-and-shift/route-table.png)

4. Nadat de routetabel is gemaakt, opent u het en klik op **Routes** > **+ toevoegen**.
5. In **Add route** > **naam**, Geef een routenaam.
6. In **adresvoorvoegsel**, 0.0.0.0/0 opgeven.
7. In **volgende hop type**, selecteer **Internet**. Klik vervolgens op **OK**.

     ![Routetabel](media/migrate-scenarios-lift-and-shift/route-table2.png)


### <a name="apply-the-route-to-the-managed-instance-subnet"></a>De route van toepassing op het subnet beheerde exemplaar

1. Open het virtuele netwerk dat u hebt gemaakt. Klik op **subnetten** > uw subnet-naam.
2. Klik op **routetabel** > uw tabel-naam. Klik vervolgens op **Opslaan**.

     ![Routetabel](media/migrate-scenarios-lift-and-shift/route-table3.png)

### <a name="create-a-managed-instance"></a>Maak een beheerde exemplaar

1. Klik op **maken van een resource**, zoek **beheerd exemplaar**, en selecteer **Azure SQL Database beheerd-instantie (preview)**.
2. Klik op **Create**.
3. In **SQL-exemplaar beheerd**, selecteer uw abonnement en controleer of **Preview-voorwaarden** weergeven **geaccepteerde**.
4. In **beheerd exemplaarnaam**, Geef een naam op. 
5. Geef een gebruikersnaam en wachtwoord op als beheerder voor het exemplaar.
6. Selecteer de resourcegroep, locatie en het virtuele netwerk voor het exemplaar.
7. Klik op **prijscategorie** grootte rekenkracht en opslag. Standaard wordt het exemplaar 32 GB aan opslagruimte kosteloos (April 2018).
8. Geef de opslag, en het aantal virtuele kernen.
9. Klik op **toepassen** de instellingen op te slaan en **maken** voor het implementeren van het beheerde-exemplaar. Als u wilt weergeven van de implementatiestatus **meldingen**, en **implementatie wordt uitgevoerd**.

    ![Beheerd exemplaar](media/migrate-scenarios-lift-and-shift/mi-1.png)

Na de beheerde implementatie is voltooid, twee nieuwe resources worden weergegeven in de resourcegroep ContosoRG: virtueel Cluster voor de beheerde exemplaren en de SQL-exemplaar worden beheerd. Beide zijn op de locatie VS-Oost 2.

![Beheerd exemplaar](media/migrate-scenarios-lift-and-shift/managed-instance2.png)


## <a name="step-3-get-a-sas-uri-for-dms"></a>Stap 3: Een SAS-URI voor DMS ophalen

Een SAS URI ophalen zodat DMS toegang heeft tot uw storage-account-container, om de back-up van bestanden die worden gebruikt voor het migreren van databases naar Azure SQL Database-beheerd instantie te uploaden. 

1. Open Opslagverkenner (Preview).
2. Vouw in het linkerdeelvenster het storage-account met de blob-container die u wilt een SAS ophalen. Vouw het opslagaccount **Blob-Containers**.
3. Met de rechtermuisknop op de container en selecteer **Shared Access Signature ophalen**.

     ![SAS](media/migrate-scenarios-lift-and-shift/sas-1.png)

4. In **Shared Access Signature**, geeft u het beleid, de begin- en verloopdatum datums, de tijdzone en toegangsniveaus voor de resource. Klik vervolgens op **Maken**.

    ![SAS](media/migrate-scenarios-lift-and-shift/sas-2.png)

5. Een tweede dialoogvenster de blob-container, samen met de URL en u gebruiken kunt voor toegang tot de opslagbronnen QueryStrings worden weergegeven. Selecteer **kopie** om de waarden te kopiëren. Op een veilige plaats bewaren. U moet ze bij het instellen van DMS.

      ![SAS](media/migrate-scenarios-lift-and-shift/sas-3.png)  

## <a name="step-4-prepare-dms"></a>Stap 4: DMS voorbereiden

U Registreer de provider van de migratie van de Database en maak een exemplaar.

### <a name="register-the-microsoftdatamigration-resource-provider"></a>De registerbronprovider is Microsoft.DataMigration

1. Selecteer in uw abonnement, **Resourceproviders**. 
2. Zoek naar 'migreren' en selecteer vervolgens aan de rechterkant van Microsoft.DataMigration **registreren**. 


### <a name="create-an-instance"></a>Geen exemplaar maken

1. Selecteer **+ maken van een resource**, zoeken naar Azure Database migratieservice en selecteer vervolgens **Azure migratie databaseservice** uit de vervolgkeuzelijst.
2. Selecteer op het scherm Migratieservice voor Azure-Database (preview) **maken**.
3. Geef een naam voor de service, het abonnement, resourcegroep, virtueel netwerk en de prijscategorie.
4. Selecteer **maken** de service te maken.

    ![DMS](media/migrate-scenarios-lift-and-shift/dms-instance.png)  




## <a name="step-5-prepare-azure-for-the-site-recovery-service"></a>Stap 5: Voorbereiden op Azure Site Recovery-service

### <a name="set-up-a-virtual-network"></a>Een virtueel netwerk instellen

U moet een Azure-netwerk waarin de Azure VM's moeten worden geplaatst nadat ze zijn gemaakt na een failover en Azure storage-account waarin gerepliceerde gegevens worden opgeslagen.

- We gebruiken de Azure-netwerk dat we eerder hebt gemaakt, voor DMS voor de doeleinden van dit scenario.
- Houd er rekening mee dat het netwerk die u moet zich in dezelfde regio bevinden als de Site Recovery-kluis.


### <a name="create-an-azure-storage-account"></a>Een Azure-opslagaccount maken

VM-gegevens van de site Recovery worden gerepliceerd naar Azure storage. Azure VM's worden gemaakt vanuit de opslag wanneer u een failover van on-premises naar Azure uitvoert.

1. Selecteer in het menu van [Azure Portal](https://portal.azure.com) **Nieuw** > **Opslag** > **Opslagaccount**.
2. Voer in **Opslagaccount maken** een naam voor het account in. Voor deze zelfstudies gebruiken we de naam **contosovmsacct1910171607**. De naam moet uniek zijn in Azure, tussen de 3 en 24 tekens lang zijn en mag alleen cijfers en kleine letters bevatten.
3. Selecteer bij **implementatiemodel** **Resource Manager**.
4. Selecteer bij **Soort account** **Algemeen gebruik**. Selecteer bij **Prestaties** **Standard**. Selecteer niet blob-opslag.
5. Selecteer bij **Replicatie** de standaardoptie **Geografisch redundante opslag met leestoegang** voor opslagredundantie.
6. Selecteer bij **Abonnement** het abonnement waarin u het nieuwe opslagaccount wilt maken.
7. Geef bij **Resourcegroep** een nieuwe resourcegroep op. Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Gebruik voor deze zelfstudies de naam **ContosoRG**.
8. Selecteer bij **Locatie** de geografische locatie voor het opslagaccount. Het opslagaccount moet zich in dezelfde regio bevinden als de Recovery Services-kluis. In dit scenario we maken gebruik van de **VS-Oost 2** regio.

   ![Create a storage account](media/migrate-scenarios-lift-and-shift/create-storageacct.png)

9. Selecteer **Maken** om het opslagaccount te maken.

### <a name="create-a-recovery-services-vault"></a>Een Recovery Services-kluis maken

1. Selecteer in Azure Portal **Een resource maken** > **Controle en beheer** > **Backup en Site Recovery**.
2. Voer in **Naam** een beschrijvende naam in om de kluis aan te duiden. Gebruik voor deze zelfstudie **ContosoVMVault**.
3. Selecteer bij **Resourcegroep** de bestaande resourcegroep met de naam **contosoRG**.
4. In **locatie**, voer de Azure-regio **VS-Oost 2**.
5. Voor snelle toegang tot de kluis vanuit het dashboard, selecteert u **Aan dashboard vastmaken** > **Maken**.
De nieuwe kluis wordt weergegeven in **Dashboard** > **Alle resources** en op de hoofdpagina **Recovery Services-kluizen**.

## <a name="step-6-prepare-on-premises-vmware-for-site-recovery"></a>Stap 6: On-premises VMware voor herstel van sites voorbereiden

VMware om voor te bereiden van Site Recovery, hier wat u moet doen:

- Een account voorbereiden op de vCenter-server of vSphere ESXi-host om VM-detectie te automatiseren
- Een account voorbereiden voor de automatische installatie van de Mobility-service op VMware VM’s
- Voorbereiden op de lokale virtuele machines als u wilt verbinding maken met virtuele Azure-machines na een failover.


### <a name="prepare-an-account-for-automatic-discovery"></a>Een account voorbereiden voor automatische detectie

Site Recovery moet toegang hebben tot de VMware-servers om het volgende te kunnen doen:

- Virtuele machines automatisch detecteren. Er is minimaal een alleen-lezen-account vereist.
- Replicatie, failover en failback orkestreren. U moet een account die bewerkingen zoals het maken en schijven verwijderen en het inschakelen van virtuele machines kan worden uitgevoerd.

Ga als volgt te werk om het account te maken:

1. Als u een vast account wilt gebruiken, maakt u een rol op vCenter-niveau. Geef de rol een naam zoals **Azure_Site_Recovery**.
2. Wijs de rol de rechten toe die worden samengevat in onderstaande tabel.
3. Maak een gebruiker op de vCenter-server of vSphere-host. Wijs de rol toe aan de gebruiker.

**Taak** | **Rol/machtigingen** | **Details**
--- | --- | ---
**VM-detectie** | Ten minste een alleen-lezen-gebruiker<br/><br/> Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).
**Volledige replicatie, failover en failback** |  Maak een rol (Azure_Site_Recovery) met de vereiste machtigingen en wijs de rol toe aan een VMware-gebruiker of -groep<br/><br/> Datacentrumobject –> Doorgeven naar onderliggend object, rol=Azure_Site_Recovery<br/><br/> Gegevensopslag –> Ruimte toewijzen, browsen in de gegevensopslag, bestandsbewerkingen op laag niveau, bestand verwijderen, bestanden van virtuele machines bijwerken<br/><br/> Netwerk –> Netwerk toewijzen<br/><br/> Resource –> VM toewijzen aan resourcegroep, uitgeschakelde VM migreren, ingeschakelde VM migreren<br/><br/> Taken –> Taak maken, taak bijwerken<br/><br/> Virtuele machine –> Configuratie<br/><br/> Virtuele machine –> Interactie –> vraag beantwoorden, apparaatverbinding, CD's configureren, diskettes configureren, uitschakelen, inschakelen, VMware-hulpprogramma's installeren<br/><br/> Virtuele machine -> Inventaris –> Maken, registreren, registratie opheffen<br/><br/> Virtuele machine –> Inrichten –> Downloaden van virtuele machine toestaan, toestaan dat bestanden van virtuele machines worden geüpload<br/><br/> Virtuele machine –> Momentopnamen -> Momentopnamen verwijderen | Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.<br/><br/> Wijs om de toegang te beperken de rol **Geen toegang** met het object **Doorgeven naar onderliggend object** toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM’s en netwerken).

### <a name="prepare-an-account-for-mobility-service-installation"></a>Een account voorbereiden voor installatie van de Mobility-service

De Mobility-service moet worden geïnstalleerd op de VM die u wilt repliceren.

- Site Recovery kan een automatische push-installatie van dit onderdeel kunt doen wanneer u replicatie voor de virtuele machine inschakelt.
- Voor automatische push-installatie moet u een account die door Site Recovery wordt gebruikt voor toegang tot de virtuele machine voorbereiden.
- U geeft dit account bij het instellen van de replicatie in de Azure-console.
- U moet een domein of lokale account met machtigingen op de virtuele machine te installeren.


### <a name="prepare-to-connect-to-azure-vms-after-failover"></a>Voorbereiden op het verbinden met virtuele Azure-machines na een failover

Na een failover naar Azure is het raadzaam vanaf uw on-premises netwerk verbinding maken met uw gerepliceerde virtuele machines in Azure.

Als u na een failover verbinding wilt maken met Windows-VM’s met behulp van RDP, doet u het volgende:

1. Om toegang te krijgen via het internet, schakelt u voor de failover RDP in op de on-premises VM. Zorg ervoor dat TCP en UDP-regels worden toegevoegd voor de **openbare** profiel, en dat RDP is toegestaan in **Windows Firewall** > **Apps toegestaan** voor alle profielen.
2. Voor toegang via VPN tussen sites schakelt u RDP in op de on-premises computer. RDP moet toegestaan zijn in de **Windows Firewall** -> **Toegestane apps en onderdelen** voor **Domein en Privé**-netwerken.
3. Controleer of het SAN-beleid van het besturingssysteem is ingesteld op **OnlineAll**. [Meer informatie](https://support.microsoft.com/kb/3031135).
4. Er mogen geen Windows-updates in behandeling zijn op de VM wanneer u een failover activeert. Als die er zijn, kunt u pas weer bij de virtuele machine inloggen als de update is voltooid.
5. Controleer na een failover **Diagnostische gegevens over opstarten** op de Windows Azure VM om een schermopname van de VM weer te geven. Als u geen verbinding kunt maken, controleer dan of de VM actief is en bekijk deze [tips voor het oplossen van problemen](http://social.technet.microsoft.com/wiki/contents/articles/31666.troubleshooting-remote-desktop-connection-after-failover-using-asr.aspx).



## <a name="step-7-replicate-vms-with-site-recovery"></a>Stap 7: Virtuele machines met Site Recovery repliceren

### <a name="select-a-replication-goal"></a>Een replicatiedoel selecteren

1. Selecteer in **Recovery Services-kluizen** de kluisnaam, **ContosoVMVault**.
2. Selecteer in **Aan de slag** Site Recovery. Selecteer vervolgens **Infrastructuur voorbereiden**.
3. In **Beveiligingsdoel** > **Waar bevinden de machines zich**, selecteert u **On-premises**.
4. In **Waarnaartoe wilt u de machines repliceren** selecteert u **Naar Azure**.
5. In **Zijn de machines gevirtualiseerd** selecteert **Ja, met VMware vSphere Hypervisor**. Selecteer vervolgens **OK**.

    ![OVF-sjabloon](./media/migrate-scenarios-lift-and-shift/replication-goal.png)


### <a name="confirm-deployment-planning"></a>Controleer de implementatie plannen

In **-implementatie plannen**, klikt u op **Ja, ik dit hebt gedaan**, in de vervolgkeuzelijst.

### <a name="set-up-the-source-environment"></a>De bronomgeving instellen


Voor het instellen van de bronomgeving hebt u één maximaal beschikbare on-premises computer nodig om de on-premises Site Recovery-onderdelen te hosten. Onderdelen zijn de configuratieserver en de processerver.

- De configuratieserver coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.
- De processerver fungeert als replicatiegateway. Deze ontvangt replicatiegegevens, optimaliseert de gegevens met caching, compressie en versleuteling, en verzendt ze naar de Azure-opslag.
- De processerver installeert ook Mobility Service op VM's die u wilt repliceren, en detecteert automatisch on-premises VMware-VM's.


De configuratieserver instellen als een maximaal beschikbare VMware-VM:

- Een voorbereid Open Virtualization Format OVF ()-sjabloon downloaden.
- Importeer de sjabloon in VMware voor de virtuele machine maken.
- Instellen van de configuratieserver en deze te registreren in de kluis. 

Na de registratie detecteert de Site Recovery on-premises VMware-VM’s.



#### <a name="download-the-vm-template"></a>De VM-sjabloon downloaden

1. Ga in de kluis naar **Infrastructuur voorbereiden** > **Bron**.
2. Selecteer **+Configuratieserver** in **Bron voorbereiden**.

    ![Een sjabloon downloaden](./media/migrate-scenarios-lift-and-shift/new-cs.png)

4. Controleer in **Server toevoegen** of **Configuratieserver voor VMware** wordt weergegeven in **Servertype**.
2. Download de OVF-sjabloon voor de configuratieserver.

    ![OVF downloaden](./media/migrate-scenarios-lift-and-shift/add-cs.png)

  > [!TIP]
  U kunt de nieuwste versie van de configuratieserversjabloon rechtstreeks downloaden vanuit het [Microsoft Download Center](https://aka.ms/asrconfigurationserver).

#### <a name="import-the-template-in-vmware"></a>De sjabloon in VMware importeren

1. Aanmelden bij de VMware vCenter-server met behulp van de VMWare vSphere Client.
2. Selecteer in het menu **Bestand** de optie **OVF-sjabloon implementeren** om de wizard voor het implementeren van OVF-sjablonen te starten. 

     ![OVF-sjabloon](./media/migrate-scenarios-lift-and-shift/vcenter-wizard.png)

3. Voer in **Bron selecteren** de locatie van de gedownloade OVF in.
4. Selecteer **Volgende** in **Beoordelingsdetails**.
5. Accepteer de standaardinstellingen in **Naam en map selecteren** en **Configuratie selecteren**.
6. Selecteer in **Opslag selecteren****Thick Provision Eager Zeroed** in **Indeling virtuele schijf selecteren**.
7. Accepteer de standaardinstellingen in de rest van de wizardpagina's.
8. In **Klaar om te voltooien** doet u het volgende:

    * Selecteer **Inschakelen na de implementatie** > **Voltooien** om de VM in te stellen met de standaardinstellingen.

    * Als u een extra netwerkinterface wilt toevoegen, wist u **Inschakelen na de implementatie**. Selecteer vervolgens **Voltooien**. Standaard wordt de configuratieserversjabloon met één NIC geïmplementeerd. Na de implementatie kunt u meer NIC’s toevoegen.



#### <a name="register-the-configuration-server"></a>De configuratieserver registreren 

1. Schakel de VM in vanuit de VMWare vSphere Client-console.
2. De VM wordt opgestart in een Windows Server 2016-installatie-ervaring. Accepteer de gebruiksrechtovereenkomst en voer een Administrator-wachtwoord in.
3. Meld u nadat de installatie is voltooid bij de virtuele machine aan als de administrator.
4. De eerste keer dat u zich aanmeldt, wordt het configuratieprogramma van Azure Site Recovery gestart.
5. Voer een naam in die wordt gebruikt voor het registreren van de configuratieserver bij Site Recovery. Selecteer vervolgens **Volgende**.

    ![OVF-sjabloon](./media/migrate-scenarios-lift-and-shift/config-server-register1.png)

6. Het hulpprogramma controleert of de VM verbinding kan maken met Azure. Nadat de verbinding tot stand is gebracht, selecteert u **Aanmelden** om u aan te melden bij uw Azure-abonnement. De referenties moeten toegang hebben tot de kluis waarin u de configuratieserver wilt registreren.

    ![OVF-sjabloon](./media/migrate-scenarios-lift-and-shift/config-server-register2.png)

7. Het hulpprogramma voert enkele configuratietaken uit en start opnieuw op.
8. Meld u opnieuw aan bij de machine. De wizard voor het beheer van de configuratieserver wordt automatisch gestart.

#### <a name="configure-settings-and-add-the-vmware-server"></a>Instellingen configureren en de VMware-server toevoegen

1. Selecteer in de wizard voor het beheer van de configuratieserver **Connectiviteit instellen** en selecteer vervolgens de NIC die replicatieverkeer moet ontvangen. Selecteer vervolgens **Opslaan**. U kunt deze instelling niet wijzigen nadat deze is geconfigureerd.
2. Selecteer in **Selecteer Recovery Services-kluis** uw Azure-abonnement en de relevante resourcegroep en kluis.

    ![Kluis](./media/migrate-scenarios-lift-and-shift/cswiz1.png) 

3. Accepteer de gebruiksrechtovereenkomst in **Software van derden installeren**. Selecteer **Downloaden en installeren** om MySQL Server te installeren.
4. Selecteer **VMware PowerCLI installeren**. Zorg ervoor dat alle browservensters zijn gesloten voordat u dit doet. Selecteer vervolgens **Doorgaan**.
5. In **De configuratie van het apparaat valideren** worden de vereisten gecontroleerd voordat u doorgaat.
6. In **vCenter Server vSphere/ESXi-server configureren** voert u de FQDN of het IP-adres van de vCenter-server, of vSphere-host, in waar de VM's die u wilt repliceren zich bevinden. Voer de poort in waarop de server luistert. Voer een beschrijvende naam in voor de VMware-server in de kluis.
7. Voer referenties in die door de configuratieserver moeten worden gebruikt voor verbinding met de VMware-server. Site Recovery gebruikt deze referenties voor het automatisch detecteren van VMware-VM’s die beschikbaar zijn voor replicatie. Selecteer **Toevoegen** en vervolgens **Doorgaan**.

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

8. Voer in **Referenties voor virtuele machine configureren** de gebruikersnaam en het wachtwoord in die moeten worden gebruikt om Mobility Service automatisch op computers te installeren wanneer replicatie wordt ingeschakeld. Voor Windows-machines moet het account lokale administrator-machtigingen hebben op de machines die u wilt repliceren. 

    ![vCenter](./media/migrate-scenarios-lift-and-shift/cswiz2.png)

7. Selecteer **Configuratie voltooien** om de registratie te voltooien. 
8. Controleer nadat de registratie is voltooid in Azure Portal of de configuratieserver en de VMware-server worden weergegeven op de pagina **Bron** in de kluis. Selecteer vervolgens **OK** om de doelinstellingen te configureren.
9. Site Recovery maakt verbinding met de VMware-servers met behulp van de opgegeven instellingen en de VM's worden gedetecteerd.

> [!NOTE]
> Het duurt 15 minuten of langer voordat de accountnaam wordt weergegeven in de portal. Selecteer **Configuratieservers** > ***servernaam*** > **Server vernieuwen** om direct bij te werken.

### <a name="set-up-the-target-environment"></a>De doelomgeving instellen

Selecteer en controleer doelbronnen.

1. In **infrastructuur voorbereiden** > **doel**, selecteer de Azure-abonnement u wilt gebruiken.
2. Selecteer voor het doelmodel implementatie **Azure Resource Manager**.

3. Site Recovery controleert of u een of meer compatibele Azure-opslagaccounts en -netwerken hebt.


### <a name="create-a-replication-policy"></a>Een replicatiebeleid maken

1. In **infrastructuur voorbereiden** > **replicatie-instellingen** > **replicatiebeleid**, klikt u op **maken en koppelen** .
1. Voer in **Replicatiebeleid maken** de beleidsnaam **VMwareRepPolicy** in.
2. Gebruik in **RPO-drempelwaarde** de standaardwaarde van 60 minuten. Deze waarde bepaalt hoe vaak herstelpunten worden gemaakt. Wanneer de continue replicatie deze limiet overschrijdt, wordt er een waarschuwing gegenereerd.
3. Gebruik in **Bewaarperiode van het herstelpunt** de standaardwaarde van 24 uur als de bewaarperiode voor elk herstelpunt. Gebruik voor deze zelfstudie 72 uur. Gerepliceerde VM’s kunnen worden hersteld naar een willekeurig punt in een tijdvenster.
4. Gebruik in **Frequentie van de app-consistente momentopname** de standaardwaarde van 60 minuten voor de frequentie waarmee app-consistente momentopnamen worden gemaakt. Selecteer **OK** om het beleid te maken.

    ![Replicatiebeleid maken](./media/migrate-scenarios-lift-and-shift/replication-policy.png)

5. Het beleid wordt automatisch gekoppeld aan de configuratieserver. 

    ![Beleid voor wachtwoordreplicatie koppelen](./media/migrate-scenarios-lift-and-shift/replication-policy2.png)



### <a name="enable-replication"></a>Replicatie inschakelen

Nu starten voor het repliceren van de virtuele machines. Site Recovery installeert de Mobility-Service op elke virtuele machine wanneer u replicatie inschakelt. 


1. Selecteer **Toepassing repliceren** > **Bron**.
2. Selecteer in **Bron** de configuratieserver.
3. Selecteer **Virtuele machines** in **Type machine**.
4. Selecteer in **vCenter/vSphere-hypervisor** de vCenter-server waarmee de vSphere-host wordt beheerd, of selecteer de host.
5. Selecteer de processerver (configuratieserver). Selecteer vervolgens **OK**.

    ![Replicatie inschakelen](./media/migrate-scenarios-lift-and-shift/enable-replication1.png)

1. Selecteer in **Doel** het abonnement en de resourcegroep waarin u de failover-VM's wilt maken. Kies het implementatiemodel dat u wilt gebruiken in Azure (klassiek of Resource Manager) voor de failover-VM's.
2. Selecteer het Azure-opslagaccount dat u wilt gebruiken voor het repliceren van gegevens.
3. Selecteer het Azure-netwerk en -subnet waarmee virtuele Azure-machines verbinding maken wanneer ze na een failover worden gemaakt.
4. Selecteer **Nu configureren voor geselecteerde machines** om de netwerkinstelling toe te passen op alle machines die u voor beveiliging selecteert. Selecteer **Later configureren** om per machine een Azure-netwerk te selecteren.
5. Selecteer het subnet in het virtuele netwerk. Selecteer vervolgens **OK**.

    ![Replicatie inschakelen](./media/migrate-scenarios-lift-and-shift/enable-replication2.png)

6. In **virtuele Machines** > **virtuele machines selecteren**, de virtuele machine (WEBVM) die u wilt selecteren. U kunt alleen machines selecteren waarvoor replicatie kan worden ingeschakeld. 

    ![Replicatie inschakelen](./media/migrate-scenarios-lift-and-shift/enable-replication3.png)

7. Voor het bewaken van virtuele machines die u toevoegt, controleert u de laatste detectietijd voor VM's in **Configuratieservers** > **Laatst contact om**. Als u VM's wilt toevoegen zonder te wachten op de geplande detectie, markeert u de configuratieserver (zonder deze te selecteren), en selecteert u **Vernieuwen**. Het kan 15 minuten of langer duren voordat wijzigingen zijn doorgevoerd en in de portal worden weergegeven.
8. Selecteer in **Eigenschappen** > **Eigenschappen configureren** het account dat door de processerver moet worden gebruikt om automatisch Mobility Service op de computer te installeren. 

    ![Replicatie inschakelen](./media/migrate-scenarios-lift-and-shift/enable-replication4.png)

9. Controleer of het juiste replicatiebeleid is geselecteerd in **Replicatie-instellingen** > **Replicatie-instellingen configureren**.
10. Selecteer **Replicatie inschakelen**.

    ![Replicatie inschakelen](./media/migrate-scenarios-lift-and-shift/enable-replication5.png)

11. Voortgang van volgen de **beveiliging inschakelen** taak **instellingen** > **taken** > **Site Recovery-taken**. 
12. Na de **beveiliging voltooien** taak wordt uitgevoerd, de machine is gereed voor failover en wordt weergegeven in **overzicht** > **Essentials**.

    ![Essentials](./media/migrate-scenarios-lift-and-shift/essentials.png)


## <a name="step-8-migrate-the-database-with-dms"></a>Stap 8: De database met DMS migreren

Maak een DMS-project en voer de migratie.


### <a name="create-a-database-migration-project"></a>Een migratie van de Database-project maken

1. De DMS-bron niet vinden in de Azure-portal en open het.
2. Selecteer **+ nieuw migratieproject**.
3. In **nieuw migratieproject**, Geef een naam voor het project.
4. Selecteer in **Bronservertype** de optie **SQL Server**. In **server doeltype**, selecteer **Azure SQL Database-beheerd instantie**.
5. Klik op **maken** om het project te maken.
6. In **bron details**, geef de verbindingsgegevens voor de on-premises gegevensbron SQL Server. Klik op **Opslaan**.
7. In **doel details**, geef de details van de verbinding voor het doel, is het vooraf is ingericht Azure SQL Database beheerd exemplaar waarnaar de database lokaal worden gemigreerd. Klik vervolgens op **Opslaan**.
8. In **Projectsamenvatting**controleert en controleer de gegevens die zijn gekoppeld aan het migratieproject.

    ![DMS-project](./media/migrate-scenarios-lift-and-shift/dms-project.png)

### <a name="migrate-the-database"></a>Migreren van de database

1. Nadat het project is gemaakt, is de migratiewizard wordt weergegeven.
2. Geef de referenties voor de bron en doel-servers en op **opslaan**. De wizard probeert aan te melden met uw lokale SQL Server.

    ![DMS-wizard](./media/migrate-scenarios-lift-and-shift/dms-wiz1.png)

3. In **toewijzen aan de doeldatabases**, selecteert u de brondatabase die u wilt migreren. Klik vervolgens op **Opslaan**.

    ![DMS-wizard](./media/migrate-scenarios-lift-and-shift/dms-wiz2.png)

4. In **doel details**, selecteer **ik weet mijn Doeldetails**. Geef de DNS-naam van uw beheerde exemplaren van SQL, samen met de referenties. Klik vervolgens op **Opslaan**.

    ![DMS-wizard](./media/migrate-scenarios-lift-and-shift/dms-wiz3.png)

5. Selecteer in het opgeslagen project **+ nieuwe activiteit**. Selecteer vervolgens **migratie uitvoeren**.
6. Voer desgevraagd de referenties voor de bron en doel-servers. Klik vervolgens op **Opslaan**.
7. In **toewijzen aan de doeldatabases**, selecteert u de brondatabase die u wilt migreren. Klik vervolgens op **opslaan**
8. In **migratie-instellingen configureren** > **back-uplocatie**, geef de netwerkshare die u hebt gemaakt in de on-premises machine. DMS gaat bron back-ups naar deze share.  Houd er rekening mee dat het serviceaccount met een SQL Server-exemplaar van bron schrijfbevoegdheden op deze share hebben moet.
9. Geef de gebruikersnaam en wachtwoord op dat DMS imiteren kan voor het uploaden van de back-upbestanden naar de Azure storage-container, om te herstellen.
10. Geef de SAS-URI die de DMS biedt toegang tot uw storage-account-container waarnaar de service de back-upbestanden uploadt en die wordt gebruikt voor migratie naar Azure SQL Database-beheerd instantie.  Klik vervolgens op **Opslaan**.
11. In **migratie samenvatting**, Geef een naam voor de migratieactiviteit > **uitvoeren van de migratie**.
12. In het project > **overzicht**, de migratiestatus controleren. Nadat de migratie is voltooid, moet u controleren dat de doeldatabases op de beheerde exemplaar bestaat.


## <a name="step-9-migrate-vms-with-site-recovery"></a>Stap 9: Migreren van virtuele machines met Site Recovery

We raden dat u een testfailover om ervoor te zorgen dat alles werkt zoals verwacht, voordat u een volledige migratie uitvoert uitvoeren. Wanneer u een testfailover uitvoert, gebeurt het volgende:

1. Controle wordt uitgevoerd om te controleren of alle van de voorwaarden die zijn vereist voor de migratie zijn in plaats van een vereisten.
2. De failover verwerkt de gegevens, zodat een virtuele Azure-machine kan worden gemaakt. Als u het meest recente herstelpunt selecteert, wordt een herstelpunt van de gegevens gemaakt.
3. Er wordt een Azure VM gemaakt met behulp van de gegevens die zijn verwerkt in de vorige stap.

### <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. Klik in **Beveiligde items** op **Gerepliceerde items** > VM.
2. Klik in de VM-eigenschappen op **+ Testfailover**.

    ![Testfailover](./media/migrate-scenarios-lift-and-shift/test-failover.png)

3. Selecteer **meest recente verwerkte**, de virtuele machine naar het laatste punt in tijd failover. Het tijdstempel wordt weergegeven. Met deze optie wordt er geen tijd besteed aan het verwerken van gegevens, zodat er een lage RTO (Recovery Time Objective) is.
2. Selecteer in **Failover testen** het Azure-doelnetwerk waarmee de virtuele Azure-machines moeten worden verbonden nadat de failover heeft plaatsgevonden.
3. Klik op **OK** om te beginnen met de failover. U kunt de voortgang bin de kluis volgen > **instellingen** > **taken** > **Testfailover**.
4. Nadat de failover is voltooid, wordt de replica-Azure-VM weergegeven in de Azure Portal > **Virtuele machines**. Controleer of de virtuele machine de juiste grootte heeft, of deze is verbonden met het juiste netwerk en of deze actief is. Nu moet u verbinding maken met de gerepliceerde virtuele machine in Azure.
5. Als u VM's van Azure wilt verwijderen die tijdens de failovertest zijn gemaakt, klikt u in de VM op **Failovertest opschonen**. Leg in **Notities** eventuele opmerkingen over de testfailover vast en sla deze op.

### <a name="migrate-the-machines"></a>De machines migreren

Nadat u hebt geverifieerd dat de testfailover werkt zoals verwacht, maakt u een herstelplan om te migreren naar Azure.

### <a name="create-a-recovery-plan"></a>Een herstelplan maken

1. Selecteer in de kluis **herstelplannen (Site Recovery)** > **+ herstelplan**.
2. In **herstelplan maken**, Geef een naam voor het plan.
3. Kies de bronserver voor configuratie en Azure als het doel. Selecteer **Resource Manager** voor het implementatiemodel. De bronlocatie moet hebben machines die zijn ingeschakeld voor failover en herstel. 
4. In **items selecteren**, de machines (WEBVM) toevoegen aan de planning. Klik vervolgens op **OK**.
5. Klik op **OK** om het plan te maken.

    ![Plan voor herstel](./media/migrate-scenarios-lift-and-shift/recovery-plan1.png)

### <a name="run-a-failover"></a>Een failover uitvoeren

1. In **herstelplannen**, klikt u op het plan > **Failover**.
2. In **Failover** > **herstelpunt**, selecteert u het meest recente herstelpunt.
3. De instelling voor de coderingssleutel is niet relevant in dit scenario.
4. Selecteer **Sluit de computer af voordat de failover wordt gestart**. Site Recovery zal proberen om virtuele bronmachines af te sluiten voordat de failover wordt geactiveerd. De failover wordt voortgezet zelfs als het afsluiten is mislukt. U kunt de voortgang van de failover volgen op de pagina **Taken**.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover1.png)

5. Controleer of de virtuele Azure-machine in Azure wordt weergegeven zoals verwacht.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover2.png)

6. Klik in **Gerepliceerde items** met de rechtermuisknop op de virtuele machine > **Migratie voltooien**. Hiermee wordt het migratieproces voltooid, de replicatie voor de VM gestopt en Site Recovery-facturering voor de virtuele machine gestopt.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover3.png)

### <a name="update-the-connection-string"></a>De verbindingsreeks bijwerken

De laatste stap in het migratieproces is de verbindingsreeks van de toepassing om te verwijzen naar de gemigreerde uitgevoerd op uw MI SQL-database bijwerken.

1. De nieuwe verbindingsreeks niet vinden in de Azure-portal door te klikken op **instellingen** > **verbindingsreeksen**.

    ![Failover](./media/migrate-scenarios-lift-and-shift/failover4.png)  

2. U moet deze tekenreeks met de gebruikersnaam en het wachtwoord voor uw SQL-MI bijwerken.
3. Nadat de tekenreeks is geconfigureerd, moet u de huidige verbindingsreeks in het bestand web.config van uw toepassing te vervangen.
4. Na het bijwerken van het bestand en opgeslagen, IIS op de WEBVM opnieuw te starten. U kunt dit doen met behulp van de IISRESET /RESTART vanaf een opdrachtprompt.
5. Nadat IIS opnieuw is opgestart, wordt uw toepassing nu gebruikmaken van de database die wordt uitgevoerd op uw SQL-MI.
6. Op dit moment de SQLVM machine on-premises af te sluiten en de migratie is voltooid.



## <a name="conclusion"></a>Conclusie

In dit scenario hebben we het volgende gedaan:

> [!div class="checklist"]
> * Onze lokale-database naar een beheerde-exemplaar van Azure SQL met DMS gemigreerd.
> * Onze lokale virtuele machines naar Azure virtuele machines, gemigreerd met de Azure Site Recovery-service.

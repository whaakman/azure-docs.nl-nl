---
title: Een meerlaagse op basis van een IIS-webtoepassing met Azure Site Recovery repliceren | Microsoft Docs
description: Informatie over het repliceren van de IIS web farm virtuele machines met Azure Site Recovery.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: gauravd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: nisoneji
ms.openlocfilehash: 7ed7df2451a44075a79f514cf67efbf479a2ebb1
ms.sourcegitcommit: d87b039e13a5f8df1ee9d82a727e6bc04715c341
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2018
---
# <a name="replicate-a-multi-tier-iis-based-web-application-by-using-site-recovery"></a>Een toepassing met meerdere lagen, op basis van een IIS-webserver repliceren met behulp van Site Recovery

Toepassingssoftware is de engine van de zakelijke productiviteit in een organisatie. Verschillende webtoepassingen kunnen een ander doel in een organisatie. Het is mogelijk dat sommige toepassingen, zoals toepassingen die worden gebruikt voor de verwerking van salaris, financiële toepassingen en klantgerichte websites, essentieel is voor een organisatie. Om verlies van productiviteit te voorkomen, is het belangrijk voor de organisatie deze toepassingen continu up-to-date en worden uitgevoerd. Belangrijker is, kan moeten deze toepassingen continu beschikbaar en helpen voorkomen beschadiging van het merk of afbeelding van de organisatie.

Kritieke webtoepassingen zijn meestal ingesteld als toepassingen met meerdere lagen: het web, de database en de toepassing zijn op andere niveaus. Naast de wordt verdeeld over verschillende lagen, de toepassingen mogelijk ook gebruiken meerdere servers in elke laag voor taakverdeling van het verkeer. Bovendien kunnen de toewijzingen tussen verschillende lagen en op de webserver worden gebaseerd op vaste IP-adressen. Failover, sommige van deze toewijzingen moeten worden bijgewerkt, met name als u meerdere websites zijn geconfigureerd op de webserver. Als u webtoepassingen SSL gebruiken, moet u certificaatbindingen bijwerken.

Van traditionele herstelmethoden die niet zijn gebaseerd op replicatie hebben betrekking op back-ups van verschillende configuratiebestanden, registerinstellingen, bindingen, aangepaste onderdelen (COM of .NET), inhoud en certificaten. Bestanden zijn hersteld met een reeks stappen. De herstelmethoden traditionele van back-up en herstellen van bestanden handmatig zijn omslachtige, foutgevoelige en niet schaalbaar. U kunt bijvoorbeeld eenvoudig back-up certificaten vergeet. Na een failover, bent u nog geen keuze maar aanschaffen van nieuwe certificaten voor de server.

Een goede noodhersteloplossing ondersteunt herstel van modellering plannen voor complexe toepassingsarchitecturen. Ook moet u kunnen aangepaste stappen toevoegen aan het herstelplan om af te handelen Toepassingstoewijzingen tussen lagen. Als er een ramp, bieden Toepassingstoewijzingen een enkele klik, ervoor maken oplossing die leiden tot een lagere RTO helpt.

Dit artikel wordt beschreven hoe u een webtoepassing die op basis van op Internet Information Services (IIS) met behulp van beveiligt [Azure Site Recovery](site-recovery-overview.md). Het artikel bevat informatie over aanbevolen procedures voor het repliceren van een drie lagen, op basis van de IIS webtoepassing naar Azure, hoe u een herstel na noodgevallen detailanalyse doet en hoe u failover voor de toepassing in Azure.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u weet hoe u de volgende taken uitvoeren:

* [Een virtuele machine repliceren naar Azure](site-recovery-vmware-to-azure.md)
* [Een recovery netwerk ontwerpen](site-recovery-network-design.md)
* [Voer een testfailover naar Azure](site-recovery-test-failover-to-azure.md)
* [Voer een failover naar Azure](site-recovery-failover.md)
* [Een domeincontroller worden gerepliceerd.](site-recovery-active-directory.md)
* [SQL Server repliceren](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Implementaties
Een IIS-gebaseerde webtoepassing volgt meestal een van de volgende implementaties:

**Implementatie-patroon 1**

Een-op basis van een IIS-webfarm met Application Request Routing (ARR), een IIS-server en SQL Server.

![Diagram van een IIS-webservices-farm met drie lagen](./media/site-recovery-iis/deployment-pattern1.png)

**Implementatie-patroon 2**

Een-op basis van een IIS-webfarm met ARR, een IIS-server, een toepassingsserver en SQL Server.

![Diagram van een IIS-webservices-farm met vier lagen](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site Recovery-ondersteuning

Voor de voorbeelden in dit artikel gebruiken we virtuele VMware-machines met IIS 7.5 op Windows Server 2012 R2 Enterprise. Omdat de replicatie van Site Recovery niet toepassingsspecifieke, worden de aanbevelingen in dit artikel om toe te passen in de scenario's die worden vermeld in de volgende tabel en voor verschillende versies van IIS verwacht.

### <a name="source-and-target"></a>Bron en doel

Scenario | Een secundaire site | Naar Azure
--- | --- | ---
Hyper-V | Ja | Ja
VMware | Ja | Ja
Fysieke server | Nee | Ja
Azure|N.v.t.|Ja

## <a name="replicate-virtual-machines"></a>Virtuele machines repliceren

Volg de instructies in om te starten op alle IIS web farm virtuele machines repliceren naar Azure, [testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

Als u een statisch IP-adres gebruikt, kunt u de IP-adres dat u wilt dat de virtuele machine te laten worden. Als u wilt het IP-adres instellen, gaat u naar **berekening en netwerk instellingen** > [**IP-adres doel**](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties).

![Schermopname die laat zien hoe het doel-IP-adres in het deelvenster Site Recovery berekenen en netwerk instellen](./media/site-recovery-active-directory/dns-target-ip.png)

## <a name="create-a-recovery-plan"></a>Een herstelplan maken
Een herstelplan ondersteunt het sequentiëren van verschillende niveaus in een toepassing met meerdere lagen tijdens een failover. Sequentiëren helpt consistentie van de toepassing. Wanneer u een herstelplan voor een webtoepassing met meerdere lagen maakt, volledige de stappen beschreven in [een herstelplan maken met behulp van Site Recovery](site-recovery-create-recovery-plans.md).

### <a name="add-virtual-machines-to-failover-groups"></a>Virtuele machines toevoegen aan de failover-groepen
Een typische IIS webtoepassing met meerdere lagen bestaat uit de volgende onderdelen:
* Een databaselaag met virtuele machines van SQL voldoende.
* De weblaag, die uit een IIS-server bestaat en een toepassingslaag. 

Virtuele machines op verschillende gebruikersgroepen op basis van de laag toevoegen:

1. Een herstelplan maken. De database laag virtuele machines onder groep 1 toevoegen. Dit zorgt ervoor dat virtuele laagmachines database worden uitgeschakeld laatste en eerste beschikbaar komen.
1. De toepassing laag virtuele machines onder groep 2 toevoegen. Dit zorgt ervoor dat de virtuele laagmachines toepassing komen nadat de databaselaag opstarten.
1. De web-laag virtuele machines in de groep 3 toevoegen. Dit zorgt ervoor dat de virtuele laagmachines web komen nadat de toepassingslaag opstarten.
1. Load balance virtual machines toevoegen in de groep 4. Dit zorgt ervoor dat virtuele machines voor load balance komen nadat de weblaag opstarten.

Zie voor meer informatie [aanpassen van het herstelplan](site-recovery-runbook-automation.md#customize-the-recovery-plan).


### <a name="add-a-script-to-the-recovery-plan"></a>Een script toevoegen aan het herstelplan
Voor de IIS-webfarm te laten functioneren, moet u mogelijk enkele bewerkingen op de virtuele Azure-machines na een failover of tijdens een testfailover uitvoeren. U kunt bepaalde na een failover-bewerkingen kunt automatiseren. U kunt bijvoorbeeld de DNS-vermelding bijwerken, wijzig de sitebinding van een of wijzigen van een verbindingsreeks met het bijbehorende scripts toevoegen aan het herstelplan. [Een VMM-script toevoegen aan een herstelplan](site-recovery-how-to-add-vmmscript.md) wordt beschreven hoe u geautomatiseerde taken instelt met behulp van een script.

#### <a name="dns-update"></a>DNS-updates
Als DNS is geconfigureerd voor dynamische DNS-updates, bijwerken virtuele machines de DNS-server gewoonlijk met het nieuwe IP-adres wanneer ze worden gestart. Als u toevoegen van een expliciete stap wilt in de DNS bijwerken met het nieuwe IP-adressen van de virtuele machines, het toevoegen van een [script voor het bijwerken van IP-adres in DNS](https://aka.ms/asr-dns-update) als een actie na een failover in recovery planningsgroepen.  

#### <a name="connection-string-in-an-applications-webconfig"></a>De verbindingsreeks in het bestand web.config van de toepassing
De verbindingsreeks kunt u de database die de website met communiceert. Als de verbindingsreeks aanwezig zijn op de naam van de database virtuele machine, zijn geen verdere stappen nodig na een failover. De toepassing kan automatisch communiceren met de database. Ook als het IP-adres voor de database virtuele machine wordt bewaard, dit niet worden moet de verbindingsreeks bijwerken. 

Als de verbindingsreeks naar de database virtuele machine verwijst met behulp van een IP-adres, moet deze worden bijgewerkt na een failover. De volgende verbindingspunten voor een tekenreeks met de database met het IP-adres bijvoorbeeld 127.0.1.2:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Voor het bijwerken van de verbindingsreeks in de weblaag toevoegen een [IIS verbinding updatescript](https://aka.ms/asr-update-webtier-script-classic) na groep 3 in het herstelplan.

#### <a name="site-bindings-for-the-application"></a>Sitebindingen voor de toepassing
Elke site bestaat uit de bindingsgegevens. De bindingsgegevens bevat het type van de binding, het IP-adres waarmee de IIS-server naar de aanvragen voor de site, het poortnummer en de hostnamen voor de site luistert. Tijdens de failover moet u mogelijk deze bindingen bijwerken als er een wijziging in de IP-adres dat is gekoppeld.

> [!NOTE]
>
> Als u de sitebinding ingesteld op **alle niet-toegewezen**, u hoeft niet te werken van deze binding na een failover. Ook als het IP-adres die zijn gekoppeld aan een site niet is gewijzigd na een failover, hoeft u niet te bijwerken van de sitebinding. (De retentie van het IP-adres is afhankelijk van de netwerkarchitectuur en subnetten die zijn toegewezen aan de primaire en herstelsites sites. Deze bij te werken mogelijk niet haalbaar voor uw organisatie.)

![Schermafbeelding van het SSL-binding instellen](./media/site-recovery-iis/sslbinding.png)

Als u het IP-adres gekoppeld aan een site, moet u alle sitebindingen bijwerken met het nieuwe IP-adres. Om te wijzigen van de sitebindingen, Voeg een [van IIS-updatescript laag](https://aka.ms/asr-web-tier-update-runbook-classic) na groep 3 in het herstelplan.

#### <a name="update-the-load-balancer-ip-address"></a>Het IP-adres van de load balancer bijwerken
Als u een virtuele machine ARR, bijwerken van de IP-adres toevoegen een [IIS ARR failover script](https://aka.ms/asr-iis-arrtier-failover-script-classic) na groep 4.

#### <a name="ssl-certificate-binding-for-an-https-connection"></a>SSL-certificaat-binding voor een HTTPS-verbinding
Een website wellicht een bijbehorende SSL-certificaat dat u helpt een veilige communicatie tussen de webserver en de browser van de gebruiker. Als de website een HTTPS-verbinding heeft en ook een bijbehorende HTTPS-site-binding naar het IP-adres van de IIS-server met een SSL-certificaat-binding heeft, moet u een nieuwe sitebinding voor het certificaat met de IP-adres van de IIS virtuele machine na een failover toevoegen.

Het SSL-certificaat kan worden uitgegeven op basis van deze onderdelen:

* De volledig gekwalificeerde domeinnaam van de website.
* De naam van de server.
* Een jokertekencertificaat voor de domeinnaam.  
* Een IP-adres. Als het SSL-certificaat is uitgegeven voor de IP-adres van de IIS-server, moet een ander SSL-certificaat worden uitgegeven voor de IP-adres van de IIS-server op de Azure-site. Een extra SSL-binding voor dit certificaat moet worden gemaakt. Daarom raden geen een SSL-certificaat uitgegeven voor het IP-adres. Deze optie is minder veel gebruikt en wordt binnenkort afgeschaft overeenstemming met het nieuwe certificaat autoriteit/browser forum wijzigingen.

#### <a name="update-the-dependency-between-the-web-tier-and-the-application-tier"></a>De afhankelijkheid tussen de weblaag en de toepassingslaag bijwerken
Als u een afhankelijkheid toepassingsspecifieke die gebaseerd op het IP-adres van de virtuele machines hebt, kunt u deze afhankelijkheid na een failover moet bijwerken.

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. Selecteer in de Azure-portal de Recovery Services-kluis.
2. Selecteer het herstelplan die u hebt gemaakt voor de IIS-webfarm.
3. Selecteer **Testfailover**.
4. U start de failover-test, selecteer het herstelpunt en het Azure-netwerk.
5. Wanneer de secundaire-omgeving is, kunt u validaties uitvoeren.
6. Wanneer validaties voltooid zijn, om op te ruimen de testfailoveromgeving selecteren **validaties voltooien**.

Zie voor meer informatie [testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Een failover uitvoeren

1. Selecteer in de Azure-portal de Recovery Services-kluis.
1. Selecteer het herstelplan die u hebt gemaakt voor de IIS-webfarm.
1. Selecteer **Failover**.
1. U start de failover, selecteer het herstelpunt.

Zie voor meer informatie [-Failover in Site Recovery](site-recovery-failover.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [andere toepassingen repliceren](site-recovery-workload.md) met behulp van Site Recovery.

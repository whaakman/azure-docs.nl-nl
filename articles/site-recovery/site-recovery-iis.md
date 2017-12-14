---
title: Repliceren een meerlaagse IIS-webtoepassing met Azure Site Recovery gebaseerd | Microsoft Docs
description: In dit artikel wordt beschreven hoe IIS web farm virtuele machines met Azure Site Recovery repliceren.
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
ms.openlocfilehash: cff6a7502e80eb4ff447cc99fe31b48cb660c27e
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="replicate-a-multi-tier-iis-based-web-application-using-azure-site-recovery"></a>Een meerlaagse op basis van IIS-webtoepassing met Azure Site Recovery repliceren

## <a name="overview"></a>Overzicht


Toepassingssoftware is de engine van de zakelijke productiviteit in een organisatie. Verschillende webtoepassingen kunnen een ander doel in een organisatie. Sommige van deze zoals salarissen verwerking, financiële toepassingen en websites klantgerichte kan zoveel mogelijk zijn essentieel voor een organisatie. Is belangrijk voor de organisatie van de vragen hebt en uitgevoerd op alle keren om te voorkomen dat verlies van productiviteit en, belangrijker schade voorkomen op de installatiekopie van het merk van de organisatie.

Kritieke webtoepassingen zijn meestal ingesteld als toepassingen met meerdere lagen met de webservice, de database en de toepassing op andere niveaus. Naast de wordt verdeeld over verschillende lagen, de toepassingen mogelijk ook gebruik van meerdere servers in elke laag verdelen het verkeer. Bovendien kunnen de toewijzingen tussen verschillende lagen en op de webserver worden gebaseerd op vaste IP-adressen. Op failover moet sommige van deze toewijzingen worden bijgewerkt, met name, als er meerdere websites geconfigureerd op de webserver. In geval van webtoepassingen met SSL moet-certificaatbindingen worden bijgewerkt.

Traditionele niet-replicatie op basis herstelmethoden hebben betrekking op een back-up van de verschillende configuratiebestanden, registerinstellingen, bindingen, aangepaste onderdelen (COM of .NET), inhoud en ook certificaten en het herstellen van de bestanden via een set van handmatige stappen. Deze technieken worden duidelijk lastige fout foutgevoelige en niet schaalbaar. Het is bijvoorbeeld eenvoudig mogelijk dat u vergeet back-ups van certificaten en geen andere keuze laten staan, maar het aanschaffen van nieuwe certificaten voor de server na een failover.

Een goede noodhersteloplossing mag modellering van herstelplannen rond de bovenstaande complexe toepassingsarchitecturen en hebben de mogelijkheid aangepaste stappen voor het afhandelen van de toepassingstoewijzingen tussen verschillende lagen daarom bieden een éénkliks-toevoegen Controleer of de schermopname oplossing er in een noodsituatie leidt tot een lagere RTO.


Dit artikel wordt beschreven hoe u een IIS op basis van web application gebruik beveiligt een [Azure Site Recovery](site-recovery-overview.md). Dit artikel wordt uitgelegd van aanbevolen procedures voor het repliceren van een drielaagse op basis van IIS-webtoepassing naar Azure, hoe u een herstel na noodgevallen detailanalyse kunt doen en hoe u de failover kan de toepassing in Azure.


## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg er dan voor dat u het volgende weten:

1. [Een virtuele machine repliceren naar Azure](site-recovery-vmware-to-azure.md)
1. Hoe [ontwerpen van een netwerk herstel](site-recovery-network-design.md)
1. [Een testfailover uitvoeren naar Azure](./site-recovery-test-failover-to-azure.md)
1. [U een failover naar Azure](site-recovery-failover.md)
1. Hoe [repliceren van een domeincontroller](site-recovery-active-directory.md)
1. Hoe [SQL-Server repliceren](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Implementaties
Hier volgt doorgaans een webtoepassing IIS op basis van een van de volgende implementaties:

** Implementatie patroon 1 ** webfarm met toepassing aanvragen Routing(ARR), IIS-Server en Microsoft SQL Server op basis van een IIS.

![Patroon van de implementatie](./media/site-recovery-iis/deployment-pattern1.png)

**Implementatie-patroon 2** webfarm met toepassing aanvragen Routing(ARR), IIS-Server, Application Server en Microsoft SQL Server op basis van een IIS.


![Patroon van de implementatie](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Site Recovery-ondersteuning

Omwille van dit artikel virtuele VMware-machines maken met Server IIS versie 7.5 op Windows Server 2012 R2 Enterprise werden gebruikt. Als de replicatie van site recovery networkdirect van toepassing is, worden de aanbevelingen die verwacht voor het opslaan op de volgende scenario's, evenals en voor andere versie van IIS.

### <a name="source-and-target"></a>Bron en doel

**Scenario** | **Een secundaire site** | **Naar Azure**
--- | --- | ---
**Hyper-V** | Ja | Ja
**VMware** | Ja | Ja
**Fysieke server** | Nee | Ja
**Azure**|N.v.t.|Ja

## <a name="replicate-virtual-machines"></a>Virtuele machines repliceren

Ga als volgt [in deze richtlijnen](site-recovery-vmware-to-azure.md) starten alle IIS web farm virtuele machines repliceren naar Azure.

Als u met behulp van een statische IP-adres en geeft u het IP-adres dat u wilt dat de virtuele machine moet worden uitgevoerd de [ **IP-adres doel** ](./site-recovery-replicate-vmware-to-azure.md#view-and-manage-vm-properties) instellen in de berekenings-en netwerkinstellingen.

![Doel-IP](./media/site-recovery-active-directory/dns-target-ip.png)


## <a name="creating-a-recovery-plan"></a>Een herstelplan maken

Een herstelplan kunt de failover van de verschillende lagen in een toepassing met meerdere lagen, daarom toepassing consistentie van de sequentiëring. Volg de onderstaande stappen te volgen bij het maken van een herstelplan voor een webtoepassing met meerdere lagen.  [Meer informatie over het maken van een herstelplan](./site-recovery-create-recovery-plans.md).

### <a name="adding-virtual-machines-to-failover-groups"></a>Virtuele machines worden toegevoegd aan failover-groepen
Een typische meerdere lagen IIS-webtoepassing bestaat uit een databaselaag met SQL-virtuele machines, de weblaag gevormd door een IIS-server en een toepassingslaag. Deze virtuele machines toevoegen aan andere groep op basis van een laag als hieronder. [Meer informatie over het aanpassen van herstelplan](site-recovery-runbook-automation.md#customize-the-recovery-plan).

1. Een herstelplan maken. De database laag virtuele machines onder groep 1 om ervoor te zorgen dat deze afsluiten laatste en eerste gebracht toevoegen.

1. De toepassing laag virtuele machines onder groep 2 toevoegen zodat ze beschikbaar komen nadat de databaselaag opstarten.

1. De web-laag virtuele machines in de groep 3 toevoegen zodat ze beschikbaar komen nadat de toepassingslaag opstarten.

1. Load balance virtuele machines in de groep 4 toevoegen zodat ze beschikbaar komen nadat de weblaag opstarten.


### <a name="adding-scripts-to-the-recovery-plan"></a>Scripts toe te voegen aan het herstelplan
U moet wellicht enkele bewerkingen op de virtuele Azure-machines na failover/testfailover kan ervoor zorgen dat IIS web-farm werkt goed. U kunt de failoverbewerking post zoals het bijwerken van DNS-vermelding automatiseren sitebinding wijzigen, wijzigen in de verbindingsreeks door de bijbehorende scripts toevoegen in het herstelplan zoals hieronder. [Meer informatie over het toevoegen van herstelplan script](./site-recovery-create-recovery-plans.md#add-scripts).

#### <a name="dns-update"></a>DNS-updates
Als de DNS-server is geconfigureerd voor dynamische DNS-updates vervolgens virtuele machines normaal gesproken werkt u de DNS-server met het nieuwe IP-zodra ze starten. Als u toevoegen van een expliciete stap wilt voor het bijwerken van DNS met het nieuwe IP-adressen van de virtuele machines en vervolgens voegt u dit [script voor het bijwerken van IP-adres in DNS](https://aka.ms/asr-dns-update) als een post-actie op herstel planningsgroepen.  

#### <a name="connection-string-in-an-applications-webconfig"></a>De verbindingsreeks in het bestand web.config van de toepassing
De verbindingsreeks kunt u de database die de website met communiceert.

Als de verbindingsreeks aanwezig zijn op de naam van de database virtuele machine, zijn geen verdere stappen nodig na failover en de toepassing wordt automatisch communicatie mogelijk met de database. Ook als het IP-adres voor de database virtuele machine wordt bewaard, meer deze niet nodig om bij te werken van de verbindingsreeks. Als de verbindingsreeks naar de database-virtuele machine een IP-adres verwijst, moet deze worden bijgewerkt na failover. Bijvoorbeeld de onderstaande tekenreeks verwijst naar de database met IP-adres 127.0.1.2

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

U kunt de verbindingsreeks in de weblaag bijwerken door toe te voegen [IIS verbinding updatescript](https://aka.ms/asr-update-webtier-script-classic) na groep 3 in het herstelplan.

#### <a name="site-bindings-for-the-application"></a>Sitebindingen voor de toepassing
Elke site bestaat uit het binden van gegevens met het type van de binding, het IP-adres waarmee de IIS-server naar de aanvragen voor de site, het poortnummer en de hostnamen van de voor de site luistert. Op het moment van een failover moet deze bindingen mogelijk worden bijgewerkt als er een wijziging in het IP-adres dat is gekoppeld.

> [!NOTE]
>
> Als u hebt gemarkeerd 'alle niet-toegewezen' voor de sitebinding zoals in het onderstaande voorbeeld, moet u niet bijwerken van deze binding post voor failover. Ook als het IP-adres die zijn gekoppeld aan een site niet is gewijzigd na failover, de sitebinding moet niet worden bijgewerkt (bewaren van het IP-adres, is afhankelijk van de netwerkarchitectuur en subnetten toegewezen aan de primaire en herstelsites sites en daarom kunnen mogelijk niet haalbaar voor uw organisatie.)

![SSL-Binding](./media/site-recovery-iis/sslbinding.png)

Als u het IP-adres aan een site hebt gekoppeld, moet u alle sitebindingen bijwerken met het nieuwe IP-adres. U kunt toevoegen [updatescript voor IIS-webserver laag](https://aka.ms/asr-web-tier-update-runbook-classic) na groep 3 in herstelplan om de sitebindingen te wijzigen.


#### <a name="update-load-balancer-ip-address"></a>IP-adres van load balancer bijwerken
Als u routering van toepassingsaanvragen virtuele machine hebt, voegt u [IIS ARR failover script](https://aka.ms/asr-iis-arrtier-failover-script-classic) na groep 4 bijwerken van het IP-adres.

#### <a name="the-ssl-cert-binding-for-an-https-connection"></a>De binding van de SSL-certificaat voor een https-verbinding
Websites kan een bijbehorende SSL-certificaat die helpt om een veilige communicatie tussen de webserver en de browser van de gebruiker hebben. Als de website een https-verbinding en een binding van de bijbehorende https-site naar het IP-adres van de IIS-server met een SSL-certificaat-binding heeft, wordt een nieuwe sitebinding moet worden toegevoegd voor het certificaat met de IP-adres van de post failover van de IIS-virtuele machine.

Het SSL-certificaat kan worden uitgegeven tegen-

een) de FQDN-naam van de website<br>
b) de naam van de server<br>
c) een jokertekencertificaat voor de domeinnaam<br>
d) een IP-adres – als het SSL-certificaat is uitgegeven voor de IP-adres van de IIS-server een andere SSL-certificaat moet worden uitgegeven voor de IP-adres van de IIS-server op de Azure-site en een extra SSL-binding voor dit certificaat moet worden gemaakt. U wordt daarom aangeraden om niet te gebruiken een SSL-certificaat dat is uitgegeven op basis van IP. Dit is een minder gebruikte optie en snel aan de hand van nieuwe wijzigingen van de CA/browser forum wordt afgeschaft.

#### <a name="update-the-dependency-between-the-web-and-the-application-tier"></a>De afhankelijkheid tussen de webserver en de toepassingslaag bijwerken
Als u een specifieke afhankelijkheid van toepassing op basis van het IP-adres van de virtuele machines hebt, moet u deze afhankelijkheid post failover bijwerken.

## <a name="doing-a-test-failover"></a>Een testfailover uitvoeren
Ga als volgt [in deze richtlijnen](site-recovery-test-failover-to-azure.md) een testfailover uitvoeren.

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
1.  Klik op het herstelplan gemaakt voor IIS-webfarm.
1.  Klik op 'Testfailover'.
1.  Selecteer het herstelpunt en virtuele Azure-netwerk om de test failover-proces te starten.
1.  Als de secundaire-omgeving is, kunt u uw validaties kunt uitvoeren.
1.  Zodra de validaties voltooid zijn, kunt u 'Validaties voltooid' en de testfailoveromgeving wordt gereinigd.

## <a name="doing-a-failover"></a>U een failover uitvoert
Ga als volgt [in deze richtlijnen](site-recovery-failover.md) bij het uitvoeren van een failover.

1.  Ga naar de Azure-portal en selecteer de Recovery Services-kluis.
1.  Klik op het herstelplan gemaakt voor IIS-webfarm.
1.  Klik op 'Failover'.
1.  Selecteer het herstelpunt om de failoverproces te starten.

## <a name="next-steps"></a>Volgende stappen
U kunt meer lezen over [andere toepassingen repliceren](site-recovery-workload.md) met Site Recovery.

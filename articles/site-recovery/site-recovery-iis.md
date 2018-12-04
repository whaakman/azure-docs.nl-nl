---
title: Herstel na noodgevallen instellen voor een meerdere lagen op basis van een IIS-webtoepassing met Azure Site Recovery | Microsoft Docs
description: Leer hoe u IIS web farm virtuele machines met Azure Site Recovery repliceert.
author: nsoneji
manager: gauravd
ms.service: site-recovery
ms.topic: article
ms.date: 11/27/2018
ms.author: nisoneji
ms.openlocfilehash: 49435665ae1e99dd2b9696e5e5bb048e438dcc4c
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52832953"
---
# <a name="set-up-disaster-recovery-for-a-multi-tier-iis-based-web-application"></a>Herstel na noodgevallen instellen voor een meerdere lagen op basis van een IIS-webtoepassing

Toepassingssoftware is de engine van bedrijfsproductiviteit in een organisatie. Verschillende webtoepassingen kunnen verschillende doeleinden gebruikt in een organisatie. Het is mogelijk dat sommige toepassingen, zoals toepassingen die worden gebruikt voor de verwerking van de salarisadministratie, financiële toepassingen en klantgerichte websites, essentieel is voor een organisatie. Om te voorkomen dat verlies van productiviteit, is het belangrijk voor de organisatie deze toepassingen continu ingesteld en geactiveerd. Nog belangrijker, kan deze toepassingen continu beschikbaar die helpen voorkomen schade aan het merk of de afbeelding van de organisatie.

Essentiële web-apps zijn doorgaans ingesteld als toepassingen met meerdere lagen: de web-, database en toepassing zijn op verschillende lagen. Naast wordt verdeeld over verschillende lagen, de toepassingen mogelijk ook gebruiken meerdere servers in elke laag voor taakverdeling tussen het verkeer. Bovendien kunnen de toewijzingen tussen verschillende lagen en op de webserver worden gebaseerd op vaste IP-adressen. Bij failover, sommige van deze toewijzingen moeten worden bijgewerkt, met name als er meerdere websites zijn geconfigureerd op de webserver. Als de web-apps gebruik van SSL, moet u certificaatbindingen bijwerken.

Traditionele herstelmethoden die niet zijn gebaseerd op replicatie worden back-ups van verschillende configuratiebestanden, registerinstellingen, bindingen, aangepaste onderdelen (COM of .NET), inhoud en certificaten. Bestanden zijn hersteld door een set handmatige stappen. De herstelmethoden traditionele van back-up en herstellen van bestanden handmatig zijn verkeerd ingevoerde adressen, foutgevoelige en niet schaalbaar. U kunt bijvoorbeeld eenvoudig vergeten om de back-up van certificaten. Na een failover, bent u nog geen keuze maar aan te schaffen, nieuwe certificaten voor de server.

Een goede noodhersteloplossing ondersteunt het maken van modellering herstel plannen voor architecturen voor complexe toepassingen. Ook moet u kunnen het toevoegen van aangepaste stappen aan het herstelplan om af te handelen Toepassingstoewijzingen tussen lagen. Als er een noodgeval, bieden Toepassingstoewijzingen een één-op, of-shot oplossing waarmee leiden tot een lagere RTO.

In dit artikel wordt beschreven hoe u een webtoepassing die wordt op basis van op Internet Information Services (IIS) met behulp van beveiligen [Azure Site Recovery](site-recovery-overview.md). Het artikel bevat informatie over aanbevolen procedures voor het repliceren van een drie lagen, op basis van een IIS-toepassing naar Azure, hoe u een Dr-herstelanalyse doet en hoe u failover van de toepassing in Azure.

## <a name="prerequisites"></a>Vereisten

Voordat u begint, zorg ervoor dat u weet hoe u de volgende taken uitvoeren:

* [Een virtuele machine repliceren naar Azure](vmware-azure-tutorial.md)
* [Een herstelnetwerk ontwerpen](site-recovery-network-design.md)
* [Voer een failovertest uit naar Azure](site-recovery-test-failover-to-azure.md)
* [Voer een failover naar Azure](site-recovery-failover.md)
* [Een domeincontroller worden gerepliceerd](site-recovery-active-directory.md)
* [SQL Server repliceren](site-recovery-sql.md)

## <a name="deployment-patterns"></a>Implementaties
De volgende doorgaans een op basis van een IIS-webtoepassing een van de volgende implementaties:

**Implementatie-patroon 1**

Een-op basis van een IIS-webfarm met Application Request Routing (ARR), een IIS-server en SQL Server.

![Diagram van een-op basis van een IIS-webfarm met drie lagen](./media/site-recovery-iis/deployment-pattern1.png)

**Implementatie-patroon 2**

Een-op basis van een IIS-webfarm met ARR, een IIS-server, een toepassingsserver bevinden en SQL Server.

![Diagram van een-op basis van een IIS-webfarm met vier lagen](./media/site-recovery-iis/deployment-pattern2.png)

## <a name="site-recovery-support"></a>Ondersteuning voor Site Recovery

Voor de voorbeelden in dit artikel gebruiken we virtuele VMware-machines met IIS 7.5 op Windows Server 2012 R2 Enterprise. Omdat de Site Recovery-replicatie niet toepassingsspecifieke, worden de aanbevelingen in dit artikel om toe te passen in de scenario's die worden vermeld in de volgende tabel, en voor verschillende versies van IIS verwacht.

### <a name="source-and-target"></a>Bron en doel

Scenario | Op een secundaire site | In Azure
--- | --- | ---
Hyper-V | Ja | Ja
VMware | Ja | Ja
Fysieke server | Nee | Ja
Azure|N.v.t.|Ja

## <a name="replicate-virtual-machines"></a>Virtuele machines repliceren

Volg de instructies in voor het starten van de IIS web farm virtuele machines repliceren naar Azure, [testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

Als u een statisch IP-adres gebruikt, kunt u de IP-adres dat u wilt dat de virtuele machine op te nemen. Om het IP-adres, gaat u naar **berekening en netwerk instellingen** > **doel-IP**.

![Schermopname die laat zien hoe u het doel-IP-adres instellen in het deelvenster met Site Recovery Compute en netwerk](./media/site-recovery-active-directory/dns-target-ip.png)

## <a name="create-a-recovery-plan"></a>Een herstelplan maken
Een herstelplan ondersteunt de sequentiëren van de verschillende lagen in een toepassing met meerdere lagen tijdens een failover. Sequentiëren helpt consistentie van de toepassing. Wanneer u een plan voor herstel voor een webtoepassing met meerdere lagen maakt, voltooid de stappen beschreven in [een herstelplan maken met behulp van Site Recovery](site-recovery-create-recovery-plans.md).

### <a name="add-virtual-machines-to-failover-groups"></a>Virtuele machines toevoegen aan de failover-groepen
Een typische IIS webtoepassing met meerdere lagen bestaat uit de volgende onderdelen:
* Een databaselaag met SQL virtuele machines.
* De weblaag, die uit een IIS-server bestaat en een toepassingslaag. 

Virtuele machines toevoegen aan verschillende groepen op basis van de laag:

1. Maak een plan voor herstel. De database-laag virtuele machines onder groep 1 toevoegen. Dit zorgt ervoor dat de database-laag virtuele machines zijn laatste afsluiten en eerst een gebracht.
1. De toepassing laag virtuele machines onder groep 2 toevoegen. Dit zorgt ervoor dat de toepassing laag virtuele machines worden gebracht van nadat de databaselaag van pas.
1. De web-laag virtuele machines in de groep 3 toevoegen. Dit zorgt ervoor dat de web-laag virtuele machines worden gebracht van nadat de toepassingslaag van pas.
1. Belasting verdelen over virtuele machines in de groep 4 toevoegen. Dit zorgt ervoor dat de belasting verdelen over virtuele machines worden gebracht van nadat de weblaag van pas.

Zie voor meer informatie, [aanpassen van het herstelplan](site-recovery-runbook-automation.md#customize-the-recovery-plan).


### <a name="add-a-script-to-the-recovery-plan"></a>Een script toevoegen aan het herstelplan te gaan
Voor de IIS-webfarm te laten functioneren, moet u bepaalde bewerkingen op de virtuele Azure-machines na een failover of tijdens een testfailover uitvoeren. U kunt bepaalde bewerkingen na een failover automatiseren. U kunt bijvoorbeeld de DNS-vermelding bijwerken, de sitebinding van een wijzigen, of een verbindingsreeks wijzigen door de bijbehorende scripts toevoegen aan het herstelplan te gaan. [Een VMM-script toevoegen aan een herstelplan](site-recovery-how-to-add-vmmscript.md) wordt beschreven hoe u het instellen van geautomatiseerde taken met behulp van een script.

#### <a name="dns-update"></a>DNS-update
Als DNS-server is geconfigureerd voor dynamische DNS-updates, bijwerken virtuele machines de DNS-server gewoonlijk met het nieuwe IP-adres wanneer ze worden gestart. Als u toevoegen van een expliciete stap DNS bijwerken met het nieuwe IP-adressen van de virtuele machines wilt, voegt u een [script voor het bijwerken van IP-adres in DNS](https://aka.ms/asr-dns-update) als een actie na de failover voor herstel planningsgroepen.  

#### <a name="connection-string-in-an-applications-webconfig"></a>Verbindingsreeks in web.config van een toepassing
De verbindingsreeks Hiermee geeft u de database die de website met communiceert. Als de verbindingsreeks de naam van de database virtuele machine heeft, zijn geen verdere stappen vereist na de failover. De toepassing kan automatisch communiceren met de database. Ook als het IP-adres voor de database virtuele machine wordt bewaard, zijn deze niet moet de verbindingsreeks bijwerken. 

Als de verbindingsreeks naar de database virtuele machine verwijst met behulp van een IP-adres, moet deze worden bijgewerkt nadat failover is uitgevoerd. De volgende verbindingspunten voor een tekenreeks met de database met het IP-adres bijvoorbeeld 127.0.1.2:

        <?xml version="1.0" encoding="utf-8"?>
        <configuration>
        <connectionStrings>
        <add name="ConnStringDb1" connectionString="Data Source= 127.0.1.2\SqlExpress; Initial Catalog=TestDB1;Integrated Security=False;" />
        </connectionStrings>
        </configuration>

Voor het bijwerken van de verbindingsreeks in de weblaag, Voeg een [script voor het bijwerken van IIS-verbinding](https://aka.ms/asr-update-webtier-script-classic) na 3 van de groep in het herstelplan te gaan.

#### <a name="site-bindings-for-the-application"></a>Sitebindingen voor de toepassing
Elke site bestaat uit de bindingsgegevens. De bindingsgegevens bevatten de aard van de binding, het IP-adres waarop de IIS-server naar aanvragen voor de site en het poortnummer dat de hostnamen voor de site luistert. Tijdens de failover moet u mogelijk deze bindingen bijwerken als er een wijziging in de IP-adres dat is gekoppeld aan deze.

> [!NOTE]
>
> Als u de sitebinding ingesteld op **alle niet-toegewezen**, moet u niet bijwerken van deze binding nadat failover is uitgevoerd. Ook als het IP-adres dat is gekoppeld aan een site niet is gewijzigd na een failover, moet niet u de sitebinding niet bijwerken. (De retentie van het IP-adres is afhankelijk van de netwerkarchitectuur en subnetten die zijn toegewezen aan de primaire en herstellocaties. Deze bij te werken mogelijk niet haalbaar is voor uw organisatie.)

![Schermafbeelding van het SSL-binding instellen](./media/site-recovery-iis/sslbinding.png)

Als u het IP-adres gekoppeld aan een site, moet u alle sitebindingen bijwerken met het nieuwe IP-adres. Toevoegen als u wilt wijzigen van de sitebindingen, een [updatescript laag voor IIS-webserver](https://aka.ms/asr-web-tier-update-runbook-classic) na 3 van de groep in het herstelplan te gaan.

#### <a name="update-the-load-balancer-ip-address"></a>De IP-adres van de load balancer bijwerken
Als u een virtuele machine ARR, om bij te werken van het IP-adres toevoegen een [IIS ARR failoverscript](https://aka.ms/asr-iis-arrtier-failover-script-classic) na 4 van de groep.

#### <a name="ssl-certificate-binding-for-an-https-connection"></a>SSL-certificaatbinding voor een HTTPS-verbinding
Een website mogelijk een bijbehorende SSL-certificaat dat helpt zorgen voor een veilige communicatie tussen de webserver en de browser van de gebruiker. Als de website een HTTPS-verbinding heeft, en ook een bijbehorende HTTPS sitebinding naar het IP-adres van de IIS-server met een SSL-certificaatbinding heeft, moet u een nieuwe sitebinding voor het certificaat met het IP-adres van de IIS virtuele machine na een failover toevoegen.

Het SSL-certificaat kan worden uitgegeven op basis van deze onderdelen:

* De volledig gekwalificeerde domeinnaam van de website.
* De naam van de server.
* Een jokertekencertificaat voor de domeinnaam.  
* Een IP-adres. Als het SSL-certificaat is uitgegeven voor de IP-adres van de IIS-server, moet een ander SSL-certificaat worden uitgegeven voor de IP-adres van de IIS-server op de Azure-site. Een extra SSL-binding voor dit certificaat moet worden gemaakt. Als gevolg hiervan, wordt u aangeraden niet met behulp van een SSL-certificaat dat is uitgegeven voor het IP-adres. Deze optie is minder gebruikte en binnenkort worden afgeschaft in overeenstemming met het nieuwe certificaat instantie/browser forum wijzigingen.

#### <a name="update-the-dependency-between-the-web-tier-and-the-application-tier"></a>Bijwerken van de afhankelijkheid tussen de weblaag en de application tier
Als u een toepassingsspecifieke afhankelijkheid die gebaseerd op het IP-adres van de virtuele machines hebt, kunt u deze afhankelijkheid na een failover moet bijwerken.

## <a name="run-a-test-failover"></a>Een testfailover uitvoeren

1. Selecteer uw Recovery Services-kluis in de Azure-portal.
2. Selecteer het herstelplan te gaan die u hebt gemaakt voor de IIS-webfarm.
3. Selecteer **Failover testen**.
4. Selecteer het herstelpunt en de Azure-netwerk voor het starten van de test-failover-proces.
5. Wanneer de secundaire-omgeving is, kunt u controles uitvoeren.
6. Wanneer de controles zijn voltooid, schoon de testfailoveromgeving selecteren **voltooien van validaties**.

Zie voor meer informatie, [testfailover naar Azure in Site Recovery](site-recovery-test-failover-to-azure.md).

## <a name="run-a-failover"></a>Een failover uitvoeren

1. Selecteer uw Recovery Services-kluis in de Azure-portal.
1. Selecteer het herstelplan te gaan die u hebt gemaakt voor de IIS-webfarm.
1. Selecteer **Failover**.
1. Selecteer het herstelpunt dat voor het starten van het failoverproces.

Zie voor meer informatie, [Failover in Site Recovery](site-recovery-failover.md).

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [andere toepassingen repliceren](site-recovery-workload.md) met behulp van Site Recovery.

---
title: Hoge beschikbaarheid en herstel na noodgevallen van SAP HANA in Azure (grote exemplaren) | Microsoft Docs
description: Tot stand brengen voor hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA in Azure (grote exemplaren)
services: virtual-machines-linux
documentationcenter: 
author: saghorpa
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/31/2017
ms.author: saghorpa
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 09aa98a35fa8286828a99c49a33a80d5938afe3a
ms.sourcegitcommit: 43c3d0d61c008195a0177ec56bf0795dc103b8fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="sap-hana-large-instances-high-availability-and-disaster-recovery-on-azure"></a>SAP HANA grote exemplaren hoge beschikbaarheid en herstel na noodgevallen in Azure 

Hoge beschikbaarheid en herstel na noodgeval (DR) zijn belangrijke aspecten van uw bedrijfskritieke SAP HANA worden uitgevoerd op Azure (grote exemplaren)-server. Het is belangrijk om te werken met SAP, uw system integrator of Microsoft goed ontwerpen en implementeren van het juiste hoge beschikbaarheid en de strategie voor herstel na noodgevallen. Het is ook belangrijk dat u rekening houden met het beoogde herstelpunt (RPO) en de beoogde hersteltijd die specifiek voor uw omgeving zijn.

Microsoft ondersteunt de mogelijkheden van sommige SAP HANA hoge beschikbaarheid met grote HANA-exemplaren. Deze mogelijkheden zijn:

- **Storage-replicatie**: het opslagsysteem kunnen alle gegevens repliceren naar een andere grote exemplaar HANA stempel in een andere Azure-regio. SAP HANA werkt onafhankelijk van deze methode.
- **HANA system replicatie**: de replicatie van alle gegevens in SAP HANA naar een afzonderlijke SAP HANA-systeem. De beoogde hersteltijd wordt geminimaliseerd door middel van gegevensreplicatie met regelmatige tussenpozen. SAP HANA ondersteunt asynchrone, synchrone in het geheugen en synchrone modus. Synchrone modus wordt alleen aanbevolen voor SAP HANA-systemen die binnen de hetzelfde datacenter of minder dan 100 km uit elkaar liggen. In het huidige ontwerp HANA grote exemplaar stempels kan HANA system replicatie voor hoge beschikbaarheid alleen worden gebruikt. HANA system replicatie is een derde partij reverse proxy-onderdeel op dit moment is vereist voor herstel na noodgevallen configuraties in een andere Azure-regio. 
- **Automatische failover host**: een oplossing voor lokale fout met betrekking tot herstel voor SAP HANA als alternatief voor HANA system replication wilt gebruiken. Als het hoofdknooppunt niet meer beschikbaar is, het configureren van een of meer stand-by SAP HANA-knooppunten in de modus voor scale-out en SAP HANA automatisch wordt overgenomen door een stand-by-knooppunt.

SAP HANA in Azure (grote exemplaren) wordt aangeboden in twee Azure-regio's die betrekking hebben op drie verschillende geopolitieke regio's (VS, Australië en Europa). Twee verschillende regio's of host HANA grote exemplaar stempels met afzonderlijke speciaal netwerk circuits die worden gebruikt verbonden bent voor het repliceren van opslag-momentopnamen voor herstel na noodgevallen methoden. De replicatie is niet standaard ingesteld. Deze is ingesteld voor klanten die besteld functionaliteit voor herstel na noodgevallen. Storage-replicatie is afhankelijk van het gebruik van opslag-momentopnamen voor grote HANA-exemplaren. Het is niet mogelijk een Azure-regio kiezen als een DR-gebied dat in een ander geopolitieke gebied. 

De volgende tabel ziet u de combinaties van ondersteunde methoden voor hoge beschikbaarheid en herstel na noodgevallen en:

| In grote exemplaren HANA ondersteund scenario | Optie voor hoge beschikbaarheid | Optie voor herstel na noodgevallen | Opmerkingen |
| --- | --- | --- | --- |
| Eén knooppunt | Niet beschikbaar. | Speciale DR-instellingen.<br /> Multipurpose DR-instellingen. | |
| Automatische failover hosten: N + m<br /> met inbegrip van 1 + 1 | Mogelijk met het maken van de functie active stand-by.<br /> HANA Hiermee bepaalt u de functie switch. | Speciale DR-instellingen.<br /> Multipurpose DR-instellingen.<br /> DR-synchronisatie met behulp van storage-replicatie. | HANA volumesets zijn gekoppeld aan alle knooppunten (n + m).<br /> DR-site moet hetzelfde aantal knooppunten hebben. |
| HANA system replicatie | Mogelijk met de primaire of secundaire installatie.<br /> Secundaire verplaatst naar de primaire rol in het geval van een failover.<br /> Beheer de failover HANA system replicatie- en OS. | Speciale DR-instellingen.<br /> Multipurpose DR-instellingen.<br /> DR-synchronisatie met behulp van storage-replicatie.<br /> DR met behulp van HANA system replicatie is nog niet mogelijk zonder onderdelen van derden. | Afzonderlijke set volumes op schijven zijn gekoppeld aan elk knooppunt.<br /> Alleen volumes op schijven van de secundaire replica in de productiesite worden gerepliceerd naar de locatie voor Noodherstel.<br /> Een set van volumes is vereist op de DR-site. | 

Een speciale DR-setup is waar de eenheid HANA grote exemplaar in de DR-site niet wordt gebruikt voor het uitvoeren van een andere workload of niet-productieomgeving. De eenheid passief is en is geïmplementeerd alleen als er een ramp failover wordt uitgevoerd. Dit is echter geen een uitstekende keuze is voor veel klanten.

Een DR-configuratie voor meerdere doeleinden is waarop een niet-productieve werkbelasting wordt uitgevoerd in de eenheid HANA grote exemplaar op de DR-site. In noodgevallen afsluiten van de niet-productieomgeving, koppelt u de sets (Extra) volume opslag gerepliceerd en vervolgens het starten van de productie HANA-exemplaar. Deze configuratie gebruiken in de meeste klanten die de functionaliteit van het herstel na noodgevallen HANA grote exemplaar gebruiken. 


U vindt meer informatie over maximale beschikbaarheid voor SAP HANA in de volgende SAP-artikelen: 

- [Technisch document SAP HANA hoge beschikbaarheid](http://go.sap.com/documents/2016/05/f8e5eeba-737c-0010-82c7-eda71af511fa.html)
- [Handleiding voor SAP HANA-beheer](http://help.sap.com/hana/SAP_HANA_Administration_Guide_en.pdf)
- [SAP Academy Video voor SAP HANA System replicatie](http://scn.sap.com/community/hana-in-memory/blog/2015/05/19/sap-hana-system-replication)
- [SAP-notitie ondersteuning #1999880: veelgestelde vragen over SAP HANA System Replication](https://bcs.wdf.sap.corp/sap/support/notes/1999880)
- [SAP ondersteuning Opmerking #2165547 – SAP HANA Back-en herstel binnen replicatieomgeving voor SAP HANA-systeem](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3231363535343726)
- [SAP-notitie ondersteuning #1984882 – met SAP HANA System replicatie voor Hardware-uitwisseling met minimale/Zero Downtime](https://websmp230.sap-ag.de/sap(bD1lbiZjPTAwMQ==)/bc/bsp/sno/ui_entry/entry.htm?param=69765F6D6F64653D3030312669765F7361706E6F7465735F6E756D6265723D3139383438383226)

## <a name="network-considerations-for-disaster-recovery-with-hana-large-instances"></a>Overwegingen voor herstel na noodgevallen met grote exemplaren HANA netwerk

Om te profiteren van de functionaliteit herstel na noodgevallen van grote exemplaren HANA, moet u voor het ontwerpen van netwerkconnectiviteit voor twee verschillende Azure-regio's. U moet een Azure ExpressRoute-circuit-verbinding van on-premises in uw Azure-regio en een andere circuit verbinding on-premises voor uw regio herstel na noodgevallen. Deze meting bevat informatie over een situatie waarin er een probleem in een Azure-regio is, met inbegrip van een locatie van Microsoft Enterprise Edge Router (MSEE).

Als een tweede maat, kunt u alle virtuele Azure-netwerken die verbinding met de SAP HANA in Azure (grote exemplaren maken) in een van de regio's aan een ExpressRoute-circuit dat verbinding HANA grote exemplaren in andere regio maakt. Met deze *kruislings verbinding*, services die worden uitgevoerd op een virtuele Azure-netwerk in gebied #1, kunnen verbinding maken met grote exemplaar HANA eenheden per regio #2 en andersom. Deze meting heeft betrekking op een aanvraag waarin slechts één van de MSEE-locaties die verbinding met uw on-premises-locatie met Azure maakt offline gaat.

De volgende afbeelding ziet u de configuratie van een robuuste voor herstel na noodgevallen:

![Optimale configuratie voor herstel na noodgevallen](./media/hana-overview-high-availability-disaster-recovery/image1-optimal-configuration.png)



## <a name="other-requirements-when-you-use-hana-large-instances-storage-replication-for-disaster-recovery"></a>Andere vereisten wanneer u grote exemplaren HANA storage-replicatie voor herstel na noodgevallen gebruikt

Aanvullende vereisten voor de installatie van een herstel na noodgevallen met grote HANA-exemplaren zijn:

- U moet SAP HANA order niet uitvoeren op Azure (grote exemplaren)-SKU's van dezelfde grootte hebben als uw productie-SKU's en deze implementeren in de regio van het herstel na noodgevallen. In de huidige implementaties van klanten, worden deze exemplaren gebruikt voor het uitvoeren van niet-productieve HANA exemplaren. We verwijzen naar deze als *multipurpose DR-instellingen*.   
- Voor elk van uw SAP HANA op Azure (grote exemplaren)-SKU's die u wilt herstellen in de site voor herstel na noodgevallen, moet u extra opslagruimte op de site DR rangschikken. Extra opslagruimte kopen, kunt u de opslagvolumes toewijzen. U kunt de volumes die het doel van de storage-replicatie van uw productie-Azure-regio in de noodherstel Azure-regio zijn toewijzen.
 

## <a name="backup-and-restore"></a>Back-ups en herstellen

Een van de belangrijkste aspecten voor de operationele databases is zorg bescherming tegen verschillende rampen. De oorzaak van deze gebeurtenissen kan van alles van natuurramp eenvoudig fouten zijn.

Back-ups van een database met de mogelijkheid om het te herstellen naar een willekeurig punt in tijd (zoals voordat iemand kritieke gegevens verwijderd), kunt u herstellen naar een status die zo veel mogelijk lijken op de manier waarop het was voordat de onderbreking is.

Twee soorten back-ups moeten worden uitgevoerd voor de beste resultaten:

- Databaseback-ups: volledige en incrementele en differentiële back-ups
- Transactie-logboekback-ups

Naast de volledige-databaseback-ups uitgevoerd op het niveau van een toepassing, kunt u back-ups met de opslag-momentopnamen uitvoeren. Opslag-momentopnamen vervangen transactie logboekback-ups niet. Transactie-logboekback-ups blijven belangrijk de database herstellen naar een bepaald punt in tijd of de logboeken van al doorgevoerde transacties leegmaken. Opslag-momentopnamen kunnen echter herstel versnellen door snel een installatiekopie van het doorvoeren van de database. 

SAP HANA in Azure (grote exemplaren) biedt twee opties voor back-up en herstel:

- Hiervoor zelf (DIY). Nadat u hebt berekend om te controleren of dat er voldoende schijfruimte beschikbaar is, uitvoeren van de volledige database en logboekback-ups met behulp van de back-upmethoden schijf. U kunt back-up ofwel rechtstreeks op volumes die zijn gekoppeld aan de eenheden HANA grote exemplaar of naar Shares NFS (Network File) instellen in Azure een virtuele machine (VM). Azure Storage koppelen aan de virtuele machine en opslag via een geconfigureerde NFS-server in die virtuele machine wordt gedeeld in het laatste geval klanten instellen van een Linux VM in Azure. Als u een back-up op volumes die rechtstreeks aan HANA grote exemplaar eenheden koppelen, moet u kopiëren van de back-ups naar Azure storage-account (na het instellen van een Azure-virtuele machine die wordt geëxporteerd NFS-shares die zijn gebaseerd op Azure Storage). Of u kunt een Azure Backup-kluis of een Azure koude opslag gebruiken. 

   Een andere optie is het gebruik van een derde partij gegevens beveiligingshulpprogramma voor het opslaan van de back-ups nadat ze zijn gekopieerd naar een Azure storage-account. De optie voor ZELFOPLOSSING back-up kan ook zijn nodig voor gegevens die u nodig hebt om op te slaan voor langere tijd voor naleving en controledoeleinden. In alle gevallen worden de back-ups gekopieerd naar de NFS-shares vertegenwoordigd door een virtuele machine en de Azure Storage.

- Gebruik de back-up en herstel van functionaliteit die de onderliggende infrastructuur van SAP HANA in Azure (grote exemplaren) biedt. Deze optie wordt voldaan aan de noodzaak van back-ups en snelle herstelbewerkingen. De rest van deze sectie heeft betrekking op de back-up en herstel functionaliteit die wordt aangeboden met grote HANA-exemplaren. Deze sectie bevat ook de relatie back-up en herstel heeft voor herstel na noodgevallen functionaliteit die worden aangeboden door grote HANA-exemplaren.

> [!NOTE]
> De momentopnametechnologie die wordt gebruikt door de onderliggende infrastructuur van grote exemplaren HANA heeft een afhankelijkheid voor SAP HANA-momentopnamen. SAP HANA momentopnamen werken op dit moment niet in combinatie met meerdere tenants van containers voor SAP HANA-multitenant-database. Dus kan niet deze methode van back-up worden gebruikt bij het implementeren van meerdere tenants in containers voor SAP HANA-multitenant-database. Als er slechts één tenant wordt geïmplementeerd, werken SAP HANA-momentopnamen.

### <a name="using-storage-snapshots-of-sap-hana-on-azure-large-instances"></a>Met behulp van opslag-momentopnamen van SAP HANA in Azure (grote exemplaren)

De infrastructuur van de onderliggende SAP HANA in Azure (grote exemplaren) ondersteunt opslag van momentopnamen van volumes. Zowel back-up en herstel van volumes wordt ondersteund met de volgende overwegingen:

- In plaats van de volledige-databaseback-ups, worden opslag volume momentopnamen regelmatig.
- Wanneer een momentopname van een activering via hana/gegevens, hana/log en /hana/shared (inclusief /usr/sap) start volumes, de opslag momentopname een SAP HANA momentopname voordat deze de momentopname van de opslag wordt uitgevoerd. Deze momentopname met de SAP HANA is de installatie voor uiteindelijke logboek herstelbewerkingen kunnen na het herstel van de momentopname van de opslag.
- Na het punt waar de opslag-momentopnamen met succes is uitgevoerd, wordt de momentopname SAP HANA verwijderd.
- Transactie logboekback-ups regelmatig worden genomen en worden opgeslagen in het volume /hana/logbackups of in Azure. U kunt het /hana/logbackups volume met de transactie-logboekback-ups om een momentopname van een afzonderlijk te activeren. In dat geval hoeft u niet een momentopname HANA uitvoeren.
- Als u een database naar een bepaalde tijd herstellen moet, aanvraag Microsoft Azure-ondersteuning (voor een productie-storing) of een SAP HANA op Azure Service Management om te zetten op een bepaalde opslag-momentopnamen. Een voorbeeld is een geplande herstel van een sandbox naar de oorspronkelijke staat.
- De SAP HANA-momentopname die opgenomen in de opslag momentopname is een offset punt voor het toepassen van de transactie-logboekback-ups die zijn uitgevoerd en opgeslagen nadat de momentopname van de opslag is gemaakt.
- Deze transactie-logboekback-ups zijn genomen om de database herstellen naar een bepaald punt in tijd.

U kunt opslag-momentopnamen die gericht is op drie verschillende soorten volumes uitvoeren:

- Een momentopname van een gecombineerde via hana/gegevens en /hana/shared (inclusief/usr/sap). Deze momentopname is vereist voor het maken van een momentopname van een SAP HANA als voorbereiding voor de opslag-momentopnamen. De momentopname SAP HANA zorgt ervoor dat de database zich in een consistente status uit het oogpunt van een opslaggroep zijn.
- Een momentopname van een afzonderlijke via hana/logbackups.
- De partitie van een besturingssysteem (alleen voor Type I HANA grote exemplaren van).


### <a name="storage-snapshot-considerations"></a>Overwegingen met betrekking tot opslag-momentopnamen

>[!NOTE]
>Opslag-momentopnamen opslagruimte die is toegewezen aan de eenheden HANA grote exemplaar gebruiken. Daarom moet u rekening houden met de volgende aspecten van het plannen van opslag-momentopnamen en hoeveel opslag-momentopnamen te houden. 

Het specifieke mechanisme van opslag-momentopnamen voor SAP HANA in Azure (grote exemplaren) zijn onder andere:

- Een momentopname van een bepaalde opslag (op het punt in tijd waarop deze is gemaakt) verbruikt weinig opslag.
- Als gegevens inhoud wordt gewijzigd en de inhoud van bestanden op het opslagvolume wijzigen SAP HANA-gegevens, moet de momentopname voor het opslaan van de oorspronkelijke inhoud blokkeren, evenals de gewijzigde gegevens.
- Als gevolg hiervan de momentopname van de opslag wordt vergroot. Hoe langer de momentopname bestaat, hoe groter de momentopname van de opslag wordt.
- De meer wijzigingen die zijn aangebracht aan het volume SAP HANA-database gedurende de levensduur van een momentopname opslag, hoe groter het verbruik van de ruimte van de momentopname van de opslag.

SAP HANA in Azure (grote exemplaren) wordt geleverd met vaste volumes voor de volumes van gegevens en logboekbestanden SAP HANA. Uitvoeren van momentopnamen van de volumes eats in de ruimte op volume. U moet bepalen bij het plannen van opslag-momentopnamen. Ook moet u het verbruik van de ruimte van de opslagvolumes bewaken, evenals het aantal momentopnamen die u opslaat beheren. U kunt de opslag-momentopnamen uitschakelen wanneer u grote hoeveelheden gegevens te importeren of andere belangrijke wijzigingen aangebracht in de HANA-database uitvoeren. 


De volgende secties bevatten informatie voor het uitvoeren van deze momentopnamen, waaronder algemene aanbevelingen:

- Hoewel de hardware 255 momentopnamen per volume tolereren kan, wordt ten zeerste aanbevolen ruim onder dit nummer te blijven.
- Voordat u opslag-momentopnamen uitvoert, bewaken en bijhouden vrije ruimte.
- Verlaag het aantal momentopnamen van opslag op basis van de vrije ruimte. U kunt het aantal momentopnamen die u bijhoudt verlagen of u de volumes kunt uitbreiden. U kunt extra opslagruimte in 1 terabyte eenheden bestellen.
- Tijdens de activiteiten, zoals het verplaatsen van gegevens in SAP HANA met SAP platform hulpprogramma's voor migratie (R3load) of het SAP HANA-databases herstellen vanuit back-ups, opslag-momentopnamen op het volume /hana/data niet uitschakelen. 
- Tijdens de grotere reorganisaties SAP HANA-tabellen, moeten opslag-momentopnamen worden vermeden, indien mogelijk.
- Opslag-momentopnamen zijn vereist om te profiteren van de mogelijkheden voor herstel na noodgevallen van SAP HANA in Azure (grote exemplaren).

### <a name="setting-up-storage-snapshots"></a>Instellen van opslag-momentopnamen

De stappen voor het instellen van opslag-momentopnamen met grote HANA-exemplaren zijn als volgt:
1. Zorg ervoor dat Perl is geïnstalleerd op de Linux-besturingssysteem op de server HANA grote exemplaren.
2. Wijzig de/etc/ssh/ssh\_config voor toevoegen van de regel _MACs hmac-sha1_.
3. Maak een back-up SAP HANA-gebruikersaccount op het hoofdknooppunt voor elk SAP HANA-exemplaar dat u uitvoert, indien van toepassing.
4. De SAP HANA HDB-client installeren op alle servers in de grote SAP HANA-exemplaren.
5. Maak een openbare sleutel voor toegang tot de onderliggende opslaginfrastructuur die het maken van momentopnamen bepaalt op de eerste server voor SAP HANA grote exemplaren van elke regio.
6. Kopieer de scripts en het configuratiebestand van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts) naar de locatie van **hdbsql** in de SAP HANA-installatie.
7. Het wijzigen van het bestand HANABackupDetails.txt die nodig zijn voor de gewenste klant-specificaties.

### <a name="step-1-install-the-sap-hana-hdb-client"></a>Stap 1: De SAP HANA HDB-client installeren

Het Linux-besturingssysteem geïnstalleerd op een SAP HANA in Azure (grote exemplaren) omvat de mappen en scripts nodig zijn voor het uitvoeren van SAP HANA-opslag-momentopnamen voor back-up en herstel na noodgevallen. Controleer op recentere versies in [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). De meest recente versie van de scripts is 2.1.
Het is echter zelf verantwoordelijk voor de SAP HANA HDB-client installeren op de eenheden HANA grote exemplaar tijdens de installatie van SAP HANA. (Microsoft installeert niet de HDB client of SAP HANA.)

### <a name="step-2-change-the-etcsshsshconfig"></a>Stap 2: Wijzig/etc/ssh/ssh\_config

Wijziging `/etc/ssh/ssh_config` door toe te voegen de _MACs hmac-sha1_ regel als volgt te werk:
```
#   RhostsRSAAuthentication no
#   RSAAuthentication yes
#   PasswordAuthentication yes
#   HostbasedAuthentication no
#   GSSAPIAuthentication no
#   GSSAPIDelegateCredentials no
#   GSSAPIKeyExchange no
#   GSSAPITrustDNS no
#   BatchMode no
#   CheckHostIP yes
#   AddressFamily any
#   ConnectTimeout 0
#   StrictHostKeyChecking ask
#   IdentityFile ~/.ssh/identity
#   IdentityFile ~/.ssh/id_rsa
#   IdentityFile ~/.ssh/id_dsa
#   Port 22
Protocol 2
#   Cipher 3des
#   Ciphers aes128-ctr,aes192-ctr,aes256-ctr,arcfour256,arcfour128,aes128-cbc,3des-cbc
#   MACs hmac-md5,hmac-sha1,umac-64@openssh.com,hmac-ripemd160
MACs hmac-sha1
#   EscapeChar ~
#   Tunnel no
#   TunnelDevice any:any
#   PermitLocalCommand no
#   VisualHostKey no
#   ProxyCommand ssh -q -W %h:%p gateway.example.com
```

### <a name="step-3-create-a-public-key"></a>Stap 3: Maak een openbare sleutel

Voor toegang tot de opslag momentopname-interfaces van uw tenant HANA grote exemplaar, moet u een aanmeldingspagina tot stand kunnen brengen via een openbare sleutel. Maak een openbare sleutel moet worden gebruikt voor toegang tot de infrastructuur voor opslag zodat u kunt momentopnamen maken op de eerste SAP HANA op Azure (grote exemplaren)-server in uw tenant. De openbare sleutel zorgt ervoor dat een wachtwoord niet aan te melden bij de interfaces van de opslag momentopname is vereist. Maken van een openbare sleutel betekent ook dat u niet wilt onderhouden wachtwoord opgeven. Voer de volgende opdracht om de openbare sleutel te genereren in Linux op de server voor SAP HANA grote exemplaren:
```
  ssh-keygen –t dsa –b 1024
```
De nieuwe locatie is **_/root/.ssh/id\_dsa.pub**. Voer een wachtwoord zelf niet, anders moet u het wachtwoord invoeren telkens wanneer die u zich aanmeldt. Selecteer in plaats daarvan **Enter** twee keer te verwijderen van de vereiste 'wachtwoord invoeren' voor het aanmelden.

Controleer of de openbare sleutel is opgelost, zoals werd verwacht in mappen die u wilt wijzigen **/root/.ssh/** en vervolgens uitvoert de `ls` opdracht. Als de sleutel aanwezig is, kunt u deze kunt kopiëren door de volgende opdracht uit te voeren:

![De openbare sleutel wordt gekopieerd met deze opdracht uit te voeren](./media/hana-overview-high-availability-disaster-recovery/image2-public-key.png)

Op dit moment Neem contact op met de SAP HANA op Azure-servicebeheer en hen voorzien van de openbare sleutel. De klantenservice maakt gebruik van de openbare sleutel registreren in de onderliggende opslaginfrastructuur die voor uw tenant HANA grote exemplaar is gehaald.

### <a name="step-4-create-an-sap-hana-user-account"></a>Stap 4: Een SAP HANA-gebruikersaccount maken

Voor het initiëren van het maken van momentopnamen voor SAP HANA, moet u een gebruikersaccount in SAP HANA waarmee de opslag momentopname scripts kunt maken. Een SAP HANA-gebruikersaccount in SAP HANA Studio voor dit doel maken. Dit account moet hebben de volgende bevoegdheden: **back-up Admin** en **catalogus**. In dit voorbeeld wordt de gebruikersnaam is **SCADMIN**. Naam van de gebruikersaccount gemaakt in HANA Studio is hoofdlettergevoelig. Zorg ervoor dat u selecteert **Nee** voor het vereisen van de gebruiker het wachtwoord bij de volgende aanmelding wijzigen.

![Maken van een gebruiker in HANA Studio](./media/hana-overview-high-availability-disaster-recovery/image3-creating-user.png)

### <a name="step-5-authorize-the-sap-hana-user-account"></a>Stap 5: Machtig de SAP HANA-gebruikersaccount

In deze stap maakt autoriseren u het SAP HANA-gebruikersaccount dat u hebt gemaakt, zodat de scripts niet hoeft in te dienen wachtwoorden tijdens runtime. De opdracht SAP HANA `hdbuserstore` wordt het maken van een sleutel voor SAP HANA, die is opgeslagen op een of meer SAP HANA-knooppunten. De sleutel van de gebruiker kunt de gebruikerstoegang SAP HANA zonder het beheren van wachtwoorden op in het proces voor het uitvoeren van scripts. Het uitvoeren van scripts proces wordt later besproken.

>[!IMPORTANT]
>Voer de volgende opdracht als `root`. Anders wordt kan niet het script correct werken.

Voer de `hdbuserstore` opdracht als volgt:

**Voor de installatie niet MDC HANA**
```
hdbuserstore set <key> <host>:<3[instance]15> <user> <password>
```

**Voor MDC HANA setup**
```
hdbuserstore set <key> <host>:<3[instance]13> <user> <password>
```

In het volgende voorbeeld wordt de gebruiker is **SCADMIN01**, is de hostnaam **lhanad01**, en het exemplaarnummer **01**:
```
hdbuserstore set SCADMIN01 lhanad01:30115 <backup username> <password>
```
Als u een scale-out SAP HANA configuratie hebt, kunt u alle scripts van één server moet beheren. In dit voorbeeld wordt de sleutel voor SAP HANA **SCADMIN01** voor elke host op een manier die aangeeft welke host is gerelateerd aan de sleutel moet worden gewijzigd. De back-up SAP HANA-account met het exemplaarnummer van de HANA-database te wijzigen. De sleutel moet beheerdersbevoegdheden hebben op de host die is toegewezen en de back-gebruiker voor configuraties van scale-out moet beschikken over rechten voor alle exemplaren van de SAP HANA. De namen ervan uitgaande dat de drie knooppunten van de scale-out hebben **lhanad01**, **lhanad02**, en **lhanad03**, de reeks opdrachten uitziet:

```
hdbuserstore set SCADMIN01 lhanad01:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad02:30115 SCADMIN <password>
hdbuserstore set SCADMIN01 lhanad03:30115 SCADMIN <password>
```

### <a name="step-6-get-the-snapshot-scripts-configure-the-snapshots-and-test-the-configuration-and-connectivity"></a>Stap 6: Ophalen van de momentopname-scripts, de momentopnamen te configureren en testen van de configuratie en connectiviteit

Download de meest recente versie van de scripts van [GitHub](https://github.com/Azure/hana-large-instances-self-service-scripts). Kopieer de gedownloade scripts en het tekstbestand dat naar de werkmap voor **hdbsql**. Voor installaties van de huidige HANA deze map is vergelijkbaar met /hana/shared/D01/exe/linuxx86\_64/hdb. 
``` 
azure_hana_backup.pl 
azure_hana_replication_status.pl 
azure_hana_snapshot_details.pl 
azure_hana_snapshot_delete.pl 
testHANAConnection.pl 
testStorageSnapshotConnection.pl 
removeTestStorageSnapshot.pl 
HANABackupCustomerDetails.txt 
``` 


Dit is het doel van de andere scripts en bestanden:

- **Azure\_hana\_backup.pl**: dit script met cron voor het uitvoeren van opslag-momentopnamen op de HANA gegevens/log/gedeelde clustervolumes, het volume/hana/logbackups of het besturingssysteem (op het Type I-SKU's van HANA grote exemplaren) plannen.
- **Azure\_hana\_replicatie\_status.pl**: dit script geeft de algemene informatie over de replicatiestatus van de productiesite naar de site voor herstel na noodgevallen. De monitoren script om ervoor te zorgen dat de replicatie plaatsvindt en de grootte van de items die wordt worden gerepliceerd. Het bevat ook richtlijnen als een replicatie duurt te lang is of als de koppeling niet actief is.
- **Azure\_hana\_momentopname\_details.pl**: dit script geeft een lijst van algemene informatie over alle momentopnamen, per volume, die aanwezig zijn in uw omgeving. Dit script kan worden uitgevoerd op de primaire server of op een server-eenheid in de locatie van het herstel na noodgevallen. Het script bevat de volgende informatie onderverdeeld op basis van elk volume dat momentopnamen bevat:
   * Grootte van het totale aantal momentopnamen in een volume
   * Elke momentopname in dat volume bevat de volgende details: 
      - Naam van de momentopname 
      - Het maken van 
      - Grootte van de momentopname
      - Frequentie van de momentopname
      - HANA back-up-ID die is gekoppeld aan de momentopname, indien van toepassing
- **Azure\_hana\_momentopname\_delete.pl**: dit script verwijdert u een momentopname van de opslag of een set van momentopnamen. U kunt de back-up SAP HANA-ID zoals gevonden in HANA Studio of de naam van de momentopname. De back-up-ID is op dit moment wordt alleen gekoppeld aan de momentopnamen die zijn gemaakt voor de HANA gegevens/log/gedeelde clustervolumes. Anders, als de momentopname-ID is ingevoerd, gericht alle momentopnamen die overeenkomen met de opgegeven momentopname-ID.  
- **testHANAConnection.pl**: dit script test de verbinding met de SAP HANA-exemplaar en is vereist voor het instellen van de opslag-momentopnamen.
- **testStorageSnapshotConnection.pl**: dit script heeft twee doeleinden. Eerst het zorgt ervoor dat de grote exemplaar HANA-eenheid die wordt uitgevoerd de scripts toegang tot de toegewezen opslag virtuele machine en de opslaginterface momentopname van de grote HANA-exemplaren heeft. Het tweede doel is een tijdelijke momentopname maken voor het HANA-exemplaar dat u wilt testen. Dit script moet worden uitgevoerd voor elke instantie HANA op een server om ervoor te zorgen dat de back-scripts werkt zoals verwacht.
- **removeTestStorageSnapshot.pl**: dit script wordt verwijderd van de momentopname zoals gemaakt met het script test **testStorageSnapshotConnection.pl**. 
- **HANABackupCustomerDetails.txt**: dit bestand is een bewerkbaar configuratiebestand dat u wilt wijzigen om aan te passen aan uw SAP HANA-configuratie.

 
Het bestand HANABackupCustomerDetails.txt is het besturingselement en configuratie-bestand voor het script dat wordt uitgevoerd van de opslag-momentopnamen. Het bestand voor uw toepassing en de instellingen aanpassen. U hebt ontvangen de **back-up Opslagnaam** en **opslag IP-adres** uit SAP HANA op Azure-servicebeheer wanneer uw exemplaren zijn geïmplementeerd. U kunt de volgorde niet wijzigen, rangschikken of afstand van een van de variabelen in dit bestand. Anders gaat de scripts niet correct kunnen worden uitgevoerd. Bovendien ontvangen u het IP-adres van het knooppunt omhoog schalen of het hoofdknooppunt (als scale-out) van SAP HANA op Azure Service Management. U ook kennen het exemplaarnummer HANA die u hebt verkregen tijdens de installatie van de SAP HANA. Nu moet u een back-naam toevoegen aan het configuratiebestand.

Voor een scale-up of scale-out-implementatie, zou het configuratiebestand eruit het volgende voorbeeld nadat u de naam van de back-up en het IP-adres van opslag ingevuld. U moet ook de volgende gegevens in het configuratiebestand invullen:
- Één knooppunt of hoofdknooppunt IP-adres
- HANA exemplaarnummer
- De naam van de back-up 
    
```
#Provided by Microsoft Service Management
Storage Backup Name: client1hm3backup
Storage IP Address: 10.240.20.31
#Node IP addresses, instance numbers, and HANA backup name
#provided by customer.  HANA backup name created using
#hdbuserstore utility.
Node 1 IP Address: 
Node 1 HANA instance number:
Node 1 HANA userstore Name:
```

>[!NOTE]
>Op dit moment worden alleen gegevens voor knooppunt 1 in het werkelijke HANA opslag momentopname script gebruikt. Het is raadzaam de toegang tot of van alle HANA knooppunten te testen, zodat als de back-hoofdknooppunt ooit wijzigt, al u zorgt dat een ander knooppunt kan worden uitgevoerd zijn door het wijzigen van de details in het knooppunt 1.

Nadat u de configuratiegegevens in het bestand HANABackupCustomerDetails.txt zet, moet u controleren of de configuraties met betrekking tot de instantiegegevens HANA juist zijn. Gebruik het script `testHANAConnection.pl`. Dit script is onafhankelijk van een SAP HANA-configuratie voor scale-up of scale-out.

```
testHANAConnection.pl
```

Als u een scale-out SAP HANA configuratie hebt, zorg ervoor dat het master HANA exemplaar toegang tot de vereiste HANA servers en -exemplaren heeft. Er zijn geen parameters aan het testscript, maar u moet uw gegevens toevoegen aan het configuratiebestand HANABackupCustomerDetails.txt voor het script correct kunnen worden uitgevoerd. Alleen de shell-opdracht-foutcodes worden geretourneerd, zodat het is niet mogelijk voor het script op fout in elk exemplaar. Het script biedt, zodat sommige nuttige opmerkingen voor u om te controleren.

Voer het script wordt uitgevoerd, de volgende opdracht:
```
 ./testHANAConnection.pl
```
Als het script is de status van het exemplaar HANA krijgt, wordt er een bericht weergegeven dat de HANA-verbinding geslaagd is.


De volgende stap van de test is Controleer de verbinding met de opslag op basis van de gegevens die u in het configuratiebestand HANABackupCustomerDetails.txt plaatsen en vervolgens een momentopname van de test wordt uitgevoerd. Voordat u uitvoert het `azure_hana_backup.pl` script, moet u deze test uitvoeren. Als een volume geen momentopnamen bevat, is het onmogelijk om te bepalen of het volume leeg is of als er een SSH-fout is voor de details van de momentopname. Daarom wordt het script wordt uitgevoerd twee stappen:

- Er wordt gecontroleerd dat de opslag virtuele machine en de interfaces van de tenant toegankelijk is voor de scripts uit te voeren momentopnamen zijn.
- Een test- of pop, momentopname voor elk volume wordt gemaakt door HANA-exemplaar.

Daarom is het exemplaar HANA als argument. Als de uitvoering is mislukt, is het niet mogelijk voor het leveren van controleren voor de opslagverbinding. Zelfs als er geen foutcontrole, biedt het script nuttige tips.

Als wordt het script uitgevoerd:
```
 ./testStorageSnapshotConnection.pl <HANA SID>
```
Vervolgens wordt het script probeert aan te melden bij de opslag met behulp van de openbare sleutel die is opgegeven in de vorige stappen voor het installatieprogramma en met de gegevens die zijn geconfigureerd in het bestand HANABackupCustomerDetails.txt. Als de aanmelding is geslaagd, ziet u de volgende inhoud:

```
**********************Checking access to Storage**********************
Storage Access successful!!!!!!!!!!!!!!
```

Als er problemen optreden verbinden met de opslag-console, wordt de uitvoer ziet er als volgt:

```
**********************Checking access to Storage**********************
WARNING: Storage check status command 'volume show -type RW -fields volume' failed: 65280
WARNING: Please check the following:
WARNING: Was publickey sent to Microsoft Service Team?
WARNING: If passphrase entered while using tool, publickey must be re-created and passphrase must be left blank for both entries
WARNING: Ensure correct IP address was entered in HANABackupCustomerDetails.txt
WARNING: Ensure correct Storage backup name was entered in HANABackupCustomerDetails.txt
WARNING: Ensure that no modification in format HANABackupCustomerDetails.txt like additional lines, line numbers or spacing
WARNING: ******************Exiting Script*******************************
```

Na een geslaagde aanmelding bij de interfaces van de virtuele machine opslag, het script verder met de fase #2 en maakt een momentopname van de test. Voor een configuratie van de drie knooppunten scale-out van SAP HANA wordt hier de uitvoer weergegeven:

```
**********************Creating Storage snapshot**********************
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_data_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_dp ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_backups_hm3_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00001_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00002_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_log_hm3_mnt00003_t020_vol ...
Snapshot created successfully.
Taking snapshot testStorage.recent for hana_shared_hm3_t020_vol ...
Snapshot created successfully.
```

Als de momentopname van de test heeft met het script met succes is uitgevoerd, kunt u doorgaan met het configureren van de werkelijke opslag-momentopnamen. Als dit niet lukt, kunt u de problemen onderzoeken voordat u verder gaat. De momentopname van de test moet rond blijven totdat de eerste echte momentopnamen klaar bent.


### <a name="step-7-perform-snapshots"></a>Stap 7: Momentopnamen uitvoeren

Als de voorbereidende stappen zijn voltooid, kunt u starten voor het configureren van de configuratie van de werkelijke opslag-momentopnamen. Het script moet worden gepland werkt met SAP HANA scale-up en scale-out-configuraties. U moet de uitvoering van scripts via cron plannen. 

Drie soorten momentopnameback-ups kunnen worden gemaakt:
- **HANA**: momentopnameback-up waarin de volumes die hana/gegevens bevatten en hana/gedeeld (met daarin ook /usr/sap) de gecoördineerde momentopname vallen gecombineerd. Een enkel bestand terugzetten is vanuit deze momentopname mogelijk.
- **Logboeken**: momentopname back-up van het volume/hana/logbackups. Er is geen momentopname HANA wordt geactiveerd voor het uitvoeren van deze momentopname met de opslag. Deze opslagvolume is het volume dat is bedoeld om de back-ups van SAP HANA-transactielogboek bevatten. SAP HANA-transactielogboek back-ups worden vaker uitgevoerd om te beperken logboek groei en mogelijk gegevensverlies te voorkomen. Een enkel bestand terugzetten is vanuit deze momentopname mogelijk. U moet de frequentie op onder de vijf minuten niet verlagen.
- **Opstarten**: momentopname van het volume met het opstarten logische eenheidnummer (LUN) van het grote HANA-exemplaar. Deze momentopname back-up kan alleen met het Type I-SKU's van HANA grote exemplaren. U kan geen enkel bestand herstelacties uitvoeren vanaf de momentopname van het volume met het opstarten van LUN. Voor het Type II SKU's van HANA grote exemplaren, kunt u het niveau van de OS back-up, en ook de afzonderlijke bestanden terugzetten. Raadpleeg het document '[hoe OS back-up voor Type II SKU's voeren](os-backup-type-ii-skus.md)' voor meer informatie.


De syntaxis van de aanroep voor deze drie verschillende soorten momentopnamen ziet er als volgt:
```
HANA backup covering /hana/data and /hana/shared (includes/usr/sap)
./azure_hana_backup.pl hana <HANA SID> manual 30

For /hana/logbackups snapshot
./azure_hana_backup.pl logs <HANA SID> manual 30

For snapshot of the volume storing the boot LUN
./azure_hana_backup.pl boot none manual 30

```

De volgende parameters moeten worden opgegeven:

- De eerste parameter geeft het type van de momentopname back-up. De toegestane waarden zijn **hana**, **logboeken**, en **boot**. 
- De tweede parameter is **HANA SID** (zoals HM3) of **geen**. Als de eerste waarde van de parameters hebt opgegeven is **hana** of **logboeken**, dan is de waarde van deze parameter **HANA SID** (zoals HM3), anders voor opstarten volume back-up, de waarde is **geen**. 
- De derde parameter is geen momentopname of een back-label voor het type van de momentopname. Heeft twee doeleinden. Het doel een voor u is hieraan een naam, zodat u wat deze momentopnamen zijn weet over. Het tweede doel is voor de azure script\_hana\_backup.pl om te bepalen het aantal momentopnamen van opslag die onder die specifiek label worden bewaard. Als u twee opslag momentopname back-ups plannen van hetzelfde type (zoals **hana**), met twee verschillende labels, en dat 30 momentopnamen voor elke moeten blijven, gaat u uiteindelijk eindigen met 60 opslag-momentopnamen van de volumes die van invloed op een definiëren. 
- De vierde parameter definieert de retentie van de momentopnamen indirect met het definiëren van het aantal momentopnamen van met hetzelfde momentopname voorvoegsel (label) moeten worden bewaard. Deze parameter is belangrijk voor een geplande uitvoering via cron. 

In het geval van een scale-out komt het script een aantal extra controles om ervoor te zorgen dat u toegang hebt tot alle HANA-servers. Het script controleert ook dat alle HANA exemplaren de juiste status van de exemplaren retourneren voordat een SAP HANA-momentopname wordt gemaakt. De SAP HANA-momentopname wordt gevolgd door een opslag-momentopnamen.

De uitvoering van het script `azure_hana_backup.pl` maakt de opslag momentopname in de volgende drie afzonderlijke fasen:

1. Een SAP HANA-momentopname worden uitgevoerd
2. Een momentopname van de opslag wordt uitgevoerd
3. Hiermee verwijdert u de SAP HANA-momentopname die is gemaakt voordat de uitvoering van de momentopname van de opslag

Voor het uitvoeren van het script, aanroepen u deze vanuit de HDB uitvoerbare map op die is gekopieerd. 

De bewaarperiode wordt beheerd met het aantal momentopnamen die tijdens het uitvoeren van het script als parameter worden verzonden (zoals **30**, eerder weergegeven). Ja, de hoeveelheid tijd die wordt gedekt door de opslag-momentopnamen is een functie van twee dingen: de periode van uitvoering en het aantal momentopnamen verzonden als parameter bij het uitvoeren van het script. Als het aantal momentopnamen worden bewaard hoger is dan het getal dat als een parameter in de aanroep van het script de oudste opslag momentopname van het label met dezelfde naam (in ons geval vorige **handmatige**) is verwijderd voordat een nieuwe momentopname wordt uitgevoerd. Het getal geeft u de laatste parameter van de aanroep is het aantal kunt u bepalen het aantal momentopnamen worden bewaard. Met dit nummer, kunt u ook bepalen, indirect, de beschikbare schijfruimte voor momentopnamen. 

> [!NOTE]
>Als u het label wijzigt, wordt de telling opnieuw gestart. Dit betekent dat u wilt worden strikte in labels zodat de momentopnamen niet per ongeluk worden verwijderd.

### <a name="snapshot-strategies"></a>Momentopname strategieën
De frequentie van momentopnamen voor de verschillende typen, is afhankelijk van of u de functionaliteit van grote exemplaar HANA herstel na noodgevallen of niet gebruiken. De functionaliteit herstel na noodgevallen van grote exemplaren HANA afhankelijk van de opslag-momentopnamen. Vertrouwen op de opslag-momentopnamen mogelijk enkele speciale aanbevelingen in termen van de frequentie en uitvoering perioden van de opslag-momentopnamen. 

In de overwegingen en aanbevelingen die volgen, gaan we ervan uit dat u doen *niet* gebruik de functie voor herstel na noodgevallen HANA grote exemplaren biedt. In plaats daarvan gebruikt u de opslag-momentopnamen als een manier om back-ups hebben en herstel van de punt in tijd voor de afgelopen 30 dagen opgeven. Gezien de beperkingen van het aantal momentopnamen en ruimte, klanten hebben beschouwd als de volgende vereisten:

- De hersteltijd voor herstel van de punt in tijd.
- De ruimte die wordt gebruikt.
- Het herstelpunt doel en de beoogde hersteltijd op mogelijk noodherstel.
- De uiteindelijke uitvoering van HANA full-back-ups op basis van schijven. Wanneer een volledige-databaseback-up op basis van schijven of de **backint** interface wordt uitgevoerd, mislukt de uitvoering van de opslag-momentopnamen. Als u van plan bent om uit te voeren van volledige-databaseback-ups boven op de opslag-momentopnamen, zorg ervoor dat de uitvoering van de opslag-momentopnamen gedurende deze tijd is uitgeschakeld.
- Het aantal momentopnamen per volume is beperkt tot 255.


De momentopname-periode is voor klanten die de functionaliteit herstel na noodgevallen van HANA grote exemplaren niet gebruikt, minder frequente. In dergelijke gevallen zien we klanten de gecombineerde momentopnamen op /hana/data en /hana/shared (inclusief /usr/sap) uitvoeren in perioden van 12 uur of 24-uurs en ze de momentopnamen ten aanzien van een hele maand houden. Hetzelfde geldt voor de momentopnamen van de back-logboekvolume. De uitvoering van een SAP HANA-transactielogboek back-ups op basis van de back-logboekvolume gebeurt echter in de 5 minuten tot 15 minuten perioden.

We raden u aan het uitvoeren van geplande opslag-momentopnamen via cron. Ook wordt aangeraden dat u hetzelfde script voor alle back-ups gebruiken en herstel na noodgevallen. U moet het script wijzigen invoerwaarden die overeenkomen met de verschillende back-up keren aangevraagd. Deze momentopnamen worden alle anders gepland in cron, afhankelijk van hun uitvoeringstijd: elk uur, 12 uur, dagelijks of wekelijks. 

Een voorbeeld van een planning cron in /etc/crontab uitzien als volgt:
```
00 1-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 46
10 00 * * *  ./azure_hana_backup.pl hana HM3 dailyhana 28
00,05,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
22 12 * * *  ./azure_hana_backup.pl log HM3 dailylogback 28
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```
In het vorige voorbeeld is een momentopname van een uur gecombineerde die betrekking heeft op de volumes die de gegevens hana/bevatten en /hana/shared (inclusief/usr/sap) locaties. Dit type momentopname zou worden gebruikt voor een punt in tijd sneller herstel in de afgelopen twee dagen. Er is bovendien een dagelijkse momentopname op deze volumes. Dus hebt u twee dagen van dekking door elk uur momentopnamen, plus vier weken van dekking door dagelijkse momentopnamen. Bovendien wordt het logboek voor databasetransacties back-volume back-up eenmaal voor elke dag. Deze back-ups zijn ook vier weken bewaard. Zoals u in de derde regel van crontab ziet, wordt de back-up van een transactielogboek HANA gepland om uit te voeren om de vijf minuten. De minuten start van de verschillende cron-taken die voor het uitvoeren van opslag-momentopnamen gespreid, zodat deze momentopnamen worden niet uitgevoerd in één keer op een bepaald punt in tijd. 

In het volgende voorbeeld voert u een momentopname van een gecombineerde die betrekking heeft op de volumes waar de hana/data/hana/gedeeld (inclusief/usr/sap) locaties en op uurbasis. U bewaren deze momentopnamen voor twee dagen. De momentopnamen van de back-upvolumes transactielogboek worden uitgevoerd op basis van de vijf minuten en van vier uur worden bewaard. Als is voordat u, de back-up van het transactielogbestand HANA gepland om uit te voeren om de vijf minuten. De momentopname van het logboek voor databasetransacties back-volume wordt uitgevoerd met een vertraging van twee minuten nadat de transactielogboek back-up is gestart. Binnen deze twee minuten voltooid de SAP HANA-transactielogboek back-up onder normale omstandigheden. Als voordat, het volume met het opstarten van LUN maar één keer per dag wordt ondersteund door een opslag-momentopnamen en vier weken wordt bewaard.

```
10 0-23 * * * ./azure_hana_backup.pl hana HM3 hourlyhana 48
0,5,10,15,20,25,30,35,40,45,50,55 * * * *  Perform SAP HANA transaction log backup
2,7,12,17,22,27,32,37,42,47,52,57 * * * *  ./azure_hana_backup.pl log HM3 logback 48
30 00 * * *  ./azure_hana_backup.pl boot dailyboot 28
```

De volgende afbeelding ziet u de volgorde van het vorige voorbeeld, met uitzondering van het bestand Boot.ini LUN:

![Relatie tussen de back-ups en momentopnamen](./media/hana-overview-high-availability-disaster-recovery/backup_snapshot_updated0921.PNG)

SAP HANA voert reguliere het volume /hana/log om vast te leggen van de wijzigingen doorgevoerd in de database te schrijven. Regelmatig een schrijft SAP HANA een opslagpunt naar het volume /hana/data. Zoals opgegeven in crontab, wordt een SAP HANA-transactielogboek back-up om de vijf minuten uitgevoerd. U ziet ook dat de momentopname van een SAP HANA elk uur als gevolg van een momentopname van een gecombineerde opslag via de volumes /hana/data en /hana/shared activering wordt uitgevoerd. Nadat de momentopname HANA is geslaagd, wordt de momentopname van de gecombineerde opslag wordt uitgevoerd. Volgens de instructies in crontab, wordt de momentopname van de opslag op het volume /hana/logbackup uitgevoerd elke vijf minuten, ongeveer twee minuten na de back-up transactielogboek HANA.


>[!IMPORTANT]
> Het gebruik van opslag-momentopnamen voor SAP HANA back-ups is het nuttigst alleen wanneer de momentopnamen die in combinatie met SAP HANA-transactielogboek back-ups worden uitgevoerd. Deze transactie-logboekback-ups moeten kunnen omvatten de tijd tussen de opslag-momentopnamen. 

Als u een toezegging voor gebruikers van een punt in tijd herstel van 30 dagen hebt ingesteld, doet het volgende:

- In uitzonderlijke gevallen moet u toegang hebt tot een gecombineerde momentopname over/hana/opslaggegevens en /hana/shared die 30 dagen.
- Aaneengesloten transactielogboek back-ups die betrekking hebben op de tijd tussen een van de gecombineerde opslag-momentopnamen hebben. Ja, de oudste momentopname van het logboek voor databasetransacties back-volume moet 30 dagen oud. Dit is niet het geval als u de transactie logboekback-ups kopiëren naar een andere NFS-share bevindt zich op Azure-opslag. In dat geval kunt u oude transactielogboek back-ups van die NFS-share pull.

Als u wilt profiteren van opslag-momentopnamen en de uiteindelijke opslagreplicatie van transactie logboekback-ups, moet u de locatie die de SAP HANA de transactie-logboekback-ups te schrijft wijzigen. U kunt deze wijziging aanbrengt in HANA Studio. Hoewel SAP HANA back-ups volledig logboek segmenten automatisch maakt, moet u een back-logboekinterval om te worden deterministische opgeven. Dit geldt vooral als u de optie herstel na noodgevallen gebruikt omdat u meestal wilt uitvoeren logboekback-ups met een deterministische periode. In het volgende geval duurde we 15 minuten als het logboek-back-interval.

![Back-uplogboeken SAP HANA in SAP HANA Studio plannen](./media/hana-overview-high-availability-disaster-recovery/image5-schedule-backup.png)

U kunt back-ups die vaker plaatsvinden dan om de 15 minuten zijn. Dit wordt vaak gedaan in combinatie met herstel na noodgevallen. Sommige klanten uitvoeren transactie logboekback-ups om de vijf minuten.  

Als de database is nooit een reservekopie is gemaakt, is de laatste stap naar een bestand gebaseerde database back-up voor het maken van een afzonderlijke back-post moet bestaan binnen de back-catalogus. SAP HANA kan anders uw opgegeven logboekback-ups niet starten.

![Maken van een back-ups maken van een afzonderlijke back-post](./media/hana-overview-high-availability-disaster-recovery/image6-make-backup.png)


Nadat de eerste geslaagde opslag-momentopnamen zijn uitgevoerd, kunt u ook de test-momentopname die is uitgevoerd in stap 6 verwijderen. Voer het script uit om dit te doen `removeTestStorageSnapshot.pl`:
```
./removeTestStorageSnapshot.pl <hana instance>
```

### <a name="monitoring-the-number-and-size-of-snapshots-on-the-disk-volume"></a>Het aantal en de grootte van momentopnamen op het schijfvolume bewaking

U kunt het aantal momentopnamen en het opslagverbruik van deze momentopnamen bewaken op een bepaalde opslagvolume. De `ls` opdracht de momentopname-map of bestanden wordt niet weergegeven. Echter, de opdracht Linux-besturingssysteem `du` bevat gedetailleerde informatie over deze opslag-momentopnamen omdat ze zijn opgeslagen op de dezelfde volumes. De opdracht kan worden gebruikt met de volgende opties:

- `du –sh .snapshot`: Bevat een totaal van alle momentopnamen binnen de momentopname-directory.
- `du –sh --max-depth=1`: Geeft een lijst van de momentopnamen die zijn opgeslagen in de **.snapshot** map en de grootte van elke momentopname.
- `du –hc`: Bevat de totale grootte, die wordt gebruikt door alle momentopnamen.

Deze opdrachten gebruiken om ervoor te zorgen dat de momentopnamen die zijn gemaakt en opgeslagen geen de opslag op de volumes worden verbruikt.

>[!NOTE]
>De momentopnamen van de opstartinstallatiekopie LUN zijn niet zichtbaar is met de vorige opdrachten.

### <a name="getting-details-of-snapshots"></a>Details van momentopnamen ophalen
Voor meer informatie over momentopnamen, kunt u ook het script gebruiken `azure_hana_snapshot_details.pl`. Dit script kan worden uitgevoerd op een locatie als er een actieve server in de locatie van het herstel na noodgevallen. Het script bevat de volgende uitvoer, onderverdeeld op basis van elk volume dat momentopnamen bevat: 
   * Grootte van het totale aantal momentopnamen in een volume
   * Elke momentopname in dat volume bevat de volgende details: 
      - Naam van de momentopname 
      - Het maken van 
      - Grootte van de momentopname
      - Frequentie van de momentopname
      - HANA back-up-ID die is gekoppeld aan de momentopname, indien van toepassing

De syntaxis van de uitvoering van het script ziet er als volgt:

```
./azure_hana_snapshot_details.pl 
```

Omdat het script probeert op te halen van de back-up HANA-ID, moet het verbinding maken met de SAP HANA-exemplaar. Deze verbinding vereist het configuratiebestand HANABackupCustomerDetails.txt correct zijn ingesteld. Uitvoer van de twee momentopnamen op een volume uitzien als volgt:

```
**********************************************************
****Volume: hana_shared_SAPTSTHDB100_t020_vol       ***********
**********************************************************
Total Snapshot Size:  411.8MB
----------------------------------------------------------
Snapshot:   customer.2016-09-20_1404.0
Create Time:   "Tue Sep 20 18:08:35 2016"
Size:   2.10MB
Frequency:   customer 
HANA Backup ID:   
----------------------------------------------------------
Snapshot:   customer2.2016-09-20_1532.0
Create Time:   "Tue Sep 20 19:36:21 2016"
Size:   2.37MB
Frequency:   customer2
HANA Backup ID:   
```


### <a name="file-level-restore-from-a-storage-snapshot"></a>Bestandsniveau terugzetten vanuit een momentopname van de opslag
Voor de momentopname typen hana en de logboeken, zijn de toegang kunnen krijgen tot de momentopnamen die rechtstreeks op de volumes in de **.snapshot** directory. Er is een submap voor elk van de momentopnamen. U moet mogelijk voor het kopiëren van elk bestand dat wordt gedekt door de momentopname in de status op het moment van de momentopname van een submap in de werkelijke mapstructuur was.

>[!NOTE]
>Enkel bestand werkt terugzetten niet voor momentopnamen van de opstartinstallatiekopie LUN. De **.snapshot** directory niet beschikbaar is in het bestand Boot.ini LUN. 


### <a name="reducing-the-number-of-snapshots-on-a-server"></a>Het aantal momentopnamen op een server verminderen

Zoals eerder beschreven, kunt u Verminder het aantal bepaalde labels van de momentopnamen die u opslaat. De laatste twee parameters van de opdracht voor het initiëren van een momentopname zijn het label en het aantal momentopnamen die u wilt behouden.

```
./azure_hana_backup.pl hana HM3 hanadaily 30
```

In het vorige voorbeeld de label van de momentopname is **klant** en is het aantal momentopnamen met dit label moet worden bewaard **30**. Als u op verbruik van schijfruimte reageren, wilt u misschien Verminder het aantal opgeslagen momentopnamen. Een eenvoudige manier om het aantal momentopnamen beperken tot 15, bijvoorbeeld, wordt het script uitvoeren met de laatste parameter die is ingesteld op **15**:

```
./azure_hana_backup.pl hana HM3 hanadaily 15
```

Als u het script uitgevoerd met deze instelling, is het aantal momentopnamen, met inbegrip van de nieuwe opslag momentopname 15. De recentste 15 momentopnamen worden bewaard, terwijl de 15 oudere momentopnamen worden verwijderd.

 >[!NOTE]
 > Dit script vermindert het aantal momentopnamen alleen als er momentopnamen die meer dan een uur oud. Het script worden niet verwijderd voor momentopnamen die minder dan een uur oud. Deze beperkingen zijn gerelateerd aan de optionele herstel na noodgevallen-functionaliteit.

Als u niet langer wilt onderhouden van een set van momentopnamen met een specifieke back-label **hanadaily** in de syntaxisvoorbeelden kunt u het script uitvoeren **0** nummer bewaren. Hiermee verwijdert u alle momentopnamen die overeenkomt met dat label. Alle momentopnamen verwijderen kan echter de mogelijkheden van herstel na noodgevallen beïnvloeden.

Een tweede mogelijkheid specifieke momentopnamen wilt verwijderen, is het gebruik van het script `azure_hana_snapshot_delete.pl`. Dit script is ontworpen voor het verwijderen van een momentopname of een set van momentopnamen, hetzij met behulp van de back-ID HANA zoals gevonden in HANA Studio of via de naam van de momentopname. Op dit moment wordt alleen de back-up-ID is gekoppeld aan de momentopnamen die zijn gemaakt voor de **hana** type momentopname. Momentopname maken van back-ups van het type **logboeken** en **boot** een SAP HANA-momentopname niet uitvoert. Daarom is er geen back-ID voor deze momentopnamen worden gevonden. Als de naam van de momentopname wordt opgegeven, zoekt alle momentopnamen op verschillende volumes die overeenkomen met de naam van de ingevoerde momentopname. De syntaxis van de aanroep van het script is:

```
./azure_hana_snapshot_delete.pl 

```

Voer het script als gebruiker **hoofdmap**.

Als u een momentopname selecteert, hebt u kunnen elke momentopname afzonderlijk verwijderen. U eerst het volume met de momentopname opgeven en geef vervolgens de naam van de momentopname. Als de momentopname in dat volume bestaat en meer dan een uur oud is, wordt deze verwijderd. U kunt de volumenamen van en momentopname vinden door het uitvoeren van de `azure_hana_snapshot_details` script. 

>[!IMPORTANT]
>Als er gegevens alleen op de momentopname die u wilt verwijderen, klikt u vervolgens als u de verwijdering uitvoert de gegevens niet verloren permanent verloren.

   

### <a name="recovering-to-the-most-recent-hana-snapshot"></a>Herstellen naar de meest recente HANA momentopname

Als u een scenario voor productie-down ondervindt, kan het proces van het herstellen van een momentopname van de opslag kan worden gestart als een klant incident met Microsoft Azure-ondersteuning. Er is een kwestie hoge urgentie als gegevens in een productiesysteem is verwijderd en de enige manier om de gegevens worden opgehaald om de productiedatabase te herstellen.

In een andere situatie een punt in tijd herstel mogelijk lage urgentie en geplande dagen van tevoren. U kunt dit herstel met SAP HANA plannen op Azure Service Management in plaats van een probleem met hoge prioriteit verhogen. U kunt bijvoorbeeld planning de SAP-software door het toepassen van een nieuwe uitbreiding pakket bijwerken. Vervolgens moet u terugkeren naar een momentopname die de status voor de uitbreiding pakketupgrade vertegenwoordigt.

Voordat u de aanvraag verzendt, moet u om voor te bereiden. De SAP HANA in Azure Service Management-team kunt vervolgens de aanvraag te verwerken en de herstelde volumes bieden. Daarna kunt u de HANA-database op basis van de momentopnamen terugzetten. Ga als volgt om voor te bereiden voor de aanvraag:

>[!NOTE]
>De gebruikersinterface kan afwijken van de volgende schermafbeeldingen, afhankelijk van de SAP HANA-versie die u gebruikt.

1. Bepaal welke momentopname om terug te zetten. Alleen hana/gegevensvolume wordt teruggezet, tenzij u anders opdracht geven. 

2. Het exemplaar HANA afgesloten.

 ![Het exemplaar HANA afsluiten](./media/hana-overview-high-availability-disaster-recovery/image7-shutdown-hana.png)

3. Ontkoppel de gegevensvolumes op elk knooppunt HANA-database. Als de gegevensvolumes zijn nog steeds gekoppeld aan het besturingssysteem, mislukt het herstel van de momentopname.
 ![Ontkoppel de gegevensvolumes op elk knooppunt HANA-database](./media/hana-overview-high-availability-disaster-recovery/image8-unmount-data-volumes.png)

4. Een Azure ondersteuningsaanvraag instructie worden gegeven over het herstel van een momentopname van een specifieke openen.

 - Tijdens het terugzetten: SAP HANA op Azure Service Management kan u vragen om een telefonische om ervoor te zorgen coördinatie, verificatie en bevestiging dat de juiste opslag momentopname is teruggezet letten. 

 - Na het herstel van de: SAP HANA op Azure Service Management waarschuwt u als de opslag momentopname is teruggezet.

5. Nadat het herstelproces voltooid is, opnieuw koppelen alle gegevensvolumes.

 ![De gegevensvolumes opnieuw koppelen](./media/hana-overview-high-availability-disaster-recovery/image9-remount-data-volumes.png)

6. Selecteer de opties voor herstel in SAP HANA Studio als ze niet automatisch afkomstig zijn van wanneer u opnieuw verbinding met HANA-database met SAP HANA Studio maken. Het volgende voorbeeld ziet een herstelbewerking tot de laatste HANA momentopname. Een opslag-momentopnamen worden ingesloten één HANA momentopname. Als u naar de meest recente opslag momentopname herstelt, moet de meest recente HANA momentopname. (Als u naar een momentopname van een oudere opslag herstellen, moet u zoeken van de momentopname HANA is gebaseerd op de tijd dat de opslag momentopname werd gemaakt.)

 ![Selecteer de opties herstellen vanuit SAP HANA-Studio](./media/hana-overview-high-availability-disaster-recovery/image10-recover-options-a.png)

7. Selecteer **herstel de database naar een specifieke gegevens back-up of opslag momentopname**.

 ![Het venster hersteltype opgeven](./media/hana-overview-high-availability-disaster-recovery/image11-recover-options-b.png)

8. Selecteer **Geef back-up zonder catalogus**.

 ![Het venster voor back-uplocatie opgeven](./media/hana-overview-high-availability-disaster-recovery/image12-recover-options-c.png)

9. In de **doeltype** selecteert **momentopname**.

 ![Geef op de back-up herstellen venster](./media/hana-overview-high-availability-disaster-recovery/image13-recover-options-d.png)

10. Selecteer **voltooien** het herstelproces te starten.

 ![Selecteer het herstelproces te starten ' voltooien'](./media/hana-overview-high-availability-disaster-recovery/image14-recover-options-e.png)

11. De HANA-database is hersteld en hersteld naar de HANA momentopname die opgenomen in de momentopname van de opslag.

 ![HANA-database is hersteld en hersteld op de momentopname HANA](./media/hana-overview-high-availability-disaster-recovery/image15-recover-options-f.png)

### <a name="recovering-to-the-most-recent-state"></a>Herstellen naar de meest recente status

Het volgende proces herstelt de HANA momentopname die is opgenomen in de momentopname van de opslag. Vervolgens worden hersteld de transactie logboekback-ups naar de meest recente status van de database voordat u de opslag momentopname terugzet.

>[!IMPORTANT]
>Voordat u doorgaat, zorg ervoor dat er een volledige en aaneengesloten reeks transactie logboekback-ups. Zonder deze back-ups, kunt u de huidige status van de database niet terugzetten.

1. Voltooi de stappen 1-6 van uit [herstelt op een momentopname van de meest recente HANA](#recovering-to-the-most-recent-hana-snapshot).

2. Selecteer **herstel de database naar de meest recente status**.

 ![Selecteer 'De database naar de meest recente status herstellen'](./media/hana-overview-high-availability-disaster-recovery/image16-recover-database-a.png)

3. Geef de locatie van de meest recente HANA logboekback-ups. De locatie moet bevatten alle de HANA transactielogboek back-ups van de momentopname HANA naar de meest recente status.

 ![Geef de locatie van de meest recente HANA logboekback-ups](./media/hana-overview-high-availability-disaster-recovery/image17-recover-database-b.png)

4. Selecteer een back-up als uitgangspunt van waaruit de database herstellen. In ons voorbeeld is de HANA momentopname in de schermafbeelding de HANA momentopname die is opgenomen in de momentopname van de opslag. 

 ![Selecteer een back-up als uitgangspunt van waaruit de database herstellen](./media/hana-overview-high-availability-disaster-recovery/image18-recover-database-c.png)

5. Schakel de **gebruik Delta back-ups** selectievakje in als delta's niet bestaan tussen de tijd van de momentopname HANA en de meest recente status.

 ![Schakel het selectievakje 'Gebruik Delta back-ups' uit als er geen delta's bestaan](./media/hana-overview-high-availability-disaster-recovery/image19-recover-database-d.png)

6. Selecteer op het scherm Samenvatting **voltooien** te beginnen met de procedure voor herstel.

 ![Klik op 'Voltooid' in het scherm Samenvatting](./media/hana-overview-high-availability-disaster-recovery/image20-recover-database-e.png)

### <a name="recovering-to-another-point-in-time"></a>Herstellen naar een ander punt in tijd
Als u wilt herstellen naar een punt in tijd tussen de HANA-momentopname (opgenomen in de opslag momentopname) en één die later is dan het herstel HANA momentopname punt in tijd, het volgende doen:

1. Zorg ervoor dat u alle de transactie-logboekback-ups van de momentopname HANA aan de tijd die u wilt herstellen.
2. Beginnen met de procedure onder [herstellen naar de meest recente status](#recovering-to-the-most-recent-state).
3. In stap 2 van de procedure in de **hersteltype opgeven** Selecteer **herstel de database naar het volgende punt in tijd**, en geef op het punt in tijd. Voltooi stap 3-6.

### <a name="monitoring-the-execution-of-snapshots"></a>Bewaking van de uitvoering van momentopnamen

Als u opslag-momentopnamen van grote exemplaren HANA gebruikt, moet u ook voor het bewaken van de uitvoering van deze opslag-momentopnamen. Het script dat wordt uitgevoerd een opslag-momentopnamen schrijft uitvoer naar een bestand en slaat deze op dezelfde locatie als de Perl-scripts. Een afzonderlijk bestand is geschreven voor elke momentopname opslag. De uitvoer van elk bestand duidelijk ziet u de verschillende fasen dat de momentopname-script wordt uitgevoerd:

1. De volumes die u wilt maken van een momentopname vinden.
2. Zoek de momentopnamen die afkomstig zijn uit deze volumes.
3. Verwijder eventuele bestaande momentopnamen overeenkomen met het aantal momentopnamen die u hebt opgegeven.
4. Een SAP HANA-momentopname maken.
5. De momentopname van de opslag via de volumes maken.
6. Verwijder de SAP HANA-momentopname.
7. Wijzig de naam van de meest recente momentopname **.0**.

Het belangrijkste deel van het script CAB-bestand geïdentificeerd, is dit gedeelte:
```
**********************Creating HANA snapshot**********************
Creating the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data create snapshot"" ...
HANA snapshot created successfully.
**********************Creating Storage snapshot**********************
Taking snapshot hourly.recent for hana_data_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_backup_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_log_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for hana_shared_lhanad01_t020_vol ...
Snapshot created successfully.
Taking snapshot hourly.recent for sapmnt_lhanad01_t020_vol ...
Snapshot created successfully.
**********************Deleting HANA snapshot**********************
Deleting the HANA snapshot with command: "./hdbsql -n localhost -i 01 -U SCADMIN01 "backup data drop snapshot"" ...
HANA snapshot deletion successfully.
```
U kunt van dit voorbeeld zien hoe het script registreert voor het maken van de momentopname HANA. Dit proces wordt in het geval van scale-out is gestart op het hoofdknooppunt. Het hoofdknooppunt initieert de synchrone maken van de momentopnamen voor SAP HANA op elke worker-knooppunten. Vervolgens wordt de momentopname van de opslag gemaakt. Na de voltooiing van uitvoering van de opslag-momentopnamen, wordt de momentopname HANA verwijderd. Het verwijderen van de momentopname HANA wordt gestart vanuit het hoofdknooppunt.


## <a name="disaster-recovery-principles"></a>Principes van herstel na noodgevallen
Met grote exemplaren HANA bieden we een functionaliteit herstel na noodgevallen tussen HANA grote exemplaar stempels in verschillende Azure-regio's. Bijvoorbeeld, als u grote exemplaar HANA eenheden in de regio VS-West van Azure implementeert, kunt u de eenheden HANA grote exemplaar in de regio VS-Oost als eenheden voor herstel na noodgevallen. Zoals eerder gezegd, herstel na noodgevallen niet automatisch geconfigureerd, omdat u betaalt voor een andere grote exemplaar HANA eenheid in de regio DR vereist. De instellingen voor herstel na noodgevallen werkt voor scale-up, evenals een scale-out-instellingen. 

In de scenario's die tot nu toe is geïmplementeerd, gebruik onze klanten de eenheid in de regio DR om uit te voeren niet-productieve systemen die gebruikmaken van een geïnstalleerde HANA-exemplaar. De eenheid HANA grote exemplaar moet van dezelfde SKU hebben als de SKU die wordt gebruikt voor productiedoeleinden. De schijfconfiguratie tussen de server-eenheid in de-Azure-productieregio en het herstel na noodgevallen regio ziet er als volgt:

![Configuratie voor Noodherstel voor de installatie uit oogpunt van schijf](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_setup.PNG)

Zoals u in deze afbeelding overzicht, moet u vervolgens een tweede reeks schijfvolumes rangschikken. De doel-schijfvolumes zijn dezelfde grootte hebben als de omvang van de productie voor de productie-exemplaar in de eenheden van het herstel na noodgevallen. Deze schijfvolumes zijn gekoppeld aan de eenheid van de server HANA grote exemplaar in de site van de herstel na noodgevallen. De volgende volumes worden gerepliceerd van de productieregio naar de DR-site:

- hana/gegevens
- hana/logbackups 
- /Hana/Shared (inclusief/usr/sap)

Het volume /hana/log worden niet gerepliceerd omdat een SAP HANA-transactielogboek is niet nodig is in de manier waarop het herstel van deze volumes wordt uitgevoerd. 

De basis van de functionaliteit herstel na noodgevallen aangeboden aan de storage-replicatie-functionaliteit wordt aangeboden door de infrastructuur HANA grote exemplaar. De functionaliteit die wordt gebruikt op de kant van de opslag is niet een constante stream met wijzigingen die worden gerepliceerd op asynchrone wijze als wijzigingen per ongeluk het opslagvolume. Het is in plaats daarvan een mechanisme die afhankelijk van het feit is dat momentopnamen van deze volumes regelmatig worden gemaakt. De verschillen tussen een al gerepliceerde momentopname en een nieuwe momentopname die nog niet is gerepliceerd wordt overgedragen naar de site voor herstel na noodgevallen in schijfvolumes doel.  Deze momentopnamen worden opgeslagen op de volumes en in het geval van een failover van de herstel na noodgevallen, moet worden hersteld op deze volumes.  

De eerste overdracht van de volledige gegevens van het volume moet zijn voordat de hoeveelheid gegevens kleiner is dan de delta's tussen momentopnamen wordt. Als gevolg hiervan bevatten de volumes in de DR-site om een van de volumemomentopnamen uitgevoerd in de productiesite. Dit feit kunt u uiteindelijk dat DR-systeem gebruiken om te gaan naar een eerdere status te herstellen van verloren gegevens, zonder de productiesysteem worden teruggedraaid.

In gevallen waar u HANA System Replication als hoge beschikbaarheid-functionaliteit in uw productiesite gebruiken worden alleen de volumes van het exemplaar Tier 2 (of replica) gerepliceerd. Deze configuratie kan leiden tot een vertraging in de storage-replicatie naar de DR-site als u onderhouden of u de secundaire replica (laag 2) server eenheid of SAP HANA-exemplaar in deze eenheid neemt. 

>[!IMPORTANT]
>Net als bij met meerdere lagen HANA System Replication blokkeert afsluiten van de laag 2 HANA exemplaar of de server eenheid replicatie naar de site voor herstel na noodgevallen wanneer u de functionaliteit van grote exemplaar HANA herstel na noodgevallen.


>[!NOTE]
>De functionaliteit van de storage-replicatie HANA grote exemplaar is spiegelen en repliceren van opslag-momentopnamen. Als u opslag-momentopnamen zoals geïntroduceerd in de back-sectie van dit document niet uitvoert, er kan daarom niet worden replicatie naar de site herstel na noodgevallen. Opslag momentopname uitvoering is een vereiste voor het storage-replicatie naar de site voor herstel na noodgevallen.



## <a name="preparation-of-the-disaster-recovery-scenario"></a>De voorbereiding van het scenario voor herstel na noodgevallen
Er wordt ervan uitgegaan dat u hebt een productiesysteem voor grote HANA-exemplaren in de Azure-regio voor productie. Voor de volgende documentatie, gaan we ervan uit dat de SID van dat systeem HANA is "PRD." We ook wordt ervan uitgegaan dat u hebt een niet-productieomgeving uitgevoerd op grote HANA instanties die worden uitgevoerd in de Azure-regio voor noodherstel. Voor documentatie over we ervan uit dat de SID is 'TST'. Zodat de configuratie ziet er als volgt:

![Starten van de DR-installatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start1.PNG)

Als de server-exemplaar is niet besteld al de extra opslagruimte volume is ingesteld, koppelt SAP HANA op Azure Service Management de aanvullende set volumes als doel voor de productie-replica aan de HANA grote exemplaar eenheid dat u de TST worden uitgevoerd HANA-exemplaar op. Hiertoe moet u de SID van uw productie HANA-exemplaar opgeven. Nadat SAP HANA op Azure Service Management de bijlage van deze volumes bevestigt, moet u de volumes aan de eenheid HANA grote exemplaar koppelen.

![Volgende stap voor DR-installatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start2.PNG)

De volgende stap voor u is voor het installeren van de tweede SAP HANA-exemplaar op de eenheid HANA grote exemplaar in de Azure-regio waar u het exemplaar TST HANA uitgevoerd voor noodherstel. De nieuw geïnstalleerde SAP HANA-exemplaar moet dezelfde SID hebben. De gebruikers die zijn gemaakt, moet dezelfde gebruikers-id en groep-ID die de productie-exemplaar heeft. Als de installatie is voltooid, moet u naar:
- Stop de nieuw geïnstalleerde SAP HANA-exemplaar op de HANA grote exemplaar eenheid in de Azure-regio voor noodherstel.
- Ontkoppel de volumes van deze PRD en neem contact op met de SAP HANA op Azure Service Management. De volumes niet actief blijven gekoppeld aan de eenheid omdat ze kunnen functioneren als replicatiedoel opslag niet toegankelijk.  

![DR instellingsstap voordat er replicatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start3.PNG)

Het operationele team gaat tot stand brengen van de replicatierelatie tussen de PRD volumes in de Azure-regio voor productie en de volumes PRD in de Azure-regio voor noodherstel.

>[!IMPORTANT]
>Het volume /hana/log wordt niet gerepliceerd omdat het is niet nodig de gerepliceerde SAP HANA-database herstellen naar een consistente status in de site van de herstel na noodgevallen.

De volgende stap voor u is het instellen of aanpassen van het back-upschema van opslag momentopname te krijgen tot uw RTO en RPO in het geval na noodgevallen. Om te beperken het beoogde herstelpunt, stelt u de volgende replicatie-intervallen in de grote exemplaar HANA-service:
- De volumes waarop de momentopname van de gecombineerde (type momentopname = **hana**) om de 15 minuten bij de doelen van het volume gelijk opslag in de site voor herstel na noodgevallen repliceren.
- Het logboek voor databasetransacties back-volume (type momentopname = **logboeken**) om de drie minuten bij de doelen van het volume gelijk opslag in de site voor herstel na noodgevallen worden gerepliceerd.

Om te beperken het beoogde herstelpunt, stelt u het volgende:
- Voer een **hana** type opslag momentopname (Zie ' stap 7: momentopnamen uitvoeren ') elke 30 minuten op 1 uur.
- Om de 5 minuten voor SAP HANA-transactielogboek back-ups uitvoeren.
- Voer een **logboeken** typt u een momentopname van elke 5 tot 15 minuten opslag. Met deze Intervalperiode moet u een RPO van ongeveer 15 25 minuten bereiken.

Met deze installatie, de volgorde van de transactie logboekback-ups, opslag-momentopnamen en de replicatie van het transactielogboek HANA back-up volume en /hana/data en /hana/shared (inclusief/usr/sap) kan eruitzien als de gegevens in deze afbeelding wordt weergegeven:

 ![Relatie tussen een momentopname van het logboek voor databasetransacties en een mirror module op een tijdas](./media/hana-overview-high-availability-disaster-recovery/snapmirror.PNG)

Om te zorgen voor een nog beter RPO in het geval van herstel na noodgevallen, kunt u de HANA transactie-logboekback-ups van SAP HANA in Azure (grote exemplaren) kopiëren naar de andere Azure-regio. Dit verdere RPO om daling te bereiken, moet u de volgende ruwe stappen uitvoeren:

1. Meld u zo vaak mogelijk /hana/logbackups back-up van de transactie HANA.
2. Gebruik rsync kopiëren van de transactie logboekback-ups naar de NFS-share gehoste virtuele machines in Azure. De virtuele machines zijn in Azure virtuele netwerken in de-Azure-productieregio en in de DR-regio's. U moet beide virtuele netwerken van Azure verbinding met het circuit verbinding maken met de productie HANA grote exemplaren naar Azure. Zie de afbeeldingen in de [netwerk overwegingen voor herstel na noodgevallen met grote exemplaren HANA](#Network-considerations-for-disaster-recovery-with-HANA-Large-Instances) sectie. 
3. Houd die de transactie-logboekback-ups in het gebied van de virtuele machine is gekoppeld aan de NFS geëxporteerd opslag.
4. In geval na noodgevallen failover vormen een aanvulling op de transactie-logboekback-ups die u vinden op het volume /hana/logbackups met meer onlangs gebruikte transactielogboek back-ups op de NFS-share op de site voor herstel na noodgevallen. 
5. U kunt nu een transactie-logboekback-up om te zetten naar de meest recente back-up die via kan worden opgeslagen op het gebied DR starten.

Als operations HANA grote exemplaar bevestigen dat de installatie van de relatie replicatie en u de back-ups van uitvoering opslag momentopname start, begint de gegevens worden gerepliceerd.

![DR instellingsstap voordat er replicatie](./media/hana-overview-high-availability-disaster-recovery/disaster_recovery_start4.PNG)

Tijdens de replicatie de voortgang van de momentopnamen voor de volumes PRD in het herstel na noodgevallen Azure regio's niet worden hersteld. Ze worden alleen opgeslagen. Als de volumes zijn gekoppeld in deze staat, vertegenwoordigen ze de status waarin u deze volumes ontkoppeld nadat het PRD SAP HANA-exemplaar is geïnstalleerd in de eenheid van de server in de Azure-regio voor noodherstel. Ze ook vertegenwoordigen de opslag back-ups die nog niet zijn hersteld.

Bij een failover kunt ook u om een momentopname van een oudere opslag in plaats van de momentopname van de meest recente opslag te herstellen.

## <a name="disaster-recovery-failover-procedure"></a>Herstel na noodgevallen failover procedure
Als moet of wilt u de failover naar de DR-site, moet u om te communiceren met de SAP HANA op Azure-bewerkingen team. In ruwe stappen is het proces tot nu toe ziet er als volgt:

1. Omdat u op de eenheid voor herstel na noodgevallen HANA grote exemplaren van een niet-productieve exemplaar van HANA uitvoert, moet u dit exemplaar afgesloten. Er wordt ervan uitgegaan dat er een inactieve HANA productie exemplaar vooraf zijn geïnstalleerd.
2. Zorg ervoor dat er geen SAP HANA-processen worden uitgevoerd. Gebruik van de volgende opdracht voor deze controle: `/usr/sap/hostctrl/exe/sapcontrol –nr <HANA instance number> - function GetProcessList`. U moet worden weergegeven door de uitvoer de **hdbdaemon** verwerken in een gestopte status en geen andere HANA processen in een status uitgevoerd of is gestart.
3. Bepalen welke naam van de momentopname of een SAP HANA-back-ID die u wilt de site herstel na noodgevallen is hersteld. In de echte noodherstel gevallen is deze momentopname meestal de meest recente momentopname. Als u gegevens herstellen verloren moet, kiest u een eerdere momentopname.
4. Neem contact op met Azure ondersteuning via een hoge prioriteit ondersteuningsaanvraag en vraag om het terugzetten van het die momentopname (naam en datum van de momentopname) of HANA back-up-ID op de DR-site. De standaardwaarde is dat bewerkingen alleen de /hana/data volume herstelt. Als u dat de hana/logbackups-volumes wilt, moet u de status die specifiek. *We bevelen niet dat u het volume /hana/shared terugzet.* In plaats daarvan de keuze van specifieke bestanden, zoals global.ini buiten de **.snapshot** directory en de bijbehorende submappen nadat u het opnieuw / hana koppelen/gedeeld clustervolume voor PRD. De operations-zijde, de volgende stappen gaat gebeuren: een. De replicatie van momentopnamen van het productievolume tot de volumes voor herstel na noodgevallen is gestopt. Dit kan al gebeurd zijn als een storing in productie is de reden moet u een Noodherstel.
    b. De opslag een momentopname van de naam of een momentopname van de back-up ID die u hebt gekozen wordt hersteld op de volumes voor herstel na noodgevallen.
    c. Na het herstel, herstel na noodgevallen volumes zijn beschikbaar, worden gekoppeld aan de eenheden HANA grote exemplaar in de regio van het herstel na noodgevallen.
5. Koppel de volumes voor herstel na noodgevallen de eenheid HANA grote exemplaar in de site voor herstel na noodgevallen. 
6. Start de tot nu toe slapende SAP HANA productie-exemplaar.
7. Als u hebt gekozen voor het kopiëren van back-uplogboeken transactielogboek ook om het RPO versnellen, moet u deze transactie-logboekback-ups in de directory van het nieuwe gekoppelde DR/hana/logbackups samenvoegen. Bestaande back-ups niet worden overschreven. Kopieer alleen nieuwere back-ups die niet zijn gerepliceerd met de meest recente replicatie van een momentopname van de opslag.
8. U kunt ook afzonderlijke bestanden buiten de momentopnamen die zijn gerepliceerd naar het volume /hana/shared/PRD in de Azure-regio voor noodherstel herstellen.

De volgende volgorde van stappen omvat het herstellen van de SAP HANA productie-exemplaren op basis van de momentopname van de herstelde opslag en het transactielogboek back-ups die beschikbaar zijn. De stappen moeten uitzien:

1. Wijzigen van de back-uplocatie op **hana/logbackups** met behulp van SAP HANA Studio.
   ![Wijzigen van de back-uplocatie voor DR-herstel](./media/hana-overview-high-availability-disaster-recovery/change_backup_location_dr1.png)

2. SAP HANA scant via de back-upbestand locaties en stelt de meest recente transactie-logboekback-up om naar te herstellen. De scan kan even duren tot een scherm, zoals hieronder wordt weergegeven: ![lijst met transactie-logboekback-ups voor DR-herstel](./media/hana-overview-high-availability-disaster-recovery/backup_list_dr2.PNG)

3. Sommige van de standaard-instellingen aanpassen:

      - Schakel **gebruik Delta back-ups**.
      - Selecteer **gebied logboek initialiseren**.

   ![Het logboek initialiseren gebied instellen](./media/hana-overview-high-availability-disaster-recovery/initialize_log_dr3.PNG)

4. Selecteer **Voltooien**.

   ![Het terugzetten van de DR voltooien](./media/hana-overview-high-availability-disaster-recovery/finish_dr4.PNG)

Een voortgangsvenster wordt weergegeven, zoals hier wordt weergegeven, worden weergegeven. Houd er rekening mee dat het voorbeeld van een herstelpunt voor herstel na noodgevallen van een configuratie met 3-knooppunt scale-out SAP HANA is.

![Voortgang van terugzetten](./media/hana-overview-high-availability-disaster-recovery/restore_progress_dr5.PNG)

Als het herstel lijkt te reageren op de **voltooien** scherm en bevat geen het voortgangsscherm controleren om te bevestigen dat de SAP HANA-op de worker-knooppunten exemplaren worden uitgevoerd. Start indien nodig de SAP HANA-exemplaren handmatig.


### <a name="failback-from-dr-to-a-production-site"></a>Failback vanuit DR aan een productiesite
U kunt een failback van een Noodherstel aan een productiesite. Bekijk het geval dat de failover naar de site voor herstel na noodgevallen is veroorzaakt door problemen in de Azure-regio voor productie en niet door uw behoeften voor het herstellen van gegevens verloren gaan. Dit betekent dat u uitvoert, de werkbelasting van uw SAP-productie voor enige tijd in de site voor herstel na noodgevallen. Als de problemen in de productiesite zijn opgelost, wilt u failback naar de productiesite. Omdat u kunt geen gegevens verloren gaan, wordt de stap terug naar de productiesite omvat meerdere stappen en nauwe samenwerking met de SAP HANA op Azure-bewerkingen team. Het is aan u voor het activeren van het operationele team om te beginnen met het synchroniseren terug naar de productiesite nadat de problemen zijn opgelost.

De volgorde van stappen ziet er als volgt:

1. De SAP HANA op Azure-bewerkingen team haalt de trigger de productie-opslagvolumes van de volumes van de opslag herstel na noodgevallen, die nu de productiestatus vertegenwoordigen te synchroniseren. In deze toestand is, is de eenheid HANA grote exemplaar in de productiesite afgesloten.
2. De SAP HANA op Azure-bewerkingen team bewaakt de replicatie en zorgt ervoor dat er een achterstallige voordat u wordt geïnformeerd als een klant wordt bereikt.
3. U afsluiten de toepassingen die gebruikmaken van de productie HANA exemplaar in de site voor herstel na noodgevallen. U vervolgens uitvoeren een back-up transactielogboek HANA. Vervolgens kunt u de HANA-exemplaar dat wordt uitgevoerd op de eenheden HANA grote exemplaar in de site voor herstel na noodgevallen stoppen.
4. Nadat de HANA-exemplaar in de eenheid HANA grote exemplaar in de site voor herstel na noodgeval wordt afgesloten, synchroniseert het operationele team handmatig de schijfvolumes opnieuw.
5. De SAP HANA op Azure het operationele team van de grote exemplaar HANA-eenheid in de productiesite opnieuw gestart en geeft die boven aan u. Zorg ervoor dat het SAP HANA-exemplaar in een afsluitingsstaat op het moment van het opstarten van de eenheid HANA grote exemplaar is.
6. U kunt stappen voor het terugzetten van dezelfde database uitvoeren als tijdens de failover van eerder naar de site voor herstel na noodgevallen.

### <a name="monitoring-disaster-recovery-replication"></a>Controleren van de disaster recovery replicatie

U kunt de status van uw opslag Replicatievoortgang controleren door het uitvoeren van script `azure_hana_replication_status.pl`. Dit script moet worden uitgevoerd vanaf een eenheid die wordt uitgevoerd op de locatie van het herstel na noodgevallen. Anders gaat het niet naar de functie zoals verwacht. Het script werkt ongeacht of de replicatie actief is. Het script kan worden uitgevoerd voor elke eenheid HANA grote exemplaar van uw tenant op de locatie van het herstel na noodgevallen. Deze kan niet worden gebruikt voor het verkrijgen van informatie over het opstartvolume.

Het script zoals aanroepen:
```
./replication_status.pl <HANA SID>
```

De uitvoer wordt opgedeeld, per volume in de volgende secties:  

- De status van koppeling
- Huidige replicatieactiviteit
- Meest recente momentopname gerepliceerd 
- Grootte van de meest recente momentopname
- Huidige vertragingstijd tussen momentopnamen--de laatste voltooide momentopnamereplicatie en nu  

Status van de koppeling wordt weergegeven als **Active** tenzij de koppeling tussen locaties niet actief is of een failover-gebeurtenis momenteel uitgevoerd wordt. De replicatieactiviteit heeft betrekking op of alle gegevens op dit moment wordt gerepliceerd of niet actief is of dat andere activiteiten zijn nu plaatsvindt op de koppeling. De laatste momentopname gerepliceerd alleen moet worden weergegeven als `snapmirror…`. De grootte van de laatste momentopname wordt weergegeven. Ten slotte wordt de vertraging weergegeven. De vertraging geeft de tijd van de geplande replicatie tijd wanneer de replicatie is voltooid. Een vertraging zijn groter dan een uur voor gegevensreplicatie, met name in de eerste replicatie, ondanks dat replicatie is gestart. De vertraging gaat verder toenemen totdat de lopende replicatie is voltooid.

Een voorbeeld van uitvoer kunt raadplegen als volgt:

```
hana_data_hm3_mnt00002_t020_dp
-------------------------------------------------
Link Status: Broken-Off
Current Replication Activity: Idle
Latest Snapshot Replicated: snapmirror.c169b434-75c0-11e6-9903-00a098a13ceb_2154095454.2017-04-21_051515
Size of Latest Snapshot Replicated: 244KB
Current Lag Time between snapshots: -   ***Less than 90 minutes is acceptable***
```













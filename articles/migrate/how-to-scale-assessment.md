---
title: Detectie en evaluatie uit te schalen met behulp van Azure Migrate | Microsoft Docs
description: Hierin wordt beschreven hoe u grote aantallen on-premises virtuele machines met behulp van de service Azure Migrate beoordeelt.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 12/05/2018
ms.author: raynew
ms.openlocfilehash: 6b74c81f44ee747cdd80a1106b669d89c8c7fbc3
ms.sourcegitcommit: a7331d0cc53805a7d3170c4368862cad0d4f3144
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55297137"
---
# <a name="discover-and-assess-a-large-vmware-environment"></a>Een grote VMware-omgeving ontdekken en beoordelen

Azure Migrate heeft een limiet van 1500 machines per project, in dit artikel wordt beschreven hoe u voor de beoordeling van grote aantallen on-premises virtuele machines (VM's) met behulp van [Azure Migrate](migrate-overview.md).   

## <a name="prerequisites"></a>Vereisten

- **VMware**: De virtuele machines die u wilt migreren, moeten worden beheerd door vCenter-Server versie 5.5, 6.0, 6.5 of 6.7. Daarnaast moet u één ESXi-host met versie 5.5 of later om de collector virtuele machine te implementeren.
- **vCenter-account**: U moet een alleen-lezen-account voor toegang tot de vCenter-Server. Azure Migrate gebruikt dit account om de on-premises virtuele machines te detecteren.
- **Machtigingen**: In de vCenter-Server moet u machtigingen voor het maken van een virtuele machine door het importeren van een bestand in OVA-indeling.
- **Instellingen voor statistieken**: Deze vereiste is alleen van toepassing op de [eenmalige detectie model](https://docs.microsoft.com/azure/migrate/concepts-collector#discovery-methods) die nu is afgeschaft. Voor detectie van eenmalige-model, moeten de instellingen voor statistieken voor de vCenter-Server worden ingesteld op niveau 3 voordat u begint met implementatie. Het statistiekniveau van de is ingesteld op 3 voor elk van de dag, week en maand verzameling intervallen. Als het niveau lager dan 3 voor elk van de drie verzameling intervallen is, werkt de beoordeling wel, maar de prestatiegegevens voor appopslag en netwerken worden niet verzameld. De aanbevelingen voor de grootte wordt vervolgens worden gebaseerd op prestatiegegevens voor CPU en geheugen en configuratiegegevens voor schijf en netwerkadapters.

> [!NOTE]
> Ondersteuning van het apparaat voor eenmalige detectie is nu beëindigd omdat deze methode gebaseerd was op statistiekinstellingen van vCenter Server voor de beschikbaarheid van prestatiegegevenspunten en gemiddelde prestatiemeteritems verzamelde, wat leidde tot een te voorzichtige schaling van virtuele machines voor migratie naar Azure.

### <a name="set-up-permissions"></a>Machtigingen instellen

Azure Migrate heeft toegang nodig tot de VMware-servers, zodat de virtuele machines automatisch kunnen worden gedetecteerd voor evaluatie. De VMware-account heeft de volgende machtigingen nodig:

- Gebruikerstype: Ten minste een alleen-lezen-gebruiker
- Machtigingen: Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen
- Details: Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.
- Wijs om de toegang te beperken de rol Geen toegang met het object Doorgeven naar onderliggend object toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM's en netwerken).

Als u in een omgeving met tenants implementeert, als volgt één manier om in te stellen deze:

1.  Maken van een gebruiker per tenant en het gebruik van [RBAC](https://docs.microsoft.com/azure/role-based-access-control/role-assignments-portal), alleen-lezen machtigingen toewijzen aan alle virtuele machines die horen bij een bepaalde tenant. Vervolgens gebruikt u deze referenties voor detectie. RBAC zorgt ervoor dat de bijbehorende vCenter-gebruiker toegang tot alleen tenant-specifieke virtuele machines heeft.
2. U RBAC instelt voor gebruikers van andere tenant zoals beschreven in het volgende voorbeeld voor 1 gebruiker en gebruiker #2:

    - In **gebruikersnaam** en **wachtwoord**, geef de referenties van het kenmerk alleen-lezen-account dat door de collector wordt gebruikt voor het detecteren van virtuele machines in
    - Datacenter1 - geven alleen-lezen machtigingen voor 1 gebruiker en gebruiker nr. 2. Deze machtigingen op alle onderliggende objecten niet worden doorgevoerd omdat u machtigingen hebt ingesteld op afzonderlijke virtuele machines.

      - VM1 (Tenant #1) (alleen-lezen-machtiging voor 1 gebruiker)
      - VM2 (Tenant #1) (alleen-lezen-machtiging voor 1 gebruiker)
      - VM3 (Tenant #2) (alleen-lezen-machtiging voor gebruiker nr. 2)
      - VM4 (Tenant #2) (alleen-lezen-machtiging voor gebruiker nr. 2)

   - Als u detectie met behulp van referenties voor 1 gebruiker uitvoert, wordt u alleen VM1 en VM2 gedetecteerd.

## <a name="plan-your-migration-projects-and-discoveries"></a>Plan uw migratieprojecten en detecties

Op basis van het aantal virtuele machines die u van plan bent om te detecteren, kunt u meerdere projecten maken en implementeren van meerdere apparaten in uw omgeving. Een apparaat kan worden verbonden met een enkele vCenter-Server en een enkel project (tenzij u de detectie stopt en start het uitvoerfragmenten).

In het geval van eenmalige discovery (nu afgeschaft), de detectie werkt in een fire en vergeet model, zodra een detectie wordt uitgevoerd, kunt u de dezelfde collector kunt gebruiken voor het verzamelen van gegevens uit een andere vCenter-Server of verzenden naar een andere migration-project.

> [!NOTE]
> Ondersteuning van het apparaat voor eenmalige detectie is nu beëindigd omdat deze methode gebaseerd was op statistiekinstellingen van vCenter Server voor de beschikbaarheid van prestatiegegevenspunten en gemiddelde prestatiemeteritems verzamelde, wat leidde tot een te voorzichtige schaling van virtuele machines voor migratie naar Azure. Het verdient aanbeveling om te verplaatsen naar het apparaat voor eenmalige detectie.

Plan uw detecties en evaluaties op basis van de volgende beperkingen:

| **Entiteit** | **Limiet voor computer** |
| ---------- | ----------------- |
| Project    | 1,500             |
| Detectie  | 1,500             |
| Beoordeling | 1,500             |

Houd rekening met deze overwegingen bij het plannen:

- Als u een detectie met behulp van de Azure Migrate collector doet, kunt u het detectiebereik ingesteld op een map van de vCenter-Server, datacenter, cluster of host.
- Controleer in de vCenter-Server die de virtuele machines die u wilt detecteren in mappen, datacenters, clusters of hosts die ondersteuning bieden voor de beperking van 1500 machines hiervoor meer dan één detectie van dezelfde vCenter-Server.
- U wordt aangeraden dat voor evaluatie, u machines met afhankelijkheden binnen het hetzelfde project en de evaluatie houden. In de vCenter-Server, ervoor zorgen dat afhankelijke machines zich in dezelfde map, datacenter of -cluster voor de evaluatie.

Afhankelijk van uw scenario, kunt u uw ontdekkingen kunnen splitsen, zoals voorgeschreven hieronder:

### <a name="multiple-vcenter-servers-with-less-than-1500-vms"></a>Meerdere vCenter-Servers met maximaal 1500 VM 's
Als u meerdere vCenter-Servers in uw omgeving hebt en het totale aantal virtuele machines minder dan 1500 is, kunt u de volgende benadering op basis van uw scenario:

**Continue detectie:** In het geval van continu detectie, kan een apparaat worden verbonden met alleen een enkel project. Daarom moet u een apparaat voor elk van de vCenter-Servers implementeert en maakt u een project voor elk apparaat en elke trigger detecties dienovereenkomstig.

**Eenmalige discovery (nu afgeschaft):** U kunt een één-collector en een enkele migration-project gebruiken voor het detecteren van alle virtuele machines voor alle vCenter-Servers. Omdat de collector eenmalige detectie een VMware vCenter Server op een tijdstip detecteert, kunt u de dezelfde collector uitvoeren op alle de vCenter-Servers elkaar en de collector verwijzen naar hetzelfde migratieproject. Als alle detecties voltooid zijn, kunt u vervolgens evaluaties voor de machines maken.


### <a name="multiple-vcenter-servers-with-more-than-1500-vms"></a>Meerdere vCenter-Servers met meer dan 1500 VM 's

Als u meerdere vCenter-Servers met minder dan 1500 virtuele machines per vCenter-Server, maar meer dan 1500 VM's voor alle vCenter-Servers hebt, moet u meerdere migration-projecten (één migration-project kan bevatten alleen 1500 VM's) maken. U kunt dit doen door het maken van een migratieproject per vCenter-Server en de detecties splitsen.

**Continue detectie:** U moet meerdere collector-apparaten (één voor elke vCenter-Server) maken en verbinden met elk apparaat een project en trigger detectie dienovereenkomstig.

**Eenmalige discovery (nu afgeschaft):** U kunt één collector gebruiken voor het detecteren van elke vCenter-Server (achter elkaar). Als u wilt dat de detecties te starten op hetzelfde moment, kunt u ook meerdere apparaten implementeren en de detecties parallel worden uitgevoerd.

### <a name="more-than-1500-machines-in-a-single-vcenter-server"></a>Meer dan 1500 machines in een enkele vCenter-Server

Als u meer dan 1500 virtuele machines in een enkele vCenter-Server hebt, moet u de detectie splitsen in meerdere migration-projecten. Als u wilt splitsen detecties, kunt u gebruikmaken van de Scope-veld in het apparaat en geef de host, cluster, map, of het datacenter die u wilt detecteren. Bijvoorbeeld, hebt u twee mappen in vCenter Server, een met 1000 VM's (Map1) en andere met 800 VM's (Folder2), kunt u in het veld scope om op te splitsen de detecties tussen deze mappen.

**Continue detectie:** In dit geval moet u twee collector toestellen maken voor de eerste collector, geef het bereik als Map1 en verbinden met de eerste migratieproject. U kunt parallel te leren kennen Folder2 met behulp van de tweede collector-apparaat en deze verbinden met de tweede migration-project.

**Eenmalige discovery (nu afgeschaft):** U kunt de dezelfde collector gebruiken voor het activeren van zowel de detecties. In de eerste detectie kunt u Map1 opgeven als het bereik en wijst u deze naar de eerste migratieproject zodra de eerste detectie is voltooid, kunt u de dezelfde collector gebruikt, wijzigen van het bereik in projectdetails Folder2 en migratie naar de tweede migration-project en de tweede detectie doen.

### <a name="multi-tenant-environment"></a>Multitenant-omgeving

Als u een omgeving die wordt gedeeld door tenants hebt en u niet wilt detecteren van de virtuele machines van één tenant in een andere tenant-abonnement, kunt u het veld bereik in het collector-apparaat als bereik voor de detectie. Als de tenants hosts delen, een referentie met alleen-lezen toegang tot alleen de virtuele machines die horen bij de specifieke tenant maken en deze referentie gebruiken in het collector-apparaat en het bereik opgeven als de host te doen de detectie.

## <a name="discover-on-premises-environment"></a>On-premises omgeving detecteren

Wanneer u klaar met uw abonnement bent, kunt u vervolgens de detectie van de on-premises virtuele machines starten:

### <a name="create-a-project"></a>Een project maken

Maak een Azure Migrate-project in overeenstemming met uw vereisten:

1. Selecteer in Azure Portal **Een resource maken**.
2. Zoek naar **Azure Migrate** en selecteer de service **Azure Migrate** in de zoekresultaten. Selecteer vervolgens **Maken**.
3. Geef een naam van het project en de Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de locatie waarin u wilt maken van het project en selecteer vervolgens **maken**. Houd er rekening mee dat u uw virtuele machines voor een andere doellocatie nog steeds kunt beoordelen. De opgegeven locatie voor het project wordt gebruikt voor het opslaan van de metagegevens die zijn verzameld van on-premises VM's.

### <a name="set-up-the-collector-appliance"></a>Het collector-apparaat instellen

Azure Migrate maakt een on-premises virtuele machine die het collector-apparaat wordt genoemd. Deze virtuele machine detecteert on-premises VMware-machines en stuurt metagegevens over deze naar de service Azure Migrate. Als u het collector-apparaat instelt, moet u een OVA-bestand downloaden en importeren naar de on-premises vCenter Server-exemplaar.

#### <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Als u meerdere projecten hebt, moet u het collector-apparaat slechts één keer downloaden met vCenter-Server. Na het downloaden en instellen van het apparaat, u deze voor elk project uitvoeren en u de unieke project-ID en sleutel opgeven.

1. Klik in het Azure Migrate-project op **Aan de slag** > **Detecteren en evalueren** > **Machines detecteren**.
2. Klik in **Machines detecteren** op **Downloaden** om het apparaat te downloaden.

    Het Azure Migrate-apparaat communiceert met vCenter Server en profileert continu de on-premises omgeving om realtime gebruiksgegevens voor elke virtuele machine te verzamelen. Het apparaat verzamelt piektellers voor elk metrisch gegeven (CPU-gebruik, geheugengebruik, enzovoort). Dit model is voor het verzamelen van prestatiegegevens niet afhankelijk van de instellingen voor statistieken in vCenter Server. U kunt de continue profilering op elk gewenst moment stoppen vanaf het apparaat.

    > [!NOTE]
    > Ondersteuning van het apparaat voor eenmalige detectie is nu beëindigd omdat deze methode gebaseerd was op statistiekinstellingen van vCenter Server voor de beschikbaarheid van prestatiegegevenspunten en gemiddelde prestatiemeteritems verzamelde, wat leidde tot een te voorzichtige schaling van virtuele machines voor migratie naar Azure.

    **Onmiddellijk resultaat:** als de detectie met het apparaat voor continue detectie voltooid is (wat enkele uren kan duren, afhankelijk van het aantal virtuele machines), kunt u meteen evaluaties gaan maken. Omdat het verzamelen van prestatiegegevens wordt gestart wanneer u detectie activeert, en als u direct resultaat wilt, moet u het schaalcriterium in de evaluatie instellen als *as on-premises*. Voor evaluaties op basis van prestaties is het raadzaam om ten minste een dag te wachten na het activeren van de detectie om betrouwbare aanbevelingen voor de schaal te krijgen.

    Houd er rekening mee dat het apparaat alleen continu prestatiegegevens verzamelt, het detecteert niet elke configuratiewijziging in de on-premises omgeving (dat wil zeggen het toevoegen/verwijderen van VM’s, toevoegen van schijven, enz.). Als er een configuratiewijziging in de on-premises omgeving is, kunt u het volgende doen om de wijzigingen door te voeren in de portal:

    - Toevoegen van items (virtuele machines, schijven, kernen enz.): om deze wijzigingen in de Azure-portal door te voeren, kunt u de detectie vanaf het apparaat stoppen en opnieuw starten. Dit zorgt ervoor dat de wijzigingen worden bijgewerkt in het Azure Migrate-project.

    - Verwijderen van VM’s: vanwege de manier waarop het apparaat is ontworpen, wordt het verwijderen van VM’s niet doorgevoerd, zelfs niet als u de detectie stopt en opnieuw start. Dit komt doordat gegevens uit volgende detecties worden toegevoegd aan de oudere detecties en niet worden overschreven. In dit geval kunt u eenvoudigweg de VM in de portal negeren door deze uit uw groep te verwijderen en de evaluatie opnieuw te berekenen.

3. In **projectreferenties kopiëren**, kopieert u de ID en de sleutel voor het project. U hebt deze nodig tijdens de configuratie van collector.


#### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Controleer dat het OVA-bestand beveiligd is voordat u deze implementeert:

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.

2. Gebruik de volgende opdracht om de hash voor het OVA-bestand te genereren:

   ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```

   Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```

3. Zorg ervoor dat de gegenereerde hash komt overeen met de volgende instellingen.

#### <a name="continuous-discovery"></a>Continue detectie

Voor OVA-versie 1.0.10.4

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be

#### <a name="one-time-discovery-deprecated-now"></a>Eenmalige detectie (nu beëindigd)

Voor OVA-versie 1.0.9.15 (vrijgegeven op 23-10-2018)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

Voor OVA-versie 1.0.9.14 (vrijgegeven op 8/24/2018)

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | 6d8446c0eeba3de3ecc9bc3713f9c8bd
SHA1 | e9f5bdfdd1a746c11910ed917511b5d91b9f939f
SHA256 | 7f7636d0959379502dfbda19b8e3f47f3a4744ee9453fc9ce548e6682a66f13c

Voor OVA-versie 1.0.9.12

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | d0363e5d1b377a8eb08843cf034ac28a
SHA1 | df4a0ada64bfa59c37acf521d15dcabe7f3f716b
SHA256 | f677b6c255e3d4d529315a31b5947edfe46f45e4eb4dbc8019d68d1d1b337c2e

Voor OVA-versie 1.0.9.8

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | b5d9f0caf15ca357ac0563468c2e6251
SHA1 | d6179b5bfe84e123fabd37f8a1e4930839eeb0e5
SHA256 | 09c68b168719cb93bd439ea6a5fe21a3b01beec0e15b84204857061ca5b116ff

Voor OVA-versie 1.0.9.7

**Algoritme** | **Hash-waarde**
--- | ---
MD5 | d5b6a03701203ff556fa78694d6d7c35
SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c

### <a name="create-the-collector-vm"></a>De collector-VM maken

Importeer het gedownloade bestand met vCenter-Server:

1. Selecteer in de vSphere Client-console **bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/how-to-scale-assessment/vcenter-wizard.png)

2. In de Wizard OVF-sjabloon implementeren > **bron**, geef de locatie van het OVA-bestand.
3. Geef in **Naam** en **Locatie** een beschrijvende naam op voor de collector-VM, en het inventarisobject waarin de virtuele machine wordt gehost.
4. Geef in **Host/Cluster** de host of het cluster op waarop de collector-VM wordt uitgevoerd.
5. Geef in de opslag de opslaglocatie voor de collector-VM op.
6. Geef in **Schijfindeling** het schijftype en de schijfgrootte op.
7. Geef in **Netwerktoewijzing** het netwerk op waarmee de collector-VM verbinding maakt. Het netwerk moet verbinding met internet voor het verzenden van metagegevens naar Azure.
8. Controleer en Bevestig de instellingen en selecteer vervolgens **voltooien**.

### <a name="identify-the-id-and-key-for-each-project"></a>Identificeren van de ID en de sleutel voor elk project

Als u meerdere projecten hebt, moet u om te bepalen van de ID en sleutel voor elk criterium. U moet de sleutel tijdens het uitvoeren van de collector voor het detecteren van de virtuele machines.

1. Selecteer in het project, **aan de slag** > **detecteren en evalueren** > **Machines detecteren**.
2. In **projectreferenties kopiëren**, kopieert u de ID en de sleutel voor het project.
    ![Projectreferenties kopiëren](./media/how-to-scale-assessment/copy-project-credentials.png)

### <a name="run-the-collector-to-discover-vms"></a>De collector uitvoeren om virtuele machines te detecteren

Voor elke detectie die u nodig hebt om uit te voeren, moet u de collector voor het detecteren van virtuele machines in het vereiste bereik uitvoeren. Voer de detecties één na de andere. Gelijktijdige detecties worden niet ondersteund moet, en elke detectieregel een ander bereik.

1.  Klik in de vSphere Client-console met de rechtermuisknop op de VM > **Console openen**.
2.  Geef de voorkeursinstellingen voor de taal, de tijdzone en wachtwoorden op voor het apparaat.
3.  Selecteer op het bureaublad, de **collector uitvoeren** snelkoppeling.
4.  Open in de Azure Migrate collector, **vereisten instellen** en vervolgens:

    a. Accepteer de licentievoorwaarden en lees de informatie van derden.

    De collector controleert of de virtuele machine toegang heeft tot internet.

    b. Als de virtuele machine toegang heeft tot internet via een proxyserver, selecteert u **Proxy-instellingen**, en geeft u het proxyadres en de poort voor luisteren. Geef referenties op als de proxy verificatie nodig heeft.

    De collector controleert of de collector-service wordt uitgevoerd. De service wordt standaard geïnstalleerd op de collector-VM.

    c. Download en installeer VMware PowerCLI.

5.  Doe het volgende in **vCenter Server-details opgeven**:
    - Geef de FQDN-naam of IP-adres van vCenter-Server.
    - In **gebruikersnaam** en **wachtwoord**, geef de referenties van het kenmerk alleen-lezen-account dat door de collector wordt gebruikt voor het detecteren van virtuele machines in VMware vCenter Server.
    - Selecteer in **Select scope** een bereik voor VM-detectie. De collector kan alleen virtuele machines binnen het opgegeven bereik detecteren. U kunt het bereik instellen op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1000 virtuele machines bevatten.

6.  In **migratieproject opgeven**, geeft u de ID en de sleutel voor het project. Als u ze hebt gekopieerd, opent u de Azure-portal van de collector-VM. Op het project tot **overzicht** weergeeft, schakelt **Machines detecteren** en kopieer de waarden.  
7.  In **voortgang van verzamelen weergeven**, het detectieproces bewaken en controleren van de metagegevens die worden verzameld van de virtuele machines zijn binnen het bereik. De collector geeft aan hoe lang de detectie ongeveer zal duren.

#### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

De collector wordt continu profiel van de on-premises omgeving en worden de prestatiegegevens blijven verzenden met een interval dat uur. U kunt de machines in de portal controleren vanaf een uur nadat de detectie is gestart. Het is raadzaam om ten minste een dag te wachten met het maken van evaluaties op basis van prestaties voor de virtuele machines.

1. Klik in het migratieproject op **Beheren** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren in de portal worden weergegeven.

### <a name="data-collected-from-on-premises-environment"></a>Gegevens verzameld van on-premises omgeving

Het collector-apparaat wordt gedetecteerd dat de volgende configuratiegegevens over de geselecteerde virtuele machines.

1. VM-weergavenaam (op vCenter)
2. Pad van de inventaris van de virtuele machine (host/map in vCenter)
3. IP-adres
4. MAC-adres
5. Besturingssysteem
5. Aantal kernen, schijven, NIC 's
6. Grootte van geheugen, schijf-grootten
7. En prestatiemeteritems van de VM, schijf en netwerk, zoals vermeld in de onderstaande tabel.

Het collector-apparaat verzamelt de volgende prestatiemeteritems voor elke virtuele machine van de ESXi-host met een interval van 20 seconden. Deze items zijn prestatiemeteritems van vCenter en hoewel de terminologie aangeeft dat deze gemiddelde, de voorbeelden 20 seconden real-time-prestatiemeteritems zijn. Het apparaat vervolgens rollen van de voorbeelden 20 seconden voor het maken van één gegevenspunt voor elke 15 minuten door de piekwaarde te selecteren in de voorbeelden 20 seconden en verzendt ze naar Azure. De prestatiegegevens voor de virtuele machines wordt gestart steeds beschikbaar in de portal twee uur nadat u de detectie hebt gestart. Het is raadzaam om te wachten op ten minste een dag voor het maken van beoordelingen op basis van prestaties om nauwkeurige juiste formaat aanbevelingen te krijgen. Als u direct resultaat zoekt, kunt u evaluaties maken met het criterium voor het instellen als *zoals on-premises* die wordt geen rekening gehouden met de prestatiegegevens voor de juiste grootte.

**Teller** |  **Gevolgen voor de evaluatie**
--- | ---
cpu.usage.average | Aanbevolen VM-grootte en kosten  
mem.usage.average | Aanbevolen VM-grootte en kosten  
virtualDisk.read.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.write.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.numberReadAveraged.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
virtualDisk.numberWriteAveraged.average | Berekent de grootte van de schijf, de kosten voor gegevensopslag, VM-grootte
NET.Received.Average | Berekent de VM-grootte                          
net.transmitted.average | Berekent de VM-grootte     

> [!WARNING]
> De eenmalige detectiemethode die op vCenter-Server statistiek-instellingen voor het verzamelen van prestatiegegevens vertrouwen is nu verouderd.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [maken van een groep](how-to-create-a-group.md) voor evaluatie.
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.

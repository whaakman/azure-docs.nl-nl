---
title: Azure Migrate gebruiken om on-premises virtuele VMware-machines te detecteren en te evalueren voor migratie naar Azure | Microsoft Docs
description: Beschrijft hoe u met behulp van de service Azure Migrate on-premises virtuele VMware-machines kunt detecteren en evalueren voor migratie naar Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/11/2019
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: 186d532f159db7d51ba4f93a250c00c7627280e0
ms.sourcegitcommit: a512360b601ce3d6f0e842a146d37890381893fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/11/2019
ms.locfileid: "54229521"
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>On-premises virtuele VMware-machines detecteren en beoordelen voor migratie naar Azure

De service [Azure Migrate](migrate-overview.md) beoordeelt on-premises workloads voor migratie naar Azure.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een account maken dat in Azure Migrate wordt gebruikt om de on-premises virtuele machines te detecteren
> * Maak een Azure Migrate-project.
> * Stel een on-premises collector-VM in om on-premises virtuele VMware-machines te detecteren voor evaluatie.
> * Groepeer virtuele machines en maak een evaluatie.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

- **VMware**: de VM’s die u wilt migreren, moeten worden beheerd door vCenter Server van versie 5.5, 6.0 of 6.5. Daarnaast hebt u één ESXi-host met versie 5.0 of hoger nodig om de collector-VM te implementeren.
- **vCenter Server-account**: u hebt een alleen-lezen-account nodig voor toegang tot vCenter Server. Azure Migrate gebruikt dit account om de on-premises virtuele machines te detecteren.
- **Machtigingen**: op vCenter Server hebt u machtigingen nodig om een virtuele machine te maken door een bestand in .OVA-indeling te importeren.

## <a name="create-an-account-for-vm-discovery"></a>Een account voor detectie van virtuele machines maken

Azure Migrate heeft toegang nodig tot de VMware-servers, zodat de virtuele machines automatisch kunnen worden gedetecteerd voor evaluatie. Maak een VMware-account met de volgende eigenschappen. U geeft dit account op tijdens de installatie van Azure Migrate.

- Gebruikerstype: Ten minste een alleen-lezen-gebruiker
- Machtigingen: Datacentrumobject –> doorgeven naar onderliggend object, rol=Alleen-lezen
- Details: Gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.
- Wijs om de toegang te beperken de rol Geen toegang met het object Doorgeven naar onderliggend object toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM's en netwerken).


## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Een project maken

1. Klik in Azure Portal op **Een resource maken**.
2. Zoek naar **Azure Migrate** en selecteer de service **Azure Migrate** in de zoekresultaten. Klik vervolgens op **Maken**.
3. Geef een projectnaam op en het Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de geografie op waarin u het project wilt maken en klik op **Maken**. U kunt een Azure Migrate-project alleen in de volgende geografische gebieden maken. U kunt de migratie echter wel plannen voor elke Azure-doellocatie. De opgegeven geografie voor het project wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines.

**Geografie** | **Opslaglocatie**
--- | ---
Azure Government | VS (overheid) - Virginia
Azië | Azië - zuidoost
Europa | Europa - noord of Europa - west
Verenigde Staten | US - oost of US - west-centraal

![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)


## <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Azure Migrate maakt een on-premises virtuele machine die het collector-apparaat wordt genoemd. Deze virtuele machine detecteert on-premises virtuele VMware-machines en stuurt metagegevens van deze machines naar de service Azure Migrate. Om het collector-apparaat in te stellen, downloadt u een .OVA-bestand en importeert u het op de on-premises vCenter Server om de virtuele machine te maken.

1. Klik in het Azure Migrate-project op **Aan de slag** > **Detecteren en evalueren** > **Machines detecteren**.
2. Klik in **Machines detecteren** op **Downloaden** om het apparaat te downloaden.

    Het Azure Migrate-apparaat communiceert met vCenter Server en profileert continu de on-premises omgeving om realtime gebruiksgegevens voor elke virtuele machine te verzamelen. Het apparaat verzamelt piektellers voor elk metrisch gegeven (CPU-gebruik, geheugengebruik, enzovoort). Dit model is voor het verzamelen van prestatiegegevens niet afhankelijk van de instellingen voor statistieken in vCenter Server. U kunt de continue profilering op elk gewenst moment stoppen vanaf het apparaat.

    > [!NOTE]
    > Ondersteuning van het apparaat voor eenmalige detectie is nu beëindigd omdat deze methode gebaseerd was op statistiekinstellingen van vCenter Server voor de beschikbaarheid van prestatiegegevenspunten en gemiddelde prestatiemeteritems verzamelde, wat leidde tot een te voorzichtige schaling van virtuele machines voor migratie naar Azure.

    **Onmiddellijk resultaat:** als de detectie met het apparaat voor continue detectie voltooid is (wat enkele uren kan duren, afhankelijk van het aantal virtuele machines), kunt u meteen evaluaties gaan maken. Omdat het verzamelen van prestatiegegevens wordt gestart wanneer u detectie activeert, en als u direct resultaat wilt, moet u het schaalcriterium in de evaluatie instellen als *as on-premises*. Voor evaluaties op basis van prestaties is het raadzaam om ten minste een dag te wachten na het activeren van de detectie om betrouwbare aanbevelingen voor de schaal te krijgen.

    Het apparaat verzamelt doorlopend alleen prestatiegegevens, het detecteert niet elke configuratiewijziging in de on-premises omgeving (dat wil zeggen het toevoegen/verwijderen van VM’s, toevoegen van schijven, enzovoort). Als er een configuratiewijziging in de on-premises omgeving is, kunt u het volgende doen om de wijzigingen door te voeren in de portal:

    - Toevoegen van items (virtuele machines, schijven, kernen enz.): om deze wijzigingen in de Azure-portal door te voeren, kunt u de detectie vanaf het apparaat stoppen en opnieuw starten. Dit zorgt ervoor dat de wijzigingen worden bijgewerkt in het Azure Migrate-project.

    - Verwijderen van VM’s: vanwege de manier waarop het apparaat is ontworpen, wordt het verwijderen van VM’s niet doorgevoerd, zelfs niet als u de detectie stopt en opnieuw start. Dit komt doordat gegevens uit volgende detecties worden toegevoegd aan de oudere detecties en niet worden overschreven. In dit geval kunt u eenvoudigweg de VM in de portal negeren door deze uit uw groep te verwijderen en de evaluatie opnieuw te berekenen.


3. Kopieer de project-id en -sleutel uit **Projectreferenties kopiëren**. U hebt deze nodig tijdens de configuratie van collector.

    ![.OVA-bestand downloaden](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Controleer of het .OVA-bestand veilig is voordat u het implementeert.

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.
2. Gebruik de volgende opdracht om de hash voor het OVA-bestand te genereren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen.

#### <a name="continuous-discovery"></a>Continue detectie

  Voor OVA-versie 1.0.10.9

  **Algoritme** | **Hash-waarde**
  --- | ---
  MD5 | 169f6449cc1955f1514059a4c30d138b
  SHA1 | f8d0a1d40c46bbbf78cd0caa594d979f1b587c8f
  SHA256 | d68fe7d94be3127eb35dd80fc5ebc60434c8571dcd0e114b87587f24d6b4ee4d

  Voor OVA-versie 1.0.10.4

  **Algoritme** | **Hash-waarde**
  --- | ---
  MD5 | 2ca5b1b93ee0675ca794dd3fd216e13d
  SHA1 | 8c46a52b18d36e91daeae62f412f5cb2a8198ee5
  SHA256 | 3b3dec0f995b3dd3c6ba218d436be003a687710abab9fcd17d4bdc90a11276be


#### <a name="one-time-discovery-deprecated-now"></a>Eenmalige detectie (nu beëindigd)

Dit model is nu beëindigd, er wordt gezorgd voor ondersteuning van bestaande apparaten.

  Voor OVA-versie 1.0.9.15

  **Algoritme** | **Hash-waarde**
  --- | ---
  MD5 | e9ef16b0c837638c506b5fc0ef75ebfa
  SHA1 | 37b4b1e92b3c6ac2782ff5258450df6686c89864
  SHA256 | 8a86fc17f69b69968eb20a5c4c288c194cdcffb4ee6568d85ae5ba96835559ba

  Voor OVA-versie 1.0.9.14

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

## <a name="create-the-collector-vm"></a>De collector-VM maken

Importeer het gedownloade bestand naar de vCenter Server.

1. Klik in de Client vSphere-console op **Bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. Geef in de wizard OVF-sjabloon implementeren > **Bron** de locatie van het .OVA-bestand op.
3. Geef in **Naam** en **Locatie** een beschrijvende naam op voor de collector-VM, en het inventarisobject waarin de virtuele machine wordt gehost.
5. Geef in **Host/Cluster** de host of het cluster op waarop de collector-VM wordt uitgevoerd.
7. Geef in de opslag de opslaglocatie voor de collector-VM op.
8. Geef in **Schijfindeling** het schijftype en de schijfgrootte op.
9. Geef in **Netwerktoewijzing** het netwerk op waarmee de collector-VM verbinding maakt. Het netwerk moet verbinding hebben met internet om metagegevens te kunnen verzenden naar Azure.
10. Controleer en bevestig de instellingen en klik op **Voltooien**.

## <a name="run-the-collector-to-discover-vms"></a>De collector uitvoeren om virtuele machines te detecteren

1. Klik in de vSphere Client-console met de rechtermuisknop op de VM > **Console openen**.
2. Geef de voorkeursinstellingen voor de taal, de tijdzone en wachtwoorden op voor het apparaat.
3. Klik op het bureaublad op de snelkoppeling **Collector uitvoeren**.
4. Klik in de bovenste balk van de gebruikersinterface van de collector op **Controleren op updates** en controleer of de nieuwste versie van de collector wordt uitgevoerd. Als dat niet het geval is, kunt u het nieuwste updatepakket downloaden via de koppeling en de collector bijwerken.
5. Open in de Azure Migrate Collector het onderdeel **Vereisten instellen**.
    - Selecteer de Azure-cloud die u wilt migreren (Azure Global of Azure Government).
    - Accepteer de licentievoorwaarden en lees de informatie van derden.
    - De collector controleert of de virtuele machine toegang heeft tot internet.
    - Als de virtuele machine via een proxy toegang heeft tot internet, klikt u op **Proxyinstellingen** en geeft u het proxyadres en de controlepoort op. Geef referenties op als de proxy verificatie nodig heeft. Lees [hier](https://docs.microsoft.com/azure/migrate/concepts-collector#collector-prerequisites) meer over de vereisten voor internetconnectiviteit en de [lijst met URL's](https://docs.microsoft.com/azure/migrate/concepts-collector#connect-to-urls) die de collector raadpleegt.

      > [!NOTE]
      > Het proxyadres moet worden ingevoerd in het formulier http://ProxyIPAddress of http://ProxyFQDN. Alleen HTTP-proxy wordt ondersteund. Als u een onderscheppende proxy hebt, kan de internetverbinding in eerste instantie mislukken als u het proxycertificaat niet hebt geïmporteerd. [Hier](https://docs.microsoft.com/azure/migrate/concepts-collector#internet-connectivity-with-intercepting-proxy) vindt u meer informatie over hoe u dit kunt oplossen door het proxycertificaat te importeren als vertrouwd in de collector-VM.

    - De collector controleert of de collector-service wordt uitgevoerd. De service wordt standaard geïnstalleerd op de collector-VM.
    - Download en installeer VMware PowerCLI.

6. Doe het volgende in **vCenter Server-details opgeven**:
    - Geef de naam (FQDN) of het IP-adres op van de vCenter Server.
    - Geef in **User name** en **Password** de alleen-lezen accountreferenties op die de collector gebruikt om virtuele machines op de vCenter Server te detecteren.
    - Selecteer in **Collection scope** een bereik voor VM-detectie. De collector kan alleen virtuele machines detecteren binnen het opgegeven bereik. U kunt het bereik instellen op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1500 virtuele machines bevatten. Lees [hier](how-to-scale-assessment.md) meer over hoe u een grotere omgeving kunt detecteren.

7. Geef in **Specify migration project** de Azure Migrate project-id en -sleutel op die u hebt gekopieerd in de portal. Als u deze niet hebt gekopieerd, opent u Azure Portal vanuit de collector-VM. Klik op de **overzichtspagina** van het project op **Machines detecteren** en kopieer de waarden.  
8. In **Voortgang van verzamelen weergeven** kunt u de detectiestatus controleren. Lees [hier](https://docs.microsoft.com/azure/migrate/concepts-collector#what-data-is-collected) meer over welke gegevens worden verzameld door de collector Azure Migrate.

> [!NOTE]
> De collector ondersteunt alleen 'Engels (Verenigde Staten)' als de taal van het besturingssysteem en de taal van de gebruikersinterface van de collector.
> Als u de instellingen wijzigt op een computer die u wilt evalueren, activeert u ontdekken opnieuw voordat u de evaluatie uitvoert. Gebruik in de collector hiervoor de optie **Start collection again**. Nadat de verzameling gereed is, selecteert u in de portal de optie **Opnieuw berekenen** voor de evaluatie om de evaluatieresultaten op te halen.


### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

De collector profileert de on-premises omgeving continu en verzendt voortdurend prestatiegegevens met een interval van een uur. U kunt de machines in de portal bekijken vanaf een uur nadat de detectie is gestart.

1. Klik in het migratieproject op **Beheren** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren in de portal worden weergegeven.


## <a name="create-and-view-an-assessment"></a>Een evaluatie maken en weergeven

Nadat virtuele machines in de portal zijn gedetecteerd, kunt u ze groeperen en een evaluatie maken. U kunt onmiddellijk 'as on-premises'-evaluaties maken zodra de virtuele machines in de portal zijn gedetecteerd. Het is raadzaam om ten minste een dag te wachten met het maken van evaluaties op basis van prestaties om betrouwbare aanbevelingen te krijgen.

1. Klik op de **overzichtspagina** van het project op **+Evaluatie maken**.
2. Klik op **Alles weergeven** om de evaluatie-eigenschappen te controleren.
3. Maak de groep en geef een groepsnaam op.
4. Selecteer de machines die u aan de groep wilt toevoegen.
5. Klik op **Evaluatie maken** om de groep en de evaluatie te maken.
6. Nadat de evaluatie is gemaakt, kunt u deze bekijken in **Overzicht** > **Dashboard**.
7. Klik op **Evaluatie exporteren** om deze te downloaden als een Excel-bestand.

> [!NOTE]
> Het is raadzaam om na het starten van detectie ten minste een dag te wachten met het maken van een evaluatie. Als u een bestaande evaluatie wilt bijwerken met de meest recente prestatiegegevens, kunt u de opdracht **Opnieuw berekenen** voor de evaluatie gebruiken om deze bij te werken.

### <a name="assessment-details"></a>Evaluatiedetails

Een evaluatie bevat de volgende informatie: of de on-premises VM's compatibel zijn met Azure, wat de juiste grootte voor elke VM is voor uitvoering in Azure en wat de geschatte maandelijkse kosten voor Azure zijn.

![Evaluatierapport](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-gereedheid

De weergave van de Azure-gereedheid in de evaluatie toont de gereedheidsstatus van alle VM's. Afhankelijk van de eigenschappen van de VM kan elke VM worden gemarkeerd als:
- Gereed voor Azure
- Voorwaardelijk gereed voor Azure
- Niet gereed voor Azure
- Gereedheid onbekend

Voor virtuele machines die gereed zijn, wordt door Azure Migrate een VM-grootte in Azure aanbevolen. De aanbeveling voor de grootte die Azure Migrate heeft gedaan, is afhankelijk van het criterium voor het instellen van de grootte dat is opgegeven in de evaluatie-eigenschappen. Als het criterium voor het instellen van de grootte is gebaseerd op de prestaties, wordt de aanbeveling voor de grootte gedaan op basis van de prestatiegeschiedenis van de VM's (CPU en geheugen) en schijven (IOPS en doorvoer). Als het criterium voor het instellen van de grootte 'Zoals on-premises' is, houdt Azure Migrate geen rekening met de prestatiegegevens van de virtuele machine en de schijven. De aanbeveling voor de VM-grootte in Azure wordt gebaseerd op de grootte van de on-premises VM. De aanbeveling voor de grootte van de schijf wordt bepaald op basis van het type opslag dat is opgegeven bij de evaluatie-eigenschappen (de standaardwaarde is Premium-schijven). [Meer informatie](concepts-assessment-calculation.md) over het instellen van de grootte in Azure Migrate.

Voor VM's die niet gereed of voorwaardelijk gereed zijn voor Azure, legt Azure Migrate de gereedheidsproblemen uit en biedt het herstelstappen aan.

VM's waarvoor Azure Migrate de Azure-gereedheid niet kan vaststellen (omdat er geen gegevens beschikbaar zijn), worden gemarkeerd als ‘gereedheid onbekend’.

Het vaststellen van de Azure-gereedheid en het instellen van de grootte is niet het enige wat Azure Migrate doet. Het stelt ook hulpprogramma’s voor die u kunt gebruiken om de VM te migreren. Hiervoor is een uitgebreidere detectie van de on-premises omgeving vereist. [Meer informatie](how-to-get-migration-tool.md) over hoe u een uitgebreidere detectie kunt uitvoeren door agents op de on-premises machines te installeren. Als de agents zijn niet geïnstalleerd op de on-premises machines, wordt een lift-and-shift-migratie met behulp van [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview) aanbevolen. Als de agents op de on-premises machine zijn geïnstalleerd, wordt in Azure Migrate gekeken naar de processen die op de machine worden uitgevoerd en wordt bepaald of de machine een databasemachine is. Als de machine een databasemachine is, wordt [Azure Database Migration Service](https://docs.microsoft.com/azure/dms/dms-overview) aanbevolen. Anders wordt Azure Site Recovery aanbevolen als hulpprogramma voor migratie.

  ![Gereedheid evalueren](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Schatting van maandelijkse kosten

Deze weergave toont de totale compute- en opslagkosten om de virtuele machines in Azure uit te voeren, evenals de details per VM. De geschatte kosten worden berekend op basis van de aanbevelingen voor de grootte die Azure Migrate geeft voor een machine, en de evaluatie-eigenschappen.

> [!NOTE]
> De kostenschatting die door Azure Migrate wordt verstrekt, geldt voor het uitvoeren van de on-premises VM's als een Azure IaaS-VM (Infrastructure as a service). Azure Migrate houdt geen rekening met PaaS- of SaaS-kosten (Platform as a service of Software as a service).

Geschatte maandelijkse kosten voor computing en opslag worden samengevoegd voor alle virtuele machines in de groep.

![Evaluatie-kosten per VM](./media/tutorial-assessment-vmware/assessment-vm-cost.png)

#### <a name="confidence-rating"></a>Betrouwbaarheidsclassificatie

Elke prestatie-evaluatie in Azure Migrate wordt gekoppeld aan een betrouwbaarheidsclassificatie van 1 ster tot 5 sterren (1 ster is de laagste score en 5 sterren de hoogste). De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen. De betrouwbaarheidsclassificatie van een evaluatie helpt u om de betrouwbaarheid in te schatten van de aanbevelingen voor de grootte die Azure Migrate geeft. Betrouwbaarheidsclassificatie is niet van toepassing op 'as-is' on-premises evaluaties.

Als u de grootte instelt op basis van de prestaties, heeft Azure Migrate de gebruiksgegevens nodig van de CPU en het geheugen van de virtuele machine. Voor elke schijf die aan de virtuele machine is gekoppeld, zijn de IOPS-gegevens van de schijf en de doorvoergegevens vereist. Ook heeft Azure Migrate voor iedere netwerkadapter die aan een VM is gekoppeld, gegevens over het inkomende/uitgaande netwerkverkeer nodig om de grootte in te kunnen stellen op basis van de prestaties. Als een of meer van de bovenstaande gebruiksgetallen niet beschikbaar zijn in vCenter Server, is de aanbeveling voor de grootte die Azure Migrate doet, mogelijk niet betrouwbaar. De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten, zoals hieronder wordt weergegeven:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

Het kan voorkomen dat niet alle gegevenspunten beschikbaar zijn voor een evaluatie. Dit kan de volgende oorzaken hebben:

- U hebt uw omgeving niet geprofileerd gedurende de periode waarvoor u de evaluatie maakt. Bijvoorbeeld, als u de evaluatie maakt waarbij de duur van de prestaties is ingesteld op 1 dag, moet u na het starten van de detectie minimaal een dag wachten voordat alle gegevenspunten zijn verzameld.

- Er zijn enkele VM's uitgeschakeld geweest in de periode waarover de evaluatie wordt berekend. Als er VM's zijn die gedurende een bepaalde periode uitgeschakeld zijn geweest, kunnen er voor die periode geen prestatiegegevens worden verzameld.

- Er zijn enkele VM's gemaakt tijdens de periode waarover de evaluatie wordt berekend. Als u bijvoorbeeld een evaluatie maakt voor de prestatiegeschiedenis van de laatste maand, maar er een week geleden enkele VM's in de omgeving zijn gemaakt. In dergelijke gevallen is de prestatiegeschiedenis van de nieuwe virtuele machines niet voor de hele periode beschikbaar.

> [!NOTE]
> Als de betrouwbaarheidsclassificatie van een evaluatie lager is dan 5 sterren, wacht dan minimaal een dag tot het apparaat de omgeving heeft geprofileerd en *bereken de evaluatie dan opnieuw*. Als de voorgaande stappen niet kunnen worden uitgevoerd, wordt de grootte mogelijk niet op betrouwbare wijze ingesteld. In dat geval doet u er verstandig aan over te schakelen naar *instelling van de grootte op basis van 'zoals on-premises'* door de evaluatie-eigenschappen te wijzigen.

## <a name="next-steps"></a>Volgende stappen

- Lees [hier](how-to-modify-assessment.md) meer over het aanpassen van een beoordeling op basis van uw vereisten.
- Meer informatie over het maken van uiterst betrouwbare evaluatiegroepen met behulp van [toewijzing van VM-afhankelijkheid](how-to-create-group-machine-dependencies.md)
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
- [Meer informatie over](how-to-scale-assessment.md) het detecteren en evalueren van een grote VMware-omgeving.
- [Lees](resources-faq.md) de veelgestelde vragen over Azure Migrate.

---
title: Azure Migrate gebruiken om on-premises virtuele VMware-machines te detecteren en te evalueren voor migratie naar Azure | Microsoft Docs
description: Beschrijft hoe u met behulp van de service Azure Migrate on-premises virtuele VMware-machines kunt detecteren en evalueren voor migratie naar Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 02/27/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: d70b4ea2d45c38fa53ab3c00f76c00ef6f3d7663
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
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
 
> [!NOTE]
> Ondersteuning voor Hyper-V staat in de roadmap en komt binnenkort beschikbaar. 

- **vCenter Server-account**: u hebt een alleen-lezen-account nodig voor toegang tot de vCenter Server. Azure Migrate gebruikt dit account om de on-premises virtuele machines te detecteren.
- **Machtigingen**: op de vCenter Server hebt u machtigingen nodig om een virtuele machine te maken door een bestand in .OVA-indeling te importeren. 
- **Instellingen voor statistieken**: de instellingen voor statistieken voor de vCenter Server moeten worden ingesteld op niveau 3 voordat u de implementatie begint. Als ze lager zijn dan niveau 3, werkt de evaluatie wel, maar worden de prestatiegegevens voor opslag en netwerk niet verzameld. De aanbeveling van de grootte wordt in dit geval gebaseerd op prestatiegegevens voor CPU en geheugen en configuratiegegevens voor schijf en netwerkadapters. 

## <a name="create-an-account-for-vm-discovery"></a>Een account voor detectie van virtuele machines maken

Azure Migrate heeft toegang nodig tot de VMware-servers, zodat de virtuele machines automatisch kunnen worden gedetecteerd voor evaluatie. Maak een VMware-account met de volgende eigenschappen. U geeft dit account op tijdens de installatie van Azure Migrate.

- Gebruikerstype: ten minste een alleen-lezen-gebruiker
- Machtigingen: Datacentrumobject –> Doorgeven naar onderliggend object, rol=Alleen-lezen
- Details: gebruiker wordt toegewezen op datacentrumniveau, en heeft toegang tot alle objecten in het datacentrum.
- Wijs om de toegang te beperken de rol Geen toegang met het object Doorgeven naar onderliggend object toe aan de onderliggende objecten (vSphere-hosts, gegevensopslag, VM's en netwerken).

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Een project maken

1. Klik in Azure Portal op **Een resource maken**.
2. Zoek naar **Azure Migrate** en selecteer de service **Azure Migrate** in de zoekresultaten. Klik vervolgens op **Maken**.
3. Geef een projectnaam op en het Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de locatie op waar u het project wilt maken en klik op **Maken**. U kunt alleen een Azure Migrate-project maken in de regio West-centraal VS of VS - oost. U kunt de migratie echter wel plannen voor elke Azure-doellocatie. De opgegeven locatie voor het project wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Het collector-apparaat downloaden

Azure Migrate maakt een on-premises virtuele machine die het collector-apparaat wordt genoemd. Deze virtuele machine detecteert on-premises virtuele VMware-machines en stuurt metagegevens van deze machines naar de service Azure Migrate. Om het collector-apparaat in te stellen, downloadt u een .OVA-bestand en importeert u het op de on-premises vCenter Server om de virtuele machine te maken.

1. Klik in het Azure Migrate-project op **Aan de slag** > **Detecteren en evalueren** > **Machines detecteren**.
2. Klik in **Machines detecteren** op **Downloaden** om het .OVA-bestand te downloaden.
3. Kopieer de project-id en -sleutel uit **Projectreferenties kopiëren**. U hebt deze nodig tijdens de configuratie van collector.

    ![.OVA-bestand downloaden](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Het collector-apparaat verifiëren

Controleer of het .OVA-bestand veilig is voordat u het implementeert.

1. Open op de machine waarop u het bestand hebt gedownload een opdrachtvenster voor beheerders.
2. Gebruik de volgende opdracht om de hash voor het OVA-bestand te genereren:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Gebruiksvoorbeeld: ```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen.
    
    Voor OVA-versie 1.0.9.7

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | d5b6a03701203ff556fa78694d6d7c35
    SHA1 | f039feaa10dccd811c3d22d9a59fb83d0b01151e
    SHA256 | e5e997c003e29036f62bf3fdce96acd4a271799211a84b34b35dfd290e9bea9c
    
    Voor OVA-versie 1.0.9.5

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | fb11ca234ed1f779a61fbb8439d82969
    SHA1 | 5bee071a6334b6a46226ec417f0d2c494709a42e
    SHA256 | b92ad637e7f522c1d7385b009e7d20904b7b9c28d6f1592e8a14d88fbdd3241c  
    
    Voor OVA-versie 1.0.9.2

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | 7326020e3b83f225b794920b7cb421fc
    SHA1 | a2d8d496fdca4bd36bfa11ddf460602fa90e30be
    SHA256 | f3d9809dd977c689dda1e482324ecd3da0a6a9a74116c1b22710acc19bea7bb2  
    
    Voor OVA-versie 1.0.8.59

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | 71139e24a532ca67669260b3062c3dad
    SHA1 | 1bdf0666b3c9c9a97a07255743d7c4a2f06d665e
    SHA256 | 6b886d23b24c543f8fc92ff8426cd782a77efb37750afac397591bda1eab8656  

    Voor OVA-versie 1.0.8.49
    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | cefd96394198b92870d650c975dbf3b8 
    SHA1 | 4367a1801cf79104b8cd801e4d17b70596481d6f
    SHA256 | fda59f076f1d7bd3ebf53c53d1691cc140c7ed54261d0dc4ed0b14d7efef0ed9

    Voor OVA-versie 1.0.8.40:

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

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
4. Open in de Azure Migrate Collector het onderdeel **Vereisten instellen**.
    - Accepteer de licentievoorwaarden en lees de informatie van derden.
    - De collector controleert of de virtuele machine toegang heeft tot internet.
    - Als de virtuele machine via een proxy toegang heeft tot internet, klikt u op **Proxyinstellingen** en geeft u het proxyadres en de controlepoort op. Geef referenties op als de proxy verificatie nodig heeft.

    > [!NOTE]
    > Het proxyadres moet worden ingevoerd in het formulier http://ProxyIPAddress of http://ProxyFQDN. Alleen HTTP-proxy wordt ondersteund.

    - De collector controleert of de collector-service wordt uitgevoerd. De service wordt standaard geïnstalleerd op de collector-VM.
    - Download en installeer de VMware PowerCLI.

5. Doe het volgende in **vCenter Server-details opgeven**:
    - Geef de naam (FQDN) of het IP-adres op van de vCenter Server.
    - Geef in **User name** en **Password** de alleen-lezen accountreferenties op die de collector gebruikt om virtuele machines op de vCenter Server te detecteren.
    - Selecteer in **Collection scope** een bereik voor VM-detectie. De collector kan alleen virtuele machines detecteren binnen het opgegeven bereik. U kunt het bereik instellen op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1000 virtuele machines bevatten. 

6. Geef in **Specify migration project** de Azure Migrate project-id en -sleutel op die u hebt gekopieerd in de portal. Als u deze niet hebt gekopieerd, opent u Azure Portal vanuit de collector-VM. Klik op de **overzichtspagina** van het project op **Machines detecteren** en kopieer de waarden.  
7. In **View collection progress** bekijkt u de detectie en controleert u of metagegevens die vanuit de virtuele machines worden verzameld, zich binnen het bereik bevinden. De collector geeft aan hoe lang de detectie ongeveer zal duren.

> [!NOTE]
> De collector ondersteunt alleen 'Engels (Verenigde Staten)' als de taal van het besturingssysteem en de taal van de gebruikersinterface van de collector. Ondersteuning voor meer talen is binnenkort beschikbaar.


### <a name="verify-vms-in-the-portal"></a>VM's verifiëren in de portal

De detectietijd is afhankelijk van het aantal virtuele machines dat u detecteert. Nadat de collector is uitgevoerd, duurt het voor 100 virtuele machines gewoonlijk ongeveer een uur voordat de detectie is voltooid. 

1. Klik in het Migration Planner-project op **Manage** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren in de portal worden weergegeven.


## <a name="create-and-view-an-assessment"></a>Een evaluatie maken en weergeven

Nadat virtuele machines zijn gedetecteerd, kunt u ze groeperen en een evaluatie maken. 

1. Klik op de **overzichtspagina** van het project op **+Evaluatie maken**.
2. Klik op **Alles weergeven** om de evaluatie-eigenschappen te controleren.
3. Maak de groep en geef een groepsnaam op.
4. Selecteer de machines die u aan de groep wilt toevoegen.
5. Klik op **Evaluatie maken** om de groep en de evaluatie te maken.
6. Nadat de evaluatie is gemaakt, kunt u deze bekijken in **Overzicht** > **Dashboard**.
7. Klik op **Evaluatie exporteren** om deze te downloaden als een Excel-bestand.

### <a name="assessment-details"></a>Evaluatiedetails

Een evaluatie bevat de volgende informatie: of de on-premises VM's compatibel zijn met Azure, wat de juiste grootte voor elke VM is voor uitvoering in Azure en wat de geschatte maandelijkse kosten voor Azure zijn. 

![Evaluatierapport](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-gereedheid

De weergave van de Azure-gereedheid in de evaluatie toont de gereedheidsstatus van alle VM's. Afhankelijk van de eigenschappen van de VM kan elke VM worden gemarkeerd als:
- Gereed voor Azure
- Voorwaardelijk gereed voor Azure
- Niet gereed voor Azure
- Gereedheid onbekend 

Voor virtuele machines die gereed zijn, wordt door Azure Migrate een VM-grootte in Azure aanbevolen. De aanbeveling voor de grootte die Azure Migrate heeft gedaan, is afhankelijk van het criterium voor het instellen van de grootte dat is opgegeven in de evaluatie-eigenschappen. Als het criterium voor het instellen van de grootte is gebaseerd op de prestaties, wordt de aanbeveling voor de grootte gedaan op basis van de prestatiegeschiedenis van de VM's. Als het criterium voor het instellen van de grootte 'zoals on-premises' is, wordt de aanbeveling gedaan door te kijken naar de grootte van de on-premises VM (ongewijzigde grootte). In dat geval worden de gebruiksgegevens buiten beschouwing gelaten. [Meer informatie](concepts-assessment-calculation.md) over het instellen van de grootte in Azure Migrate. 

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

Elke evaluatie in Azure Migrate wordt gekoppeld aan een betrouwbaarheidsclassificatie van 1 ster tot 5 sterren (1 ster is de laagste score en 5 sterren de hoogste). De betrouwbaarheidsclassificatie wordt aan een evaluatie toegewezen op basis van de beschikbaarheid van de gegevenspunten die nodig zijn om de evaluatie te berekenen. De betrouwbaarheidsclassificatie van een evaluatie helpt u om de betrouwbaarheid in te schatten van de aanbevelingen voor de grootte die Azure Migrate geeft. 

Betrouwbaarheidsclassificatie is nuttig als u *de grootte instelt op basis van prestaties*, omdat in Azure Migrate mogelijk niet voldoende gegevenspunten zijn om de grootte in te stellen op basis van gebruik. Voor *het instellen van de grootte op basis van 'zoals on-premises'* is de betrouwbaarheidsclassificatie altijd 5 sterren, omdat Azure Migrate alle benodigde gegevenspunten heeft om de grootte van de VM in te stellen. 

Als u de grootte van de VM instelt op basis van de prestaties, heeft Azure Migrate de gebruiksgegevens nodig voor de CPU en het geheugen. Voor elke schijf die aan de VM is gekoppeld, is de IOPS voor lezen/schrijven en doorvoer vereist. Ook heeft Azure Migrate voor iedere netwerkadapter die aan de VM is gekoppeld, gegevens over het inkomende/uitgaande netwerkverkeer nodig om de grootte in te kunnen stellen op basis van de prestaties. Als een of meer van de bovenstaande gebruiksgetallen niet beschikbaar zijn in vCenter Server, is de aanbeveling voor de grootte die Azure Migrate doet, mogelijk niet betrouwbaar. De betrouwbaarheidsclassificatie van de evaluatie wordt toegekend op basis van het percentage beschikbare gegevenspunten:

   **Beschikbaarheid van gegevenspunten** | **Betrouwbaarheidsclassificatie**
   --- | ---
   0%-20% | 1 ster
   21%-40% | 2 sterren
   41%-60% | 3 sterren
   61%-80% | 4 sterren
   81%-100% | 5 sterren

Het kan voorkomen dat niet alle gegevenspunten beschikbaar zijn voor een evaluatie. Dit kan de volgende oorzaken hebben:
- De instelling voor statistieken in vCenter Server is niet op niveau 3 ingesteld en de evaluatie heeft instelling van de grootte op basis van de prestaties als criterium voor het instellen van de grootte. Als de instelling voor statistieken in vCenter Server op een lager niveau dan niveau 3 is ingesteld, worden de prestatiegegevens voor de schijf en het netwerk niet verzameld vanuit vCenter Server. In dat geval wordt de aanbeveling van Azure Migrate voor de schijf en het netwerk niet gebaseerd op gebruik. Voor opslag is de aanbeveling van Azure Migrate gebruik te maken van standaardschijven. Als geen rekening wordt gehouden met de IOPS/doorvoer van de schijf, kan in Azure Migrate niet worden bepaald of voor de schijf een premium-schijf in Azure nodig is.
- De instelling voor statistieken in vCenter Server is korte tijd op niveau 3 ingesteld geweest voordat de detectie begon. Laten we het scenario als voorbeeld nemen waarin u het niveau van de instelling voor statistieken vandaag bijvoorbeeld op 3 zet en morgen (na 24 uur) met de detectie begint met behulp van het collector-apparaat. Als u een evaluatie voor één dag maakt, hebt u alle gegevenspunten en is de betrouwbaarheidsclassificatie van de evaluatie 5 sterren. Maar als u de duur van de prestaties in de evaluatie-eigenschappen wijzigt in één maand, gaat de betrouwbaarheidsclassificatie omlaag, omdat de prestatiegegevens voor schijven en netwerken voor de laatste maand niet beschikbaar zijn. Als u rekening wilt houden met de prestatiegegevens voor de laatste maand, kunt u het beste de instelling voor statistieken in vCenter Server op niveau 3 laten staan gedurende één maand voordat u met de detectie begint. 
- Er zijn enkele VM's uitgeschakeld geweest in de periode waarover de evaluatie wordt berekend. Als er VM's zijn die gedurende een bepaalde tijd uitgeschakeld zijn geweest, zal vCenter Server voor die periode geen prestatiegegevens hebben. 
- Er zijn enkele VM's gemaakt tijdens de periode waarover de evaluatie wordt berekend. Als u bijvoorbeeld een evaluatie maakt voor de prestatiegeschiedenis van de laatste maand, maar er een week geleden enkele VM's in de omgeving zijn gemaakt. In dergelijke gevallen is de prestatiegeschiedenis van de nieuwe virtuele machines niet voor de hele periode beschikbaar.

> [!NOTE]
> Als de betrouwbaarheidsclassificatie van een evaluatie lager dan 4 sterren is, raden we u aan om de instelling voor statistieken in vCenter Server op 3 te zetten, te wachten gedurende de periode waarover u de evaluatie wilt uitvoeren (1 dag/1 week/1 maand) en vervolgens een detectie en evaluatie uit te voeren. Als de voorgaande stappen niet kunnen worden uitgevoerd, wordt de grootte mogelijk niet op betrouwbare wijze ingesteld. In dat geval doet u er verstandig aan over te schakelen naar *instelling van de grootte op basis van 'zoals on-premises'* door de evaluatie-eigenschappen te wijzigen.
 
## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over](how-to-scale-assessment.md) het detecteren en evalueren van een grote VMware-omgeving.
- Meer informatie over het maken van uiterst betrouwbare evaluatiegroepen met behulp van [toewijzing van VM-afhankelijkheid](how-to-create-group-machine-dependencies.md)
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.

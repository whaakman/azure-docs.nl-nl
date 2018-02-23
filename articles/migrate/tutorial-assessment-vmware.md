---
title: Azure Migrate gebruiken om on-premises virtuele VMware-machines te detecteren en te evalueren voor migratie naar Azure | Microsoft Docs
description: Beschrijft hoe u met behulp van de service Azure Migrate on-premises virtuele VMware-machines kunt detecteren en evalueren voor migratie naar Azure.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.custom: mvc
ms.openlocfilehash: bbcfe95f5427681f8d55d647b102d35fc37f15ee
ms.sourcegitcommit: 95500c068100d9c9415e8368bdffb1f1fd53714e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>On-premises virtuele VMware-machines detecteren en beoordelen voor migratie naar Azure

De service [Azure Migrate](migrate-overview.md) beoordeelt on-premises workloads voor migratie naar Azure.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een Azure Migrate-project.
> * Stel een on-premises collector-VM in om on-premises virtuele VMware-machines te detecteren voor evaluatie.
> * Groepeer virtuele machines en maak een evaluatie.


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prerequisites"></a>Vereisten

- **VMware**: de virtuele machines die u wilt migreren, moeten worden beheerd door een vCenter Server met versie 5.5, 6.0 of 6.5. Daarnaast hebt u één ESXi-host met versie 5.0 of hoger nodig om de collector-VM te implementeren. 
 
> [!NOTE]
> Ondersteuning voor Hyper-V staat in onze roadmap en komt binnenkort beschikbaar. 

- **vCenter Server-account**: u hebt een alleen-lezen-account nodig voor toegang tot de vCenter Server. Azure Migrate gebruikt dit account om de on-premises virtuele machines te detecteren.
- **Machtigingen**: op de vCenter Server hebt u machtigingen nodig om een virtuele machine te maken door een bestand in .OVA-indeling te importeren. 
- **Instellingen voor statistieken**: de instellingen voor statistieken voor de vCenter Server moeten worden ingesteld op niveau 3 voordat u de implementatie begint. Als ze lager zijn dan niveau 3, werkt de evaluatie wel, maar worden de prestatiegegevens voor opslag en netwerk niet verzameld. De aanbeveling van de grootte wordt in dit geval gebaseerd op prestatiegegevens voor CPU en geheugen en configuratiegegevens voor schijf en netwerkadapters. 

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Een project maken

1. Klik in Azure Portal op **Een resource maken**.
2. Zoek naar **Azure Migrate** en selecteer de service **Azure Migrate (preview)** in de zoekresultaten. Klik vervolgens op **Maken**.
3. Geef een projectnaam op en het Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de locatie op waar u het project wilt maken en klik op **Maken**. Voor deze preview kunt u een Azure Migrate-project alleen maken in de regio West-centraal VS. U kunt de migratie echter wel plannen voor elke Azure-doellocatie. De opgegeven locatie voor het project wordt alleen gebruikt om de metagegevens op te slaan die zijn verzameld van on-premises virtuele machines. 

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
2. Geef de taal, de tijdzone en het wachtwoord op voor het apparaat.
3. Klik op het bureaublad op de snelkoppeling **Collector uitvoeren**.
4. Open in de Azure Migrate Collector het onderdeel **Vereisten instellen**.
    - Accepteer de licentievoorwaarden en lees de informatie van derden.
    - De collector controleert of de virtuele machine toegang heeft tot internet.
    - Als de virtuele machine via een proxy toegang heeft tot internet, klikt u op **Proxyinstellingen** en geeft u het proxyadres en de controlepoort op. Geef referenties op als de proxy verificatie nodig heeft.

    > [!NOTE]
    > Het proxyadres moet worden ingevoerd in de vorm http://IP-adres van proxy of http://FQDN van proxy. Alleen HTTP-proxy wordt ondersteund.

    - De collector controleert of de collectorservice wordt uitgevoerd. De service wordt standaard geïnstalleerd op de collector-VM.
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

### <a name="sample-assessment"></a>Voorbeeldevaluatie

Hier volgt een voorbeeld van een evaluatierapport. Hierin wordt aangegeven of VM's compatibel zijn met Azure en wat de geschatte maandelijkse kosten zijn. 

![Evaluatierapport](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-gereedheid

Deze weergave toont de gereedheidsstatus van elke computer.

- Voor virtuele machines die gereed zijn, wordt door Azure Migrate een VM-grootte in Azure aanbevolen.
- Voor virtuele machines die niet gereed zijn, ziet u de reden daarvoor en worden door Azure Migrate oplossingen gegeven.
- Azure Migrate stelt de hulpprogramma's voor die u voor de migratie kunt gebruiken. Als de computer geschikt is voor lift- en shift-migratie, wordt de Azure Site Recovery Service aanbevolen. Als het een databasecomputer betreft, wordt de Azure Database Migration Service aanbevolen.

  ![Gereedheid evalueren](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Schatting van maandelijkse kosten

Deze weergave toont de totale compute- en opslagkosten om de virtuele machines in Azure uit te voeren, evenals de details per VM. De geschatte kosten worden berekend met behulp van de op prestaties gebaseerde aanbevolen grootte voor een machine en de schijven, en de evaluatie-eigenschappen. 

> [!NOTE]
> De kostenschatting die door Azure Migrate wordt verstrekt, geldt voor het uitvoeren van de on-premises VM's als een Azure IaaS-VM (Infrastructure as a service). Azure Migrate houdt geen rekening met PaaS- of SaaS-kosten (Platform as a service of Software as a service). 

Geschatte maandelijkse kosten voor computing en opslag worden samengevoegd voor alle virtuele machines in de groep. 

![Evaluatie-kosten per VM](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

U kunt inzoomen om de details voor een specifieke computer te bekijken.

![Evaluatie-kosten per VM](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over](how-to-scale-assessment.md) het detecteren en evalueren van een grote VMware-omgeving.
- Meer informatie over het maken van uiterst betrouwbare evaluatiegroepen met behulp van [toewijzing van VM-afhankelijkheid](how-to-create-group-machine-dependencies.md)
- [Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.

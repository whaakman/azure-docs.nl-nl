---
title: Detecteren en beoordelen van de lokale virtuele VMware-machines op de migratie naar Azure met Azure migreren | Microsoft Docs
description: Beschrijft hoe om te detecteren en beoordelen van de lokale virtuele VMware-machines op de migratie naar Azure met behulp van de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: tutorial
ms.date: 01/08/2018
ms.author: raynew
ms.openlocfilehash: a5019d3f729f2efbd01fca021b0089c7f99b0014
ms.sourcegitcommit: 9a8b9a24d67ba7b779fa34e67d7f2b45c941785e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="discover-and-assess-on-premises-vmware-vms-for-migration-to-azure"></a>Detecteren en beoordelen van de lokale virtuele VMware-machines op de migratie naar Azure

De [Azure migreren](migrate-overview.md) services evalueert voor lokale werkbelastingen voor migratie naar Azure.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Maak een project Azure migreren.
> * Instellen van een lokale collector virtuele machine (VM) voor het detecteren van on-premises virtuele VMware-machines voor evaluatie.
> * Groep virtuele machines en maak een beoordeling.


Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/pricing/free-trial/) aan voordat u begint.


## <a name="prerequisites"></a>Vereisten

- **VMware**: de virtuele machines die u wilt migreren, moet worden beheerd door een vCenter-serverversie 5.5, 6.0 of 6.5. Bovendien moet u één ESXi-host actief versie 5.0 of hoger voor het implementeren van de VM-collector. 
 
> [!NOTE]
> Ondersteuning voor Hyper-V is in onze roadmap en snel worden ingeschakeld. 

- **vCenter Server-account**: U moet een alleen-lezen-account voor toegang tot de vCenter-Server. Azure migreren gebruikt deze account voor het detecteren van de lokale virtuele machines.
- **Machtigingen**: op de vCenter-Server, moet u machtigingen voor het maken van een virtuele machine door het importeren van een bestand in. EICELLEN-indeling. 
- **Instellingen voor statistieken**: de instellingen van de statistieken voor de vCenter-Server moeten worden ingesteld op niveau 3 voordat u implementatie begint. Indien deze lager is dan het niveau 3 beoordeling werkt, maar prestatiegegevens voor opslag en netwerk is niet verzameld. De grootte van de aanbevelingen in dit geval wordt uitgevoerd op basis van prestatiegegevens voor de CPU en geheugen en configuratie-gegevens voor de schijf en netwerkadapters. 

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com).

## <a name="create-a-project"></a>Een project maken

1. Klik in de Azure-portal op **maken van een resource**.
2. Zoeken naar **Azure migreren**, en selecteer de service **Azure migreren (preview)** in de zoekresultaten. Klik vervolgens op **Maken**.
3. Geef de naam van het project en de Azure-abonnement voor het project.
4. Maak een nieuwe resourcegroep.
5. Geef de locatie op waarnaar u het project maken en klik vervolgens op **maken**. U kunt alleen een migreren van de Azure-project maken in de regio West-Centraal VS voor deze preview. U kunt echter nog steeds de migratie voor een doel-Azure-locatie plannen. De opgegeven locatie voor het project wordt alleen gebruikt voor het opslaan van de metagegevens die afkomstig zijn van lokale virtuele machines. 

    ![Azure Migrate](./media/tutorial-assessment-vmware/project-1.png)
    


## <a name="download-the-collector-appliance"></a>Downloaden van het toestel collector

Azure migreren maakt een lokale virtuele machine het toestel collector genoemd. Deze virtuele machine detecteert lokale virtuele VMware-machines en stuurt de metagegevens over deze naar de service Azure migreren. Als u het toestel collector instelt, downloadt u een. EICELLEN bestand en importeer ze in de lokale vCenter-server voor het maken van de virtuele machine.

1. Klik in het project Azure migreren **aan de slag** > **Discover & schatten** > **Machines detecteren**.
2. In **machines ontdekken**, klikt u op **downloaden**, voor het downloaden van de. EICELLEN-bestand.
3. In **project referenties kopiëren**, Kopieer de project-ID en sleutel. U moet deze bij het configureren van de collector.

    ![.Ova bestand downloaden](./media/tutorial-assessment-vmware/download-ova.png)

### <a name="verify-the-collector-appliance"></a>Controleer of het toestel collector

Controleer of de. EICELLEN bestand is beveiligd voordat u deze implementeert.

1. Open een opdrachtvenster beheerder op de machine waarop u het bestand hebt gedownload.
2. Voer de volgende opdracht voor het genereren van de hash voor de eicellen:
    - ```C:\>CertUtil -HashFile <file_location> [Hashing Algorithm]```
    - Voorbeeld van syntaxis:```C:\>CertUtil -HashFile C:\AzureMigrate\AzureMigrate.ova SHA256```
3. De gegenereerde hash moet overeenkomen met deze instellingen.
    
    Voor eicellen versie 1.0.8.49
    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 | 8779eea842a1ac465942295c988ac0c7 
    SHA1 | c136c52a0f785e1fd98865e16479dd103704887d
    SHA256 | 5143b1144836f01dd4eaf84ff94bc1d2c53f51ad04b1ca43ade0d14a527ac3f9

    Voor de versie van de eicellen 1.0.8.40:

    **Algoritme** | **Hash-waarde**
    --- | ---
    MD5 |afbae5a2e7142829659c21fd8a9def3f
    SHA1 | 1751849c1d709cdaef0b02a7350834a754b0e71d
    SHA256 | d093a940aebf6afdc6f616626049e97b1f9f70742a094511277c5f59eacc41ad

## <a name="create-the-collector-vm"></a>De collector VM maken

Het gedownloade bestand importeren in de vCenter-Server.

1. Klik in de console van de Client vSphere **bestand** > **OVF-sjabloon implementeren**.

    ![OVF implementeren](./media/tutorial-assessment-vmware/vcenter-wizard.png)

2. In de Wizard OVF sjabloon implementeren > **bron**, geef de locatie van het bestand .ova.
3. In **naam** en **locatie**, Geef een beschrijvende naam voor de VM-collector en de inventaris-object waarmee de virtuele machine wordt gehost.
5. In **Host/Cluster**, geeft u de host of cluster op waarmee de VM-collector wordt uitgevoerd.
7. Geef de opslaglocatie voor de collector VM in opslag.
8. In **schijfindeling**, het schijftype en de grootte opgeven.
9. In **netwerktoewijzing**, geeft u het netwerk waarmee de VM-collector verbinding maakt. Het netwerk moet verbinding met internet, metagegevens verzenden naar Azure. 
10. Controleer en Bevestig de instellingen en klik op **voltooien**.

## <a name="run-the-collector-to-discover-vms"></a>De collector voor het detecteren van virtuele machines worden uitgevoerd

1. In de console vSphere Client met de rechtermuisknop op de virtuele machine > **Console openen**.
2. Geef de taal, de tijdzone en het wachtwoord voorkeuren voor het apparaat.
3. Klik op het bureaublad op de **uitvoeren collector** snelkoppeling.
4. Open in de Collector Azure migreren **vereisten instellen**.
    - Accepteer de licentievoorwaarden en lees de informatie van derden.
    - De collector controleert of de virtuele machine toegang tot internet.
    - Als de virtuele machine toegang heeft tot internet via een proxy, klikt u op **Proxy-instellingen**, en geef de proxy-adres en poort voor luisteren. Geef referenties op als de proxy moet worden geverifieerd.

    > [!NOTE]
    > Adres voor de proxyserver moet worden ingevoerd in het formulier http://ProxyIPAddress of http://ProxyFQDN. Alleen HTTP-proxy wordt ondersteund.

    - De collector wordt gecontroleerd of de collectorservice wordt uitgevoerd. De service wordt standaard geïnstalleerd op de VM-collector.
    - Download en installeer de VMware PowerCLI.

5. In **gegevens vCenter-Server opgeven**, het volgende doen:
    - Geef de FQDN-naam of IP-adres van de vCenter-server.
    - In **gebruikersnaam** en **wachtwoord**, geef de alleen-lezen accountreferenties die door de collector wordt gebruikt voor het detecteren van virtuele machines op de vCenter-server.
    - In **verzameling bereik**, selecteert u een bereik voor detectie van de virtuele machine. De collector kan alleen detecteren voor virtuele machines binnen het opgegeven bereik. Bereik kan worden ingesteld op een specifieke map, een datacenter of een cluster. Deze mag niet meer dan 1000 virtuele machines bevatten. 

6. In **Geef migratieproject**, geeft u de Azure migreren project-ID en sleutel dat u hebt gekopieerd uit de portal. Als niet kopiëren, de Azure portal openen van de VM-collector. In het project **overzicht** pagina, klikt u op **Machines detecteren**, en kopieer de waarden.  
7. In **verzameling voortgang**, detectie bewaken en controleren van metagegevens verzameld van de virtuele machines zijn binnen het bereik. De collector zorgt voor een geschatte detectie.

> [!NOTE]
> De collector ondersteunt alleen 'Engels (Verenigde Staten)' als de taal van het besturingssysteem en de taal van de gebruikersinterface van de collector. Ondersteuning voor meer talen is binnenkort beschikbaar.


### <a name="verify-vms-in-the-portal"></a>Controleer of virtuele machines in de portal

Detectie tijd is afhankelijk van hoeveel virtuele machines die u detectie. Normaal gesproken duurt voor 100 virtuele machines, nadat de collector is uitgevoerd op het om een uur voor de detectie is voltooid. 

1. Klik in het project migratie Planner op **beheren** > **Machines**.
2. Controleer of de virtuele machines die u wilt detecteren in de portal weergegeven.


## <a name="create-and-view-an-assessment"></a>Maken en weergeven van een beoordeling

Nadat de virtuele machines zijn gedetecteerd, kunt u ze te groeperen en maken van een beoordeling. 

1. In het project **overzicht** pagina, klikt u op **+ maken assessment**.
2. Klik op **weergeven van alle** om te controleren van de evaluatie-eigenschappen.
3. De groep te maken en geef een groepsnaam.
4. Selecteer de computers die u wilt toevoegen aan de groep.
5. Klik op **Assessment maken**wilt maken van de groep en de evaluatie.
6. Nadat de beoordeling is gemaakt, weer te geven in **overzicht** > **Dashboard**.
7. Klik op **assessment exporteren**, om deze te downloaden als een Excel-bestand.

### <a name="sample-assessment"></a>Voorbeeld assessment

Hier volgt een voorbeeld van een rapport assessment. Het bevat informatie over of VM's compatibel met Azure en Geschatte maandelijkse kosten zijn. 

![Beoordelingsrapport](./media/tutorial-assessment-vmware/assessment-report.png)

#### <a name="azure-readiness"></a>Azure-gereedheid

Deze weergave toont de gereedheidsstatus voor elke computer.

- Voor virtuele machines die klaar zijn, Azure migreren raadt u aan een VM-grootte in Azure.
- Voor virtuele machines die nog niet klaar zijn, Azure migreren wordt uitgelegd waarom en biedt herstelstappen aan.
- Azure migreren stelt hulpprogramma's die u voor de migratie gebruiken kunt. Als de computer geschikt is voor migratie van de lift en shift, wordt de Azure Site Recovery-service wordt aanbevolen. Als het een machine database, wordt de Azure-databaseservice migratie aanbevolen.

  ![Readiness Assessment](./media/tutorial-assessment-vmware/assessment-suitability.png)  

#### <a name="monthly-cost-estimate"></a>Schatting maandelijkse kosten

Deze weergave toont de totale berekenings- en opslagkosten van de virtuele machines in Azure samen met de details voor elke computer wordt uitgevoerd. Maakt een schatting van kosten zijn berekend met behulp van de aanbevelingen op basis van prestaties grootte voor een machine en de schijven en de eigenschappen van de evaluatie. 

> [!NOTE]
> De kosten schatting verstrekt door Azure migreren is voor het uitvoeren van de lokale virtuele machines als Azure-infrastructuur als een dienst (IaaS) virtuele machines. Azure migreren beschouwd niet als een Platform als een service (PaaS) of Software als een dienst (SaaS)-kosten. 

Geschatte maandelijkse kosten voor berekeningen en opslag worden samengevoegd voor alle virtuele machines in de groep. 

![Assessment VM kosten](./media/tutorial-assessment-vmware/assessment-vm-cost.png) 

U kunt inzoomen op Zie details voor een specifieke computer.

![Assessment VM kosten](./media/tutorial-assessment-vmware/assessment-vm-drill.png) 

## <a name="next-steps"></a>Volgende stappen

- [Meer informatie over](how-to-scale-assessment.md) detecteren en beoordelen van een grote VMware-omgeving.
- Informatie over het maken van hoge betrouwbaarheid assessment groepen met behulp van [machine afhankelijkheid toewijzing](how-to-create-group-machine-dependencies.md)
- [Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.

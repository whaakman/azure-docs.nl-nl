---
title: Problemen met Azure Migrate oplossen | Microsoft Docs
description: Hierin vindt u een overzicht van bekende problemen in de Azure Migrate-service en tips voor het oplossen van problemen met veelvoorkomende fouten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/17/2019
ms.author: raynew
ms.openlocfilehash: 0e2a8f269a98babc17f36ceff209ee2f057e6911
ms.sourcegitcommit: af58483a9c574a10edc546f2737939a93af87b73
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/17/2019
ms.locfileid: "68302312"
---
# <a name="troubleshoot-azure-migrate"></a>Problemen met Azure Migrate oplossen

[Azure migrate](migrate-services-overview.md) biedt een hub van hulpprogram ma's voor evaluatie en migratie van micro soft, evenals onafhankelijke ISV-aanbiedingen (Independent Software Vendor) van derden. In dit document vindt u informatie over het oplossen van fouten met Azure Migrate Azure Migrate: Server evaluatie en Azure Migrate: Server migratie.

## <a name="azure-migrate-project-issues"></a>Azure Migrate project problemen

### <a name="i-am-unable-to-find-my-existing-azure-migrate-project"></a>Ik kan mijn bestaande Azure Migrate-project niet vinden.

Er zijn [twee versies](https://docs.microsoft.com/azure/migrate/migrate-services-overview#azure-migrate-versions) van Azure Migrate. Afhankelijk van de versie waarin u het project hebt gemaakt, volgt u de onderstaande stappen om het project te zoeken:

1. Als u op zoek bent naar een project dat is gemaakt met de eerdere-versie (oude ervaring) van Azure Migrate, volgt u de onderstaande stappen om het project te vinden.

    1. Ga naar [Azure Portal](https://portal.azure.com)en zoek naar **Azure migrate**.
    2. Op het dash board van Azure Migrate ziet u een banner dat spreekt over toegang tot oudere projecten. Deze banner wordt alleen weer geven als u een project hebt gemaakt met de oude ervaring. Klik op de banner.

    ![Toegang tot bestaande projecten](./media/troubleshooting-general/access-existing-projects.png)

    3. U ziet nu de lijst met bestaande projecten die zijn gemaakt met de vorige versie van Azure Migrate.

2. Als u op zoek bent naar een project dat is gemaakt met de huidige versie (nieuwe ervaring), volgt u de onderstaande stappen om het project te vinden.

    1. Ga naar [Azure Portal](https://portal.azure.com)en zoek naar **Azure migrate**.
    2. Ga op het Azure Migrate dash board naar de pagina **servers** in het linkerdeel venster en selecteer **wijzigen** in de rechter bovenhoek:

    ![Overschakelen naar een bestaand Azure Migrate project](./media/troubleshooting-general/switch-project.png)

    3. Selecteer het juiste **abonnement** en **Migreer het project**.

### <a name="i-am-unable-to-create-a-second-azure-migrate-project"></a>Ik kan geen tweede Azure Migrate project maken.

Volg de onderstaande stappen om een nieuw Azure Migrate project te maken.

1. Ga naar [Azure Portal](https://portal.azure.com)en zoek naar **Azure migrate**.
2. Ga op het Azure Migrate dash board naar de pagina **servers** in het linkerdeel venster en selecteer **wijzigen** in de rechter bovenhoek:

   ![Azure Migrate project wijzigen](./media/troubleshooting-general/switch-project.png)

3. Als u een nieuw project wilt maken, selecteert u **hier klikken** , zoals wordt weer gegeven in de onderstaande scherm afbeelding:

   ![Een tweede Azure Migrate-project maken](./media/troubleshooting-general/create-new-project.png)

### <a name="deletion-of-azure-migrate-projects-and-associated-log-analytics-workspace"></a>Verwijdering van Azure Migrate projecten en gekoppelde Log Analytics werk ruimte

Wanneer u een Azure Migrate project verwijdert, wordt het migratie project samen met de meta gegevens over gedetecteerde computers verwijderd. Als u echter een Log Analytics-werk ruimte hebt gekoppeld aan het hulp programma voor Server evaluatie, wordt de Log Analytics-werk ruimte niet automatisch verwijderd. Dit komt doordat dezelfde Log Analytics-werk ruimte kan worden gebruikt voor meerdere use cases. Als u de werk ruimte Log Analytics ook wilt verwijderen, moet u dit hand matig doen.

1. Blader naar de Log Analytics werkruimte die aan het project is gekoppeld.
     1. Als u het migratie project nog niet hebt verwijderd, kunt u de koppeling naar de werk ruimte vinden op de pagina overzicht van de server evaluatie in de sectie Essentials.

     ![De werk ruimte LA](./media/troubleshooting-general/loganalytics-workspace.png)

     2. Als u het migratie project al hebt verwijderd, selecteert u **resource groepen** in het linkerdeel venster van de Azure Portal. Ga naar de resource groep waarin de werk ruimte is gemaakt en blader ernaar.
2. Volg de instructies [in dit artikel](https://docs.microsoft.com/azure/azure-monitor/platform/delete-workspace) om de werk ruimte te verwijderen.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Het maken van het migratie project is mislukt met fout *aanvragen moet de gebruikers-id-headers bevatten*

Dit probleem kan zich voordoen voor gebruikers die geen toegang hebben tot de Azure Active Directory Azure AD-Tenant van de organisatie. Wanneer een gebruiker voor de eerste keer wordt toegevoegd aan een Azure AD-Tenant, ontvangt hij/zij een uitnodiging voor e-mail om lid te worden van de Tenant. Gebruikers moeten naar het e-mail bericht gaan en de uitnodiging accepteren om aan de Tenant te kunnen toevoegen. Als u het e-mail bericht niet kunt zien, neemt u contact op met een gebruiker die al toegang heeft tot de Tenant en vraagt u de uitnodiging opnieuw te verzenden met behulp van de stappen die u [hier](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users)opgeeft.

Zodra de uitnodigings-e-mail is ontvangen, moet u het e-mail bericht openen en klikken op de koppeling in het e-mail bericht om de uitnodiging te accepteren. Als u dit hebt gedaan, moet u zich afmelden bij Azure Portal en zich opnieuw aanmelden, anders werkt de browser niet. U kunt vervolgens het migratie project maken.

## <a name="appliance-issues"></a>Problemen met het apparaat

### <a name="deployment-of-azure-migrate-appliance-for-vmware-failed-with-the-error-the-provided-manifest-file-is-invalid-invalid-ovf-manifest-entry"></a>De implementatie van Azure Migrate apparaat voor VMware is mislukt met de volgende fout: Het opgegeven manifestbestand is ongeldig: Ongeldige vermelding van OVF-manifest.

1. Controleer of het bestand van Azure Migrate apparaat moet worden gedownload door de hash-waarde te controleren. Raadpleeg het [artikel](https://docs.microsoft.com/azure/migrate/tutorial-assessment-vmware#verify-the-collector-appliance) om de hash-waarde te controleren. Als de hash-waarde niet overeenkomt, downloadt u het bestand van de eicellen opnieuw en voert u de implementatie opnieuw uit.
2. Als deze nog steeds mislukt en u VMware vSphere Client gebruikt om de OVF te implementeren, kunt u proberen deze te implementeren via de vSphere Web Client. Als de service nog steeds niet werkt, kunt u een andere webbrowser gebruiken.
3. Als u vSphere-webclient gebruikt en probeert te implementeren op vCenter Server 6,5 of 6,7, probeert u de eicellen rechtstreeks op ESXi-host te implementeren door de onderstaande stappen te volgen:
   - Maak rechtstreeks verbinding met de ESXi-host (in plaats van vCenter Server) met behulp van de webclient (https://<*IP-adres*van de host >/UI).
   - Ga naar **huisraad** > **inventaris**.
   - Klik op **bestand** > **implementeren OVF-sjabloon**. Blader naar de eicellen en voltooi de implementatie.
4. Als de implementatie nog steeds mislukt, neemt u contact op met Azure Migrate ondersteuning.

### <a name="appliance-is-not-able-to-connect-to-the-internet"></a>Het apparaat kan geen verbinding maken met Internet

Dit kan gebeuren wanneer de computer die u gebruikt zich achter een proxy bevindt. Zorg ervoor dat u de autorisatie referenties opgeeft als de proxy er een nodig heeft.
Als u een firewall proxy op basis van een URL gebruikt om de uitgaande connectiviteit te beheren, moet u deze vereiste Url's toevoegen aan een acceptatie lijst:

Scenario | Lijst met URL'S
--- | ---
Server evaluatie voor VMware | [Veld](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#assessment-url-access-requirements)
Server evaluatie voor Hyper-V | [Veld](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#assessment-appliance-url-access)
Server migratie voor VMware (zonder agent) | [Veld](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#agentless-migration-url-access-requirements)
Server migratie voor VMware (op agent gebaseerd) | [Veld](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-vmware#replication-appliance-url-access)
Server migratie voor Hyper-V | [Veld](https://docs.microsoft.com/azure/migrate/migrate-support-matrix-hyper-v#migration-hyper-v-host-url-access)

Als u een onderscheping proxy gebruikt om verbinding te maken met internet, moet u het proxy certificaat importeren op de apparaat-VM. U kunt het proxy certificaat importeren met behulp van de stappen die [hier](https://docs.microsoft.com/azure/migrate/concepts-collector)worden beschreven.

### <a name="error-802-date-and-time-synchronization-error"></a>Fout 802: Synchronisatie fout voor datum en tijd

De klok van de server is mogelijk niet gesynchroniseerd met de huidige tijd van meer dan vijf minuten. Wijzig de klok tijd op de Collector-VM zodat deze overeenkomt met de huidige tijd, als volgt:

1. Open een opdracht prompt met beheerders rechten op de VM.
2. Als u de tijd zone wilt controleren, voert u w32tm/TZ. uit
3. Voer w32tm/resync. uit om de tijd te synchroniseren


### <a name="error-unabletoconnecttoserver"></a>Fout UnableToConnectToServer

Kan geen verbinding maken met vCenter Server: 'Servernaam.com:9443' vanwege de fout: Er luistert geen eindpunt op https://Servername.com:9443/sdk dat het bericht kan accepteren.

Controleer of u de meest recente versie van het verzamel apparaat gebruikt. als dat niet het geval is, moet u het apparaat bijwerken naar de [nieuwste versie](https://docs.microsoft.com/azure/migrate/concepts-collector).

Als het probleem zich nog steeds voordoet in de meest recente versie, kan de oorzaak zijn dat de vCenter Server opgegeven naam niet kan worden omgezet of dat de opgegeven poort onjuist is. Als de poort niet is opgegeven, probeert Collector standaard verbinding te maken met het poort nummer 443.

1. Probeer de Servername.com vanaf de Collector-machine te pingen.
2. Als stap 1 mislukt, probeert u via het IP-adres verbinding te maken met de vCenter-server.
3. Bepaal het juiste poortnummer om verbinding te maken met de vCenter-server.
4. Controleer tot slot of de vCenter-server actief is.

## <a name="discovery-issues"></a>Detectie problemen

### <a name="i-started-discovery-but-i-dont-see-the-discovered-vms-on-azure-portal-server-assessment-and-server-migrate-tiles-show-a-status-of-discovery-in-progress"></a>Ik heb detectie gestart, maar ik zie de gedetecteerde Vm's niet op Azure Portal. Tegels voor Server analyse en server migratie tonen de status ' detectie wordt uitgevoerd '
Na het starten van de detectie van het apparaat, kan het enige tijd duren voordat de gedetecteerde computers worden weer gegeven op de Azure Portal. Het duurt ongeveer 15 minuten voor een VMware-detectie en ongeveer 2 minuten per toegevoegde host voor een Hyper-V-detectie. Als u nog steeds de detectie wordt uitgevoerd, klikt u op het tabblad **servers** op **vernieuwen** . Hier wordt het aantal gedetecteerde servers weer gegeven in de tegels server evaluatie en server migratie.


### <a name="i-am-using-the-appliance-that-continuously-discovers-my-on-premises-environment-but-the-vms-that-are-deleted-in-my-on-premises-environment-are-still-being-shown-in-the-portal"></a>Ik gebruik het apparaat dat de on-premises omgeving voortdurend detecteert, maar de virtuele machines die in mijn on-premises omgeving worden verwijderd, worden nog steeds weer gegeven in de portal.

Het duurt Maxi maal 30 minuten voordat de detectie gegevens die door het apparaat zijn verzameld, in de portal worden weer gegeven. Als u na 30 minuten geen actuele informatie ziet, voert u de volgende stappen uit om de gegevens te vernieuwen:

1. In servers > Azure Migrate: Server evaluatie, klikt u op **overzicht**.
2. Klik onder **beheren**op **status van agent**
3. Klik op de optie voor het vernieuwen van de **agent**. U ziet deze onderstaande optie onder de lijst met agents.
4. Wacht tot de vernieuwings bewerking is voltooid. Controleer of u actuele informatie kunt bekijken op uw Vm's.

### <a name="i-dont-the-latest-information-on-the-on-premise-vms-on-the-portal"></a>Ik heb niet de nieuwste informatie over de on-premises Vm's in de portal

Het duurt Maxi maal 30 minuten voordat de detectie gegevens die door het apparaat zijn verzameld, in de portal worden weer gegeven. Als u na 30 minuten geen actuele informatie ziet, kunt u de gegevens vernieuwen met behulp van de volgende stappen:

1. In servers > Azure Migrate: Server evaluatie, klikt u op **overzicht**.
2. Klik onder **beheren**op **status van agent**
3. Klik op de optie voor het vernieuwen van de **agent**. Deze optie wordt weer gegeven onder de lijst met agents.
4. Wacht tot de vernieuwings bewerking is voltooid. U ziet nu actuele informatie over uw Vm's.

### <a name="unable-to-connect-to-hosts-or-cluster-as-the-server-name-cannot-be-resolved-winrm-error-code-0x803381b9-error-id-50004"></a>Kan geen verbinding maken met de host (s) of het cluster omdat de server naam niet kan worden omgezet. WinRM-fout code: 0x803381B9 (fout-ID: 50004)
Deze fout treedt op als de DNS-server van het apparaat het cluster of de hostnaam die u hebt ingevoerd, niet kan omzetten. Als deze fout op het cluster wordt weer geven, kunt u proberen om de Fully Qualified Domain Name van het cluster op te geven. 

Mogelijk ziet u deze fout ook voor hosts in een cluster. In dit geval kan het apparaat verbinding maken met het cluster. Het cluster heeft echter de hostnamen geretourneerd die geen volledig gekwalificeerde domein namen zijn. 

Om deze fout op te lossen, werkt u het hosts-bestand op het apparaat bij om een toewijzing van het IP-adres en de hostnamen toe te voegen.
1. Open Klad blok als gebruiker met beheerders rechten. Open het bestand C:\Windows\System32\Drivers\etc\hosts.
2. Voeg het IP-adres en de hostnaam toe aan een rij. Herhaal deze stap voor elke host of elk cluster waar u deze fout ziet.
3. Sla het bestand hosts op en sluit het.
4. U kunt controleren of het apparaat verbinding kan maken met de hosts met behulp van de app voor het beheren van het apparaat. Na 30 minuten zou u de meest recente informatie op deze hosts op het Azure Portal moeten kunnen zien. 


## <a name="assessment-issues"></a>Beoordelings problemen

### <a name="azure-readiness-issues"></a>Problemen met Azure-gereedheid

Probleem | Herstel
--- | ---
Niet-ondersteund opstart type | Azure biedt geen ondersteuning voor Vm's met een EFI-opstart type. U wordt aangeraden het opstart type te converteren naar BIOS voordat u een migratie uitvoert. <br/><br/>U kunt Azure Migrate server migratie gebruiken om de migratie van dergelijke Vm's uit te voeren, omdat tijdens de migratie het opstart type van de virtuele machine wordt geconverteerd naar het BIOS.
Voorwaardelijk ondersteund Windows-besturings systeem | Het besturings systeem heeft het einde van de ondersteunings datum door gegeven en heeft een aangepaste ondersteunings overeenkomst (CSA) nodig voor [ondersteuning in azure](https://aka.ms/WSosstatement). u kunt het besturings systeem upgraden voordat u naar Azure migreert.
Niet-ondersteund Windows-besturings systeem | Azure ondersteunt alleen [geselecteerde versies van Windows-besturings systemen](https://aka.ms/WSosstatement), overweeg het besturings systeem van de machine te upgraden voordat u naar Azure migreert.
Voorwaardelijk goedgekeurd Linux-besturings systeem | Azure bevestigt alleen [geselecteerde Linux-besturingssysteem versies](../virtual-machines/linux/endorsed-distros.md), overweeg het besturings systeem van de machine te upgraden voordat u naar Azure migreert.
Niet-goedgekeurd Linux-besturings systeem | De computer kan worden opgestart in azure, maar er wordt geen ondersteuning voor het besturings systeem geboden door Azure. Overweeg het besturings systeem bij te werken naar een [officiële versie van Linux](../virtual-machines/linux/endorsed-distros.md) voordat u naar Azure migreert
Onbekend besturings systeem | Het besturings systeem van de virtuele machine is als ' Overig ' opgegeven in vCenter Server, omdat Azure Migrate de Azure-gereedheid van de virtuele machine niet kan identificeren. Zorg ervoor dat het besturings systeem dat op de computer wordt uitgevoerd, wordt [ondersteund](https://aka.ms/azureoslist) door Azure voordat u de computer migreert.
Niet-ondersteunde OS-Bitness | Vm's met 32-bits besturings systeem kunnen worden opgestart in azure, maar het wordt aanbevolen het besturings systeem van de virtuele machine te upgraden van 32-bits naar 64-bits voordat u naar Azure migreert.
Er is een Visual Studio-abonnement vereist. | Op de machine wordt een Windows-client besturingssysteem uitgevoerd. dit wordt alleen ondersteund in een Visual Studio-abonnement.
Er is geen VM gevonden voor de vereiste opslag prestaties. | De opslag prestaties (IOPS/door Voer) die vereist zijn voor de machine overschrijdt de ondersteuning voor Azure-VM'S. Verminder de opslag vereisten voor de machine vóór de migratie.
Er is geen VM gevonden voor de vereiste netwerk prestaties. | De netwerk prestaties (in/uit) die vereist zijn voor de computer, overschrijden de ondersteuning voor Azure-VM'S. Verminder de netwerk vereisten voor de computer.
De virtuele machine is niet gevonden op de opgegeven locatie. | Gebruik een andere doel locatie vóór de migratie.
Een of meer ongeschikte schijven. | Een of meer schijven die zijn gekoppeld aan de virtuele machine voldoen niet aan de vereisten van Azure. Voor elke schijf die is gekoppeld aan de virtuele machine, moet u ervoor zorgen dat de grootte van de schijf < 4 TB, als dat niet het geval is, verkleint u de schijf grootte voordat u naar Azure migreert. Zorg ervoor dat de prestaties (IOPS/door Voer) die nodig zijn voor elke schijf worden ondersteund door door Azure [beheerde virtuele-machine schijven](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits).   
Een of meer niet-geschikte netwerk adapters. | Verwijder ongebruikte netwerk adapters van de machine voordat de migratie wordt gebruikt.
Aantal schijven overschrijdt de limiet | Verwijder ongebruikte schijven van de machine vóór de migratie.
De schijf grootte overschrijdt de limiet | Azure ondersteunt schijven met een maximale grootte van 4 TB. Schijven verkleinen tot minder dan 4 TB vóór de migratie.
De schijf is niet beschikbaar op de opgegeven locatie | Zorg ervoor dat de schijf zich op de doel locatie bevindt voordat u migreert.
De schijf is niet beschikbaar voor de opgegeven redundantie | De schijf moet het opslag type redundantie gebruiken dat is gedefinieerd in de instellingen voor evaluatie (standaard LRS).
Kan de schijf geschiktheid niet bepalen vanwege een interne fout | Probeer een nieuwe evaluatie voor de groep te maken.
VM met vereiste kernen en geheugen niet gevonden | Azure kan een geschikt VM-type niet verfijnen. Verminder het geheugen en het aantal kernen van de on-premises machine voordat u migreert.
Kan de geschiktheid van de virtuele machine niet bepalen vanwege een interne fout. | Probeer een nieuwe evaluatie voor de groep te maken.
Kan de geschiktheid van een of meer schijven niet bepalen vanwege een interne fout. | Probeer een nieuwe evaluatie voor de groep te maken.
Kan de geschiktheid van een of meer netwerk adapters niet bepalen vanwege een interne fout. | Probeer een nieuwe evaluatie voor de groep te maken.

### <a name="i-am-unable-to-specify-enterprise-agreement-ea-as-an-azure-offer-in-the-assessment-properties"></a>Ik kan Enterprise Agreement (EA) niet als een Azure-aanbieding opgeven in de beoordelings eigenschappen?
Azure Migrate: Server analyse biedt momenteel geen ondersteuning voor prijzen op basis van Enterprise Agreement (EA). De tijdelijke oplossing is om ' betalen per gebruik ' als de Azure-aanbieding te gebruiken en de eigenschap Discount te gebruiken om een aangepaste korting op te geven die u ontvangt. Meer [informatie over hoe u een evaluatie kunt aanpassen](https://aka.ms/migrate/selfhelp/eapricing).

### <a name="why-does-server-assessment-mark-my-linux-vms-conditionally-ready-is-there-anything-i-need-to-do-to-fix-this"></a>Waarom markeert de server evaluatie mijn Linux-Vm's voor bereid. Is er iets wat ik moet doen om dit op te lossen?
Er bevindt zich een bekende tussen ruimte in de server evaluatie waarbij de secundaire versie van het Linux-besturings systeem dat is geïnstalleerd op de on-premises Vm's (bijvoorbeeld voor RHEL 6,10) niet kan worden gedetecteerd. momenteel detecteert de server evaluatie alleen RHEL 6 als de versie van het besturings systeem. Aangezien Azure alleen specifieke versies van Linux afschrijft, zijn de virtuele Linux-machines momenteel gemarkeerd als voorwaardelijk gereed in Server evaluatie. U kunt hand matig controleren of het Linux-besturings systeem dat wordt uitgevoerd op de on-premises VM, wordt goedgekeurd in azure door de [Azure Linux](https://aka.ms/migrate/selfhost/azureendorseddistros)-ondersteunings documentatie te lezen. Nadat u de geviseerde distributie hebt gecontroleerd, kunt u deze waarschuwing negeren.

### <a name="the-vm-sku-recommended-by-server-assessment-has-more-number-of-cores-and-a-larger-memory-size-than-what-is-allocated-on-premises-why-is-that-so"></a>De VM-SKU die wordt aanbevolen door de server evaluatie heeft meer kernen en een grotere geheugen grootte dan op locatie toegewezen. Waarom is dat zo?
De aanbeveling van de VM-SKU in Server beoordeling is afhankelijk van de evaluatie-eigenschappen. U kunt twee soorten evaluaties maken in de evaluaties van server evaluatie, ' prestaties op basis van ' en ' als on-premises '. Voor evaluaties op basis van prestaties worden de gebruiks gegevens van de on-premises Vm's (CPU, geheugen, schijf en netwerk gebruik) beschouwd om de juiste doel-VM-SKU voor uw on-premises Vm's te bepalen. Daarnaast wordt bij een op prestaties gebaseerd formaat de comfort factor in rekening gebracht om het effectief gebruik te identificeren. Bij een on-premises grootte worden de prestatie gegevens niet in overweging genomen en wordt een doel-SKU aanbevolen op basis van wat lokaal is toegewezen.

Stel bijvoorbeeld dat er een on-premises VM is met 4 kernen en 8 GB geheugen met een CPU-gebruik van 50% en een geheugen gebruik van 50% en een comfort factor van 1,3 is opgegeven in de evaluatie. Als het grootte criterium van de evaluatie op on-premises is een Azure VM-SKU met 4 kernen en 8 GB geheugen wordt aanbevolen, maar als het criterium voor de grootte op prestaties gebaseerd is, op basis van effectief CPU-en geheugen gebruik (50% van 4 kernen * 1,3 = 2,6 kernen en 50% van 8 GB) geheugen * 1,3 = 5,3 GB geheugen), de goedkoopste VM-SKU van vier kernen (meest ondersteunde aantal kernen) en 8 GB geheugen grootte (dichtstbijgelegen ondersteunde geheugen grootte) wordt aanbevolen. Meer [informatie over hoe Server evaluatie het formaat van het maken kan aanpassen.](https://docs.microsoft.com/azure/migrate/concepts-assessment-calculation#sizing)

### <a name="the-disk-sku-recommended-by-server-assessment-has-a-bigger-size-than-what-is-allocated-on-premises-why-is-that-so"></a>De schijf-SKU die wordt aanbevolen door de server evaluatie, heeft een grotere grootte dan de ruimte die on-premises wordt toegewezen. Waarom is dat zo?
De schijf grootte in Server analyse is afhankelijk van twee evaluatie-eigenschappen: grootte criterium en opslag type. Als het criterium voor het instellen van de grootte is ingesteld op ' automatisch ', worden de IOPS-en doorvoer waarden van de schijf gezien als identificatie van het doel schijf type (Standard-HDD, Standard-SSD of Premium-schijven). Een schijf-SKU binnen het schijf type wordt vervolgens aanbevolen om de grootte vereisten van de on-premises schijf te overwegen. Als het criterium voor het bepalen van de grootte ' Premium ' is, wordt een Premium-schijf-SKU in azure aanbevolen op basis van de IOPS-, doorvoer-en grootte vereisten van de on-premises schijf. Dezelfde logica wordt gebruikt voor het uitvoeren van de schijf grootte wanneer het criterium voor het wijzigen van de grootte de grootte van de on-premises heeft en het opslag type is Standard-HDD, Standard-SSD of Premium.

Als u bijvoorbeeld een on-premises schijf hebt met een geheugen van 32 GB, maar de geaggregeerde Lees-en schrijf-IOPS voor de schijf 800 IOPS is, raadt de server evaluatie een Premium-schijf type aan (als gevolg van de hogere IOPS-vereisten) en raden we u aan om een schijf-SKU te ondersteunen. Dit kan de vereiste IOPS en grootte. De dichtstbijzijnde overeenkomst in dit voor beeld is P15 (256 GB, 1100 IOPS). Hoewel de grootte die is vereist door de on-premises schijf 32 GB was, aanbevolen server evaluatie een schijf met een grotere grootte vanwege de hoge IOPS-vereiste van de on-premises schijf.

### <a name="is-the-os-license-cost-of-the-vm-included-in-the-compute-cost-estimated-by-server-assessment"></a>Zijn de OS-licentie kosten van de virtuele machine opgenomen in de reken kosten geschat door de server evaluatie?
Voor de server analyse worden momenteel alleen de OS-licentie kosten voor Windows-computers beschouwd. de kosten voor OS-licenties voor Linux-machines worden momenteel niet in overweging genomen. 

### <a name="what-impact-does-performance-history-and-percentile-utilization-have-on-the-size-recommendations"></a>Welke invloed heeft de prestaties van de prestatie geschiedenis en het percentiel?
Deze eigenschappen zijn alleen van toepassing op de grootte van het prestatie niveau. Server analyse verzamelt voortdurend prestatie gegevens van on-premises machines en gebruikt deze om de SKU van de VM en schijf in azure aan te bevelen. Hieronder ziet u hoe prestatie gegevens worden verzameld door server evaluatie:
- Het Azure Migrate apparaat maakt continu een profiel van de on-premises omgeving voor het verzamelen van gegevens over realtime gebruik om de 20 seconden voor VMware-Vm's en elke 30 seconden voor Hyper-V-Vm's.
- Het apparaat implementeert de 20/30 seconden voor beelden om één gegevens punt voor elke 10 minuten te maken. Voor het maken van één gegevens punt selecteert het apparaat de piek waarde van alle voor beelden van 20/30 seconden en verzendt deze naar Azure.
- Wanneer u een evaluatie maakt in Server evaluatie, op basis van de waarde voor prestatie duur en prestatie geschiedenis percentiel, wordt de representatieve gebruiks waarde aangeduid. Als de prestatie geschiedenis bijvoorbeeld één week is en het percentiel gebruik 95e is, sorteert Azure Migrate alle tien minuten bemonsterings punten voor de afgelopen week in oplopende volg orde en selecteert u vervolgens het 95e percentiel als de representatieve waarde.
De 95e percentiel waarde zorgt ervoor dat u alle uitbijters negeert die kunnen worden opgenomen als u het 99e percentiel selecteert. Als u het piek gebruik voor de periode wilt kiezen en uitschieters niet wilt missen, moet u het 99e percentiel als het percentiel gebruik selecteren.

## <a name="dependency-visualization-issues"></a>Afhankelijkheids problemen bij de visualisatie

### <a name="i-am-unable-to-find-the-dependency-visualization-functionality-for-azure-government-projects"></a>Ik kan de functionaliteit voor de visualisatie van afhankelijkheden voor Azure Government projecten niet vinden.

Azure Migrate is afhankelijk van Servicetoewijzing voor de visualisatie functionaliteit van de afhankelijkheid en omdat Servicetoewijzing momenteel niet beschikbaar is in Azure Government, is deze functionaliteit niet beschikbaar in Azure Government.

### <a name="i-installed-the-microsoft-monitoring-agent-mma-and-the-dependency-agent-on-my-on-premises-vms-but-the-dependencies-are-now-showing-up-in-the-azure-migrate-portal"></a>Ik heb micro soft Monitoring Agent (MMA) en de dependency agent geïnstalleerd op mijn on-premises Vm's, maar de afhankelijkheden worden nu weer gegeven in de Azure Migrate Portal.

Zodra u de agents hebt geïnstalleerd, duurt Azure Migrate doorgaans 15-30 minuten om de afhankelijkheden in de portal weer te geven. Als u langer dan 30 minuten hebt gewacht, controleert u of de MMA-agent kan communiceren met de OMS-werk ruimte door de onderstaande stappen te volgen:

Voor Windows VM:
1. Ga naar **configuratie scherm** en start **micro soft Monitoring Agent**.
2. Ga naar het tabblad **Azure log Analytics (OMS)** in het pop-upvenster met MMA-eigenschappen.
3. Zorg ervoor dat de **status** van de werk ruimte groen is.
4. Als de status niet groen is, probeert u de werk ruimte te verwijderen en toe te voegen aan MMA.
        ![MMA-status](./media/troubleshooting-general/mma-status.png)

Zorg ervoor dat de installatie opdrachten voor MMA en de afhankelijkheids agent zijn geslaagd voor Linux VM.

### <a name="what-are-the-operating-systems-supported-by-mma"></a>Wat zijn de besturings systemen die worden ondersteund door MMA?

[Hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-windows-operating-systems)vindt u een lijst met Windows-besturings systemen die worden ondersteund door MMA.
De lijst met Linux-besturings systemen die door MMA worden ondersteund, is [hier](https://docs.microsoft.com/azure/log-analytics/log-analytics-concept-hybrid#supported-linux-operating-systems).

### <a name="what-are-the-operating-systems-supported-by-dependency-agent"></a>Wat zijn de besturings systemen die worden ondersteund door de afhankelijkheids agent?

[Hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-windows-operating-systems)vindt u een lijst met Windows-besturings systemen die worden ondersteund door de afhankelijkheids agent.
De lijst met Linux-besturings systemen die worden ondersteund door de afhankelijkheids agent, is [hier](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure#supported-linux-operating-systems).

### <a name="i-am-unable-to-visualize-dependencies-in-azure-migrate-for-more-than-one-hour-duration"></a>Ik kan voor meer dan één uur geen afhankelijkheden in Azure Migrate visualiseren?
Met Azure Migrate kunt u afhankelijkheden van Maxi maal één uur visualiseren. Hoewel u met Azure Migrate kunt terugkeren naar een bepaalde datum in de geschiedenis tot de laatste maand, de maximale duur waarvoor u de afhankelijkheden kunt visualiseren, is Maxi maal 1 uur. U kunt bijvoorbeeld de functie tijd duur in de afhankelijkheids toewijzing gebruiken om de afhankelijkheden voor gisteren weer te geven, maar deze kunnen alleen voor een venster van één uur worden weer gegeven. U kunt echter Azure Monitor-Logboeken gebruiken om een langere duur van [de afhankelijkheids gegevens](https://docs.microsoft.com/azure/migrate/how-to-create-group-machine-dependencies) op te vragen.

### <a name="i-am-unable-to-visualize-dependencies-for-groups-with-more-than-10-vms"></a>Ik kan afhankelijkheden voor groepen met meer dan tien Vm's niet visualiseren?
U kunt [afhankelijkheden voor groepen](https://docs.microsoft.com/azure/migrate/how-to-create-group-dependencies) met Maxi maal tien vm's visualiseren. Als u een groep met meer dan tien vm's hebt, raden we u aan de groep te splitsen in kleinere groepen en de afhankelijkheden te visualiseren.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Ik heb agents geïnstalleerd en de visualisatie voor afhankelijkheden gebruikt om groepen te maken. Na een failover worden de acties ' agent installeren ' weer gegeven in plaats van ' afhankelijkheden weer geven '.
* Geplande of niet-geplande failover, on-premises machines zijn uitgeschakeld en gelijkwaardige computers zijn in Azure. Deze machines halen een ander MAC-adres op. Ze kunnen een ander IP-adres verkrijgen op basis van het feit of de gebruiker ervoor kiest om het lokale IP-adres te bewaren of niet. Als zowel MAC als IP-adressen verschillen, Azure Migrate de on-premises machines niet koppelen aan gegevens van Servicetoewijzing afhankelijkheden en vraagt de gebruiker om agents te installeren in plaats van afhankelijkheden weer te geven.
* De on-premises machines blijven op de juiste manier worden ingeschakeld. Gelijkwaardige computers die zich in azure bevinden, halen een ander MAC-adres op en kunnen een ander IP-adres verkrijgen. Tenzij de gebruiker uitgaande Azure Monitor verkeer van deze computers blokkeert, Azure Migrate de on-premises machines niet aan een Servicetoewijzing afhankelijkheids gegevens koppelen en vraagt de gebruiker om agents te installeren in plaats van afhankelijkheden weer te geven.

## <a name="collect-azure-portal-logs"></a>Azure Portal-logboeken verzamelen

**Azure Portal logboeken met netwerk verkeer Hoe kan ik verzamelen?**

1. Open de browser en navigeer [naar de portal en meld u aan](https://portal.azure.com).
2. Druk op F12 om de Ontwikkelhulpprogramma's te starten. Als dat nodig is, schakelt u de instelling **Wis vermeldingen wissen bij navigatie**uit.
3. Klik op het tabblad **netwerk** en begin met het vastleggen van netwerk verkeer:
   - In Chrome selecteert u **logboek behouden**. De opname moet automatisch worden gestart. Een rode cirkel geeft aan dat verkeer wordt vastgelegd. Als deze niet wordt weer gegeven, klikt u op de zwarte cirkel om te beginnen.
   - In micro soft Edge/IE moet de opname automatisch worden gestart. Als dat niet het geval is, klikt u op de groene knop afspelen.
4. Probeer de fout te reproduceren.
5. Nadat u de fout hebt aangetroffen tijdens het opnemen, stoppen met opnemen en opslaan van een kopie van de geregistreerde activiteit:
   - Klik in Chrome met de rechter muisknop op **Opslaan als har met inhoud**. Hiermee worden de logboeken gecomprimeerd en geëxporteerd als een. har-bestand.
   - Klik in micro soft Edge/IE op het pictogram **vastgelegd verkeer exporteren** . Hiermee wordt het logboek gecomprimeerd en geëxporteerd.
6. Ga naar het tabblad **console** om te controleren of er waarschuwingen of fouten zijn. Het console logboek opslaan:
   - Klik in Chrome met de rechter muisknop op een wille keurige plaats in het console logboek. Selecteer **Opslaan als**, om het logboek te exporteren en uit te voeren.
   - Klik in micro soft Edge/IE met de rechter muisknop op de fouten en selecteer **Alles kopiëren**.
7. Ontwikkelhulpprogramma's sluiten.

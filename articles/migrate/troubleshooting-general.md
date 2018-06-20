---
title: Problemen met Azure migreren | Microsoft Docs
description: Biedt een overzicht van bekende problemen in de service Azure migreren en tips voor veelvoorkomende fouten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 896e918f6031f3bc6b925a2ecdfa2a5c82f00e0b
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36228251"
---
# <a name="troubleshoot-azure-migrate"></a>Problemen met Azure Migrate oplossen

## <a name="troubleshoot-common-errors"></a>Veelvoorkomende problemen oplossen

[Azure migreren](migrate-overview.md) voor lokale werkbelastingen voor migratie naar Azure evalueert. Gebruik dit artikel bij het oplossen van problemen bij het implementeren en gebruiken Azure migreren.

### <a name="migration-project-creation-failed-with-error-requests-must-contain-user-identity-headers"></a>Migratie-project maken is mislukt met fout *aanvragen moeten gebruiker identiteit kopteksten bevatten*

Dit probleem kan optreden voor gebruikers die geen toegang tot de Azure Active Directory (Azure AD)-tenant van de organisatie. Wanneer een gebruiker is toegevoegd aan een Azure AD-tenant voor de eerste keer, ontvangt hij een uitnodiging voor e-mailbericht om door te nemen aan de tenant. Gebruikers moeten gaat u naar het e-mailadres en de uitnodiging accepteren voor ophalen is toegevoegd aan de tenant. Als u niet om te zien van het e-mailbericht, bereiken aan een gebruiker die al toegang heeft tot de tenant en vraagt u ze opnieuw verzenden van de uitnodiging die u met de opgegeven stappen [hier](https://docs.microsoft.com/azure/active-directory/b2b/add-users-administrator#resend-invitations-to-guest-users).

Zodra de voor een e-uitnodigingsmail is ontvangen, moet u het e-mailbericht openen en klik op de koppeling in het e-mailbericht de uitnodiging te accepteren. Zodra dit is gebeurd, moet u afmelden bij de Azure-portal en aanmelden, de browser te vernieuwen werkt niet. Vervolgens kunt u proberen het migratieproject wordt gemaakt.

### <a name="performance-data-for-disks-and-networks-adapters-shows-as-zeros"></a>Prestatiegegevens van schijven en netwerken adapters wordt weergegeven als nullen

Dit kan gebeuren als het niveau van de instelling statistieken op de vCenter-server is ingesteld op minder dan drie. Niveau 3 of hoger, slaat vCenter VM prestatiegeschiedenis voor berekening, opslag en netwerk. VCenter wordt niet opslaan opslag en gegevens van het netwerk, maar alleen de gegevens CPU en geheugen voor minder dan niveau drie. In dit scenario prestaties gegevens wordt weergegeven als nul in Azure migreren en grootte aanbeveling voor schijven en netwerken op basis van de metagegevens die zijn verzameld van de lokale machines migreren van Azure biedt.

Om de verzameling van prestatiegegevens van de schijf en netwerk, het niveau van de instellingen statistieken te drie te wijzigen. Vervolgens wacht ten minste een dag voor detectie van uw omgeving en voor het beoordelen.

### <a name="i-installed-agents-and-used-the-dependency-visualization-to-create-groups-now-post-failover-the-machines-show-install-agent-action-instead-of-view-dependencies"></a>Ik geïnstalleerde agents en de afhankelijkheid visualisatie gebruikt om groepen te maken. Nu boeken failover, de machines actie 'Agent installeren' in plaats van 'Afhankelijkheden weergave' weergeven
* Post geplande of niet-geplande failover on-premises machines zijn uitgeschakeld en gelijkwaardige machines in Azure worden ingezet. Deze machines verkrijgen van een ander MAC-adres. Ze kunnen verkrijgen van een ander IP-adres op basis van of de gebruiker heeft ervoor gekozen voor het bewaren van lokale IP-adres of niet. Als MAC- en IP-adressen verschillen, Azure Migreer de lokale computers niet is gekoppeld met de gegevens van een Serviceoverzicht afhankelijkheid en vraagt de gebruiker installeren van agents in plaats van de afhankelijkheden weer te geven.
* Na de testfailover, de lokale machines ingeschakeld blijven zoals verwacht. Gelijkwaardige machines ingezet in Azure ander MAC-adres verkrijgen en andere IP-adres kunnen verkrijgen. Tenzij de gebruiker uitgaande Log Analytics-verkeer van deze machines blokkeert, Azure Migreer de lokale computers niet is gekoppeld met de gegevens van een Serviceoverzicht afhankelijkheid en vraagt de gebruiker installeren van agents in plaats van de afhankelijkheden weer te geven.

## <a name="collector-errors"></a>Collector fouten

### <a name="deployment-of-collector-ova-failed"></a>Implementatie van de collector eicellen is mislukt

Dit kan gebeuren als de eicellen is gedeeltelijk gedownload of als gevolg van de browser als u de webclient vSphere gebruikt voor het implementeren van de eicellen. Zorg dat het downloaden voltooid is en probeer de eicellen met een andere browser implementeren.

### <a name="collector-is-not-able-to-connect-to-the-internet"></a>Collector kan geen verbinding maken met internet

Dit kan gebeuren wanneer de door u gebruikte computer zich achter een proxy. Zorg ervoor dat u de autorisatiereferenties opgeven als de proxy een moet.
Als u elke URL gebaseerde, firewall-proxy gebruikt om te bepalen uitgaande verbinding, moet aan de lijst met geaccepteerde dat deze URL's vereist:

**URL** | **Doel**  
--- | ---
*.portal.azure.com | Vereist connectiviteit met de Azure-service controleren en valideren tijdsynchronisatie verstrekt.
*.oneget.org | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module.

**De collector geen verbinding maken met het project met de project-ID en sleutel ik gekopieerd vanuit de portal.**

Zorg ervoor dat u hebt gekopieerd en geplakt, de juiste informatie. Installeer de Microsoft Monitoring Agent (MMA) om op te lossen, en controleer of als de MMA op het project als volgt aansluiten kunt:

1. Bij de collector VM, downloaden de [MMA](https://go.microsoft.com/fwlink/?LinkId=828603).
2. Dubbelklik op het gedownloade bestand voor het starten van de installatie.
3. In setup op de **Welkom** pagina, klikt u op **volgende**. Klik op de pagina **Licentievoorwaarden** op **Akkoord** om de licentie te accepteren.
4. In **doelmap**, behouden of wijzigen van de standaardinstallatiemap > **volgende**.
5. In **installatieopties voor Agent**, selecteer **Azure Log Analytics** > **volgende**.
6. Klik op **toevoegen** een nieuwe werkruimte voor logboekanalyse toevoegen. Plak de project-ID en-sleutel die u hebt gekopieerd. Klik op **Volgende**.
7. Controleer of de agent een verbinding aan het project maken kunt. Als dit niet, Controleer de instellingen. Als de agent verbinding maken kan maar de collector niet kunt, moet u contact op met ondersteuning.


### <a name="error-802-date-and-time-synchronization-error"></a>Fout 802: Datum en tijd synchronisatiefout

De klok op de server mogelijk out van de synchronisatie met de huidige tijd door meer dan vijf minuten. De kloktijd op de collector VM aan de huidige tijd als volgt wijzigen:

1. Open een opdrachtprompt beheerder op de virtuele machine.
2. Als u wilt controleren in de tijdzone, w32tm /tz worden uitgevoerd.
3. Als u wilt synchroniseren van de tijd, w32tm/resync worden uitgevoerd.

### <a name="vmware-powercli-installation-failed"></a>VMware PowerCLI installatie is mislukt

Azure migreren collector PowerCLI downloadt en installeert deze op het apparaat. In de installatie PowerCLI kan te wijten onbereikbaar eindpunten voor de opslagplaats PowerCLI. Als u wilt oplossen, probeert PowerCLI handmatig installeren in de collector VM die gebruikmaakt van de volgende stap:

1. Open Windows PowerShell in de beheerdersmodus
2. Ga naar de map C:\ProgramFiles\ProfilerService\VMWare\Scripts\
3. Voer het script InstallPowerCLI.ps1

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Er is een fout UnhandledException interne fout opgetreden: System.IO.FileNotFoundException

Dit is een probleem met Collector-versies lager dan 1.0.9.5. Als u werkt met Collector-versie 1.0.9.2 of pre-GA-versies zoals 1.0.8.59, hebt u met dit probleem te maken. Volg de [hier opgegeven koppeling naar de forums voor een uitgebreid antwoord](https://social.msdn.microsoft.com/Forums/azure/en-US/c1f59456-7ba1-45e7-9d96-bae18112fb52/azure-migrate-connect-to-vcenter-server-error?forum=AzureMigrate).

[Voer een upgrade van uw Collector uit om het probleem te verhelpen](https://aka.ms/migrate/col/checkforupdates).

### <a name="error-unabletoconnecttoserver"></a>Fout UnableToConnectToServer

Kan geen verbinding met de vCenter-Server 'Servername.com:9443' vanwege de volgende fout: Er is geen eindpunt luistert op https://Servername.com:9443/sdk die het bericht kan accepteren.

Als u de nieuwste versie van het toestel collector worden uitgevoerd, zo niet, upgrade van het toestel om te controleren de [meest recente versie](https://docs.microsoft.com/azure/migrate/concepts-collector#how-to-upgrade-collector).

Als het probleem wordt nog steeds in de meest recente versie gebeurt, kan zijn omdat de machine collector kan niet worden omgezet, de naam van de vCenter-Server opgegeven of de opgegeven poort is onjuist. Standaard, als de poort niet is gespecificeerd, probeert collector verbinding maken met het poortnummer 443.

1. Probeer te pingen de servernaam.com van de collector-machine.
2. Als stap 1 mislukt, probeert u via het IP-adres verbinding te maken met de vCenter-server.
3. Bepaal het juiste poortnummer om verbinding te maken met de vCenter-server.
4. Controleer tot slot of de vCenter-server actief is.

## <a name="troubleshoot-readiness-issues"></a>Gereedheidsproblemen oplossen

**Probleem** | **Fix**
--- | ---
Niet-ondersteund opstarttype | Azure biedt geen ondersteuning voor virtuele machines met EFI-opstarten-type. Het verdient het opstart-type converteren naar BIOS voordat u een migratie uitvoert. <br/><br/>U kunt [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) voor de migratie van dergelijke virtuele machines uitvoeren, zoals deze het type opstarten van de virtuele machine naar BIOS tijdens de migratie converteren wordt.
Voorwaardelijk ondersteunde Windows-besturingssysteem | Het besturingssysteem is verstreken het einde van de datum van de ondersteuning en moet een aangepaste ondersteunen overeenkomst (CSA) voor [ondersteuning in Azure](https://aka.ms/WSosstatement), moet u een upgrade van het besturingssysteem vóór de migratie naar Azure.
Niet-ondersteund Windows-besturingssysteem | Azure ondersteunt alleen [geselecteerde versies van Windows-besturingssysteem](https://aka.ms/WSosstatement), moet u een upgrade van het besturingssysteem van de machine voordat u migreert naar Azure.
Voorwaardelijk goedgekeurd Linux-besturingssysteem | Azure onderschrijft alleen [geselecteerde versies van Linux-besturingssysteem](../virtual-machines/linux/endorsed-distros.md), moet u een upgrade van het besturingssysteem van de machine voordat u migreert naar Azure.
Niet-goedgekeurd Linux-besturingssysteem | De machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure, moet u een upgrade van het besturingssysteem naar een [goedgekeurd door Linux-versie](../virtual-machines/linux/endorsed-distros.md) voordat u migreert naar Azure
Onbekend besturingssysteem | Het besturingssysteem van de virtuele machine is opgegeven als 'Overig' in vCenter Server, als gevolg van die Azure migreren niet de gereedheid van de virtuele machine van Azure identificeren. Zorg ervoor dat het besturingssysteem op de machine [ondersteund](https://aka.ms/azureoslist) door Azure voordat u de machine migreert.
Niet-ondersteunde hoeveelheid bits van besturingssysteem | Virtuele machines met 32-bits besturingssysteem kunnen worden opgestart in Azure, maar het wordt aanbevolen voor upgrade van het besturingssysteem van de virtuele machine van de 32-bits naar 64-bits voordat u migreert naar Azure.
Visual Studio-abonnement is vereist. | De machines is een Windows-client uitgevoerd binnen het die OS wordt alleen ondersteund in Visual Studio-abonnement.
De VM is niet gevonden voor de prestaties van de vereiste opslag. | De prestaties van de opslag (IOP's / doorvoer) vereist voor de machine overschrijdt de ondersteuning van de virtuele machine van Azure. Verminderen de opslagvereisten voor de machine vóór de migratie.
De VM is niet gevonden voor de vereiste netwerkprestaties. | De netwerkprestaties (in/out) vereist voor de machine overschrijdt de ondersteuning van de virtuele machine van Azure. Verminder de netwerkvereisten voor de machine.
De VM is niet gevonden in de opgegeven prijscategorie. | Als de prijscategorie is ingesteld op standaard, kunt u overwegen de virtuele machine downsizing voordat u migreert naar Azure. Als de sizing laag Basic is, kunt u de prijscategorie van de beoordeling wijzigen in standaard.
De VM is niet gevonden in de opgegeven locatie. | Gebruik een andere doellocatie vóór de migratie.
Een of meer schijven niet bruikbaar. | Een of meer schijven die zijn gekoppeld aan de virtuele machine niet voldoen aan de Azure-vereisten. Voor elke schijf die is gekoppeld aan de virtuele machine, zorg ervoor dat de grootte van de schijf < 4 TB, zo niet, de grootte van de schijf voordat u migreert naar Azure te verkleinen. Zorg ervoor dat de prestaties (IOP's / doorvoer) nodig is voor elke schijf wordt ondersteund door Azure [beheerde virtuele-machineschijven](https://docs.microsoft.com/azure/azure-subscription-service-limits#storage-limits).   
Een of meer ongeschikt netwerkadapters. | Verwijder de ongebruikte netwerkadapters van de machine vóór de migratie.
Het aantal schijven overschrijdt de limiet | Verwijder de ongebruikte schijven van de machine vóór de migratie.
De schijfgrootte overschrijdt de limiet | Azure biedt ondersteuning voor schijven met tot grootte 4 TB. Schijven verkleinen tot minder dan 4 TB vóór de migratie.
Schijf niet beschikbaar op de opgegeven locatie | Zorg ervoor dat de schijf is in de doellocatie voordat u migreert.
Schijf niet beschikbaar voor de opgegeven redundantie | De schijf moet het opslagtype voor redundantie gedefinieerd in de evaluatie-instellingen (LRS standaard) gebruiken.
Kan de geschiktheid van de schijf niet bepalen vanwege een interne fout | Maak een nieuwe beoordeling van de groep.
Er is geen VM met vereiste kernen en geheugen gevonden | Azure kan niet nauwkeurig een geschikte VM-type. Verminder het geheugen en het aantal kernen van de on-premises machine voordat u migreert.
Kan de geschiktheid van de virtuele machine vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
Kan de geschiktheid voor een of meer schijven vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
Kan de geschiktheid voor een of meer netwerkadapters vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.


## <a name="collect-logs"></a>Verzamelen van Logboeken

**Hoe ik logboeken op de VM-collector verzamelen?**

Logboekregistratie is standaard ingeschakeld. Logboeken bevinden als volgt:

- C:\Profiler\ProfilerEngineDB.sqlite
- C:\Profiler\Service.log
- C:\Profiler\WebApp.log

Ga als volgt te werk voor het verzamelen van Event Tracing voor Windows:

1. Open een PowerShell-opdrachtvenster op de collector VM.
2. Voer **Get EventLog - logboeknaam toepassing | export-csv eventlog.csv**.

**Hoe ik verkeerslogboeken portal netwerk verzamelen?**

1. Open de browser en ga en meld u bij [naar de portal](https://portal.azure.com).
2. Druk op F12 de ontwikkelhulpprogramma's te starten. Indien nodig, schakelt u de instelling **ingangen op navigatie**.
3. Klik op de **netwerk** tabblad en begint met het opnemen van netwerkverkeer:
 - Selecteer in het Chrome, **Preserve logboek**. De registratie wordt automatisch gestart. Een rode cirkel geeft aan dat verkeer vastleggen wordt. Als het niet wordt weergegeven, klikt u op de zwarte cirkel starten
 - In Edge/IE opname automatisch moet worden gestart. Als dit niet het geval is, klikt u op de knop afspelen groen.
4. Wilt u de fout optreedt.
5. Nadat u de fout tijdens het vastleggen van gegevens te maken hebt, opname stoppen en sla een kopie van de vastgelegde activiteit:
 - Chrome, met de rechtermuisknop op en klik op **opslaan als HAR met inhoud**. Dit bericht wordt nu en exporteert de logboeken als een .har-bestand.
 - In Edge/Internet Explorer, klikt u op de **Export verkeer vastgelegd** pictogram. Dit bericht wordt nu en exporteert u het logboek.
6. Navigeer naar de **Console** tabblad om te controleren op fouten of waarschuwingen worden gegenereerd. Het consolelogboek opslaan:
 - Chrome, met de rechtermuisknop op een willekeurige plaats in het consolelogboek. Selecteer **opslaan als**, om te exporteren en het logboek voor de zip.
 - In Edge/Internet Explorer, met de rechtermuisknop op de fouten en selecteer **Kopieer alle**.
7. Sluit de hulpprogramma's voor ontwikkelaars.

## <a name="collector-error-codes-and-recommended-actions"></a>Foutcodes collector en de aanbevolen acties

|           |                                |                                                                               |                                                                                                       |                                                                                                                                            |
|-----------|--------------------------------|-------------------------------------------------------------------------------|-------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| Foutcode | Naam van de fout                      | Bericht                                                                       | Mogelijke oorzaken                                                                                        | Aanbevolen actie                                                                                                                          |
| 601       | CollectorExpired               | De collector is verlopen.                                                        | De collector is verlopen.                                                                                    | Download een nieuwe versie van de collector en probeer het opnieuw.                                                                                      |
| 751       | UnableToConnectToServer        | Kan geen verbinding maken met de vCenter-server %Name; vanwege de fout %ErrorMessage;     | Controleer het foutbericht voor meer informatie.                                                             | Los het probleem op en probeer het opnieuw.                                                                                                           |
| 752       | InvalidvCenterEndpoint         | De server %Name; is geen vCenter-server.                                  | Geef de gegevens van de vCenter-server op.                                                                       | Probeer de bewerking opnieuw uit te voeren met de juiste vCenter-serverdetails.                                                                                   |
| 753       | InvalidLoginCredentials        | Kan geen verbinding maken met de vCenter-server %Name; vanwege een fout: %ErrorMessage; | De verbinding met de vCenter-server is mislukt wegens ongeldige aanmeldingsreferenties.                             | Controleer of de opgegeven aanmeldingsgegevens juist zijn.                                                                                    |
| 754       | NoPerfDataAvaialable           | De prestatiegegevens zijn niet beschikbaar.                                               | Controleer statistieken niveau in de vCenter-Server. Het moet worden ingesteld op 3 om prestatiegegevens beschikbaar. | Wijzig het statistiekniveau naar 3 (voor een duur van 5 minuten, 30 minuten en 2 uur) en probeer het na ten minste een dag opnieuw.                   |
| 756       | NullInstanceUUID               | Er is een machine met null InstanceUUID aangetroffen                                  | vCenter-server bevat mogelijk een onjuist object.                                                      | Los het probleem op en probeer het opnieuw.                                                                                                           |
| 757       | VMNotFound                     | Kan de virtuele machine niet vinden                                                  | Virtuele machines kunnen worden verwijderd: %VMID;                                                                | Zorg ervoor dat de virtuele machines zijn geselecteerd en er een bereik voor de vCenter-voorraad bestaat tijdens de detectie                                      |
| 758       | GetPerfDataTimeout             | Er is een time-out opgetreden voor de VCenter-aanvraag. Bericht % Message;                                  | Referenties van de vCenter-server zijn onjuist                                                              | Controleer de referenties van de vCenter-Server en zorg ervoor dat vCenter-Server bereikbaar is. De bewerking opnieuw proberen. Als het probleem zich blijft voordoen, neem dan contact op met ondersteuning. |
| 759       | VmwareDllNotFound              | Kan het DLL-bestand voor VMWare.Vim niet vinden.                                                     | PowerCLI is niet juist geïnstalleerd.                                                                   | Controleer of de PowerCLI goed is geïnstalleerd. De bewerking opnieuw proberen. Als het probleem zich blijft voordoen, neem dan contact op met ondersteuning.                               |
| 800       | ServiceError                   | De Azure Migrate Collector-service wordt niet uitgevoerd.                               | De Azure Migrate Collector-service wordt niet uitgevoerd.                                                       | Gebruik services.msc om de service te starten en voer de bewerking opnieuw uit.                                                                             |
| 801       | PowerCLIError                  | Installatie van VMware PowerCLI is mislukt.                                          | Installatie van VMware PowerCLI is mislukt.                                                                  | De bewerking opnieuw proberen. Als het probleem zich blijft voordoen, installeer handmatig en probeer het opnieuw.                                                   |
| 802       | TimeSyncError                  | De tijd is niet gesynchroniseerd met de tijdserver op internet.                            | De tijd is niet gesynchroniseerd met de tijdserver op internet.                                                    | Zorg ervoor dat de tijd op de computer juist is ingesteld voor de tijdzone van de computer en voer de bewerking opnieuw uit.                                 |
| 702       | OMSInvalidProjectKey           | Ongeldige projectsleutel opgegeven.                                                | Ongeldige projectsleutel opgegeven.                                                                        | Probeer de bewerking opnieuw uit te voeren met de projectsleutel.                                                                                              |
| 703       | OMSHttpRequestException        | Fout tijdens het verzenden van de aanvraag. Bericht % Message;                                | Controleer de project-id en -sleutel en controleer of het eindpunt bereikbaar is.                                       | De bewerking opnieuw proberen. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen.                                                                     |
| 704       | OMSHttpRequestTimeoutException | Er is een time-out opgetreden voor de HTTP-aanvraag. Bericht % Message;                                     | Controleer de project-id en -sleutel en controleer of het eindpunt bereikbaar is.                                       | De bewerking opnieuw proberen. Neem contact op met Microsoft Ondersteuning als het probleem zich blijft voordoen.                                                                     |

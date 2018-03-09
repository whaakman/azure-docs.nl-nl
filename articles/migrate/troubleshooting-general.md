---
title: Problemen met Azure migreren | Microsoft Docs
description: Biedt een overzicht van bekende problemen in de service Azure migreren en tips voor veelvoorkomende fouten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: troubleshooting
ms.date: 02/21/2018
ms.author: raynew
ms.openlocfilehash: e1e7a1a57f780ef477379dfb1ceaead0c8654970
ms.sourcegitcommit: 168426c3545eae6287febecc8804b1035171c048
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/08/2018
---
# <a name="troubleshoot-azure-migrate"></a>Problemen met Azure Migrate oplossen

## <a name="troubleshoot-common-errors"></a>Veelvoorkomende problemen oplossen

[Azure migreren](migrate-overview.md) voor lokale werkbelastingen voor migratie naar Azure evalueert. Gebruik dit artikel bij het oplossen van problemen bij het implementeren en gebruiken Azure migreren.


**Collector kan geen verbinding maken met internet**

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
3. In setup op de **Welkom** pagina, klikt u op **volgende**. Op de **licentievoorwaarden** pagina, klikt u op **ik ga akkoord** de licentievoorwaarden accepteren.
4. In **doelmap**, behouden of wijzigen van de standaardinstallatiemap > **volgende**.
5. In **installatieopties voor Agent**, selecteer **Azure logboekanalyse (OMS)** > **volgende**.
6. Klik op **toevoegen** een nieuwe werkruimte voor logboekanalyse toevoegen. Plak de project-ID en-sleutel die u hebt gekopieerd. Klik op **Volgende**.
7. Controleer of de agent een verbinding aan het project maken kunt. Als dit niet, Controleer de instellingen. Als de agent verbinding maken kan maar de collector niet kunt, moet u contact op met ondersteuning.


**Fout 802: krijg ik een synchronisatiefout datum en tijd.**

De klok op de server mogelijk out van de synchronisatie met de huidige tijd door meer dan vijf minuten. De kloktijd op de collector VM aan de huidige tijd als volgt wijzigen:

1. Open een opdrachtprompt beheerder op de virtuele machine.
2. Als u wilt controleren in de tijdzone, w32tm /tz worden uitgevoerd.
3. Als u wilt synchroniseren van de tijd, w32tm/resync worden uitgevoerd.

**Mijn project-sleutel heeft symbolen "==" aan het einde. Deze zijn gecodeerd met andere alfanumerieke tekens door de collector. Wordt dit verwacht?**

Ja, dat elke sleutel project eindigt met "==". De project-sleutel versleuteld de collector voordat deze wordt verwerkt.

**Prestatiegegevens van schijven en netwerken adapters wordt weergegeven als nullen**

Dit kan gebeuren als het niveau van de instelling statistieken op de vCenter-server is ingesteld op minder dan drie. Niveau 3 of hoger, slaat vCenter VM prestatiegeschiedenis voor berekening, opslag en netwerk. VCenter wordt niet opslaan opslag en gegevens van het netwerk, maar alleen de gegevens CPU en geheugen voor minder dan niveau drie. In dit scenario prestaties gegevens wordt weergegeven als nul in Azure migreren en grootte aanbeveling voor schijven en netwerken op basis van de metagegevens die zijn verzameld van de lokale machines migreren van Azure biedt.

Om de verzameling van prestatiegegevens van de schijf en netwerk, het niveau van de instellingen statistieken te drie te wijzigen. Vervolgens wacht ten minste een dag voor detectie van uw omgeving en voor het beoordelen. 

**Ik geïnstalleerde agents en de afhankelijkheid visualisatie gebruikt om groepen te maken. Nu boeken failover, de machines actie 'Agent installeren' in plaats van 'Afhankelijkheden weergave' weergeven**
* Post geplande of niet-geplande failover on-premises machines zijn uitgeschakeld en gelijkwaardige machines in Azure worden ingezet. Deze machines verkrijgen van een ander MAC-adres. Ze kunnen verkrijgen van een ander IP-adres op basis van of de gebruiker heeft ervoor gekozen voor het bewaren van lokale IP-adres of niet. Als MAC- en IP-adressen verschillen, Azure Migreer de lokale computers niet is gekoppeld met de gegevens van een Serviceoverzicht afhankelijkheid en vraagt de gebruiker installeren van agents in plaats van de afhankelijkheden weer te geven.
* Na de testfailover, de lokale machines ingeschakeld blijven zoals verwacht. Gelijkwaardige machines ingezet in Azure ander MAC-adres verkrijgen en andere IP-adres kunnen verkrijgen. Tenzij de gebruiker uitgaande OMS-verkeer van deze machines blokkeert, Azure Migreer de lokale computers niet is gekoppeld met de gegevens van een Serviceoverzicht afhankelijkheid en vraagt de gebruiker installeren van agents in plaats van de afhankelijkheden weer te geven.


## <a name="troubleshoot-readiness-issues"></a>Gereedheidsproblemen oplossen

**Probleem** | **Fix**
--- | ---
Niet-ondersteund opstarttype | Azure biedt geen ondersteuning voor virtuele machines met EFI-opstarten-type. Het verdient het opstart-type converteren naar BIOS voordat u een migratie uitvoert. <br/><br/>U kunt [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/tutorial-migrate-on-premises-to-azure) voor de migratie van dergelijke virtuele machines uitvoeren, zoals deze het type opstarten van de virtuele machine naar BIOS tijdens de migratie converteren wordt.
Het aantal schijven overschrijdt de limiet | Verwijder de ongebruikte schijven van de machine vóór de migratie.
De schijfgrootte overschrijdt de limiet | Azure biedt ondersteuning voor schijven met tot grootte 4 TB. Schijven verkleinen tot minder dan 4 TB vóór de migratie. 
Schijf niet beschikbaar op de opgegeven locatie | Zorg ervoor dat de schijf is in de doellocatie voordat u migreert.
Schijf niet beschikbaar voor de opgegeven redundantie | De schijf moet het opslagtype voor redundantie gedefinieerd in de evaluatie-instellingen (LRS standaard) gebruiken.
Kan de geschiktheid van de schijf niet bepalen vanwege een interne fout | Maak een nieuwe beoordeling van de groep. 
Er is geen VM met vereiste kernen en geheugen gevonden | Azure kan niet nauwkeurig een geschikte VM-type. Verminder het geheugen en het aantal kernen van de on-premises machine voordat u migreert. 
Een of meer schijven niet bruikbaar. | Zorg ervoor dat lokale schijven zijn 4 TB of onder voordat u een migratie uitvoert.
Een of meer ongeschikt netwerkadapters. | Verwijder de ongebruikte netwerkadapters van de machine vóór de migratie.
Kan de geschiktheid van de virtuele machine vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep. 
Kan de geschiktheid voor een of meer schijven vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
Kan de geschiktheid voor een of meer netwerkadapters vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
De VM is niet gevonden voor de prestaties van de vereiste opslag. | De prestaties van de opslag (IOP's / doorvoer) vereist voor de machine overschrijdt de ondersteuning van de virtuele machine van Azure. Verminderen de opslagvereisten voor de machine vóór de migratie.
De VM is niet gevonden voor de vereiste netwerkprestaties. | De netwerkprestaties (in/out) vereist voor de machine overschrijdt de ondersteuning van de virtuele machine van Azure. Verminder de netwerkvereisten voor de machine. 
De VM is niet gevonden in de opgegeven prijscategorie. | Als de prijscategorie is ingesteld op standaard, kunt u overwegen de virtuele machine downsizing voordat u migreert naar Azure. Als de sizing laag Basic is, kunt u de prijscategorie van de beoordeling wijzigen in standaard. 
De VM is niet gevonden in de opgegeven locatie. | Gebruik een andere doellocatie vóór de migratie.
Onbekend besturingssysteem | Het besturingssysteem van de virtuele machine is opgegeven als 'Overig' in vCenter Server, als gevolg van die Azure migreren niet de gereedheid van de virtuele machine van Azure identificeren. Zorg ervoor dat het besturingssysteem op de machine [ondersteund](https://aka.ms/azureoslist) door Azure voordat u de machine migreert.
Voorwaardelijk ondersteunde Windows-besturingssysteem | Het besturingssysteem is verstreken het einde van de datum van de ondersteuning en moet een aangepaste ondersteunen overeenkomst (CSA) voor [ondersteuning in Azure](https://aka.ms/WSosstatement), moet u een upgrade van het besturingssysteem vóór de migratie naar Azure.
Niet-ondersteunde Windows-besturingssysteem | Azure ondersteunt alleen [geselecteerde versies van Windows-besturingssysteem](https://aka.ms/WSosstatement), moet u een upgrade van het besturingssysteem van de machine voordat u migreert naar Azure. 
Voorwaardelijk goedgekeurd door Linux-besturingssysteem | Azure onderschrijft alleen [geselecteerde versies van Linux-besturingssysteem](../virtual-machines/linux/endorsed-distros.md), moet u een upgrade van het besturingssysteem van de machine voordat u migreert naar Azure.
Unendorsed Linux OS | De machine kan worden opgestart in Azure, maar er is geen ondersteuning van het besturingssysteem wordt verstrekt door Azure, moet u een upgrade van het besturingssysteem naar een [goedgekeurd door Linux-versie](../virtual-machines/linux/endorsed-distros.md) voordat u migreert naar Azure
Niet-ondersteunde hoeveelheid bits van besturingssysteem | Virtuele machines met 32-bits besturingssysteem kunnen worden opgestart in Azure, maar het wordt aanbevolen voor upgrade van het besturingssysteem van de virtuele machine van de 32-bits naar 64-bits voordat u migreert naar Azure.
Visual Studio-abonnement is vereist. | De machines is een Windows-client uitgevoerd binnen het die OS wordt alleen ondersteund in Visual Studio-abonnement.


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
 

## <a name="vcenter-errors"></a>vCenter fouten

### <a name="error-unhandledexception-internal-error-occured-systemiofilenotfoundexception"></a>Er is een fout UnhandledException interne fout opgetreden: System.IO.FileNotFoundException

Dit is een probleem dat wordt weergegeven op de Collector-versies lager dan 1.0.9.5. Als u zich op een Collector versie 1.0.9.2 of pre-GA versies zoals 1.0.8.59, kunt u dit probleem ervaart. Ga als volgt de [koppelen gegeven hier in de forums voor een gedetailleerde antwoord](https://social.msdn.microsoft.com/Forums/azure/en-US/c1f59456-7ba1-45e7-9d96-bae18112fb52/azure-migrate-connect-to-vcenter-server-error?forum=AzureMigrate).

[Upgrade van uw Collector als het probleem wilt oplossen](https://aka.ms/migrate/col/checkforupdates).

### <a name="error-unabletoconnecttoserver"></a>Fout UnableToConnectToServer

Kan geen verbinding met de vCenter-Server 'Servername.com:9443' vanwege de volgende fout: Er is geen eindpunt luistert op https://Servername.com:9443/sdk die het bericht kan accepteren.

Dit gebeurt wanneer de verzamelaar machine kan niet worden omgezet van de opgegeven naam van de vCenter-server of de poort speficified is onjuist. Standaard, als de poort niet is gespecificeerd, probeert Collector verbinding maken met het poortnummer 443.

1. Probeer te pingen de servernaam.com van de Collector-machine.
2. Als u stap 1 mislukt, probeert verbinding maken met de vCenter-server via IP-adres.
3. Het juiste poortnummer verbinding maken met de vCenter identificeren.
4. Ten slotte kunt u controleren of de vCenter-server actief is.
 


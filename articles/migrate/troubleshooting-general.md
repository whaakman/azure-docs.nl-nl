---
title: Problemen met Azure migreren | Microsoft Docs
description: Biedt een overzicht van bekende problemen in de service Azure migreren en tips voor veelvoorkomende fouten.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: troubleshooting
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 1fcc9e12e63eda73d53ae2085bc2a64d31ea2067
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="troubleshoot-azure-migrate"></a>Problemen met Azure Migrate oplossen

## <a name="troubleshoot-common-errors"></a>Veelvoorkomende fouten oplossen

[Azure migreren](migrate-overview.md) voor lokale werkbelastingen voor migratie naar Azure evalueert. Gebruik dit artikel bij het oplossen van problemen bij het implementeren en gebruiken Azure migreren.


**Collector kan geen verbinding maken met internet**

Dit kan gebeuren wanneer de door u gebruikte computer zich achter een proxy. Zorg ervoor dat u de autorisatiereferenties opgeven als de proxy een moet.
Als u elke URL gebaseerde, firewall-proxy gebruikt om te bepalen uitgaande verbinding, moet aan de lijst met geaccepteerde dat deze URL's vereist:

**URL** | **Doel**  
--- | ---
*. portal.azure.com | Vereist connectiviteit met de Azure-service controleren en valideren tijdsynchronisatie verstrekt.
*. oneget.org | Vereist voor het downloaden van de powershell op basis van vCenter PowerCLI module.

**De collector geen verbinding maken met het project met de project-ID en sleutel ik gekopieerd vanuit de portal.**

Zorg ervoor dat u hebt gekopieerd en geplakt, de juiste informatie. Als u wilt oplossen, installeert u Microsoft Monitoring Agent (MMA) als volgt:

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

**Probleem** | **Oplossen**
--- | ---
Opstarten type wordt niet ondersteund | Wijzig in BIOS voordat u een migratie uitvoert.
Het aantal schijven is overschreden | Verwijder de ongebruikte schijven van de machine vóór de migratie.
Schijfgrootte overschrijdt de limiet | Schijven verkleinen tot minder dan 4 TB vóór de migratie. 
Schijf die in de opgegeven locatie niet beschikbaar | Zorg ervoor dat de schijf is in de doellocatie voordat u migreert.
Niet beschikbaar voor de redundantie van de opgegeven schijf | De schijf moet het opslagtype voor redundantie gedefinieerd in de evaluatie-instellingen (LRS standaard) gebruiken.
Kan de schijf geschiktheid vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep. 
Virtuele machine met de vereiste kernen en het geheugen is niet gevonden | Azure kan niet nauwkeurig een geschikte VM-type. Verminder het geheugen en het aantal kernen van de on-premises machine voordat u migreert. 
Een of meer schijven niet bruikbaar. | Zorg ervoor dat lokale schijven zijn 4 TB of onder voordat u een migratie uitvoert.
Een of meer ongeschikt netwerkadapters. | Verwijder de ongebruikte netwerkadapters van de machine vóór de migratie.
Kan de geschiktheid van de virtuele machine vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep. 
Kan de geschiktheid voor een of meer schijven vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
Kan de geschiktheid voor een of meer netwerkadapters vanwege een interne fout niet bepalen. | Maak een nieuwe beoordeling van de groep.
De VM is niet gevonden voor de prestaties van de vereiste opslag. | De prestaties van de opslag (IOP's / doorvoer) vereist voor de machine overschrijdt de ondersteuning van de virtuele machine van Azure. Verminderen de opslagvereisten voor de machine vóór de migratie.
De VM is niet gevonden voor de vereiste netwerkprestaties. | De netwerkprestaties (in/out) vereist voor de machine overschrijdt de ondersteuning van de virtuele machine van Azure. Verminder de netwerkvereisten voor de machine. 
De VM is niet gevonden voor de opgegeven prijscategorie. | Controleer de instellingen voor de prijscategorie laag. 
De VM is niet gevonden in de opgegeven locatie. | Gebruik een andere doellocatie vóór de migratie.
Problemen met ondersteuning voor Linux-besturingssysteem | Zorg ervoor dat je actief 64-bits met deze ondersteund [besturingssystemen](../virtual-machines/linux/endorsed-distros.md).
Problemen met ondersteuning voor Windows-besturingssysteem | Zorg ervoor dat u een ondersteund besturingssysteem uitvoert. [Meer informatie](concepts-assessment-calculation.md#azure-suitability-analysis)
Onbekend besturingssysteem. | Controleer of het besturingssysteem die is opgegeven in vCenter juist is en herhaal het detectieproces.
Visual Studio-abonnement is vereist. | Windows-clientbesturingssystemen worden alleen ondersteund voor abonnementen van Visual Studio (MSDN).


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
 




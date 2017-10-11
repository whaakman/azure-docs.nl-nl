---
title: PowerShell-Connector | Microsoft Docs
description: Dit artikel wordt beschreven hoe u van Microsoft Windows PowerShell Connector configureert.
services: active-directory
documentationcenter: 
author: AndKjell
manager: femila
editor: 
ms.assetid: 6dba8e34-a874-4ff0-90bc-bd2b0a4199b5
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 0e5ccf5a38072e31d85bbc63eb0c608b0c34cfc2
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="windows-powershell-connector-technical-reference"></a>Technische documentatie van Windows PowerShell-Connector
In dit artikel beschrijft de Windows PowerShell-Connector. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Hotfix 4.1.3671.0 moet gebruiken of later [KB3092178](https://support.microsoft.com/kb/3092178).

Voor MIM2016 en FIM2010R2, de Connector is gedownload van de [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

## <a name="overview-of-the-powershell-connector"></a>Overzicht van de PowerShell-Connector
De PowerShell-Connector kunt u de synchronization service integratie met externe systemen die worden geboden door dat Windows PowerShell gebaseerde API's. De connector biedt een brug tussen de mogelijkheden van de beheeragent op basis van oproep extensible connectivity 2 (ECMA2) framework en Windows PowerShell. Zie voor meer informatie over het framework ECMA de [Extensible Connectivity 2.2 Management Agent Reference](https://msdn.microsoft.com/library/windows/desktop/hh859557.aspx).

### <a name="prerequisites"></a>Vereisten
Zorg voordat u de Connector gebruikt, hebt u de volgende op de server voor de synchronisatie:

* Microsoft.NET 4.5.2 of hoger
* Windows PowerShell 2.0, 3.0 of 4.0

Het uitvoeringsbeleid op de server van de synchronisatie moet worden geconfigureerd, zodat de connector voor het uitvoeren van Windows PowerShell-scripts. Tenzij de scripts die de connector wordt uitgevoerd digitaal zijn ondertekend, configureert het uitvoeringsbeleid door deze opdracht uit te voeren:  
`Set-ExecutionPolicy -ExecutionPolicy RemoteSigned`

## <a name="create-a-new-connector"></a>Een nieuwe Connector maken
Voor het maken van een Windows PowerShell-connector in de synchronisatieservice, moet u een reeks Windows PowerShell-scripts die de stappen die zijn aangevraagd door de synchronisatieservice uitvoeren opgeven. Afhankelijk van de gegevensbron waarmee u verbinding met maken en de functionaliteit die u nodig hebt, varieert de scripts moeten worden geïmplementeerd. Deze sectie bevat een overzicht van elk van de scripts die kunnen worden geïmplementeerd en wanneer ze zijn vereist.

De Windows PowerShell-connector is ontworpen voor het opslaan van elk van de scripts in de database van de synchronisatieservice. Tijdens het uitvoeren van scripts die zijn opgeslagen op het bestandssysteem mogelijk is, is het gemakkelijker om de hoofdtekst van elk script rechtstreeks in de configuratie van de connector.

Maken van een PowerShell-connector in **synchronisatieservice** Selecteer **beheeragent** en **maken**. Selecteer de **PowerShell (Microsoft)** Connector.

![Connector maken](./media/active-directory-aadconnectsync-connector-powershell/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
Parameters voor de configuratie voor het verbinden met een extern systeem te geven. Deze waarden zijn veilig opgeslagen door de synchronisatieservice en beschikbaar gesteld aan uw Windows PowerShell-scripts wanneer de connector wordt uitgevoerd.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-powershell/connectivity.png)

U kunt de volgende parameters voor de connectiviteit configureren:

**Connectiviteit**

| Parameter | Standaardwaarde | Doel |
| --- | --- | --- |
| Server |<Blank> |De servernaam die de connector verbinding moet maken. |
| Domein |<Blank> |Domein van de referentie voor het opslaan voor gebruik wanneer de connector wordt uitgevoerd. |
| Gebruiker |<Blank> |De gebruikersnaam van de referentie voor het opslaan voor gebruik wanneer de connector wordt uitgevoerd. |
| Wachtwoord |<Blank> |Wachtwoord van de referentie voor het opslaan voor gebruik wanneer de connector wordt uitgevoerd. |
| Connector-Account imiteren |False |Indien true, wordt de Windows PowerShell-scripts uitgevoerd in de context van de opgegeven referenties van de synchronisatieservice. Indien mogelijk, verdient het aanbeveling om de **$Credentials** parameter is doorgegeven aan elk script wordt gebruikt in plaats van imitatie. Zie voor meer informatie over aanvullende machtigingen die nodig zijn voor gebruik deze optie [aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation). |
| Gebruikersprofiel laden als u imitatie |False |Hiermee geeft u in Windows te laden van het gebruikersprofiel van de referenties van de connector tijdens de imitatie. Als de geïmiteerde gebruiker een zwervend profiel heeft, wordt in de connector het zwervende profiel niet laden. Zie voor meer informatie over aanvullende machtigingen die nodig zijn voor het gebruik van deze parameter [aanvullende configuratie voor imitatie](#additional-configuration-for-impersonation). |
| Aanmeldingstype als u imitatie |Geen |Aanmeldingstype tijdens de imitatie. Zie voor meer informatie de [dwLogonType] [ dw] documentatie. |
| Alleen ondertekende Scripts |False |Indien waar, wordt de Windows PowerShell connector valideert dat elk script een geldige digitale handtekening heeft. Als het ONWAAR is, zorg ervoor dat de synchronisatieservice-server Windows PowerShell-uitvoeringsbeleid RemoteSigned of onbeperkt. |

**Algemene Module**  
De connector kunt u voor het opslaan van een gedeelde Windows PowerShell-module in de configuratie. Wanneer u een script van de connector wordt uitgevoerd, wordt de Windows PowerShell-module naar het bestandssysteem geëxtraheerd zodat deze kan worden geïmporteerd door elk script.

Voor Import, Export en Wachtwoordsynchronisatie, scripts, wordt de algemene module uitgepakt naar de connector MAData map. Voor detectie-scripts, Schema, validatie, hiërarchie en partitie, wordt de algemene module uitgepakt naar de map % TEMP %. In beide gevallen moet het uitgepakte algemene Module-script met de naam volgens de instelling van de algemene naam van de Module-Script.

Gebruik de volgende instructie voor het laden van een module FIMPowerShellConnectorModule.psm1 aangeroepen vanuit de map MAData:`Import-Module (Join-Path -Path [Microsoft.MetadirectoryServices.MAUtils]::MAFolder -ChildPath "FIMPowerShellConnectorModule.psm1")`

Als u wilt een FIMPowerShellConnectorModule.psm1 aangeroepen vanuit de map % TEMP %-module laden, moet u de volgende instructie gebruiken:`Import-Module (Join-Path -Path $env:TEMP -ChildPath "FIMPowerShellConnectorModule.psm1")`

**De validatie van parameter**  
Het validatiescript is een optionele Windows PowerShell-script dat kan worden gebruikt om ervoor te zorgen dat de connector-configuratieparameters die is opgegeven door de beheerder geldig zijn. Validerende server, verbindingsreferenties en connectiviteit parameters zijn algemene gebruik van het validatiescript. Het validatiescript wordt aangeroepen na de volgende tabbladen en dialoogvensters worden gewijzigd:

* Connectiviteit
* Globale Parameters
* Partitieconfiguratie

Het validatiescript ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameterPage |[ConfigParameterPage][cpp] |Tabblad configuratie of het dialoogvenster waarmee de validatieaanvraag is geactiveerd. |
| ConfigParameters |[KeyedCollection] [ keyk] [tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |

Het validatiescript moet een enkel ParameterValidationResult-object retourneren aan de pijplijn.

**Schema-detectie**  
Het Schema detectiescript is verplicht. Dit script retourneert de objecttypen, kenmerken en kenmerk beperkingen die de synchronisatieservice wordt gebruikt voor het kenmerk stroomregels configureren. Het Schema detectiescript tijdens het maken van de connector wordt uitgevoerd en de connector schema gevuld. Dit wordt ook gebruikt door de actie voor Schema vernieuwen Synchronization Service Manager.

Het detectiescript schema ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection] [ keyk] [tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |

Het script moet een enkel retourneren [Schema] [ schema] object aan de pijplijn. De Schema-object bestaat uit [SchemaType] [ schemaT] objecten die objecttypen vertegenwoordigen (bijvoorbeeld: gebruikers en groepen). Het object SchemaType bevat een verzameling van [SchemaAttribute] [ schemaA] -objecten met daarin de kenmerken (bijvoorbeeld: voornaam en achternaam postadres) van het type.

**Extra Parameters**  
Naast de standaard configuratie-instellingen kunt u aanvullende aangepaste configuratie-instellingen die specifiek voor het exemplaar van de Connector zijn definiëren. Deze parameters kunnen worden opgegeven met de connector-partitie, of Voer stap niveaus en is toegankelijk vanuit de relevante Windows PowerShell-script. Aangepaste configuratie-instellingen kunnen worden opgeslagen in de database-synchronisatieservice in tekst zonder opmaak of ze kunnen worden versleuteld. De synchronisatieservice automatisch versleuteld en ontsleuteld secure configuratie-instellingen, indien nodig.

Aangepaste configuratie-instellingen opgeven, scheidt u de naam van elke parameter met een komma (,).

Voor toegang tot aangepaste configuratie-instellingen van een script, moet u de naam met een onderstrepingsteken achtervoegsel ( \_ ) en het bereik van de parameter (Global, partitie of RunStep). Gebruik bijvoorbeeld deze codefragment voor toegang tot de globale FileName-parameter moet:`$ConfigurationParameters["FileName_Global"].Value`

### <a name="capabilities"></a>Functionaliteit
Het tabblad mogelijkheden van de ontwerpfunctie voor Management Agent definieert het gedrag en de functionaliteit van de connector. De selecties op dit tabblad worden niet gewijzigd wanneer de connector is gemaakt. Deze tabel bevat de instellingen voor apparaatfuncties.

![Functionaliteit](./media/active-directory-aadconnectsync-connector-powershell/capabilities.png)

| Mogelijkheid | Beschrijving |
| --- | --- |
| [Stijl van de DN-naam][dnstyle] |Hiermee wordt aangegeven als de connector DN-namen ondersteunt en als het geval is, wat stijl. |
| [Type exporteren][exportT] |Bepaalt het type objecten die worden aangeboden aan het script exporteren. <li>AttributeReplace – omvat de volledige set met waarden voor een kenmerk met meerdere waarden wanneer het kenmerk is gewijzigd.</li><li>AttributeUpdate – bevat alleen de delta's naar een kenmerk met meerdere waarden wanneer het kenmerk is gewijzigd.</li><li>MultivaluedReferenceAttributeUpdate - bevat een volledige set met waarden voor niet-verwijzingsvariant met meerdere waarden kenmerken en alleen delta's voor verwijzingskenmerken met meerdere waarden.</li><li>ObjectReplace – bevat alle kenmerken voor een object wanneer een kenmerk wijzigingen</li> |
| [Gegevensnormalisatie][DataNorm] |Hiermee geeft u de Synchronization Service normaliseren van kenmerken anker voordat ze worden aangeboden aan scripts. |
| [Bevestiging van object][oconf] |Hiermee configureert u het gedrag in behandeling zijnde importeren in de synchronisatieservice. <li>Normaal – standaardgedrag die alle geëxporteerde wijzigingen worden bevestigd via importeren verwacht</li><li>NoDeleteConfirmation – is wanneer een object wordt verwijderd, er geen in behandeling zijnde importeren die zijn gegenereerd.</li><li>NoAddAndDeleteConfirmation – is wanneer een object wordt gemaakt of verwijderd, geen in behandeling zijnde importeren die zijn gegenereerd.</li> |
| DN-naam gebruiken als anker |Als de Distinguished Name-stijl is ingesteld op LDAP, is het ankerkenmerk voor het connectorgebied overgebracht ook de DN-naam. |
| Gelijktijdige bewerkingen van verschillende Connectors |Wanneer dit selectievakje inschakelt, kunnen meerdere Windows PowerShell-connectors gelijktijdig uitgevoerd. |
| Partities |Wanneer dit selectievakje inschakelt, biedt de connector ondersteuning voor meerdere partities en detectie van de partitie. |
| hiërarchie |Wanneer dit selectievakje inschakelt, wordt door de connector een hiërarchische structuur voor LDAP-stijl die wordt ondersteund. |
| Importeren inschakelen |Wanneer dit selectievakje inschakelt, importeert de connector gegevens via scripts voor het importeren. |
| Delta-Import inschakelen |Wanneer dit selectievakje inschakelt, kan de connector delta's aanvragen bij de scripts voor het importeren. |
| Exporteren inschakelen |Wanneer dit selectievakje inschakelt, exporteert u de connector gegevens via de export-scripts. |
| Volledige exporteren inschakelen |Wanneer dit selectievakje inschakelt, wordt de export-scripts ondersteunen exporteren van het gehele connectorgebied overgebracht. Voor het gebruik van deze optie moet inschakelen exporteren ook worden gecontroleerd. |
| Er is geen referentiewaarden In eerste Export Pass |Wanneer dit selectievakje inschakelt, worden in een tweede stap van de export verwijzingskenmerken geëxporteerd. |
| Wijzig de naam van Object inschakelen |Wanneer dit selectievakje inschakelt, kunnen de DN-namen worden gewijzigd. |
| Als u vervanging verwijderen toevoegen |Wanneer dit selectievakje inschakelt, delete-toevoegen bewerkingen worden geëxporteerd als één vervanging. |
| Wachtwoord Operations inschakelen |Wanneer dit selectievakje inschakelt, worden wachtwoord synchronisatiescripts ondersteund. |
| Wachtwoord van de Export in de eerste keer inschakelen |Wanneer dit selectievakje inschakelt, worden de wachtwoorden zijn ingesteld tijdens het inrichten geëxporteerd als het object is gemaakt. |

### <a name="global-parameters"></a>Globale Parameters
Het tabblad algemene Parameters in de ontwerpfunctie voor Management Agent kunt u de Windows PowerShell-scripts die worden uitgevoerd door de connector te configureren. U kunt ook algemene waarden voor aangepaste configuratie-instellingen die zijn gedefinieerd op het tabblad verbindingen configureren.

**Detectie van de partitie**  
Een partitie is een afzonderlijke naamruimte binnen één gedeelde schema. In Active Directory bijvoorbeeld is elk domein een partitie binnen een forest. Een partitie is de logische groepering voor importeren en exporteren van bewerkingen. Importeren en exporteren hebben partitie wat een context en alle bewerkingen in deze context. Partities moeten vertegenwoordigen een hiërarchie in LDAP. De DN-naam van een partitie wordt gebruikt in importeren om te controleren of alle geretourneerde objecten binnen het bereik van een partitie. De partitie DN-naam wordt ook gebruikt tijdens het inrichten van metaverse naar het connectorgebied overgebracht om te bepalen van de partitie die een object gekoppeld tijdens het exporteren worden moet.

Het detectiescript partitie ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |

Het script moet retourneren een ofwel één [partitie] [ part] object of een lijst [T] met partitie-objecten aan de pijplijn.

**Detectie hiërarchieën**  
Het detectiescript hiërarchie wordt alleen gebruikt wanneer de mogelijkheid Distinguished Name stijl LDAP is. Het script wordt gebruikt voor kunt u bladeren en selecteert u een reeks wordt beschouwd als containers in of buiten het bereik voor het importeren en exporteren van bewerkingen. Het script dient alleen een lijst met knooppunten die directe onderliggende elementen van het hoofdknooppunt dat is doorgegeven aan het script.

Het detectiescript hiërarchie ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| ParentNode |[HierarchyNode][hn] |Het hoofdknooppunt van de hiërarchie waarin het script directe onderliggende elementen moet retourneren. |

Het script moet retourneren een ofwel een HierarchyNode één onderliggend object of een lijst [T] van de onderliggende HierarchyNode objecten aan de pijplijn.

#### <a name="import"></a>Importeren
Connectors die ondersteuning bieden voor importbewerkingen moeten drie scripts implementeren.

**Beginnen met importeren**  
Het begin import-script wordt uitgevoerd aan het begin van een stap importeren uitvoert. Tijdens deze stap kunt u geen verbinding maken met het bronsysteem en voorbereidende stappen voordat u gegevens importeert uit het systeem verbonden.

Het importscript begin ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Het script geeft een melding over het type partitie importeren uitvoeren (delta of volledige), hiërarchie watermerk en verwachte paginagrootte. |
| Typen |[Schema][schema] |Schema voor de connectorruimte die is geïmporteerd. |

Het script moet een enkel retourneren [OpenImportConnectionResults] [ oicres] object aan de pijplijn, bijvoorbeeld:`Write-Output (New-Object Microsoft.MetadirectoryServices.OpenImportConnectionResults)`

**Gegevens importeren**  
Het script voor importeren van gegevens wordt aangeroepen door de connector totdat het script geeft aan dat er geen gegevens meer importeren. De Windows PowerShell-connector heeft een paginagrootte van 9.999 objecten. Als uw script meer dan 9.999 objecten voor importeren retourneert, moet u ondersteuning voor paginering. De connector zichtbaar gemaakt een aangepaste eigenschap waarmee u naar een winkel een watermerk kunt zodat elke keer dat het script voor importeren van gegevens wordt genoemd, uw script hervatten importeren van objecten waarop dit werd afgebroken.

Het importscript gegevens ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| GetImportEntriesRunStep |[ImportRunStep][irs] |Bevat het watermerk (CustomData) die kan worden gebruikt tijdens wisselbaar geheugen: invoer- en delta importeert. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Het script geeft een melding over het type partitie importeren uitvoeren (delta of volledige), hiërarchie watermerk en verwachte paginagrootte. |
| Typen |[Schema][schema] |Schema voor de connectorruimte die is geïmporteerd. |

Het script voor importeren van gegevens moet een lijst schrijven [[CSEntryChange][csec]] object aan de pijplijn. Deze verzameling bestaat uit CSEntryChange kenmerken voor elk object dat wordt geïmporteerd. Deze verzameling moet een volledige set met CSEntryChange-objecten met alle kenmerken voor elk object hebben tijdens de uitvoering van een volledige Import. Tijdens een Delta importeert, moet het object CSEntryChange ofwel het kenmerk niveau delta's voor elk object om te importeren of een volledige weergave van de objecten die zijn gewijzigd (modus vervangen) bevatten.

**Einde importeren**  
Aan het einde van het importproces worden uitgevoerd, wordt het importeren van End-script uitgevoerd. Dit script moet een Opschoningstaken nodig (bijvoorbeeld sluiten verbindingen met systemen) en reageren op fouten uitvoeren.

Het importscript end ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| OpenImportConnectionRunStep |[OpenImportConnectionRunStep][oicrs] |Het script geeft een melding over het type partitie importeren uitvoeren (delta of volledige), hiërarchie watermerk en verwachte paginagrootte. |
| CloseImportConnectionRunStep |[CloseImportConnectionRunStep][cecrs] |Het script informeert over de reden dat het importeren is beëindigd. |

Het script moet een enkel retourneren [CloseImportConnectionResults] [ cicres] object aan de pijplijn, bijvoorbeeld:`Write-Output (New-Object Microsoft.MetadirectoryServices.CloseImportConnectionResults)`

#### <a name="export"></a>Exporteren
Identiek aan de import-architectuur van de connector, connectors die ondersteuning bieden voor uitvoer moeten worden geïmplementeerd drie scripts.

**Begin exporteren**  
Het begin export-script wordt uitgevoerd aan het begin van een stap exporteren worden uitgevoerd. Tijdens deze stap kunt u geen verbinding maken met het bronsysteem en eventuele voorbereidende stappen uitvoeren voordat u gegevens naar het verbonden systeem exporteert.

Het exportscript begin ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Het script geeft een melding over het type uitvoer uitvoeren (delta of volledige) partitie, hiërarchie en verwachte paginagrootte. |
| Typen |[Schema][schema] |Schema voor de connectorruimte die wordt geëxporteerd. |

Het script moet retourneert geen uitvoer voor de pijplijn.

**Gegevens exporteren**  
De synchronisatieservice roept het script voor gegevens exporteren zo vaak als nodig is voor het verwerken van in behandeling zijnde uitvoer. Als het connectorgebied overgebracht meer in behandeling zijnde uitvoer dan de paginagrootte van de connector heeft, kan het script voor uitvoer gegevens worden aangeroepen meermaals en mogelijk meerdere keren voor hetzelfde object.

Het script voor uitvoer gegevens ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| CSEntries |IList[CSEntryChange][csec] |Lijst van het connectorgebied overgebracht objecten met in behandeling zijnde uitvoer moeten worden verwerkt tijdens deze fase. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Het script geeft een melding over het type uitvoer uitvoeren (delta of volledige) partitie, hiërarchie en verwachte paginagrootte. |
| Typen |[Schema][schema] |Schema voor de connectorruimte die wordt geëxporteerd. |

Het script voor uitvoer gegevens moet retourneren een [PutExportEntriesResults] [ peeres] object aan de pijplijn. Dit object hoeft niet te bevatten informatie voor elke geëxporteerde connector resultaat tenzij er een fout of een wijziging in de ankerkenmerk optreedt. Als u bijvoorbeeld een object PutExportEntriesResults geretourneerd aan de pijplijn:`Write-Output (New-Object Microsoft.MetadirectoryServices.PutExportEntriesResults)`

**Einde exporteren**  
Aan het einde van de export uitvoert, het exporteren van End-script uit te voeren. Dit script moet een Opschoningstaken nodig (bijvoorbeeld sluiten verbindingen met systemen) en reageren op fouten uitvoeren.

Het exportscript end ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| OpenExportConnectionRunStep |[OpenExportConnectionRunStep][oecrs] |Het script geeft een melding over het type uitvoer uitvoeren (delta of volledige) partitie, hiërarchie en verwachte paginagrootte. |
| CloseExportConnectionRunStep |[CloseExportConnectionRunStep][cecrs] |Het script informeert over de reden dat het exporteren is beëindigd. |

Het script moet retourneert geen uitvoer voor de pijplijn.

#### <a name="password-synchronization"></a>Wachtwoordsynchronisatie
Windows PowerShell-connectors kunnen worden gebruikt als doel voor wachtwoorden wijzigingen.

Het script wachtwoord ontvangt de volgende parameters van de connector:

| Naam | Gegevenstype | Beschrijving |
| --- | --- | --- |
| ConfigParameters |[KeyedCollection][keyk][tekenreeks, [ConfigParameter][cp]] |Tabel met de parameters voor de configuratie voor de Connector. |
| Referentie |[PSCredential][pscred] |Bevat geen referenties opgegeven door de beheerder op het tabblad verbinding. |
| Partitie |[Partitie][part] |Directory-partitie die de CSEntry in. |
| CSEntry |[CSEntry][cse] |Connector ruimte vermelding voor het object dat is ontvangen een wachtwoord wijzigen of opnieuw instellen. |
| OperationType |Tekenreeks |Hiermee wordt aangegeven of de bewerking opnieuw instellen (**SetPassword**) of een wijziging (**ChangePassword**). |
| PasswordOptions |[PasswordOptions][pwdopt] |Vlaggen die een wachtwoord opgeven voor het beoogde instellen gedrag. Deze parameter is alleen beschikbaar als OperationType **SetPassword**. |
| OudWachtwoord |Tekenreeks |Ingevuld met de oude wachtwoord van het object voor wachtwoordwijzigingen. Deze parameter is alleen beschikbaar als OperationType **ChangePassword**. |
| NieuwWachtwoord |Tekenreeks |Gevuld met een nieuw wachtwoord in van het object dat het script moet worden ingesteld. |

Het script wachtwoord wordt niet verwacht geen resultaten geretourneerd aan de Windows PowerShell-pijplijn. Als er een fout optreedt in het script wachtwoord moet een van de volgende uitzonderingen op de synchronisatieservice informeren over het probleem door het script genereert:

* [PasswordPolicyViolationException] [ pwdex1] – gegenereerd als het wachtwoord voldoet niet aan het wachtwoordbeleid in het systeem verbonden.
* [PasswordIllFormedException] [ pwdex2] – gegenereerd als het wachtwoord niet geschikt voor het verbonden systeem is.
* [PasswordExtension] [ pwdex3] – gegenereerd voor alle andere fouten in het script wachtwoord.

## <a name="sample-connectors"></a>Voorbeeld-Connectors
Zie voor een volledig overzicht van de connectors beschikbaar voorbeeld [Windows PowerShell Connector voorbeeld Connector verzameling][samp].

## <a name="other-notes"></a>Aanvullende opmerkingen
### <a name="additional-configuration-for-impersonation"></a>Aanvullende configuratie voor imitatie
Verleen de gebruiker die de volgende machtigingen op de server-synchronisatieservice geïmiteerd:

Leestoegang tot de volgende registersleutels:

* HKEY_USERS\\\Software\Microsoft\PowerShell [SynchronizationServiceServiceAccountSID]
* HKEY_USERS\\\Environment [SynchronizationServiceServiceAccountSID]

Om te bepalen van de beveiligings-id (SID) van het serviceaccount van de synchronisatieservice, voert u de volgende PowerShell-opdrachten:

```
$account = New-Object System.Security.Principal.NTAccount "<domain>\<username>"
$account.Translate([System.Security.Principal.SecurityIdentifier]).Value
```

Leestoegang tot de volgende mappen van het bestandssysteem:

* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\Extensions
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\ExtensionsCache
* %ProgramFiles%\Microsoft forefront Identity Manager\2010\Synchronization Service\MaData\\{ConnectorName}

Vervang de naam van de Windows PowerShell-connector voor de tijdelijke aanduiding voor de {ConnectorName}.

## <a name="troubleshooting"></a>Problemen oplossen
* Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector de [hoe ETW-tracering inschakelen voor Connectors](http://go.microsoft.com/fwlink/?LinkId=335731).

<!--Reference style links - using these makes the source content way more readable than using inline links-->
[cpp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameterpage.aspx
[keyk]: https://msdn.microsoft.com/library/ms132438.aspx
[cp]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.configparameter.aspx
[pscred]: https://msdn.microsoft.com/library/system.management.automation.pscredential.aspx
[schema]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schema.aspx
[schemaT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schematype.aspx
[schemaA]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.schemaattribute.aspx
[dnstyle]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.madistinguishednamestyle.aspx
[exportT]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maexporttype.aspx
[DataNorm]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.manormalizations.aspx
[oconf]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.maobjectconfirmation.aspx
[dw]: https://msdn.microsoft.com/library/windows/desktop/aa378184.aspx
[part]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.partition.aspx
[hn]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.hierarchynode.aspx
[oicrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionrunstep.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[oicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openimportconnectionresults.aspx
[cecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeexportconnectionrunstep.aspx
[cicres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.closeimportconnectionresults.aspx
[oecrs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.openexportconnectionrunstep.aspx
[irs]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.importrunstep.aspx
[cse]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentry.aspx
[csec]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.csentrychange.aspx
[peeres]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.putexportentriesresults.aspx
[pwdopt]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordoptions.aspx
[pwdex1]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordpolicyviolationexception.aspx
[pwdex2]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordillformedexception.aspx
[pwdex3]: https://msdn.microsoft.com/library/windows/desktop/microsoft.metadirectoryservices.passwordextensionexception.aspx
[samp]: http://go.microsoft.com/fwlink/?LinkId=394291

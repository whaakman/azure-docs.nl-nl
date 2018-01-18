---
title: Algemene LDAP-Connector | Microsoft Docs
description: In dit artikel wordt beschreven hoe algemene LDAP-Connector van Microsoft te configureren.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 984beeb0-4d91-4908-ad81-c19797c4891b
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: billmath
ms.openlocfilehash: 6e2b7d23162673f0c66b1fd6c654336da42b8f6e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="generic-ldap-connector-technical-reference"></a>Algemene LDAP-Connector, technische naslaginformatie
Dit artikel worden de algemene LDAP-Connector. Het artikel is van toepassing op de volgende producten:

* Microsoft Identity Manager 2016 (MIM2016)
* Forefront Identity Manager 2010 R2 (FIM2010R2)
  * Hotfix 4.1.3671.0 moet gebruiken of later [KB3092178](https://support.microsoft.com/kb/3092178).

Voor MIM2016 en FIM2010R2, de Connector is gedownload van de [Microsoft Download Center](http://go.microsoft.com/fwlink/?LinkId=717495).

Met betrekking tot de IETF RFC's, is dit document met de volgende indeling (RFC [RFC aantal] / [section in RFC document]), bijvoorbeeld (RFC 4512/4.3).
U vindt meer informatie op http://tools.ietf.org/html/rfc4500 (u moet 4500 vervangen door de juiste RFC-nummer).

## <a name="overview-of-the-generic-ldap-connector"></a>Overzicht van de algemene LDAP-Connector
De algemene LDAP-Connector kunt u de synchronization service integreren met een LDAP v3-server.

Bepaalde bewerkingen en schema-elementen, zoals die nodig zijn om uit te voeren van delta-import zijn niet opgegeven in de IETF RFC's. Voor deze bewerkingen worden alleen LDAP-adreslijsten expliciet worden opgegeven ondersteund.

Vanuit een perspectief op hoog niveau, worden de volgende functies worden ondersteund door de huidige release van de connector:

| Functie | Ondersteuning |
| --- | --- |
| Gekoppelde gegevensbron |De Connector wordt aan alle LDAP v3-servers (compatibel met RFC 4510) ondersteund. Het is getest met de volgende opties: <li>Microsoft Active Directory Lightweight Directory Services (AD LDS)</li><li>Microsoft Active Directory Global Catalog (AD GC)</li><li>389 Directory Server</li><li>Apache Directory Server</li><li>IBM Tivoli DS</li><li>Isode Directory</li><li>NetIQ eDirectory</li><li>Novell eDirectory</li><li>Open DJ</li><li>Open DS</li><li>Open LDAP (openldap.org)</li><li>Oracle (eerder Sun) Directory Server Enterprise Edition</li><li>Server van de virtuele map RadiantOne (VDS)</li><li>Een Sun directoryserver</li>**Opmerkelijke mappen wordt niet ondersteund:** <li>Microsoft Active Directory Domain Services (AD DS) [gebruik in plaats daarvan de ingebouwde Active Directory-Connector]</li><li>Oracle Internet Directory (OID)</li> |
| Scenario's |<li>Object levenscyclusbeheer</li><li>Beheer van groepen</li><li>Wachtwoordbeheer</li> |
| Bewerkingen |De volgende bewerkingen worden ondersteund op alle LDAP-mappen: <li>Volledige Import</li><li>Exporteren</li>De volgende bewerkingen worden alleen ondersteund op de opgegeven mappen:<li>Delta-import</li><li>Wachtwoord instellen, wachtwoord wijzigen</li> |
| Schema |<li>Schema is gedetecteerd door de LDAP-schema (RFC3673 en RFC4512/4.2)</li><li>Ondersteunt structurele klassen, aux klassen en klasse extensibleObject-object (RFC4512/4.3)</li> |

### <a name="delta-import-and-password-management-support"></a>Delta-ondersteuning voor importeren en het wachtwoord
Mappen voor de Delta-import en een wachtwoordbeheer ondersteund:

* Microsoft Active Directory Lightweight Directory Services (AD LDS)
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen van wachtwoord
* Microsoft Active Directory Global Catalog (AD GC)
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen van wachtwoord
* 389 Directory Server
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Apache Directory Server
  * Biedt geen ondersteuning voor delta-import omdat deze map beschikt niet over een permanente wijzigingenlogboek
  * Ondersteunt het instellen van wachtwoord
* IBM Tivoli DS
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Isode Directory
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Novell eDirectory en NetIQ eDirectory
  * Ondersteunt bewerkingen voor toevoegen, bijwerken en wijzig de naam voor de delta-import
  * Biedt geen ondersteuning voor Delete-bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Open DJ
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Open DS
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Open LDAP (openldap.org)
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen van wachtwoord
  * Biedt geen ondersteuning voor wachtwoord wijzigen
* Oracle (eerder Sun) Directory Server Enterprise Edition
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Server van de virtuele map RadiantOne (VDS)
  * Versie 7.1.1 of hoger
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord
* Een Sun directoryserver
  * Biedt ondersteuning voor alle bewerkingen voor de delta-import
  * Ondersteunt het instellen en wijzigen-wachtwoord

### <a name="prerequisites"></a>Vereisten
Zorg voordat u de Connector gebruikt, hebt u de volgende op de server voor de synchronisatie:

* Microsoft.NET 4.5.2 of hoger

### <a name="detecting-the-ldap-server"></a>Detecteren van de LDAP-server
De Connector is afhankelijk van verschillende manieren voor het opsporen en identificeren van de LDAP-server. De Connector gebruikt de hoofdmap DSE, de naam van de leverancier of versie en het inspecteert het schema om unieke objecten en kenmerken bekend in bepaalde LDAP-servers te zoeken. Deze gegevens als gevonden, wordt de configuratie-opties in de Connector vooraf in te vullen.

### <a name="connected-data-source-permissions"></a>Verbonden gegevensbron machtigingen
Als u wilt importeren en exporteren van bewerkingen op de objecten in de gekoppelde adreslijst, moet de connector-account voldoende machtigingen hebben. De connector moet schrijfmachtigingen om te exporteren en leesrechten heeft voor het importeren. Machtiging configuratie wordt uitgevoerd binnen de ervaringen van de doelmap zelf.

### <a name="ports-and-protocols"></a>Poorten en protocollen
Het poortnummer dat is opgegeven in de configuratie die standaard 389 voor LDAP en 636 voor LDAPS is wordt gebruikt door de connector.

Voor LDAPS, moet u SSL 3.0 of TLS gebruiken. SSL 2.0 wordt niet ondersteund en kan niet worden geactiveerd.

### <a name="required-controls-and-features"></a>Vereiste besturingselementen en onderdelen
De volgende LDAP-besturingselementen en-functies zijn beschikbaar op de LDAP-server voor de connector goed te laten werken:  
`1.3.6.1.4.1.4203.1.5.3`True/False-filters

Het filter True/False is vaak niet gerapporteerd als ondersteund door de LDAP-adreslijsten en mogelijk weergegeven op de **globale pagina** onder **verplichte functies niet gevonden**. Wordt gebruikt voor het maken van **of** filters in de LDAP-query's, bijvoorbeeld bij het importeren van meerdere objecttypen. Als u meer dan een objecttype importeren kunt, ondersteunt deze functie met de LDAP-server.

Als u een map waarin een unieke id het anker is het volgende moet ook beschikbaar zijn (Zie voor meer informatie de [ankers configureren](#configure-anchors) sectie):  
`1.3.6.1.4.1.4203.1.5.1`Alle operationele kenmerken

Als de map meer objecten heeft dan wat in één aanroep naar de map past, klikt u vervolgens het verdient aanbeveling paginering gebruiken. Voor de paginering werken, moet u een van de volgende opties:

**Optie 1:**  
`1.2.840.113556.1.4.319`pagedResultsControl

**Optie 2:**  
`2.16.840.1.113730.3.4.9` VLVControl  
`1.2.840.113556.1.4.473` SortControl

Als beide opties zijn ingeschakeld in de configuratie van de connector, wordt pagedResultsControl gebruikt.

`1.2.840.113556.1.4.417` ShowDeletedControl

ShowDeletedControl wordt alleen gebruikt met de methode USNChanged delta-import voor verwijderde objecten zien.

De connector wordt gezocht naar de opties die aanwezig zijn op de server. Als de opties kunnen worden gedetecteerd, wordt een waarschuwing is aanwezig op de algemene pagina in de Connectoreigenschappen van de. Niet alle LDAP-servers aanwezig alle besturingselementen en-functies ze ondersteunen en zelfs als deze waarschuwing aanwezig is, de connector werkt mogelijk zonder problemen.

### <a name="delta-import"></a>Delta-import
Delta-import is alleen beschikbaar wanneer u een map ondersteuning is gedetecteerd. De volgende methoden worden momenteel gebruikt:

* LDAP-Accesslog. Zie [http://www.openldap.org/doc/admin24/overlays.html#Access logboekregistratie](http://www.openldap.org/doc/admin24/overlays.html#Access Logging)
* LDAP-Changelog. Zie [http://tools.ietf.org/html/draft-good-ldap-changelog-04](http://tools.ietf.org/html/draft-good-ldap-changelog-04)
* TimeStamp. De Connector gebruikt voor Novell/NetIQ eDirectory, datum/tijd van laatste ophalen gemaakt en objecten bijgewerkt. Novell/NetIQ eDirectory biedt geen een gelijkwaardige betekent dat voor het ophalen van verwijderde objecten. Deze optie kan ook worden gebruikt als geen andere delta-import-methode op de LDAP-server actief is. Deze optie kan niet naar objecten importeren die zijn verwijderd.
* USNChanged. Zie: [https://msdn.microsoft.com/library/ms677627.aspx](https://msdn.microsoft.com/library/ms677627.aspx)

### <a name="not-supported"></a>Niet ondersteund
De volgende LDAP-functies worden niet ondersteund:

* LDAP-verwijzingen tussen servers (RFC 4511/4.1.10)

## <a name="create-a-new-connector"></a>Een nieuwe Connector maken
Maken van een algemene LDAP-connector in **synchronisatieservice** Selecteer **beheeragent** en **maken**. Selecteer de **algemene LDAP (Microsoft)** Connector.

![CreateConnector](./media/active-directory-aadconnectsync-connector-genericldap/createconnector.png)

### <a name="connectivity"></a>Connectiviteit
U moet de Host, poort en Binding-informatie opgeven op de pagina connectiviteit. Afhankelijk van die Binding geselecteerd, extra is mogelijk informatie worden opgegeven in de volgende secties.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/connectivity.png)

* De instelling voor time-out van de verbinding wordt alleen gebruikt voor de eerste verbinding met de server bij het detecteren van het schema.
* Als de Binding is anoniem vervolgens geen van beide gebruikersnaam / wachtwoord of certificaat worden gebruikt.
* Andere bindingen informatie invoeren voor beide in gebruikersnaam / wachtwoord of Selecteer een certificaat.
* Als u van Kerberos gebruikmaakt om te verifiëren, ook geeft de Realm of het domein van de gebruiker.

De **aliassen kenmerk** in het tekstvak wordt gebruikt voor kenmerken die zijn gedefinieerd in het schema met RFC4522 syntaxis. Deze kenmerken kunnen niet worden gedetecteerd tijdens de detectie van schema en de Connector moet worden geïdentificeerd die kenmerken. Bijvoorbeeld moeten de volgende worden ingevoerd in het vak kenmerk aliassen te herkennen, het kenmerk userCertificate als een binair kenmerk:

`userCertificate;binary`

Hier volgt een voorbeeld van hoe deze configuratie als eruitzien kan:

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/connectivityattributes.png)

Selecteer de **operationele kenmerken opnemen in het schema** selectievakje kenmerken die zijn gemaakt door de server ook opnemen. Het gaat hierbij om kenmerken, zoals wanneer het object is gemaakt en de tijd van laatste update.

Selecteer **uitbreidbare kenmerken die zijn opgenomen in het schema** extensible objecten (RFC4512/4.3) worden gebruikt als deze optie inschakelt, kan elk kenmerk moet worden gebruikt voor alle object. Deze optie maakt het schema zeer grote zodat tenzij deze functie wordt gebruikt door de gekoppelde adreslijst aangeraden wordt om deze optie uitgeschakeld.

### <a name="global-parameters"></a>Globale Parameters
Op de pagina Algemene Parameters, moet u de DN-naam naar de delta-wijzigingenlogboek en extra functies voor LDAP configureren. De pagina is al ingevuld met de informatie die wordt geleverd door de LDAP-server.

![Connectiviteit](./media/active-directory-aadconnectsync-connector-genericldap/globalparameters.png)

Het bovenste gedeelte bevat informatie die door de server zelf, zoals de naam van de server. De Connector controleert ook of de verplichte besturingselementen in de hoofdmap DSE aanwezig zijn. Als deze besturingselementen worden niet vermeld, wordt een waarschuwing wordt weergegeven. Sommige LDAP-adreslijsten takenlijst niet alle functies in de hoofdmap DSE en is het mogelijk dat de Connector zonder problemen werkt zelfs als er een waarschuwing aanwezig is.

De **besturingselementen ondersteund** selectievakjes bepalen het gedrag voor bepaalde bewerkingen:

* Met de structuur verwijderen is geselecteerd, een hiërarchie wordt verwijderd met een LDAP-aanroep. De connector biedt een recursieve verwijderen met de structuur verwijderen uitgeschakeld, indien nodig.
* Met wisselbare resultaten hebt geselecteerd, biedt de Connector een wisselbare importeren met de grootte die is opgegeven voor de stappen uitvoeren.
* De VLVControl en SortControl is een alternatief voor het pagedResultsControl gegevens lezen uit de LDAP-adreslijst.
* Als alle drie de opties (pagedResultsControl, VLVControl en SortControl) uitgeschakeld zijn importeert de Connector alle objecten in één bewerking die kan mislukken als het een grote map is.
* ShowDeletedControl wordt alleen gebruikt wanneer de methode Delta-import USNChanged.

Het wijzigingenlogboek DN-naam is de naamgevingscontext gebruikt door het wijzigingenlogboek delta bijvoorbeeld **cn = changelog**. Deze waarde moet worden opgegeven om te kunnen doen delta-import.

Hier volgt een lijst met standaard wijzigingenlogboek DNs:

| Directory | Delta-wijzigingenlogboek |
| --- | --- |
| Microsoft AD LDS and AD GC |Automatisch gedetecteerd. USNChanged. |
| Apache Directory Server |Niet beschikbaar. |
| Directory 389 |Logboek wijzigen. De standaardwaarde moet worden gebruikt: **cn = changelog** |
| IBM Tivoli DS |Logboek wijzigen. De standaardwaarde moet worden gebruikt: **cn = changelog** |
| Isode Directory |Logboek wijzigen. De standaardwaarde moet worden gebruikt: **cn = changelog** |
| Novell/NetIQ eDirectory |Niet beschikbaar. TimeStamp. De Connector gebruikt datum/tijd om op te halen voor het laatst bijgewerkt toegevoegd en records bijgewerkt. |
| Open DJ Active Directory |Logboek wijzigen.  De standaardwaarde moet worden gebruikt: **cn = changelog** |
| Open LDAP |Toegangslogboek. De standaardwaarde moet worden gebruikt: **cn = accesslog** |
| Oracle DSEE |Logboek wijzigen. De standaardwaarde moet worden gebruikt: **cn = changelog** |
| RadiantOne VDS |Virtuele map. Afhankelijk van de map die is verbonden met VDS. |
| Een Sun directoryserver |Logboek wijzigen. De standaardwaarde moet worden gebruikt: **cn = changelog** |

Het kenmerk password is de naam van het kenmerk dat de Connector gebruiken moet voor het wachtwoord instellen in wachtwoordwijziging en bewerkingen voor het wachtwoord instellen.
Deze waarde is standaard ingesteld op **userPassword** maar kan worden gewijzigd wanneer deze nodig is voor een bepaald LDAP-systeem.

In de lijst van extra partities is het mogelijk toe te voegen aanvullende naamruimten niet automatisch gedetecteerd. Deze instelling kan bijvoorbeeld worden gebruikt als verschillende servers gezamenlijk een logische cluster, moet allemaal op hetzelfde moment worden geïmporteerd. Net zoals Active Directory meerdere domeinen in een forest hebben, maar alle domeinen één schema delen, kunnen dezelfde gesimuleerd door te voeren van de aanvullende naamruimten in dit vak. Elke naamruimte kunt importeren uit verschillende servers en verder is geconfigureerd op de pagina configureren partities en hiërarchieën. Gebruik Ctrl + Enter om een nieuwe regel.

### <a name="configure-provisioning-hierarchy"></a>Hiërarchie inrichten configureren
Deze pagina wordt gebruikt voor het onderdeel DN-naam, bijvoorbeeld OU toewijzen aan het objecttype dat moet worden ingericht, bijvoorbeeld organizationalUnit.

![Hiërarchie inrichten](./media/active-directory-aadconnectsync-connector-genericldap/provisioninghierarchy.png)

Inrichting hiërarchie configureert, kunt u de Connector voor het automatisch maken van een structuur wanneer deze nodig is. Bijvoorbeeld, als er een naamruimte-dc = contoso, dc = com- en een nieuw object cn Kees, ou = = Seattle, c = US, dc = contoso, dc = com is ingericht en vervolgens de Connector kan een object van het type land voor VS en een organizationalUnit voor Seattle maken als deze nog niet aanwezig in de map.

### <a name="configure-partitions-and-hierarchies"></a>Partities en hiërarchieën configureren
Selecteer op de pagina partities en hiërarchieën alle naamruimten met objecten die u wilt importeren en exporteren.

![Partities](./media/active-directory-aadconnectsync-connector-genericldap/partitions.png)

Voor elke naamruimte is het ook mogelijk om te configureren van instellingen voor verbindingen die de waarden die zijn opgegeven op het scherm verbinding wilt overschrijven. Als deze waarden op hun standaardwaarde leeg worden gelaten, wordt de informatie van het scherm verbinding gebruikt.

Het is ook mogelijk om te selecteren welke containers en de Connector moet uit importeren en naar exporteren organisatie-eenheden.

Bij het uitvoeren van een zoekopdracht wordt dit gedaan via alle containers in de partitie. In gevallen waarin grote aantallen containers dit gedrag tot verminderde prestaties leidt.

>[!NOTE]
Vanaf de update van maart 2017 algemene LDAP kunnen connector zoekopdrachten worden beperkt tot alleen de geselecteerde containers. Dit kan worden gedaan door het selecteren van het selectievakje 'alleen zoeken in geselecteerde containers, zoals wordt weergegeven in de onderstaande afbeelding.

![Zoeken naar alleen geselecteerde containers](./media/active-directory-aadconnectsync-connector-genericldap/partitions-only-selected-containers.png)

### <a name="configure-anchors"></a>Ankers configureren
Deze pagina heeft altijd een vooraf geconfigureerde waarde en kan niet worden gewijzigd. Als de leverancier van de server is geïdentificeerd, kan het anker worden ingevuld met een niet-wijzigbaar kenmerk, bijvoorbeeld de GUID voor een object. Als dit niet is gedetecteerd of bekend is dat een niet-wijzigbaar kenmerk niet hebt, gebruikt de connector dn (distinguished name) als het anker.

![vertrouwensankers](./media/active-directory-aadconnectsync-connector-genericldap/anchors.png)


Hier volgt een lijst met LDAP-servers en het anker wordt gebruikt:

| Directory | Ankerkenmerk |
| --- | --- |
| Microsoft AD LDS and AD GC |objectGUID |
| 389 Directory Server |DN-naam |
| Apache Directory |DN-naam |
| IBM Tivoli DS |DN-naam |
| Isode Directory |DN-naam |
| Novell/NetIQ eDirectory |GUID |
| Open DJ Active Directory |DN-naam |
| Open LDAP |DN-naam |
| Oracle ODSEE |DN-naam |
| RadiantOne VDS |DN-naam |
| Een Sun directoryserver |DN-naam |

## <a name="other-notes"></a>Aanvullende opmerkingen
Deze sectie bevat informatie van aspecten die specifiek zijn voor deze Connector of om andere redenen zijn belangrijk te weten.

### <a name="delta-import"></a>Delta-import
Het watermerk delta in Open LDAP is UTC-datum/tijd. Daarom moeten de klokken tussen FIM-synchronisatieservice en de Open LDAP worden gesynchroniseerd. Als dat niet het geval is, is enkele vermeldingen in het logboekbestand delta mogelijk worden weggelaten.

Voor Novell eDirectory detecteert de delta-import geen een verwijderd object. Daarom is het nodig zijn voor het uitvoeren van een volledige import regelmatig om te zoeken naar alle verwijderde objecten.

Voor de mappen met een delta-wijzigingenlogboek die is gebaseerd op de datum/tijd en is het raadzaam om uit te voeren van een volledige import op periodieke tijdstippen. Dit proces kunt de synchronisatie-engine vinden en samenstellingstraject tussen de LDAP-server en wat is momenteel in het connectorgebied overgebracht.

## <a name="troubleshooting"></a>Problemen oplossen
* Zie voor meer informatie over het inschakelen van logboekregistratie voor het oplossen van de connector de [hoe ETW-tracering inschakelen voor Connectors](http://go.microsoft.com/fwlink/?LinkId=335731).

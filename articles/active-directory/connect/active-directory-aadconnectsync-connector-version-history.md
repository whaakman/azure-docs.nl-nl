---
title: Connector versiegeschiedenis van Release | Microsoft Docs
description: In dit onderwerp geeft een lijst van alle versies van de Connectors voor Forefront Identity Manager (FIM) en Microsoft Identity Manager (MIM)
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 03/22/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 9bbf75f258f9853803ca4c00155eb186ceca54a3
ms.sourcegitcommit: a06c4177068aafc8387ddcd54e3071099faf659d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/09/2018
ms.locfileid: "37916402"
---
# <a name="connector-version-release-history"></a>Releasegeschiedenis van connectorversie
De Connectors voor Forefront Identity Manager (FIM) en Microsoft Identity Manager (MIM) worden regelmatig bijgewerkt.

> [!NOTE]
> In dit onderwerp is alleen van FIM en MIM. Deze Connectors worden niet ondersteund voor installatie op Azure AD Connect. Uitgebrachte Connectors zijn vooraf geïnstalleerd op AADConnect wanneer een upgrade naar Build opgegeven.


In dit onderwerp lijst met alle versies van de Connectors die zijn uitgebracht.

Verwante koppelingen:

* [Meest recente Connectors downloaden](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Algemene LDAP-Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericldap) referentiedocumentatie
* [Algemene SQL Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql) referentiedocumentatie
* [Web-Services Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-ma-ws) referentiedocumentatie
* [PowerShell-Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-powershell) referentiedocumentatie
* [Lotus Domino-Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-domino) referentiedocumentatie


## <a name="118300"></a>1.1.830.0

### <a name="fixed-issues"></a>Opgeloste problemen:
* Opgelost ConnectorsLog System.Diagnostics.EventLogInternal.InternalWriteEvent(Message: A device attached to the system is not functioning)
* In deze release van connectors moet u omleiding van de binding van 3.3.0.0-4.1.3.0 naar 4.1.4.0 in miiserver.exe.config bijwerken
* Algemene webservices:
    * Opgelost geldig JSON-antwoord kan niet worden opgeslagen in het configuratieprogramma
* Algemene SQL:
    * Uitvoer genereert altijd alleen update-query voor de bewerking van het verwijderen. Toegevoegd voor het genereren van een verwijderquery
    * De SQL-query Hiermee haalt u objecten voor de werking van Delta-Import, als Delta-strategie is 'Wijzigingen bijhouden' is opgelost. In deze implementatie bekende beperking: Delta-Import met de modus 'Wijzigingen bijhouden' komt niet bijhouden van wijzigingen in kenmerken met meerdere waarden
    * Toegevoegde mogelijkheid voor het genereren van een verwijderquery voor aanvraag, wanneer het is nodig om de laatste waarde van kenmerk met meerdere waarden te verwijderen en deze rij bevat geen andere gegevens, behalve de waarde die nodig zijn om te verwijderen.
    * System.ArgumentException verwerken als uitvoerparameters door SP geïmplementeerd 
    * Ongeldige query waarmee de werking van uitvoer in het veld dat is van het type varbinary(max)
    * Probleem met de variabele parameterList is tweemaal (in de functies ExportAttributes en GetQueryForMultiValue) geïnitialiseerd


## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Opgeloste problemen:

* Lotus Notes:
  * Aangepaste certifiers optie filteren
  * Het importeren van de klasse ImportOperations vaste de definitie van welke bewerkingen kunnen worden uitgevoerd in de modus 'Weergaven' en die in de modus 'Search'.
* Algemene LDAP:
  * DN-naam OpenLDAP Directory gebruikt als anker in plaats van entryUUI. Nieuwe optie voor het GLDAP-connector waarmee anker wijzigen
* Algemene SQL:
  * Vaste exporteren in het veld dat is van het type varbinary(max).
  * Bij het toevoegen van binaire gegevens uit een gegevensbron op CSEntry-object, de DataTypeConversion-functie op nul bytes is mislukt. Vaste DataTypeConversion-functie van de klasse CSEntryOperationBase.




### <a name="enhancements"></a>Verbeteringen:

* Algemene SQL:
  * De mogelijkheid om te configureren van de modus voor uitvoeren die zijn opgeslagen procedure met benoemde parameters of niet met de naam in een venster van de configuratie van de algemene SQL-beheeragent op de pagina 'Globale Parameters' wordt toegevoegd. Op de pagina 'Globale Parameters' er selectievakje met het label 'Gebruik benoemde parameters voor het uitvoeren van een opgeslagen procedure' die verantwoordelijk is voor de modus voor execute opgeslagen procedure met de naam parameters of niet.
    * Op dit moment de mogelijkheid om uit te voeren van de opgeslagen procedure met benoemde parameters werkt alleen voor IBM DB2 en MSSQL-databases. Niet voor Oracle en MySQL-databases werkt deze methode: 
      * De SQL-syntaxis van MySQL biedt geen ondersteuning voor benoemde parameters in opgeslagen procedures.
      * Het ODBC-stuurprogramma voor de Oracle-biedt geen ondersteuning voor benoemde parameters voor benoemde parameters in opgeslagen procedures)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Er is een probleem zo wordt voorkomen dat een SOAP-project wordt gemaakt wanneer er twee of meer eindpunten zijn opgelost.
* Algemene SQL:
  * In de werking van importeren is de GSQL niet converteren tijd goed bij het opslaan in een connectorgebied. De standaardnotatie voor datum en tijd voor het connectorgebied van de GSQL is gewijzigd van 'jjjj-MM-dd: ssZ' in 'jjjj-MM-dd: ssZ'.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Het hulpprogramma Wsconfig hebt niet geconverteerd correct de Json-matrix van 'voorbeeld van een aanvraag' voor de REST-service. Dit veroorzaakt problemen met serialisatie deze Json-matrix voor de REST-aanvraag.
  * Web Service-Connector Configuration Tool biedt geen ondersteuning voor het gebruik van symbolen ruimte in JSON-kenmerknamen 
    * Het patroon van een vervanging kan handmatig worden toegevoegd aan het bestand WSConfigTool.exe.config bijvoorbeeld ```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```
> [!NOTE]
> JSONSpaceNamePattern sleutel is vereist als voor het exporteren van u de volgende fout ontvangt: bericht: lege naam is niet geldig. 

* Lotus Notes:
  * Wanneer de optie **aangepaste certifiers toestaan voor een organisatie/organisatie-eenheden** is uitgeschakeld en vervolgens de connector is mislukt tijdens het exporteren (Update) na de export-stroom alle kenmerken worden geëxporteerd, op het moment van exporteren Domino maar een KeyNotFoundException wordt geretourneerd om te synchroniseren. 
    * Dit komt doordat de naamswijziging mislukt als er wordt geprobeerd DN-naam (kenmerk gebruikersnaam) wijzigen door het veranderen van een van de volgende kenmerken:  
      - LastName
      - Voornaam
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - organisatie-eenheid
      - altcommonname

  * Wanneer **aangepaste certifiers toestaan voor een organisatie/organisatie-eenheden** optie is ingeschakeld, maar vereist certifiers zijn nog steeds leeg en klik vervolgens KeyNotFoundException optreedt.

### <a name="enhancements"></a>Verbeteringen:

* Algemene SQL:
  * **Scenario: opnieuw ontworpen geïmplementeerd:** ' * ' functie
  * **Beschrijving van de oplossing:** gewijzigd benadering voor [meerdere waarden reference attributes verwerking](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql).


### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Configuratie van de Server kan niet importeren als WebService Connector aanwezig is
  * WebService-Connector werkt niet met meerdere Web-Services

* Algemene SQL:
  * Er is geen objecttypen die worden vermeld voor één waarde waarnaar wordt verwezen, kenmerk
  * Delta-import voor wijzigingen bijhouden strategie verwijderd object wanneer de waarde wordt verwijderd uit de tabel met meerdere waarden
  * OverflowException in GSQL-connector met DB2 op AS / 400

Lotus:
  * Extra optie voor het enable\disable te zoeken naar organisatie-eenheden voor het openen van GlobalParameters pagina

## <a name="114430"></a>1.1.443.0

Uitgebracht: 2017 maart

### <a name="enhancements"></a>Verbeteringen

* Algemene SQL:</br>
  **Scenario symptomen:** is een bekende beperking met de SQL-Connector waar we alleen toestaan dat een verwijzing naar een objecttype en vereisen kruisverwijzingen met leden. </br>
  **Beschrijving van de oplossing:** zijn In de verwerkingsstap voor verwijzingen naar "*" optie kiest, worden alle combinaties van objecttypen die worden geretourneerd naar de synchronisatie-engine.

>[!Important]
- Hiermee maakt u veel tijdelijke aanduidingen
- Het is vereist om ervoor te zorgen dat de naamgeving cross-objecttypen die uniek is.


* Algemene LDAP:</br>
 **Scenario:** als slechts enkele containers in een specifieke partitie hebt geselecteerd, klikt u vervolgens de zoekopdracht nog steeds wordt uitgevoerd in hele partitie. Specifieke worden gefilterd door Synchronization Service, maar niet door MA wat kan leiden tot verminderde prestaties. </br>

 **Beschrijving van de oplossing:** GLDAP gewijzigd van de connector-code maken het mogelijk Doorloop alle containers en objecten zoeken in elk van deze in plaats van zoeken in de hele partitie.


* Lotus Domino:

  **Scenario:** Domino mail verwijdering ondersteuning voor een persoon verwijderen tijdens het exporteren van een. </br>
  **Oplossing:** ondersteuning voor het verwijderen voor een persoon verwijderen tijdens het exporteren van een configureerbaar e.

### <a name="fixed-issues"></a>Opgeloste problemen:
* Algemene webservices:
 * Bij het wijzigen van de service-URL in SAP wsconfig via de WebService-configuratieprogramma projecten en vervolgens de volgende fout treedt op: kan een deel van het pad niet vinden

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Algemene LDAP:
 * GLDAP Connector niet alle kenmerken in AD LDS te zien
 * Wizard einden wanneer er geen UPN-kenmerken worden gedetecteerd in de LDAP-directory-schema
 * Delta-invoer is mislukt met fouten van detectie niet aanwezig zijn tijdens de volledige import, als 'objectklasse'-kenmerk is niet ingeschakeld
 * Een pagina van de configuratie 'Partities en hiërarchieën configureren' wordt niet weergegeven van alle objecten welk type is gelijk aan de partitie voor nieuwe servers in de algemene  
LDAP-MA. Deze hebt u geleerd alleen objecten van de RootDSE-partitie.


* Algemene SQL:
 * Opgelost voor algemene SQL watermerk Delta-Import kenmerk met meerdere waarden niet worden geïmporteerd
 * Bij het exporteren van deleted\added waarden van het kenmerk met meerdere waarden, maar ze zijn niet deleted\added in gegevensbron.  


* Lotus Notes:
 * Een bepaald veld 'Volledige naam' wordt weergegeven in de metaverse correct maar bij het exporteren naar opmerkingen bij de waarde voor het kenmerk Null of leeg is.
 * Voor dubbele Certifier fout oplossen
 * Als het Object zonder gegevens is ingeschakeld op de Lotus Domino-Connector met andere objecten vervolgens ontvangen we de detectie-fout tijdens het uitvoeren van volledige Import.
 * Wanneer de Delta-Import wordt uitgevoerd op de Lotus Domino-Connector aan het einde van die wordt uitgevoerd, de Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service soms retourneert een fout.
 * Groep lidmaatschap algehele werkt prima en blijft behouden, behalve bij het uitvoeren van de export proberen te verwijderen van een gebruiker uit het lidmaatschap wordt deze als een update geslaagd weergegeven, maar de gebruiker niet daadwerkelijk verwijderd uit het lidmaatschap in Lotus Notes.
 * Een mogelijkheid om te kiezen modus van uitvoer als 'Item toevoegen onder' is toegevoegd in configuratie van de GUI van Lotus MA toe te voegen nieuwe items onderaan tijdens het exporteren van meerwaardige kenmerken.
 * Connector wordt de benodigde logica voor het verwijderen van het bestand in de Mail-map en de kluis-ID toevoegen.
 * Lidmaatschap werkt niet voor cross-NAB lid verwijderen.
 * Waarden moeten worden verwijderd uit een kenmerk met meerdere waarden

## <a name="111170"></a>1.1.117.0
Uitgebracht: 2016 maart

**Nieuwe Connector**  
Voor de eerste release van de [algemene SQL Connector](https://docs.microsoft.com/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-genericsql).

**Nieuwe functies:**

* Algemene LDAP-Connector:
  * Ondersteuning toegevoegd voor de delta-import met Isode.
* Web Services-Connector:
  * De csEntryChangeResult en setImportErrorCode activiteit waarmee object fouten moeten worden geretourneerd naar de synchronisatie-engine bijgewerkt.
  * De SAP6 en SAP6User sjablonen voor het gebruik van de nieuwe functionaliteit van de object-niveau wordt bijgewerkt.
* Lotus Domino-Connector:
  * Voor het exporteren moet u één certifier per adresboek. U kunt nu hetzelfde wachtwoord voor alle certifiers gebruiken om het beheer te vereenvoudigen.

**Opgeloste problemen:**

* Algemene LDAP-Connector:
  * Voor IBM Tivoli DS, zijn sommige verwijzingskenmerken niet correct gedetecteerd.
  * Voor Open LDAP tijdens een delta-import zijn spaties aan het begin en einde van tekenreeksen afgekapt.
  * Een uitvoer die verplaatst van een object tussen OE's / containers en op hetzelfde moment gewijzigd voor Novell en NetIQ, het object is mislukt.
* Web Services-Connector:
  * Als de webservice meerdere eindpunten voor dezelfde binding heeft, is vervolgens de Connector niet correct detecteert deze eindpunten.
* Lotus Domino-Connector:
  * Een export van het kenmerk fullName met een e-mail-in-database werkt niet.
  * Een uitvoer die zowel toegevoegd als lid verwijderd uit een groep wordt alleen de toegevoegde leden geëxporteerd.
  * Als een Document opmerkingen bij de is ongeldig (de kenmerk-isValid ingesteld op false), wordt de Connector werkt niet.

## <a name="older-releases"></a>Oudere versies
De Connectors zijn vóór maart 2016 uitgebracht als voor ondersteuningsonderwerpen.

**Algemene LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, September 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) -1.0.0549, maart 2015
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, januari 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) -1.0.0419, September 2014
* [KB2936070](https://support.microsoft.com/kb/2936070) -4.3.1082, maart 2014

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) -1.0.0419, September 2014

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) -1.0.0419, September 2014

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, September 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) -1.0.0549, maart 2015
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, augustus 2014
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, februari 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) -5.3.0721, oktober 2013
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, augustus 2013

## <a name="troubleshooting"></a>Problemen oplossen 

> [!NOTE]
> Tijdens het bijwerken van Microsoft Identity Manager of AADConnect met gebruik van een van de ECMA2-connectors. 

U moet de definitie van de connector na een upgrade uit naar overeenkomen met vernieuwen of ontvangt u de volgende fout in het begin van het gebeurtenislogboek van toepassing voor het rapporteren van waarschuwing-ID 6947: "Assembly-versie in de configuratie van de AAD-Connector ("X.X.XXX. X') is lager dan de werkelijke versie ("X.X.XXX. X)' van 'C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll'.

De definitie van de vernieuwen:
* Open de eigenschappen voor de Connector-exemplaar
* Klik op de verbinding / verbinding maken met het tabblad
  * Voer het wachtwoord voor de Connector-account
* Klik op elk van de tabbladen, op zijn beurt
  * Als dit type Connector een tabblad partities heeft met een vernieuwknop, klikt u op de knop vernieuwen terwijl op dit tabblad
* Nadat alle tabbladen zijn geopend, klikt u op de knop OK om de wijzigingen hebt opgeslagen.


## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).

---
title: Connector versiegeschiedenis van Release | Microsoft Docs
description: In dit onderwerp geeft een lijst van alle versies van de Connectors voor Forefront Identity Manager (FIM) en Microsoft Identity Manager (MIM)
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: 6a0c66ab-55df-4669-a0c7-1fe1a091a7f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 09/06/2017
ms.author: billmath
ms.openlocfilehash: 5b43284a86a7e5d4cdbf50a29d73f970c9ad9d58
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="connector-version-release-history"></a>Releasegeschiedenis van connectorversie
De Connectors voor Forefront Identity Manager (FIM) en Microsoft Identity Manager (MIM) worden regelmatig bijgewerkt.

> [!NOTE]
> In dit onderwerp is alleen van FIM en MIM. Deze Connectors worden niet ondersteund voor installatie op Azure AD Connect. Uitgebrachte Connectors zijn vooraf geïnstalleerd op AADConnect wanneer een upgrade naar Build opgegeven.


In dit onderwerp lijst van alle versies van de Connectors die zijn uitgebracht.

Verwante koppelingen:

* [Download de meest recente Connectors](http://go.microsoft.com/fwlink/?LinkId=717495)
* [Algemene LDAP-Connector](active-directory-aadconnectsync-connector-genericldap.md) documentatie verwijst naar
* [Algemene SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md) documentatie verwijst naar
* [Web-Services-Connector](http://go.microsoft.com/fwlink/?LinkID=226245) documentatie verwijst naar
* [PowerShell Connector](active-directory-aadconnectsync-connector-powershell.md) documentatie verwijst naar
* [Lotus Domino-Connector](active-directory-aadconnectsync-connector-domino.md) documentatie verwijst naar

## <a name="116490-aadconnect-116490"></a>1.1.649.0 (AADConnect 1.1.649.0)

### <a name="fixed-issues"></a>Opgeloste problemen:

* Lotus Notes:
  * Aangepaste certifiers filteroptie
  * Importeren van de klasse ImportOperations vast de definitie van welke bewerkingen kunnen worden uitgevoerd in de modus 'Weergaven' en die in de zoekmodus.
* Algemene LDAP:
  * DN-naam OpenLDAP Directory gebruikt als anker in plaats van entryUUI. Nieuwe opties GLDAP-connector waarmee anker wijzigen
* Algemene SQL:
  * Vaste exporteren naar veld die varbinary (max)-type.
  * Bij het toevoegen van binaire gegevens van een gegevensbron naar CSEntry object, de DataTypeConversion-functie op nul bytes is mislukt. Vaste DataTypeConversion-functie van de klasse CSEntryOperationBase.




### <a name="enhancements"></a>Verbeteringen:

* Algemene SQL:
  * De mogelijkheid voor het configureren van de modus voor het uitvoeren van opgeslagen procedure met benoemde parameters of geen naam is toegevoegd in een venster van de configuratie van de algemene SQL-beheeragent op de pagina 'Globale Parameters'. Op de pagina 'Globale Parameters' Er wordt selectievakje met het label 'Gebruik benoemde parameters uit te voeren van een opgeslagen procedure' die verantwoordelijk is voor de modus voor execute opgeslagen procedure met benoemde parameters of niet.
    * Op dit moment de mogelijkheid om uit te voeren van opgeslagen procedure met benoemde parameters werkt alleen voor IBM DB2 en MSSQL-databases. Niet voor Oracle en MySQL-databases werkt deze methode: 
      * De SQL-syntaxis van MySQL biedt geen ondersteuning voor benoemde parameters in opgeslagen procedures.
      * Het ODBC-stuurprogramma voor de Oracle biedt geen ondersteuning voor benoemde parameters voor benoemde parameters in de opgeslagen procedures)

## <a name="116040-aadconnect-116140"></a>1.1.604.0 (AADConnect 1.1.614.0)


### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Een probleem dat verhindert dat er een SOAP-project wordt gemaakt wanneer er twee of meer eindpunten zijn opgelost.
* Algemene SQL:
  * In de werking van het importeren is de GSQL niet converteren van tijd correct wanneer opgeslagen naar het connectorgebied overgebracht. De standaardnotatie voor datum en tijd voor connectorgebied overgebracht van de GSQL is gewijzigd van 'jjjj-MM-dd: ssZ' in 'jjjj-MM-dd: ssZ'.

## <a name="115510-aadconnect-115530"></a>1.1.551.0 (AADConnect 1.1.553.0)

### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Het hulpprogramma Wsconfig is niet juist geconverteerd de Json-matrix van 'voorbeeldaanvraag' voor de methode voor de REST. Dit veroorzaakt problemen met serialisatie deze Json-matrix voor de REST-aanvraag.
  * Web Service-Connector Configuration Tool biedt geen ondersteuning voor informatie over het gebruik van symbolen ruimte in JSON-kenmerknamen 
    * Het patroon van een vervanging kan handmatig worden toegevoegd aan het bestand WSConfigTool.exe.config bijvoorbeeld```<appSettings> <add key=”JSONSpaceNamePattern” value="__" /> </appSettings>```

* Lotus Notes:
  * Wanneer de optie **aangepaste certifiers toestaan voor organisatie/organisatorische eenheden** is uitgeschakeld en vervolgens de connector is mislukt tijdens het exporteren (Update) na de export-stroom alle kenmerken worden geëxporteerd naar Domino maar op het moment van exporteren een KeyNotFoundException wordt geretourneerd om te synchroniseren. 
    * Dit gebeurt omdat de naamswijziging mislukt als er wordt geprobeerd DN-naam (kenmerk UserName) wijzigen door het wijzigen van een van de volgende kenmerken:  
      - LastName
      - FirstName
      - MiddleInitial
      - AltFullName
      - AltFullNameLanguage
      - organisatie-eenheid
      - altcommonname

  * Wanneer **aangepaste certifiers toestaan voor organisatie/organisatorische eenheden** optie is ingeschakeld, maar vereist certifiers zijn nog steeds leeg vervolgens KeyNotFoundException optreedt.

### <a name="enhancements"></a>Verbeteringen:

* Algemene SQL:
  * **Scenario: aangepast geïmplementeerd:** ' * ' functie
  * **Beschrijving van oplossing:** gewijzigd benadering voor [met meerdere waarden verwijzing kenmerken verwerking](active-directory-aadconnectsync-connector-genericsql.md).


### <a name="fixed-issues"></a>Opgeloste problemen:

* Algemene webservices:
  * Serverconfiguratie kan niet worden geïmporteerd als WebService Connector aanwezig is
  * WebService-Connector werkt niet met meerdere Web-Services

* Algemene SQL:
  * Er is geen objecttypen worden vermeld voor één waarde waarnaar wordt verwezen kenmerk
  * Delta-import voor wijzigingen bijhouden strategie verwijderingen object wanneer de waarde wordt verwijderd uit de tabel met meerdere waarden
  * OverflowException in GSQL connector met de DB2 op AS / 400

Lotus:
  * Toegevoegde optie om te zoeken naar organisatie-eenheden voor het openen van de pagina GlobalParameters enable\disable

## <a name="114430"></a>1.1.443.0

Uitgebracht: 2017 maart

### <a name="enhancements"></a>Verbeteringen

* Algemene SQL:</br>
  **Scenario symptomen:** is een bekende beperking met de SQL-Connector waar we alleen een verwijzing naar een objecttype toestaan en vereisen kruisverwijzingen met leden. </br>
  **Beschrijving van oplossing:** In de verwerkingsstap voor verwijzingen zijn ' * ' optie kiest, worden alle combinaties van objecttypen terug naar de synchronisatie-engine geretourneerd.

>[!Important]
- Hiermee maakt u veel tijdelijke aanduidingen
- Is vereist om ervoor te zorgen dat de naamgeving is kruislings objecttypen die uniek zijn.


* Algemene LDAP:</br>
 **Scenario:** wanneer slechts enkele containers in specifieke partitie zijn geselecteerd, klikt u vervolgens de zoekopdracht nog wordt uitgevoerd in hele partitie. Specifieke worden gefilterd door de synchronisatieservice, maar niet door MA wat kan leiden tot verminderde prestaties. </br>

 **Beschrijving van oplossing:** code GLDAP gewijzigd-connector maken het mogelijk Doorloop alle containers en objecten zoeken in elk van deze in plaats van zoeken in de hele partitie.


* Lotus Domino:

  **Scenario:** Domino e-mailondersteuning voor verwijderen van een persoon worden verwijderd tijdens het exporteren van een. </br>
  **Oplossing:** ondersteuning voor het verwijderen van een persoon worden verwijderd tijdens het exporteren van een configureerbare e.

### <a name="fixed-issues"></a>Opgeloste problemen:
* Algemene webservices:
 * Bij het wijzigen van de service-URL in standaard via de WebService-configuratieprogramma projecten SAP wsconfig en vervolgens de volgende fout gebeurt: kan een deel van het pad niet vinden

      ``'C:\Users\cstpopovaz\AppData\Local\Temp\2\e2c9d9b0-0d8a-4409-b059-dceeb900a2b3\b9bedcc0-88ac-454c-8c69-7d6ea1c41d17\cfg.config\cloneconfig.xml'. ``

* Algemene LDAP:
 * GLDAP Connector ondersteunt niet alle kenmerken in AD LDS-raadpleegt u
 * Wizard einden wanneer geen UPN-kenmerken worden gedetecteerd vanuit de LDAP-directory-schema
 * Delta-invoer is mislukt met detectie-fouten is niet aanwezig zijn tijdens de volledige import, als 'objectclass'-kenmerk niet is geselecteerd
 * Een pagina van de configuratie 'Partities en hiërarchieën configureren' wordt niet weergegeven van alle objecten welk type gelijk is aan de partitie voor de nieuwe servers in de algemene  
LDAP MA. Ze hebt u geleerd alleen objecten van de RootDSE-partitie.


* Algemene SQL:
 * Herstel voor algemene SQL watermerk bug van Delta-Import-kenmerk met meerdere waarden niet worden geïmporteerd
 * Bij het exporteren van deleted\added waarden van het kenmerk met meerdere waarden, maar ze zijn geen deleted\added in gegevensbron.  


* Lotus Notes:
 * Een bepaald veld 'Volledige naam' wordt weergegeven in de metaverse correct echter bij het exporteren naar de opmerkingen bij de waarde voor het kenmerk Null of leeg is.
 * Voor dubbele Certifier fout oplossen
 * Als het Object zonder gegevens is ingeschakeld op de Lotus Domino-Connector met andere objecten ontvangt we de Discovery-fout tijdens het uitvoeren van volledige Import.
 * Wanneer de Delta-Import wordt uitgevoerd op de Lotus Domino-Connector aan het einde van die wordt uitgevoerd, de Microsoft.IdentityManagement.MA.LotusDomino.Service.exe service soms retourneert een toepassingsfout.
 * Groep lidmaatschap algehele werkt goed samen en wordt onderhouden, behalve wanneer de export proberen te verwijderen van een gebruiker van het lidmaatschap wordt uitgevoerd als succesvol kunnen werken met een update te zien, maar de gebruiker niet daadwerkelijk ophalen verwijderd uit lidmaatschap van Lotus Notes.
 * Een kans om de modus van uitvoer kiezen, zoals 'Append Item onderin' is toegevoegd in de configuratie GUI van Lotus MA nieuwe items onderin tijdens het exporteren voor kenmerken met meerdere waarden toevoegen.
 * Connector voegt de benodigde logica voor het bestand verwijderen uit de e-mailmap en ID-kluis.
 * Verwijderen van lidmaatschap voor cross-NAB lid niet werkt.
 * Waarden moeten worden verwijderd uit een kenmerk met meerdere waarden

## <a name="111170"></a>1.1.117.0
Uitgebracht: 2016 maart

**Nieuwe verbindingslijn**  
Initiële release van de [algemene SQL-Connector](active-directory-aadconnectsync-connector-genericsql.md).

**Nieuwe functies:**

* Algemene LDAP-Connector:
  * Ondersteuning toegevoegd voor de delta-import met Isode.
* Web Services Connector:
  * Bijgewerkt in de csEntryChangeResult en setImportErrorCode activiteit om toe te staan fouten object terug naar de synchronisatie-engine wordt geretourneerd.
  * De SAP6 en SAP6User sjablonen voor het gebruik van de nieuwe functionaliteit van de object-fout op niveau wordt bijgewerkt.
* Lotus Domino-Connector:
  * Voor het exporteren moet u één certifier per adresboek. U kunt nu voor alle certifiers hetzelfde wachtwoord gebruiken om het beheer te vereenvoudigen.

**Opgeloste problemen:**

* Algemene LDAP-Connector:
  * Voor IBM Tivoli DS, sommige reference-kenmerken zijn niet correct is gedetecteerd.
  * Voor Open LDAP tijdens een delta-import zijn spaties aan het begin en einde van tekenreeksen afgekapt.
  * Voor Novell en NetIQ hernoemd exporteren van een die een object tussen de organisatie-eenheden/containers en tegelijkertijd verplaatst het object is mislukt.
* Web Services Connector:
  * Als de web-service meerdere eindpunten voor dezelfde binding heeft, is klikt u vervolgens de Connector niet correct detecteren deze eindpunten.
* Lotus Domino-Connector:
  * Exporteren van een van de volledige naam-kenmerk met een database mail in werkt niet.
  * Exporteren van een die zowel toegevoegd en verwijderd lid uit een groep alleen de toegevoegde leden geëxporteerd.
  * Als een Notes-Document is ongeldig (het kenmerk isValid ingesteld op false), is mislukt voor de Connector.

## <a name="older-releases"></a>Oudere versies
De Connectors zijn vóór maart 2016 uitgebracht als ondersteuning onderwerpen.

**Generic LDAP**

* [KB3078617](https://support.microsoft.com/kb/3078617) -1.0.0597, September 2015
* [KB3044896](https://support.microsoft.com/kb/3044896) - 1.0.0549, 2015 March
* [KB3031009](https://support.microsoft.com/kb/3031009) -1.0.0534, januari 2015
* [KB3008177](https://support.microsoft.com/kb/3008177) - 1.0.0419, 2014 September
* [KB2936070](https://support.microsoft.com/kb/2936070) - 4.3.1082, 2014 March

**WebServices**

* [KB3008178](https://support.microsoft.com/kb/3008178) - 1.0.0419, 2014 September

**PowerShell**

* [KB3008179](https://support.microsoft.com/kb/3008179) - 1.0.0419, 2014 September

**Lotus Domino**

* [KB3096533](https://support.microsoft.com/kb/3096533) -1.0.0597, September 2015
* [KB3044895](https://support.microsoft.com/kb/3044895) - 1.0.0549, 2015 March
* [KB2977286](https://support.microsoft.com/kb/2977286) -5.3.0712, 2014-augustus
* [KB2932635](https://support.microsoft.com/kb/2932635) -5.3.1003, februari 2014  
* [KB2899874](https://support.microsoft.com/kb/2899874) - 5.3.0721, 2013 October
* [KB2875551](https://support.microsoft.com/kb/2875551) -5.3.0534, 2013-augustus

## <a name="troubleshooting"></a>Problemen oplossen 

> [!NOTE]
> Bij het bijwerken van Microsoft Identity Manager of AADConnect met gebruik van een van de ECMA2-connectors. 

U kunt de upgrade naar overeenkomen met de definitie van de connector moet vernieuwen of ontvangt u de volgende fout in het begin van het gebeurtenislogboek toepassing voor het rapporteren van waarschuwing 6947-ID: 'Assembly-versie in de configuratie van de AAD-Connector ('X.X.XXX. X') is ouder dan de werkelijke versie ('X.X.XXX. X)' van 'C:\Program Files\Microsoft Azure AD Sync\Extensions\Microsoft.IAM.Connector.GenericLdap.dll'.

De definitie vernieuwen:
* Open de eigenschappen voor het exemplaar van de Connector
* Klik op de verbinding / verbinding maken met het tabblad
  * Voer het wachtwoord voor de Connector-account
* Klik op elk van de tabbladen, op zijn beurt
  * Als dit type verbindingslijn een tabblad partities heeft met een knop vernieuwen, klikt u op de knop vernieuwen terwijl u op dat tabblad
* Nadat alle eigenschappentabbladen zijn geopend, klikt u op de knop OK om de wijzigingen opslaan.


## <a name="next-steps"></a>Volgende stappen
Meer informatie over de [Azure AD Connect-synchronisatie](active-directory-aadconnectsync-whatis.md) configuratie.

Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).

---
title: 'Azure AD Connect: Versiegeschiedenis van release | Microsoft Docs'
description: In dit artikel geeft een lijst van alle versies van Azure AD Connect en Azure AD Sync
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: reference
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/18/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 840ea818c7c2e197f1ab65f4bd61067bf5e51283
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57836977"
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Releasegeschiedenis van versie
Azure AD Connect het team van Azure Active Directory (Azure AD) regelmatig bijgewerkt met nieuwe functies en functionaliteit. Niet alle toevoegingen gelden voor alle doelgroepen.


In dit artikel is ontworpen om u te helpen u de versies die zijn uitgebracht, en om te begrijpen wat de wijzigingen zijn in de meest recente versie.

Deze tabel is een lijst met verwante onderwerpen:

Onderwerp |  Details
--------- | --------- |
Stappen voor het upgraden van Azure AD Connect | Verschillende methoden voor [een upgrade uitvoeren van een eerdere versie naar de meest recente](how-to-upgrade-previous-version.md) Azure AD Connect-versie.
Vereiste machtigingen | Zie voor de vereiste machtigingen voor een update toepast, [accounts en machtigingen](reference-connect-accounts-permissions.md#upgrade).

Downloaden | [Azure AD Connect downloaden](https://go.microsoft.com/fwlink/?LinkId=615771).

>[!NOTE]
>Niet alle versies van Azure AD Connect wordt beschikbaar gesteld voor automatische upgrade. De status van de release wordt aangegeven of een release voor automatische upgrade of voor download beschikbaar is gemaakt. Als u automatisch bijwerken is ingeschakeld op uw Azure AD Connect-server bijwerken die server automatisch naar de nieuwste versie van Azure AD Connect die voor automatische upgrade wordt vrijgegeven. Houd er rekening mee dat niet alle Azure AD Connect-configuraties die in aanmerking komen voor automatische upgrade zijn. Volg deze koppeling voor meer informatie over [Automatische upgrade](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-install-automatic-upgrade)

## <a name="12700"></a>1.2.70.0

### <a name="release-status"></a>Releasestatus

12/18/2018: Die zijn uitgebracht voor downloaden

### <a name="fixed-issues"></a>Problemen opgelost

Deze versie werkt het niet-standaard-connectors (bijvoorbeeld algemene LDAP-Connector en algemene SQL Connector) geleverd met Azure AD Connect. Zie voor meer informatie over die van toepassing zijn connectors versie 1.1.911.0 in [Connectorversie](/microsoft-identity-manager/reference/microsoft-identity-manager-2016-connector-version-history).


## <a name="12690"></a>1.2.69.0

### <a name="release-status"></a>Releasestatus
12/11/2018: Die zijn uitgebracht voor downloaden

### <a name="fixed-issues"></a>Problemen opgelost
Deze hotfix-versie kan de gebruiker een doeldomein in het opgegeven forest, voor de container RegisteredDevices bij het inschakelen van Write-back van apparaat selecteren.  In de vorige versies met de nieuwe functionaliteit van de opties voor apparaten (1.1.819.0 – 1.2.68.0), de locatie van de container RegisteredDevices is beperkt tot het foresthoofddomein en staat niet toe dat onderliggende domeinen.  Deze beperking alleen gemanifesteerd zelf op nieuwe implementaties – in-place upgrades zijn niet beïnvloed.  

Als een build met de bijgewerkte functionaliteit van de opties voor apparaten is geïmplementeerd naar een nieuwe server en Write-back van apparaat is ingeschakeld, moet u handmatig de locatie van de container opgeven als u niet dat deze in het foresthoofddomein wilt.  Om dit te doen, moet u Write-back van apparaat uitschakelen en weer inschakelen waarmee u de containerlocatie opgeven op de pagina 'Write-back-forest'.



## <a name="12680"></a>1.2.68.0

### <a name="release-status"></a>Releasestatus 

11/30/2018:  Die zijn uitgebracht voor downloaden

### <a name="fixed-issues"></a>Problemen opgelost

Deze hotfix-versie wordt een conflict opgelost waarbij een verificatiefout fout kan optreden als de onafhankelijke aanwezigheid van de module MSOnline PowerShell Gallery op de server voor de synchronisatie.



## <a name="12670"></a>1.2.67.0

### <a name="release-status"></a>Releasestatus 

11/19/2018:  Die zijn uitgebracht voor downloaden

### <a name="fixed-issues"></a>Problemen opgelost

Deze hotfix build corrigeert een regressie in de vorige build waar het terugschrijven van wachtwoorden is mislukt bij het gebruik van een domeincontroller wordt toegevoegd in Windows Server 2008/R2.

## <a name="12650"></a>1.2.65.0 

### <a name="release-status"></a>Releasestatus 

25-10-2018: die zijn uitgebracht voor downloaden

 
### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen 


- Gewijzigd van de functionaliteit van kenmerk terugschrijven om ervoor te zorgen gehoste voicemail werkt zoals verwacht.  In bepaalde scenario's, is Azure AD het kenmerk msExchUcVoicemailSettings overschreven tijdens write-back met een null-waarde.  Azure AD wordt nu niet meer de on-premises-waarde van dit kenmerk gewist als de waarde van de cloud is niet ingesteld.
- Diagnostische gegevens in de Azure AD Connect-wizard voor het onderzoeken en identificeren van problemen met de netwerkverbinding met Azure AD toegevoegd. Deze dezelfde diagnostische gegevens kunnen ook worden uitgevoerd via Powershell direct met de Cmdlet Test - AdSyncAzureServiceConnectivity. 
- Diagnostische gegevens in de Azure AD Connect-wizard voor het onderzoeken en identificeren van problemen met de netwerkverbinding naar AD toegevoegd. Deze dezelfde diagnostische gegevens kunnen ook worden uitgevoerd via Powershell direct met de functie Start ConnectivityValidation in de ADConnectivityTools Powershell-module.  Zie voor meer informatie [wat is de ADConnectivityTool PowerShell-Module?](how-to-connect-adconnectivitytools.md)
- Een AD-schema versiecontrole vooraf voor de toegevoegd voor hybride Azure Active Directory-Join en write-back van apparaat 
- De Directory-extensie pagina kenmerk zoekopdracht zo dat niet-hoofdlettergevoelig worden gewijzigd.
-   Volledige ondersteuning toegevoegd voor TLS 1.2. Deze versie biedt ondersteuning voor alle andere protocollen wordt uitgeschakeld en alleen TLS 1.2 wordt ingeschakeld op de machine waarop Azure AD Connect is geïnstalleerd.  Zie voor meer informatie [TLS 1.2 afdwingen voor Azure AD Connect](reference-connect-tls-enforcement.md)

 

### <a name="fixed-issues"></a>Problemen opgelost   

- Een bug opgelost waarbij Azure AD Connect upgraden, mislukken als SQL Always On wordt gebruikt. 
- Een opgelost voor het correct parseren van de namen van de organisatie-eenheid die een slash bevatten. 
- Een probleem opgelost waarbij Pass through-verificatie wordt uitgeschakeld voor een schone installatie in de faseringsmodus bevindt. 
- Een opgelost waardoor de PowerShell-module worden geladen bij het uitvoeren van de hulpprogramma's voor probleemoplossing 
- Een opgelost die klanten uit met behulp van numerieke waarden in het eerste teken van een hostnaam wilt blokkeren. 
- Een bug opgelost waarbij Azure AD Connect wordt bevraagd ongeldige partities en -container selecteren 
- Het foutbericht 'Ongeldig wachtwoord' opgelost wanneer de Desktop-SSO is ingeschakeld. 
- Verschillende oplossingen voor beheer van AD FS-vertrouwensrelatie  
- Bij het configureren van Write-back van apparaat - vaste de schemacontrole om te zoeken naar de objectklasse msDs-DeviceContainer is (die is geïntroduceerd in WS2012-R2)

 
## <a name="118820"></a>1.1.882.0  

7-9-2018: uitgebracht om te downloaden, worden niet vrijgeven voor automatische upgrade 

### <a name="fixed-issues"></a>Problemen opgelost  

Azure AD Connect upgraden mislukt als altijd op beschikbaarheid van SQL is geconfigureerd voor de ADSync-database. Deze hotfix lost dit probleem op en kunt de Upgrade te voltooien. 

## <a name="118800"></a>1.1.880.0

### <a name="release-status"></a>Releasestatus

8/21/2018: Voor het downloaden en Automatische upgrade uitgebracht. 

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

- De Ping federeren integratie in Azure AD Connect is nu beschikbaar voor algemene beschikbaarheid. [Meer informatie over het federatieve Azure AD met Ping federeren](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-user-signin#federation-with-pingfederate)
- Azure AD Connect maakt nu de back-up van Azure AD-vertrouwensrelatie in AD FS telkens wanneer een update wordt gemaakt en in een afzonderlijk bestand voor eenvoudig herstel, opgeslagen indien nodig. [Meer informatie over de nieuwe functionaliteit en de Azure AD management in Azure AD Connect vertrouwen](https://aka.ms/fedtrustinaadconnect).
- Nieuwe hulpprogramma's voor het oplossen van problemen helpt u bij het oplossen van wijzigen van primaire e-mailadres en het account van de globale adreslijst verbergen
- Azure AD Connect is bijgewerkt met de nieuwste SQL Server 2012 Native Client
- Als u aanmelden van gebruikers naar wachtwoord-Hashsynchronisatie of Pass through-verificatie in de taak 'Wijziging gebruiker aanmelden', wordt het selectievakje naadloze eenmalige aanmelding is standaard ingeschakeld.
- Er is ondersteuning toegevoegd voor Windows Server Essentials 2019
- De Azure AD Connect Health-agent is bijgewerkt naar de nieuwste versie 3.1.7.0
- Als het installatieprogramma wijzigingen in de standaardregels voor synchronisatie detecteert wordt de beheerder tijdens een upgrade gevraagd met een waarschuwing voor het overschrijven van de gewijzigde regels. Hierdoor wordt de gebruiker corrigerende maatregelen en later hervatten. Oude gedrag: Als er een gewijzigde out-of-box-regel vervolgens handmatige is upgrade is deze regels overschrijven zonder opgave van een waarschuwing voor de gebruiker en Synchronisatieplanning zonder waarin gebruiker is uitgeschakeld. Nieuw gedrag: Gebruiker wordt gevraagd met een waarschuwing voordat de gewijzigde out-of-box-synchronisatieregels wordt overschreven. Gebruikers hebben de keuze het upgradeproces stoppen en hervatten nadat corrigerende actie te ondernemen.
- Bieden een betere verwerking van een compatibiliteitsprobleem FIPS, bieden een foutbericht weergegeven voor het genereren van de MD5-hash in een omgeving van FIPS-compatibel zijn en een koppeling naar documentatie waarmee een tijdelijke oplossing voor dit probleem.
- Gebruikersinterface bijwerken ter verbetering van de federation-taken in de wizard, die nu naar een afzonderlijke subgroep voor Federatie. 
- Alle federation aanvullende taken worden nu gegroepeerd onder een enkele submenu voor gebruiksgemak.
- Een nieuwe vernieuwd ADSyncConfig Posh Module (AdSyncConfig.psm1) met nieuwe AD-machtigingen functions verplaatst van de oude ADSyncPrep.psm1 (dit kan binnenkort worden afgeschaft)

### <a name="fixed-issues"></a>Problemen opgelost 

- Een bug opgelost waarbij de AAD Connect-server toont hoog CPU-gebruik na een upgrade naar .NET 4.7.2
- Er is een fout die niet continu geeft als een foutbericht weergegeven voor een probleem SQL impasse automatisch opgelost resultaat opgelost
- Verschillende toegankelijkheidsproblemen opgelost voor de regeleditor synchronisatie en de Sync-Service Manager  
- Een bug opgelost waarbij Azure AD Connect kan geen toestemming krijgen instelling registergegevens
- Er is een fout die problemen gemaakt wanneer de gebruiker doorsturen/terug in de wizard gaat opgelost
- Een probleem opgelost om te voorkomen dat een fout opgetreden vanwege een onjuist meerdere threads verwerken in de wizard gebeurt
- Pagina synchronisatiefilters groep, wordt er een LDAP-fout optreedt bij het omzetten van beveiligingsgroepen, Azure AD Connect nu is, retourneert de uitzondering met volledige betrouwbaarheid.  De hoofdoorzaak voor de referentie-uitzondering is nog steeds onbekend en wordt verholpen door een andere fout.
-  Een bug opgelost waarbij de machtigingen voor de BEURS en NGC sleutels (ms-DS-KeyCredentialLink kenmerk op objecten voor WHfB gebruiker/apparaat) zijn niet juist ingesteld.     
- Een bug opgelost waarbij 'Set-ADSyncRestrictedPermissions' is niet correct aangeroepen
-  Ondersteuning toegevoegd voor machtigingen worden verleend voor write-back van groep in de installatiewizard van AADConnect
- Wanneer u aanmelden bij de methode van synchronisatie van Wachtwoordhashes naar AD FS, is geen synchronisatie van Wachtwoordhashes uitgeschakeld.
- Extra verificatie voor IPv6-adressen in AD FS-configuratie
- De melding om te informeren dat een bestaande configuratie is bijgewerkt.
- Write-back van apparaat mislukt voor het detecteren van de container in niet-vertrouwd forest. Dit is bijgewerkt voor een verbeterd foutbericht weergegeven en een koppeling naar de relevante documentatie
- Als u een organisatie-eenheid en synchronisatie/terugschrijven overeenkomt met dat de organisatie-eenheid een algemene synchronisatiefout biedt uitschakelt. Dit is gewijzigd voor het maken van een inzichtelijk foutbericht.

## <a name="118190"></a>1.1.819.0

### <a name="release-status"></a>Releasestatus

5/14/2018: Die zijn uitgebracht voor automatische upgrade en downloaden.

### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

Nieuwe functies en verbeteringen

- Deze versie bevat de openbare preview-versie van de integratie van PingFederate in Azure AD Connect. Met deze versie klanten kunnen eenvoudig en betrouwbaar hun Azure Active Directory-omgeving als u wilt gebruikmaken van PingFederate als de federation-provider configureren. Voor meer informatie over hoe u deze nieuwe functie wilt gebruiken, gaat u naar onze [onlinedocumentatie](plan-connect-user-signin.md#federation-with-pingfederate). 
- Bijgewerkt de Azure AD Connect Wizard hulpprogramma voor probleemoplossing, waar deze nu meer fout-scenario analyseert, zoals gekoppelde postvakken en dynamische AD-groepen. Meer informatie over het oplossen van problemen met hulpprogramma [hier](tshoot-connect-objectsync.md).
- Configuratie van de Write-back van apparaat wordt nu beheerd uitsluitend binnen de Azure AD Connect-Wizard.
- Een nieuwe PowerShell-Module met de naam ADSyncTools.psm1 wordt toegevoegd die kunnen worden gebruikt voor het oplossen van problemen met de SQL-verbinding en verschillende andere hulpprogramma's voor probleemoplossing. Meer informatie over de module ADSyncTools [hier](tshoot-connect-tshoot-sql-connectivity.md). 
- Een nieuwe aanvullende taak "Apparaatopties configureren" is toegevoegd. U kunt de taak voor het configureren van de volgende twee bewerkingen: 
  - **Hybride Azure AD join**: Als uw omgeving een on-premises AD-voetafdruk heeft en u ook wilt profiteren van de mogelijkheden die Azure Active Directory biedt, kunt u hybride Azure AD-gekoppelde apparaten implementeren. Dit zijn apparaten die zowel aan uw on-premises Active Directory als aan uw Azure Active Directory zijn gekoppeld.
  - **Apparaat terugschrijven**: Write-back van apparaat wordt gebruikt voor het inschakelen van voorwaardelijke toegang op basis van apparaten naar AD FS (2012 R2 of hoger) apparaten beveiligd

    >[!NOTE] 
    > - De optie voor het inschakelen van Write-back van apparaat uit de synchronisatieopties aanpassen wordt grijs weergegeven. 
    > -  De PowerShell-module voor ADPrep is afgeschaft in deze versie.



### <a name="fixed-issues"></a>Problemen opgelost 

- De SQL Server Express-installatie deze versie bijgewerkt naar SQL Server 2012 SP4, die, onder andere oplossingen voor enkele beveiligingsproblemen biedt.  Raadpleeg [hier](https://support.microsoft.com/help/4018073/sql-server-2012-service-pack-4-release-information) voor meer informatie over SQL Server 2012 SP4.
- Verwerken van de regel voor synchronisatie: uitgaande Join-synchronisatieregels met geen Join-voorwaarde moet ongedaan maken toegepast als de bovenliggende synchronisatieregel niet langer van toepassing is
- Verschillende toegankelijkheid correcties zijn toegepast op de Synchronization Service Manager-UI en de regeleditor synchroniseren
- Azure AD Connect-Wizard: Fout bij het maken van AD Connector-account bij Azure AD Connect in een werkgroep is
- Azure AD Connect-Wizard: Weergeven op de pagina voor aanmelding bij Azure AD het selectievakje voor verificatie wanneer er sprake is van een niet-overeenkomend in AD-domeinen en Azure AD-geverifieerde domeinen
- Automatische upgrade PowerShell oplossing om in te stellen de upgradestatus automatisch correct in bepaalde gevallen na geprobeerd automatisch een upgrade.
- Azure AD Connect-Wizard: Bijgewerkte telemetrie om vast te leggen eerder ontbrekende gegevens
- Azure AD Connect-Wizard: De volgende wijzigingen zijn aangebracht, wanneer u de **aanmelden van gebruikers wijzigen** taak overschakelen van AD FS naar Pass-through-verificatie:
    - De Pass through-verificatie-Agent is geïnstalleerd op de Azure AD Connect-server en de functie voor Pass through-verificatie is ingeschakeld, voordat we domein(en) uit die zijn gefedereerd converteren met het beheerd.
    - Gebruikers zijn niet meer geconverteerd van federatieve beheerd. Alleen domeinen worden geconverteerd.
- Azure AD Connect-Wizard: AD FS Multi-domein reguliere expressie is niet correct wanneer gebruiker UPN ' speciaal teken Regex-update voor de ondersteuning van speciale tekens
- Azure AD Connect-Wizard: Onnodig 'Configureren bronankerkenmerk' wordt weergegeven wanneer er geen wijzigingen verwijderen 
- Azure AD Connect-Wizard: AD FS-ondersteuning voor het federatiescenario dubbele
- Azure AD Connect-Wizard: AD FS-Claims worden niet bijgewerkt voor domein toegevoegd tijdens het converteren van een beheerd domein naar federatief
- Azure AD Connect-Wizard: Tijdens de detectie van geïnstalleerde pakketten, vinden we verouderde Dirsync/Azure AD Sync-/ Azure AD Connect verwante producten. Er wordt nu geprobeerd om te verwijderen van de verouderde producten.
- Azure AD Connect-Wizard: Fout bericht toewijzing te corrigeren wanneer de installatie van passthrough-verificatie-agent is mislukt
- Azure AD Connect-Wizard: Verwijderde "Configuratie" container van pagina domein OE filteren
- Synchronisatie-Engine installeren: onnodige verouderde logica die af en toe kan niet verwijderen uit msi voor synchronisatie-Engine installeren
- Azure AD Connect-Wizard: Pop-upvenster help-tekst op de pagina optionele functies voor synchronisatie van Wachtwoordhashes oplossen
- Synchronisatie-Engine runtime: Corrigeer het scenario waarbij een CS-object heeft een geïmporteerde verwijderen en synchronisatieregels voor het inrichten van het object opnieuw proberen.
- Synchronisatie-Engine runtime: Help-koppeling voor Online-connectiviteit oplossen handleiding voor het gebeurtenislogboek voor een fout bij het importeren toevoegen
- Synchronisatie-Engine runtime: Verminderd geheugengebruik van Sync Scheduler bij het opsommen van Connectors
- Azure AD Connect-Wizard: Een probleem met het oplossen van een aangepaste Sync-serviceaccount die geen AD-lezen-machtigingen heeft
- Azure AD Connect-Wizard: Logboekregistratie van het domein en OE filteren selecties verbeteren
- Azure AD Connect-Wizard: AD FS toevoegen standaard claims naar de federatieve vertrouwensrelatie gemaakt voor MFA-scenario
- Azure AD Connect-Wizard: AD FS implementeren WAP: Toe te voegen server is mislukt om nieuwe certificaat te gebruiken
- Azure AD Connect-Wizard: DSSO uitzondering wanneer onPremCredentials voor een domein zijn niet geïnitialiseerd 
- Flow voorkeursvolumes selecteren waarop de AD-distinguishedName-kenmerk van het object actieve gebruiker.
- Vaste een cosmetisch bug zijn de prioriteit van de eerste regel van de OOB-synchronisatie is ingesteld op 99 in plaats van 100



## <a name="117510"></a>1.1.751.0
Status 4/12/2018: Die zijn uitgebracht voor alleen downloaden

>[!NOTE]
>Deze release is een hotfix voor Azure AD Connect

### <a name="azure-ad-connect-sync"></a>Azure AD Connect-synchronisatie
#### <a name="fixed-issues"></a>Problemen opgelost
Een probleem opgelost zijn Azure-instantie automatische detectie voor tijd tot tijd niet in China tenants is geslaagd.  

### <a name="ad-fs-management"></a>AD FS-Management
#### <a name="fixed-issues"></a>Problemen opgelost

Er is een probleem in de configuratie van logica voor opnieuw proberen die zouden resulteren in een ArgumentException met vermelding van "een item met dezelfde sleutel is al toegevoegd."  Hierdoor zou de bewerkingen van de nieuwe poging mislukken.

## <a name="117500"></a>1.1.750.0
Status van 22-3-2018: Die zijn uitgebracht voor automatische clientupdate en downloaden.
>[!NOTE]
>Wanneer de upgrade naar deze nieuwe versie is voltooid, wordt deze automatisch een volledige synchronisatie en een volledige import voor de Azure AD-connector en een volledige synchronisatie uit voor de AD-connector geactiveerd. Omdat dit, afhankelijk van de grootte van uw Azure AD Connect-omgeving duren kan, zorg dat u de benodigde stappen voor de ondersteuning van dit of houd uit voor het upgraden van totdat u een handige moment dat dit niet doet, hebt gevonden hebt ondernomen.

>[!NOTE]
>"AutoUpgrade functionaliteit is niet correct uitgeschakeld voor enkele tenants die hoger is dan 1.1.524.0 en hoger builds geïmplementeerd. Om ervoor te zorgen dat uw Azure AD Connect-exemplaar nog steeds in aanmerking komen voor AutoUpgrade is, moet u de volgende PowerShell-cmdlet uitvoeren: 'Set-ADSyncAutoUpgrade - AutoupGradeState ingeschakeld'


### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Problemen opgelost

* De cmdlet Set-ADSyncAutoUpgrade blokkeren Autoupgrade eerder als automatische upgrade-status is ingesteld op uitgesteld. Deze functionaliteit is nu gewijzigd zodat AutoUpgrade van toekomstige builds niet worden geblokkeerd.
* Gewijzigd de **aanmelden van gebruikers** pagina de optie 'Password Synchronization' op 'Wachtwoord-Hashsynchronisatie'.  Azure AD Connect synchroniseert de wachtwoord-hashes, wachtwoorden, zodat deze correspondeert met wat daadwerkelijk gebeurt.  Zie voor meer informatie [implementeren u wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie](how-to-connect-password-hash-synchronization.md)

## <a name="117490"></a>1.1.749.0
Status: Beschikbaar voor het selecteren van klanten

>[!NOTE]
>Wanneer de upgrade naar deze nieuwe versie is voltooid, wordt deze automatisch een volledige synchronisatie en een volledige import voor de Azure AD-connector en een volledige synchronisatie uit voor de AD-connector geactiveerd. Zorg ervoor dat u de benodigde stappen voor de ondersteuning van dit of houd uit voor het upgraden van totdat u een handige moment dat dit niet doet, hebt gevonden hebt ondernomen omdat dit kan enige tijd, afhankelijk van de grootte van uw Azure AD Connect-omgeving, duren.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Problemen opgelost
* Los tijdvenster op achtergrondtaken voor pagina partities filteren wanneer u overschakelt naar de volgende pagina.

* Een opgelost waardoor toegangsfout tijdens de aangepaste actie ConfigDB.

* Een opgelost om te herstellen van de time-out van de SQL-verbinding.

* Een bug opgelost waar een controle van de certificaten met jokertekens SAN is mislukt.

* Een opgelost waardoor miiserver.exe vastloopt tijdens het exporteren van een Azure AD-connector.

* Vaste op DC een bug die onjuist wachtwoord geregistreerd bij het uitvoeren van de Azure AD Connect-wizard om configuratie te wijzigen.


#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

* Privacyinstellingen voor de General Data Protection Regulation (GDPR) toe te voegen.  Zie voor meer informatie het artikel [hier](reference-connect-user-privacy.md).

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]  

* toepassingstelemetrie - beheerder kan deze klasse van gegevens in- / uitschakelen op overstappen wordt

* Statusgegevens van Azure AD - beheerder moet Ga naar de health-portal voor het beheren van de health-instellingen.
   Zodra de service-beleid is gewijzigd, wordt de agents lezen en deze toepassen.

* Apparaat terugschrijven Configuratieacties en een voortgangsbalk voor de initialisatie van de pagina

* Algemene diagnostische gegevens verbeterd met HTML-rapport en de volledige gegevens verzamelen in een ZIP-tekst / HTML-rapport

* Verbeterd wat betreft de betrouwbaarheid van Automatische upgrade en extra extra telemetrie om te controleren of dat de status van de server kan worden bepaald.

* Machtigingen die beschikbaar zijn beperkt tot beschermde accounts in AD Connector-account

  * Voor nieuwe installaties van de wizard zorgen ervoor dat de machtigingen die accounts met bevoegdheden hebben op de MSOL-account na het maken van de MSOL-account.

De wijzigingen die zorgt voor het volgende:
1. Snelle installatie
2. Aangepaste installaties met account automatisch maken
3. Het installatieprogramma wordt gewijzigd zodat het SA-bevoegdheden op een schone installatie van Azure AD Connect niet nodig

* Een nieuw hulpprogramma voor het oplossen van synchronisatieproblemen met voor een specifiek object toegevoegd. Het is beschikbaar onder de optie 'Objectsynchronisatie oplossen' van Azure AD Connect Wizard oplossen aanvullende taak. Op dit moment controleert het hulpprogramma voor het volgende:

  * UserPrincipalName komt niet overeen tussen gesynchroniseerde gebruikersobject en het gebruikersaccount in Azure AD-Tenant.
  * Als het object is uitgesloten voor synchronisatie vanwege domein filteren
  * Als het object is uitgesloten voor synchronisatie vanwege een organisatie-eenheid (OE) filteren

* Een nieuw hulpprogramma voor het synchroniseren van de huidige wachtwoord-hash die zijn opgeslagen in de on-premises Active Directory voor een specifiek gebruikersaccount toegevoegd.

Het hulpprogramma is niet vereist voor een wachtwoordwijziging. Het is beschikbaar onder de optie 'Oplossen van wachtwoord-Hashsynchronisatie' van Azure AD Connect Wizard oplossen aanvullende taak.






## <a name="116540"></a>1.1.654.0
Status: 12 december 2017

>[!NOTE]
>Deze release is een beveiliging gerelateerde hotfix voor Azure AD Connect

### <a name="azure-ad-connect"></a>Azure AD Connect
Een verbetering is toegevoegd aan Azure AD Connect versie 1.1.654.0 (en na) om ervoor te zorgen dat de aanbevolen beschreven in sectie wijzigingen [het vergrendelen van toegang tot de AD DS-account](#lock) worden automatisch toegepast wanneer Azure AD Verbinding maken met het AD DS-account wordt gemaakt. 

- Bij het instellen van Azure AD Connect, kunt de beheerder van de installatie een bestaand AD DS-account opgeven of kunt Azure AD Connect automatisch maken van het account. De wijzigingen in de machtigingen worden automatisch toegepast op de AD DS-account dat is gemaakt door Azure AD Connect tijdens de installatie. Ze worden niet toegepast op bestaande AD DS-account opgegeven door de beheerder van de installatie.
- Voor klanten die een upgrade hebt uitgevoerd van een oudere versie van Azure AD Connect naar 1.1.654.0 (of na), de machtiging worden wijzigingen niet met terugwerkende kracht toegepast op bestaande AD DS-accounts die zijn gemaakt vóór de upgrade. Ze worden alleen toegepast op nieuwe AD DS-accounts die zijn gemaakt na de upgrade. Dit gebeurt wanneer u nieuwe AD-forests worden gesynchroniseerd met Azure AD toevoegt.

>[!NOTE]
>Deze release worden alleen de kwetsbaarheid voor nieuwe installaties van Azure AD Connect waarin de serviceaccount is gemaakt door het installatieproces verwijderd. Voor bestaande installaties, of in gevallen waarin u het account zelf opgeven, moet u ervoor zorgen dat dit beveiligingslek niet bestaat.

#### <a name="lock"></a> Vergrendelen van toegang tot de AD DS-account
Het vergrendelen van toegang tot de AD DS-account door het implementeren van de volgende wijzigingen in de machtigingen in de on-premises AD:  

*   Overname in het opgegeven object uitschakelen
*   Verwijder alle ACE's op het specifieke object, met uitzondering van ACE's die specifiek zijn voor zelf. We willen de standaardmachtigingen behouden als het gaat om zelf.
*   Deze specifieke machtigingen toewijzen:

Type     | Name                          | Access               | Van toepassing op
---------|-------------------------------|----------------------|--------------|
Toestaan    | SYSTEEM                        | Volledig beheer         | Dit object  |
Toestaan    | Ondernemingsadministrators             | Volledig beheer         | Dit object  |
Toestaan    | Domeinadministrators                 | Volledig beheer         | Dit object  |
Toestaan    | Beheerders                | Volledig beheer         | Dit object  |
Toestaan    | Enterprise-domeincontrollers | Inhoud weergeven        | Dit object  |
Toestaan    | Enterprise-domeincontrollers | Alle eigenschappen lezen  | Dit object  |
Toestaan    | Enterprise-domeincontrollers | Machtigingen lezen     | Dit object  |
Toestaan    | Geverifieerde gebruikers           | Inhoud weergeven        | Dit object  |
Toestaan    | Geverifieerde gebruikers           | Alle eigenschappen lezen  | Dit object  |
Toestaan    | Geverifieerde gebruikers           | Machtigingen lezen     | Dit object  |

Om de instellingen voor de AD DS account dat u kunt uitvoeren [dit PowerShell-script](https://gallery.technet.microsoft.com/Prepare-Active-Directory-ef20d978). Het PowerShell-script worden de machtigingen zoals hierboven wordt vermeld op het AD DS-account toewijzen.

#### <a name="powershell-script-to-tighten-a-pre-existing-service-account"></a>PowerShell-script om een bestaande service-account

Deze instellingen toepassen op een bestaande AD DS-account met het PowerShell-script (ether geleverd door uw organisatie of die zijn gemaakt door een eerdere installatie van Azure AD Connect, download het script uit de aangeboden koppeling hierboven.

##### <a name="usage"></a>Gebruik:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN <$ObjectDN> -Credential <$Credential>
```

Waar 

**$ObjectDN** = de Active Directory-account waarvan de machtigingen wilt hoger worden ingesteld.

**$Credential** = beheerdersreferentie waarvoor de vereiste bevoegdheden beschikt om de machtigingen voor het account $ObjectDN te beperken. Deze bevoegdheden worden doorgaans opgeslagen door de beheerder van de Enterprise- of -domein. Gebruik de volledig gekwalificeerde domeinnaam van het administrator-account om te voorkomen dat account opzoeken fouten. Voorbeeld: contoso.com\admin.

>[!NOTE] 
>$credential. Gebruikersnaam moet FQDN\gebruikersnaam indeling hebben. Voorbeeld: contoso.com\admin 

##### <a name="example"></a>Voorbeeld:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN "CN=TestAccount1,CN=Users,DC=bvtadwbackdc,DC=com" -Credential $credential 
```
### <a name="was-this-vulnerability-used-to-gain-unauthorized-access"></a>Is dit beveiligingsprobleem gebruikt om niet-gemachtigde toegang te krijgen?

Connect-configuratie moet u controleren of het laatste wachtwoord opnieuw instellen van datum van de serviceaccount om te zien als deze kwetsbaarheid is gebruikt voor het manipuleren van uw Azure AD.  Als de tijdstempel in onverwachte, nader onderzoek, via het gebeurtenislogboek voor dat wachtwoord opnieuw instellen van gebeurtenis, moeten worden ondernomen.

Zie voor meer informatie, [Microsoft Security Advisory 4056318](https://technet.microsoft.com/library/security/4056318)

## <a name="116490"></a>1.1.649.0
Status: Oktober 27 2017

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie voor Azure AD Connect automatisch upgraden.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issue"></a>Probleem opgelost
* Er is een compatibiliteitsprobleem versie tussen Azure AD Connect en Azure AD Connect Health-Agent (voor synchronisatie) opgelost. Dit probleem is van invloed op klanten die Azure AD Connect in-place upgrade naar versie 1.1.647.0 uitvoert, maar heeft momenteel de Health-Agent versie 3.0.127.0. Na de upgrade kan de Health-Agent niet meer statusgegevens over Azure AD Connect-synchronisatieservice verzenden naar Azure AD Health-Service. Met deze oplossing is tijdens in-place upgrade van Azure AD Connect Health-Agent versie 3.0.129.0 geïnstalleerd. Health-Agent versie 3.0.129.0 heeft geen compatibiliteitsproblemen met Azure AD Connect versie 1.1.649.0.


## <a name="116470"></a>1.1.647.0
Status: 19 oktober-2017

> [!IMPORTANT]
> Er is een probleem met de bekende compatibiliteitsproblemen tussen Azure AD Connect 1.1.647.0 en Azure AD Connect Health-Agent (voor synchronisatie) versie 3.0.127.0. Dit probleem wordt voorkomen dat de Health-Agent health gegevens over de Azure AD Connect-synchronisatieservice (inclusief synchronisatiefouten en gegevens van uitvoeringsgeschiedenis) te verzenden naar Azure AD Health-Service. Voordat u uw Azure AD Connect-implementatie handmatig upgraden naar versie 1.1.647.0, Controleer of dat de huidige versie van Azure AD Connect Health-Agent geïnstalleerd op uw Azure AD Connect-server. U kunt dit doen door te gaan naar *Configuratiescherm → Software* en zoek naar toepassing *Microsoft Azure AD Connect Health-Agent voor synchronisatie*. Als de versie 3.0.127.0, verdient het aanbeveling wachten op de volgende Azure AD Connect-versie beschikbaar is voor de upgrade. Als de versie van de Health-Agent niet 3.0.127.0, is het fijn om door te gaan met de handmatige, in-place upgrade. Houd er rekening mee dat dit probleem heeft geen invloed op swing-upgrade of klanten die een nieuwe installatie van Azure AD Connect uitvoert.
>
>

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Problemen opgelost
* Er is een probleem met opgelost de *aanmelden van gebruikers wijzigen* taak in Azure AD Connect-wizard:

  * Dit probleem doet zich voor wanneer u een bestaande Azure AD Connect-implementatie met Wachtwoordsynchronisatie **ingeschakeld**, en u probeert om in te stellen van de gebruiker aanmelden methode als *Pass through-verificatie*. Voordat de wijziging wordt toegepast, wordt de wizard onjuist geeft de "*Wachtwoordsynchronisatie uitschakelen*" prompt. Synchronisatie van wachtwoord blijft ingeschakeld nadat de wijziging wordt toegepast. Met deze oplossing wordt de wizard niet meer weergegeven de prompt.

  * Door de wizard wordt niet uitgeschakeld Wachtwoordsynchronisatie tijdens het bijwerken van het gebruik van de gebruiker aanmelden methode de *aanmelden van gebruikers wijzigen* taak. Dit is om te voorkomen dat klanten die houden Wachtwoordsynchronisatie, willen zelfs als ze bij het inschakelen van Pass-through-verificatie of Federatie als hun primaire gebruiker aanmelden methode onderbreking.
  
  * Als u uitschakelen synchronisatie van wachtwoord wilt na het bijwerken van de gebruiker aanmelden methode, moet u uitvoeren de *synchronisatieconfiguratie aanpassen* taak in de wizard. Wanneer u gaat u naar de *optionele functies* pagina, schakel het selectievakje de *Wachtwoordsynchronisatie* optie.
  
  * Houd er rekening mee dat hetzelfde probleem vindt ook plaats als u wilt in-of uitschakelen naadloze eenmalige aanmelding. Speciaal, hebt u een bestaande Azure AD Connect-implementatie met Wachtwoordsynchronisatie is ingeschakeld en de aanmeldingsmethode gebruiker is al geconfigureerd als *Pass through-verificatie*. Met behulp van de *aanmelden van gebruikers wijzigen* taak, die u wilt inschakelen/uitschakelen de *inschakelen naadloze eenmalige aanmelding* optie terwijl de gebruiker aanmelden methode geconfigureerd als 'Pass through-verificatie blijft'. Voordat de wijziging wordt toegepast, wordt de wizard onjuist geeft de "*Wachtwoordsynchronisatie uitschakelen*" prompt. Synchronisatie van wachtwoord blijft ingeschakeld nadat de wijziging wordt toegepast. Met deze oplossing wordt de wizard niet meer weergegeven de prompt.

* Er is een probleem met opgelost de *aanmelden van gebruikers wijzigen* taak in Azure AD Connect-wizard:

  * Dit probleem doet zich voor wanneer u een bestaande Azure AD Connect-implementatie met Wachtwoordsynchronisatie **uitgeschakeld**, en u probeert om in te stellen van de gebruiker aanmelden methode als *Pass through-verificatie*. Als de wijziging wordt toegepast, wordt de wizard kan zowel Wachtwoordsynchronisatie als Pass-through-verificatie. Met deze oplossing wordt kunt de wizard niet meer Wachtwoordsynchronisatie.

  * Voorheen was Wachtwoordsynchronisatie een vereiste voor het inschakelen van Pass-through-verificatie. Als u instelt dat de gebruiker aanmelden methode als *Pass through-verificatie*, de wizard zou kunnen zowel Wachtwoordsynchronisatie als Pass-through-verificatie. Wachtwoordsynchronisatie is onlangs verwijderd als een vereiste. Als onderdeel van Azure AD Connect versie 1.1.557.0, een wijziging is aangebracht in Azure AD Connect niet wachtwoordsynchronisatie inschakelen bij het instellen van de gebruiker aanmelden methode als *Pass through-verificatie*. De wijziging is echter alleen toegepast op Azure AD Connect-installatie. Met deze oplossing ook dezelfde wijziging wordt toegepast op de *aanmelden van gebruikers wijzigen* taak.
  
  * Houd er rekening mee dat hetzelfde probleem vindt ook plaats als u wilt in-of uitschakelen naadloze eenmalige aanmelding. Speciaal, hebt u een bestaande Azure AD Connect-implementatie met Wachtwoordsynchronisatie is uitgeschakeld en de aanmeldingsmethode gebruiker is al geconfigureerd als *Pass through-verificatie*. Met behulp van de *aanmelden van gebruikers wijzigen* taak, die u wilt inschakelen/uitschakelen de *inschakelen naadloze eenmalige aanmelding* optie terwijl de gebruiker aanmelden methode geconfigureerd als 'Pass through-verificatie blijft'. Wanneer de wijziging wordt toegepast, kunt met de wizard Wachtwoordsynchronisatie. Met deze oplossing wordt kunt de wizard niet meer Wachtwoordsynchronisatie. 

* Er is een probleem waardoor Azure AD Connect-upgrade mislukken met fout opgelost "*kan geen upgrade uitvoeren van de synchronisatieservice*'. Bovendien is de synchronisatieservice niet meer kunt beginnen met gebeurtenis met de fout '*de service kan niet worden gestart omdat de versie van de database nieuwer dan de versie van de binaire bestanden geïnstalleerd is is*'. Het probleem treedt op wanneer de beheerder uitvoeren van de upgrade heeft geen sysadmin-bevoegdheden aan de SQL-server die wordt gebruikt door Azure AD Connect. Met deze oplossing vereist Azure AD Connect alleen de beheerder db_owner-bevoegdheden aan de ADSync-database hebben tijdens de upgrade.

* Er is een probleem met Azure AD Connect upgraden dat betrokken klanten die hebben ingeschakeld opgelost [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). Na de upgrade van Azure AD Connect, naadloze eenmalige aanmelding niet correct wordt weergegeven als uitgeschakeld in Azure AD Connect-wizard, zelfs als de functie ingeschakeld en volledig functioneel blijft. Met deze oplossing wordt de functie nu correct ingeschakeld in de wizard wordt weergegeven.

* Er is een probleem waardoor Azure AD Connect-wizard altijd show opgelost de "*Bronanker configureren*" vragen op de *klaar om te configureren* pagina, zelfs als er geen wijzigingen met betrekking tot Bronanker doorgevoerd.

* Bij het uitvoeren van handmatige in-place upgrade van Azure AD Connect, wordt de klant is vereist voor de referenties van de globale beheerder van de bijbehorende Azure AD-tenant. Voorheen upgrade kan doorgaan, ondanks dat de hoofdbeheerdersreferenties deel uitmaakten van een andere Azure AD-tenant. Tijdens de upgrade wordt weergegeven is voltooid, worden bepaalde configuraties niet correct met de upgrade behouden. Met deze wijziging, de wizard wordt voorkomen dat de upgrade procedure als de opgegeven referenties komen niet overeen met de Azure AD-tenant.

* Redundante logica die onnodig opnieuw Azure AD Connect Health-service aan het begin van een handmatige upgrade opgestart verwijderd.


#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Toegevoegde logica voor het vereenvoudigen van de stappen die nodig zijn voor het instellen van Azure AD Connect met Microsoft-Cloud voor Duitsland. Eerder, bent u vereist voor het bijwerken van specifieke registersleutels op de Azure AD Connect-server voor het correct te laten werken met Microsoft-Cloud voor Duitsland, zoals beschreven in dit artikel. Azure AD Connect kan nu automatisch detecteren als uw tenant is in de Microsoft-Cloud voor Duitsland op basis van de hoofdbeheerdersreferenties die zijn opgegeven tijdens de installatie.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
> [!NOTE]
> Opmerking: De Synchronization Service heeft een WMI-interface waarmee u uw eigen aangepaste scheduler ontwikkelen. Deze interface is afgeschaft en wordt verwijderd uit toekomstige versies van Azure AD Connect na 30 juni 2018 is verzonden. Klanten die willen aanpassen Synchronisatieplanning moeten gebruiken de [ingebouwde scheduler (https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-scheduler).

#### <a name="fixed-issues"></a>Problemen opgelost
* Als u Azure AD Connect-wizard het AD-Connector-account vereist voor het synchroniseren van wijzigingen van on-premises Active Directory maakt, deze geen correct toegewezen het account de machtiging is vereist voor het lezen van PublicFolder objecten. Dit probleem is van invloed op zowel de Express-installatie en de aangepaste installatie. Deze wijziging kunt u het probleem.

* Een probleem dat heeft het oplossen van problemen met Azure AD Connect-Wizard-pagina niet weergeven correct voor beheerders voeren vanaf Windows Server 2016 opgelost.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Wanneer u problemen met Wachtwoordsynchronisatie met de Azure AD Connect-wizard pagina voor probleemoplossing, retourneert de pagina voor probleemoplossing nu domeinspecifieke status.

* Eerder, als u probeert om in te schakelen van wachtwoord-Hashsynchronisatie, Azure AD Connect controleert niet of het AD-Connector-account vereist machtigingen voor het synchroniseren van wachtwoord-hashes van on-premises AD. Azure AD Connect-wizard wordt nu controleren en waarschuwen als de AD-Connector-account niet beschikt over voldoende machtigingen.

### <a name="ad-fs-management"></a>AD FS-Management
#### <a name="fixed-issue"></a>Probleem opgelost
* Er is een probleem met betrekking tot het gebruik van opgelost [ms-DS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) functie. Dit probleem heeft betrekking op klanten die hebben geconfigureerd *Federatie met AD FS* als de gebruiker aanmelden-methode. Wanneer u uitvoert *Bronanker configureren* verandert in met behulp van een taak in de wizard, Azure AD Connect * ms-DS-ConsistencyGuid als bronkenmerk voor immutableId. Als onderdeel van deze wijziging probeert Azure AD Connect bij te werken van de claimregels voor ImmutableId in AD FS. Deze stap is echter mislukt omdat Azure AD Connect heeft niet de beheerdersreferenties vereist voor het configureren van AD FS. Met deze oplossing wordt Azure AD Connect nu vraagt u de beheerdersreferenties opgeven voor AD FS, wanneer u uitvoert het *Bronanker configureren* taak.



## <a name="116140"></a>1.1.614.0
Status: September-05-2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="known-issues"></a>Bekende problemen
* Er is een bekend probleem wordt veroorzaakt door Azure AD Connect-upgrade mislukken met fout "*kan geen upgrade uitvoeren van de synchronisatieservice*'. Bovendien is de synchronisatieservice niet meer kunt beginnen met gebeurtenis met de fout '*de service kan niet worden gestart omdat de versie van de database nieuwer dan de versie van de binaire bestanden geïnstalleerd is is*'. Het probleem treedt op wanneer de beheerder uitvoeren van de upgrade heeft geen sysadmin-bevoegdheden aan de SQL-server die wordt gebruikt door Azure AD Connect. Dbo-machtigingen zijn niet voldoende.

* Er is een bekend probleem met Azure AD Connect upgraden die invloed heeft op klanten die hebben ingeschakeld [naadloze eenmalige aanmelding](how-to-connect-sso.md). Na de upgrade van Azure AD Connect, de functie wordt weergegeven als uitgeschakeld in de wizard, zelfs als de functie ingeschakeld blijft. Een oplossing voor dit probleem wordt gegeven in de toekomst brengt. Klanten die zich zorgen maakt over dit probleem weergeven kunnen dit handmatig oplossen door in te schakelen naadloze eenmalige aanmelding in de wizard.

#### <a name="fixed-issues"></a>Problemen opgelost
* Er is een probleem waardoor Azure AD Connect uit het bijwerken van de claims in de on-premises opgelost AD FS tijdens het inschakelen van de [ms-DS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) functie. Het probleem treedt op als u de functie voor een bestaande Azure AD Connect-implementatie met AD FS geconfigureerd als de aanmeldingsmethode in te schakelen. Het probleem treedt op omdat de wizard wordt niet gevraagd om referenties van de AD FS voor het bijwerken van de claimregels in AD FS.
* Er is een probleem waardoor Azure AD Connect installatie mislukt als opgelost de on-premises AD-forest heeft NTLM is uitgeschakeld. Het probleem wordt veroorzaakt door van Azure AD Connect-wizard volledig gekwalificeerde referenties niet op te geven bij het maken van de beveiligingscontext die is vereist voor Kerberos-verificatie. Dit zorgt ervoor dat Azure AD Connect-wizard terugvallen op met behulp van NTLM en Kerberos-verificatie mislukt.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
#### <a name="fixed-issues"></a>Problemen opgelost
* Een probleem opgelost waarbij nieuwe regel voor synchronisatie kan niet worden gemaakt als het kenmerk code niet is ingevuld.
* Vaste een probleem waardoor Azure AD Connect verbinding maken met on-premises AD voor Wachtwoordsynchronisatie met NTLM, hoewel Kerberos beschikbaar is. Dit probleem treedt op als de on-premises AD-topologie heeft een of meer domeincontrollers die zijn teruggezet vanuit een back-up.
* Er is een probleem waardoor een volledige synchronisatie-stappen voor het onnodig optreden na de upgrade opgelost. In het algemeen is stappen van de volledige synchronisatie uitgevoerd vereist na de upgrade als er wijzigingen in synchronisatieregels out-of-box. Het probleem is vanwege een fout in de logica voor het detecteren van wijziging die ten onrechte een wijziging gedetecteerd tijdens synchronisatie Regelexpressie met regeltekens worden aangetroffen. Regeltekens worden ingevoegd in de Regelexpressie synchroniseren om leesbaarheid te verbeteren.
* Er is een probleem dat kan leiden tot de Azure AD Connect-server niet goed werkt nadat de automatische Upgrade opgelost. Dit probleem is van invloed op de Azure AD Connect-servers met versie 1.1.443.0 (of lager). Raadpleeg het artikel voor meer informatie over het probleem [Azure AD Connect is niet correct werken na een automatische upgrade](https://support.microsoft.com/help/4038479/azure-ad-connect-is-not-working-correctly-after-an-automatic-upgrade).
* Er is een probleem waardoor automatisch een upgrade uitvoert naar opnieuw geprobeerd om de 5 minuten wanneer er fouten zijn aangetroffen opgelost. Met de oplossing, automatische Upgrade opnieuw proberen met exponentieel uitstel wanneer er fouten optreden.
* Er is een probleem opgelost waar wachtwoord synchronisatiegebeurtenis 611 ten onrechte wordt weergegeven in de Windows-toepassingsgebeurtenislogboeken als **informatief** in plaats van **fout**. Gebeurtenis 611 wordt gegenereerd wanneer er een probleem wordt aangetroffen Wachtwoordsynchronisatie. 
* Een probleem opgelost in de Azure AD Connect-wizard waarmee groep Write-back-functie wordt ingeschakeld zonder te selecteren van een organisatie-eenheid voor write-back van groep vereist.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Een taak oplossen toegevoegd aan Azure AD Connect-wizard onder aanvullende taken. Klanten kunnen gebruikmaken van deze taak voor het oplossen van problemen met betrekking tot Wachtwoordsynchronisatie en algemene diagnostische gegevens verzamelen. De taak oplossen wordt in de toekomst worden uitgebreid om op te nemen van andere directory-synchronisatie-problemen.
* Azure AD Connect ondersteunt voor de nu een nieuwe installatiemodus met de naam **bestaande Database gebruiken**. Deze installatiemodus kan klanten voor het installeren van Azure AD Connect die Hiermee geeft u een bestaande ADSync-database. Raadpleeg voor meer informatie over deze functie, artikel [gebruik een bestaande database](how-to-connect-install-existing-database.md).
* Voor betere beveiliging, is Azure AD Connect nu standaard ingesteld op via TLS1.2 verbinding maken met Azure AD voor directory-synchronisatie. Voorheen was de standaard TLS1.0.
* Wanneer Azure AD Connect wachtwoord synchronisatie-Agent wordt gestart, wordt geprobeerd verbinding maken met Azure AD-bekende eindpunt voor Wachtwoordsynchronisatie. Na de verbinding is geslaagd, wordt die omgeleid naar een regiospecifiek-eindpunt. De synchronisatieagent wachtwoord caches eerder, het eindpunt regiospecifieke totdat deze opnieuw is opgestart. Nu de agent Hiermee schakelt u de cache en nieuwe pogingen met het eindpunt van de bekende als er problemen met het eindpunt regiospecifiek. Deze wijziging zorgt ervoor dat Wachtwoordsynchronisatie failover naar een ander regiospecifiek-eindpunt kunt wanneer het in de cache regiospecifiek-eindpunt is niet meer beschikbaar.
* Voor het synchroniseren van wijzigingen vanuit een on-premises AD-forest is een AD DS-account vereist. U kunt ofwel (i) maken het AD DS-account zelf en geef de referentie naar Azure AD Connect, of (ii) een ondernemingsadministratorreferenties opgeven en kunt u Azure AD Connect het AD DS-account voor u maken. Voorheen is (i) de standaardoptie in de Azure AD Connect-wizard. Nu, (ii) is de standaardoptie.

### <a name="azure-ad-connect-health"></a>Azure AD Connect Health (Engelstalig)

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Er is ondersteuning toegevoegd voor Microsoft Azure Government-Cloud- en Microsoft Cloud Duitsland.

### <a name="ad-fs-management"></a>AD FS-Management
#### <a name="fixed-issues"></a>Problemen opgelost
* De initialisatie is mislukt-ADSyncNGCKeysWriteBack cmdlet in de AD prep powershell-module is onjuist ACL's toepassen op de container voor apparaatregistratie en zou daarom alleen overnemen van bestaande machtigingen.  Dit is bijgewerkt, zodat de sync-serviceaccount de juiste machtigingen heeft.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* De taak AAD Connect controleren of AD FS-aanmelding is bijgewerkt zodat er aanmeldpogingen uitvoeren op Microsoft Online en niet alleen token voor het ophalen van AD FS wordt gecontroleerd.
* Bij het instellen van een nieuwe AD FS-farm met behulp van AAD Connect, wordt de pagina gevraagd om referenties van de AD FS is verplaatst zodat deze nu plaatsvindt voordat de gebruiker wordt gevraagd voor AD FS en WAP-servers.  Hierdoor kan de AAD Connect om te controleren of het opgegeven account de juiste machtigingen heeft.
* Tijdens de upgrade voor AAD Connect wordt we niet langer een upgrade mislukt als de AD FS AAD-vertrouwensrelatie is mislukt om bij te werken.  Als dit gebeurt, wordt de gebruiker een waarschuwing wordt weergegeven en moet doorgaan naar de vertrouwensrelatie via de AAD Connect extra taak opnieuw instellen.

### <a name="seamless-single-sign-on"></a>Naadloze eenmalige aanmelding
#### <a name="fixed-issues"></a>Problemen opgelost
* Er is een probleem waardoor Azure AD Connect-wizard een fout geretourneerd als u probeert om in te schakelen opgelost [naadloze eenmalige aanmelding](how-to-connect-sso.md). Het foutbericht is *"Configuratie van Microsoft Azure AD Connect Authentication-Agent is mislukt."* Dit probleem is van invloed op bestaande klanten die handmatig upgrade hebben uitgevoerd van de preview-versie van de verificatie-Agents voor [Pass through-verificatie](how-to-connect-sso.md) op basis van de stappen in deze [artikel](how-to-connect-pta-upgrade-preview-authentication-agents.md).


## <a name="115610"></a>1.1.561.0
Status: 23 juli-2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Probleem opgelost

* Er is een probleem waardoor de synchronisatieregel voor out-of-box "Out met AD - gebruiker ImmutableId" opgelost moet worden verwijderd:

  * Dit probleem doet zich voor wanneer Azure AD Connect is bijgewerkt, of wanneer de taakoptie *synchronisatieconfiguratie bijwerken* in de Azure AD Connect-wizard om bij te werken van de configuratie van de Azure AD Connect-synchronisatie wordt gebruikt.
  
  * Deze synchronisatieregel van toepassing op klanten die hebben ingeschakeld is de [ms-DS-ConsistencyGuid als Bronanker functie](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor). Deze functie is geïntroduceerd in versie 1.1.524.0 en hoger en na. Wanneer de regel voor synchronisatie wordt verwijderd, Azure AD Connect niet meer kunt vullen on-premises AD-ms-DS-ConsistencyGuid kenmerk met de waarde van het kenmerk ObjectGuid. Hiermee wordt niet voorkomen dat nieuwe gebruikers wordt ingericht in Azure AD.
  
  * De oplossing zorgt ervoor dat de regel voor synchronisatie niet meer worden verwijderd tijdens de upgrade of tijdens het wijzigen van de configuratie, zolang de functie is ingeschakeld. Voor bestaande klanten die voor dit probleem gevolgen hebben, zorgt de oplossing er ook voor dat de synchronisatieregel terug na de upgrade naar deze versie van Azure AD Connect wordt toegevoegd.

* Er is een probleem dat ervoor zorgt out-of-box-synchronisatieregels dat hebben prioriteit die lager is dan 100 opgelost:

  * In het algemeen zijn prioriteit waarden, 0 - 99 gereserveerd voor aangepaste synchronisatieregels. Tijdens de upgrade, worden de waarden van de prioriteit voor out-of-box-synchronisatieregels bijgewerkt zodat wijzigingen in synchronisatie-regel. Vanwege dit probleem op door worden out-of-box-synchronisatieregels toegewezen prioriteit die lager is dan 100.
  
  * De oplossing wordt voorkomen dat het probleem zich voordoet tijdens de upgrade. Echter, worden niet hersteld de prioriteit van waarden voor bestaande klanten die met het probleem gevolgen hebben. Een afzonderlijke oplossing wordt in de toekomst worden opgegeven om te helpen bij het herstel.

* Er is een probleem opgelost waarbij de [domein en OE filteren scherm](how-to-connect-install-custom.md#domain-and-ou-filtering) in de Azure AD Connect wizard wordt weergegeven *alle domeinen en OE's filteren* optie geselecteerd is, zelfs als op basis van een organisatie-eenheid-filtering is ingeschakeld.

*   Er is een probleem opgelost waardoor de [mappartities configureren scherm](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) in de Synchronization Service Manager op een fout geretourneerd als de *vernieuwen* knop wordt geklikt. Het foutbericht is *"is een fout opgetreden tijdens het vernieuwen van domeinen: Kan geen cast-object van het type 'System.Collections.ArrayList' naar het type ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject. "* De fout treedt op wanneer een nieuw AD-domein is toegevoegd aan een bestaand AD-forest en u probeert bij te werken van Azure AD Connect met de knop vernieuwen.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

* [De functie voor automatisch bijwerken](how-to-connect-install-automatic-upgrade.md) is uitgebreid om ondersteuning voor klanten met de volgende configuraties:
  * U kunt de functie apparaat terugschrijven hebt ingeschakeld.
  * U kunt de Write-back van groep functie hebt ingeschakeld.
  * De installatie is niet een Express-instellingen of een upgrade van DirSync.
  * U hebt meer dan 100.000 objecten in de metaverse.
  * U verbinding maakt met meer dan één forest. Snelle installatie maakt alleen verbinding met één forest.
  * De AD-Connector-account is niet het standaardaccount voor MSOL_ meer.
  * De server is ingesteld op in de faseringsmodus bevindt.
  * U kunt de functie terugschrijven hebt ingeschakeld.
  
  >[!NOTE]
  >De uitbreiding van het bereik van de functie Automatische Upgrade heeft betrekking op klanten met Azure AD Connect build 1.1.105.0 en na. Als u niet dat uw Azure AD Connect-server automatisch worden bijgewerkt wilt, moet u uitvoeren de volgende cmdlet op uw Azure AD Connect-server: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Raadpleeg het artikel voor meer informatie over het inschakelen/uitschakelen van automatische Upgrade [Azure AD Connect: Automatische upgrade](how-to-connect-install-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Status: Wordt niet vrijgegeven. Wijzigingen in deze versie zijn opgenomen in versie 1.1.561.0.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Probleem opgelost

* Er is een probleem waardoor de synchronisatieregel voor out-of-box "Out met AD - gebruiker ImmutableId" opgelost moeten worden verwijderd wanneer de configuratie van filteren op basis van een organisatie-eenheid is bijgewerkt. Deze synchronisatieregel is vereist voor de [ms-DS-ConsistencyGuid als Bronanker functie](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor).

* Er is een probleem opgelost waarbij de [domein en OE filteren scherm](how-to-connect-install-custom.md#domain-and-ou-filtering) in de Azure AD Connect wizard wordt weergegeven *alle domeinen en OE's filteren* optie geselecteerd is, zelfs als op basis van een organisatie-eenheid-filtering is ingeschakeld.

*   Er is een probleem opgelost waardoor de [mappartities configureren scherm](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) in de Synchronization Service Manager op een fout geretourneerd als de *vernieuwen* knop wordt geklikt. Het foutbericht is *"is een fout opgetreden tijdens het vernieuwen van domeinen: Kan geen cast-object van het type 'System.Collections.ArrayList' naar het type ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject. "* De fout treedt op wanneer een nieuw AD-domein is toegevoegd aan een bestaand AD-forest en u probeert bij te werken van Azure AD Connect met de knop vernieuwen.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

* [De functie voor automatisch bijwerken](how-to-connect-install-automatic-upgrade.md) is uitgebreid om ondersteuning voor klanten met de volgende configuraties:
  * U kunt de functie apparaat terugschrijven hebt ingeschakeld.
  * U kunt de Write-back van groep functie hebt ingeschakeld.
  * De installatie is niet een Express-instellingen of een upgrade van DirSync.
  * U hebt meer dan 100.000 objecten in de metaverse.
  * U verbinding maakt met meer dan één forest. Snelle installatie maakt alleen verbinding met één forest.
  * De AD-Connector-account is niet het standaardaccount voor MSOL_ meer.
  * De server is ingesteld op in de faseringsmodus bevindt.
  * U kunt de functie terugschrijven hebt ingeschakeld.
  
  >[!NOTE]
  >De uitbreiding van het bereik van de functie Automatische Upgrade heeft betrekking op klanten met Azure AD Connect build 1.1.105.0 en na. Als u niet dat uw Azure AD Connect-server automatisch worden bijgewerkt wilt, moet u uitvoeren de volgende cmdlet op uw Azure AD Connect-server: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Raadpleeg het artikel voor meer informatie over het inschakelen/uitschakelen van automatische Upgrade [Azure AD Connect: Automatische upgrade](how-to-connect-install-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Status: Juli 2017

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie voor Azure AD Connect automatisch upgraden.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Probleem opgelost
* Een probleem opgelost met de cmdlet Initialize-ADSyncDomainJoinedComputerSync waardoor het geverifieerde domein voor op het bestaande object verbinding servicepunt moet worden gewijzigd, zelfs als deze nog steeds een geldig domein geconfigureerd. Dit probleem treedt op wanneer uw Azure AD-tenant meer dan één geverifieerde domeinen die kunnen worden gebruikt heeft voor het configureren van de service connection point.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Wachtwoord terugschrijven is nu beschikbaar voor Preview-versie met Microsoft Azure Government-cloud- en Microsoft Cloud Duitsland. Raadpleeg het artikel voor meer informatie over Azure AD Connect-ondersteuning voor de exemplaren van de verschillende [Azure AD Connect: Speciale overwegingen voor exemplaren](reference-connect-instances.md).

* De cmdlet Initialize-ADSyncDomainJoinedComputerSync heeft nu een nieuwe optionele parameter met de naam AzureADDomain. Deze parameter kunt u opgeven welke geverifieerd domein moet worden gebruikt voor het configureren van de service connection point.

### <a name="pass-through-authentication"></a>Pass-through-verificatie

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* De naam van de agent vereist voor Pass through-verificatie is gewijzigd van *Microsoft Azure AD Application Proxy Connector* naar *Microsoft Azure AD Connect Authentication-Agent*.

* Wachtwoord-Hashsynchronisatie inschakelen van Pass-through-verificatie niet meer standaard worden ingeschakeld.


## <a name="115530"></a>1.1.553.0
Status: Juni 2017

> [!IMPORTANT]
> Er zijn schema en de synchronisatie regel wijzigingen die zijn geïntroduceerd in deze versie. Azure AD Connect-synchronisatieservice activeren volledige Import en een volledige synchronisatie stappen na de upgrade. Details van de wijzigingen worden hieronder beschreven. Om een tijdelijk volledige Import en een volledige synchronisatie stappen na de upgrade, raadpleegt u het artikel [het uitstellen van volledige synchronisatie na de upgrade](how-to-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync

#### <a name="known-issue"></a>Bekend probleem
* Er is een probleem dat betrekking heeft op klanten die [OE filteren](how-to-connect-sync-configure-filtering.md#organizational-unitbased-filtering) met Azure AD Connect-synchronisatie. Wanneer u gaat u naar de [domein en OE filteren pagina](how-to-connect-install-custom.md#domain-and-ou-filtering) verwacht in de Azure AD Connect-wizard het volgende gedrag:
  * Als u OE filteren is ingeschakeld, de **geselecteerde domeinen en OE's filteren** optie is geselecteerd.
  * Anders wordt de **alle domeinen en OE's filteren** optie is geselecteerd.

Het probleem dat zich voordoet, is dat de **alle domeinen en organisatie-eenheden optie** is altijd ingeschakeld wanneer u de Wizard uitvoert.  Dit gebeurt zelfs als filteren op basis van organisatie-eenheid van eerder was geconfigureerd. Voordat u opslaat eventuele configuratiewijzigingen AAD Connect, zorg ervoor dat de **synchroniseren geselecteerde domeinen en organisatie-eenheden-optie is geselecteerd** en bevestigt u dat alle OE's die u nodig hebt om te synchroniseren opnieuw zijn ingeschakeld. Anders wordt OE filteren uitgeschakeld.

#### <a name="fixed-issues"></a>Problemen opgelost

* Er is een probleem opgelost met wachtwoord terugschrijven waarmee een Azure AD-beheerder opnieuw instellen van het wachtwoord van een on-premises AD-gebruikersaccount bevoegdheden. Het probleem treedt op wanneer Azure AD Connect is de machtiging wachtwoord opnieuw instellen via het bevoorrechte account. Het probleem is verholpen in deze versie van Azure AD Connect door Azure AD-beheerder opnieuw instellen van het wachtwoord van een willekeurige on-premises AD-gebruikersaccount bevoegde, tenzij de beheerder de eigenaar van dat account is. Raadpleeg voor meer informatie, [Security Advisory 4033453](https://technet.microsoft.com/library/security/4033453).

* Er is een probleem met betrekking tot opgelost de [ms-DS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) functie waar Azure AD Connect niet terugschrijven naar biedt lokale AD-ms-DS-ConsistencyGuid-kenmerk. Dit probleem doet zich voor wanneer er meerdere on-premises AD-forests die zijn toegevoegd aan Azure AD Connect en de *gebruikers-id's aanwezig zijn in meerdere mappen optie* is geselecteerd. Wanneer deze configuratie wordt gebruikt, kunnen de resulterende synchronisatieregels niet het kenmerk sourceAnchorBinary in de Metaverse invullen. Het kenmerk sourceAnchorBinary wordt gebruikt als het bronkenmerk voor ms-DS-ConsistencyGuid kenmerk. Write-back met het kenmerk ms-DSConsistencyGuid als gevolg hiervan wordt niet uitgevoerd. Los het probleem, zijn volgende synchronisatieregels bijgewerkt om ervoor te zorgen dat het kenmerk sourceAnchorBinary in de Metaverse altijd werden ingevuld:
  * In uit Active Directory - InetOrgPerson AccountEnabled.xml
  * In uit Active Directory - InetOrgPerson Common.xml
  * In uit Active Directory - gebruiker AccountEnabled.xml
  * In uit Active Directory - gebruiker Common.xml
  * In uit Active Directory - gebruiker toevoegen SOAInAAD.xml

* Eerder, zelfs als de [ms-DS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) functie is niet ingeschakeld, de synchronisatieregel "Out met AD-gebruiker ImmutableId" is nog steeds toegevoegd aan Azure AD Connect. Het effect is goedaardige en niet leidt tot Write-back van ms-DS-ConsistencyGuid-kenmerk moet plaatsvinden. Om verwarring te voorkomen, is logische toegevoegd om ervoor te zorgen dat er alleen de synchronisatieregel wordt toegevoegd als de functie is ingeschakeld.

* Er is een probleem waardoor de wachtwoord-hashsynchronisatie mislukken met foutgebeurtenis 611 opgelost. Dit probleem treedt op een of meer domain controllers zijn verwijderd uit on-premises AD. Aan het einde van elke cyclus van de synchronisatie van wachtwoord, de cookie voor adreslijstsynchronisatie van dat is uitgegeven door on-premises AD aanroep-id's van de verwijderde domeincontrollers met de waarde van 0 USN (Update Sequence Number) bevat. De Manager van de synchronisatie van wachtwoord kan niet worden om vast te leggen synchronisatie cookie met USN-waarde van 0 en mislukt en er is een foutgebeurtenis 611. Tijdens de volgende synchronisatiecyclus hergebruikt wachtwoord Synchronization Manager de laatste synchronisatie persistente cookie die geen USN-waarde van 0 bevat. Dit zorgt ervoor dat de dezelfde wachtwoordwijzigingen opnieuw worden gesynchroniseerd. Met deze oplossing wordt Synchronisatiebeheer wachtwoord clusterverbinding blijven behouden de cookie voor adreslijstsynchronisatie van correct.

* Eerder, zelfs als automatische bijwerken is uitgeschakeld met behulp van de cmdlet Set-ADSyncAutoUpgrade, de automatische Upgrade dat proces wordt voortgezet om te controleren op periodiek bijwerken, en is afhankelijk van het gedownloade installatieprogramma deactivering in acht neemt. Met deze oplossing wordt de automatische Upgrade-proces niet langer wordt gecontroleerd voor upgrade periodiek. De oplossing wordt automatisch toegepast wanneer de upgrade installatieprogramma voor deze versie van Azure AD Connect eenmaal wordt uitgevoerd.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

* Voorheen was de [ms-DS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-ms-ds-consistencyguid-as-sourceanchor) functie is beschikbaar voor alleen voor nieuwe implementaties. Het is nu beschikbaar voor bestaande implementaties. Met name:
  * Voor toegang tot de functie, start u de Azure AD Connect-wizard en kies de *Update Bronanker* optie.
  * Deze optie is alleen zichtbaar voor bestaande implementaties die van objectGuid als sourceAnchor-kenmerk gebruikmaken.
  * Bij het configureren van de optie, valideert de wizard de status van het kenmerk ms-DS-ConsistencyGuid in uw on-premises Active Directory. Als het kenmerk is niet geconfigureerd op een gebruikersobject in de directory, gebruikt de wizard ms-DS-ConsistencyGuid als het kenmerk sourceAnchor. Als het kenmerk is geconfigureerd op een of meer objecten in de map, wordt de wizard concludeert het kenmerk wordt gebruikt door andere toepassingen en is niet geschikt als sourceAnchor-kenmerk en staat niet toe dat de wijziging Bronanker om door te gaan. Als u er zeker van zijn dat het kenmerk wordt niet door bestaande toepassingen gebruikt, moet u contact op met ondersteuning voor meer informatie over het onderdrukken van de fout.

* Specifieke **userCertificate** kenmerk op apparaatobjecten, Azure AD Connect nu eruitziet voor waarden van de certificaten die zijn vereist voor [domein apparaten verbinden met Azure AD voor Windows 10](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) en filters voor de overige voordat u synchroniseert met Azure AD. Als u wilt inschakelen dit gedrag, de synchronisatieregel voor out-of-box "Uit naar AAD – apparaat Join SOAInAD" is bijgewerkt.

* Azure AD Connect nu ondersteunt Write-back van Exchange Online **cloudPublicDelegates** kenmerk naar on-premises AD dat **publicDelegates** kenmerk. Hierdoor kan het scenario waarbij een Exchange Online-postbus kan worden verleend rechten SendOnBehalfTo aan gebruikers met on-premises Exchange-postvak. Ter ondersteuning van deze functie, een nieuwe synchronisatieregel van de out-of-box 'Out met AD-Write-back van gebruiker Exchange Hybrid PublicDelegates' is toegevoegd. Deze synchronisatieregel wordt alleen toegevoegd aan Azure AD Connect als Exchange hybride-functie is ingeschakeld.

* Azure AD Connect ondersteunt voor de nu synchroniseren van de **altRecipient** kenmerk uit Azure AD. Ter ondersteuning van deze wijziging, zijn volgende out-of-box-synchronisatieregels bijgewerkt om op te nemen van de stroom vereist kenmerk:
  * In uit Active Directory-gebruiker Exchange
  * Out voor AAD-gebruiker ExchangeOnline
  
* De **cloudSOAExchMailbox** kenmerk in de Metaverse geeft aan of een bepaalde gebruiker Exchange Online-postbus of niet heeft. De definitie ervan is bijgewerkt om op te nemen van extra Exchange Online RecipientDisplayTypes als die apparatuur en vergaderruimte postvakken. Als u wilt deze wijziging hebt ingeschakeld, is de definitie van het kenmerk cloudSOAExchMailbox, vindt u onder out-of-box synchronisatieregel 'In uit AAD – gebruiker Exchange hybride', bijgewerkt van:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

... naar het volgende:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* De volgende set X509Certificate2-compatibel is met functies voor het maken van synchronisatie Regelexpressies voor het afhandelen van certificaatwaarden in het kenmerk userCertificate toegevoegd:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|CertThumbprint|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Selecteer|
    |CertKeyAlgorithmParams|CertHashString|Waar|
    |||Met|

* Wijzigingen in het volgende schema zijn geïntroduceerd om toe te staan van klanten om van aangepaste synchronisatieregels sAMAccountName, domainNetBios en domainFQDN voor groepsobjecten worden weergegeven, evenals distinguishedName voor gebruikersobjecten stromen te maken:

  * Volgende kenmerken zijn toegevoegd aan MV-schema:
    * Groep: Accountnaam
    * Groep: domainNetBios
    * Groep: domainFQDN
    * Persoon: distinguishedName

  * Volgende kenmerken zijn toegevoegd aan Azure AD-Connector schema:
    * Groep: OnPremisesSamAccountName
    * Groep: NetBiosName
    * Groep: DnsDomainName
    * Gebruiker: OnPremisesDistinguishedName

* Het script van de cmdlet ADSyncDomainJoinedComputerSync heeft nu een nieuwe optionele parameter met de naam Azure-omgeving. De parameter wordt gebruikt om op te geven welke regio waarin de bijbehorende Azure Active Directory-tenant wordt gehost in. Geldige waarden zijn:
  * AzureCloud (default)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Bijgewerkte synchronisatie Rule Editor gebruiken deelnemen aan (in plaats van inrichten) als de standaardwaarde van het koppelingstype tijdens het maken van de regel voor synchronisatie.

### <a name="ad-fs-management"></a>AD FS-management

#### <a name="issues-fixed"></a>Problemen opgelost

* De volgende URL's zijn nieuwe WS-Federation-eindpunten die zijn geïntroduceerd door Azure AD voor het verbeteren van tolerantie tegen uitval van verificatie en wordt toegevoegd met on-premises AD FS-configuratie voor het vertrouwensrelatie van Relying Party derden:
  * https://ests.login.microsoftonline.com/login.srf
  * https://stamp2.login.microsoftonline.com/login.srf
  * https://ccs.login.microsoftonline.com/login.srf
  * https://ccs-sdf.login.microsoftonline.com/login.srf
  
* Er is een probleem waardoor AD FS voor het genereren van onjuist claimwaarde voor IssuerID opgelost. Dit probleem doet zich voor als er meerdere geverifieerd domeinen in de Azure AD-tenant zijn en het domeinachtervoegsel van het kenmerk userPrincipalName is gebruikt voor het genereren van de IssuerID-claim ten minste is 3 niveaus diep (bijvoorbeeld johndoe@us.contoso.com). Het probleem is opgelost door het bijwerken van de reguliere expressie die wordt gebruikt door de claimregels.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Eerder kan de AD FS Certificate Management-functie geleverd door Azure AD Connect alleen worden gebruikt met AD FS-farms die worden beheerd via Azure AD Connect. U kunt de functie nu gebruiken met AD FS-farms die niet worden beheerd met Azure AD Connect.

## <a name="115240"></a>1.1.524.0
Uitgebracht: Mei 2017

> [!IMPORTANT]
> Er zijn schema en de synchronisatie regel wijzigingen die zijn geïntroduceerd in deze versie. Azure AD Connect-synchronisatieservice activeren volledige Import en een volledige synchronisatie stappen na de upgrade. Details van de wijzigingen worden hieronder beschreven.
>
>

**Opgeloste problemen:**

Azure AD Connect-synchronisatie

* Er is een probleem dat ervoor zorgt dat de automatische Upgrade worden uitgevoerd op de Azure AD Connect-server, zelfs als de functie met de cmdlet Set-ADSyncAutoUpgrade is uitgeschakeld door de klant opgelost. Met deze oplossing wordt de automatische Upgrade-proces op de server nog steeds wordt gecontroleerd voor upgrade periodiek, maar het gedownloade installatieprogramma zich houdt aan de configuratie voor automatisch bijwerken.
* Tijdens het in-place upgrade van DirSync maakt Azure AD Connect een Azure AD-serviceaccount moet worden gebruikt door de Azure AD-connector voor het synchroniseren met Azure AD. Wanneer het account is gemaakt, wordt Azure AD Connect verifieert met Azure AD met behulp van het account. Soms verificatie is mislukt vanwege problemen van voorbijgaande aard, die op zijn beurt zorgt ervoor dat in-place upgrade van DirSync mislukt met fout *"uitvoerende configureren AAD Sync-taak is een fout is opgetreden: AADSTS50034: Als u wilt aanmelden bij deze toepassing, het account moet worden toegevoegd aan de map xxx.onmicrosoft.com."* Ter verbetering van de flexibiliteit van de upgrade van DirSync, Azure AD Connect nu de stap opnieuw probeert verificatie.
* Er is een probleem met build 443 die ervoor zorgt dat de in-place upgrade van DirSync te voltooien, maar vereist is voor adreslijstsynchronisatie uitvoeringsprofielen worden niet gemaakt. Herstel van logica is opgenomen in deze versie van Azure AD Connect. Wanneer de klant wordt bijgewerkt naar deze versie, wordt Azure AD Connect detecteert uitvoeringsprofielen ontbreekt en ze zijn gemaakt.
* Er is een probleem dat ervoor zorgt dat de synchronisatie van wachtwoord-proces niet worden gestart met de gebeurtenis-ID 6900 en fout opgelost *'een item met dezelfde sleutel is al toegevoegd'*. Dit probleem treedt op als u OE filteren configureren om op te nemen van AD-configuratiepartitie bijwerken. U kunt dit probleem oplossen, synchroniseert Wachtwoordsynchronisatie proces nu wachtwoordwijzigingen vanuit AD domeinpartities alleen. Niet-domeinpartities, zoals de configuratiepartitie worden overgeslagen.
* Tijdens de snelle installatie, Azure AD Connect maakt een on-premises AD DS-account moet worden gebruikt door de AD-connector om te communiceren met on-premises AD. Voorheen was het account is gemaakt met de PASSWD_NOTREQD-vlag ingesteld op het Gebruikersaccountbeheer kenmerk en een willekeurig wachtwoord is ingesteld op het account. Nu Azure AD Connect expliciet Hiermee verwijdert u de vlag PASSWD_NOTREQD nadat het wachtwoord is ingesteld op het account.
* Er is een probleem dat ervoor zorgt de upgrade van DirSync dat mislukken met fout opgelost *"een impasse opgetreden in sql server die bij het verkrijgen van een toepassingsvergrendeling"* als het kenmerk mailNickname is gevonden in de on-premises AD-schema, maar is niet beperkt tot aan de objectklasse AD-gebruiker.
* Een probleem dat ervoor zorgt dat de functie van Write-back van apparaat automatisch uitgeschakeld wanneer een beheerder wordt bijgewerkt met behulp van Azure AD Connect-wizard Azure AD Connect-synchronisatieconfiguratie is opgelost. Dit probleem wordt veroorzaakt door de wizard een vereiste controle uitvoeren voor de configuratie van het bestaande apparaat terugschrijven van wachtwoorden in on-premises AD en de controle is mislukt. De oplossing is de controle overslaat als write-back van apparaat al eerder is ingeschakeld.
* Als u wilt configureren OE filteren, kunt u ofwel de Azure AD Connect-wizard of de Synchronization Service Manager gebruiken. Eerder, als u de wizard Azure AD Connect configureren OE filteren, nieuwe organisatie-eenheden gemaakt daarna zijn opgenomen voor directorysynchronisatie. Als u niet dat nieuwe OE's wilt moeten worden opgenomen, moet u het OE filteren met behulp van de Synchronization Service Manager configureren. U kunt nu hetzelfde gedrag met behulp van Azure AD Connect-wizard bereiken.
* Een probleem dat ervoor zorgt opgeslagen procedures die zijn vereist voor Azure AD Connect dat moet worden gemaakt onder het schema van de installatie-beheerder, in plaats van onder het dbo-schema is opgelost.
* Een probleem dat ervoor zorgt het kenmerk TrackingId is geretourneerd door Azure AD dat moeten worden weggelaten in de AAD Connect Server gebeurtenislogboeken is opgelost. Het probleem doet zich voor als Azure AD Connect een bericht voor de omleiding van Azure AD ontvangt en Azure AD Connect kan geen verbinding maken met het opgegeven eindpunt is. De TrackingId wordt gebruikt door ondersteuningsmedewerkers voor het correleren met de service aan logboeken tijdens het oplossen van problemen.
* Wanneer Azure AD Connect LargeObject-fout van Azure AD ontvangt, Azure AD Connect genereert een gebeurtenis met gebeurtenis-id 6941 en het bericht *"het ingerichte object is te groot. Verklein het aantal kenmerkwaarden voor dit object."* Op hetzelfde moment, genereert Azure AD Connect ook een misleidend gebeurtenis met gebeurtenis-id 6900 en het bericht *"Microsoft.Online.Coexistence.ProvisionRetryException: Kan niet communiceren met de service Windows Azure Active Directory. "* Om verwarring, genereert Azure AD Connect niet langer de laatste gebeurtenis wanneer LargeObject-fout is ontvangen.
* Er is een probleem dat ervoor zorgt de Synchronization Service Manager dat te reageren tijdens het bijwerken van de configuratie voor algemene LDAP-connector opgelost.

**Nieuwe functies/verbeteringen:**

Azure AD Connect-synchronisatie
* Wijzigingen in synchronisatie-regel: wijzigingen in de volgende synchronisatie de regel zijn geïmplementeerd:
  * Synchronisatieregel bijgewerkt standaard ingesteld op niet exporteren kenmerken **userCertificate** en **usersmimecertificate wordt door** als de kenmerken meer dan 15 waarden hebben.
  * AD-kenmerken **employeeID** en **msExchBypassModerationLink** zijn nu opgenomen in de regelset van de standaard-synchronisatie.
  * AD-kenmerk **foto** is verwijderd uit de standaardset van het synchronisatie-regel.
  * Toegevoegd **preferredDataLocation** naar de Metaverse-schema en het schema van de AAD-Connector. Klanten die willen werken beide kenmerken in Azure AD kunnen implementeren, aangepaste synchronisatieregels om dit te doen. 
  * Toegevoegd **userType** naar de Metaverse-schema en het schema van de AAD-Connector. Klanten die willen werken beide kenmerken in Azure AD kunnen implementeren, aangepaste synchronisatieregels om dit te doen.

* Azure AD Connect automatisch kunt nu het gebruik van ConsistencyGuid kenmerk als het kenmerk Bronanker voor on-premises AD-objecten. Bovendien kunnen de Azure AD Connect vult het ConsistencyGuid-kenmerk met de waarde van het kenmerk objectGuid als deze leeg is. Deze functie is van toepassing op nieuwe alleen-implementatie. Meer informatie over deze functie, Raadpleeg de sectie [Azure AD Connect: Ontwerpconcepten - ms-DS-ConsistencyGuid gebruiken als sourceAnchor](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor).
* Nieuwe probleemoplossing cmdlet is Invoke-ADSyncDiagnostics toegevoegd om u te helpen bij het analyseren van wachtwoord-Hashsynchronisatie problemen met betrekking tot. Raadpleeg het artikel voor meer informatie over het gebruik van de cmdlet [wachtwoord-hashsynchronisatie met Azure AD Connect-synchronisatie oplossen](tshoot-connect-password-hash-synchronization.md).
* Azure AD Connect nu ondersteunt het synchroniseren van de berichtgrootte openbare map waarin de objecten van on-premises AD met Azure AD. U kunt de functie met behulp van Azure AD Connect-wizard onder optionele functies inschakelen. Meer informatie over deze functie, raadpleegt u het artikel [Office 365 Directory op basis van Edge blokkeren van ondersteuning voor on-premises ingeschakeld openbare e-mailmappen](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect vereist een AD DS-account voor synchronisatie vanuit on-premises AD. Eerder, als u Azure AD Connect met behulp van de Express-modus hebt geïnstalleerd, kunt u opgeven dat de referenties van een ondernemingsadministrator-account en een Azure AD Connect maakt de AD DS-account vereist. Voor een aangepaste installatie en het forests toevoegen aan een bestaande implementatie, moet u zijn echter in plaats daarvan geeft u de AD DS-account. Nu hebt u ook de optie voor het opgeven van de referenties van een Enterprise-beheerdersaccount tijdens een aangepaste installatie en laat Azure AD Connect maken van de AD DS-account vereist.
* Azure AD Connect biedt nu ondersteuning voor SQL AOA. Voordat u Azure AD Connect installeert, moet u SQL AOA inschakelen. Tijdens de installatie van detecteert Azure AD Connect of de opgegeven SQL-exemplaar voor SQL AOA is ingeschakeld of niet. Als SQL AOA is ingeschakeld, wordt Azure AD Connect verder zoekt uit als SQL AOA is geconfigureerd voor gebruik van de replicatie van synchrone of asynchrone replicatie. Bij het instellen van de beschikbaarheidsgroep-Listener, is het raadzaam dat u de eigenschap RegisterAllProvidersIP ingesteld op 0. Deze aanbeveling is omdat Azure AD Connect momenteel SQL Native Client gebruikt verbinding maken met SQL en SQL Native Client biedt geen ondersteuning voor het gebruik van de eigenschap MultiSubNetFailover.
* Als u LocalDB als de database voor uw Azure AD Connect-server gebruikt en de grootte van 10 GB heeft bereikt, wordt de synchronisatieservice niet langer wordt gestart. Eerder, moet u ShrinkDatabase bewerking uitvoeren op de LocalDB het vrijmaken van voldoende ruimte DB voor de synchronisatieservice om te starten. Waarna, kunt u de Synchronization Service Manager verwijderen van uitvoeringsgeschiedenis om meer DB vrij te maken. Nu kunt u de cmdlet Start-ADSyncPurgeRunHistory aan gegevens van geschiedenis van leegmaken uitvoeren van LocalDB DB ruimte vrij te maken. Bovendien is deze cmdlet biedt ondersteuning voor offline modus (door op te geven de parameter - offline) die kan worden gebruikt wanneer de synchronisatieservice niet wordt uitgevoerd. Opmerking: De offline modus kan alleen worden gebruikt als de synchronisatieservice niet actief is en de database die wordt gebruikt LocalDB is.
* Om te verminderen de hoeveelheid opslagruimte vereist, Azure AD comprimeert Connect nu synchronisatie foutdetails voordat ze worden opgeslagen in LocalDB/SQL-databases. Bij een upgrade van een oudere versie van Azure AD Connect naar deze versie, voert Azure AD Connect een eenmalige compressie op bestaande synchronisatie-foutdetails.
* Eerder, na het bijwerken van organisatie-eenheid filteren configuratie, moet u handmatig uitvoeren volledige import om ervoor te zorgen bestaande objecten zijn correct opgenomen/uitgesloten van directory-synchronisatie. Azure AD Connect wordt nu automatisch volledige import cyclus tijdens de volgende synchronisatie. Verdere, volledige import wordt alleen toegepast op de AD-connectors beïnvloed door de update. Opmerking: deze verbetering is van toepassing op organisatie-eenheid gemaakt met behulp van de Azure AD Connect-wizard alleen updates te filteren. Het is niet van toepassing op OE filteren update gemaakt met behulp van de Synchronization Service Manager.
* Voorheen filteren op basis van een groep biedt ondersteuning voor gebruikers, groepen en neem contact op met alleen objecten. Op basis van een groep filteren ondersteunt nu ook computerobjecten.
* U kunt eerder Connectorgebied gegevens verwijderen zonder Azure AD Connect sync scheduler uit te schakelen. Synchronization Service Manager nu, blokkeert de verwijdering van Connectorgebied gegevens als wordt gedetecteerd dat de planner is ingeschakeld. Bovendien wordt wordt een waarschuwing geretourneerd naar het klanten informeren over het verlies van gegevens als de gegevens van de Connector-ruimte is verwijderd.
* Eerder, moet u PowerShell transcriptie voor Azure AD Connect-wizard correct uit te voeren uitschakelen. Dit probleem is gedeeltelijk opgelost. U kunt PowerShell transcriptie inschakelen als u Azure AD Connect-wizard voor het beheren van configuratie voor synchroniseren. Als u Azure AD Connect-wizard voor het beheren van AD FS-configuratie, moet u PowerShell transcriptie uitschakelen.



## <a name="114860"></a>1.1.486.0
Uitgebracht: April 2017

**Opgeloste problemen:**
* Het probleem opgelost waarbij Azure AD Connect wordt niet geïnstalleerd op gelokaliseerde versie van Windows Server.

## <a name="114840"></a>1.1.484.0
Uitgebracht: April 2017

**Bekende problemen:**

* Deze versie van Azure AD Connect wordt niet goed geïnstalleerd als de volgende voorwaarden alle waar zijn:
   1. U kunt een van beide DirSync in-place upgrade of nieuwe installatie van Azure AD Connect wilt uitvoeren.
   2. U gebruikt een gelokaliseerde versie van Windows Server waarbij de naam van de ingebouwde groep Administrators op de server niet 'Administrators'.
   3. U gebruikt de standaard SQL Server 2012 Express LocalDB met Azure AD Connect in plaats van het leveren van uw eigen volledige SQL geïnstalleerd.

**Opgeloste problemen:**

Azure AD Connect-synchronisatie
* Een probleem opgelost waarbij de volledige synchronisatie stap door sync scheduler wordt overgeslagen als een of meer connectors uitvoeringsprofiel dat voor deze stap synchronisatie ontbreken. Bijvoorbeeld, u handmatig hebt toegevoegd een connector met behulp van de Synchronization Service Manager zonder te maken van het profiel voor het uitvoeren van een Delta-Import. Deze oplossing zorgt ervoor dat de synchronisatieplanning om uit te voeren van Delta-Import voor andere connectors blijft.
* Een probleem opgelost waarbij de Synchronization Service onmiddellijk stopt met de verwerking een uitvoeringsprofiel dat, wanneer het is een probleem met een van de stappen uitvoeren tegenkomt. Deze oplossing zorgt ervoor dat de synchronisatieservice wordt met de stap overgeslagen en voor het verwerken van de rest blijft. U hebt bijvoorbeeld een Delta-Import profiel voor uw AD-connector uitvoert met meerdere uitvoeren stappen (één voor elke on-premises AD-domein). De synchronisatieservice wordt Delta-Import uitgevoerd met de andere AD-domeinen, zelfs als een van deze problemen met de netwerkverbinding heeft.
* Er is een probleem dat ervoor zorgt de update die Azure AD-Connector dat moet worden overgeslagen tijdens de automatische Upgrade opgelost.
* Een probleem dat ervoor zorgt Azure AD Connect onjuist bepalen dat of de server een domeincontroller tijdens de installatie is is opgelost, welke in inschakelen zorgt ervoor dat de upgrade van DirSync mislukken.
* Een probleem dat ervoor zorgt dat de in-place upgrade van DirSync naar elk uitvoeringsprofiel niet maken voor de Azure AD-Connector is opgelost.
* Een probleem opgelost waarbij de Synchronization Service Manager-gebruikersinterface wordt niet meer reageert wanneer bij het configureren van algemene LDAP-Connector.

AD FS-management
* Een probleem opgelost waarbij de Azure AD Connect-wizard mislukt als de primaire AD FS-knooppunt is verplaatst naar een andere server.

Desktop-SSO
* Een probleem opgelost in de Azure AD Connect-wizard waar het aanmeldingsscherm kunt u niet Desktop-SSO-functie inschakelen als u ervoor hebt gekozen Wachtwoordsynchronisatie als uw aanmeldingsoptie tijdens de installatie van nieuwe.

**Nieuwe functies/verbeteringen:**

Azure AD Connect-synchronisatie
* Azure AD Connect Sync biedt nu ondersteuning voor het gebruik van Virtual Service Account, een beheerd serviceaccount en een groep beheerd serviceaccount als de service-account. Dit geldt voor de nieuwe installatie van Azure AD Connect. Bij het installeren van Azure AD Connect:
    * Standaard Azure AD Connect-wizard maakt u een Virtual Service Account en gebruikt deze als de service-account.
    * Als u op een domeincontroller installeert, Azure AD Connect gebruikgemaakt van vorige gedrag, waarbij een domeingebruikersaccount maakt en gebruikt deze als de service-account in plaats daarvan.
    * U kunt het standaardgedrag negeren door op te geven van een van de volgende:
      * Een groep beheerd serviceaccount
      * Een beheerd serviceaccount
      * Een domeingebruikersaccount
      * Een lokaal gebruikersaccount
* Als u een naar een nieuwe build van Azure AD Connect die upgrade voorheen connectors bijwerken of wijzigingen in synchronisatie-regel, Azure AD Connect de cyclus van een volledige synchronisatie wordt geactiveerd. Azure AD Connect activeert nu selectief volledige Import stap alleen voor connectors met update, en volledige synchronisatie alleen voor connectors met wijzigingen in synchronisatie-regel.
* Voorheen de drempelwaarde voor exporteren verwijderen is alleen van toepassing op de uitvoer die worden geactiveerd via de synchronisatieplanning. De functie is nu uitgebreid met uitvoer handmatig wordt geactiveerd door de klant die gebruikmaken van de Synchronization Service Manager.
* Er is een serviceconfiguratie waarmee wordt aangegeven of de synchronisatie van wachtwoord-functie is ingeschakeld voor uw tenant of niet op uw Azure AD-tenant. Eerder, is het eenvoudig voor de configuratie van de service niet correct worden geconfigureerd door Azure AD Connect wanneer u een actief en een staging-server. Nu Azure AD Connect probeert de serviceconfiguratie om consistent te houden aan uw actieve alleen Azure AD Connect-server.
* Azure AD Connect wizard detecteert nu en een waarschuwing wordt gegeven als on-premises AD geen AD Recycle Bin is ingeschakeld.
* Voorheen exporteren naar Azure AD-time-out en mislukt als de gecombineerde grootte van de objecten in de batch een bepaalde drempelwaarde overschrijdt. Nu de synchronisatieservice wordt opnieuw proberen te opnieuw te verzenden van de objecten in afzonderlijke, kleinere batches als het probleem is opgetreden.
* De synchronisatie-Service Key Management-toepassing is verwijderd uit het Menu Start van Windows. Beheer van de versleutelingssleutel blijven worden ondersteund via de opdrachtregelinterface miiskmu.exe gebruiken. Raadpleeg het artikel voor informatie over het beheren van de versleutelingssleutel, [wordt de versleutelingssleutel van de Azure AD Connect Sync afgebroken](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Eerder, als u het wachtwoord voor de Azure AD Connect sync-serviceaccount wijzigen, de Synchronization Service zich niet kunnen starten goed totdat u hebt de versleutelingssleutel afgebroken en het wachtwoord voor de Azure AD Connect sync-serviceaccount opnieuw geïnitialiseerd. Dit proces is nu niet langer vereist.

Desktop-SSO

* Azure AD Connect-wizard vereist niet langer poort 9090 worden geopend op het netwerk bij het configureren van Pass-through-verificatie en Desktop-SSO. Alleen poort 443 is vereist. 

## <a name="114430"></a>1.1.443.0
Uitgebracht: Maart 2017

**Opgeloste problemen:**

Azure AD Connect-synchronisatie
* Een probleem waardoor Azure AD Connect-wizard mislukken als de weergavenaam van de Azure AD-Connector, de initiële onmicrosoft.com-domein dat is toegewezen aan de Azure AD-tenant niet bevat is opgelost.
* Er is een probleem waardoor Azure AD Connect-wizard mislukken tijdens het maken van verbinding met SQL-database als het wachtwoord van het synchronisatieserviceaccount speciale tekens zoals apostrof, dubbele punt en ruimte bevat opgelost.
* Er is een probleem die ervoor zorgt de fout dat "de afbeelding heeft een anker die anders is dan de installatiekopie" opgelost worden uitgevoerd op een Azure AD Connect-server in de faseringsmodus bevindt, nadat u hebt tijdelijk een on-premises uitgesloten AD object kunnen worden gesynchroniseerd en vervolgens opnieuw uit voor het synchroniseren van opgenomen.
* Er is een probleem die ervoor zorgt de fout dat 'het object zich bevindt door de DN-naam is een phantomstuklijst' opgelost worden uitgevoerd op een Azure AD Connect-server in de faseringsmodus bevindt, nadat u hebt tijdelijk een on-premises uitgesloten AD object wordt gesynchroniseerd en vervolgens opnieuw opgenomen voor het synchroniseren.

AD FS-management
* Er is een probleem waar Azure AD Connect-wizard niet bijwerken van de AD FS-configuratie en de juiste claims op de relying party trust ingesteld nadat de alternatieve aanmeldings-ID is geconfigureerd opgelost.
* Een probleem opgelost waarbij Azure AD Connect-wizard is niet goed verwerkt AD FS-servers waarvan service-accounts zijn geconfigureerd met behulp van de indeling van de userPrincipalName in plaats van de sAMAccountName-indeling.

Pass-through-verificatie
* Een probleem waardoor Azure AD Connect-wizard mislukken als doorgeven via verificatie is ingeschakeld maar de registratie van de connector niet is opgelost.
* Er is een probleem waardoor Azure AD Connect-wizard als volgt te omzeilen validatie van controle op de Desktop-SSO-functie is ingeschakeld aanmeldingsmethode opgelost.

Wachtwoord opnieuw instellen
* Een probleem dat ertoe leiden de server Azure AAD Connect dat kan om niet opnieuw verbinding te maken als de verbinding is verwijderd door een firewall of proxy is opgelost.

**Nieuwe functies/verbeteringen:**

Azure AD Connect-synchronisatie
* Get-ADSyncScheduler cmdlet retourneert nu een nieuwe Booleaanse eigenschap met de naam SyncCycleInProgress. Als de geretourneerde waarde true is, betekent dit dat er een cyclus geplande synchronisatie wordt uitgevoerd is.
* Doelmap voor het opslaan van Azure AD Connect-installatie en setup-Logboeken is verplaatst uit %localappdata%\AADConnect naar %programdata%\AADConnect voor het verbeteren van toegankelijkheid van de logboekbestanden.

AD FS-management
* Er is ondersteuning toegevoegd voor het bijwerken van SSL-certificaat voor AD FS-Farm.
* Er is ondersteuning toegevoegd voor het beheer van AD FS 2016.
* U kunt nu bestaande gMSA (groep beheerd serviceaccount) opgeven tijdens de installatie van AD FS.
* U kunt nu SHA-256 als de handtekening hash-algoritme voor Azure AD-vertrouwensrelatie van relying party configureren.

Wachtwoord opnieuw instellen
* Verbeteringen voor het product te laten functioneren in omgevingen met strengere firewallregels geïntroduceerd.
* De betrouwbaarheid van de verbeterde verbinding met Azure Service Bus.

## <a name="113800"></a>1.1.380.0
Uitgebracht: December 2016

**Is een probleem opgelost:**

* Het probleem opgelost waarbij de issuerid-claimregel voor Active Directory Federation Services (AD FS) ontbreekt in deze versie.

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie voor Azure AD Connect automatisch upgraden.

## <a name="113710"></a>1.1.371.0
Uitgebracht: December 2016

**Bekende problemen:**

* De issuerid-claimregel voor AD FS ontbreekt in deze versie. De regel van de issuerid-claim is vereist als u meerdere domeinen met Azure Active Directory (Azure AD) zijn federeren. Als u Azure AD Connect voor het beheren van uw on-premises AD FS-implementatie, een upgrade naar deze versie verwijdert de bestaande issuerid-claimregel uit uw AD FS-configuratie. U kunt het probleem omzeilen door de issuerid-claimregel toe te voegen na de installatie/upgrade. Voor meer informatie over het toevoegen van de issuerid claim regel, verwijzen naar dit artikel op [ondersteuning voor meerdere domeinen voor federatie met Azure AD](how-to-connect-install-multiple-domains.md).

**Is een probleem opgelost:**

* Als poort 9090 is niet geopend voor de uitgaande verbinding, wordt de Azure AD Connect-installatie of upgrade mislukt.

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie voor Azure AD Connect automatisch upgraden.

## <a name="113700"></a>1.1.370.0
Uitgebracht: December 2016

**Bekende problemen:**

* De issuerid-claimregel voor AD FS ontbreekt in deze versie. De regel van de issuerid-claim is vereist als u meerdere domeinen met Azure AD federeert. Als u Azure AD Connect voor het beheren van uw on-premises AD FS-implementatie, een upgrade naar deze versie verwijdert de bestaande issuerid-claimregel uit uw AD FS-configuratie. U kunt het probleem omzeilen door de issuerid-claimregel toe te voegen na de installatie/upgrade. Voor meer informatie over het toevoegen van issuerid claim regel, verwijzen naar dit artikel op [ondersteuning voor meerdere domeinen voor federatie met Azure AD](how-to-connect-install-multiple-domains.md).
* Poort 9090 moet openen uitgaande om installatie te voltooien.

**Nieuwe functies**

* Pass through-verificatie (Preview).

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie voor Azure AD Connect automatisch upgraden.

## <a name="113430"></a>1.1.343.0
Uitgebracht: November 2016

**Bekende problemen:**

* De issuerid-claimregel voor AD FS ontbreekt in deze versie. De regel van de issuerid-claim is vereist als u meerdere domeinen met Azure AD federeert. Als u Azure AD Connect voor het beheren van uw on-premises AD FS-implementatie, een upgrade naar deze versie verwijdert de bestaande issuerid-claimregel uit uw AD FS-configuratie. U kunt het probleem omzeilen door de issuerid-claimregel toe te voegen na de installatie/upgrade. Voor meer informatie over het toevoegen van issuerid claim regel, verwijzen naar dit artikel op [ondersteuning voor meerdere domeinen voor federatie met Azure AD](how-to-connect-install-multiple-domains.md).

**Opgeloste problemen:**

* Azure AD Connect installeert mislukt soms, omdat deze kan niet worden gemaakt van een lokaal serviceaccount waarvan het wachtwoord voldoet aan het niveau van de complexiteit die zijn opgegeven door het wachtwoordbeleid van de organisatie.
* Een probleem opgelost waarbij join regels niet opnieuw geëvalueerd wanneer een object in het connectorgebied tegelijkertijd buiten het bereik wordt voor een lid van de regel en worden binnen de regeling vallen voor een andere. Dit kan gebeuren als er twee of meer join regels waarvan join-voorwaarden zijn sluiten elkaar wederzijds uit.
* Een probleem opgelost waarbij inkomende synchronisatieregels (vanuit Azure AD), die geen lid worden regels bevatten, niet worden verwerkt als ze hebben lagere prioriteit waarden dan die met regels voor lid worden.

**Verbeteringen**

* Er is ondersteuning toegevoegd voor het installeren van Azure AD Connect op Windows Server 2016 Standard of hoger.
* Er is ondersteuning toegevoegd voor het gebruik van SQL Server 2016 als de externe database voor Azure AD Connect.

## <a name="112810"></a>1.1.281.0
Uitgebracht: Augustus 2016

**Opgeloste problemen:**

* Wijzigingen in synchronisatie-interval doen niet plaatsvinden pas nadat de volgende synchronisatiecyclus voltooid is.
* Azure AD Connect-wizard accepteert geen een Azure AD-account waarvan de gebruikersnaam wordt gestart met een onderstrepingsteken (\_).
* Azure AD Connect-wizard is mislukt voor verificatie van de Azure AD-account als het accountwachtwoord te veel tekens bevat. Foutbericht 'kan niet om referenties te valideren. Een onverwachte fout is opgetreden." wordt geretourneerd.
* Staging-server verwijderen, schakelt Wachtwoordsynchronisatie in Azure AD-tenant en zorgt ervoor dat de synchronisatie van wachtwoord mislukt met de actieve server.
* Wachtwoordsynchronisatie is mislukt in zeldzame gevallen wanneer er geen wachtwoord-hash die zijn opgeslagen op de gebruiker.
* Wanneer Azure AD Connect-server van de faseringsmodus is ingeschakeld, wordt het terugschrijven van wachtwoorden niet tijdelijk uitgeschakeld.
* Azure AD Connect-wizard weergegeven niet. de werkelijke Wachtwoordsynchronisatie en wachtwoord terugschrijven configuratie als server is in de faseringsmodus bevindt. Deze worden altijd weergegeven ze als uitgeschakeld.
* Configuratiewijzigingen voor de synchronisatie van wachtwoord en het terugschrijven van wachtwoorden zijn niet permanent opgeslagen door Azure AD Connect-wizard wanneer-server zich in de faseringsmodus bevindt.

**Verbeteringen**

* De cmdlet Start-ADSyncSyncCycle om aan te geven of het een nieuwe synchronisatiecyclus is gestart of niet is bijgewerkt.
* De cmdlet Stop-ADSyncSyncCycle moet worden beëindigd synchronisatiecyclus en bewerking, die momenteel uitgevoerd worden toegevoegd.
* Bijgewerkt in de cmdlet Stop-ADSyncScheduler moet worden beëindigd synchronisatiecyclus en bewerking, die momenteel uitgevoerd worden.
* Bij het configureren van [mapextensies](how-to-connect-sync-feature-directory-extensions.md) in Azure AD Connect-wizard kan nu de Azure AD-kenmerk van het type 'Tekenreeks Teletex' worden geselecteerd.

## <a name="111890"></a>1.1.189.0
Uitgebracht: Juni 2016

**Opgeloste problemen en verbeteringen:**

* Azure AD Connect kan nu worden geïnstalleerd op een compatibele FIPS-server.
  * Zie voor Wachtwoordsynchronisatie, [wachtwoordhashsynchronisatie en FIPS](how-to-connect-password-hash-synchronization.md#password-hash-synchronization-and-fips).
* Een probleem opgelost waarbij een NetBIOS-naam kan niet worden omgezet naar de FQDN-naam in de Active Directory-Connector.

## <a name="111800"></a>1.1.180.0
Uitgebracht: Mei 2016

**Nieuwe functies**

* Geeft een waarschuwing en helpt u bij het controleren van domeinen, als u deze niet hebt gedaan voordat Azure AD Connect wordt uitgevoerd.
* Ondersteuning toegevoegd voor [Microsoft Cloud Duitsland](reference-connect-instances.md#microsoft-cloud-germany).
* Ondersteuning toegevoegd voor de meest recente [Microsoft Azure Government-cloud](reference-connect-instances.md#microsoft-azure-government-cloud) infrastructuur met nieuwe URL-vereisten.

**Opgeloste problemen en verbeteringen:**

* Toegevoegd aan de synchronisatie regel Editor om gemakkelijk te vinden van synchronisatieregels filteren.
* Verbeterde prestaties bij het verwijderen van een connectorgebied.
* Een probleem is opgelost wanneer hetzelfde object is verwijderd en toegevoegd aan hetzelfde uitvoeren (met de naam verwijderen/toevoegen).
* Een uitgeschakelde synchronisatie regel niet meer opnieuw kunt toegevoegde objecten en kenmerken van de upgrade of Active directory-schema vernieuwen.

## <a name="111300"></a>1.1.130.0
Uitgebracht: April 2016

**Nieuwe functies**

* Ondersteuning toegevoegd voor kenmerken met meerdere waarden [mapextensies](how-to-connect-sync-feature-directory-extensions.md).
* Ondersteuning toegevoegd voor meer configuratie variaties voor [Automatische upgrade](how-to-connect-install-automatic-upgrade.md) worden overwogen in aanmerking voor upgrade.
* Sommige cmdlets voor toegevoegd [aangepaste scheduler](how-to-connect-sync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Uitgebracht: Maart 2016

**Opgeloste problemen:**

* Gemaakt wordt of Express-installatie kan niet worden gebruikt op Windows Server 2008 (pre-R2), omdat wachtwoord synchroniseren niet ondersteund op dit besturingssysteem.
* Upgraden van DirSync met de configuratie van een aangepast filter werkt niet zoals verwacht.
* Bij een upgrade naar een nieuwere versie en er zijn geen wijzigingen aan de configuratie, een volledige import/synchronisatie moet niet worden gepland.

## <a name="111100"></a>1.1.110.0
Uitgebracht: Februari 2016

**Opgeloste problemen:**

* Upgrade van eerdere versies werkt niet als de installatie niet in de standaardmap van C:\Program Files is.
* Als u installeert en schakel **Start het synchronisatieproces** aan het einde van de installatiewizard uitvoeren van de installatiewizard van een tweede keer niet mogelijk de scheduler.
* De scheduler werkt niet zoals verwacht op servers waarop de US-en datum/tijd-indeling niet wordt gebruikt. Er worden ook geblokkeerd `Get-ADSyncScheduler` om terug te keren van de juiste tijden.
* Als u een eerdere versie van Azure AD Connect met AD FS hebt geïnstalleerd als de optie aanmelden en upgrade, niet kan u de installatiewizard opnieuw uitvoeren.

## <a name="111050"></a>1.1.105.0
Uitgebracht: Februari 2016

**Nieuwe functies**

* [Automatische upgrade](how-to-connect-install-automatic-upgrade.md) functie voor snelle instellingen voor klanten.
* Ondersteuning voor de globale beheerder met behulp van Azure multi-factor Authentication en Privileged Identity Management in de installatiewizard.
  * U wilt toestaan dat uw proxy om toe te staan ook verkeer naar https://secure.aadcdn.microsoftonline-p.com als u multi-factor Authentication.
  * U moet toevoegen https://secure.aadcdn.microsoftonline-p.com aan uw lijst met vertrouwde websites voor meervoudige verificatie correct werkt.
* Het wijzigen van de aanmeldingsmethode van de gebruiker na de eerste installatie toegestaan.
* Toestaan dat [domein en OE filteren](how-to-connect-install-custom.md#domain-and-ou-filtering) in de installatiewizard. Hierdoor kunnen ook verbinding maken met forests waarin niet alle domeinen beschikbaar zijn.
* [Scheduler](how-to-connect-sync-feature-scheduler.md) is ingebouwd in de synchronisatie-engine.

**Functies van preview gepromoveerd naar algemene beschikbaarheid:**

* [Apparaat terugschrijven](how-to-connect-device-writeback.md).
* [Mapextensies](how-to-connect-sync-feature-directory-extensions.md).

**Nieuwe preview-functies:**

* De nieuwe standaard cyclus interval is 30 minuten gesynchroniseerd. Gebruikt om te worden drie uur voor alle eerdere versies. Voegt ondersteuning om te wijzigen de [scheduler](how-to-connect-sync-feature-scheduler.md) gedrag.

**Opgeloste problemen:**

* Controleer of DNS-domeinen pagina herkend niet altijd de domeinen.
* Als u wordt gevraagd om beheerdersreferenties domein bij het configureren van AD FS.
* De on-premises AD-accounts worden niet herkend door de installatiewizard als zich bevindt in een domein met een andere DNS-structuur dan het hoofddomein.

## <a name="1091310"></a>1.0.9131.0
Uitgebracht: December 2015

**Opgeloste problemen:**

* Wachtwoordsynchronisatie werkt mogelijk niet wanneer u wachtwoorden in Active Directory Domain Services (AD DS), maar werkt wijzigen wanneer u een wachtwoord instelt.
* Wanneer u een proxyserver hebt, Azure AD-verificatie kan mislukken tijdens de installatie, of als een upgrade op de configuratiepagina is geannuleerd.
* Bijwerken van een vorige versie van Azure AD Connect met een volledige SQL Server-exemplaar als u niet een SQL Server-systeembeheerder (SA) is mislukt.
* Bijwerken van een vorige versie van Azure AD Connect met een externe SQL Server, ziet u de fout 'Kan geen toegang tot de ADSync SQL-database'.

## <a name="1091250"></a>1.0.9125.0
Uitgebracht: November 2015

**Nieuwe functies**

* Kan configuratie van AD FS en Azure AD-vertrouwensrelatie.
* Kan het Active Directory-schema vernieuwen en opnieuw genereren van synchronisatieregels.
* Een synchronisatieregel kan worden uitgeschakeld.
* Kan 'AuthoritativeNull' definiëren als een nieuwe letterlijke waarde in een synchronisatieregel voor.

**Nieuwe preview-functies:**

* [Azure AD Connect Health voor synchroniseren](how-to-connect-health-sync.md).
* Ondersteuning voor [Azure AD Domain Services](../user-help/active-directory-passwords-update-your-own-password.md) Wachtwoordsynchronisatie.

**Nieuwe ondersteunde scenario:**

* Biedt ondersteuning voor meerdere on-premises Exchange-organisaties. Zie voor meer informatie, [hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx).

**Opgeloste problemen:**

* Problemen met synchronisatie van wachtwoord:
  * Een object van buiten het bereik wordt verplaatst naar binnen de regeling vallen geen wachtwoord voor de gesynchroniseerd. Dit omvat zowel organisatie-eenheid en kenmerkfilters.
  * Een volledige Wachtwoordsynchronisatie is niet vereist als u een nieuwe organisatie-eenheid om op te nemen synchroon te selecteren.
  * Wanneer een uitgeschakelde gebruiker is ingeschakeld wordt het wachtwoord niet synchroniseren.
  * De wachtrij met wachtwoorden opnieuw proberen is oneindig en de eerdere limiet van 5000 objecten om te worden buiten gebruik gesteld is verwijderd.
* Kan geen verbinding maken met Active Directory met Windows Server 2016 forest-functionaliteitsniveau.
* Kan de groep die wordt gebruikt voor het filteren van de groep na de eerste installatie wijzigen.
* Maakt een nieuw gebruikersprofiel niet langer op de Azure AD Connect-server voor elke gebruiker een wachtwoordwijziging doen met wachtwoord terugschrijven is ingeschakeld.
* Niet meer gebruiken Lange Integer waarden synchroon regels bereiken.
* Het selectievakje 'apparaat terugschrijven' blijft uitgeschakeld als er domeincontrollers niet bereikbaar zijn.

## <a name="1086670"></a>1.0.8667.0
Uitgebracht: Augustus 2015

**Nieuwe functies**

* De Azure AD Connect-installatiewizard is nu op alle Windows Server-talen vertaald.
* Er is ondersteuning toegevoegd voor het account ontgrendelen bij het gebruik van Azure AD-wachtwoordbeheer.

**Opgeloste problemen:**

* Azure AD Connect-installatiewizard loopt vast als een andere gebruiker heeft nog steeds installatie in plaats van de persoon die de installatie van de eerste keer wordt gestart.
* Als een eerdere installatie van Azure AD Connect niet correct verwijderen van Azure AD Connect-synchronisatie, is het niet mogelijk om opnieuw te installeren.
* Kan Azure AD Connect met Express-installatie als de gebruiker zich niet in het hoofddomein van het forest of als een niet-Engelse versie van Active Directory wordt gebruikt niet installeren.
* Als de FQDN-naam van het Active Directory-gebruikersaccount kan niet worden omgezet, wordt een foutbericht misleidend 'Mislukt om door te voeren van het schema' weergegeven.
* Als het account dat wordt gebruikt op de Active Directory-Connector is gewijzigd buiten de wizard, mislukt de wizard op de volgende wordt uitgevoerd.
* Soms kan Azure AD Connect installeren op een domeincontroller.
* Kan geen in- en uitschakelen van 'Staging mode' als extensiekenmerken zijn toegevoegd.
* Wachtwoord terugschrijven is mislukt in sommige configuraties vanwege een onjuist wachtwoord op de Active Directory-Connector.
* DirSync kan niet worden bijgewerkt als een DN-naam (Distinguished Name) wordt gebruikt bij het filteren van het kenmerk.
* Buitensporig CPU-gebruik bij het gebruik van wachtwoord opnieuw instellen.

**Verwijderde preview-functies:**

* De preview-functie [Write-back van gebruiker](how-to-connect-preview.md#user-writeback) tijdelijk op basis van feedback van onze klanten Preview-versie is verwijderd. Dit wordt later opnieuw worden toegevoegd nadat de opgegeven feedback is verholpen.

## <a name="1086410"></a>1.0.8641.0
Uitgebracht: Juni 2015

**Initiële versie van Azure AD Connect.**

Gewijzigde naam van Azure AD Sync naar Azure AD Connect.

**Nieuwe functies**

* [Snelle instellingen](how-to-connect-install-express.md) installatie
* Kan [AD FS configureren](how-to-connect-install-custom.md#configuring-federation-with-ad-fs)
* Kan [upgraden van DirSync](how-to-dirsync-upgrade-get-started.md)
* [Onopzettelijke verwijderingen voorkomen](how-to-connect-sync-feature-prevent-accidental-deletes.md)
* Geïntroduceerd [faseringsmodus](how-to-connect-sync-staging-server.md)

**Nieuwe preview-functies:**

* [Write-back van gebruiker](how-to-connect-preview.md#user-writeback)
* [Write-back van groep](how-to-connect-preview.md#group-writeback)
* [Write-back van apparaat](how-to-connect-device-writeback.md)
* [Uitbreidingen van de directory](how-to-connect-preview.md)

## <a name="104940501"></a>1.0.494.0501
Uitgebracht: Mei 2015

**Nieuwe vereiste:**

* Azure AD Sync is nu vereist versie van .NET Framework 4.5.1 om te worden geïnstalleerd.

**Opgeloste problemen:**

* Het terugschrijven van wachtwoorden van Azure AD is mislukt met een Azure Service Bus-verbindingsfout.

## <a name="104910413"></a>1.0.491.0413
Uitgebracht: April 2015

**Opgeloste problemen en verbeteringen:**

* De Active Directory-Connector worden verwijderd niet correct verwerkt als de Prullenbak is ingeschakeld en er meerdere domeinen in het forest zijn.
* De prestaties van bewerkingen is, verbeterd voor de Azure Active Directory-Connector.
* Wanneer een groep het lidmaatschap limiet heeft overschreden (standaard de limiet is ingesteld op 50.000 objecten), de groep in Azure Active Directory is verwijderd. Dankzij het nieuwe gedrag, de groep is niet verwijderd, wordt een fout gegenereerd en nieuwe wijzigingen in het lidmaatschap worden niet geëxporteerd.
* Een nieuw object kan niet worden ingericht als een gefaseerde verwijderen met de dezelfde DN-naam al aanwezig in het connectorgebied is.
* Sommige objecten zijn gemarkeerd voor synchronisatie tijdens een Deltasynchronisatie, zelfs als er geen wijziging tijdelijk worden opgeslagen op het object is.
* Een Wachtwoordsynchronisatie forceren, verwijdert ook de voorkeur DC-lijst.
* CSExportAnalyzer heeft problemen met de statussen van sommige objecten.

**Nieuwe functies**

* Een join kan nu verbinding maken met het objecttype 'ANY' in de MV.

## <a name="104850222"></a>1.0.485.0222
Uitgebracht: Februari 2015

**Verbeteringen**

* Importeren van verbeterde prestaties.

**Opgeloste problemen:**

* Wachtwoordsynchronisatie zich houdt aan de cloudFiltered-kenmerk dat wordt gebruikt door het filteren van het kenmerk. Gefilterde objecten zijn niet meer binnen het bereik van Wachtwoordsynchronisatie.
* In zeldzame gevallen de topologie waar veel domeincontrollers zijn, werkt Wachtwoordsynchronisatie niet.
* 'Gestopt-server' bij het importeren vanuit de Azure AD-Connector nadat Apparaatbeheer in Azure AD/Intune is ingeschakeld.
* Lid worden van afwijkende beveiligings-Principals (FSP's) van meerdere domeinen in hetzelfde forest, veroorzaakt een fout die niet-eenduidige join.

## <a name="104751202"></a>1.0.475.1202
Uitgebracht: December 2014

**Nieuwe functies**

* Wachtwoordsynchronisatie met filteren op basis van een kenmerk wordt nu ondersteund. Zie voor meer informatie, [Wachtwoordsynchronisatie met filteren](how-to-connect-sync-configure-filtering.md).
* Het kenmerk ms-DS-ExternalDirectoryObjectID teruggeschreven naar Active Directory. Deze functie wordt ondersteuning toegevoegd voor Office 365-toepassingen. Het OAuth2 gebruikt voor toegang tot de postvakken in een hybride implementatie voor Exchange Online en On-Premises.

**Vaste problemen met de upgrade:**

* Er is een nieuwere versie van de aanmeldhulp beschikbaar op de server.
* Een aangepaste installatiepad is gebruikt voor het installeren van Azure AD Sync.
* Een ongeldige aangepaste join criterium Hiermee blokkeert u de upgrade.

**Andere oplossingen:**

* De sjablonen vastgesteld voor Office Professional Plus.
* Vaste installatieproblemen veroorzaakt door gebruikersnamen die met een streepje beginnen.
* Vast dat de instelling sourceAnchor verloren gaat bij het uitvoeren van de installatiewizard van een tweede keer.
* Vaste ETW-tracering voor Wachtwoordsynchronisatie.

## <a name="104701023"></a>1.0.470.1023
Uitgebracht: Oktober 2014

**Nieuwe functies**

* Wachtwoordsynchronisatie vanaf meerdere on-premises Active Directory naar Azure AD.
* Gelokaliseerde gebruikersinterface voor installatie op alle Windows Server-talen.

**Een upgrade uitvoert van AADSync 1.0 algemeen beschikbaar**

Als u al Azure AD Sync geïnstalleerd hebt, is er een extra stap die u uitvoeren moet als u een van de out-of-box-synchronisatieregels zijn gewijzigd. Nadat u een upgrade hebt uitgevoerd naar de 1.0.470.1023 vrijgeven, de synchronisatie regels die u hebt gewijzigd worden gedupliceerd. Voor elke gewijzigde synchronisatieregel, het volgende doen:

1. Ga naar de synchronisatieregel die u hebt gewijzigd en noteer de wijzigingen.
1. Verwijder de synchronisatieregel.
1. Ga naar de nieuwe regel voor synchroniseren die is gemaakt door Azure AD Sync en pas vervolgens de wijzigingen opnieuw toe.

**Machtigingen voor het Active Directory-account**

De Active Directory-account moet extra machtigingen om het lezen van de wachtwoord-hashes van Active Directory te kunnen worden verleend. De machtigingen te verlenen, worden met de naam "Directory-wijzigingen repliceren" en "Repliceren Directory alle gewijzigd." Beide machtigingen zijn vereist om te kunnen lezen van de wachtwoord-hashes.

## <a name="104190911"></a>1.0.419.0911
Uitgebracht: September 2014

**Initiële versie van Azure AD Sync.**

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).

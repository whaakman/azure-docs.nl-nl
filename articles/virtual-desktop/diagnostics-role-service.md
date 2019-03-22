---
title: Identificeren van problemen met de diagnostics-functie (preview) - Azure
description: Beschrijft de functie van de diagnostische gegevens over virtuele Windows-bureaublad en het gebruik ervan.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 03/21/2019
ms.author: helohr
ms.openlocfilehash: f44f4d632fa1fc607f3933be8e15eb939e20a8ae
ms.sourcegitcommit: 90dcc3d427af1264d6ac2b9bde6cdad364ceefcc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/21/2019
ms.locfileid: "58318612"
---
# <a name="identify-issues-with-the-diagnostics-feature-preview"></a>Identificeren van problemen met de functie voor diagnostische gegevens (Preview)

Virtuele Windows-Desktop (preview) biedt een functie voor diagnostische gegevens (preview) waarmee de beheerder om te identificeren van problemen melden via één interface. De rollen van virtuele Windows-bureaublad Meld een diagnostische activiteit wanneer een gebruiker interactie met het systeem heeft. Elk logboek bevat relevante informatie zoals de virtuele Windows-bureaublad rollen die betrokken zijn bij de transactie, foutberichten, tenantgegevens en gebruikersgegevens. Diagnostische activiteiten zijn gemaakt door zowel eindgebruikers als administratieve taken en kunnen worden onderverdeeld in drie belangrijkste buckets:

* Feed abonnement activiteiten: de eindgebruiker deze activiteiten wordt geactiveerd wanneer ze proberen verbinding maken met hun feed via Microsoft Extern bureaublad-toepassingen.
* Activiteiten van de verbinding: de eindgebruiker deze activiteiten wordt geactiveerd wanneer ze proberen verbinding maken met een desktop of RemoteApp via Microsoft Extern bureaublad-toepassingen.
* Beheeractiviteiten: activeert de beheerder deze activiteiten wanneer ze op het systeem, zoals het maken van pools van de host, gebruikers toewijzen aan app-groepen en het maken van roltoewijzingen bewerkingen uitvoeren.
  
Verbindingen die niet virtuele Windows-bureaublad bereiken weergegeven niet in de resultaten van de diagnostische gegevens omdat de diagnostics-functieservice zelf deel uitmaakt van een virtuele Windows-bureaublad. Problemen met virtuele Windows-bureaublad-verbindingen kunnen gebeuren wanneer de eindgebruiker problemen met de netwerkverbinding ondervindt.

Aan de slag [downloaden en importeren van de Windows virtuele bureaublad PowerShell-module](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) te gebruiken in uw PowerShell-sessie als u dat nog niet gedaan hebt.

## <a name="diagnose-issues-with-powershell"></a>Problemen met PowerShell

Windows virtuele bureaublad diagnostische gegevens slechts één PowerShell-cmdlet gebruikt, maar bevat veel optionele parameters Verfijn help en problemen isoleren. De volgende secties worden de cmdlets die u kunt uitvoeren voor het vaststellen van problemen. De meeste filters kunnen samen worden toegepast. Waarden in de vierkante haken, zoals `<tenantName>`, moet worden vervangen door de waarden die betrekking hebben op uw situatie.

### <a name="retrieve-diagnostic-activities-in-your-tenant"></a>Diagnostische activiteiten in uw tenant ophalen

U kunt diagnostische activiteiten ophalen door in te voeren de **Get-RdsDiagnosticsActivities** cmdlet. Het volgende voorbeeld-cmdlet retourneert een lijst met diagnostische activiteiten, gesorteerd van meest naar minst recente.

```powershell
Get-RdsDiagnosticsActivities -TenantName <tenantName>
```

Net als andere Windows virtuele bureaublad PowerShell-cmdlets, moet u de **- Tenantnaam** parameter opgeven voor de naam van de tenant die u wilt gebruiken voor uw query. Naam van de tenant is van toepassing voor bijna alle diagnostische activiteit query's.

### <a name="retrieve-detailed-diagnostic-activities"></a>Gedetailleerde diagnostische activiteiten ophalen

De **-gedetailleerde** parameter bevat aanvullende informatie voor elke diagnostische activiteit die wordt geretourneerd. De indeling voor elke activiteit is afhankelijk van het activiteitstype. De **-gedetailleerde** parameter kan worden toegevoegd aan een **Get-RdsDiagnosticsActivities** opvragen, zoals weergegeven in het volgende voorbeeld.

```powershell
Get-RdsDiagnosticsActivities -TenantName <tenantName> -Detailed
```

### <a name="retrieve-a-specific-diagnostic-activity-by-activity-id"></a>Een specifieke diagnostische activiteit door activiteit-ID ophalen

De **- ActivityId** parameter retourneert een specifieke diagnostische activiteit als deze bestaat, zoals wordt weergegeven in het volgende voorbeeld-cmdlet.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityId <ActivityIdGuid>
```

### <a name="filter-diagnostic-activities-by-user"></a>Filter diagnostische activiteiten door gebruiker

De **- UserName** parameter retourneert een lijst van diagnostische activiteiten die zijn gestart door de opgegeven gebruiker, zoals wordt weergegeven in het volgende voorbeeld-cmdlet.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -UserName <UserUPN>
```

De **- UserName** parameter kan ook worden gecombineerd met andere optionele filterparameters.

### <a name="filter-diagnostic-activities-by-time"></a>Diagnostische activiteiten filteren op tijd

U kunt de activiteitenlijst met geretourneerde diagnostische met filteren de **- StartTime** en **- EndTime** parameters. De **- StartTime** parameter retourneert een activiteitenlijst met diagnostische vanaf een specifieke datum, zoals wordt weergegeven in het volgende voorbeeld.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -StartTime "08/01/2018"
```

De **- EndTime** parameter kan worden toegevoegd aan een cmdlet met de **- StartTime** parameter opgeven voor een bepaalde periode wilt u resultaten voor ontvangen. Het volgende voorbeeld-cmdlet retourneert een lijst van diagnostische activiteiten tussen 1 augustus en 10 augustus.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -StartTime "08/01/2018" -EndTime "08/10/2018"
```

De **- StartTime** en **- EndTime** parameters kunnen ook worden gecombineerd met andere optionele filterparameters.

### <a name="filter-diagnostic-activities-by-activity-type"></a>Filter diagnostische activiteiten per activiteitstype

U kunt ook filteren diagnostische activiteiten op basis van het activiteitstype met de **- ActivityType** parameter. De volgende cmdlet retourneert een lijst met verbindingen voor eindgebruikers:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityType Connection
```

De volgende cmdlet retourneert een lijst van beheertaken voor beheerder:

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -ActivityType Management
```

De **Get-RdsDiagnosticActivities** cmdlet ondersteunt momenteel geen Feed als de ActivityType op te geven.

### <a name="filter-diagnostic-activities-by-outcome"></a>Diagnostische activiteiten filteren op resultaat

U kunt de activiteitenlijst met geretourneerde diagnostische filteren op resultaat met de **-resultaat** parameter. Het volgende voorbeeld-cmdlet retourneert een lijst van geslaagde diagnostische activiteiten.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Outcome Success
```

Het volgende voorbeeld-cmdlet retourneert een lijst met mislukte diagnostische activiteiten.

```powershell
Get-RdsDiagnosticActivities -TenantName <tenantName> -Outcome Failure
```

De **-resultaat** parameter kan ook worden gecombineerd met andere optionele filterparameters.

## <a name="common-error-scenarios"></a>Algemene fout-scenario 's

Fout bij scenario's zijn onderverdeeld in interne naar de service en externe virtuele Windows-bureaublad.

* Intern probleem: Hiermee geeft u de scenario's die niet kunnen worden verholpen door de tenantbeheerder en moeten worden opgelost als Ondersteuningsprobleem. Wanneer een ticket verhogen bieden de activiteits-ID, de tenantnaam en de geschatte tijdsduur het probleem is opgetreden.
* Probleem met de externe: betrekking op scenario's die kunnen worden opgelost door de systeembeheerder. Dit zijn externe met virtuele Windows-bureaublad.

De volgende tabel geeft een lijst met veelvoorkomende fouten in die uw beheerders tegenkomen.

>[!NOTE]
>Deze Preview-versie niet een volledige classificatie van fouten bevat en wordt regelmatig bijgewerkt. Als u wilt zorgen dat u hebt de meest actuele informatie, vergeet weer in dit artikel ten minste één keer per maand.

### <a name="external-management-error-codes"></a>Foutcodes voor extern beheer

|Numerieke code|Foutcode|Voorgestelde oplossing|
|---|---|---|
|3|UnauthorizedAccess|De gebruiker die probeert uit te voeren van de administratieve PowerShell-cmdlet beschikt niet over machtigingen om dit te doen of hun gebruikersnaam een typefout gemaakt.|
|1000|TenantNotFound|De naam van de tenant die u hebt ingevoerd komt niet overeen met alle bestaande tenants. Controleer de naam van de tenant voor typfouten en probeer het opnieuw.|
|1006|TenantCannotBeRemovedHasSessionHostPools|U kunt een tenant niet verwijderen omdat lang deze objecten bevat. Verwijder eerst de session host-adresgroepen en probeer het opnieuw.|
|2000|HostPoolNotFound|De hostnaam van de groep van toepassingen die u hebt ingevoerd komt niet overeen met een bestaande host-adresgroepen. Controleer de hostnaam van de groep van toepassingen voor typfouten en probeer het opnieuw.|
|2005|HostPoolCannotBeRemovedHasApplicationGroups|U kunt een host-groep niet verwijderen zolang deze objecten bevat. Verwijder eerst alle app-groepen in de groep host.|
|2004|HostPoolCannotBeRemovedHasSessionHosts|Verwijder alle sessies hosts de session host-groep eerst voordat verwijderen.|
|5001|SessionHostNotFound|De host van de sessie die u hebt opgevraagd, is mogelijk offline. Controleer de status van de host-pool.|
|5008|SessionHostUserSessionsExist |U moet alle gebruikers op de host van de sessie afmelden voordat uw beoogde management-activiteit wordt uitgevoerd.|
|6000|AppGroupNotFound|De naam van de app die u hebt ingevoerd komt niet overeen met een bestaande app-groepen. Controleer de naam van de app voor typfouten en probeer het opnieuw.|
|6022|RemoteAppNotFound|De RemoteApp-naam die u hebt ingevoerd komt niet overeen met alle RemoteApps. RemoteApp-naam voor typfouten van de beoordeling en probeer het opnieuw.|
|6010|PublishedItemsExist|De naam van de resource die u probeert te publiceren is hetzelfde als een resource die al bestaat. Wijzig de resourcenaam van de en probeer het opnieuw.|
|7002|NameNotValidWhiteSpace|Gebruik geen witruimte in de naam.|
|8000|InvalidAuthorizationRoleScope|De naam van de rol die u hebt ingevoerd komt niet overeen met de rolnamen van alle bestaande. Controleer de naam van de rol voor typfouten en probeer het opnieuw. |
|8001|UserNotFound |De naam van de gebruiker die u hebt ingevoerd komt niet overeen met de gebruikersnamen van een bestaande. Controleer de naam voor typfouten en probeer het opnieuw.|
|8005|UserNotFoundInAAD |De naam van de gebruiker die u hebt ingevoerd komt niet overeen met de gebruikersnamen van een bestaande. Controleer de naam voor typfouten en probeer het opnieuw.|
|8008|TenantConsentRequired|Volg de instructies [hier](tenant-setup-azure-active-directory.md#grant-azure-active-directory-permissions-to-the-windows-virtual-desktop-service) voor toestemming van uw tenant.|

### <a name="external-connection-error-codes"></a>Foutcodes voor externe verbinding

|Numerieke code|Foutcode|Voorgestelde oplossing|
|---|---|---|
|-2147467259|ConnectionFailedAdTrustedRelationshipFailure|Host van de sessie is niet correct toegevoegd aan de Active Directory.|
|-2146233088|ConnectionFailedUserHasValidSessionButRdshIsUnhealthy|De verbindingen is mislukt omdat de host niet beschikbaar is. Controleer de status van de sessiehost.|
|-2146233088|ConnectionFailedClientDisconnect|Als u deze fout vaak ziet, zorg er dan voor dat de computer van de gebruiker is verbonden met het netwerk.|
|-2146233088|ConnectionFailedNoHealthyRdshAvailable|De sessie die de host de gebruiker heeft geprobeerd verbinding maken met is niet in orde. Fouten opsporen in de virtuele machine.|
|-2146233088|ConnectionFailedUserNotAuthorized|De gebruiker is niet gemachtigd voor toegang tot de gepubliceerde app of het bureaublad. Nadat de beheerder verwijderd gepubliceerde bronnen mogelijk de fout weergegeven. Vraag de gebruiker om te vernieuwen van de feed in de extern bureaublad-toepassing.|
|2|FileNotFound|De toepassing die de gebruiker toegang probeert te krijgen is niet juist geïnstalleerd of ingesteld op een onjuist pad.|
|3|InvalidCredentials|De gebruikersnaam of het wachtwoord van de gebruiker heeft ingevoerd komt niet overeen met alle bestaande gebruikersnamen of wachtwoorden. Controleer de referenties voor typfouten en probeer het opnieuw.|
|8|ConnectionBroken|De verbinding tussen Client en de Gateway of de Server verwijderd. Er is geen actie nodig, tenzij het onverwachts gebeurt.|
|14|UnexpectedNetworkDisconnect|De verbinding met het netwerk is verwijderd. Vraag de gebruiker opnieuw verbinding te maken.|
|24|ReverseConnectFailed|De virtuele machine van de host heeft geen directe verbinding met extern bureaublad-Gateway. Zorg ervoor dat het Gateway-IP-adres kan worden omgezet.|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over rollen binnen virtuele Windows-bureaublad, [virtueel bureaublad van Windows-omgeving](environment-setup.md).

Zie voor een lijst met beschikbare PowerShell-cmdlets voor virtuele Windows-bureaublad de [PowerShell-referentie](/powershell/windows-virtual-desktop/overview).
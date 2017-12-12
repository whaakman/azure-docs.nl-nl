---
title: Azure AD Connect installeert met behulp van een bestaande database van ADSync | Microsoft Docs
description: Dit onderwerp wordt beschreven hoe u een bestaande database van ADSync gebruikt.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.reviewer: cychua
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/30/2017
ms.author: billmath
ms.openlocfilehash: d65e12350e6302b0f95091f48f71cdc7d4610e2c
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="install-azure-ad-connect-using-an-existing-adsync-database"></a>Azure AD Connect met een bestaande database van ADSync installeren
Azure AD Connect vereist een SQL Server-database voor het opslaan van gegevens. De standaard die SQL Server 2012 Express LocalDB met Azure AD Connect geïnstalleerd of uw eigen volledige versie van SQL gebruiken. Voorheen wanneer u Azure AD Connect geïnstalleerd, is een nieuwe database met de naam ADSync altijd gemaakt. U hebt de optie voor het Azure AD Connect installeert door deze aan te wijzen aan een bestaande database van ADSync met Azure AD Connect versie 1.1.613.0 (of na).

## <a name="benefits-of-using-an-existing-adsync-database"></a>Voordelen van het gebruik van een bestaande database van ADSync
Door te verwijzen naar een bestaande database van ADSync:

- Met uitzondering van referenties informatie is synchronisatieconfiguratie opgeslagen in de ADSync database (inclusief aangepaste synchronisatieregels, connectors filteren en configuratie van de optionele functies) automatisch hersteld en gebruikt tijdens de installatie . Referenties die door Azure AD Connect worden gebruikt om wijzigingen te synchroniseren met on-premises AD en Azure AD zijn versleuteld en kan alleen worden geopend door de vorige Azure AD Connect-server.
- De id-gegevens (gekoppeld aan connectorspaces en metaverse) en synchronisatie cookies die zijn opgeslagen in de database van ADSync ook worden hersteld. De nieuw geïnstalleerde Azure AD Connect dat wordt gesynchroniseerd vanaf waar de vorige Azure AD Connect-server is gestopt, in plaats van de noodzaak om uit te voeren van een volledige synchronisatie van server kunt blijven.

## <a name="scenarios-where-using-an-existing-adsync-database-is-beneficial"></a>Scenario's waarin een bestaande database van ADSync nuttige
Deze voordelen zijn nuttig in de volgende scenario's:


- U hebt een bestaande implementatie van Azure AD Connect. De bestaande Azure AD Connect-server niet langer werkt, maar de SQL-server met de database van ADSync nog werkt. U kunt een nieuwe Azure AD Connect-server installeren en wijs de bestaande database van ADSync. 
- U hebt een bestaande implementatie van Azure AD Connect. De SQL-server met de database van ADSync werkt niet meer. Maar hebt u een recente back-up van de database. U kunt de database van ADSync eerst herstellen naar een nieuwe SQL-server. Waarna, kunt u een nieuwe Azure AD Connect-server installeren en wijs de teruggezette database van ADSync.
- U hebt een bestaande Azure AD Connect-implementatie die van LocalDB gebruikmaakt. Als gevolg van het maximum van 10 GB die zijn opgelegd door LocalDB, wilt u migreren naar een volledige SQL. U kunt back-up van de database van ADSync van LocalDB en herstellen naar een SQL server. Waarna u kunt een nieuwe Azure AD Connect-server opnieuw en wijs de teruggezette database van ADSync.
- U probeert een testserver instellen en wil ervoor zorgen dat de configuratie ervan overeenkomt met die van de huidige actieve server. U kunt back-up van de database van ADSync en herstellen naar een andere SQL-server. Waarna u kunt een nieuwe Azure AD Connect-server opnieuw en wijs de teruggezette database van ADSync.

## <a name="prerequisite-information"></a>Vereiste informatie

Voordat u doorgaat, let er van belangrijke opmerkingen te laten op:

- Zorg ervoor dat aan de vereisten voor Azure AD Connect op Hardware en vereisten, en -account en machtigingen die vereist zijn voor het installeren van Azure AD Connect installeert bekijken. De machtigingen die vereist zijn voor het installeren van Azure AD Connect met modus 'bestaande database gebruiken' is hetzelfde als 'aangepaste' installatie.
- Database, dat is implementatie van Azure AD Connect op basis van een bestaande ADSync wordt alleen ondersteund met de volledige SQL. Dit wordt niet ondersteund met SQL Express LocalDB. Hebt u een bestaande database van ADSync in LocalDB die u wilt gebruiken, moet u back-up van de database van ADSync (LocalDB) en deze terugzetten naar volledige SQL. Waarna u Azure AD Connect op de herstelde database met deze methode kunt implementeren.
- De versie van de Azure AD Connect gebruiken voor de installatie moet voldoen aan de volgende criteria:
    - 1.1.613.0 of hoger, en
    - Dezelfde of hoger is dan de versie van de Azure AD Connect met de database van ADSync het laatst is gebruikt. Als de Azure AD Connect-versie die wordt gebruikt voor de installatie hoger dan de versie met de database van ADSync het laatst is gebruikt is, kan een volledige synchronisatie vereist zijn.  Volledige synchronisatie is vereist als er wijzigingen in de regel schema of de synchronisatie tussen de twee versies. 
- De ADSync database die wordt gebruikt, moet een relatief recente synchronisatiestatus van bevatten. De laatste synchronisatieactiviteit met de bestaande database van ADSync moet binnen de afgelopen drie weken.
- Bij het installeren van Azure AD Connect met de methode 'bestaande database gebruiken', is aanmelden methode geconfigureerd op de vorige Azure AD Connect-server niet behouden. U kunt de aanmeldingsmethode bovendien niet configureren tijdens de installatie. U kunt alleen aanmeldingsmethode configureren nadat de installatie is voltooid.
- U kunt geen meerdere Azure AD Connect-servers dezelfde database ADSync. De methode 'bestaande database gebruiken' kunt u een bestaande database ADSync met een nieuwe Azure AD Connect-server. Het ondersteunt niet delen.

## <a name="steps-to-install-azure-ad-connect-with-use-existing-database-mode"></a>Stappen voor het Azure AD Connect installeren met de modus "bestaande database gebruiken"
1.  Azure AD Connect-installatieprogramma (AzureADConnect.MSI) naar de Windows-server downloaden. Dubbelklik op de Azure AD Connect-installatieprogramma voor het starten van Azure AD Connect installeert.
2.  Zodra de MSI-installatie is voltooid, wordt de Azure AD Connect-wizard gestart met het instellen van de Express-modus. Het scherm sluiten door te klikken op het pictogram afsluiten.
![Welkom](media/active-directory-aadconnect-existing-database/db1.png)
3.  Start een nieuwe opdrachtprompt of een PowerShell-sessie. Navigeer naar de map <drive>\program files\Microsoft Azure AD Connect. Voer de opdracht.\AzureADConnect.exe /useexistingdatabase voor het starten van de Azure AD Connect-wizard in de setup-modus 'Bestaande database gebruiken'.
![PowerShell](media/active-directory-aadconnect-existing-database/db2.png)
4.  Wordt met het welkomstscherm van Azure AD Connect. Nadat u akkoord met de licentievoorwaarden en privacyverklaring gaat, klikt u op **doorgaan**.
![Welkom](media/active-directory-aadconnect-existing-database/db3.png)
5.  Op de **vereiste onderdelen installeren** scherm de **gebruik van een bestaande SQL Server** optie is ingeschakeld. Geef de naam van de SQL-server die als host voor de database van ADSync fungeert. Als het exemplaar van de SQL-engine gebruikt als host voor de database van ADSync niet het standaardexemplaar op de SQL server is, moet u de naam van de SQL-engine-exemplaar opgeven. Als SQL bladeren niet is ingeschakeld, moet u bovendien ook het poortnummer voor SQL-engine-exemplaar opgeven. Bijvoorbeeld:         
![Welkom](media/active-directory-aadconnect-existing-database/db4.png)           

6.  Op de **verbinding maken met Azure AD** scherm, moet u de referenties van een globale beheerder van uw Azure AD-directory opgeven. De aanbeveling is het gebruik van een account in het standaarddomein onmicrosoft.com. Dit account wordt alleen gebruikt om een serviceaccount in Azure AD aan te maken en wordt niet gebruikt wanneer de wizard is voltooid.
![Verbinding maken](media/active-directory-aadconnect-existing-database/db5.png)
 
7.  Op de **verbinding maken met uw adreslijsten** scherm, de bestaande AD-forest is geconfigureerd voor directory-synchronisatie met een rode x pictogram naast de naam wordt weergegeven. Als u wilt synchroniseren van wijzigingen uit een on-premises is AD-forest, een AD DS-account vereist. De Azure AD Connect-wizard is kan niet worden opgehaald van de referenties van de AD DS-account dat is opgeslagen in de database ADSync omdat de referenties zijn versleuteld en kunnen alleen worden ontsleuteld door de vorige Azure AD Connect-server. Klik op **wijziging referenties** om op te geven van de AD DS-account voor de AD-forest.
![Mappen](media/active-directory-aadconnect-existing-database/db6.png)
 
 
8.  In het pop-updialoogvenster, kunt u ofwel (i) bieden een ondernemingsadministrator-referentie en Azure AD Connect het AD DS-account maken voor u te laten, of (ii) de AD DS-account zelf maken en geef de referenties naar Azure AD Connect. Wanneer u een optie hebt geselecteerd en geef de benodigde referenties, klikt u op **OK** te sluiten van een pop-upvenster.
![Welkom](media/active-directory-aadconnect-existing-database/db7.png)
 
 
9.  Zodra de referenties zijn opgegeven, wordt het pictogram rode x vervangen door een pictogram groen maatstreepjes. Klik op **Volgende**.
![Welkom](media/active-directory-aadconnect-existing-database/db8.png)
 
 
10. Op de **klaar om te configureren** scherm, klikt u op **installeren**.
![Welkom](media/active-directory-aadconnect-existing-database/db9.png)
 
 
11. Zodra de installatie is voltooid, wordt de Azure AD Connect-server automatisch ingeschakeld voor de Faseringsmodus. Het is raadzaam om de serverconfiguratie en in behandeling zijnde uitvoer onverwachte wijzigingen voordat u de Faseringsmodus uitschakelt. 

## <a name="next-steps"></a>Volgende stappen

- Nu u Azure AD Connect geïnstalleerd hebt kunt u [de installatie verifiëren en licenties toewijzen](active-directory-aadconnect-whats-next.md).
- Meer informatie over deze functies, die tijdens de installatie zijn ingeschakeld: [Prevent accidental deletes](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md) en [Azure AD Connect Health](../connect-health/active-directory-aadconnect-health-sync.md).
- Meer informatie over deze algemene onderwerpen: [scheduler and how to trigger sync](active-directory-aadconnectsync-feature-scheduler.md).
- Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](active-directory-aadconnect.md).

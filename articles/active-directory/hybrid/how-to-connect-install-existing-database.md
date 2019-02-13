---
title: Azure AD Connect installeren met behulp van een bestaande ADSync-database | Microsoft Docs
description: In dit onderwerp wordt beschreven hoe u een bestaande ADSync-database.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.reviewer: cychua
ms.assetid: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/30/2017
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8254766568c54748ee3646dd627a102ffc86e743
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56191347"
---
# <a name="install-azure-ad-connect-using-an-existing-adsync-database"></a>Azure AD Connect met behulp van een bestaande ADSync-database installeren
Azure AD Connect vereist een SQL Server-database voor het opslaan van gegevens. U kunt de standaard die SQL Server 2012 Express LocalDB geïnstalleerd met Azure AD Connect of gebruik uw eigen volledige versie van SQL. Voorheen was tijdens de installatie van Azure AD Connect, is een nieuwe database met de naam ADSync altijd gemaakt. Met Azure AD Connect versie 1.1.613.0 (of na) hebt u de optie voor het Azure AD Connect installeren door het aan te wijzen aan een bestaande ADSync-database.

## <a name="benefits-of-using-an-existing-adsync-database"></a>Voordelen van het gebruik van een bestaande ADSync-database
Door aan te wijzen aan een bestaande ADSync-database:

- Met uitzondering van referenties informatie, wordt de configuratie van de synchronisatie is opgeslagen in de ADSync-database (inclusief aangepaste synchronisatieregels, connectors, filteren en configuratie van optionele functies) automatisch hersteld en gebruikt tijdens de installatie . Referenties die door Azure AD Connect wordt gebruikt om wijzigingen te synchroniseren met on-premises AD en Azure AD zijn versleuteld en kan alleen worden geopend door de vorige Azure AD Connect-server.
- De id-gegevens (die zijn gekoppeld aan connectorspaces en metaverse) en synchronisatie cookies die zijn opgeslagen in de ADSync-database ook worden hersteld. De nieuw geïnstalleerde Azure AD Connect dat wordt server kunt doorgaan met het synchroniseren van waar de vorige Azure AD Connect-server is gestopt, in plaats van de noodzaak om uit te voeren een volledige synchronisatie.

## <a name="scenarios-where-using-an-existing-adsync-database-is-beneficial"></a>Scenario's waarin met behulp van een bestaande ADSync-database nuttig
Deze voordelen zijn nuttig in de volgende scenario's:


- U hebt een bestaande Azure AD Connect-implementatie. Uw bestaande Azure AD Connect-server niet meer werkt, maar de SQL-server met de ADSync-database nog steeds wordt gebruikt. U kunt een nieuwe Azure AD Connect-server installeren en koppelen aan de bestaande ADSync-database. 
- U hebt een bestaande Azure AD Connect-implementatie. Uw SQL-server met de ADSync-database werkt niet meer. Maar hebt u een recente back-up van de database. U kunt eerst de ADSync-database herstellen naar een nieuwe SQL-server. Waarna u kunt een nieuwe Azure AD Connect-server installeren en wijs de herstelde ADSync-database.
- U hebt een bestaande Azure AD Connect-implementatie die van LocalDB gebruikmaakt. Vanwege de limiet van 10 GB die zijn opgelegd door LocalDB, wilt u migreren naar volledige SQL. U kunt back-up van de ADSync-database vanuit LocalDB en deze herstellen naar een SQL-server. Waarna u kunt een nieuwe Azure AD Connect-server opnieuw en wijs de herstelde ADSync-database.
- U probeert het instellen van een server met tijdelijke bestanden en wil ervoor zorgen dat de configuratie ervan overeenkomt met die van de huidige actieve server. U kunt back-up van de ADSync-database en deze herstellen naar een andere SQL server. Waarna u kunt een nieuwe Azure AD Connect-server opnieuw en wijs de herstelde ADSync-database.

## <a name="prerequisite-information"></a>Vereiste informatie

Belangrijke opmerkingen te nemen Let van op voordat u doorgaat:

- Zorg ervoor dat u het bekijken van de vereisten voor het installeren van Azure AD Connect op Hardware en de vereisten, en de account en de machtigingen die vereist zijn voor het installeren van Azure AD Connect. De machtigingen die vereist zijn voor het installeren van Azure AD Connect met modus 'bestaande database gebruiken' is gelijk aan 'aangepaste' installatie.
- Database, dat is het implementeren van Azure AD Connect op basis van een bestaande ADSync wordt alleen ondersteund met volledige SQL. Het wordt niet ondersteund met SQL Express LocalDB. Hebt u een bestaande ADSync-database in LocalDB die u wilt gebruiken, moet u back-up de ADSync-database (LocalDB) en deze terugzetten naar volledige SQL. Waarna u Azure AD Connect op basis van de herstelde database met behulp van deze methode kunt implementeren.
- De versie van de Azure AD Connect gebruiken voor de installatie moet voldoen aan de volgende criteria:
    - 1.1.613.0 of hoger, en
    - Dezelfde of hoger is dan de versie van de Azure AD Connect laatst gebruikt met de ADSync-database. Als de Azure AD Connect-versie die wordt gebruikt voor de installatie hoger dan de versie met de ADSync-database het laatst is gebruikt is, kan een volledige synchronisatie vereist zijn.  Volledige synchronisatie is vereist als er wijzigingen in de regel schema of de synchronisatie tussen de twee versies. 
- De ADSync-database die wordt gebruikt, moet een relatief recente van de synchronisatiestatus bevatten. De laatste synchronisatieactiviteit met de bestaande ADSync-database moet binnen de afgelopen drie weken.
- Bij het installeren van Azure AD Connect met behulp van de methode 'bestaande database gebruiken', is niet geconfigureerd op de vorige Azure AD Connect-server aanmeldingsmethode behouden. U kunt de aanmeldingsmethode verder niet configureren tijdens de installatie. U kunt alleen aanmeldingsmethode configureren nadat de installatie is voltooid.
- U kunt geen meerdere Azure AD Connect-servers dezelfde ADSync-database. De methode 'bestaande database gebruiken' kunt u een bestaande ADSync-database met een nieuwe Azure AD Connect-server opnieuw kunt gebruiken. Het biedt geen ondersteuning voor delen.

## <a name="steps-to-install-azure-ad-connect-with-use-existing-database-mode"></a>Stappen voor het Azure AD Connect installeren met de modus "bestaande database gebruiken"
1.  Azure AD Connect-installatieprogramma (AzureADConnect.MSI) naar de Windows-server te downloaden. Dubbelklik op het Azure AD Connect-installatieprogramma voor het starten van Azure AD Connect installeert.
2.  Zodra de MSI-installatie is voltooid, wordt de wizard Azure AD Connect gestart met de Express-installatiemodus. Sluit het scherm door op het pictogram Afsluiten te klikken.
![Welkom](./media/how-to-connect-install-existing-database/db1.png)
3.  Start een nieuwe opdrachtprompt of PowerShell-sessie. Ga naar de map <drive>\program files\Microsoft Azure AD Connect. Voer de opdracht .\AzureADConnect.exe /useexistingdatabase uit om de wizard Azure AD Connect te starten in de installatiemodus Bestaande database gebruiken.

> [!NOTE]
> Gebruik de schakeloptie **/useexistingdatabase** alleen wanneer de database bevat al gegevens uit een eerdere installatie van de Azure AD Connect. Bijvoorbeeld, wanneer u vanaf een lokale database wilt verplaatsen naar een volledige SQL Server-database of wanneer de Azure AD Connect-server opnieuw is opgebouwd en u een SQL back-up teruggezet van de ADSync-database van een eerdere installatie van Azure AD Connect. Als u een lege database gemaakt en deze voor installatie gebruiken, moet u deze stap overslaan.

![PowerShell](./media/how-to-connect-install-existing-database/db2.png)
4.  U wordt verwelkomd met het scherm Welkom bij Azure AD Connect. Nadat u akkoord bent gegaan met de licentievoorwaarden en privacyverklaring, klikt u op **Doorgaan**.
![Welkom](./media/how-to-connect-install-existing-database/db3.png)
5.  Op het scherm **Vereiste onderdelen installeren** is de optie **Een bestaande SQL-server gebruiken** ingeschakeld. Geef de naam op van de SQL-server waarop de ADSync-database wordt gehost. Als het SQL Engine-exemplaar dat wordt gebruikt om de ADSync-database te hosten, niet het standaardexemplaar is op de SQL-server, moet u de naam van het SQL Engine-exemplaar opgeven. Daarnaast moet u, indien bladeren in SQL niet is ingeschakeld, ook het poortnummer voor het SQL Engine-exemplaar opgeven. Bijvoorbeeld:         
![Welkom](./media/how-to-connect-install-existing-database/db4.png)           

6.  Op het scherm **Verbinding maken met Azure AD** moet u de referenties opgeven van een globale beheerder van de Azure AD-adreslijst. U wordt aangeraden om een account te gebruiken in het standaarddomein onmicrosoft.com. Dit account wordt alleen gebruikt om een serviceaccount in Azure AD aan te maken en wordt niet gebruikt wanneer de wizard is voltooid.
![Verbinding maken](./media/how-to-connect-install-existing-database/db5.png)
 
7.  Op het scherm **Verbinding maken met uw adreslijsten** wordt het bestaande AD-forest dat is geconfigureerd voor adreslijstsynchronisatie, weergegeven met een rood kruis ernaast. Voor het synchroniseren van wijzigingen vanuit een on-premises AD-forest is een AD DS-account vereist. Met de wizard Azure AD Connect kunnen de referenties van het AD DS-account dat is opgeslagen in de ADsync-database, niet worden opgehaald, omdat deze referenties zijn versleuteld en alleen kunnen worden ontsleuteld met de vorige Azure AD Connect-server. Klik op **Referenties wijzigen** om het AD DS-account voor het AD-forest op te geven.
![Adreslijsten](./media/how-to-connect-install-existing-database/db6.png)
 
 
8.  In het pop-updialoogvenster kunt u (i) de referenties van een ondernemingsadministrator opgeven en het AD DS-account voor u laten maken in Azure AD Connect of (ii) zelf het AD DS-account maken en de bijbehorende referenties opgeven in Azure AD Connect. Zodra u een optie hebt geselecteerd en de benodigde referenties hebt opgegeven, klikt u op **OK** om het pop-updialoogvenster te sluiten.
![Welkom](./media/how-to-connect-install-existing-database/db7.png)
 
 
9.  Zodra de referenties zijn opgegeven, verandert het rode kruis in een groen vinkje. Klik op **volgende**.
![Welkom](./media/how-to-connect-install-existing-database/db8.png)
 
 
10. Klik in het scherm **Gereed om te configureren** op **Installeren**.
![Welkom](./media/how-to-connect-install-existing-database/db9.png)
 
 
11. Zodra de installatie is voltooid, wordt de Azure AD Connect-server automatisch ingeschakeld voor de faseringsmodus. U wordt aangeraden om de serverconfiguratie en exports die in behandeling zijn, te controleren op onverwachte wijzigingen, voordat u de faseringsmodus uitschakelt. 

## <a name="post-installation-tasks"></a>Taken na de installatie
Bij het herstellen van een databaseback-up die is gemaakt met een versie van Azure AD Connect voordat 1.2.65.0, de staging-server een aanmeldingsmethode van automatisch selecteren **niet configureert**. Terwijl uw wachtwoordhashsynchronisatie en wachtwoord terugschrijven voorkeuren wordt hersteld, moet u de aanmeldingsmethode zodat deze overeenkomt met de andere beleidsregels van toepassing zijn op uw server voor de actieve synchronisatie later wijzigen.  Deze stappen is mislukt mogelijk te voorkomen dat gebruikers zich in moet deze server wordt geactiveerd.  

Gebruik de onderstaande tabel om te controleren of eventuele extra stappen die nodig zijn.

|Functie|Stappen|
|-----|-----|
|Wachtwoord-hashsynchronisatie| de wachtwoord-Hashsynchronisatie en instellingen voor het terugschrijven van wachtwoorden zijn volledig hersteld voor versies van Azure AD Connect beginnen met 1.2.65.0.  Als het herstellen van een oudere versie van Azure AD Connect, controleert u de instellingen van de synchronisatie-optie voor deze functies om te controleren of ze overeenkomen met uw server voor de actieve synchronisatie.  Er zijn geen andere configuratiestappen moeten nodig zijn.|
|Federatie met AD FS|Azure verificaties zullen echter ook doorgaan met het AD FS-beleid dat is geconfigureerd voor uw server voor de actieve synchronisatie.  Als u Azure AD Connect gebruikt voor het beheren van uw AD FS-farm, kunt u de aanmeldingsmethode kunt (optioneel) voor AD FS-federatie ter voorbereiding op de stand-by-server steeds de actieve synchronisatie-instantie wijzigen.   Als opties voor apparaten op de server voor de actieve synchronisatie zijn ingeschakeld, configureert u deze opties op deze server door het uitvoeren van de taak 'Apparaatopties configureren'.|
|Pass through-verificatie en Desktop Single Sign-On|Bijwerken van de aanmelding methode zodat deze overeenkomen met de configuratie op uw server voor de actieve synchronisatie.  Als dit niet wordt gevolgd voordat bevordering van de server naar de primaire, Pass through-verificatie samen met naadloze eenmalige aanmelding op uitgeschakeld en uw tenant mogelijk worden vergrendeld als u geen synchronisatie van wachtwoordhashes als back-up maken van optie aanmelden. Ook Houd er rekening mee dat wanneer u in de faseringsmodus Pass through-verificatie inschakelt, een nieuwe verificatieagent wordt geïnstalleerd, geregistreerd en wordt uitgevoerd als een hoge beschikbaarheid-agent die aanmelden aanvragen accepteert.|
|Federatie met PingFederate|Azure verificaties zullen echter ook doorgaan met de PingFederate-beleid dat is geconfigureerd voor uw server voor de actieve synchronisatie.  U mag de methode aanmelden (optioneel) wijzigen in PingFederate ter voorbereiding op de stand-by-server steeds de actieve synchronisatie-exemplaar.  Deze stap kan worden uitgesteld totdat u nodig hebt voor het federeren van aanvullende domeinen met PingFederate.|
## <a name="next-steps"></a>Volgende stappen

- Nu u Azure AD Connect geïnstalleerd hebt kunt u [de installatie verifiëren en licenties toewijzen](how-to-connect-post-installation.md).
- Meer informatie over deze functies, die de installatie zijn ingeschakeld: [Onopzettelijke verwijderingen voorkomen](how-to-connect-sync-feature-prevent-accidental-deletes.md) en [Azure AD Connect Health](how-to-connect-health-sync.md).
- Meer informatie over deze algemene onderwerpen: [scheduler and how to trigger sync](how-to-connect-sync-feature-scheduler.md).
- Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory ](whatis-hybrid-identity.md).

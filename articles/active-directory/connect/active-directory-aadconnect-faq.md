---
title: Azure Active Directory Connect Veelgestelde vragen - | Microsoft Docs
description: In dit artikel vindt u antwoorden op veelgestelde vragen over Azure AD Connect.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 545766aa6c39fe678113297e584422872a6ded87
ms.sourcegitcommit: e0a678acb0dc928e5c5edde3ca04e6854eb05ea6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/13/2018
ms.locfileid: "39008141"
---
# <a name="azure-active-directory-connect-faq"></a>Veelgestelde vragen over Azure Active Directory Connect

## <a name="general-installation"></a>Algemene installatie
**V: installatie werkt als de globale beheerder van Azure Active Directory (Azure AD) tweeledige verificatie (2FA heeft) ingeschakeld?**  
Dit scenario wordt ondersteund vanaf de builds februari 2016.

**V: is er een manier voor het installeren van Azure AD Connect zonder toezicht?**  
Azure AD Connect-installatie wordt alleen ondersteund bij gebruik van de wizard. Een installatie zonder toezicht, op de achtergrond wordt niet ondersteund.

**V: ik heb een forest waar één domein niet bereikbaar. Hoe installeer ik Azure AD Connect?**  
Dit scenario wordt ondersteund vanaf de builds februari 2016.

**V: bevat Azure Active Directory Domain Services (Azure AD DS)-health-agent werkt op server core?**  
Ja. Nadat u de agent hebt geïnstalleerd, kunt u het registratieproces voltooien met behulp van de volgende PowerShell-cmdlet: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**V: bevat Azure AD Connect ondersteuning synchroniseert uit twee domeinen in Azure AD?**  
Ja, dit scenario wordt ondersteund. Raadpleeg [meerdere domeinen](active-directory-aadconnect-multiple-domains.md).
 
**V: kan bevatten meerdere connectors voor hetzelfde Active Directory-domein in Azure AD Connect?**  
Nee, meerdere connectors voor hetzelfde AD-domein worden niet ondersteund. 

**V: kan ik de Azure AD Connect-database uit de lokale database verplaatsen naar een extern exemplaar van SQL Server?**   
Ja, bieden de volgende stappen uit algemene richtlijnen over hoe u dit doet. Er zijn momenteel gewerkt aan een meer gedetailleerde document.
1. Back-up van de LocalDB ADSync-database.
De eenvoudigste manier om dit te doen is met SQL Server Management Studio is geïnstalleerd op dezelfde computer als Azure AD Connect. Verbinding maken met *(localdb)\.\ADSync*, en vervolgens back-up van de ADSync-database.

2. De ADSync-database herstellen naar de externe SQL Server-exemplaar.

3. Installeer Azure AD Connect op basis van de bestaande [externe SQL-database](active-directory-aadconnect-existing-database.md).
   Het artikel ziet u hoe u migreren naar een lokale SQL-database gebruiken. Als u migreert naar een externe SQL-database, in stap 5 van het proces moet ook u een bestaand serviceaccount in de Windows-Sync-service wordt uitgevoerd. Dit serviceaccount voor synchronisatie-engine wordt hier beschreven:
   
      **Een bestaand serviceaccount gebruiken**: Azure AD Connect maakt standaard een virtueel serviceaccount voor de Synchronisatieservices gebruik om te gebruiken. Als u een extern exemplaar van SQL Server gebruiken of een proxyserver waarvoor verificatie, gebruikt u een beheerd serviceaccount of een service-account in het domein en het wachtwoord kennen. Voer in dat geval het te gebruiken account in. Zorg ervoor dat gebruikers met de installatie van de systeembeheerders in SQL zijn zodat aanmeldingsreferenties voor het serviceaccount kunnen worden gemaakt. Zie voor meer informatie, [Azure AD Connect accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). 
   
      In de laatste versie kan de inrichting van de database out-of-band worden uitgevoerd door de SQL-beheerder en vervolgens worden geïnstalleerd door de Azure AD Connect-beheerder met eigendomsrechten voor de database. Zie voor meer informatie, [Installeer Azure AD Connect met behulp van SQL-gedelegeerde beheerdersmachtigingen](active-directory-aadconnect-sql-delegation.md).

Dingen om eenvoudig te houden, is het raadzaam dat gebruikers die Azure AD Connect installeert systeembeheerders in SQL zijn. Echter met recente builds kunt u nu gebruikt u een SQL-beheerders gedelegeerde, zoals beschreven in [Installeer Azure AD Connect met behulp van SQL-gedelegeerde beheerdersmachtigingen](active-directory-aadconnect-sql-delegation.md).

## <a name="network"></a>Netwerk
**V: ik heb een firewall, netwerkapparaat of iets anders dat de tijd die verbindingen geopend op mijn netwerk blijven kunnen beperkt. Wat moet mijn drempel voor time-out van client-side wanneer ik Azure AD Connect gebruiken?**  
Alle netwerksoftware, fysieke apparaten of iets anders waardoor de maximumtijd die verbindingen kunnen open blijven moet een drempel van ten minste vijf minuten (300 seconden) gebruiken voor verbinding tussen de server waarop de Azure AD Connect-client is geïnstalleerd en Azure Active Directory. Deze aanbeveling geldt ook voor alle eerder uitgebrachte Microsoft Identity synchronisatie-hulpprogramma's.

**V: zijn enkelvoudige domeinen (niveaudomeinen) ondersteund?**  
Terwijl het wordt aangeraden op basis van deze configuratie van het netwerk ([Zie artikel](https://support.microsoft.com/en-us/help/2269810/microsoft-support-for-single-label-domains)), met behulp van Azure AD Connect-synchronisatie met een domein met enkelvoudig label wordt ondersteund, zolang de netwerkconfiguratie voor het domein op één niveau werkt correct.

**V: worden Forests met niet-aaneengesloten AD-domeinen ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor on-premises forests met niet-aaneengesloten naamruimten.

**V: worden "gestippeld' NetBIOS-namen ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor on-premises-forests of domeinen waar de NetBIOS-naam bevat een punt (.).

**Vraag: is puur IPv6-omgeving die wordt ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor een pure IPv6-omgeving.

## <a name="federation"></a>Federatie
**V: wat moet ik doen als ik ontvang een e-mail waarin wordt gevraagd om mijn Office 365-certificaat te vernieuwen?**  
Zie voor instructies over het verlengen van het certificaat [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md).

**V: ik heb 'Automatisch bijwerken relying party' instellen voor de Office 365 relying party. Heb ik dan actie ondernemen wanneer mijn voor token-ondertekening certificaat automatisch wordt getotaliseerd via?**  
Gebruik de richtlijnen die wordt beschreven in het artikel [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Omgeving
**Vraag: is dit dan ondersteund als u wilt wijzigen van de server nadat Azure AD Connect is geïnstalleerd?**  
Nee. De synchronisatie-engine kan geen verbinding met de SQL database-instantie wijzigen van de servernaam wordt weergegeven en de service niet starten.

## <a name="identity-data"></a>Identiteitsgegevens
**V: waarom niet het kenmerk userPrincipalName (UPN) in Azure AD overeenkomt met de on-premises UPN?**  
Voor meer informatie, Zie de volgende artikelen:

* [Gebruikersnamen in Office 365, Azure of Intune komen niet overeen met de on-premises UPN of alternatieve aanmeldings-ID](https://support.microsoft.com/en-us/kb/2523192)
* [Wijzigingen worden niet gesynchroniseerd met het hulpprogramma Azure Active Directory-synchronisatie nadat u de UPN van een gebruikersaccount voor het gebruik van een ander federatief domein wijzigen](https://support.microsoft.com/en-us/kb/2669550)

U kunt ook configureren met Azure AD zodat de synchronisatie-engine om bij te werken van de UPN, zoals beschreven in [functies van Azure AD Connect sync service](active-directory-aadconnectsyncservice-features.md).

**Vraag: is het zachte match een on-premises Azure AD-groep of neem contact op met object met een bestaande Azure AD-groep of neem contact op met het object ondersteund?**  
Ja, dit zachte match is gebaseerd op de proxyAddress. Voorlopig overeenkomende wordt niet ondersteund voor groepen die niet zijn e-mail-ingeschakeld.

**Vraag: is het handmatig instellen van het kenmerk ImmutableId op een bestaande Azure AD-groep of neem contact op met vaste-none-match deze aan een on-premises Azure AD-groep of neem contact op met de object-object ondersteund?**  
Nee, het kenmerk ImmutableId handmatig instellen op een bestaande Azure AD-groep of neem contact op met object naar vaste-none-match er wordt momenteel niet ondersteund.

## <a name="custom-configuration"></a>Aangepaste configuratie
**V: waar worden de PowerShell-cmdlets voor Azure AD Connect beschreven?**  
Met uitzondering van de cmdlets die worden beschreven op deze site, worden andere PowerShell-cmdlets vinden in Azure AD Connect worden niet ondersteund voor gebruik door de klant.

**V: kan ik met de optie 'Server exportserver/importeren' dat gevonden in Synchronization Service Manager naar de configuratie te verplaatsen tussen servers gebruiken?**  
Nee. Deze optie is niet ophalen voor alle configuratie-instellingen en mag niet worden gebruikt. In plaats daarvan gebruik de wizard voor het maken van de basisconfiguratie van de tweede server en de regel voor synchronisatie editor gebruiken voor het genereren van PowerShell-scripts voor het verplaatsen van een aangepaste regel tussen servers. Zie voor meer informatie, [Swingmigratie](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**Vraag: kunnen wachtwoorden worden opgeslagen voor de Azure-aanmeldingspagina en kan deze opslaan in cache worden voorkomen omdat deze een wachtwoord input-element met bevat de *automatisch aanvullen = "false"* kenmerk?**  
Op dit moment wijzigen van de HTML-kenmerken van de **wachtwoord** veld, met inbegrip van de code automatisch aanvullen, wordt niet ondersteund. We werken op dit moment in een functie voor aangepaste JavaScript, waarmee u een kenmerk toevoegen waarmee de **wachtwoord** veld.

**V: de Azure-aanmeldingspagina wordt weergegeven de gebruikersnamen van gebruikers die hebben aangemeld is. U kunt dit gedrag uitschakelen?**  
Op dit moment wijzigen van de HTML-kenmerken van de **wachtwoord** invoerveld, met inbegrip van de code automatisch aanvullen, wordt niet ondersteund. We werken op dit moment in een functie voor aangepaste JavaScript, waarmee u een kenmerk toevoegen waarmee de **wachtwoord** veld.

**V: is er een manier om te voorkomen dat gelijktijdige sessies?**  
Nee.

## <a name="auto-upgrade"></a>Automatische upgrade

**V: wat zijn de voordelen en consequenties van het gebruik van Automatische upgrade?**  
We zijn adviseren alle klanten om in te schakelen van Automatische upgrade voor hun Azure AD Connect-installatie. Het voordeel is dat u altijd de meest recente patches, met inbegrip van beveiligingsupdates voor zwakke plekken die zijn gevonden in Azure AD Connect ontvangt. Het upgradeproces is eenvoudig en wordt automatisch uitgevoerd zodra een nieuwe versie beschikbaar is. Automatische upgrade duizenden klanten van Azure AD Connect gebruiken met elke nieuwe release.

Het proces van automatische upgrades altijd eerst bepaalt of een installatie die in aanmerking komen voor automatische upgrade is. Als u in aanmerking komt, wordt de upgrade uitgevoerd en getest. Het proces omvat ook op zoek naar aangepaste wijzigingen in regels en specifieke omgevingsfactoren. Als de tests aangeven dat een upgrade mislukt is, wordt automatisch de vorige versie hersteld.

Het proces kan een paar uur duren, afhankelijk van de grootte van de omgeving. Terwijl de upgrade uitgevoerd wordt, wordt er geen synchronisatie tussen Windows Server Active Directory en Azure AD gebeurt.

**V: ik heb ontvangen een e-mailbericht gemeld dat mijn automatische upgrade niet meer werkt en ik wil een nieuwe versie installeert. Waarom heb ik nodig om dit te doen?**  
Vorig jaar, we een versie van Azure AD Connect die onder bepaalde omstandigheden, mogelijk uitgeschakeld de functie auto-upgrade op uw server uitgebracht. We hebben het probleem opgelost in Azure AD Connect versie 1.1.750.0. Als u hebt met het probleem problemen dergelijke, kunt u het beperken door het uitvoeren van een PowerShell-script om te herstellen of handmatig een upgrade uitvoert naar de nieuwste versie van Azure AD Connect. 

De PowerShell-script uit te voeren [download het script](https://aka.ms/repairaadconnect) en uitvoeren op uw Azure AD Connect-server in een PowerShell-venster met beheerdersrechten. Voor meer informatie over het uitvoeren van het script [weergeven in deze korte video](https://aka.ms/repairaadcau).

Als u handmatig bijwerken, moet u downloaden en uitvoeren van de meest recente versie van het bestand AADConnect.msi.
 
-  Als uw huidige versie ouder dan 1.1.750.0 is, [downloaden en te upgraden naar de nieuwste versie](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Als uw Azure AD Connect-versie 1.1.750.0 is of hoger, er geen verdere actie vereist is. U gebruikt al de versie die de automatische upgrade-oplossing bevat. 

**V: ik heb ontvangen een e-mailbericht waarin staat dat de upgrade naar de meest recente versie opnieuw inschakelen Automatische upgrade. Ik gebruik versie 1.1.654.0. Heb ik nodig om bij te werken?**  
Ja, moet u bijwerken naar versie 1.1.750.0 of later opnieuw inschakelen Automatische upgrade. [Downloaden en te upgraden naar de nieuwste versie](https://www.microsoft.com/en-us/download/details.aspx?id=47594).

**V: ik heb ontvangen een e-mailbericht waarin staat dat de upgrade naar de meest recente versie opnieuw inschakelen Automatische upgrade. Als ik PowerShell gebruikt hebt om in te schakelen van Automatische upgrade, nog steeds moet ik de meest recente versie installeren?**  
Ja, u toch wilt bijwerken naar versie 1.1.750.0 of hoger. Inschakelen van de service automatisch upgraden met PowerShell, wordt de automatische upgrade-probleem gevonden in versies vóór 1.1.750.0 niet opgelost.

**V: ik wil een upgrade uitvoert naar een nieuwere versie, maar ik weet niet zeker of die Azure AD Connect geïnstalleerd en er geen gebruikersnaam en wachtwoord. Moeten we dit?**
U hoeft niet te weten de gebruikersnaam en wachtwoord die in eerste instantie is gebruikt voor het Azure AD Connect upgraden. Elke Azure AD-account waarop de rol globale beheerder gebruiken.

**V: hoe vind ik welke versie van Azure AD Connect ik gebruik?**  
Om te controleren welke versie van Azure AD Connect is geïnstalleerd op uw server, gaat u naar het Configuratiescherm en de geïnstalleerde versie van Microsoft Azure AD Connect opzoeken door te selecteren **programma's** > **programma's en onderdelen** , zoals hier wordt weergegeven:

![Azure AD Connect-versie in het Configuratiescherm](media/active-directory-aadconnect-faq/faq1.png)

**V: hoe voer ik een upgrade naar de nieuwste versie van Azure AD Connect?**  
Zie voor informatie over het upgraden naar de nieuwste versie, [Azure AD Connect: upgraden van een eerdere versie naar de meest recente](active-directory-aadconnect-upgrade-previous-version.md). 

**V: er al een upgrade uitgevoerd naar de nieuwste versie van Azure AD Connect vorig jaar. Moeten we upgrade opnieuw uitvoeren?**  
De Azure AD Connect-team maakt regelmatig updates voor de service. Als u wilt profiteren van oplossingen voor problemen en beveiligingsupdates, evenals nieuwe functies, is het belangrijk dat u uw server up-to-date te houden met de meest recente versie. Als u Automatische upgrade inschakelt, wordt uw softwareversie wordt automatisch bijgewerkt. De versiegeschiedenis van release van Azure AD Connect, Zie [Azure AD Connect: releasegeschiedenis van versie](active-directory-aadconnect-version-history.md).

**V: hoe lang duurt het voordat de upgrade uitvoert, en wat zijn de gevolgen voor Mijn gebruikers?**  
De tijd die nodig is om bij te werken, is afhankelijk van de grootte van uw tenant. Voor grote organisaties is het mogelijk om uit te voeren van de upgrade in het weekend of 's avonds aanbevolen. Tijdens de upgrade is geen synchronisatieactiviteit plaatsvindt.

**V: ik denkt dat ik een upgrade uitgevoerd naar Azure AD Connect, maar de Office-portal noemt nog steeds DirSync. Waarom is dit?**  
De Office-team werkt als u de updates in overeenstemming met de naam van het huidige product in de Office-portal. Dit komt niet overeen welke sync-hulpprogramma dat u gebruikt.

**V: Mijn status voor automatische clientupdate zegt: 'Onderbroken'. Waarom is deze onderbroken? Moet ik dit inschakelen?**  
Een bug werd geïntroduceerd in een vorige versie die, onder bepaalde omstandigheden, blijft de status van de automatische upgrade ingesteld op 'Onderbroken'. Het handmatig inschakelen is technisch mogelijk, maar vergt wel verschillende complexe stappen. Het aanbevolen dat u kunt doen, is de meest recente versie van Azure AD Connect installeren.

**V: Mijn bedrijf heeft strikte wijzigingsbeheer vereisten en ik wil beheren wanneer deze is geactiveerd. Kan ik bepalen wanneer de automatische upgrade wordt gestart?**  
Nee, er is geen dergelijke functie vandaag nog. De functie wordt geëvalueerd voor een toekomstige release.

**V: Ik krijg een e-mailbericht als de automatische upgrade is mislukt? Hoe weet ik of deze geslaagd is?**  
U wordt niet gewaarschuwd over het resultaat van de upgrade. De functie wordt geëvalueerd voor een toekomstige release.

**V: kan u publiceren op een tijdlijn voor wanneer u van plan bent om te pushen automatische upgrades uitvoeren?**  
Automatische upgrade is de eerste stap bij de release van een nieuwere versie. Wanneer er sprake is van een nieuwe versie, worden automatisch upgrades gepusht. Er zijn nieuwere versies van Azure AD Connect vooraf aangekondigde in de [Roadmap voor Azure AD](../fundamentals/whats-new.md).

**V: bevat automatische upgrade ook Azure AD Connect Health bijwerken?**  
Ja, upgrades Automatische upgrade ook Azure AD Connect Health.

**V: u ook automatische clientupdate Azure AD Connect-servers in de faseringsmodus bevindt?**  
Ja, u kunt automatische clientupdate een Azure AD Connect-server die zich in de faseringsmodus bevindt.

**V: als automatische upgrade mislukt en Mijn Azure AD Connect-server niet wordt gestart, moet ik doen?**  
In zeldzame gevallen, de Azure AD Connect-service niet wordt gestart nadat u de upgrade uitvoert. In dergelijke gevallen kunt u het probleem meestal de server opnieuw wordt opgestart. Als de Azure AD Connect-service nog steeds niet wordt gestart, opent u een ondersteuningsticket. Zie voor meer informatie, [maken van een serviceaanvraag contact opnemen met ondersteuning voor Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**V: ik weet niet zeker wat de risico's zijn wanneer ik een upgrade naar een nieuwere versie van Azure AD Connect uitvoeren. Kunt u bellen om mij te helpen bij het upgraden?**  
Als u hulp nodig upgraden naar een nieuwere versie van Azure AD Connect hebt, open een ondersteuningsticket bij [maken van een serviceaanvraag contact opnemen met ondersteuning voor Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Problemen oplossen
**V: hoe kan ik hulp met Azure AD Connect?**

[Zoeken naar de Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Zoeken naar de KB voor technische oplossingen voor veelvoorkomende break-fix-problemen over ondersteuning voor Azure AD Connect.

[Azure Active Directory-Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Zoeken naar technische vragen en antwoorden of uw eigen vragen door te gaan naar [de Azure AD-community](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Ondersteuning voor Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

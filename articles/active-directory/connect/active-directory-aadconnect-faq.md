---
title: 'Azure Active Directory Connect: Veelgestelde vragen - | Microsoft Docs'
description: Deze pagina is Veelgestelde vragen over Azure AD Connect.
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
ms.openlocfilehash: 2e5a7cab5c9db0c13ca0c0986c18c86adf675562
ms.sourcegitcommit: 3c3488fb16a3c3287c3e1cd11435174711e92126
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/07/2018
ms.locfileid: "34850283"
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Veelgestelde vragen over Azure Active Directory Connect

## <a name="general-installation"></a>Algemene installatie
**V: installatie werkt als de globale beheerder van Azure AD 2FA ingeschakeld heeft?**  
Dit scenario wordt ondersteund met de opbouw van februari 2016.

**V: is er een manier voor het installeren van Azure AD Connect zonder toezicht?**  
Alleen wordt ondersteund voor het installeren van Azure AD Connect met de installatiewizard. Een installatie zonder toezicht en de achtergrond worden niet ondersteund.

**V: ik heb een forest waar één domein is niet bereikbaar. Hoe kan ik Azure AD Connect installeren?**  
Dit scenario wordt ondersteund met de opbouw van februari 2016.

**V: de health-agent voor AD DS werkt op server core?**  
Ja. Nadat de agent is geïnstalleerd, kunt u het registratieproces met de volgende PowerShell-cmdlet uitvoeren: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**V: AADConnect ondersteunt synchroniseren van de twee domeinen naar op Azure AD?**</br>
Ja, dit scenario wordt ondersteund. Raadpleeg [meerdere domeinen](active-directory-aadconnect-multiple-domains.md)
 
**V: kan hebt u meerdere connectors voor hetzelfde Active Directory-domein in Azure AD connect?**</br> Nee, meerdere connectors voor hetzelfde AD-domein worden niet ondersteund. 

**V: kan ik de Azure AD Connect-database verplaatsen uit de lokale database met een externe SQL-Server?**</br> De volgende stappen bevatten Ja, algemene richtlijnen over hoe u dit doet.  Er zijn momenteel gewerkt aan een meer gedetailleerde-document dat is binnenkort beschikbaar.


   1. Back-up van de 'ADSync' LocalDB-Database de eenvoudigste manier om dit te doen is het gebruik van SQL Server Management Studio is geïnstalleerd op dezelfde computer als Azure AD Connect. Verbinding maken met ' (localdb)\.\ADSync ' – vervolgens maakt u een Back-up van de database van ADSync
   2. Herstel de Database 'ADSync' naar de externe SQL-exemplaar
   3. Azure AD Connect op basis van de bestaande installeert [externe SQL-database](active-directory-aadconnect-existing-database.md) de koppeling bevat de stappen die nodig zijn wanneer u migreert naar een lokale SQL-Database gebruiken. Als u migreert naar een externe SQL-database in stap 5 van dit proces wordt ook moet u een bestaande serviceaccount dat de Windows-Sync-Service wordt uitgevoerd als op te geven. Dit serviceaccount voor synchronisatie-engine wordt hier beschreven:</br></br>
   **Een bestaand serviceaccount gebruiken**- standaard Azure AD Connect maakt gebruik van een virtuele-serviceaccount voor de synchronisatieservices te gebruiken. Als u een externe SQL server gebruiken of een proxy die verificatie vereist, moet u een beheerd serviceaccount gebruiken of gebruik van een serviceaccount in het domein en het wachtwoord weten. Voer in dat geval het te gebruiken account in. Zorg dat de gebruiker die de installatie uitvoert een SA in SQL is, zodat een aanmelding voor het serviceaccount kan worden aangemaakt. Zie [Azure AD Connect: accounts and permissions](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account) (Azure AD Connect: accounts en machtigingen).</br></br> In de laatste versie kan de inrichting van de database out-of-band worden uitgevoerd door de SQL-beheerder en vervolgens worden geïnstalleerd door de Azure AD Connect-beheerder met eigendomsrechten voor de database. Raadpleeg [Install Azure AD Connect using SQL delegated administrator permissions](active-directory-aadconnect-sql-delegation.md) (Azure AD Connect installeren met SQL-gedelegeerde beheerdersmachtigingen) voor meer informatie.

Het wordt aanbevolen dat de gebruiker Azure AD Connect installeert een SA in SQL is om de zaken eenvoudig. (SQL-beheerder echter met recente builds kunt u nu gebruiken worden overgedragen, zoals wordt beschreven [hier](active-directory-aadconnect-sql-delegation.md).

## <a name="network"></a>Netwerk
**V: ik heb een firewall, netwerkapparaat of iets anders dat de maximale tijd verbindingen beperkt geopend op mijn netwerk kunt blijven. Hoe lang mijn drempelwaarde clientzijde time-out moet bij gebruik van Azure AD Connect?**  
Alle netwerksoftware, fysieke apparaten of iets anders dat de maximale tijd verbindingen kunnen open blijven beperkt moet een drempel van minstens 5 minuten (300 seconden) gebruiken voor verbindingen tussen de server waarop de Azure AD Connect-client is geïnstalleerd en Azure Active Directory. Deze aanbeveling is ook van toepassing op alle eerder uitgebrachte Microsoft Identity hulpprogramma's voor synchronisatie.

**V: zijn niveaudomeinen (één Label domeinen) ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor lokale forests/domeinen met behulp van niveaudomeinen.

**V: zijn Forests met niet-aaneengesloten AD-domeinen ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor lokale-forests met niet-aaneengesloten naamruimten.

**V: zijn 'scheidingspunten' NetBios-naam ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor lokale forests/domeinen waar de NetBios-naam een punt bevat '. ' in de naam.

**V: is pure IPv6-omgeving ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor pure IPv6-omgeving.

## <a name="federation"></a>Federatie
**V: wat moet ik doen als ik een e-mailbericht ontvangen die vragen om mijn Office 365-certificaat te vernieuwen**  
Gebruik de richtlijnen die wordt beschreven in de [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md) document over het vernieuwen van het certificaat.

**V: ik heb 'Automatisch bijwerken relying party' instellen voor de relying party O365. Heb ik geen actie te ondernemen als mijn voor token-ondertekening certificaat automatisch boven?**  
Gebruik de richtlijnen die wordt beschreven in het artikel [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Omgeving
**V: is dit dan ondersteund als u wilt wijzigen van de server nadat Azure AD Connect is geïnstalleerd?**  
Nee. Naam van de server wijzigt, worden de synchronisatie-engine niet mogelijk om te verbinden met de SQL-database en de service kan niet worden gestart.

## <a name="identity-data"></a>Id-gegevens
**V: in Azure AD-de UPN (userPrincipalName)-kenmerk niet overeenkomt met de on-premises UPN - waarom?**  
Zie de volgende artikelen:

* [Gebruikersnamen in Office 365, Azure of Intune komen niet overeen met de lokale UPN of de alternatieve aanmeldings-ID](https://support.microsoft.com/en-us/kb/2523192)
* [Wijzigingen zijn niet gesynchroniseerd door de Azure Active Directory-synchronisatie nadat u de UPN van een gebruikersaccount voor het gebruik van een ander federatieve domein wijzigen](https://support.microsoft.com/en-us/kb/2669550)

U kunt ook configureren met Azure AD, zodat de synchronisatie-engine de userPrincipalName bijwerken zoals beschreven in [functies van de service Azure AD Connect-synchronisatie](active-directory-aadconnectsyncservice-features.md).

**V: is het ondersteund op zachte overeenkomst met lokale AD-groep/Contact objecten met bestaande Azure AD-groep/Contact objecten?**  
Ja, deze zachte overeenkomst wordt gebaseerd op de proxyAddress.  Voorlopig overeenkomende wordt niet ondersteund voor groepen die zijn geen e-mailfunctionaliteit.

**V: is het handmatig instellen ondersteund onveranderbare id genoemd-kenmerk op bestaande Azure AD-groep/Contact objecten hard overeenkomen met het lokale AD-groep/Contact objecten?**  
Nee, het kenmerk onveranderbare id genoemd handmatig instellen op een bestaande Azure AD-groep/Contact-object op vaste overeenkomst met het is momenteel niet ondersteund.

## <a name="custom-configuration"></a>Aangepaste configuratie
**V: waar zijn de PowerShell-cmdlets voor Azure AD Connect beschreven?**  
Met uitzondering van de cmdlets beschreven op deze site, worden andere gevonden in Azure AD Connect PowerShell-cmdlets worden niet ondersteund voor gebruik van de klant.

**V: kan ik gebruiken 'Server exportserver/importeren' gevonden in *Synchronization Service Manager* configuratie verplaatsen tussen servers?**  
Nee. Deze optie worden alle configuratie-instellingen niet ophalen en mag niet worden gebruikt. In plaats daarvan moet u de wizard gebruiken voor het maken van de basisconfiguratie op de tweede server en de regeleditor sync gebruiken voor het genereren van PowerShell-scripts voor het verplaatsen van een aangepaste regel tussen servers. Zie [migratie bewegen](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**V: kunnen wachtwoorden worden opgeslagen voor de Azure-aanmeldingspagina en kan dit worden voorkomen omdat het een wachtwoord invoerelement met automatisch aanvullen bevat = 'false' kenmerk?**</br>
De HTML-kenmerken van het invoerveld wachtwoord wijzigt, wordt met inbegrip van de tag automatisch aanvullen, niet ondersteund. Een functie die wordt toegestaan voor aangepaste Javascript, zodat u een kenmerk toevoegen aan het wachtwoordveld is nu wordt gewerkt.

**V: op de Azure-aanmeldingspagina, worden gebruikersnamen voor gebruikers die zich eerder hebt geregistreerd in met succes weergegeven.  U kunt dit gedrag uitschakelen?**</br>
De HTML-kenmerken van het invoerveld wachtwoord wijzigt, wordt met inbegrip van de tag automatisch aanvullen, niet ondersteund. Een functie die wordt toegestaan voor aangepaste Javascript, zodat u een kenmerk toevoegen aan het wachtwoordveld is nu wordt gewerkt.

**V: is er een manier om te voorkomen dat gelijktijdige sessies?**</br>
Nee.

## <a name="auto-upgrade"></a>Automatische upgrade

**V: wat zijn de voordelen en de gevolgen van het gebruik van Automatische upgrade?**</br>
Alle klanten wordt aangeraden te Automatische upgrade inschakelen voor hun Azure AD Connect-installatie. De voordelen zijn dat ze altijd de meest recente patches, inclusief beveiligingsupdates voor beveiligingsproblemen die zijn gevonden in Azure AD Connect ontvangen. Het upgradeproces is eenvoudig en gebeurt automatisch als een nieuwe versie beschikbaar is. Automatische upgrade gebruik duizenden Azure AD Connect klanten met elke nieuwe release.

Het upgradeproces automatisch wordt altijd eerst tot stand brengen of een installatie in aanmerking komen voor automatische upgrade is (dit proces omvat aangepaste wijzigingen in regels, zoekt u specifieke omgevingsfactoren enz.) en zo ja, de upgrade is uitgevoerd en getest. Als de tests weergeven dat een upgrade niet gelukt is, wordt de vorige versie automatisch ophalen hersteld.

Het proces kan enkele uren duren, afhankelijk van de grootte van de omgeving en tijdens de upgrade gebeurt, gebeurt er is geen synchronisatie tussen Windows Server AD en Azure AD.

**V: ik heb ontvangen een e-mailbericht gemeld dat mijn automatische upgrade werkt niet meer en moet ik nieuwe versie installeren. Waarom heb ik nodig om dit te doen?**</br>
Afgelopen jaar weer, een versie van Azure AD Connect die onder bepaalde omstandigheden mogelijk uitgeschakeld met de functie voor automatisch bijwerken op uw server is uitgegeven. Dit probleem is verholpen in Azure AD Connect versie 1.1.750.0. Klanten die zijn beïnvloed door dit probleem moeten het uitvoeren van een PowerShell-script voor het herstellen van dit of upgrade handmatig naar de meest recente versie van Azure AD Connect te verhelpen van het probleem. 

Het PowerShell-script wordt uitgevoerd, downloadt u het script uit [hier](https://aka.ms/repairaadconnect) en voer het script op uw server AADConnect in een administratieve PowerShell-venster. [Dit is een korte video](https://aka.ms/repairaadcau) die wordt gedetailleerd uitgelegd hoe u dit doet.

Als u handmatig upgraden, moet u downloaden en uitvoeren van de meest recente versie van het bestand AADConnect.msi.
 
-  Als uw huidige versie ouder dan 1.1.750.0 is, moet u upgraden naar de nieuwste versie [die hier kan worden gedownload](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Als uw Azure AD Connect-versie 1.1.750.0 is of hoger, u hoeft niet te doen om te beperken van het probleem dat Automatische upgrade, zoals u al op de versie met een oplossing voor dit. 

**V: ik heb een melding om te upgraden naar de nieuwste versie opnieuw inschakelen Automatische upgrade e-mailbericht hebt ontvangen. Ik ben op 1.1.654.0, heb ik nodig om bij te werken?** </br>    
Ja, u moet een upgrade uitvoeren naar 1.1.750 of nieuwere Automatische upgrade opnieuw inschakelen. Hier wordt de koppeling die wordt uitgelegd hoe u een upgrade uitvoert naar een nieuwere versie

**V: ik heb een melding om te upgraden naar de nieuwste versie opnieuw inschakelen Automatische upgrade e-mailbericht hebt ontvangen. Ik heb PowerShell gebruikt Automatische upgrade inschakelen, moet ik de meest recente versie installeren?**</br>    
Ja, u toch wilt bijwerken naar versie 1.1.750.0 of hoger. Inschakelen van de automatische upgrade service met PowerShell, wordt de automatische upgrade probleem gevonden in de versies vóór 1.1.750 niet opgelost

**V: ik een upgrade naar een nieuwere versie wilt maken, maar ik niet zeker weet die Azure AD Connect geïnstalleerd en er geen gebruikersnaam en wachtwoord.  Dit nodig?**</br>
U hoeft niet te weten de gebruikersnaam en wachtwoord die in eerste instantie is gebruikt voor het upgraden van Azure AD Connect: een Azure AD-account met de rol globale beheerder kan worden gebruikt.

**V: hoe vind ik welke versie van Azure AD Connect ik ben op?**</br>   
Om te controleren welke versie van Azure AD Connect is geïnstalleerd op uw server, gaat u naar Configuratiescherm en opzoeken van de geïnstalleerde versie van Microsoft Azure AD Connect in 'programma's > programma's en-onderdelen:

![versie](media/active-directory-aadconnect-faq/faq1.png)

**V: hoe voer ik een upgrade naar de nieuwste versie van AADConnect uit?**</br>    
Dit [artikel](active-directory-aadconnect-upgrade-previous-version.md) wordt uitgelegd hoe u een upgrade uitvoert naar een nieuwere versie. 

**V: we al is bijgewerkt naar de nieuwste versie van AADConnect vorig jaar, moeten we upgrade opnieuw starten?**</br> De Azure AD Connect-teams vaak worden bijgewerkt met de service maakt en het is belangrijk dat de server is bijgewerkt door de meest recente versie om te profiteren van oplossingen voor problemen en beveiligingsupdates, evenals nieuwe functies. Als u Automatische upgrade inschakelen wordt uw softwareversie automatisch bijgewerkt. Als u de versiegeschiedenis van Azure AD Connect zoekt, volgt u deze [koppeling](active-directory-aadconnect-version-history.md).

**V: hoe lang duurt het uitvoeren van de upgrade en wat zijn de gevolgen voor Mijn gebruikers?**</br>    
De tijd die nodig is om bij te werken, is afhankelijk van de grootte van uw tenant en voor grotere organisaties kan het beste om dit te doen in het weekend of buiten kantooruren zijn. Tijdens de upgrade is er geen synchronisatieactiviteit plaatsvindt.

**V: ik ervan overtuigd dat ik een upgrade uitgevoerd naar AADConnect, maar in de Office-portal er nog steeds DirSync vermeld.  Waarom is dit?**</br>    
Het Office-team werkt als u de Office-portal-updates in overeenstemming met de naam van het huidige product – deze komt niet overeen met welke sync-hulpprogramma dat u gebruikt.

**V: ik mijn Auto-Upgrade-status gecontroleerd en wordt 'onderbroken'. Waarom is deze onderbroken Moet ik deze functie inschakelen?**</br>     
Een fout is geïntroduceerd in een vorige versie die onder bepaalde omstandigheden dat de upgradestatus van automatisch ingesteld zorgt ervoor op 'onderbroken'. Handmatig waardoor het technisch mogelijk is, maar zou vereisen verschillende stappen van complexe, dus het beste dat kunt u doen is de meest recente versie van Azure AD Connect installeren

**V: Mijn bedrijf heeft beheervereisten strikte wijzigen en ik wil beheren wanneer deze wordt gepusht. Kan ik bepalen wanneer de automatische Upgrade wordt gestart?**</br> Nee, kan niet worden dergelijke vandaag, deze functie is iets dat wordt geëvalueerd voor een toekomstige release.

**V: ik krijgt een e-mail als de automatische upgrade is mislukt? Hoe weet ik of geslaagd is?**</br>     
U wordt niet gewaarschuwd dat het resultaat van de upgrade is deze functie is iets dat wordt geëvalueerd voor een toekomstige release.

**Q:do publiceren van een tijdlijn over wanneer u plant voor de push-automatische upgrades uitvoeren?**</br>    
Automatische clientupdate is de eerste stap bij de release van een nieuwere versie dus wanneer er een nieuwe release automatische upgrades wordt gepusht. Er zijn nieuwere versies van Azure AD Connect vooraf aangekondigde in de [Azure AD Roadmap](../../active-directory/whats-new.md).

**V: automatische clientupdate upgrade AAD Connect Health?**</br>   Ja, Automatische upgrade ook upgrades AAD Connect Health

**V: wilt u ook automatische clientupdate AAD Connect servers in de Faseringsmodus?**</br>   
Ja, u kunt automatische clientupdate een Azure AD Connect-server die zich in de faseringsmodus.

**V: als Auto-Upgrade is mislukt en Mijn server AAD Connect niet wordt gestart, wat moet ik doen?**</br>   
In zeldzame gevallen, de Azure AD Connect-service niet wordt gestart na het uitvoeren van de upgrade. In dergelijke gevallen start opnieuw op de server, die meestal wordt het probleem opgelost. Als de service Azure AD Connect nog steeds niet start, moet u een ondersteuningsticket opent. Hier volgt een [koppeling](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) waarin wordt uitgelegd hoe u dat doet. 

**V: ik niet zeker weet wat de risico's zijn bij een upgrade naar een nieuwere versie van Azure AD Connect. Kunt u Bel mij om mij te helpen bij het upgraden?**</br>
Als u de fout te upgraden naar een nieuwere versie van Azure AD Connect, een ondersteuningsticket opent, wordt hier een [koppeling](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) die wordt beschreven hoe u die.

## <a name="troubleshooting"></a>Problemen oplossen
**V: hoe vind ik meer informatie over Azure AD Connect?**

[Zoeken in de Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Zoek in de Microsoft Knowledge Base (KB) voor technische oplossingen voor problemen schadevergoedings over ondersteuning voor Azure AD Connect.

[Microsoft Azure Active Directory-Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* U kunt zoeken en bladeren voor technische vragen en van de community antwoorden of uw eigen vraag door te klikken op [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Ondersteuning voor Azure AD krijgen](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)




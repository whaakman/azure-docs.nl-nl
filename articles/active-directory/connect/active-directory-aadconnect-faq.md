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
ms.date: 05/09/2018
ms.author: billmath
ms.openlocfilehash: 46a9bf47b4998c4d5be47f67556fbdb3ba7b71db
ms.sourcegitcommit: d28bba5fd49049ec7492e88f2519d7f42184e3a8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/11/2018
---
# <a name="frequently-asked-questions-for-azure-active-directory-connect"></a>Veelgestelde vragen over Azure Active Directory Connect

## <a name="general-installation"></a>Algemene installatie
**V: installatie werkt als de globale beheerder van Azure AD 2FA ingeschakeld heeft?**  
Met de opbouw van februari 2016, wordt dit ondersteund.

**V: is er een manier voor het installeren van Azure AD Connect zonder toezicht?**  
Alleen wordt ondersteund voor het installeren van Azure AD Connect met de installatiewizard. Een installatie zonder toezicht en de achtergrond wordt niet ondersteund.

**V: ik heb een forest waar één domein is niet bereikbaar. Hoe kan ik Azure AD Connect installeren?**  
Met de opbouw van februari 2016, wordt dit ondersteund.

**V: de health-agent voor AD DS werkt op server core?**  
Ja. Nadat de agent is geïnstalleerd, kunt u het registratieproces met de volgende PowerShell-cmdlet uitvoeren: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**V: AADConnect ondersteunt synchroniseren van de twee domeinen naar op Azure AD?**</br>
Ja, dit wordt ondersteund. Raadpleeg [meerdere domeinen](active-directory-aadconnect-multiple-domains.md)
 
**V: bieden we ondersteuning voor meerdere connectors voor hetzelfde Active Directory-domein in Azure AD connect met?**</br> Nee, dit wordt niet ondersteund 

## <a name="network"></a>Netwerk
**V: ik heb een firewall, netwerkapparaat of iets anders dat de maximale tijd verbindingen beperkt geopend op mijn netwerk kunt blijven. Hoe lang mijn client side time-out drempelwaarde moet bij gebruik van Azure AD Connect?**  
Alle netwerksoftware, fysieke apparaten of iets anders dat de maximale tijd verbindingen kunnen open blijven beperkt moet een drempel van minstens 5 minuten (300 seconden) gebruiken voor verbindingen tussen de server waarop de Azure AD Connect-client is geïnstalleerd en Azure Active Directory. Dit geldt ook voor alle eerder uitgebrachte Microsoft Identity hulpprogramma's voor synchronisatie.

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
Gebruik de richtlijnen die wordt beschreven in de [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md) onderwerp over het vernieuwen van het certificaat.

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
Ja, wordt dit wordt gebaseerd op de proxyAddress.  Voorlopig overeenkomende wordt niet ondersteund voor groepen die zijn geen e-mailfunctionaliteit.

**V: is het handmatig instellen ondersteund onveranderbare id genoemd-kenmerk op bestaande Azure AD-groep/Contact objecten hard overeenkomen met het lokale AD-groep/Contact objecten?**  
Nee, dit wordt momenteel niet ondersteund.

## <a name="custom-configuration"></a>Aangepaste configuratie
**V: waar zijn de PowerShell-cmdlets voor Azure AD Connect beschreven?**  
Met uitzondering van de cmdlets beschreven op deze site, worden andere gevonden in Azure AD Connect PowerShell-cmdlets worden niet ondersteund voor gebruik van de klant.

**V: kan ik gebruiken 'Server exportserver/importeren' gevonden in *Synchronization Service Manager* configuratie verplaatsen tussen servers?**  
Nee. Deze optie worden alle configuratie-instellingen niet ophalen en mag niet worden gebruikt. In plaats daarvan moet u de wizard gebruiken voor het maken van de basisconfiguratie op de tweede server en de regeleditor sync gebruiken voor het genereren van PowerShell-scripts voor het verplaatsen van een aangepaste regel tussen servers. Zie [migratie bewegen](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**V: kunnen wachtwoorden worden opgeslagen voor de Azure-aanmeldingspagina en kan dit worden voorkomen omdat het een wachtwoord invoerelement met automatisch aanvullen bevat = 'false' kenmerk?**</br>
Wordt momenteel niet ondersteund voor het wijzigen van de HTML-kenmerken van het invoerveld wachtwoord, met inbegrip van de tag automatisch aanvullen. Er zijn momenteel gewerkt aan een functie die wordt toegestaan voor aangepaste Javascript waarmee u een kenmerk toevoegen aan het wachtwoordveld. Dit moet deel uitmaken beschikbaar hoger van 2017.

**V: op de Azure-aanmeldingspagina, worden gebruikersnamen voor gebruikers die zich eerder hebt geregistreerd in met succes weergegeven.  U kunt dit gedrag uitschakelen?**</br>
Wordt momenteel niet ondersteund voor het wijzigen van de HTML-kenmerken van de aanmeldingspagina. Er zijn momenteel gewerkt aan een functie die wordt toegestaan voor aangepaste Javascript waarmee u een kenmerk toevoegen aan het wachtwoordveld. Dit moet deel uitmaken beschikbaar hoger van 2017.

**V: is er een manier om te voorkomen dat gelijktijdige sessies?**</br>
Nee.

## <a name="auto-upgrade"></a>Automatische upgrade

**V: wat zijn de voordelen en de gevolgen van het gebruik van Automatische upgrade?**</br>
We raden alle klanten Automatische upgrade inschakelen voor hun Azure AD Connect-installatie. De voordelen zijn dat ze altijd de meest recente patches, inclusief beveiligingsupdates voor zwakke plekken die we in Azure AD Connect gevonden ontvangen. Het upgradeproces is eenvoudig en gebeurt automatisch als een nieuwe versie beschikbaar is. We fungeren duizenden Azure AD Connect klanten door middel van Automatische upgrade met elke nieuwe release.

Het upgradeproces automatisch wordt altijd eerst tot stand brengen of een installatie in aanmerking komen voor automatische upgrade is (dit omvat aangepaste wijzigingen in regels, zoekt u specifieke omgevingsfactoren enz.) en zo ja, de upgrade is uitgevoerd en getest. Als de tests weergeven dat een upgrade niet gelukt is, wordt de vorige versie automatisch ophalen hersteld.

Het proces kan enkele uren duren, afhankelijk van de grootte van de omgeving en tijdens de upgrade gebeurt, gebeurt er is geen synchronisatie tussen Windows Server AD en Azure AD.

**V: ik heb ontvangen een e-mailbericht gemeld dat mijn automatische upgrade werkt niet meer en moet ik nieuwe versie installeren. Waarom heb ik nodig om dit te doen?**</br>
Afgelopen jaar we uitbrachten een versie van Azure AD Connect die onder bepaalde omstandigheden mogelijk uitgeschakeld met de functie voor automatisch bijwerken op uw server. We dit probleem opgelost in Azure AD Connect versie 1.1.750.0, die werd gepubliceerd aan het einde van de afgelopen maand. We moeten klanten die zijn beïnvloed door dit probleem handmatig te upgraden naar de nieuwste versie van Azure AD Connect te verhelpen van het probleem. Als u handmatig upgraden, moet u downloaden en uitvoeren van de meest recente versie van het bestand AADConnect.msi.
 
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

**V: we al is bijgewerkt naar de nieuwste versie van AADConnect vorig jaar, moeten we upgrade opnieuw starten?**</br> De Azure AD Connect-teams vaak worden bijgewerkt met de service maakt en is het belangrijk dat de server wordt bijgewerkt met de meest recente versie om te profiteren van oplossingen voor problemen en beveiligingsupdates, evenals nieuwe functies. Als u Automatische upgrade inschakelen wordt uw softwareversie automatisch bijgewerkt. Als u de versiegeschiedenis van Azure AD Connect zoekt, volgt u deze [koppeling](active-directory-aadconnect-version-history.md).

**V: hoe lang duurt het uitvoeren van de upgrade en wat zijn de gevolgen voor Mijn gebruikers?**</br>    
De tijd die nodig is om bij te werken, is afhankelijk van de grootte van uw tenant en voor grotere organisaties kan het beste om dit te doen in het weekend of buiten kantooruren zijn. Houd er rekening mee dat tijdens de upgrade geen synchronisatieactiviteit plaatsvindt.

**V: ik ervan overtuigd dat ik een upgrade uitgevoerd naar AADConnect, maar in de Office-portal er nog steeds DirSync vermeld.  Wny is dit?**</br>    
Het Office-team werkt als u de Office-portal-updates in overeenstemming met de naam van het huidige product – deze komt niet overeen met welke sync-hulpprogramma dat u gebruikt.

**V: ik mijn Auto-Upgrade-status gecontroleerd en wordt 'onderbroken'. Waarom is deze onderbroken Moet ik deze functie inschakelen?**</br>     
Een fout is geïntroduceerd in een vorige versie die onder bepaalde omstandigheden dat de upgradestatus van automatisch ingesteld zorgt ervoor op 'onderbroken'. Handmatig waardoor het technisch mogelijk is, maar zou vereisen verschillende stappen van complexe, dus het beste dat kunt u doen is de meest recente versie van Azure AD Connect installeren

**V: Mijn bedrijf heeft beheervereisten strikte wijzigen en ik wil beheren wanneer deze wordt gepusht. Kan ik bepalen wanneer de automatische Upgrade wordt gestart?**</br> Nee, kan niet worden dergelijke vandaag, is dit iets die we in een toekomstige release evalueren wilt.

**V: ik krijgt een e-mail als de automatische upgrade is mislukt? Hoe weet ik of geslaagd is?**</br>     
U wordt niet gewaarschuwd dat het resultaat van de upgrade, dit is iets die we in een toekomstige release evalueren wilt.

**Q:do publiceren van een tijdlijn over wanneer u plant voor de push-automatische upgrades uitvoeren?**</br>    
Automatische clientupdate is de eerste stap in onze proces release van een nieuwere versie, zodat wanneer er een nieuwe release we de automatische upgrades zendt. Er zijn nieuwere versies van Azure AD Connect vooraf aangekondigde in de [Azure AD Roadmap](../../active-directory/whats-new.md).

**V: automatische clientupdate upgrade AAD Connect Health?**</br>   Ja, Automatische upgrade ook upgrades AAD Connect Health

**V: wilt u ook automatische clientupdate AAD Connect servers in de Faseringsmodus?**</br>   
Nee, u kunt geen automatische clientupdate een Azure AD Connect-server die zich in de faseringsmodus.

**V: als Auto-Upgrade is mislukt en Mijn server AAD Connect niet wordt gestart, wat moet ik doen?**</br>   
In zeldzame gevallen, de Azure AD Connect-service niet wordt gestart na het uitvoeren van de upgrade. In deze gevallen start opnieuw op de server, die meestal wordt het probleem opgelost. Als de service Azure AD Connect nog steeds niet wordt gestart, opent u een ondersteuningsticket. Hier volgt een [koppeling](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) waarin wordt uitgelegd hoe u dat doet. 

**V: ik niet zeker weet wat de risico's zijn bij een upgrade naar een nieuwere versie van Azure AD Connect. Kunt u Bel mij om mij te helpen bij het upgraden?**</br>
Als u nodig te upgraden naar een nieuwere versie van Azure AD Connect, opent u een ondersteuningsticket hebt, is dit een [koppeling](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/) die wordt beschreven hoe u die.

## <a name="troubleshooting"></a>Problemen oplossen
**V: hoe vind ik meer informatie over Azure AD Connect?**

[Zoeken in de Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Zoek in de Microsoft Knowledge Base (KB) voor technische oplossingen voor problemen schadevergoedings over ondersteuning voor Azure AD Connect.

[Microsoft Azure Active Directory-Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* U kunt zoeken en bladeren voor technische vragen en van de community antwoorden of uw eigen vraag door te klikken op [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Ondersteuning voor Azure AD krijgen](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

* Gebruik deze koppeling om ondersteuning te krijgen via de Azure portal.


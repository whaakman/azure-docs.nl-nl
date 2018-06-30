---
title: Azure Active Directory Connect FAQ - | Microsoft Docs
description: Dit artikel bevat antwoorden op veelgestelde vragen over Azure AD Connect.
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
ms.openlocfilehash: 7edabc99da5e1466e848336c647a33213c9edd8b
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/29/2018
ms.locfileid: "37132885"
---
# <a name="azure-active-directory-connect-faq"></a>Veelgestelde vragen over Azure Active Directory Connect

## <a name="general-installation"></a>Algemene installatie
**V: installatie werkt als de globale beheerder van Azure Active Directory (Azure AD) heeft een tweeledige verificatie (2FA) ingeschakeld?**  
Dit scenario wordt ondersteund vanaf de builds februari 2016.

**V: is er een manier voor het installeren van Azure AD Connect zonder toezicht?**  
Azure AD Connect-installatie wordt alleen ondersteund als u de installatiewizard gebruiken. Een installatie zonder toezicht, achtergrond, wordt niet ondersteund.

**V: ik heb een forest waar één domein is niet bereikbaar. Hoe kan ik Azure AD Connect installeren?**  
Dit scenario wordt ondersteund vanaf de builds februari 2016.

**V: bevat Azure Active Directory Domain Services (Azure AD DS)-health-agent werkt op server core?**  
Ja. Nadat u de agent installeert, kunt u het registratieproces voltooien met behulp van de volgende PowerShell-cmdlet: 

`Register-AzureADConnectHealthADDSAgent -Credentials $cred`

**V: bevat Azure AD Connect ondersteuning synchroniseren van de twee domeinen naar op Azure AD?**  
Ja, dit scenario wordt ondersteund. Raadpleeg [meerdere domeinen](active-directory-aadconnect-multiple-domains.md).
 
**V: kan hebt u meerdere connectors voor hetzelfde Active Directory-domein in Azure AD Connect?**  
Nee, meerdere connectors voor hetzelfde AD-domein worden niet ondersteund. 

**V: kan ik de Azure AD Connect-database uit de lokale database verplaatsen naar een extern exemplaar van SQL Server?**   
Ja, biedt de volgende stappen uit algemene richtlijnen over hoe u dit doet. We zijn momenteel gewerkt aan een meer gedetailleerde document.
1. Back-up van de LocalDB ADSync-database.
De eenvoudigste manier om dit te doen is met SQL Server Management Studio is geïnstalleerd op dezelfde computer als Azure AD Connect. Verbinding maken met *(localdb)\.\ADSync*, en vervolgens back-up van de database van ADSync.

2. Herstel de database ADSync naar uw externe SQL Server-exemplaar.

3. Azure AD Connect op basis van de bestaande installeert [externe SQL-database](active-directory-aadconnect-existing-database.md).
   Het artikel laat zien hoe migreren naar een lokale SQL-database gebruiken. Als u migreert naar een externe SQL-database, in stap 5 van het proces moet ook u een bestaand serviceaccount het Windows-Sync-service wordt uitgevoerd. Dit serviceaccount voor synchronisatie-engine wordt hier beschreven:
   
      **Een bestaand serviceaccount gebruiken**: Azure AD Connect gebruikt standaard een virtuele-serviceaccount voor de synchronisatieservices te gebruiken. Als u een extern exemplaar van SQL Server gebruiken of een proxy die verificatie vereist, gebruik van een beheerd serviceaccount of een serviceaccount in het domein en het wachtwoord weten. Voer in dat geval het te gebruiken account in. Zorg ervoor dat gebruikers die de installatie uitvoeren systeembeheerders in SQL zodat aanmeldingsreferenties voor het serviceaccount kunnen worden gemaakt. Zie voor meer informatie [Azure AD Connect accounts en machtigingen](active-directory-aadconnect-accounts-permissions.md#azure-ad-connect-sync-service-account). 
   
      In de laatste versie kan de inrichting van de database out-of-band worden uitgevoerd door de SQL-beheerder en vervolgens worden geïnstalleerd door de Azure AD Connect-beheerder met eigendomsrechten voor de database. Zie voor meer informatie [Azure AD Connect installeren met behulp van SQL gedelegeerde beheerdersmachtigingen](active-directory-aadconnect-sql-delegation.md).

Om de zaken eenvoudige, wordt u aangeraden dat gebruikers die Azure AD Connect installeert systeembeheerders in SQL zijn. Echter met recente builds kunt u nu gebruik SQL-beheerders gedelegeerde, zoals beschreven in [Azure AD Connect installeren met behulp van SQL gedelegeerde beheerdersmachtigingen](active-directory-aadconnect-sql-delegation.md).

## <a name="network"></a>Netwerk
**V: ik heb een firewall, netwerkapparaat of iets anders waarmee de tijd dat verbindingen geopend op mijn netwerk blijven kunnen wordt beperkt. Wat moet mijn drempelwaarde clientzijde time-out bij het gebruik van Azure AD Connect?**  
Alle netwerksoftware, fysieke apparaten of iets anders dat de maximale tijd dat verbindingen kunnen open blijven beperkt moet een drempelwaarde voor ten minste vijf minuten (300 seconden) gebruiken voor verbindingen tussen de server waarop de Azure AD Connect-client is geïnstalleerd en Azure Active Directory. Deze aanbeveling is ook van toepassing op alle eerder uitgebrachte Microsoft Identity hulpprogramma's voor synchronisatie.

**V: zijn enkelvoudige domeinen (niveaudomeinen) ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor on-premises-forests of domeinen die niveaudomeinen gebruiken.

**V: zijn Forests met niet-aaneengesloten AD-domeinen ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor lokale-forests met niet-aaneengesloten naamruimten.

**V: zijn 'scheidingspunten' NetBIOS-namen ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor on-premises-forests of domeinen waar de NetBIOS-naam bevat een punt (.).

**V: is pure IPv6-omgeving ondersteund?**  
Nee, Azure AD Connect biedt geen ondersteuning voor een pure IPv6-omgeving.

## <a name="federation"></a>Federatie
**V: wat moet ik doen als ik ontvangt een e-mailbericht waarin wordt gevraagd om mijn Office 365-certificaat te vernieuwen?**  
Zie voor instructies over het vernieuwen van het certificaat [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md).

**V: ik heb 'Automatisch bijwerken relying party' instellen voor de Office 365 relying party. Heb ik geen actie te ondernemen als mijn voor token-ondertekening certificaat automatisch boven?**  
Gebruik de richtlijnen die wordt beschreven in het artikel [certificaten vernieuwen](active-directory-aadconnect-o365-certs.md).

## <a name="environment"></a>Omgeving
**V: is dit dan ondersteund als u wilt wijzigen van de server nadat Azure AD Connect is geïnstalleerd?**  
Nee. De servernaam van de wijzigen Hiermee maakt u de synchronisatie-engine kan geen verbinding maken met het exemplaar van SQL database en de service niet starten.

## <a name="identity-data"></a>Id-gegevens
**V: waarom niet het kenmerk userPrincipalName (User Principal Name) in Azure AD overeenkomt met de lokale UPN?**  
Zie voor informatie in deze artikelen:

* [Gebruikersnamen in Office 365, Azure of Intune komen niet overeen met de lokale UPN of de alternatieve aanmeldings-ID](https://support.microsoft.com/en-us/kb/2523192)
* [Wijzigingen zijn niet gesynchroniseerd door de Azure Active Directory-synchronisatie nadat u de UPN van een gebruikersaccount voor het gebruik van een ander federatieve domein wijzigen](https://support.microsoft.com/en-us/kb/2669550)

U kunt ook configureren met Azure AD, zodat de synchronisatie-engine voor het bijwerken van de UPN, zoals beschreven in [functies van de service Azure AD Connect-synchronisatie](active-directory-aadconnectsyncservice-features.md).

**V: is het zachte overeen met een Azure AD-groep voor lokale of neem contact op met object met een bestaande Azure AD-groep of neem contact op met het object ondersteund?**  
Ja, deze zachte treffer is gebaseerd op de proxyAddress. Voorlopig overeenkomende wordt niet ondersteund voor groepen die zijn geen e-mailfunctionaliteit.

**V: is het handmatig Stel het kenmerk onveranderbare id genoemd in een bestaande Azure AD-groep of neem contact op met het object op om vaste overeen met het lokale Azure AD-groep of neem contact op met het object ondersteund?**  
Nee, het kenmerk onveranderbare id genoemd handmatig instellen op een bestaande Azure AD-groep of contact-object op harde-overeenkomst met het is momenteel niet ondersteund.

## <a name="custom-configuration"></a>Aangepaste configuratie
**V: waar zijn de PowerShell-cmdlets voor Azure AD Connect beschreven?**  
Met uitzondering van de cmdlets die zijn beschreven op deze site, worden andere gevonden in Azure AD Connect PowerShell-cmdlets worden niet ondersteund voor gebruik van de klant.

**V: kan ik de optie 'Server exportserver/importeren' die gevonden in Synchronization Service Manager voor de configuratie verplaatsen tussen servers gebruiken?**  
Nee. Deze optie niet ophalen van alle configuratie-instellingen en mag niet worden gebruikt. In plaats daarvan met de wizard kunt u de basisconfiguratie maken op de tweede server en de regeleditor sync gebruiken voor het genereren van PowerShell-scripts voor het verplaatsen van een aangepaste regel tussen servers. Zie voor meer informatie [migratie bewegen](active-directory-aadconnect-upgrade-previous-version.md#swing-migration).

**V: kunnen wachtwoorden worden opgeslagen voor de Azure-aanmeldingspagina en kan deze caching voorkomen omdat deze een wachtwoord invoerelement met bevat de *automatisch aanvullen = "false"* kenmerk?**  
Op dit moment wijzigen van de HTML-kenmerken van de **wachtwoord** veld, inclusief de tag automatisch aanvullen, wordt niet ondersteund. We zijn momenteel gewerkt aan een functie waarmee aangepaste JavaScript, waarmee u het toevoegen van een kenmerk voor de **wachtwoord** veld.

**V: de Azure-aanmeldingspagina worden de gebruikersnamen van gebruikers die zich eerder hebt geregistreerd in is weergegeven. U kunt dit gedrag uitschakelen?**  
Op dit moment wijzigen van de HTML-kenmerken van de **wachtwoord** invoerveld, met inbegrip van de tag automatisch aanvullen, wordt niet ondersteund. We zijn momenteel gewerkt aan een functie waarmee aangepaste JavaScript, waarmee u het toevoegen van een kenmerk voor de **wachtwoord** veld.

**V: is er een manier om te voorkomen dat gelijktijdige sessies?**  
Nee.

## <a name="auto-upgrade"></a>Automatische upgrade

**V: wat zijn de voordelen en de gevolgen van het gebruik van Automatische upgrade?**  
We zijn adviseren alle klanten Automatische upgrade inschakelen voor hun Azure AD Connect-installatie. Het voordeel is dat u altijd de meest recente patches, inclusief beveiligingsupdates voor beveiligingsproblemen die zijn gevonden in Azure AD Connect ontvangt. Het upgradeproces is eenvoudig en gebeurt automatisch als een nieuwe versie beschikbaar is. Automatische upgrade gebruik duizenden Azure AD Connect klanten met elke nieuwe release.

Het proces van automatische upgrades altijd eerst vaststelt of een installatie van een in aanmerking komen voor automatische upgrade. Als u in aanmerking komt, wordt de upgrade uitgevoerd en getest. Het proces omvat ook aangepaste wijzigingen in regels en specifieke omgevingsfactoren zoekt. Als de tests weergeven dat een upgrade mislukt is, wordt de vorige versie automatisch hersteld.

Afhankelijk van de grootte van de omgeving, kan een aantal uren in beslag nemen. Terwijl de upgrade uitgevoerd wordt, gebeurt er is geen synchronisatie tussen Windows Server Active Directory en Azure AD.

**V: ik heb ontvangen een e-mailbericht gemeld dat mijn automatische upgrade werkt niet meer en moet ik een nieuwe versie installeren. Waarom heb ik nodig om dit te doen?**  
Afgelopen jaar we een versie van Azure AD Connect die onder bepaalde omstandigheden mogelijk uitgeschakeld op uw server de functie voor automatische clientupdate uitgebracht. We hebben het probleem opgelost in Azure AD Connect versie 1.1.750.0. Als u zijn beïnvloed door het probleem, kunt u het verhelpen door een PowerShell-script voor het herstellen of handmatig upgraden naar de meest recente versie van Azure AD Connect. 

Het PowerShell-script uitvoeren [downloaden van het script](https://aka.ms/repairaadconnect) en uitvoeren op uw Azure AD Connect-server in een administratieve PowerShell-venster. Voor informatie over het uitvoeren van het script [in deze korte video bekijken](https://aka.ms/repairaadcau).

Als u handmatig upgraden, moet u downloaden en uitvoeren van de meest recente versie van het bestand AADConnect.msi.
 
-  Als uw huidige versie ouder dan 1.1.750.0 is, [downloaden en te upgraden naar de nieuwste versie](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
- Als uw Azure AD Connect-versie 1.1.750.0 is of later zijn er geen verdere actie vereist. U de versie die de oplossing voor automatische clientupdate bevat al gebruikt. 

**V: ik heb een melding om te upgraden naar de nieuwste versie opnieuw inschakelen Automatische upgrade e-mailbericht hebt ontvangen. Ik gebruik versie 1.1.654.0. Heb ik nodig om bij te werken?**  
Ja, moet u bijwerken naar versie 1.1.750.0 of hoger Automatische upgrade opnieuw inschakelen. [Downloaden en te upgraden naar de nieuwste versie](https://www.microsoft.com/en-us/download/details.aspx?id=47594).

**V: ik heb een melding om te upgraden naar de nieuwste versie opnieuw inschakelen Automatische upgrade e-mailbericht hebt ontvangen. Als ik PowerShell Automatische upgrade inschakelen hebt gebruikt, nog steeds moet ik de meest recente versie installeren?**  
Ja, u toch wilt bijwerken naar versie 1.1.750.0 of hoger. Inschakelen van de service Automatische clientupdate met PowerShell, wordt de automatische upgrade probleem gevonden in de versies vóór 1.1.750.0 niet opgelost.

**V: ik een upgrade naar een nieuwere versie wilt maken, maar ik niet zeker weet die Azure AD Connect geïnstalleerd en er geen gebruikersnaam en wachtwoord. Dit nodig?**
U hoeft niet te weten de gebruikersnaam en wachtwoord op die in eerste instantie is gebruikt voor het upgraden van Azure AD Connect. Elke Azure AD-account met de rol globale beheerder gebruiken.

**V: hoe vind ik welke versie van Azure AD Connect ik gebruik?**  
Om te controleren welke versie van Azure AD Connect is geïnstalleerd op uw server, gaat u naar Configuratiescherm en selecteert u de geïnstalleerde versie van Microsoft Azure AD Connect opzoekt **programma's** > **programma's en onderdelen** , zoals hier wordt weergegeven:

![Azure AD Connect-versie in het Configuratiescherm](media/active-directory-aadconnect-faq/faq1.png)

**V: hoe voer ik een upgrade naar de nieuwste versie van Azure AD Connect?**  
Zie voor informatie over het upgraden naar de nieuwste versie, [Azure AD Connect: upgraden van een eerdere versie naar de meest recente](active-directory-aadconnect-upgrade-previous-version.md). 

**V: we al is bijgewerkt naar de nieuwste versie van Azure AD Connect afgelopen jaar. Moet er opnieuw bijwerken?**  
Het team van Azure AD Connect maakt regelmatig updates voor de service. Als u wilt profiteren van oplossingen voor problemen en beveiligingsupdates, evenals nieuwe functies, is het belangrijk dat u uw server up-to-date te houden met de nieuwste versie. Als u Automatische upgrade inschakelt, wordt uw softwareversie automatisch bijgewerkt. Zie informatie over de versiegeschiedenis van Azure AD Connect [Azure AD Connect: versiegeschiedenis van release](active-directory-aadconnect-version-history.md).

**V: hoe lang duurt om uit te voeren van de upgrade en wat zijn de gevolgen voor Mijn gebruikers?**  
De tijd die nodig is om bij te werken, is afhankelijk van de grootte van uw tenant. Voor grote organisaties is het mogelijk dat het best om de upgrade in de 's avonds of het weekend. Tijdens de upgrade is geen synchronisatieactiviteit plaatsvindt.

**V: ik meent upgraden naar Azure AD Connect, maar de Office-portal noemt nog steeds DirSync. Waarom is dit?**  
Het Office-team werkt als u de Office-portal-updates in overeenstemming met de naam van het huidige product. Dit komt niet overeen met welke sync-hulpprogramma dat u gebruikt.

**V: Mijn automatische clientupdate status aangeeft, 'Onderbroken'. Waarom is deze onderbroken Moet ik deze functie inschakelen?**  
Een fout is geïntroduceerd in een vorige versie die onder bepaalde omstandigheden, blijft de status van de automatische upgrade ingesteld op 'Onderbroken'. Handmatig waardoor het technisch mogelijk is, maar verschillende complexe stappen vereist. Het aanbevolen dat u kunt doen, is de meest recente versie van Azure AD Connect installeert.

**V: Mijn bedrijf heeft strikte wijzigingsbeheer vereisten en ik wil beheren wanneer deze is geactiveerd. Kan ik bepalen wanneer de automatische upgrade wordt gestart?**  
Nee, er is geen functie dergelijke vandaag. De functie wordt geëvalueerd voor een toekomstige release.

**V: ik krijgt een e-mail als de automatische upgrade is mislukt? Hoe weet ik of geslaagd is?**  
U wordt niet gewaarschuwd voor het resultaat van de upgrade. De functie wordt geëvalueerd voor een toekomstige release.

**V: kan u een tijdlijn voor publiceren wanneer u van plan bent voor de push-automatische upgrades uitvoeren?**  
Automatische upgrade is de eerste stap in het vrijgaveproces van een nieuwere versie. Wanneer er een nieuwe versie, worden automatisch upgrades gepusht. Er zijn nieuwere versies van Azure AD Connect vooraf aangekondigde in de [Azure AD Roadmap](../fundamentals/whats-new.md).

**V: bevat automatische upgrade ook Azure AD Connect Health bijwerken?**  
Ja, worden automatische upgrade ook bijgewerkt Azure AD Connect Health.

**V: heb u ook automatische clientupdate Azure AD Connect-servers in de faseringsmodus?**  
Ja, u kunt automatische clientupdate een Azure AD Connect-server die zich in de faseringsmodus.

**V: wat moet ik doen als automatische upgrade mislukt en Mijn Azure AD Connect-server niet wordt gestart?**  
In zeldzame gevallen starten de Azure AD Connect-service niet nadat u de upgrade uitvoert. In dergelijke gevallen kunt u het probleem meestal de server opnieuw opstarten. Als de service Azure AD Connect nog steeds niet start, moet u een ondersteuningsticket opent. Zie voor meer informatie [maken van een serviceaanvraag aan contact op met ondersteuning voor Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/). 

**V: ik niet zeker weet wat de risico's zijn wanneer ik een naar een nieuwere versie van Azure AD Connect upgrade. Kunt u Bel mij om mij te helpen bij het upgraden?**  
Als u nodig te upgraden naar een nieuwere versie van Azure AD Connect, opent u een ondersteuningsticket op [maken van een serviceaanvraag aan contact op met ondersteuning voor Office 365](https://blogs.technet.microsoft.com/praveenkumar/2013/07/17/how-to-create-service-requests-to-contact-office-365-support/).

## <a name="troubleshooting"></a>Problemen oplossen
**V: hoe vind ik meer informatie over Azure AD Connect?**

[Zoeken in de Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Zoek de KB voor technische oplossingen voor problemen schadevergoedings over ondersteuning voor Azure AD Connect.

[Azure Active Directory-Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* Zoeken naar technische vragen en antwoorden of uw eigen vragen door te gaan naar [de Azure AD-community](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Ondersteuning voor Azure AD](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

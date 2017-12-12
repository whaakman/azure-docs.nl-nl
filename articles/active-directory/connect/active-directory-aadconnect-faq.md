---
title: 'Azure Active Directory Connect: Veelgestelde vragen - | Microsoft Docs'
description: Deze pagina is Veelgestelde vragen over Azure AD Connect.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
ms.assetid: 4e47a087-ebcd-4b63-9574-0c31907a39a3
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/12/2017
ms.author: billmath
ms.openlocfilehash: 13f12f39fb72bd7d4e32056753a012c38034f807
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
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

## <a name="federation"></a>Federation
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
Nee, dit wordt momenteel niet ondersteund.

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

## <a name="troubleshooting"></a>Problemen oplossen
**V: hoe vind ik meer informatie over Azure AD Connect?**

[Zoeken in de Microsoft Knowledge Base (KB)](https://www.microsoft.com/en-us/Search/result.aspx?q=azure%20active%20directory%20connect&form=mssupport)

* Zoek in de Microsoft Knowledge Base (KB) voor technische oplossingen voor problemen schadevergoedings over ondersteuning voor Azure AD Connect.

[Microsoft Azure Active Directory-Forums](https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=WindowsAzureAD)

* U kunt zoeken en bladeren voor technische vragen en van de community antwoorden of uw eigen vraag door te klikken op [hier](https://social.msdn.microsoft.com/Forums/azure/en-US/newthread?category=windowsazureplatform&forum=WindowsAzureAD&prof=required).

[Ondersteuning voor Azure AD krijgen](https://docs.microsoft.com/azure/active-directory/active-directory-troubleshooting-support-howto)

* Gebruik deze koppeling om ondersteuning te krijgen via de Azure portal.


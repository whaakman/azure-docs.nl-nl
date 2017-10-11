---
title: Azure Active Directory Connect Health FAQ - Azure | Microsoft Docs
description: Deze Veelgestelde vragen over de antwoorden op vragen over Azure AD Connect Health. In deze Veelgestelde vragen worden vragen behandeld over het gebruik van de service, zoals het factureringsmodel, de mogelijkheden, beperkingen en ondersteuning.
services: active-directory
documentationcenter: 
author: billmath
manager: samueld
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 902e5bdfbbf04ab70989be8c41e16eb69e475908
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Veelgestelde vragen over Azure AD Connect Health
In dit artikel bevat antwoorden op veelgestelde vragen (FAQ's) over Azure Active Directory (Azure AD) Connect Health. Deze Veelgestelde vragen hebben betrekking op vragen over het gebruik van de service, waaronder het facturering model, mogelijkheden, beperkingen en ondersteuning.

## <a name="general-questions"></a>Algemene vragen
**V: ik beheren meerdere directory's van Azure AD. Hoe schakel ik met de fout die Azure Active Directory Premium is**

Schakelen tussen verschillende Azure AD-tenants, selecteer de die momenteel is aangemeld **gebruikersnaam** in de rechterbovenhoek hoek en kies vervolgens de juiste account. Als het account hier niet wordt weergegeven, selecteert u **Afmelden**, en gebruik vervolgens de referenties van de globale beheerder van de directory met Azure Active Directory Premium ingeschakeld aan te melden.

**V: welke versie van de identiteit functies worden ondersteund door Azure AD Connect Health?**

De volgende tabel vindt u de rollen en versies van besturingssystemen ondersteund.

|Rol| Besturingssysteem / versie|
|--|--|
|Active Directory Federation Services (AD FS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | Versie 1.0.9125 of hoger|
|Active Directory Domain Services (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Houd er rekening mee dat de functies van de service op basis van de rol en het besturingssysteem verschillen kunnen. Met andere woorden, alle functies mogelijk niet beschikbaar voor alle versies van besturingssystemen. Zie de functiebeschrijvingen van de voor meer informatie.

**V: hoeveel licenties moet ik mijn infrastructuur controleren?**

* De eerste Connect Health-Agent is ten minste één Azure AD Premium-licentie vereist.
* Elke extra geregistreerde agent vereist 25 extra licenties voor Azure AD Premium.
* Aantal agents is gelijk aan het totaal aantal agents die zijn geregistreerd via alle bewaakte rollen (AD FS, Azure AD Connect en/of AD DS).

Licentie-informatie is ook te vinden op de [prijzen van Azure AD-pagina](https://aka.ms/aadpricing).

Voorbeeld:

| Geregistreerde agents | Licenties nodig | Voorbeeld van de configuratie van bewaking |
| ------ | --------------- | --- |
| 1 | 1 | 1 azure AD Connect-server |
| 2 | 26| 1 azure AD Connect-server en 1-domeincontroller |
| 3 | 51 | 1 active Directory Federation Services (AD FS)-server, 1 AD FS-proxy en 1-domeincontroller |
| 4 | 76 | 1 AD FS-server, 1 AD FS-proxy en 2 domeincontrollers |
| 5 | 101 | 1 azure AD Connect-server, 1 AD FS-server, 1 AD FS-proxy en 2-domeincontrollers |


## <a name="installation-questions"></a>Installatievragen

**V: Wat is de invloed van de Azure AD Connect Health-Agent installeren op afzonderlijke servers?**

Het effect van het installeren van de Microsoft Azure AD Connect Health-Agent, AD FS, webtoepassingsproxyservers, Azure AD Connect (sync)-servers, domeincontrollers is minimale ten opzichte van de CPU, geheugen, netwerkbandbreedte en opslag.

De volgende cijfers zijn een benadering:

* CPU-verbruik: ~ 1-5% verhogen.
* Geheugengebruik: maximaal 10% van het systeemgeheugen.

> [!NOTE]
> Als de agent kan niet met Azure communiceren, worden de gegevens voor een gedefinieerde maximumlimiet lokaal opgeslagen in de agent. De agent worden de gegevens 'in de cache' op basis van 'minst recentelijk onderhouden' overschreven.
>
>

* Lokale buffer opslag voor Azure AD Connect Health-Agents: ~ 20 MB.
* Voor AD FS-servers, wordt aangeraden dat u een schijfruimte van maximaal 1024 MB (1 GB inricht) voor de AD FS-audit-kanaal van Azure AD Connect Health-Agents de controlegegevens verwerken voordat deze wordt overschreven.

**V: wordt ik heb mijn servers starten tijdens de installatie van de Azure AD Connect Health-Agents?**

Nee. Start de server is niet nodig voor de installatie van de agents die zijn vereist. Installatie van bepaalde vereiste stappen vereisen echter een herstart van de server.

Installatie van .NET-Framework 4.5 vereist bijvoorbeeld in Windows Server 2008 R2, server opnieuw opstarten.

**V: bevat Azure AD Connect Health werk via een Pass Through-HTTP-proxy?**

Ja. U kunt de Health-Agent voor het gebruik van een HTTP-proxy voor het doorsturen van uitgaande HTTP-aanvragen te configureren voor lopende bewerkingen.
Lees meer over [HTTP-Proxy voor Health-Agents configureren](active-directory-aadconnect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Als u een proxy configureren tijdens de agentregistratie nodig hebt, moet u mogelijk de proxyinstellingen van Internet Explorer vooraf te wijzigen.

1. Open Internet Explorer > **instellingen** > **Internetopties** > **verbindingen** > **LAN-instellingen** .
2. Selecteer **een proxyserver gebruiken voor uw LAN**.
3. Selecteer **Geavanceerd** hebt u verschillende proxyservers poorten voor HTTP en HTTPS/Secure.

**V: bevat Azure AD Connect Health ondersteuning basisverificatie bij het verbinden met de HTTP-proxy's?**

Nee. Een mechanisme voor het opgeven van een willekeurige gebruikersnaam en wachtwoord voor basisverificatie is momenteel niet ondersteund.

**V: wat firewallpoorten heb ik nodig voor de Azure AD Connect Health-Agent werkt openen?**

Zie de [gedeelte vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor de lijst met firewall-poorten en andere vereisten voor connectiviteit.

**V: Waarom zie ik twee servers met dezelfde naam in de portal voor Azure AD Connect Health?**

Wanneer u een agent van een server verwijdert, wordt de server niet automatisch verwijderd uit de portal Azure AD Connect Health. Als u handmatig een agent van een server verwijderen of de server zelf verwijderen, moet u de vermelding van de server handmatig verwijderen van de portal Azure AD Connect Health.

U kunt installatiekopie van een server of een nieuwe server maken met de dezelfde gegevens (zoals de naam van de machine). Als de reeds geregistreerde server niet hebt verwijderd van de portal Azure AD Connect Health en u de agent op de nieuwe server hebt geïnstalleerd, ziet u mogelijk twee vermeldingen met dezelfde naam.

In dit geval wordt de vermelding die deel uitmaakt van de oudere server handmatig verwijderen. De gegevens voor deze server moet niet actueel zijn.

## <a name="health-agent-registration-and-data-freshness"></a>Registratie en gegevens versheid van Health-Agent

**V: wat zijn veelvoorkomende redenen voor de Health Agent-registratiefouten en hoe kan ik problemen oplossen?**

De health-agent kan worden geregistreerd vanwege de volgende redenen mislukken:

* De agent kan niet communiceren met de vereiste eindpunten omdat verkeer door een firewall wordt geblokkeerd. Dit geldt met name op webtoepassingsproxyservers. Zorg ervoor dat u uitgaande communicatie met de vereiste eindpunten en poorten zijn toegestaan. Zie de [gedeelte vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor meer informatie.
* Uitgaande communicatie is onderworpen aan een SSL-inspectie door de network-laag. Dit zorgt ervoor dat het certificaat dat de agent gebruikt om te worden vervangen door de controle van de server/entiteit en de stappen voor het voltooien van de agentregistratie van de mislukken.
* De gebruiker heeft geen toegang tot het uitvoeren van de registratie van de agent. Globale beheerders heeft standaard toegang. U kunt [rollen gebaseerd toegangsbeheer](active-directory-aadconnect-health-operations.md#manage-access-with-role-based-access-control) gemachtigde aan andere gebruikers.

**V: ik ben ophalen gewaarschuwd dat 'Health-servicegegevens is niet actueel.' Hoe kan ik het probleem oplossen?**

Azure AD Connect Health, wordt er een waarschuwing gegenereerd wanneer deze niet de gegevenspunten van de server in de afgelopen twee uur ontvangt. Er kan ook meerdere oorzaken voor deze waarschuwing.

* De agent kan niet communiceren met de vereiste eindpunten omdat verkeer door een firewall wordt geblokkeerd. Dit geldt met name op webtoepassingsproxyservers. Zorg ervoor dat u uitgaande communicatie met de vereiste eindpunten en poorten zijn toegestaan. Zie de [gedeelte vereisten](active-directory-aadconnect-health-agent-install.md#requirements) voor meer informatie.
* Uitgaande communicatie is onderworpen aan een SSL-inspectie door de network-laag. Dit zorgt ervoor dat het certificaat dat de agent gebruikt om te worden vervangen door de controle van de server/entiteit en het proces om gegevens te uploaden naar de Azure AD Connect Health-service mislukt.
* U kunt de connectiviteitsopdracht is ingebouwd in de agent. [Lees meer](active-directory-aadconnect-health-agent-install.md#test-connectivity-to-azure-ad-connect-health-service).
* De agents bieden ook ondersteuning voor uitgaande verbindingen via een niet-geverifieerde HTTP-Proxy. [Lees meer](active-directory-aadconnect-health-agent-install.md##configure-azure-ad-connect-health-agents-to-use-http-proxy).

## <a name="operations-questions"></a>Bewerkingen vragen
**V: heb ik nodig voor het inschakelen van controle voor de web application proxy-servers?**

Nee, controle hoeft niet te worden ingeschakeld op de web application proxy-servers.

**V: hoe Azure AD Connect Health waarschuwingen opgelost ophalen?**

Waarschuwingen van Azure AD Connect Health ophalen van een voorwaarde geslaagd opgelost. Azure AD Connect Health-Agents detecteren en de voorwaarden geslaagd periodiek rapporteren aan de service. De onderdrukking is voor enkele waarschuwingen op basis van tijd. Met andere woorden, als de foutvoorwaarde voor de dezelfde is niet-naleving binnen 72 uur van het genereren van waarschuwingen de waarschuwing automatisch opgelost.

**V: ik ben ophalen gewaarschuwd dat 'Test-verificatieaanvraag (synthetische transactie) kan niet een token verkrijgen." Hoe kan ik het probleem oplossen?**

Azure AD Connect Health voor AD FS wordt deze waarschuwing gegenereerd als de Health-Agent geïnstalleerd op een AD FS-server een token te verkrijgen als onderdeel van een synthetische transactie is gestart door de Health-Agent is mislukt. De Health-agent de context van het lokale systeemaccount gebruikt en probeert op te halen van een token voor een relying party zelf. Dit is een catch all-test om ervoor te zorgen dat AD FS in een status is van de uitgifte van tokens.

Deze test mislukt meestal omdat de Health-Agent niet kan omzetten van de naam van de AD FS-farm. Dit kan gebeuren als de AD FS-servers achter een network load balancers en de aanvraag wordt geïnitieerd door een knooppunt dat zich achter de load balancer (in plaats van een reguliere-client die voor de load balancer). Dit kan worden opgelost door het bijwerken van het Hostsbestand '' onder 'C:\Windows\System32\drivers\etc' de IP-adres van de AD FS-server of een loopback-IP-adres (127.0.0.1) voor de naam van de AD FS-farm (bijvoorbeeld sts.contoso.com) op te nemen. Bestand met de host toe te voegen, wordt de netwerkaanroep van het, waardoor de Health-Agent voor het ophalen van het token kortsluiting.

**V: Ik krijg een e-mailbericht dat aangeeft dat mijn machines zijn niet gevuld voor de recente ransomeware aanvallen. Waarom krijg ik dit e-mailadres**

Azure AD Connect Health-service gescand alle de machines die deze controleert om te controleren of de vereiste patches zijn geïnstalleerd. Het e-mailbericht is verzonden naar de tenantbeheerders als ten minste één machine geen essentiële patches. De volgende logica is gebruikt om dit te bepalen.
1. De hotfixes die zijn geïnstalleerd op de computer vinden.
2. Controleer of ten minste één van de HotFixes uit de lijst met gedefinieerde aanwezig is.
3. Zo ja, is de machine beveiligd. Als dat niet het geval is, wordt de computer is kwetsbaar voor de aanval.

De volgende PowerShell-script kunt u deze controle handmatig uitvoeren. De bovenstaande logica geïmplementeerd.

```
Function CheckForMS17-010 ()
{
    $hotfixes = "KB3205409", "KB3210720", "KB3210721", "KB3212646", "KB3213986", "KB4012212", "KB4012213", "KB4012214", "KB4012215", "KB4012216", "KB4012217", "KB4012218", "KB4012220", "KB4012598", "KB4012606", "KB4013198", "KB4013389", "KB4013429", "KB4015217", "KB4015438", "KB4015546", "KB4015547", "KB4015548", "KB4015549", "KB4015550", "KB4015551", "KB4015552", "KB4015553", "KB4015554", "KB4016635", "KB4019213", "KB4019214", "KB4019215", "KB4019216", "KB4019263", "KB4019264", "KB4019472", "KB4015221", "KB4019474", "KB4015219", "KB4019473"

    #checks the computer it's run on if any of the listed hotfixes are present
    $hotfix = Get-HotFix -ComputerName $env:computername | Where-Object {$hotfixes -contains $_.HotfixID} | Select-Object -property "HotFixID"

    #confirms whether hotfix is found or not
    if (Get-HotFix | Where-Object {$hotfixes -contains $_.HotfixID})
    {
        "Found HotFix: " + $hotfix.HotFixID
    } else {
        "Didn't Find HotFix"
    }
}

CheckForMS17-010

```



## <a name="related-links"></a>Verwante koppelingen
* [Azure AD Connect Health (Engelstalig)](active-directory-aadconnect-health.md)
* [De Azure AD Connect Health-agent installeren](active-directory-aadconnect-health-agent-install.md)
* [Azure AD Connect Health-bewerkingen](active-directory-aadconnect-health-operations.md)
* [Azure AD Connect Health gebruiken met AD FS](active-directory-aadconnect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](active-directory-aadconnect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](active-directory-aadconnect-health-adds.md)
* [Versiegeschiedenis van Azure AD Connect Health](active-directory-aadconnect-health-version-history.md)

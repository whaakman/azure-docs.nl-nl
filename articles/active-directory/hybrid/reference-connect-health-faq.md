---
title: Azure Active Directory Connect Health FAQ - Azure | Microsoft Docs
description: Deze Veelgestelde vragen vindt u antwoorden op vragen over Azure AD Connect Health. In deze Veelgestelde vragen worden vragen behandeld over het gebruik van de service, zoals het factureringsmodel, de mogelijkheden, beperkingen en ondersteuning.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: curtand
ms.assetid: f1b851aa-54d7-4cb4-8f5c-60680e2ce866
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 07/18/2017
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: ec88caafa9a6168860a8e9e2ff9e2abe0cfd0e77
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "57852956"
---
# <a name="azure-ad-connect-health-frequently-asked-questions"></a>Veelgestelde vragen over Azure AD Connect Health
In dit artikel bevat antwoorden op veelgestelde vragen over Azure Active Directory (Azure AD) Connect Health. Deze Veelgestelde vragen hebben betrekking op vragen over het gebruik van de service, waaronder het facturering model, mogelijkheden, beperkingen en ondersteuning.

## <a name="general-questions"></a>Algemene vragen
**V: Kan ik beheren meerdere Azure AD-directory's. Hoe kan ik schakelen naar de met Azure Active Directory Premium?**

Schakelen tussen de verschillende Azure AD-tenants, selecteert u de momenteel aangemelde **gebruikersnaam** in de rechterbovenhoek hoek en kies vervolgens het gewenste account. Als het account hier niet wordt weergegeven, selecteert u **Afmelden**, en gebruik vervolgens de referenties van de globale beheerder van de directory met Azure Active Directory Premium aan te melden bij ingeschakeld.

**V: Welke versie van de identiteit rollen worden ondersteund door Azure AD Connect Health?**

De volgende tabel geeft een lijst van de rollen en versies van besturingssystemen ondersteund.

|Rol| Besturingssysteem / versie|
|--|--|
|Active Directory Federation Services (AD FS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|
|Azure AD Connect | Versie 1.0.9125 of hoger|
|Active Directory Domain Services (AD DS)| <ul> <li> Windows Server 2008 R2 </li><li> Windows Server 2012  </li> <li>Windows Server 2012 R2 </li> <li> Windows Server 2016  </li> </ul>|

Houd er rekening mee dat de functies van de service op basis van de rol en het besturingssysteem verschillen kunnen. Met andere woorden, alle functies mogelijk niet beschikbaar voor alle versies van besturingssystemen. Zie de functiebeschrijvingen van de voor meer informatie.

**V: Hoeveel licenties heb ik nodig voor het bewaken van mijn infrastructuur?**

* De eerste Connect Health-Agent is ten minste één Azure AD Premium-licentie vereist.
* Voor elke extra geregistreerde agent vereist 25 extra Azure AD Premium-licenties.
* Aantal agents is gelijk aan het totale aantal agents die zijn geregistreerd voor alle bewaakte rollen (AD FS, Azure AD Connect en/of AD DS).
* AAD Connect Health-licentieverlening vereist niet dat u de licentie toewijzen aan specifieke gebruikers. U hoeft alleen het vereiste aantal geldige licenties hebt.

Licentie-informatie is ook gevonden op de [pagina met prijzen voor Azure AD](https://aka.ms/aadpricing).

Voorbeeld:

| Geregistreerde agents | Benodigde licenties | Configuratie van de voorbeeld-bewaking |
| ------ | --------------- | --- |
| 1 | 1 | 1 azure AD Connect-server |
| 2 | 26| 1 azure AD Connect-server en 1-domeincontroller |
| 3 | 51 | 1 active Directory Federation Services (AD FS)-server, 1 AD FS-proxy en 1-domeincontroller |
| 4 | 76 | 1 AD FS-server, 1 AD FS-proxy en 2 domeincontrollers |
| 5 | 101 | 1 azure AD Connect-server, 1 AD FS-server 1 AD FS-proxy en 2 domeincontrollers |

**V: Azure AD Connect Health biedt ondersteuning voor Azure-Cloud voor Duitsland?**

Azure AD Connect Health wordt niet ondersteund in de Duitse Cloud, behalve voor de [synchronisatiefunctie fouten rapport](how-to-connect-health-sync.md#object-level-synchronization-error-report).

| Rollen | Functies | Ondersteund in de Duitse Cloud |
| ------ | --------------- | --- |
| Connect Health for Sync | Bewaking / inzicht / waarschuwingen / analyse | Nee |
|  | Rapport over synchronisatiefouten | Ja |
| Connect Health voor AD FS | Bewaking / inzicht / waarschuwingen / analyse | Nee |
| Connect Health voor ADDS | Bewaking / inzicht / waarschuwingen / analyse | Nee |

Om te controleren of de agent met de connectiviteit van Connect Health voor synchroniseren, configureer de [installatievereiste](how-to-connect-health-agent-install.md#outbound-connectivity-to-the-azure-service-endpoints) dienovereenkomstig.

## <a name="installation-questions"></a>Vragen over de installatie

**V: Wat zijn de gevolgen van de Azure AD Connect Health-Agent installeren op afzonderlijke servers?**

De impact van het installeren van de Microsoft Azure AD Connect Health-Agent, de AD FS, de WAP-servers, de Azure AD Connect (sync)-servers, domeincontrollers is minimale met betrekking tot de CPU, geheugengebruik, bandbreedte van het netwerk en opslag.

De volgende getallen zijn een benadering:

* CPU-verbruik: ~ 1-5% verhogen.
* Geheugengebruik: Maximaal 10% van de totale systeemgeheugen.

> [!NOTE]
> Als de agent kan niet met Azure communiceren, worden de gegevens voor een gedefinieerde maximumlimiet lokaal opgeslagen in de agent. De agent worden de gegevens 'in de cache' op basis van "minst recentelijk onderhouden" overschreven.
>
>

* Lokale buffer opslag voor Azure AD Connect Health-Agents: ~ 20 MB.
* Voor AD FS-servers raden wij het inrichten van een schijfruimte van 1024 MB (1 GB) voor het kanaal voor het controleren van AD FS voor Azure AD Connect Health-Agents voor het verwerken van de controlegegevens voordat deze wordt overschreven.

**V: Ik moet mijn servers opnieuw tijdens de installatie van de Azure AD Connect Health-Agents?**

Nee. De server opnieuw opstarten is niet nodig voor de installatie van de agents zijn vereist. Installatie van bepaalde vereiste stappen moet echter een opnieuw opstarten van de server.

Installatie van .NET-Framework 4.5 vereist bijvoorbeeld op Windows Server 2008 R2, server opnieuw opstarten.

**V: Werkt de Azure AD Connect Health via een Pass Through-HTTP-proxy?**

Ja. U kunt de Health-Agent voor het gebruik van een HTTP-proxy voor het doorsturen van uitgaande HTTP-aanvragen te configureren voor lopende bewerkingen.
Meer informatie over [HTTP-Proxy voor de Health-Agents configureren](how-to-connect-health-agent-install.md#configure-azure-ad-connect-health-agents-to-use-http-proxy).

Als u een proxy configureren tijdens de registratie van de agent wilt, moet u mogelijk vooraf de Internet Explorer-Proxy-instellingen wijzigen.

1. Open Internet Explorer > **instellingen** > **Internetopties** > **verbindingen** > **LAN-instellingen** .
2. Selecteer **een proxyserver gebruiken voor uw LAN**.
3. Selecteer **Geavanceerd** hebt u verschillende proxyservers poorten voor HTTP en HTTPS/Secure.

**V: Ondersteunt Azure AD Connect Health basisverificatie wordt gebruikt bij het verbinden met HTTP-proxy's?**

Nee. Een mechanisme om op te geven van een willekeurige naam en het wachtwoord voor basisverificatie is momenteel niet ondersteund.

**V: Welke firewall-poorten moet ik openen voor de Azure AD Connect Health-Agent om te werken?**

Zie de [gedeelte vereisten](how-to-connect-health-agent-install.md#requirements) voor een lijst van firewall-poorten en andere vereisten voor connectiviteit.

**V: Waarom zie ik twee servers met dezelfde naam in de portal voor Azure AD Connect Health?**

Wanneer u een agent van een server verwijderen, wordt de server niet automatisch verwijderd uit de Azure AD Connect Health-portal. Als u handmatig een agent van een server verwijderen of de server zelf verwijderen, moet u de serververmelding handmatig verwijderen van de Azure AD Connect Health-portal.

U kunt de installatiekopie van een server terugzetten of een nieuwe server maken met de dezelfde informatie (zoals de naam van de machine). Als u de reeds geregistreerde server niet hebt verwijderd uit de Azure AD Connect Health-portal en de agent op de nieuwe server hebt geïnstalleerd, ziet u mogelijk twee vermeldingen met dezelfde naam.

In dit geval wordt de vermelding die deel uitmaakt van de oude server handmatig verwijderen. De gegevens voor deze server moet zijn verouderd.

## <a name="health-agent-registration-and-data-freshness"></a>Registratie en gegevens nieuwheid van Health-Agent

**V: Wat zijn veelvoorkomende redenen voor de Health-Agent-registratiefouten en hoe los ik problemen?**

De health-agent kan niet worden geregistreerd door de volgende mogelijke oorzaken:

* De agent kan niet communiceren met de vereiste eindpunten omdat verkeer wordt geblokkeerd door een firewall. Dit geldt met name op de WAP-servers. Zorg ervoor dat u uitgaande communicatie naar de vereiste eindpunten en de poorten zijn toegestaan. Zie de [gedeelte vereisten](how-to-connect-health-agent-install.md#requirements) voor meer informatie.
* Uitgaande communicatie is door de netwerklaag een SSL-inspectie onderworpen. Dit zorgt ervoor dat het certificaat die gebruikmaakt van de agent moet worden vervangen door de controle-server/entiteit en de stappen voor het voltooien van de registratie van de agent mislukken.
* De gebruiker heeft geen toegang tot het uitvoeren van de registratie van de agent. Globale beheerders hebben standaard toegang. U kunt [rollen gebaseerd toegangsbeheer](how-to-connect-health-operations.md#manage-access-with-role-based-access-control) voor toegang tot andere gebruikers delegeren.

**V: Ik ben ophalen gewaarschuwd dat 'Health-servicegegevens is niet up-to-date." Hoe kan ik het probleem oplossen?**

Azure AD Connect Health, wordt er een waarschuwing gegenereerd wanneer deze niet alle gegevenspunten van de server in de afgelopen twee uur ontvangen worden. [Meer informatie](how-to-connect-health-data-freshness.md).

## <a name="operations-questions"></a>Operations-vragen
**V: Heb ik nodig om te controleren voor de WAP-servers inschakelen?**

Nee, controle hoeft niet te worden ingeschakeld op de WAP-servers.

**V: Hoe Azure AD Connect Health waarschuwingen opgelost?**

Waarschuwingen van Azure AD Connect Health worden opgelost op een voorwaarde geslaagd. Azure AD Connect Health-Agents detecteren en de voorwaarden succes periodiek rapporteren aan de service. Voor enkele waarschuwingen is de onderdrukking op basis van tijd. Met andere woorden, als het hetzelfde probleem niet binnen 72 uur van het genereren van waarschuwingen is waargenomen, de waarschuwing automatisch opgelost.

**V: Ik ben ophalen gewaarschuwd dat 'Test-verificatieaanvraag (synthetische transactie) kan niet een token verkrijgen." Hoe kan ik het probleem oplossen?**

Azure AD Connect Health voor AD FS wordt deze waarschuwing gegenereerd wanneer de Health-Agent geïnstalleerd op een AD FS-server niet kan een token verkrijgen als onderdeel van een synthetische transactie is gestart door de Health-Agent. De Health-agent maakt gebruik van de lokale system-context en probeert op te halen van een token voor een relying party zelf. Dit is een catch-all-test om ervoor te zorgen dat AD FS heeft de status van het uitgeven van tokens.

Deze test mislukt meestal omdat de Health-Agent niet kan omzetten van de naam van de AD FS-farm. Dit kan gebeuren als de AD FS-servers achter een load balancers voor het netwerk en de aanvraag wordt gestart vanaf een knooppunt dat achter de load balancer (in plaats van een reguliere-client die is voor de load balancer). Dit kan worden hersteld door het bijwerken van het Hostsbestand '' onder 'C:\Windows\System32\drivers\etc"het IP-adres van de AD FS-server of een loopback-IP-adres (127.0.0.1) voor de naam van de AD FS-farm (bijvoorbeeld sts.contoso.com) op te nemen. Het bestand met de host toe te voegen, wordt de netwerkaanroep, waardoor de Health-Agent om op te halen van het token kortsluiting.

**V: Er treedt een e-mailbericht dat aangeeft dat mijn machines zijn niet gevuld voor de recente aanvallen met ransomware. Waarom krijg ik dit e-mailbericht?**

Azure AD Connect Health-service gescand alle computers die wordt bewaakt om te controleren of de vereiste patches zijn geïnstalleerd. Het e-mailbericht is verzonden naar de tenantbeheerders als ten minste één computer beschikt niet over de kritieke patches. De volgende logica is gebruikt om dit te bepalen.
1. Alle hotfixes die zijn geïnstalleerd op de machine vinden.
2. Controleer of ten minste één van de HotFixes in de lijst met gedefinieerde aanwezig is.
3. Als u Ja kiest, wordt de machine is beveiligd. Als dat niet het geval is, moet u de machine loopt het risico voor de aanval.

U kunt de volgende PowerShell-script gebruiken om uit te voeren van deze controle handmatig. De bovenstaande logica geïmplementeerd.

```powershell
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

**V: Waarom wordt de PowerShell-cmdlet <i>Get-MsolDirSyncProvisioningError</i> minder synchronisatiefouten in het resultaat weergeven?**

<i>Get-MsolDirSyncProvisioningError</i> retourneert alleen fouten bij het inrichten DirSync. Behalve dat bevat Connect Health-portal ook andere synchronisatiecomponenten fouttypen zoals exportfouten. Dit komt overeen met de Azure AD Connect delta-resultaat. Meer informatie over [Azure AD Connect Sync fouten](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-troubleshoot-sync-errors).

**V: Waarom wordt mijn ADFS controles zijn uitgevoerd niet wordt gegenereerd?**

Gebruik PowerShell-cmdlet <i>Get-AdfsProperties - AuditLevel</i> uitgeschakeld om te controleren of de logboeken voor controle is niet in staat. Meer informatie over [AD FS-auditlogboeken](https://docs.microsoft.com/windows-server/identity/ad-fs/technical-reference/auditing-enhancements-to-ad-fs-in-windows-server#auditing-levels-in-ad-fs-for-windows-server-2016). U ziet dat als er geavanceerde controle-instellingen naar de ADFS-server gepusht, worden eventuele wijzigingen in auditpol.exe overschreven (gebeurtenis als toepassing gegenereerd niet is geconfigureerd). Stel in dat geval wordt het lokale beveiligingsbeleid om aan te melden fouten toepassing gegenereerd en het slagen.

**V: Wanneer worden certificaat van de agent automatisch vernieuwd voordat verlopen?**
De certificering van de agent wordt automatisch worden vernieuwd **6 maanden** vóór de vervaldatum. Als u deze niet verlengt, zorg ervoor dat de netwerkverbinding van de agent is stabiel. Start de agent-services of update van de meest recente versie kan het probleem ook oplossen.


## <a name="related-links"></a>Verwante koppelingen
* [Azure AD Connect Health (Engelstalig)](whatis-hybrid-identity-health.md)
* [De Azure AD Connect Health-agent installeren](how-to-connect-health-agent-install.md)
* [Azure AD Connect Health-bewerkingen](how-to-connect-health-operations.md)
* [Azure AD Connect Health gebruiken met AD FS](how-to-connect-health-adfs.md)
* [Azure AD Connect Health for Sync gebruiken](how-to-connect-health-sync.md)
* [Azure AD Connect Health gebruiken met AD DS](how-to-connect-health-adds.md)
* [Versiegeschiedenis van Azure AD Connect Health](reference-connect-health-version-history.md)

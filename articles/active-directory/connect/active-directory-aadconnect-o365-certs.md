---
title: Certificaat vernieuwen voor Office 365 en Azure AD-gebruikers | Microsoft Docs
description: In dit artikel wordt uitgelegd aan Office 365-gebruikers over het oplossen van problemen met e-mailberichten die ze hierover te informeren over het vernieuwen van een certificaat.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: curtand
ms.assetid: 543b7dc1-ccc9-407f-85a1-a9944c0ba1be
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: 4de24608ba9db174f343bf0d78029913e4b7868f
ms.sourcegitcommit: 7ad9db3d5f5fd35cfaa9f0735e8c0187b9c32ab1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39325680"
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Federatiecertificaten vernieuwen voor Office 365 en Azure Active Directory
## <a name="overview"></a>Overzicht
Geslaagde federatie tussen Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS), de certificaten die zijn gebruikt door AD FS voor het ondertekenen van beveiligingstokens aan Azure AD moeten overeenkomen met wat in Azure AD is geconfigureerd. Niet overeenkomen, kan leiden tot verbroken vertrouwensrelatie. Azure AD zorgt ervoor dat deze gegevens synchroon worden gehouden bij het implementeren van AD FS en Web Application Proxy (voor toegang tot het extranet).

Dit artikel vindt u aanvullende informatie voor het beheren van uw token handtekeningcertificaten en synchroniseren met Azure AD in de volgende gevallen:

* Implementeert u de Web Application Proxy en de federatiemetagegevens is daarom niet beschikbaar in het extranet.
* U de standaardconfiguratie van AD FS niet gebruikt voor token-ondertekening van certificaten.
* U kunt een externe id-provider worden gebruikt.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Standaardconfiguratie van AD FS voor token-ondertekening van certificaten
Het token-ondertekening en certificaten decoderen-token zijn meestal zelfondertekende certificaten en geschikt zijn voor één jaar. Standaard AD FS bevat een automatische verlenging-proces met de naam **AutoCertificateRollover**. Als u AD FS 2.0 of hoger, Office 365 en Azure AD automatisch bijwerken van uw certificaat voordat deze verloopt.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Vernieuwen van meldingen vanuit de Office 365-portal of een e-mailbericht
> [!NOTE]
> Als u een e-mailadres of een portalmelding of u wilt een certificaat vernieuwen voor Office, Zie ontvangen [beheren van wijzigingen naar certificaten voor tokenondertekening](#managecerts) om te controleren als u geen actie te ondernemen. Microsoft is op de hoogte van een mogelijk probleem die tot meldingen voor certificaatvernieuwing wordt verzonden leiden kan, zelfs wanneer er is geen actie vereist.
>
>

Azure AD wordt geprobeerd om te controleren van de federatiemetagegevens en bijwerken van het token-ondertekening van certificaten, zoals aangegeven door deze metagegevens. 30 dagen vóór de vervaldatum van token-ondertekening van certificaten, controleert Azure AD of er nieuwe certificaten beschikbaar zijn door de federatiemetagegevens polling.

* Als deze kan pollen van de federatiemetagegevens en de nieuwe certificaten ophalen, wordt geen e-mailmelding of waarschuwingen in de Office 365-beheerportal verleend aan de gebruiker.
* Als deze geen nieuwe token-ondertekening van certificaten ophalen, ofwel omdat de federatiemetagegevens is niet bereikbaar is of automatische certificaatrollover niet is ingeschakeld, omdat Azure AD geeft een e-mailbericht en een waarschuwing in de Office 365-beheerportal.

![Office 365-portalmelding](./media/active-directory-aadconnect-o365-certs/notification.png)

> [!IMPORTANT]
> Als u AD FS wordt gebruikt voor bedrijfscontinuïteit te waarborgen, Controleer of dat uw servers beschikken over de volgende updates zodat verificatiefouten voor bekende problemen, niet wordt uitgevoerd. Dit vermindert het probleem van bekende problemen met AD FS proxy-server voor deze vernieuwen en van toekomstige vernieuwingsperioden:
>
> Server 2012 R2 - [WindowsServer mei 2014 samenvouwen](http://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 en 2012 - [verificatie via proxy is mislukt in Windows Server 2012 of Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)
>
>

## Controleer of de certificaten moeten worden bijgewerkt <a name="managecerts"></a>
### <a name="step-1-check-the-autocertificaterollover-state"></a>Stap 1: Controleer de status van de AutoCertificateRollover
Open PowerShell op uw AD FS-server. Controleer of de waarde AutoCertificateRollover is ingesteld op True.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

>[!NOTE] 
>Als u van AD FS 2.0 gebruikmaakt, moet u eerst Add-Pssnapin Microsoft.Adfs.Powershell uitvoeren.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Stap 2: Controleer of AD FS en Azure AD gesynchroniseerd zijn
Open de MSOnline PowerShell-prompt op uw AD FS-server en verbinding maken met Azure AD.

> [!NOTE]
> MSOL-Cmdlets zijn onderdeel van de MSOnline PowerShell-module.
> U kunt de MSOnline PowerShell-Module downloaden rechtstreeks vanuit de PowerShell Gallery.
> 
>

    Install-Module MSOnline

Verbinding maken met Azure AD met behulp van de MSOnline PowerShell-Module.

    Import-Module MSOnline
    Connect-MsolService

Controleer de certificaten die zijn geconfigureerd in AD FS en Azure AD-eigenschappen voor het opgegeven domein vertrouwen.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Als de vingerafdrukken in zowel de uitvoer overeenkomen, is uw certificaten zijn gesynchroniseerd met Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Stap 3: Controleren of het certificaat verloopt binnenkort
Schakel in de uitvoer van Get-MsolFederationProperty of Get-AdfsCertificate, voor de datum bij "Niet na." Als de datum minder dan 30 dagen opgeslagen is, moet u actie ondernemen.

| AutoCertificateRollover | Met Azure AD gesynchroniseerde certificaten | Federatiemetagegevens is openbaar toegankelijk | Geldigheidsduur | Bewerking |
|:---:|:---:|:---:|:---:|:---:|
| Ja |Ja |Ja |- |Er is geen actie nodig. Zie [automatisch vernieuwen token-ondertekening certificaat](#autorenew). |
| Ja |Nee |- |Minder dan 15 dagen |Onmiddellijk vernieuwen. Zie [handmatig vernieuwen token-ondertekening certificaat](#manualrenew). |
| Nee |- |- |Minder dan 30 dagen |Onmiddellijk vernieuwen. Zie [handmatig vernieuwen token-ondertekening certificaat](#manualrenew). |

\[-] Maakt niet uit

## Het token-ondertekening certificaat automatisch vernieuwen (aanbevolen) <a name="autorenew"></a>
U moet geen eventuele handmatige stappen uitvoeren als beide van de volgende waar zijn:

* Web Application Proxy, die toegang tot de federatiemetagegevens van het extranet kunt inschakelen als u hebt geïmplementeerd.
* U gebruikt de standaardconfiguratie van de AD FS (AutoCertificateRollover is ingeschakeld).

Controleer het volgende om te bevestigen dat het certificaat automatisch kan worden bijgewerkt.

**1. De AD FS-eigenschap AutoCertificateRollover moet worden ingesteld op True.** Hiermee wordt aangegeven dat AD FS genereert automatisch nieuwe token-ondertekening en certificaten voor token-decodering, voordat u de oude zijn verlopen.

**2. De AD FS-federatiemetagegevens is openbaar toegankelijk.** Controleer of de federatiemetagegevens openbaar toegankelijk is door te navigeren naar de volgende URL vanaf een computer op het openbare internet (buiten het bedrijfsnetwerk):

https://(your_FS_name)/federationmetadata/2007-06/federationmetadata.xml

waar `(your_FS_name) `wordt vervangen door de hostnaam van de federation-service gebruikmaakt van uw organisatie, zoals fs.contoso.com.  Als u kunt om te controleren of beide van deze instellingen is, u niet hoeft te doen.  

Voorbeeld: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml
## Het token-ondertekening certificaat handmatig vernieuwen <a name="manualrenew"></a>
U kunt kiezen om het token handtekeningcertificaten handmatig te verlengen. Bijvoorbeeld, de volgende scenario's werken mogelijk beter voor handmatig verlengen:

* Token handtekeningcertificaten zijn geen zelfondertekende certificaten. De meest voorkomende reden hiervoor is dat AD FS-certificaten die zijn geregistreerd bij een organisatie-certificeringsinstantie wordt beheerd door uw organisatie.
* Beveiliging van het netwerk is niet toegestaan voor de federatiemetagegevens moet openbaar beschikbaar.

In deze scenario's telkens wanneer u het token bijwerkt-ondertekening van certificaten, moet u ook bijwerken uw Office 365-domein met behulp van de PowerShell-opdracht, Update-MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Stap 1: Zorg ervoor dat AD FS heeft nieuwe token handtekeningcertificaten
**Niet-standaard-configuratie**

Als u een niet-standaard-configuratie van AD FS (waarbij **AutoCertificateRollover** is ingesteld op **False**), gebruikt u waarschijnlijk aangepaste certificaten (niet zelf ondertekend). Zie voor meer informatie over het vernieuwen van de AD FS-token handtekeningcertificaten [richtlijnen voor klanten die AD FS niet gebruikt zelfondertekende certificaten](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federatiemetagegevens is niet openbaar beschikbaar**

Aan de andere kant als **AutoCertificateRollover** is ingesteld op **waar**, maar uw federatiemetagegevens is niet openbaar toegankelijk is, eerst voor zorgen dat nieuwe certificaten voor tokenondertekening zijn gegenereerd door de AD FS. Controleer of er nieuwe token-ondertekening van certificaten door de volgende stappen uit:

1. Controleer of u bent aangemeld met de primaire AD FS-server.
2. Controleer de huidige handtekeningcertificaten in AD FS door een PowerShell-opdrachtvenster openen en het uitvoeren van de volgende opdracht uit:

    PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

   > [!NOTE]
   > Als u van AD FS 2.0 gebruikmaakt, moet u eerst Add-Pssnapin Microsoft.Adfs.Powershell uitvoeren.
   >
   >
3. Bekijk de uitvoer van de opdracht op alle certificaten die worden vermeld. Als u AD FS is een nieuw certificaat gegenereerd, ziet u twee certificaten in de uitvoer: een waarvoor de **IsPrimary** waarde is **waar** en de **NotAfter** datum valt binnen vijf dagen , en één waarvoor **IsPrimary** is **False** en **NotAfter** is over een jaar in de toekomst.
4. Als er slechts één certificaat en de **NotAfter** datum valt binnen vijf dagen, moet u een nieuw certificaat genereren.
5. Als u wilt een nieuw certificaat genereren, dan de volgende opdracht achter de PowerShell-opdrachtprompt: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Controleer of de update door de volgende opdracht nogmaals uit te voeren: PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

Twee certificaten moeten nu worden weergegeven, die een **NotAfter** datum van ongeveer één jaar in de toekomst, en waarvoor de **IsPrimary** waarde **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Stap 2: Het nieuwe token-ondertekening van certificaten voor de Office 365-vertrouwensrelatie bijwerken
Office 365 met het nieuwe token-ondertekening van certificaten moet worden gebruikt voor de vertrouwensrelatie als volgt bijwerken.

1. Open de Microsoft Azure Active Directory-Module voor Windows PowerShell.
2. Voer $cred = Get-Credential. Wanneer deze cmdlet u om referenties vraagt, typt u uw referenties met administrator-account voor cloud-service.
3. Connect-MsolService uitvoeren: $cred van referenties. Deze cmdlet maakt u verbinding met de cloudservice. Het maken van een context die u maakt verbinding met de cloudservice is vereist voordat u een van de aanvullende cmdlets geïnstalleerd door het hulpprogramma uitvoert.
4. Als u deze opdrachten op een computer die niet de primaire AD FS-federatieserver uitvoert, voert u Set-MSOLAdfscontext-Computer &lt;primaire AD FS-server&gt;, waarbij &lt;primaire AD FS-server&gt; is de interne FQDN-naam de naam van de primaire AD FS-server. Met deze cmdlet maakt een context die u met AD FS verbindt.
5. Voer Update-MSOLFederatedDomain – DomainName &lt;domein&gt;. Deze cmdlet worden de instellingen van AD FS in de cloudservice-updates en configureert u de vertrouwensrelatie tussen de twee.

> [!NOTE]
> Als u nodig hebt ter ondersteuning van meerdere domeinen op het hoogste niveau, zoals contoso.com en fabrikam.com, moet u de **SupportMultipleDomain** overschakelen met cmdlets. Zie voor meer informatie, [ondersteuning voor meerdere domeinen van het hoogste niveau](active-directory-aadconnect-multiple-domains.md).
>


## Azure AD-vertrouwensrelatie herstellen met behulp van Azure AD Connect <a name="connectrenew"></a>
Als u uw AD FS-farm en Azure AD-vertrouwensrelatie met behulp van Azure AD Connect hebt geconfigureerd, kunt u Azure AD Connect kunt gebruiken om te detecteren als u geen actie voor uw token handtekeningcertificaten te ondernemen. Als u de certificaten vernieuwen wilt, kunt u Azure AD Connect kunt gebruiken om dit te doen.

Zie voor meer informatie, [herstellen van de vertrouwensrelatie](active-directory-aadconnect-federation-management.md).

## <a name="ad-fs-and-azure-ad-certificate-update-steps"></a>AD FS en Azure AD-certificaten updatestappen
Token handtekeningcertificaten worden standaard X509 certificaten die worden gebruikt voor het ondertekenen van veilig alle tokens die de federatieserver heeft uitgegeven. Certificaten voor token-decodering zijn standaard X509 certificaten die worden gebruikt voor het ontsleutelen van eventuele binnenkomend tokens. 

AD FS is standaard geconfigureerd automatisch genereren van certificaten voor token-decodering en token-ondertekening, zowel op het moment van de eerste configuratie en wanneer de certificaten de vervaldatum nadert.

Azure AD probeert op te halen van een nieuw certificaat uit de metagegevens van uw federation service 30 dagen vóór de vervaldatum van het huidige certificaat. Een nieuw certificaat op dat moment niet beschikbaar is, blijft Azure AD voor het bewaken van de metagegevens van de dagelijkse regelmatig. Zodra het nieuwe certificaat in de metagegevens beschikbaar is, wordt de federatie-instellingen voor het domein worden bijgewerkt met de gegevens van het nieuwe certificaat. U kunt `Get-MsolDomainFederationSettings` om te controleren als u het nieuwe certificaat in de NextSigningCertificate ziet / SigningCertificate.

Zie voor meer informatie over het Token-ondertekening certificaten in AD FS [verkrijgen en configureren van Token-ondertekening en Token Ontsleutelingscertificaten voor AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ts-td-certs-ad-fs)

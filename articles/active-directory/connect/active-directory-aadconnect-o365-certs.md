---
title: Certificaat vernieuwen voor Office 365 en Azure AD-gebruikers | Microsoft Docs
description: Dit artikel wordt uitgelegd voor Office 365-gebruikers het oplossen van problemen met e-mailberichten die gebruikers informeren over het vernieuwen van een certificaat.
services: active-directory
documentationcenter: 
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
ms.author: billmath
ms.openlocfilehash: a0e3b65c108f8d839b8107e98a5cd59df78e1ab0
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="renew-federation-certificates-for-office-365-and-azure-active-directory"></a>Federatiecertificaten vernieuwen voor Office 365 en Azure Active Directory
## <a name="overview"></a>Overzicht
Geslaagde federatie tussen Azure Active Directory (Azure AD) en Active Directory Federation Services (AD FS), de certificaten die door AD FS wordt gebruikt voor het ondertekenen van beveiligingstokens aan Azure AD moeten overeenkomen met wat in Azure AD is geconfigureerd. Niet overeenkomen, kan leiden tot verbroken vertrouwensrelatie. Azure AD zorgt ervoor dat deze informatie wordt gesynchroniseerd bij het implementeren van AD FS en Webtoepassingsproxy (voor toegang tot het extranet).

Dit artikel vindt u aanvullende informatie over het beheren van uw token-ondertekening van certificaten en gesynchroniseerd met Azure AD houden in de volgende gevallen:

* U implementeert niet de Webtoepassingsproxy en de federatiemetagegevens is daarom niet beschikbaar in het extranet.
* U de standaardconfiguratie van AD FS niet gebruikt voor token-ondertekening van certificaten.
* U gebruikt een derde partij id-provider.

## <a name="default-configuration-of-ad-fs-for-token-signing-certificates"></a>Standaardconfiguratie van AD FS voor token-ondertekening van certificaten
De token-ondertekening en token-ontsleuteling van certificaten worden meestal zelfondertekende certificaten en geschikt zijn voor één jaar. AD FS bevat standaard een proces dat automatisch verlengen wordt genoemd **AutoCertificateRollover**. Als u AD FS 2.0 of hoger, Office 365 en Azure AD automatisch bijwerken van uw certificaat voordat deze verloopt.

### <a name="renewal-notification-from-the-office-365-portal-or-an-email"></a>Vernieuwing melding van de Office 365-portal of een e-mailbericht
> [!NOTE]
> Als u een e-mailadres of een vraag of u een certificaat vernieuwen voor Office, raadpleeg dan portalmelding ontvangen [beheren van wijzigingen naar certificaten voor tokenondertekening](#managecerts) om te controleren als u geen actie te ondernemen. Microsoft is op de hoogte van een mogelijk probleem dat tot meldingen voor certificaatvernieuwing wordt verzonden leiden kan, zelfs wanneer er is geen actie vereist.
>
>

Azure AD probeert om te controleren van de federatiemetagegevens en bijwerken van het token-ondertekening van certificaten, zoals aangegeven door deze metagegevens. 30 dagen vóór de vervaldatum van het token-ondertekening van certificaten, Azure AD gecontroleerd of er nieuwe certificaten beschikbaar zijn door de federatiemetagegevens.

* Als deze kan pollen van de federatiemetagegevens en de nieuwe certificaten ophalen, is geen e-mailmeldingen of waarschuwingen in de Office 365-portal aan de gebruiker worden verstrekt.
* Als deze kan niet van het nieuwe token ophalen-ondertekening van certificaten, ofwel omdat de federatiemetagegevens kan niet worden bereikt of automatische certificaataanvraag rollover niet is ingeschakeld, Azure AD geeft een e-mailbericht en een waarschuwing in de Office 365-beheerportal.

![Office 365-portal melding](./media/active-directory-aadconnect-o365-certs/notification.png)

> [!IMPORTANT]
> Als u AD FS worden gebruikt voor de bedrijfscontinuïteit te waarborgen, Controleer of uw servers hebt de volgende updates zodat verificatiefouten voor bekende problemen wordt niet bijgewerkt. Dit vermindert bekende problemen van AD FS proxy-server voor deze vernieuwing en toekomstige vernieuwingsperioden:
>
> Server 2012 R2 - [Windows Server mei 2014 updatepakket](http://support.microsoft.com/kb/2955164)
>
> Server 2008 R2 en 2012 - [verificatie via proxy is mislukt in Windows Server 2012 of Windows 2008 R2 SP1](http://support.microsoft.com/kb/3094446)
>
>

## Controleer of de certificaten moeten worden bijgewerkt<a name="managecerts"></a>
### <a name="step-1-check-the-autocertificaterollover-state"></a>Stap 1: Controleer de status van de AutoCertificateRollover
Open PowerShell op uw AD FS-server. Controleer of de waarde AutoCertificateRollover is ingesteld op True.

    Get-Adfsproperties

![AutoCertificateRollover](./media/active-directory-aadconnect-o365-certs/autocertrollover.png)

>[!NOTE] 
>Als u AD FS 2.0 gebruikt, moet u eerst Add-Pssnapin Microsoft.Adfs.Powershell uitvoeren.

### <a name="step-2-confirm-that-ad-fs-and-azure-ad-are-in-sync"></a>Stap 2: Controleer of de AD FS en Azure AD gesynchroniseerd zijn
Open de Azure AD PowerShell-prompt op uw AD FS-server en verbinding maken met Azure AD.

> [!NOTE]
> U kunt downloaden via Azure AD PowerShell [hier](https://technet.microsoft.com/library/jj151815.aspx).
>
>

    Connect-MsolService

Controleer de certificaten zijn geconfigureerd in AD FS en Azure AD-eigenschappen voor het opgegeven domein vertrouwen.

    Get-MsolFederationProperty -DomainName <domain.name> | FL Source, TokenSigningCertificate

![Get-MsolFederationProperty](./media/active-directory-aadconnect-o365-certs/certsync.png)

Als de vingerafdrukken in zowel de uitvoer overeenkomen, is uw certificaten zijn gesynchroniseerd met Azure AD.

### <a name="step-3-check-if-your-certificate-is-about-to-expire"></a>Stap 3: Controleren of het certificaat verloopt
Schakel in de uitvoer van Get-MsolFederationProperty of Get-AdfsCertificate voor de datum onder "Niet na." Als de datum minder dan 30 dagen verwijderd is, moet u actie ondernemen.

| AutoCertificateRollover | Met Azure AD gesynchroniseerde certificaten | Federatiemetagegevens is openbaar toegankelijk | Geldigheid | Actie |
|:---:|:---:|:---:|:---:|:---:|
| Ja |Ja |Ja |- |Geen actie nodig. Zie [automatisch verlengen token-ondertekening certificaat](#autorenew). |
| Ja |Nee |- |Minder dan 15 dagen |Onmiddellijk vernieuwen. Zie [vernieuwen token-ondertekening handmatig van het certificaat](#manualrenew). |
| Nee |- |- |Minder dan 30 dagen |Onmiddellijk vernieuwen. Zie [vernieuwen token-ondertekening handmatig van het certificaat](#manualrenew). |

\[-] Maakt niet uit.

## Het token-ondertekening certificaat automatisch vernieuwen (aanbevolen)<a name="autorenew"></a>
U hoeft niet alle stappen handmatig uitvoeren als het volgende waar zijn:

* U hebt geïmplementeerd Web Application Proxy, die toegang tot de federatiemetagegevens via het extranet inschakelen kunt.
* U gebruikt de standaardconfiguratie voor AD FS (AutoCertificateRollover is ingeschakeld).

Controleer het volgende om te bevestigen dat het certificaat automatisch kan worden bijgewerkt.

**1. De AD FS-eigenschap AutoCertificateRollover moet worden ingesteld op True.** Dit geeft aan dat AD FS automatisch gegenereerd nieuwe token-ondertekening en tokenontsleuteling certificaten, voordat de oude zijn verlopen.

**2. De metagegevens van de federatieve AD FS is openbaar toegankelijk.** Controleer of uw federatiemetagegevens openbaar toegankelijk is door te navigeren naar de volgende URL vanaf een computer in het openbare internet (buiten het bedrijfsnetwerk):

/federationmetadata/2007-06/federationmetadata.xml https:// (your_FS_name)

waar `(your_FS_name) `is vervangen door de hostnaam van de federation service gebruikmaakt van uw organisatie, zoals fs.contoso.com.  Als u kunt controleren of beide van deze instellingen zijn, er geen te doen.  

Voorbeeld: https://fs.contoso.com/federationmetadata/2007-06/federationmetadata.xml

## Het token-ondertekening certificaat handmatig vernieuwen<a name="manualrenew"></a>
U kunt kiezen om de token handtekeningcertificaten handmatig te verlengen. De volgende scenario's kunnen bijvoorbeeld beter werken voor handmatige vernieuwing:

* Token handtekeningcertificaten zijn geen zelfondertekende certificaten. De meest voorkomende reden hiervoor is dat AD FS-certificaten die zijn ingeschreven door een organisatie-certificeringsinstantie wordt beheerd door uw organisatie.
* Netwerkbeveiliging is niet toegestaan voor de federatiemetagegevens openbaar beschikbaar.

In deze scenario's, elke keer dat u bij het bijwerken van het token-ondertekening van certificaten, moet u ook bijwerken uw Office 365-domein met behulp van de PowerShell-opdracht Update MsolFederatedDomain.

### <a name="step-1-ensure-that-ad-fs-has-new-token-signing-certificates"></a>Stap 1: Zorg ervoor dat AD FS nieuwe token-ondertekening van certificaten
**Configuratie van de niet-standaard**

Als u een niet-standaard configuratie van AD FS (waarbij **AutoCertificateRollover** is ingesteld op **False**), gebruikt u waarschijnlijk aangepaste certificaten (niet zelf ondertekend). Zie voor meer informatie over het vernieuwen van de AD FS-token handtekeningcertificaten [richtlijnen voor klanten die gebruikmaken van AD FS niet zelfondertekend certificaten](https://msdn.microsoft.com/library/azure/JJ933264.aspx#BKMK_NotADFSCert).

**Federatiemetagegevens is niet openbaar beschikbaar**

Aan de andere kant als **AutoCertificateRollover** is ingesteld op **True**, maar uw federatiemetagegevens is niet openbaar toegankelijk, eerst voor zorgen dat nieuwe certificaten voor tokenondertekening zijn gegenereerd door AD FS. Bevestig hebt u een nieuw token-ondertekening van certificaten door de volgende stappen:

1. Controleer of u bent aangemeld met de primaire AD FS-server.
2. Controleer de huidige handtekeningcertificaten in AD FS door een PowerShell-opdrachtvenster openen en de volgende opdracht uit te voeren:

    PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

   > [!NOTE]
   > Als u AD FS 2.0 gebruikt, moet u eerst Add-Pssnapin Microsoft.Adfs.Powershell uitvoeren.
   >
   >
3. Bekijk de uitvoer van de opdracht op alle certificaten die worden vermeld. Als AD FS is een nieuw certificaat gegenereerd, ziet u twee certificaten in de uitvoer: een waarvoor de **IsPrimary** waarde is **True** en de **NotAfter** datum valt binnen vijf dagen , en één waarvoor **IsPrimary** is **False** en **NotAfter** staat op het punt een jaar in de toekomst.
4. Als er slechts één certificaat en de **NotAfter** datum valt binnen vijf dagen, moet u een nieuw certificaat genereren.
5. Als u wilt een nieuw certificaat genereren, voer de volgende opdracht achter de PowerShell-opdrachtprompt: `PS C:\>Update-ADFSCertificate –CertificateType token-signing`.
6. Controleren of de update opnieuw door de volgende opdracht: PS C:\>Get-ADFSCertificate – CertificateType token-ondertekening

Twee certificaten zou nu moeten worden vermeld, die een **NotAfter** datum van ongeveer één jaar in de toekomst en waarvoor de **IsPrimary** waarde is **False**.

### <a name="step-2-update-the-new-token-signing-certificates-for-the-office-365-trust"></a>Stap 2: Het nieuwe token-ondertekening van certificaten voor de vertrouwensrelatie van de Office 365 bijwerken
Office 365 met het nieuwe token-ondertekening van certificaten moet worden gebruikt voor de vertrouwensrelatie als volgt bijgewerkt.

1. Open de Microsoft Azure Active Directory-Module voor Windows PowerShell.
2. Voer $cred = Get-Credential. Wanneer deze cmdlet wordt u gevraagd om referenties, typt u de referenties van uw cloud-beheerder serviceaccount.
3. Uitvoeren van Connect MsolService – $cred referenties. Deze cmdlet maakt een verbinding met de cloudservice. Maken van een context die u met de cloudservice verbindt is vereist voordat u een van de aanvullende cmdlets geïnstalleerd door het hulpprogramma uitvoert.
4. Als u deze opdrachten worden uitgevoerd op een computer die niet de primaire AD FS-federatieserver, voert u Set-MSOLAdfscontext-Computer <AD FS primary server>, waarbij <AD FS primary server> is de interne FQDN-naam van de primaire AD FS-server. Deze cmdlet maakt een context die u met AD FS verbindt.
5. Voer Update-MSOLFederatedDomain – DomainName <domain>. Deze cmdlet werkt de instellingen van de AD FS in de cloudservice en configureert u de vertrouwensrelatie tussen de twee.

> [!NOTE]
> Als u wilt ondersteunen meerdere topleveldomeinen, zoals contoso.com en fabrikam.com, moet u de **SupportMultipleDomain** overschakelen met cmdlets. Zie voor meer informatie [ondersteuning voor meerdere domeinen van het hoogste niveau](active-directory-aadconnect-multiple-domains.md).
>


## Azure AD-vertrouwensrelatie herstellen met behulp van Azure AD Connect<a name="connectrenew"></a>
Als u uw AD FS-farm en Azure AD-vertrouwensrelatie met behulp van Azure AD Connect geconfigureerd, kunt u Azure AD Connect kunt gebruiken als u niets te doen voor uw token-ondertekening van certificaten moet detecteren. Als u de certificaten vernieuwen moet, kunt u Azure AD Connect gebruiken om dit te doen.

Zie voor meer informatie [repareren van de vertrouwensrelatie](active-directory-aadconnect-federation-management.md).

## <a name="ad-fs-and-azure-ad-certificate-update-steps"></a>AD FS en Azure AD updatestappen van het certificaat
Token handtekeningcertificaten zijn standaard X509 certificaten die worden gebruikt voor het veilig ondertekenen van alle tokens die door de federatieserver. Certificaten voor tokenontsleuteling zijn standaard X509 certificaten die worden gebruikt om alle binnenkomende tokens te ontsleutelen. 

AD FS is standaard geconfigureerd om te genereren voor tokenondertekening en tokenontsleuteling certificaten automatisch, zowel op het moment van de eerste configuratie en wanneer de certificaten zijn de vervaldatum bijna.

Azure AD probeert op te halen van een nieuw certificaat uit uw federatieve servicemetagegevens 30 dagen vóór de vervaldatum van het huidige certificaat. Als een nieuw certificaat niet beschikbaar op dat moment is, blijven Azure AD voor de bewaking van de metagegevens van de dagelijkse regelmatig. Als het nieuwe certificaat in de metagegevens beschikbaar is, wordt de federation-instellingen voor het domein worden bijgewerkt met de gegevens van het nieuwe certificaat. U kunt `Get-MsolDomainFederationSettings` controleren als u het nieuwe certificaat in de NextSigningCertificate ziet / SigningCertificate.

Zie voor meer informatie over het Token-ondertekening certificaten in AD FS [verkrijgen en configureren van Token-ondertekening en certificaten voor Token-ontsleuteling voor AD FS](https://docs.microsoft.com/windows-server/identity/ad-fs/operations/configure-ts-td-certs-ad-fs)
---
title: Wat is Selfserviceregistratie voor Azure? | Microsoft Docs
description: Een overzicht van toepassing met selfserviceregistratie voor Azure, het aanmeldingsproces beheren en hoe u over een DNS-domeinnaam op te nemen.
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 511088156d3546e2e0f3ac40e72bf2b8e4ae2cb9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="what-is-self-service-signup-for-azure"></a>Wat is Selfserviceregistratie voor Azure?
Dit onderwerp wordt het proces van de toepassing met selfserviceregistratie en hoe u over een DNS-domeinnaam op te nemen.  

## <a name="why-use-self-service-signup"></a>Waarom toepassing met selfserviceregistratie gebruiken?
* Klanten krijgen met services die ze sneller willen.
* Maak aanbiedingen voor een service op basis van e-mail.
* Maak de aanmelding op basis van e-stromen die snel gebruikers kunnen zich met hun werk gemakkelijk te onthouden e-aliassen identiteiten maken.
* Niet-beheerde Azure mappen kunnen later in beheerde mappen worden gemaakt en opnieuw worden gebruikt voor andere services.

## <a name="terms-and-definitions"></a>Termen en definities
* **Selfserviceaanmelding**: dit is de methode waarmee een gebruiker zich aanmeldt voor een cloudservice en heeft een identiteit voor ze in Azure Active Directory (Azure AD) automatisch gemaakt op basis van hun e-maildomein.
* **Niet-beheerde Azure-map**: dit is de directory waar die identiteit is gemaakt. Een niet-beheerde map is een map die geen globale beheerder heeft.
* **E-mailbericht geverifieerde gebruiker**: dit is een type gebruikersaccount in Azure AD. Een gebruiker met een automatisch gemaakt na het aanmelden voor een aanbieding selfservice identiteit staat bekend als een gebruiker met e-mailadres gecontroleerd. Een e-mailbericht geverifieerde gebruiker reguliere lid is van een map die is gemarkeerd met creationmethod = EmailVerified.

## <a name="user-experience"></a>Gebruikerservaring
Bijvoorbeeld, Stel dat een gebruiker waarvan e-mail is Dan@BellowsCollege.com vertrouwelijke bestanden via e-mail ontvangt. De bestanden zijn beveiligd door Azure Rights Management (Azure RMS). Maar Dan de organisatie, balg universiteit bent, is niet aangemeld voor Azure RMS of heeft deze Active Directory RMS geïmplementeerd. In dit geval, kan Dan zich registreren voor een gratis abonnement voor RMS voor personen om de beveiligde bestanden lezen.

Als Dan de eerste gebruiker met een e-mailadres van BellowsCollege.com is aanmelden voor deze self-service aanbieden, en vervolgens een niet-beheerde map wordt gemaakt voor BellowsCollege.com in Azure AD. Als andere gebruikers uit het domein BellowsCollege.com zich registreert voor deze aanbieding of een vergelijkbare selfservice aanbieding, hebben ze ook e-mailadres gecontroleerd gebruikersaccounts die zijn gemaakt in dezelfde niet-beheerde map in Azure.

## <a name="admin-experience"></a>Ervaring beheerder
Een beheerder die eigenaar is van de DNS-naam van een niet-beheerde Azure directory kan overnemen of samenvoegen van de map na eigendom aan te tonen. De volgende secties wordt de ervaring voor de beheerder in meer detail uitgelegd, maar hier volgt een samenvatting:

* Wanneer u een niet-beheerde Azure-map overneemt, worden de globale beheerder van de niet-beheerde map simpelweg zijn. Dit wordt soms een interne overname genoemd.
* Wanneer u een niet-beheerde Azure-map samenvoegt, u de DNS-naam van de niet-beheerde map aan uw beheerde Azure-map toevoegen en een toewijzing van gebruikers voor resources is gemaakt zodat gebruikers toegang houden tot services zonder onderbreking. Dit wordt soms een externe overname genoemd.

## <a name="what-gets-created-in-azure-active-directory"></a>Wat wordt gemaakt in Azure Active Directory?
#### <a name="directory"></a>Directory
* Een Azure Active Directory-map voor het domein wordt gemaakt, één map per domein.
* De Azure AD-directory heeft geen globale beheerder.

#### <a name="users"></a>Gebruikers
* Voor elke gebruiker die zich aanmeldt, wordt een gebruikersobject gemaakt in de Azure AD-directory.
* Elk gebruikersobject is gemarkeerd als extern.
* Elke gebruiker toegang krijgt tot de service die zij zich heeft aangemeld.

### <a name="how-do-i-claim-a-self-service-azure-ad-directory-for-a-domain-i-own"></a>Hoe claim een selfservice Azure AD voor een domein dat ik eigen map?
U kunt een Azure AD met selfservice claim map door het valideren van het domein. Domeinvalidatie bewijst eigenaar van het domein door het maken van DNS-records.

Er zijn twee manieren doen, een DNS-overname van een Azure Active directory:

* interne overname (Admin detecteert een niet-beheerde Azure-map en wil omzetten in een beheerde map)
* externe overname (Admin probeert een nieuw domein toevoegen aan hun beheerde Azure-map)

U mogelijk geïnteresseerd in dat u eigenaar van een domein omdat u via een niet-beheerde directory onderneemt nadat een gebruiker de toepassing met selfserviceregistratie uitgevoerd, of u mogelijk wordt een nieuw domein aan een bestaande beheerde directory toevoegen worden gevalideerd. Bijvoorbeeld, u een domein met de naam contoso.com hebt en u wilt een nieuw domein met de naam contoso.co.uk of contoso.uk toevoegen.

## <a name="what-is-domain-takeover"></a>Wat is de overname van domein?
Deze sectie bevat informatie over het valideren van de eigenaar van een domein

### <a name="what-is-domain-validation-and-why-is-it-used"></a>Wat is de domeinvalidatie van het en waarom is het gebruikt?
Om bewerkingen uitvoeren op een map, Azure AD is vereist dat u het eigendom van het DNS-domein valideren.  Validatie van het domein kunt u de map en ofwel de map Self-service naar een beheerde map promoveren claim of de map Self-service samenvoegen in een bestaande beheerde map.

## <a name="examples-of-domain-validation"></a>Voorbeelden validatie van het domein
Er zijn twee manieren doen, een DNS-overname van een map:

* interne overname (bijvoorbeeld een beheerder een selfservice, niet-beheerde adreslijst detecteert en wil zetten in beheerde map)
* externe overname (bijvoorbeeld een beheerder probeert een nieuw domein toevoegen aan een beheerde map)

### <a name="internal-takeover---promote-a-self-service-unmanaged-directory-to-be-a-managed-directory"></a>Interne overname - promoveren van een map selfservice, niet-beheerd als een beheerde map
Als u interne overname doet, wordt de map uit een niet-beheerde directory geconverteerd naar een beheerde map. U moet voltooien van de DNS-domeinvalidatie van de naam, waarin u een MX-record of een TXT-record in de DNS-zone maken. Deze actie:

* Valideert de eigenaar van het domein
* Zorgt ervoor dat de map beheerd
* Maakt u de globale beheerder van de map

Stel dat een IT-beheerder van balg school detecteert dat gebruikers van de school hebt aangemeld voor de offerings selfservice. De geregistreerde eigenaar van de DNS-namen, BellowsCollege.com, kan de IT-beheerder valideren van de eigenaar van de DNS-naam in Azure en los onbeheerde Active Directory. De map wordt vervolgens een beheerde map en de IT-beheerder voor de map BellowsCollege.com de globale beheerdersrol is toegewezen.

### <a name="external-takeover---merge-a-self-service-directory-into-an-existing-managed-directory"></a>Externe overname - een map Self-service samenvoegen met een bestaande beheerde map
In een externe overname hebt u al een beheerde map en u wilt dat alle gebruikers en groepen uit een niet-beheerde directory aan die directory beheerde plaats eigen twee afzonderlijke mappen.

Als een beheerder van een beheerde map toevoegen van een domein en dat domein gebeurt met een niet-beheerde map die is gekoppeld.

Stel dat bijvoorbeeld u een IT-beheerder bent en u hebt al een beheerde map voor Contoso.com, een domeinnaam die bij uw organisatie is geregistreerd. U ontdekt die gebruikers van uw organisatie hebt uitgevoerd selfservice sign up voor een aanbieding met behulp van e-domeinnaam user@contoso.co.uk, dit is een andere domeinnaam die uw organisatie eigenaar is. Deze gebruikers hebben momenteel accounts in een niet-beheerde map voor contoso.co.uk.

U wilt niet beheren twee afzonderlijke mappen, zodat u de niet-beheerde map voor contoso.co.uk in uw bestaande directory IT beheerd voor contoso.com samenvoegen.

Externe overname volgt het validatieproces van hetzelfde DNS-als interne overname.  Om het verschil: gebruikers en services worden toegewezen aan de directory IT beheerd.

#### <a name="whats-the-impact-of-performing-an-external-takeover"></a>Wat zijn de gevolgen van het uitvoeren van een externe overname?
Met een externe overname een toewijzing van gebruikers naar resources gemaakt zodat gebruikers toegang houden tot services zonder onderbreking. Veel toepassingen, waaronder RMS voor personen, de toewijzing van gebruikers aan bronnen goed verwerken en gebruikers toegang houden tot deze services zonder wijzigingen. Als een toepassing de toewijzing van gebruikers aan bronnen niet effectief verwerkt, externe overname mogelijk expliciet geblokkeerd om te voorkomen dat gebruikers een slechte ervaring.

#### <a name="directory-takeover-support-by-service"></a>Directory overname ondersteuning door service
De volgende services ondersteunen momenteel overname:

* RMS

De volgende services wordt binnenkort overname ondersteund:

* PowerBI

Het volgende niet doen en actie van extra admin om gebruikersgegevens te migreren na een externe overname is vereist.

* SharePoint/OneDrive

## <a name="how-to-perform-a-dns-domain-name-takeover"></a>Het uitvoeren van een DNS-domein naam overname
U hebt een aantal opties voor het uitvoeren van de domeinvalidatie van een (en een overname doen als u wenst):

1. Azure Management Portal

   Een overname wordt geactiveerd als volgt de toevoeging van een domein.  Als er al een map voor het domein bestaat, hebt u de optie voor het uitvoeren van een externe overname.

   Aanmelden bij de Azure-portal met uw referenties.  Navigeer naar uw bestaande directory en vervolgens naar **domein toevoegen**.
2. Office 365

   U kunt de opties op de [domeinen beheren](https://support.office.com/article/Navigate-to-the-Office-365-Manage-domains-page-026af1f2-0e6d-4f2d-9b33-fd147420fac2/) pagina in Office 365 werken met uw domeinen en DNS-records. Zie [Verifieer uw domein in Office 365](https://support.office.com/article/Verify-your-domain-in-Office-365-6383f56d-3d09-4dcb-9b41-b5f5a5efd611/).
3. Windows Powershell

   De volgende stappen zijn vereist om uit te voeren een validatie met behulp van Windows PowerShell.

   | Stap | Cmdlet te gebruiken |
   | --- | --- |
   | Een referentieobject maken |Get-Credential |
   | Verbinding maken met Azure AD |Connect-MsolService |
   | een lijst met domeinen |Get-MsolDomain |
   | Een vraag maken |Get-MsolDomainVerificationDns |
   | DNS-record maken |Dit doen op de DNS-server |
   | Controleer of de uitdaging |Confirm-MsolEmailVerifiedDomain |

Bijvoorbeeld:

1. Verbinding maken met Azure AD met behulp van de referenties die zijn gebruikt om te reageren op het aanbod selfservice:

        import-module MSOnline
        $msolcred = get-credential
        connect-msolservice -credential $msolcred
2. Haal een lijst met domeinen:

    Get-MsolDomain
3. Voer vervolgens de cmdlet Get-MsolDomainVerificationDns voor het maken van een challenge:

    Get-MsolDomainVerificationDns – DomainName *your_domain_name* – modus DnsTxtRecord

    Bijvoorbeeld:

    Get-MsolDomainVerificationDns – domeinnaam contoso.com – modus DnsTxtRecord
4. De waarde die wordt geretourneerd door deze opdracht (challenge) kopiëren.

    Bijvoorbeeld:

    MS 32DD01B82C05D27151EA9AE93C5890787F0E65D9 =
5. Maak een DNS txt-record met de waarde die u in de vorige stap hebt gekopieerd in uw openbare DNS-naamruimte.

    De naam voor deze record is de naam van het bovenliggende domein, dus als u deze bronrecord maakt met behulp van de DNS-functie van Windows Server, laat de plakken Record naam leeg zijn en alleen de waarde in het tekstvak
6. Voer de cmdlet bevestigen MsolDomain om te controleren of de uitdaging waar:

    Bevestig MsolEmailVerifiedDomain - DomainName *your_domain_name*

    Bijvoorbeeld:

    Bevestig MsolEmailVerifiedDomain - domeinnaam contoso.com

Een geslaagde uitdaging keert u terug naar de prompt zonder fouten.

## <a name="how-do-i-control-self-service-settings"></a>Hoe bepaal ik selfservice-instellingen
Beheerders hebben twee selfservice besturingselementen vandaag. Ze kunnen bepalen:

* Of gebruikers kunnen lid worden van de map via e-mail.
* Gebruikers kunnen de zelf of licentie voor toepassingen en services.

### <a name="how-can-i-control-these-capabilities"></a>Hoe kan ik deze mogelijkheden bepalen?
Een beheerder kan deze mogelijkheden gebruik van deze parameters van de cmdlet Set-MsolCompanySettings Azure AD configureren:

* **AllowEmailVerifiedUsers** bepaalt of een gebruiker kunt maken of koppelen van een niet-beheerde directory. Als u deze parameter op $false instelt, kunnen geen e-mailbericht geverifieerde gebruikers lid worden van de map.
* **AllowAdHocSubscriptions** regelt de mogelijkheid voor gebruikers Self-service sign up uitvoeren. Als u deze parameter op $false instelt, kunnen geen gebruikers de toepassing met selfserviceregistratie uitvoeren.

### <a name="how-do-the-controls-work-together"></a>Hoe werken de besturingselementen samen?
Deze twee parameters kunnen worden gebruikt in combinatie nauwkeuriger controle over Self-service sign up definiëren. Bijvoorbeeld de volgende opdracht, kunnen gebruikers zelf ondertekenen, maar alleen wordt uitgevoerd als deze gebruikers al een account in Azure AD hebben (met andere woorden, gebruikers hoeven een e-mailadres gecontroleerd account moet worden gemaakt niet uitvoeren selfservice sign up):

    Set-MsolCompanySettings -AllowEmailVerifiedUsers $false -AllowAdHocSubscriptions $true

Het volgende stroomdiagram wordt uitgelegd van de verschillende combinaties voor deze parameters en de resulterende voorwaarden voor de directory en selfservice Meld u omhoog.

![][1]

Zie voor meer informatie over en voorbeelden van het gebruik van deze parameters, [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0).

## <a name="see-also"></a>Zie ook
* [Azure PowerShell installeren en configureren](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-cmdlet-naslaginformatie](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png

---
title: Overname van de beheerder van een niet-beheerde map of shadow tenant in Azure Active Directory | Microsoft Docs
description: Klik hier voor meer informatie over het overnemen van een DNS-domeinnaam in een niet-beheerde map (shadow tenant) in Azure Active Directory.
services: active-directory
documentationcenter: 
author: curtand
manager: mtillman
editor: 
ms.assetid: b9f01876-29d1-4ab8-8b74-04d43d532f4b
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 11/14/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: f18e5883fca9291eb1447c1eebfe0883936fe84f
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="take-over-an-unmanaged-directory-as-administrator-in-azure-active-directory"></a>Een niet-beheerde directory overnemen als administrator in Azure Active Directory
Dit artikel wordt beschreven op twee manieren overnemen van een DNS-domeinnaam in een niet-beheerde map in Azure Active Directory (Azure AD). Wanneer een self-service gebruiker zich voor een cloudservice die gebruikmaakt van Azure AD aanmeldt, worden ze toegevoegd aan een niet-beheerde Azure AD-directory op basis van hun e-maildomein. Zie voor meer informatie over self-service of "een" aanmelden voor een service [wat is er toepassing met selfserviceregistratie voor Azure Active Directory?]()

## <a name="decide-how-you-want-to-take-over-an-unmanaged-directory"></a>Bepalen hoe u een niet-beheerde directory overnemen
Tijdens het proces van beheerder overname, kunt u eigendom bewijzen, zoals beschreven in [een aangepaste domeinnaam toevoegen aan Azure AD](add-custom-domain.md). De volgende secties wordt de ervaring voor de beheerder in meer detail uitgelegd, maar hier volgt een samenvatting:

* Wanneer u uitvoert een ['interne' admin overname](#internal-admin-takeover) van een niet-beheerde Azure-map die u als globale beheerder van de niet-beheerde directory wordt toegevoegd. Er zijn geen gebruikers, domeinen of service-abonnementen worden gemigreerd naar een map die u beheert.

* Wanneer u uitvoert een ['externe' beheerder overname](#external-admin-takeover) van een niet-beheerde Azure-map u de DNS-naam van de niet-beheerde map kunt toevoegen aan uw beheerde Azure-map. Wanneer u de naam van het domein toevoegt, wordt een toewijzing van gebruikers tot bronnen in uw beheerde Azure-map gemaakt zodat gebruikers toegang houden tot services zonder onderbreking. 

## <a name="internal-admin-takeover"></a>Interne admin overname

Sommige producten zoals SharePoint en OneDrive, zoals Office 365, bieden geen ondersteuning voor externe overname. Als uw scenario is of als u een beheerder bent en wilt brengen via een niet-beheerde of 'shadow' tenant maken door gebruikers die self-service aanmelding gebruikt, kunt u dit doen met een intern beheerder overname.

1. De context van een gebruiker maken in de niet-beheerde tenant via aanmeldt bij, zoals Power BI. Deze stappen voor het gemak van voorbeeld wordt ervan uitgegaan dat pad.

2. Open de [Power BI-site](https://powerbi.com) en selecteer **gratis Start**. Voer een gebruikersaccount die gebruikmaakt van de domeinnaam voor de organisatie; bijvoorbeeld: `admin@fourthcoffee.xyz`. Nadat u hebt ingevoerd in de verificatiecode, Controleer uw e-mailadres voor de bevestigingscode.

3. Selecteer in het bevestigingsbericht van Power BI **Ja, ik**.

4. Aanmelden bij de [Office 365-beheercentrum](https://portal.office.com/adminportal/Home) met het Power BI-gebruikersaccount. U ontvangt een bericht dat u de opdracht geeft **de beheerder worden** van de domeinnaam die was al geverifieerd in de niet-beheerde tenant. Selecteer **Ja, ik wil de beheerder**.
  
  ![eerste schermafbeelding voor geworden Admin](./media/domains-admin-takeover/become-admin-first.png)
  
5. De TXT-record om te bewijzen dat jij de eigenaar de domeinnaam toevoegen **fourthcoffee.xyz** op uw domeinnaamregistrar. In dit voorbeeld is het GoDaddy.com.
  
  ![Een txt-record voor de domeinnaam toevoegen](./media/domains-admin-takeover/become-admin-txt-record.png)

Wanneer de DNS TXT-records bij uw domeinregistrar naam zijn gecontroleerd, kunt u de Azure AD-tenant beheren.

Wanneer u de voorgaande stappen hebt voltooid, maar u bent nu de globale beheerder van de vierde koffie tenant in Office 365. U kunt voor het integreren van de domeinnaam met andere Azure-services, verwijderen van Office 365 en toe te voegen aan een andere beheerde tenant in Azure.

### <a name="adding-the-domain-name-to-a-managed-tenant-in-azure-ad"></a>De domeinnaam toevoegen aan een beheerde tenant in Azure AD 

1. Open de [Office 365-beheercentrum](https://portal.office.com/adminportal/Home).
2. Selecteer **gebruikers** tabblad en een nieuw gebruikersaccount maken met een naam zoals  *user@fourthcoffeexyz.onmicrosoft.com*  gebruikt die geen aangepaste domeinnaam. 
3. Zorg ervoor dat het nieuwe gebruikersaccount globale beheerder machtigingen voor de Azure AD-tenant heeft.
4. Open **domeinen** tabblad in het Office 365-beheercentrum, selecteer de domeinnaam en selecteer **verwijderen**. 
  
  ![de domeinnaam verwijderen vanuit Office 365](./media/domains-admin-takeover/remove-domain-from-o365.png)
  
5. Als u alle gebruikers of groepen in Office 365 die verwijzen naar de verwijderde domeinnaam hebt, moeten u ze gewijzigd in de. onmicrosoft.com-domein. Als u wilt afdwingen verwijderen van de domeinnaam, alle gebruikers automatisch worden gewijzigd, klikt u in dit voorbeeld  *user@fourthcoffeexyz.onmicrosoft.com* .
  
6. Aanmelden bij de [Azure AD-beheercentrum](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account dat de globale beheerder voor de Azure AD-tenant.
  
7. Selecteer **aangepaste domeinnamen**, voegt u de domeinnaam. U hebt de DNS TXT-records om te verifiëren dat de domeinnaam invoeren. 
  
  ![domein is toegevoegd aan Azure AD](./media/domains-admin-takeover/add-domain-to-azure-ad.png)
  
> [!NOTE]
> Alle gebruikers van Power BI- of Azure Rights Management-service die beschikken over licenties die zijn toegewezen in de Office 365-tenant moeten hun dashboards opslaan als de domeinnaam is verwijderd. Ze moeten zich aanmelden met een gebruikersnaam zoals  *user@fourthcoffeexyz.onmicrosoft.com*  plaats  *user@fourthcoffee.xyz* .

## <a name="external-admin-takeover"></a>Overname van extern beheer

Als u al een tenant met Azure-services of Office 365 beheren, bent u een aangepaste domeinnaam kan niet toevoegen als deze al is geverifieerd in een andere Azure AD-tenant. Echter van uw beheerde tenant in Azure AD kunt u een niet-beheerde tenant als een externe beheerder overname overnemen. De algemene procedure volgt het artikel [een aangepast domein toevoegen aan Azure AD](add-custom-domain.md).

Wanneer u verifiëren dat de domeinnaam, wordt Azure AD verwijdert u de domeinnaam van de niet-beheerde tenant en verplaatst naar uw bestaande tenant. Overname van extern beheer van een niet-beheerde directory vereist het validatieproces van hetzelfde DNS TXT als intern beheerder overname. Het verschil is dat het volgende ook via met de naam van het domein verplaatst worden:

- Gebruikers
- Abonnementen
- Toewijzen van licenties
 
De [ **ForceTakeover** optie](#azure-ad-powershell-cmdlets-for-the-forcetakeover-option) voor extern beheer van domain name overname wordt ondersteund voor slechts twee services, Power BI en Azure RMS.

### <a name="support-for-external-admin-takeover"></a>Ondersteuning voor extern beheer overname
Extern beheer overname wordt ondersteund door de volgende online services:

- Power BI
- Azure Rights Management-Service (RMS)
- Exchange Online

De ondersteunde serviceplannen zijn onder andere:

- Power BI-gratis
- Power BI Pro
- Gratis PowerApps
- Gratis PowerFlow
- Basic van Azure Rights Management-Service (RMS)
- Azure Rights Management Service Enterprise (RMS)
- Microsoft-Stream
- Dynamics 365 gratis proefversie

Exernal admin overname wordt niet ondersteund voor elke service met de service-abonnementen met SharePoint, OneDrive of Skype voor bedrijven; bijvoorbeeld via een gratis Office-abonnement of de basis-SKU van Office.

### <a name="azure-ad-powershell-cmdlets-for-the-forcetakeover-option"></a>Azure AD PowerShell-cmdlets voor de optie ForceTakeover
U kunt deze cmdlets gebruikt in zien [PowerShell-voorbeeld](#powershell-example).


Cmdlet | Gebruik 
------- | -------
`connect-msolservice` | Wanneer u wordt gevraagd, moet u zich aanmelden bij uw beheerde tenant.
`get-msoldomain` | Toont uw domeinnamen gekoppeld aan de huidige tenant.
`new-msoldomain –name <domainname>` | Voegt de domeinnaam in voor de tenant als Unverified (geen DNS-controle is uitgevoerd nog).
`get-msoldomain` | De domeinnaam is nu opgenomen in de lijst van domeinnamen die zijn gekoppeld aan uw beheerde tenant, maar wordt vermeld als **Unverified**.
`get-msoldomainverificationdns –Domainname <domainname> –Mode DnsTxtRecord` | Bevat de informatie in de nieuwe DNS TXT-record voor het domein plaatsen (MS = xxxxx). Verificatie mogelijk niet plaatsvinden onmiddellijk omdat het duurt enige tijd voor de TXT-record te kunnen doorvoeren, dus wacht een paar minuten voordat de **- ForceTakeover** optie. 
`confirm-msoldomain –Domainname <domainname> –ForceTakeover Force` | <li>Als uw domeinnaam is nog niet geverifieerd, kunt u doorgaan met de **- ForceTakeover** optie. Wordt gecontroleerd of de TXT-record is gemaakt en het proces overname serversysteemstatus.<li>De **- ForceTakeover** optie moet worden toegevoegd aan de cmdlet, alleen wanneer het forceren van een overname extern beheer, zoals wanneer het niet-beheerde tenant heeft voor Office 365-services de overname blokkeren.
`get-msoldomain` | Nu ziet u de lijst met de naam van het domein als **geverifieerde**.

### <a name="powershell-example"></a>PowerShell-voorbeeld

1. Verbinding maken met Azure AD met behulp van de referenties die zijn gebruikt om te reageren op het aanbod selfservice:
  ````
    import-module MSOnline
    $msolcred = get-credential
    
    connect-msolservice -credential $msolcred
  ````
2. Haal een lijst met domeinen:
  
  ````
    Get-MsolDomain
  ````
3. Voer de cmdlet Get-MsolDomainVerificationDns voor het maken van een challenge:
  ````
    Get-MsolDomainVerificationDns –DomainName *your_domain_name* –Mode DnsTxtRecord
  
    For example:
  
    Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord
  ````

4. De waarde die wordt geretourneerd door deze opdracht (challenge) kopiëren. Bijvoorbeeld:
  ````
    MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9
  ````
5. Maak een DNS txt-record met de waarde die u in de vorige stap hebt gekopieerd in uw openbare DNS-naamruimte. De naam voor deze record is de naam van het bovenliggende domein, dus als u deze bronrecord maakt met behulp van de DNS-functie van Windows Server, laat de plakken Record naam leeg zijn en alleen de waarde in het tekstvak.
6. Voer de cmdlet bevestigen MsolDomain om te controleren of de uitdaging waar:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName *your_domain_name*
  ````
  
  Bijvoorbeeld:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName contoso.com
  ````

Een geslaagde uitdaging keert u terug naar de prompt zonder fouten.

## <a name="next-steps"></a>Volgende stappen
* [Een aangepaste domeinnaam toevoegen aan Azure AD](add-custom-domain.md)
* [Azure PowerShell installeren en configureren](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-cmdlet-naslaginformatie](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png

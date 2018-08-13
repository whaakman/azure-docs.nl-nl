---
title: Overname van de beheerder van een niet-beheerde adreslijst of een schaduwkopie tenant in Azure Active Directory | Microsoft Docs
description: Klik hier voor meer informatie over het overnemen van een DNS-domeinnaam in een niet-beheerde adreslijst (shadow tenant) in Azure Active Directory.
services: active-directory
documentationcenter: ''
author: curtand
manager: mtillman
editor: ''
ms.service: active-directory
ms.component: users-groups-roles
ms.topic: article
ms.workload: identity
ms.date: 04/06/2017
ms.author: curtand
ms.reviewer: elkuzmen
ms.custom: it-pro
ms.openlocfilehash: 52ae7da666acaf234920a7f03afe3766f29a1e85
ms.sourcegitcommit: 4de6a8671c445fae31f760385710f17d504228f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/08/2018
ms.locfileid: "39629120"
---
# <a name="take-over-an-unmanaged-directory-as-administrator-in-azure-active-directory"></a>Een niet-beheerde directory overnemen als in Azure Active Directory-beheerder
Dit artikel wordt beschreven in een niet-beheerde adreslijst in Azure Active Directory (Azure AD) een DNS-domeinnaam overnemen op twee manieren. Wanneer een selfservice-gebruiker zich registreert voor een cloudservice die gebruikmaakt van Azure AD, wordt deze toegevoegd aan een niet-beheerde Azure AD-adreslijst op basis van het e-maildomein. Zie voor meer informatie over selfservice of 'viraal' aanmelden voor een service [wat is selfserviceregistratie voor Azure Active Directory?](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-self-service-signup)

## <a name="decide-how-you-want-to-take-over-an-unmanaged-directory"></a>Bepaal hoe u wilt een niet-beheerde directory overnemen
Tijdens het proces van overname door een beheerder kunt u eigendom bewijzen, zoals beschreven in [Een aangepaste domeinnaam toevoegen aan Azure AD](../fundamentals/add-custom-domain.md). In de volgende secties wordt de ervaring voor de beheerder gedetailleerder uitgelegd, maar hier volgt een samenvatting:

* Wanneer u een ['interne' beheerdersovername](#internal-admin-takeover) van een niet-beheerde Azure-adreslijst uitvoert, wordt u als globale beheerder van de niet-beheerde adreslijst toegevoegd. Er worden geen gebruikers, domeinen of serviceabonnementen gemigreerd naar een andere adreslijst die u beheert.

* Wanneer u een ['externe' beheerdersovername](#external-admin-takeover) uitvoert van een niet-beheerde Azure-adreslijst, voegt u de naam van het DNS-domein van de niet-beheerde adreslijst toe aan uw beheerde Azure-adreslijst. Wanneer u de domeinnaam toevoegt, wordt een toewijzing van gebruikers aan bronnen gemaakt in uw beheerde Azure-adreslijst, zodat gebruikers zonder onderbreking toegang houden tot services. 

## <a name="internal-admin-takeover"></a>Interne beheerdersovername

Sommige producten, zoals SharePoint en OneDrive, zoals Office 365, bieden geen ondersteuning voor externe overname. Als uw scenario, of als u een beheerder bent en wilt overnemen een niet-beheerde of 'shadow' tenant maken door gebruikers die self-service-aanmelding gebruikt, kunt u dit doen met een interne beheerdersrechten overneemt.

1. Maak de context van een gebruiker in de niet-beheerde tenant bij het aanmelden met zoals Power BI. Deze stappen voor het gemak van het voorbeeld wordt ervan uitgegaan dat het pad.

2. Open de [Power BI-site](https://powerbi.com) en selecteer **gratis starten**. Voer een gebruikersaccount die gebruikmaakt van de domain name voor de organisatie. bijvoorbeeld, `admin@fourthcoffee.xyz`. Nadat u in de verificatiecode invoert, controleert u uw e-mailadres voor de bevestigingscode.

3. Selecteer in de bevestigingse-mail van Power BI, **Ja, dat ben ik**.

4. Aanmelden bij de [Office 365-beheercentrum](https://portal.office.com/adminportal/Home) met de Power BI-gebruikersaccount. U ontvangt een bericht dat u de opdracht geeft **beheerder** van de domeinnaam die was al geverifieerd in de niet-beheerde tenant. Selecteer **Ja, ik wil de beheerder**.
  
  ![eerste schermafbeelding voor worden de beheerder](./media/domains-admin-takeover/become-admin-first.png)
  
5. De TXT-record om te bewijzen dat u eigenaar van de domeinnaam toevoegen **fourthcoffee.xyz** aan uw domeinnaamregistrar. In dit voorbeeld is het GoDaddy.com.
  
  ![Een txt-record voor de domeinnaam toevoegen](./media/domains-admin-takeover/become-admin-txt-record.png)

Wanneer de DNS TXT-records worden geverifieerd bij uw domeinnaamregistrar, kunt u de Azure AD-tenant kunt beheren.

Wanneer u de voorgaande stappen hebt voltooid, bent u nu de globale beheerder van de vierde koffie tenant in Office 365. Als u wilt de domeinnaam integreren met uw andere Azure-services, kunt u verwijderen uit Office 365 en toe te voegen aan een andere beheerde tenant in Azure.

### <a name="adding-the-domain-name-to-a-managed-tenant-in-azure-ad"></a>Naam van het domein toe te voegen aan een beheerde tenant in Azure AD 

1. Open de [Office 365-beheercentrum](https://portal.office.com/adminportal/Home).
2. Selecteer **gebruikers** tabblad en een nieuw gebruikersaccount maken met een naam, zoals * user@fourthcoffeexyz.onmicrosoft.com * die gebruikt niet de aangepaste domeinnaam. 
3. Zorg ervoor dat het nieuwe gebruikersaccount globale beheerdersmachtigingen voor de Azure AD-tenant.
4. Open **domeinen** tabblad in het Office 365-beheercentrum, selecteert u de domeinnaam en selecteer **verwijderen**. 
  
  ![de domeinnaam verwijderen uit Office 365](./media/domains-admin-takeover/remove-domain-from-o365.png)
  
5. Als u gebruikers of groepen in Office 365 die verwijzen naar de verwijderde domeinnaam hebt, moeten u ze gewijzigd in de. onmicrosoft.com-domein. Als u geforceerd verwijderen van de domeinnaam, alle gebruikers worden automatisch gewijzigd, in dit voorbeeld * user@fourthcoffeexyz.onmicrosoft.com *.
  
6. Aanmelden bij de [Azure AD-beheercentrum](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) met een account dat de globale beheerder voor de Azure AD-tenant.
  
7. Selecteer **aangepaste-domeinnamen**, voegt u de domeinnaam. Hebt u de DNS TXT-records om te controleren of de eigenaar van de domain name invoeren. 
  
  ![domein toegevoegd aan Azure AD](./media/domains-admin-takeover/add-domain-to-azure-ad.png)
  
> [!NOTE]
> Alle gebruikers van Power BI- of Azure Rights Management-service die een licentie toegewezen in de Office 365-tenant zijn, moeten hun dashboards opslaan als de domeinnaam wordt verwijderd. Ze moeten zich aanmelden met een gebruikersnaam, zoals * user@fourthcoffeexyz.onmicrosoft.com * in plaats van * user@fourthcoffee.xyz *.

## <a name="external-admin-takeover"></a>Externe beheerdersovername

Als u al een tenant met de Azure-services of Office 365 beheren, kunt u een aangepaste domeinnaam kan niet toevoegen als deze al is geverifieerd in een andere Azure AD-tenant. Echter uit uw beheerde tenant in Azure AD kunt u een niet-beheerde tenant als een externe beheerdersovername overnemen. De algemene procedure volgt het artikel [een aangepast domein toevoegen aan Azure AD](../fundamentals/add-custom-domain.md).

Wanneer u eigendom van de domeinnaam te verifiëren, wordt Azure AD Hiermee verwijdert u de domeinnaam van de niet-beheerde tenant en verplaatst die naar uw bestaande tenant. Externe beheerdersrechten overneemt van een niet-beheerde adreslijst vereist het validatieproces van dezelfde DNS TXT als interne beheerdersrechten overneemt. Het verschil is dat het volgende ook met de naam van het domein verplaatst worden:

- Gebruikers
- Abonnementen
- Toewijzing van licenties

### <a name="support-for-external-admin-takeover"></a>Ondersteuning voor externe beheerdersovername
Externe beheerdersovername wordt ondersteund door de volgende online services:

- Power BI
- Azure Rights Management
- Exchange Online

De ondersteunde service-abonnementen bevatten:

- Power BI gratis
- Power BI Pro
- PowerApps-gratis
- Gratis PowerFlow
- RMS voor personen
- Microsoft Stream
- Gratis proefversie van Dynamics 365

Externe beheerdersovername wordt niet ondersteund voor alle services waarvoor serviceabonnementen met SharePoint, OneDrive of Skype voor bedrijven; bijvoorbeeld via een gratis Office-abonnement of de basis-SKU van Office. U kunt eventueel de [ **ForceTakeover** optie](#azure-ad-powershell-cmdlets-for-the-forcetakeover-option) voor het verwijderen van de domeinnaam van de niet-beheerde tenant en het controleren op de gewenste tenant. Deze ForceTakeover-optie wordt niet verplaatsen over gebruikers, of toegang tot het abonnement behouden. Met deze optie wordt alleen in plaats daarvan de domeinnaam. 

#### <a name="more-information-about-rms-for-individuals"></a>Meer informatie over RMS voor personen

Voor [RMS voor personen](/azure/information-protection/rms-for-individuals), wanneer het niet-beheerde tenant bevindt zich in dezelfde regio als de tenant dat u eigenaar bent, de automatisch gemaakte [Azure Information Protection-tenantsleutel](/azure/information-protection/plan-implement-tenant-key) en [standaard Beveiligingssjablonen](/azure/information-protection/configure-usage-rights#rights-included-in-the-default-templates) verder met de naam van het domein worden verplaatst. 

De sleutel en -sjablonen zijn niet verplaatst via wanneer het niet-beheerde tenant bevindt zich in een andere regio. De niet-beheerde tenant is bijvoorbeeld in Europa en de tenant die van jou is in Noord-Amerika. 

Hoewel RMS voor personen is ontworpen ter ondersteuning van Azure AD-verificatie om te openen van beveiligde inhoud, niet te voorkomen dat gebruikers inhoud ook beveiligen. Als gebruikers inhoud met de RMS voor personen-abonnement beveiligen en de sleutel en -sjablonen niet via verplaatst zijn, wordt die inhoud niet toegankelijk nadat de domeinovername.    

### <a name="azure-ad-powershell-cmdlets-for-the-forcetakeover-option"></a>Azure AD PowerShell-cmdlets voor de ForceTakeover-optie
U ziet deze cmdlets gebruikt in [PowerShell-voorbeeld](#powershell-example).


Cmdlet | Gebruik 
------- | -------
`connect-msolservice` | Wanneer u hierom wordt gevraagd, moet u zich aanmelden bij uw beheerde tenant.
`get-msoldomain` | Geeft de domeinnamen van uw dat is gekoppeld aan de huidige tenant.
`new-msoldomain –name <domainname>` | Wordt toegevoegd aan de tenant als niet-geverifieerd (Er is geen DNS-verificatie is uitgevoerd nog) de naam van het domein.
`get-msoldomain` | De domeinnaam is nu opgenomen in de lijst met domeinnamen die zijn gekoppeld aan uw beheerde tenant, maar wordt vermeld als **niet geverifieerd**.
`get-msoldomainverificationdns –Domainname <domainname> –Mode DnsTxtRecord` | Bevat de informatie in de nieuwe DNS TXT-record voor het domein plaatsen (MS = xxxxx). Verificatie mogelijk niet gebeurt onmiddellijk omdat het duurt even voordat de TXT-record worden doorgegeven, dus wacht een paar minuten voordat de **- ForceTakeover** optie. 
`confirm-msoldomain –Domainname <domainname> –ForceTakeover Force` | <li>Als de domeinnaam van uw nog niet is geverifieerd, kunt u doorgaan met de **- ForceTakeover** optie. Wordt gecontroleerd of de TXT-record is gemaakt en het proces van overname begint.<li>De **- ForceTakeover** optie moet worden toegevoegd aan de cmdlet alleen wanneer het afdwingen van een externe beheerdersovername, zoals wanneer het niet-beheerde tenant heeft voor Office 365-services de overname blokkeren.
`get-msoldomain` | De lijst met domeinen wordt nu weergegeven als de naam van het domein **Verified**.

### <a name="powershell-example"></a>PowerShell-voorbeeld

1. Verbinding maken met Azure AD met behulp van de referenties die zijn gebruikt om te reageren op de aanbieding selfservice:
  ````
    import-module MSOnline
    $msolcred = get-credential
    
    connect-msolservice -credential $msolcred
  ````
2. Haal een lijst met domeinen:
  
  ````
    Get-MsolDomain
  ````
3. De cmdlet Get-MsolDomainVerificationDns voor het maken van een uitdaging uitvoeren:
  ````
    Get-MsolDomainVerificationDns –DomainName *your_domain_name* –Mode DnsTxtRecord
  
    For example:
  
    Get-MsolDomainVerificationDns –DomainName contoso.com –Mode DnsTxtRecord
  ````

4. Kopieer de waarde (de challenge) die wordt geretourneerd door deze opdracht. Bijvoorbeeld:
  ````
    MS=32DD01B82C05D27151EA9AE93C5890787F0E65D9
  ````
5. Maak een DNS txt-record met de waarde die u in de vorige stap hebt gekopieerd in uw openbare DNS-naamruimte. De naam voor deze record is de naam van het bovenliggende domein, dus als u deze bronrecord maakt met behulp van de DNS-rol vanaf Windows Server, laat de plakken Record de naam leeg en alleen de waarde in het tekstvak.
6. De Confirm-MsolDomain-cmdlet om te controleren of de uitdaging uitvoeren:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName *your_domain_name*
  ````
  
  Bijvoorbeeld:
  
  ````
    Confirm-MsolEmailVerifiedDomain -DomainName contoso.com
  ````

Een geslaagde uitdaging gaat u terug naar de prompt zonder fouten.

## <a name="next-steps"></a>Volgende stappen
* [Een aangepaste domeinnaam toevoegen aan Azure AD](../fundamentals/add-custom-domain.md)
* [Azure PowerShell installeren en configureren](/powershell/azure/overview)
* [Azure PowerShell](/powershell/azure/overview)
* [Azure-cmdlet-naslaginformatie](/powershell/azure/get-started-azureps)
* [Set-MsolCompanySettings](/powershell/module/msonline/set-msolcompanysettings?view=azureadps-1.0)

<!--Image references-->
[1]: ./media/active-directory-self-service-signup/SelfServiceSignUpControls.png

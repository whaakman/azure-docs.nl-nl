---
title: 'Azure Active Directory Domain Services: Problemen oplossen met | Microsoft Docs'
description: Gids voor probleemoplossing voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: ''
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory
ms.component: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 01/08/2018
ms.author: maheshu
ms.openlocfilehash: 41a06846e914eb79e15aa47c7203b17166731ae0
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39502325"
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services - gids problemen oplossen
Dit artikel vindt tips voor probleemoplossing voor problemen die optreden kunnen bij het instellen of beheren van Azure Active Directory (AD) Domain Services.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>U kunt Azure AD Domain Services voor uw Azure AD-directory niet inschakelen
In deze sectie helpt u fouten oplossen wanneer u probeert te Azure AD Domain Services inschakelt voor uw directory.

Kies de stappen voor probleemoplossing die overeenkomen met het foutbericht dat u ondervindt.

| **Foutbericht** | **Resolutie** |
| --- |:--- |
| *De naam contoso100.com is al in gebruik op dit netwerk. Geef een naam op die niet in gebruik is.* |[Conflict met domein in het virtuele netwerk](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| *Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De service heeft niet voldoende machtigingen voor de toepassing met de naam 'Azure AD Domain Services Sync'. Verwijder de toepassing met de naam 'Azure AD Domain Services Sync' en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.* |[Domain Services beschikt niet over voldoende machtigingen voor de Azure AD Domain Services Sync-toepassing](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| *Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Domain Services in uw Azure AD-tenant heeft niet de vereiste machtigingen om Domain Services in te schakelen. Verwijder de toepassing met de toepassings-id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.* |[De toepassing Domain Services is niet juist geconfigureerd in uw tenant](active-directory-ds-troubleshooting.md#invalid-configuration) |
| *Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Microsoft Azure AD is uitgeschakeld in uw Azure AD-tenant. Schakel de toepassing met de toepassings-id 00000002-0000-0000-c000-000000000000 in en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.* |[De toepassing Microsoft Graph is uitgeschakeld in uw Azure AD-tenant](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Conflict met domein
**Foutbericht:**

*De naam contoso100.com is al in gebruik op dit netwerk. Geef een naam op die niet in gebruik is.*

**Herstel:**

Zorg ervoor dat u geen bestaand domein met dezelfde domeinnaam beschikbaar is op dit virtuele netwerk. Het domein met de naam contoso.com is bijvoorbeeld al beschikbaar op het geselecteerde virtuele netwerk. Later kunt proberen u om in te schakelen van een Azure AD Domain Services beheerde domein met dezelfde domeinnaam (dat wil zeggen ' contoso.com') op dit virtuele netwerk. U kunt een fout optreedt tijdens het inschakelen van Azure AD Domain Services.

Deze fout is ontstaan door naamconflicten voor de domeinnaam op dit virtuele netwerk. In dit geval moet u een andere naam gebruiken om het beheerde domein van Azure AD Domain Services in te stellen. U kunt ook de inrichting van het bestaande domein ongedaan maken en doorgaan met het inschakelen van Azure AD Domain Services.

### <a name="inadequate-permissions"></a>Onvoldoende machtigingen
**Foutbericht:**

*Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De service heeft niet voldoende machtigingen voor de toepassing met de naam 'Azure AD Domain Services Sync'. Verwijder de toepassing met de naam 'Azure AD Domain Services Sync' en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.*

**Herstel:**

Controleer of er is een toepassing met de naam 'Azure AD Domain Services Sync' in uw Azure AD-directory. Als deze toepassing bestaat, verwijderen en vervolgens Azure AD Domain Services opnieuw inschakelen.

Voer de volgende stappen uit om te controleren op de aanwezigheid van de toepassing en verwijderen, als de toepassing bestaat:

1. Navigeer naar de **toepassingen** sectie van uw Azure AD-directory in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/).
2. Selecteer **alle toepassingen** in de **weergeven** vervolgkeuzelijst. Selecteer **eventuele** in de **toepassingsstatus** vervolgkeuzelijst. Selecteer **eventuele** in de **zichtbaarheid van toepassing** vervolgkeuzelijst.
3. Type **Azure AD Domain Services Sync** in het zoekvak in. Als de toepassing aanwezig is, klikt u erop klikt u op de **verwijderen** knop op de werkbalk om deze te verwijderen.
4. Nadat u de toepassing hebt verwijderd, probeer het opnieuw inschakelen van Azure AD Domain Services.

### <a name="invalid-configuration"></a>Ongeldige configuratie
**Foutbericht:**

*Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Domain Services in uw Azure AD-tenant heeft niet de vereiste machtigingen om Domain Services in te schakelen. Verwijder de toepassing met de toepassings-id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.*

**Herstel:**

Controleer of u een toepassing met de naam 'AzureActiveDirectoryDomainControllerServices' (met een toepassings-id van d87dcbc6-a371-462e-88e3-28ad15ec4e64) hebt in uw Azure AD-directory. Als deze toepassing bestaat, moet u deze verwijderen en vervolgens Azure AD Domain Services opnieuw inschakelen.

Gebruik de volgende PowerShell-script te vinden van de toepassing en deze verwijdert.

> [!NOTE]
> Dit script maakt gebruik van **Azure AD PowerShell versie 2** cmdlets. Lees voor een volledige lijst van alle beschikbare cmdlets en de module downloaden, de [AzureAD PowerShell-referentiedocumentatie](https://msdn.microsoft.com/library/azure/mt757189.aspx).
>
>

```powershell
$InformationPreference = "Continue"
$WarningPreference = "Continue"

$aadDsSp = Get-AzureADServicePrincipal -Filter "AppId eq 'd87dcbc6-a371-462e-88e3-28ad15ec4e64'" -ErrorAction Ignore
if ($aadDsSp -ne $null)
{
    Write-Information "Found Azure AD Domain Services application. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $aadDsSp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services application."
}

$identifierUri = "https://sync.aaddc.activedirectory.windowsazure.com"
$appFilter = "IdentifierUris eq '" + $identifierUri + "'"
$app = Get-AzureADApplication -Filter $appFilter
if ($app -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync application. Deleting it ..."
    Remove-AzureADApplication -ObjectId $app.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync application."
}

$spFilter = "ServicePrincipalNames eq '" + $identifierUri + "'"
$sp = Get-AzureADServicePrincipal -Filter $spFilter
if ($sp -ne $null)
{
    Write-Information "Found Azure AD Domain Services Sync service principal. Deleting it ..."
    Remove-AzureADServicePrincipal -ObjectId $sp.ObjectId
    Write-Information "Deleted the Azure AD Domain Services Sync service principal."
}
```
<br>

### <a name="microsoft-graph-disabled"></a>Microsoft Graph uitgeschakeld
**Foutbericht:**

Domain Services kunnen niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Microsoft Azure AD is uitgeschakeld in uw Azure AD-tenant. Schakel de toepassing met de toepassing-id 00000002-0000-0000-C000-000000000000 in en probeer vervolgens te domeinservices inschakelen voor uw Azure AD-tenant.

**Herstel:**

Controleer als u een toepassing met de id 00000002-0000-0000-c000-000000000000 hebt uitgeschakeld. Deze toepassing is de Microsoft Azure AD-toepassing en biedt Graph API-toegang tot uw Azure AD-tenant. Azure AD Domain Services moet deze toepassing worden ingeschakeld om te synchroniseren van uw Azure AD-tenant aan uw beheerde domein.

U kunt deze fout oplossen door deze toepassing inschakelen en probeer vervolgens te domeinservices inschakelen voor uw Azure AD-tenant.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Gebruikers kunnen zich niet aanmelden bij het in Azure AD Domain Services beheerde domein
Als een of meer gebruikers in uw Azure AD-tenant zich niet aanmelden bij de zojuist gemaakte beheerde domein, voer de volgende stappen:

* **Meld u aan met de UPN-indeling:** probeert aan te melden met behulp van de UPN-indeling (bijvoorbeeld 'joeuser@contoso.com') in plaats van de SAMAccountName-indeling ('CONTOSO\joeuser'). De SAMAccountName kan automatisch worden gegenereerd voor gebruikers wiens UPN-voorvoegsel is te lang of is hetzelfde als een andere gebruiker in het beheerde domein. De UPN-indeling is gegarandeerd uniek zijn binnen een Azure AD-tenant.

> [!NOTE]
> U wordt aangeraden met behulp van de UPN-indeling te melden bij de Azure AD Domain Services beheerde domein.
>
>

* Zorg ervoor dat u [wachtwoordsynchronisatie hebt ingeschakeld](active-directory-ds-getting-started-password-sync.md) volgens de stappen beschreven in de handleiding.
* **Externe accounts:** Zorg ervoor dat het betreffende gebruikersaccount geen extern account in de Azure AD-tenant. Voorbeelden van externe accounts zijn Microsoft-accounts (bijvoorbeeld 'joe@live.com') of gebruikersaccounts met een externe Azure AD-directory. Omdat Azure AD Domain Services geen referenties voor deze gebruikersaccounts, kunnen deze gebruikers zich niet aanmelden bij het beheerde domein.
* **Accounts gesynchroniseerd:** als de betrokken gebruikersaccounts worden gesynchroniseerd vanuit een on-premises directory, controleert u of die:

  * U hebt geïmplementeerd of bijgewerkt naar de [meest recente aanbevolen versie van Azure AD Connect](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
  * U kunt Azure AD Connect hebt geconfigureerd [een volledige synchronisatie uitvoeren](active-directory-ds-getting-started-password-sync.md).
  * Afhankelijk van de grootte van uw directory, kan het even duren voor gebruikersaccounts en referentie-hashes worden weergegeven in Azure AD Domain Services. Zorg ervoor dat u wacht lang genoeg voordat opnieuw wordt geprobeerd de verificatie.
  * Als het probleem zich blijft voordoen nadat u hebt gecontroleerd van de voorgaande stappen, probeert u de Microsoft Azure AD Sync-Service opnieuw te starten. Vanaf uw sync-computer, start een opdrachtprompt en voer de volgende opdrachten uit:

    1. net stop 'Microsoft Azure AD Sync'
    2. net start 'Microsoft Azure AD Sync'
* **Alleen in de cloud accounts**: als het betrokken gebruikersaccount een alleen-cloud-gebruikersaccount is, zorgt u ervoor dat de gebruiker het wachtwoord is gewijzigd nadat u Azure AD Domain Services hebt ingeschakeld. Door deze stap worden de referentie-hashes gegenereerd die zijn vereist voor Azure AD Domain Services.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Er zijn een of meer waarschuwingen op uw beheerde domein

Informatie over het oplossen van waarschuwingen op uw beheerde domein door naar de pagina de [waarschuwingen oplossen](active-directory-ds-troubleshoot-alerts.md) artikel.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Verwijderd uit uw Azure AD-tenant gebruikers worden niet verwijderd uit uw beheerde domein
Azure AD voorkomt dat u onbedoeld gebruikersobjecten verwijdert. Wanneer u een gebruikersaccount uit uw Azure AD-tenant verwijdert, wordt het overeenkomstige gebruikersobject verplaatst naar de Prullenbak. Wanneer deze verwijderingsbewerking wordt gesynchroniseerd met uw beheerde domein, zorgt deze ervoor dat het bijbehorende gebruikersaccount gemarkeerd als uitgeschakeld. Deze functie kunt u herstellen of het gebruikersaccount later ongedaan maken.

Het gebruikersaccount dat blijft in de status uitgeschakeld in uw beheerde domein, zelfs als u opnieuw een gebruikersaccount met dezelfde UPN in uw Azure AD-directory maakt. Als u wilt het gebruikersaccount verwijderen uit uw beheerde domein, moet u deze geforceerd verwijderen uit uw Azure AD-tenant.

Als wilt verwijderen het gebruikersaccount dat volledig uit uw beheerde domein, verwijdert u de gebruiker permanent van uw Azure AD-tenant. Gebruik de `Remove-MsolUser` PowerShell-cmdlet met de `-RemoveFromRecycleBin` optie, zoals beschreven in dit [MSDN-artikel](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Contact opnemen
Neem contact op met het productteam van Azure Active Directory Domain Services naar [feedback geven of voor ondersteuning van](active-directory-ds-contact-us.md).

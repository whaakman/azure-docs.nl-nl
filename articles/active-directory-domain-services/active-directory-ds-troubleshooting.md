---
title: 'Azure Active Directory Domain Services: Troubleshooting Guide | Microsoft Docs'
description: Gids voor probleemoplossing voor Azure AD Domain Services
services: active-directory-ds
documentationcenter: 
author: mahesh-unnikrishnan
manager: mtillman
editor: curtand
ms.assetid: 4bc8c604-f57c-4f28-9dac-8b9164a0cf0b
ms.service: active-directory-ds
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/08/2018
ms.author: maheshu
ms.openlocfilehash: 0956476931396c6455bf3e4fc7582da3bf3deb33
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-ad-domain-services---troubleshooting-guide"></a>Azure AD Domain Services - probleemoplossingsgids
Dit artikel bevat tips voor probleemoplossing voor problemen die optreden kunnen bij het instellen of het beheer van Azure Active Directory (AD) Domain Services.

## <a name="you-cannot-enable-azure-ad-domain-services-for-your-azure-ad-directory"></a>U kunt Azure AD Domain Services voor uw Azure AD-directory niet inschakelen
Deze sectie helpt u fouten oplossen wanneer u Azure AD Domain Services inschakelt voor uw directory.

Kies de stappen voor probleemoplossing die overeenkomen met het foutbericht dat u ondervindt.

| **Foutbericht** | **Naamomzetting** |
| --- |:--- |
| *De naam contoso100.com is al in gebruik op dit netwerk. Geef een naam op die niet in gebruik is.* |[Naamconflict domein in het virtuele netwerk](active-directory-ds-troubleshooting.md#domain-name-conflict) |
| *Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De service heeft niet voldoende machtigingen voor de toepassing met de naam 'Azure AD Domain Services Sync'. Verwijder de toepassing met de naam 'Azure AD Domain Services Sync' en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.* |[Domain Services heeft niet voldoende machtigingen voor de synchronisatie van Azure AD Domain Services-toepassing](active-directory-ds-troubleshooting.md#inadequate-permissions) |
| *Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Domain Services in uw Azure AD-tenant heeft niet de vereiste machtigingen om Domain Services in te schakelen. Verwijder de toepassing met de toepassings-id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.* |[De toepassing Domain Services is niet juist geconfigureerd in uw tenant](active-directory-ds-troubleshooting.md#invalid-configuration) |
| *Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Microsoft Azure AD is uitgeschakeld in uw Azure AD-tenant. Schakel de toepassing met de toepassings-id 00000002-0000-0000-c000-000000000000 in en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.* |[De toepassing Microsoft Graph is uitgeschakeld in uw Azure AD-tenant](active-directory-ds-troubleshooting.md#microsoft-graph-disabled) |

### <a name="domain-name-conflict"></a>Conflicten met domein
**Foutbericht:**

*De naam contoso100.com is al in gebruik op dit netwerk. Geef een naam op die niet in gebruik is.*

**Herstel:**

Zorg ervoor dat u geen een bestaand domein met dezelfde domeinnaam beschikbaar is op dit virtuele netwerk hebt. Het domein met de naam contoso.com is bijvoorbeeld al beschikbaar op het geselecteerde virtuele netwerk. Later kunt proberen u om in te schakelen van een beheerd domein van Azure AD Domain Services met dezelfde domeinnaam (dat wil zeggen, ' contoso.com') op dit virtuele netwerk. U kunt een fout optreedt tijdens het inschakelen van Azure AD Domain Services.

Deze fout komt door naamconflicten voor de domeinnaam op dit virtuele netwerk. In dit geval moet u een andere naam gebruiken om het beheerde domein van Azure AD Domain Services in te stellen. U kunt ook de inrichting van het bestaande domein ongedaan maken en doorgaan met het inschakelen van Azure AD Domain Services.

### <a name="inadequate-permissions"></a>Onvoldoende machtigingen
**Foutbericht:**

*Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De service heeft niet voldoende machtigingen voor de toepassing met de naam 'Azure AD Domain Services Sync'. Verwijder de toepassing met de naam 'Azure AD Domain Services Sync' en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.*

**Herstel:**

Controleer of er een toepassing met de naam 'Azure AD Domain Services Sync' in uw Azure AD-directory is. Als deze toepassing bestaat, verwijderen en Azure AD Domain Services opnieuw inschakelen.

Voer de volgende stappen uit om te controleren op de aanwezigheid van de toepassing en verwijderen, als de toepassing bestaat:

1. Navigeer naar de **toepassingen** gedeelte van uw Azure AD-directory in de [Azure-portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/StartboardApplicationsMenuBlade/AllApps/menuId/).
2. Selecteer **alle toepassingen** in de **weergeven** vervolgkeuzelijst. Selecteer **eventuele** in de **toepassingen status** vervolgkeuzelijst. Selecteer **eventuele** in de **toepassing zichtbaarheid** vervolgkeuzelijst.
3. Type **Azure AD Domain Services Sync** in het zoekvak. Als de toepassing bestaat, klikt u op en klik op de **verwijderen** knop op de werkbalk om deze te verwijderen.
4. Zodra u de toepassing hebt verwijderd, probeer het opnieuw inschakelen van Azure AD Domain Services.

### <a name="invalid-configuration"></a>Ongeldige configuratie
**Foutbericht:**

*Domain Services kan niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Domain Services in uw Azure AD-tenant heeft niet de vereiste machtigingen om Domain Services in te schakelen. Verwijder de toepassing met de toepassings-id d87dcbc6-a371-462e-88e3-28ad15ec4e64 en probeer vervolgens om Domain Services in te schakelen voor uw Azure AD-tenant.*

**Herstel:**

Controleer als u een toepassing met de naam 'AzureActiveDirectoryDomainControllerServices' (met een toepassings-id van d87dcbc6-a371-462e-88e3-28ad15ec4e64) in uw Azure AD-directory hebt. Als deze toepassing bestaat, moet u deze verwijderen en Azure AD Domain Services opnieuw inschakelen.

Gebruik de volgende PowerShell-script voor de toepassing zoeken en te verwijderen.

> [!NOTE]
> Dit script gebruikt **Azure AD PowerShell versie 2** cmdlets. Lees voor een volledige lijst van alle beschikbare cmdlets en om te downloaden van de module de [AzureAD PowerShell naslagdocumentatie](https://msdn.microsoft.com/library/azure/mt757189.aspx).
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

Domain Services kunnen niet worden ingeschakeld in deze Azure AD-tenant. De toepassing Microsoft Azure AD is uitgeschakeld in uw Azure AD-tenant. Inschakelen van de toepassing met de id-00000002-0000-0000-c000-000000000000 toepassing en probeer het vervolgens aan domeinservices inschakelen voor uw Azure AD-tenant.

**Herstel:**

Controleer als u een toepassing met de id 00000002-0000-0000-c000-000000000000 hebt uitgeschakeld. Deze toepassing is de Microsoft Azure AD-toepassing en biedt Graph API toegang tot uw Azure AD-tenant. Azure AD Domain Services moet deze toepassing opnieuw moet worden ingeschakeld om te synchroniseren van uw Azure AD-tenant aan uw beheerde domein.

U lost deze fout door deze toepassing inschakelen en probeer vervolgens te domeinservices inschakelen voor uw Azure AD-tenant.


## <a name="users-are-unable-to-sign-in-to-the-azure-ad-domain-services-managed-domain"></a>Gebruikers kunnen zich niet aanmelden bij het in Azure AD Domain Services beheerde domein
Als een of meer gebruikers in uw Azure AD-tenant kan niet aanmelden bij de zojuist gemaakte beheerd domein, voert u de volgende stappen uit:

* **Meld u aan met de UPN-indeling:** proberen aan te melden met behulp van de UPN-indeling (bijvoorbeeld 'joeuser@contoso.com') in plaats van de SAMAccountName-indeling (CONTOSO\joeuser). De SAMAccountName kan automatisch worden gegenereerd voor gebruikers waarvan UPN-voorvoegsel is te lang of hetzelfde is als een andere gebruiker op het beheerde domein. De UPN-indeling is gegarandeerd uniek zijn binnen een Azure AD-tenant.

> [!NOTE]
> Het wordt aangeraden om aan te melden bij het beheerde domein van Azure AD Domain Services met behulp van de UPN-indeling.
>
>

* Zorg ervoor dat u [wachtwoordsynchronisatie hebt ingeschakeld](active-directory-ds-getting-started-password-sync.md) volgens de stappen beschreven in de handleiding.
* **Externe accounts:** Zorg ervoor dat het account van de betrokken gebruiker niet een externe account in de Azure AD-tenant. Voorbeelden van externe accounts zijn Microsoft-accounts (bijvoorbeeld 'joe@live.com') of gebruikersaccounts uit een extern Azure AD-directory. Omdat Azure AD Domain Services heeft geen referenties voor dergelijke gebruikersaccounts, kunnen deze gebruikers zich niet aanmelden bij het beheerde domein.
* **Accounts die gesynchroniseerd:** als de betrokken gebruikersaccounts worden gesynchroniseerd vanuit een on-premises adreslijst, controleert u of die:

  * U hebt geïmplementeerd of bijgewerkt naar de [meest recente versie van Azure AD Connect aanbevolen](https://www.microsoft.com/en-us/download/details.aspx?id=47594).
  * U hebt Azure AD Connect naar geconfigureerd [een volledige synchronisatie uitvoeren](active-directory-ds-getting-started-password-sync.md).
  * Afhankelijk van de grootte van uw directory, kan het even duren voor gebruikersaccounts en referentie-hashes worden weergegeven in Azure AD Domain Services. Zorg ervoor dat u lang genoeg voordat u verificatie wachten.
  * Als het probleem zich blijft voordoen nadat u de voorgaande stappen hebt gecontroleerd, probeert u de Microsoft Azure AD Sync-Service opnieuw te starten. Op uw machine synchronisatie start vanaf de opdrachtprompt en voer de volgende opdrachten:

    1. net stop 'Microsoft Azure AD Sync'
    2. net start 'Microsoft Azure AD Sync'
* **Alleen in de cloud accounts**: als het account van de betrokken gebruiker een alleen-gebruikersaccount is, zorg ervoor dat de gebruiker het wachtwoord is gewijzigd nadat u Azure AD Domain Services hebt ingeschakeld. Door deze stap worden de referentie-hashes gegenereerd die zijn vereist voor Azure AD Domain Services.

## <a name="there-are-one-or-more-alerts-on-your-managed-domain"></a>Er zijn een of meer waarschuwingen op uw beheerde domein

Informatie over het oplossen van waarschuwingen op uw beheerde domein in via de [waarschuwingen oplossen](active-directory-ds-troubleshoot-alerts.md) artikel.

## <a name="users-removed-from-your-azure-ad-tenant-are-not-removed-from-your-managed-domain"></a>Gebruikers van uw Azure AD-tenant is verwijderd, worden niet verwijderd van uw beheerde domein
Azure AD voorkomt dat u onbedoeld gebruikersobjecten verwijdert. Wanneer u een gebruikersaccount uit uw Azure AD-tenant verwijdert, wordt het overeenkomstige gebruikersobject verplaatst naar de Prullenbak. Wanneer deze bewerking is gesynchroniseerd met uw beheerde domein, worden de bijbehorende gebruikersaccount zijn gemarkeerd als uitgeschakeld. Deze functie kunt u herstellen of het gebruikersaccount later ongedaan maken.

Het gebruikersaccount blijft uitgeschakeld in uw beheerde domein, zelfs als u een gebruikersaccount met dezelfde UPN opnieuw in uw Azure AD-directory maken. Als het gebruikersaccount uit uw beheerde domein verwijderen, moet u deze geforceerd verwijderen uit uw Azure AD-tenant.

Als het gebruikersaccount volledig uit uw beheerde domein verwijderen, de gebruiker verwijderen permanent van uw Azure AD-tenant. Gebruik de `Remove-MsolUser` PowerShell-cmdlet met de `-RemoveFromRecycleBin` optie, zoals beschreven in dit [MSDN-artikel](https://msdn.microsoft.com/library/azure/dn194132.aspx).


## <a name="contact-us"></a>Contact opnemen
Neem contact op met de Azure Active Directory Domain Services-productteam voor [feedback delen of voor ondersteuning](active-directory-ds-contact-us.md).

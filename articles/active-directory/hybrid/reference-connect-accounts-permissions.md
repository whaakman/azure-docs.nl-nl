---
title: 'Azure AD Connect: Accounts en machtigingen | Microsoft Docs'
description: Dit onderwerp beschrijft de accounts gebruikt en die zijn gemaakt en de machtigingen die vereist zijn.
services: active-directory
documentationcenter: ''
author: billmath
manager: mtillman
editor: ''
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/12/2018
ms.component: hybrid
ms.author: billmath
ms.openlocfilehash: a3bce69236586bcd0a250c47f1129ac0d94e8b26
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51231479"
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: Accounts en machtigingen

## <a name="accounts-used-for-azure-ad-connect"></a>Accounts die worden gebruikt voor Azure AD Connect

![](media/reference-connect-accounts-permissions/account5.png)

Azure AD Connect maakt gebruik van 3-accounts om te synchroniseren van gegevens van on-premises of Windows Server Active Directory naar Azure Active Directory.  Deze accounts zijn:

- **AD DS-Connector-account**: gebruikt voor het lezen/schrijven gegevens naar Windows Server Active Directory

- **Account van de service ADSync**: gebruikt voor het uitvoeren van de synchronisatieservice en toegang tot de SQL-database

- **Azure AD-Connector-account**: gebruikt voor het schrijven van gegevens in Azure AD

Naast deze drie accounts gebruikt voor het uitvoeren van Azure AD Connect, moet u ook de volgende extra accounts aan Azure AD Connect installeert.  Dit zijn:

- **AD DS Enterprise-beheerdersaccount**: gebruikt voor het installeren van Azure AD Connect
- **Account van Azure AD-hoofdbeheerder**: gebruikt voor het maken van de Azure AD-Connector-account en Azure AD configureren.

- **SQL-SA-account (optioneel)**: gebruikt voor het maken van de ADSync-database bij het gebruik van de volledige versie van SQL Server.  Deze SQL-Server kan lokaal of extern zijn aan de Azure AD Connect-installatie zijn.  Dit account is mogelijk niet hetzelfde account als de Enterprise-beheerder.  Inrichten van de database kan nu worden uitgevoerd buiten-band door de SQL-beheerder en vervolgens worden geïnstalleerd door de Azure AD Connect-beheerder met eigendomsrechten van de database.  Voor meer informatie over deze Zie [Installeer Azure AD Connect met behulp van SQL delegated administrator-machtigingen](how-to-connect-install-sql-delegation.md)

## <a name="installing-azure-ad-connect"></a>Azure AD Connect installeren
De installatiewizard van Azure AD Connect biedt twee verschillende paden:

* In de Express-instellingen, worden in de wizard meer bevoegdheden vereist.  Dit is zodat deze van uw configuratie eenvoudig, instellen kunt zonder dat u gebruikers maken of configureren van machtigingen.
* In de aangepaste instellingen biedt de wizard u meer keuzemogelijkheden en opties. Er zijn echter situaties waarin u nodig hebt om ervoor te zorgen dat u over de juiste machtigingen beschikt.



## <a name="express-settings-installation"></a>Snelle installatie van de instellingen
In de Express-instellingen vraagt de installatiewizard voor het volgende:

  - Enterprise-beheerder van AD DS-referenties
  - Azure AD-hoofdbeheerder referenties

### <a name="ad-ds-enterprise-admin-credentials"></a>AD DS ondernemingsadministratorreferenties
De AD DS-ondernemingsadministrator-account wordt gebruikt voor het configureren van uw on-premises Active Directory. Deze referenties worden alleen gebruikt tijdens de installatie en niet worden gebruikt nadat de installatie is voltooid. De Enterprise-beheerder, niet de domeinbeheerder moet ervoor zorgen dat de machtigingen in Active Directory kunnen worden ingesteld in alle domeinen zijn.

Als u een van DirSync upgrade, wordt de Ondernemingsadministrators van AD DS-referenties worden gebruikt om het opnieuw instellen van het wachtwoord voor het account wordt gebruikt door DirSync. Ook moet u Azure AD-hoofdbeheerder referenties.

### <a name="azure-ad-global-admin-credentials"></a>Azure AD-hoofdbeheerder referenties
Deze referenties worden alleen gebruikt tijdens de installatie en niet worden gebruikt nadat de installatie is voltooid. Wordt gebruikt voor het maken van de [Azure AD Connector-account](#azure-ad-service-account) gebruikt voor het synchroniseren van wijzigingen in Azure AD. Het account kunt synchronisatie ook als een functie in Azure AD.

### <a name="ad-ds-connector-account-required-permissions-for-express-settings"></a>AD DS-Connector-account vereist machtigingen voor de express-instellingen
De [Connector voor AD DS-account](#active-directory-account) is gemaakt voor het lezen en schrijven naar Windows Server AD en heeft de volgende machtigingen als die zijn gemaakt door de express-instellingen:

| Machtiging | Gebruikt voor |
| --- | --- |
| <li>Directorywijzigingen repliceren</li><li>Alle repliceren Directory gewijzigd |Synchronisatie van wachtwoordhashes |
| Lezen/schrijven alle eigenschappen van gebruiker |Import- en Exchange hybrid |
| Lezen/schrijven alle eigenschappen iNetOrgPerson |Import- en Exchange hybrid |
| Groep van alle eigenschappen voor lezen/schrijven |Import- en Exchange hybrid |
| Neem contact op met lezen/schrijven alle eigenschappen |Import- en Exchange hybrid |
| Wachtwoord opnieuw instellen |Voorbereiding voor het inschakelen van wachtwoord terugschrijven |

### <a name="express-installation-wizard-summary"></a>Snelle installatiewizard-overzicht

![Snelle installatie](./media/reference-connect-accounts-permissions/express.png)

Hier volgt een samenvatting van de snelle installatie wizardpagina's, de referenties die worden verzameld, en waarvoor ze worden gebruikt.

| Wizardpagina | Referenties die worden verzameld | Machtigingen die vereist zijn | Gebruikt voor |
| --- | --- | --- | --- |
| N/A |Gebruiker met de installatiewizard |Beheerder van de lokale server |<li>Hiermee maakt u de [ADSync-serviceaccount](#azure-ad-connect-sync-service-account) account dat over het uitvoeren van de synchronisatieservice wordt gebruikt. |
| Verbinding maken met Azure AD |Azure AD-directory-referenties |De rol globale beheerder in Azure AD |<li>Inschakelen van synchronisatie in de Azure AD-directory.</li>  <li>Het maken van de [Azure AD Connector-account](#azure-ad-service-account) die wordt gebruikt voor continue synchronisatiebewerkingen in Azure AD.</li> |
| Verbinding maken met AD DS |On-premises Active Directory-referenties |Lid van de groep Ondernemingsadministrators (EA) in Active Directory |<li>Hiermee maakt u de [Connector voor AD DS-account](#active-directory-account) in Active Directory en machtigingen verlenen aan deze. Dit account hebt gemaakt wordt om te lezen en schrijven van directory-informatie tijdens de synchronisatie gebruikt.</li> |


## <a name="custom-installation-settings"></a>Aangepaste installatie-instellingen

Met de installatie van de aangepaste instellingen, de wizard biedt u meer keuzemogelijkheden en opties. 

### <a name="custom-installation-wizard-summary"></a>Overzicht van de wizard Aangepaste installatie

Hier volgt een samenvatting van de aangepaste installatie wizardpagina's, de referenties die worden verzameld, en waarvoor ze worden gebruikt.

![Snelle installatie](./media/reference-connect-accounts-permissions/customize.png)

| Wizardpagina | Referenties die worden verzameld | Machtigingen die vereist zijn | Gebruikt voor |
| --- | --- | --- | --- |
| N/A |Gebruiker met de installatiewizard |<li>Beheerder van de lokale server</li><li>Als u een volledige SQL Server gebruikt, moet de gebruiker System Administrator (SA) in SQL</li> |Maakt standaard het lokale account dat wordt gebruikt als de [synchronisatie-engine-serviceaccount](#azure-ad-connect-sync-service-account). Het account wordt alleen gemaakt wanneer de beheerder geen een bepaald account geeft. |
| Synchronisatieservices, Service-account-optie installeren |AD of lokale gebruikersaccountreferenties |Gebruiker machtigingen zijn verleend door de installatiewizard |Als de beheerder een account is opgegeven, wordt dit account gebruikt als het serviceaccount voor de synchronisatieservice. |
| Verbinding maken met Azure AD |Azure AD-directory-referenties |De rol globale beheerder in Azure AD |<li>Inschakelen van synchronisatie in de Azure AD-directory.</li>  <li>Het maken van de [Azure AD Connector-account](#azure-ad-service-account) die wordt gebruikt voor continue synchronisatiebewerkingen in Azure AD.</li> |
| Verbinding maken met uw directory’s |On-premises Active Directory-referenties voor elk forest dat is verbonden met Azure AD |De machtigingen zijn afhankelijk van welke functies u inschakelen en kunt u vinden in [het AD DS-Connector-account maken](#create-the-ad-dso-connector-account) |Dit account wordt gebruikt om te lezen en schrijven van directory-informatie tijdens de synchronisatie. |
| AD FS-Servers |Voor elke server in de lijst verzamelt met de wizard referenties wanneer de aanmeldingsreferenties van de gebruiker die de wizard zijn niet voldoende om verbinding te maken |Domeinbeheerder |Installatie en configuratie van de AD FS-serverfunctie. |
| WAP-servers |Voor elke server in de lijst verzamelt met de wizard referenties wanneer de aanmeldingsreferenties van de gebruiker die de wizard zijn niet voldoende om verbinding te maken |Lokale beheerder op de doel-VM |Installatie en configuratie van WAP-server-rol. |
| Vertrouwde proxyreferenties |Federation-service vertrouwen referenties (de referenties van de proxy wordt gebruikt om in te schrijven voor een vertrouwensrelatie certificaat van de FS |Domeinaccount dat een lokale beheerder van de AD FS-server |Initiële inschrijving van FS WAP-vertrouwensrelatie certificaat. |
| Pagina voor AD FS-serviceaccount, "De accountoptie van een domein-gebruiker gebruiken" |Accountreferenties van de AD-gebruiker |Domeingebruiker |Het account van de AD-gebruiker waarvan de referenties zijn opgegeven, wordt gebruikt als het aanmeldingsaccount van de AD FS-service. |

### <a name="create-the-ad-ds-connector-account"></a>Het AD DS-Connector-account maken

>[!IMPORTANT]
>Een nieuwe PowerShell-Module met de naam ADSyncConfig.psm1 geïntroduceerd met build **1.1.880.0** (uitgebracht in augustus 2018) die bestaat uit een verzameling van cmdlets kunt u de juiste Active Directory-machtigingen voor de Azure AD DS configureren Connector-account.
>
>Zie voor meer informatie [Azure AD Connect: machtiging voor AD DS-Connector-Account configureren](how-to-connect-configure-ad-ds-connector-account.md)

Het account dat u opgeeft op de **verbinding maken met uw mappen** pagina moet aanwezig zijn in Active Directory voorafgaand aan de installatie.  Azure AD Connect versie 1.1.524.0 en hoger en later is de optie voor de Azure AD Connect-wizard maken gebruiken om de **Connector voor AD DS-account** gebruikt voor verbinding met Active Directory.  

Ook moet de vereiste machtigingen hebben. De installatiewizard controleert niet of dat de machtigingen en eventuele problemen zijn alleen tijdens de synchronisatie te vinden.

Welke machtigingen die u nodig hebt, is afhankelijk van de optionele functies inschakelen. Als u meerdere domeinen hebt, moeten de machtigingen worden verleend voor alle domeinen in het forest. Als u niet inschakelt dat een van deze functies, de standaard **domeingebruiker** machtigingen zijn voldoende.

| Functie | Machtigingen |
| --- | --- |
| MS-DS-ConsistencyGuid functie |Schrijfmachtigingen heeft voor het kenmerk ms-DS-ConsistencyGuid is beschreven in [ontwerpconcepten - ms-DS-ConsistencyGuid gebruiken als sourceAnchor](plan-connect-design-concepts.md#using-ms-ds-consistencyguid-as-sourceanchor). | 
| Synchronisatie van wachtwoordhashes |<li>Directorywijzigingen repliceren</li>  <li>Alle repliceren Directory gewijzigd |
| Hybride implementatie voor Exchange |Schrijfmachtigingen heeft voor de kenmerken beschreven in [Exchange hybrid terugschrijven](reference-connect-sync-attributes-synchronized.md#exchange-hybrid-writeback) voor gebruikers, groepen en contactpersonen. |
| Openbare map voor Exchange-e-Mail |Leesmachtigingen voor de kenmerken beschreven in [Exchange Mail openbare map](reference-connect-sync-attributes-synchronized.md#exchange-mail-public-folder) voor openbare mappen. | 
| Wachtwoord terugschrijven |Schrijfmachtigingen heeft voor de kenmerken beschreven in [aan de slag met wachtwoordbeheer](../authentication/howto-sspr-writeback.md) voor gebruikers. |
| Apparaat terugschrijven |Machtigingen met een PowerShell-script, zoals beschreven in [Write-back van apparaat](how-to-connect-device-writeback.md). |
| Groep terugschrijven |Lezen, maken, bijwerken en verwijderen van groep objecten voor gesynchroniseerd **Office 365-groepen**.  Zie voor meer informatie [Write-back van groep](how-to-connect-preview.md#group-writeback).|

## <a name="upgrade"></a>Upgraden
Wanneer u een upgrade van één versie van Azure AD Connect naar een nieuwe versie uitvoeren, moet u de volgende machtigingen:

>[!IMPORTANT]
>Beginnen met bouwen 1.1.484, Azure AD Connect een bug regressie waarvoor sysadmin-machtigingen voor het bijwerken van de SQL-database ingevoerd.  Deze fout is verholpen in build 1.1.647.  Als u een naar deze versie upgrade, moet u sysadmin-bevoegdheden.  Dbo-machtigingen zijn niet voldoende.  Als u probeert te Azure AD Connect upgraden zonder sysadmin-bevoegdheden, mislukt de upgrade en Azure AD Connect wordt niet meer correct werkt daarna.  Microsoft is hiervan op de hoogte en werkt om dit te corrigeren.


| Hoofd | Machtigingen die vereist zijn | Gebruikt voor |
| --- | --- | --- |
| Gebruiker met de installatiewizard |Beheerder van de lokale server |Binaire bestanden worden bijgewerkt. |
| Gebruiker met de installatiewizard |Lid van de ADSyncAdmins |Breng wijzigingen in synchronisatieregels en andere configuratie. |
| Gebruiker met de installatiewizard |Als u een volledige SQL server: DBO (of vergelijkbaar) van de synchronisatie-engine-database |Controleer op wijzigingen in de database, zoals het bijwerken van tabellen met nieuwe kolommen. |

## <a name="more-about-the-created-accounts"></a>Meer informatie over de gemaakte accounts
### <a name="ad-ds-connector-account"></a>AD DS-Connector-account
Als u express-instellingen, wordt klikt u vervolgens een account gemaakt in Active Directory die wordt gebruikt voor synchronisatie. Het account bevindt zich in het foresthoofddomein in de container gebruikers en heeft de naam ervan voorafgegaan door **MSOL_**. Het account is gemaakt met een lange complex wachtwoord in dat niet verloopt. Als u een wachtwoordbeleid in uw domein hebt, zorg ervoor dat lange en complexe wachtwoorden is toegestaan voor dit account.

![AD-account](./media/reference-connect-accounts-permissions/adsyncserviceaccount.png)

Als u aangepaste instellingen, klikt bent u verantwoordelijk voor het account te maken voordat u begint met de installatie.  Zie [het AD DS-Connector-account maken](#create-the-ad-dso-connector-account).

### <a name="adsync-service-account"></a>ADSync-serviceaccount
De synchronisatieservice kan uitvoeren onder verschillende accounts. Het kan worden uitgevoerd onder een **Virtual Service Account** (leverancierspecifiek Kenmerk), een **groep beheerd serviceaccount** (gMSA/sMSA), of een normaal gebruikersaccount. De ondersteunde opties zijn gewijzigd met de 2017 April versie van Connect wanneer u een nieuwe installatie uitvoeren. Als u een upgrade vanaf een eerdere versie van Azure AD Connect uitvoert, zijn deze extra opties niet beschikbaar.

| Type account | Installatie-optie | Beschrijving |
| --- | --- | --- |
| [Virtual serviceaccount](#virtual-service-account) | Express en aangepast, 2017 April en hoger | Dit is de optie gebruikt voor alle installaties van snelle, met uitzondering van installaties op een domeincontroller. Voor aangepaste, is er op de standaardoptie, tenzij een andere optie wordt gebruikt. |
| [Groep beheerd serviceaccount](#group-managed-service-account) | Aangepaste, 2017 April en hoger | Als u een externe SQL server gebruikt, klikt u vervolgens het beste aan een groep beheerd serviceaccount gebruiken. |
| [Gebruikersaccount](#user-account) | Express en aangepast, 2017 April en hoger | Een gebruikersaccount voorafgegaan door AAD_ wordt alleen gemaakt tijdens de installatie, indien geïnstalleerd op Windows Server 2008 en wanneer geïnstalleerd op een domeincontroller. |
| [Gebruikersaccount](#user-account) | Express en aangepast, 2017 maart en oudere versies | Een lokaal account voorafgegaan door AAD_ is gemaakt tijdens de installatie. Wanneer u aangepaste installatie gebruikt, kan een ander account kan worden opgegeven. |

Als u verbinding maken met een build van 2017 maart of eerder, klikt u vervolgens u moet niet het wachtwoord opnieuw op het serviceaccount instellen sinds Windows de versleutelingssleutels voor opmaaktalen wordt om beveiligingsredenen vernietigt. U kunt het account niet wijzigen naar een ander account zonder Azure AD Connect opnieuw te installeren. Als u naar een build 2017 April upgraden of later, het wordt wijzigen ondersteund als u wilt wijzigen van het wachtwoord voor het serviceaccount, maar u niet het account dat wordt gebruikt.

> [!Important]
> U kunt alleen het serviceaccount instellen op de eerste installatie. Het serviceaccount te wijzigen nadat de installatie is voltooid wordt niet ondersteund.

Dit is een tabel van de standaard, aanbevolen en ondersteunde opties voor het serviceaccount voor synchronisatie.

Legenda:

- **Vet** geeft aan dat de standaardoptie en in de meeste gevallen de aanbevolen optie.
- *Cursief* geeft aan dat de aanbevolen optie wanneer het is niet standaard ingeschakeld.
- 2008 - optie standaard geïnstalleerd op Windows Server 2008
- Niet-vet: ondersteunde optie
- Lokaal account - lokale gebruikersaccount op de server
- Domeinaccount - domeingebruikersaccount
- sMSA - [zelfstandig beheerd serviceaccount](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [groep beheerd serviceaccount](https://technet.microsoft.com/library/hh831782.aspx)

| | LocalDB</br>Express | LocalDB/LocalSQL</br>Aangepast telefoonnummer | Externe SQL</br>Aangepast telefoonnummer |
| --- | --- | --- | --- |
| **zelfstandige/werkgroep-machine** | Niet ondersteund | **VSA**</br>Lokaal account (2008)</br>Lokaal account |  Niet ondersteund |
| **domein machine** | **VSA**</br>Lokaal account (2008) | **VSA**</br>Lokaal account (2008)</br>Lokaal account</br>Domeinaccount</br>sMSA, gMSA | **gMSA**</br>Domeinaccount |
| **Domeincontroller** | **Domeinaccount** | *gMSA*</br>**Domeinaccount**</br>sMSA| *gMSA*</br>**Domeinaccount**|

#### <a name="virtual-service-account"></a>Virtuele-serviceaccount
Een virtueel serviceaccount is een speciaal type account dat geen een wachtwoord en wordt beheerd door Windows.

![VSA](./media/reference-connect-accounts-permissions/aadsyncvsa.png)

De leverancierspecifiek Kenmerk is bedoeld om te worden gebruikt met scenario's waarbij de synchronisatie-engine en SQL zijn op dezelfde server. Als u externe SQL, wordt aangeraden om te gebruiken een [groep beheerd serviceaccount](#managed-service-account) in plaats daarvan.

Deze functie moet Windows Server 2008 R2 of hoger. Als u Azure AD Connect op Windows Server 2008 installeren, wordt de installatie van de terugvalt op het met behulp van een [gebruikersaccount](#user-account) in plaats daarvan.

#### <a name="group-managed-service-account"></a>Groep beheerd serviceaccount
Als u een externe SQL server gebruikt, wordt aangeraden voor het gebruik van een **groep beheerd serviceaccount**. Zie voor meer informatie over het voorbereiden van uw Active Directory voor een groep beheerd serviceaccount [Group Managed Service Accounts Overview](https://technet.microsoft.com/library/hh831782.aspx).

Deze optie wilt gebruiken, op de [vereiste onderdelen installeren](how-to-connect-install-custom.md#install-required-components) weergeeft, schakelt **een bestaand serviceaccount gebruiken**, en selecteer **beheerd serviceaccount**.  
![VSA](./media/reference-connect-accounts-permissions/serviceaccount.png)  
Deze functie wordt ook ondersteund voor het gebruik van een [zelfstandig beheerd serviceaccount](https://technet.microsoft.com/library/dd548356.aspx). Maar deze kunnen alleen worden gebruikt op de lokale computer en er is geen voordeel om ze te gebruiken via de standaard virtuele-serviceaccount.

Deze functie moet WindowsServer 2012 of hoger. Als u wilt gebruiken van een ouder besturingssysteem en gebruiken van externe SQL, dan moet u een [gebruikersaccount](#user-account).

#### <a name="user-account"></a>Gebruikersaccount
Een lokale service-account wordt gemaakt door de installatiewizard (tenzij u het account moet worden gebruikt in de aangepaste instellingen opgeven). Het account wordt voorafgegaan **AAD_** en gebruikt voor de werkelijke sync-service uit te voeren als. Als u Azure AD Connect op een domeincontroller installeert, moet het account wordt gemaakt in het domein. De **AAD_** -serviceaccount moet zich bevinden in het domein als:
   - u een externe server met SQL server gebruiken
   - gebruik van een proxy die verificatie vereist

![Serviceaccount voor synchronisatie](./media/reference-connect-accounts-permissions/syncserviceaccount.png)

Het account is gemaakt met een lange complex wachtwoord in dat niet verloopt.

Dit account wordt gebruikt voor het opslaan van de wachtwoorden voor de andere accounts op een veilige manier. Deze andere accounts wachtwoorden worden versleuteld opgeslagen in de database. De persoonlijke sleutels voor de versleutelingssleutels zijn beveiligd met de cryptografische services geheime sleutel versleuteling met behulp van Windows Data Protection API (DPAPI).

Als u een volledige SQL Server gebruikt, is de serviceaccount de DBO van de database is gemaakt voor de synchronisatie-engine. De service werkt niet zoals bedoeld met andere machtigingen. Een SQL-aanmelding wordt ook gemaakt.

Het account ook krijgt de machtigingen voor bestanden, registersleutels en andere objecten met betrekking tot de synchronisatie-Engine.

### <a name="azure-ad-connector-account"></a>Azure AD-Connector-account
Een account in Azure AD is bedoeld voor gebruik van de sync-service. Dit account kan worden geïdentificeerd door de naam weergegeven.

![AD-account](./media/reference-connect-accounts-permissions/aadsyncserviceaccount2.png)

De naam van het account wordt gebruikt op de-server kan worden geïdentificeerd in het tweede gedeelte van de naam van de gebruiker. In de gaten is de naam van de server DC1. Als u de staging-servers hebt, heeft elke server een eigen account.

Het account is gemaakt met een lange complex wachtwoord in dat niet verloopt. Een speciale rol zijn toegekend **Directory-Accounts voor synchronisatie** waarvoor alleen machtigingen voor het uitvoeren van taken voor directory-synchronisatie. Deze speciale ingebouwde rol kan niet worden toegekend buiten de Azure AD Connect-wizard. De Azure-portal ziet u dit account met de rol **gebruiker**.

Er is een limiet van 20 sync-service-accounts in Azure AD. Als u de lijst met bestaande Azure AD-service-accounts in uw Azure AD, moet u de volgende Azure AD PowerShell-cmdlet uitvoeren: `Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

Verwijderen van niet-gebruikte Azure AD-serviceaccounts, voer de volgende Azure AD PowerShell-cmdlet uit: `Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

## <a name="related-documentation"></a>Verwante documentatie
Als u hebt niet de documentatie lezen op [uw on-premises identiteiten integreren met Azure Active Directory](whatis-hybrid-identity.md), de volgende tabel vindt u koppelingen naar verwante onderwerpen.

|Onderwerp |Koppeling|  
| --- | --- |
|Azure AD Connect downloaden | [Azure AD Connect downloaden](https://go.microsoft.com/fwlink/?LinkId=615771)|
|Installeren met behulp van snelle instellingen | [Snelle installatie van Azure AD Connect](how-to-connect-install-express.md)|
|Installeren met behulp van aangepaste instellingen | [Aangepaste installatie van Azure AD Connect](./how-to-connect-install-custom.md)|
|Upgraden van DirSync | [Upgraden van Azure AD-synchronisatiehulpprogramma (DirSync)](how-to-dirsync-upgrade-get-started.md)|
|Na installatie | [De installatie controleren en licenties toewijzen](how-to-connect-post-installation.md)|

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](whatis-hybrid-identity.md).

---
title: 'Azure AD Connect: Accounts en machtigingen | Microsoft Docs'
description: Dit onderwerp beschrijft de accounts gebruikt en gemaakt en de vereiste machtigingen.
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.reviewer: cychua
ms.assetid: b93e595b-354a-479d-85ec-a95553dd9cc2
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2017
ms.author: billmath
ms.openlocfilehash: cde406bd745fe61757eaa69c9fc0cfc98a42d205
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/11/2017
---
# <a name="azure-ad-connect-accounts-and-permissions"></a>Azure AD Connect: De Accounts en machtigingen
De Azure AD Connect-installatiewizard biedt twee verschillende paden:

* In de Express-instellingen in de wizard meer bevoegdheden vereist.  Dit is zodat deze kan ingesteld van de configuratie eenvoudig, zonder dat u wilt maken van gebruikers of configureren van machtigingen.
* In de aangepaste instellingen, de wizard biedt u meer en opties. Er zijn echter situaties waarin moet u ervoor zorgen dat u over de juiste machtigingen beschikt.

## <a name="related-documentation"></a>Verwante documentatie
Als u hebt niet de documentatie te lezen op [uw on-premises identiteiten integreren met Azure Active Directory](../active-directory-aadconnect.md), de volgende tabel bevat koppelingen naar verwante onderwerpen.

|Onderwerp |Koppeling|  
| --- | --- |
|Azure AD Connect downloaden | [Azure AD Connect downloaden](http://go.microsoft.com/fwlink/?LinkId=615771)|
|Installeren met behulp van snelle instellingen | [Snelle installatie van Azure AD Connect](./active-directory-aadconnect-get-started-express.md)|
|Installeren met behulp van aangepaste instellingen | [Aangepaste installatie van Azure AD Connect](./active-directory-aadconnect-get-started-custom.md)|
|Upgraden van DirSync | [Upgraden van Azure AD-synchronisatiehulpprogramma (DirSync)](./active-directory-aadconnect-dirsync-upgrade-get-started.md)|
|Na installatie | [De installatie controleren en licenties toewijzen](active-directory-aadconnect-whats-next.md)|

## <a name="express-settings-installation"></a>Snelle instellingen voor installatie
In instellingen voor snelle installatie wordt gevraagd om AD DS-ondernemingsadministrator referenties.  Dit is uw lokale Active Directory kunnen worden geconfigureerd met de vereiste machtigingen voor Azure AD Connect. Als u een van DirSync upgrade, worden de Ondernemingsadministrators van AD DS-referenties gebruikt voor het opnieuw instellen van het wachtwoord voor het account dat wordt gebruikt door DirSync. U moet ook een globale beheerder van Azure AD-referenties.

| Pagina van wizard | Referenties die worden verzameld | Machtigingen die vereist zijn | Gebruikt voor |
| --- | --- | --- | --- |
| N.v.t. |Gebruiker met de installatiewizard |De beheerder van de lokale server |<li>Hiermee maakt u het lokale account dat wordt gebruikt als de [sync engine-serviceaccount](#azure-ad-connect-sync-service-account). |
| Verbinding maken met Azure AD |Azure AD-directory-referenties |De rol globale beheerder in Azure AD |<li>In de Azure AD-directory-synchronisatie inschakelen.</li>  <li>Het maken van de [Azure AD-account](#azure-ad-service-account) die wordt gebruikt voor continu synchronisatiebewerkingen in Azure AD.</li> |
| Verbinding maken met AD DS |On-premises Active Directory-referenties |Lid van de groep Ondernemingsadministrators (EA) in Active Directory |<li>Maakt een [account](#active-directory-account) in Active Directory en machtigingen verleent. Dit account hebt gemaakt wordt te lezen en schrijven van directory-informatie tijdens de synchronisatie gebruikt.</li> |

### <a name="enterprise-admin-credentials"></a>Enterprise-referenties
Deze referenties worden alleen gebruikt tijdens de installatie en worden niet gebruikt nadat de installatie is voltooid. De ondernemingsbeheerder niet de domeinbeheerder moet ervoor zorgen dat de machtigingen in Active Directory kunnen worden ingesteld in alle domeinen zijn.

### <a name="global-admin-credentials"></a>Referenties van de globale beheerder
Deze referenties worden alleen gebruikt tijdens de installatie en worden niet gebruikt nadat de installatie is voltooid. Wordt gebruikt voor het maken van de [Azure AD-account](#azure-ad-service-account) gebruikt voor het synchroniseren van wijzigingen naar Azure AD. Het account kunt synchronisatie ook als een functie in Azure AD.

### <a name="permissions-for-the-created-ad-ds-account-for-express-settings"></a>Machtigingen voor de gemaakte AD DS-account voor snelle instellingen
De [account](#active-directory-account) gemaakt voor de volgende machtigingen wanneer gemaakt met expresinstellingen worden gelezen en geschreven naar AD DS:

| Machtiging | Gebruikt voor |
| --- | --- |
| <li>Directorywijzigingen repliceren</li><li>Alle repliceren Directory gewijzigd |Wachtwoordsynchronisatie |
| Lezen/schrijven alle eigenschappen van gebruiker |Importeren en Exchange hybrid |
| Lezen/schrijven alle eigenschappen iNetOrgPerson |Importeren en Exchange hybrid |
| Lezen/schrijven-groep voor alle eigenschappen |Importeren en Exchange hybrid |
| Neem contact op met lees-/ schrijffout alle eigenschappen |Importeren en Exchange hybrid |
| Wachtwoord opnieuw instellen |Voorbereiding voor het wachtwoord terugschrijven inschakelen |

## <a name="custom-settings-installation"></a>Instellingen voor aangepaste installatie
Azure AD Connect versie 1.1.524.0 en later is de optie om de Azure AD Connect-wizard maken van het account waarmee verbinding met Active Directory.  Eerdere versies is vereist dat het account is gemaakt voordat de installatie. De machtigingen die u moet dit account de machtigingen kunnen worden gevonden in [het AD DS-account maken](#create-the-ad-ds-account). 

| Pagina van wizard | Referenties die worden verzameld | Machtigingen die vereist zijn | Gebruikt voor |
| --- | --- | --- | --- |
| N.v.t. |Gebruiker met de installatiewizard |<li>De beheerder van de lokale server</li><li>Als een volledige SQL Server wordt gebruikt, moet de gebruiker System Administrator (SA) in SQL</li> |Maakt standaard het lokale account dat wordt gebruikt als de [sync engine-serviceaccount](#azure-ad-connect-sync-service-account). Het account wordt alleen gemaakt wanneer de beheerder geen een bepaalde account geeft. |
| Installeren van de Synchronisatieservices, optie voor Service-account |AD of lokale gebruikersaccountreferenties |Gebruiker met machtigingen worden verleend door de installatiewizard |Als de beheerder een account opgeeft, wordt dit account gebruikt als het serviceaccount voor de synchronisatieservice. |
| Verbinding maken met Azure AD |Azure AD-directory-referenties |De rol globale beheerder in Azure AD |<li>In de Azure AD-directory-synchronisatie inschakelen.</li>  <li>Het maken van de [Azure AD-account](#azure-ad-service-account) die wordt gebruikt voor continu synchronisatiebewerkingen in Azure AD.</li> |
| Verbinding maken met uw directory’s |On-premises Active Directory-referenties voor elke forest die is verbonden met Azure AD |De machtigingen zijn afhankelijk van welke functies u inschakelen en kunt u vinden in [het AD DS-account maken](#create-the-ad-ds-account) |Dit account wordt gebruikt om te lezen en schrijven van directory-informatie tijdens de synchronisatie. |
| AD FS-Servers |Voor elke server in de lijst verzamelt met de wizard referenties wanneer de referenties voor de aanmelding van gebruiker die de wizard onvoldoende zijn om te verbinden |Domeinbeheerder |Installatie en configuratie van de AD FS-serverfunctie. |
| Webtoepassingsproxyservers |Voor elke server in de lijst verzamelt met de wizard referenties wanneer de referenties voor de aanmelding van gebruiker die de wizard onvoldoende zijn om te verbinden |Lokale beheerder op de doelcomputer |Installatie en configuratie van WAP-serverfunctie. |
| Vertrouwde proxyreferenties |Federatieservice vertrouwen referenties (de referenties op de proxy wordt gebruikt om in te schrijven voor een certificaat van de vertrouwensrelatie van de FS |Domeinaccount die een lokale beheerder van de AD FS-server |Initiële inschrijving van FS WAP vertrouwensrelatie certificaat. |
| AD FS-serviceaccount pagina "Gebruiken een optie gebruiker account" |AD-gebruikersaccountreferenties |Domeingebruiker |De AD-gebruikersaccount met de referenties die zijn opgegeven wordt als de aanmeldingsaccount van de AD FS-service gebruikt. |

### <a name="create-the-ad-ds-account"></a>Het AD DS-account maken
Het account dat u opgeeft op de **verbinding maken met uw adreslijsten** pagina moet aanwezig zijn in Active Directory voorafgaand aan de installatie.  Ook moet het de vereiste machtigingen verleend. De installatiewizard controleert niet of dat de machtigingen en eventuele problemen zijn alleen gevonden tijdens de synchronisatie.

Machtigingen die u nodig hebt, is afhankelijk van de optionele functies inschakelen. Als u meerdere domeinen hebt, moeten de machtigingen worden verleend voor alle domeinen in het forest. Indien u niet een van deze functies, de standaard **domeingebruiker** machtigingen voldoende zijn.

| Functie | Machtigingen |
| --- | --- |
| de functie msDS-ConsistencyGuid |Schrijfmachtigingen voor het kenmerk msDS-ConsistencyGuid is beschreven in [ontwerpconcepten - msDS-ConsistencyGuid met als sourceAnchor](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). | 
| Wachtwoordsynchronisatie |<li>Directorywijzigingen repliceren</li>  <li>Alle repliceren Directory gewijzigd |
| Hybride implementatie voor Exchange |Schrijfmachtigingen heeft voor de kenmerken die zijn beschreven in [Exchange hybride terugschrijven](active-directory-aadconnectsync-attributes-synchronized.md#exchange-hybrid-writeback) voor gebruikers, groepen en contactpersonen. |
| Exchange Mail openbare map |Leesmachtigingen voor de kenmerken die zijn beschreven in [Exchange Mail openbare map](active-directory-aadconnectsync-attributes-synchronized.md#exchange-mail-public-folder) voor openbare mappen. | 
| Wachtwoord terugschrijven |Schrijfmachtigingen heeft voor de kenmerken die zijn beschreven in [aan de slag met wachtwoordbeheer](../active-directory-passwords-writeback.md) voor gebruikers. |
| Apparaat terugschrijven |Machtigingen verleend met een PowerShell-script, zoals beschreven in [apparaat terugschrijven](active-directory-aadconnect-feature-device-writeback.md). |
| Groep terugschrijven |Lezen, maken, bijwerken en verwijderen van groep objecten voor gesynchroniseerd **Office 365-groepen**.  Zie voor meer informatie [Write-back van groep](active-directory-aadconnect-feature-preview.md#group-writeback).|

## <a name="upgrade"></a>Upgraden
Wanneer u een van één versie van Azure AD Connect naar een nieuwe versie upgrade, moet u de volgende machtigingen:

>[!IMPORTANT]
>Beginnen met build 1.1.484, Azure AD Connect geïntroduceerd een bug regressie waarvoor sysadmin-machtigingen voor de SQL-database bijwerken.  Deze fout is nog steeds aanwezig zijn in de laatste build 1.1.614.  Als u naar deze versie upgraden wilt, moet u sysadmin-bevoegdheden.  Dbo-machtigingen zijn niet voldoende.  Als u probeert bij te werken van Azure AD Connect zonder sysadmin-bevoegdheden, mislukt de upgrade en Azure AD Connect wordt niet meer correct werkt daarna.  Microsoft is op de hoogte en werkt om dit te corrigeren.


| Hoofd | Machtigingen die vereist zijn | Gebruikt voor |
| --- | --- | --- |
| Gebruiker met de installatiewizard |De beheerder van de lokale server |Binaire bestanden worden bijgewerkt. |
| Gebruiker met de installatiewizard |Lid van ADSyncAdmins |Breng wijzigingen in synchronisatie-regels en andere configuratie. |
| Gebruiker met de installatiewizard |Als u een volledige SQL server gebruikt: DBO (of vergelijkbaar) van de synchronisatie-engine-database |Database niveau wijzigingen aanbrengen, zoals het bijwerken van tabellen met nieuwe kolommen. |

## <a name="more-about-the-created-accounts"></a>Meer informatie over de gemaakte accounts
### <a name="active-directory-account"></a>Active Directory-account
Als u snelle instellingen gebruikt, wordt een account gemaakt in Active Directory dat wordt gebruikt voor synchronisatie. Het gemaakte account bevindt zich in het foresthoofddomein in de container gebruikers en heeft de naam ervan voorafgegaan door **MSOL_**. Het account is gemaakt met een lange complex wachtwoord dat niet verloopt. Als u een wachtwoordbeleid in uw domein hebt, zorg ervoor dat lange en complexe wachtwoorden zou worden toegestaan voor dit account.

![AD-account](./media/active-directory-aadconnect-accounts-permissions/adsyncserviceaccount.png)

Als u aangepaste instellingen, klikt u vervolgens bent u verantwoordelijk voor het maken van het account voordat u de installatie start.

### <a name="azure-ad-connect-sync-service-account"></a>Azure AD Connect sync-serviceaccount
De synchronisatieservice kunt uitvoeren onder verschillende accounts. Kunnen worden uitgevoerd binnen een **virtuele serviceaccount** (leverancierspecifiek Kenmerk), een **groep beheerd serviceaccount gebruiken** (gMSA/sMSA), of een normaal gebruikersaccount. De ondersteunde opties zijn gewijzigd met de 2017 release van April van verbinding maken wanneer u een nieuwe installatie doen. Als u een van een oudere versie van Azure AD Connect upgrade, zijn deze extra opties niet beschikbaar.

| Type account | Installatie-optie | Beschrijving |
| --- | --- | --- |
| [Virtuele-serviceaccount](#virtual-service-account) | Snelle en aangepaste, 2017 April en hoger | Dit is de optie gebruikt voor alle installaties van snelle, met uitzondering van installaties op een domeincontroller. Voor aangepaste is dit de standaardoptie tenzij een andere optie wordt gebruikt. |
| [Groep beheerd serviceaccount](#group-managed-service-account) | Aangepaste, 2017 April en hoger | Als u een externe SQL server gebruikt, vervolgens wordt aangeraden een groep beheerd serviceaccount moet worden gebruikt. |
| [Gebruikersaccount](#user-account) | Snelle en aangepaste, 2017 April en hoger | Een gebruikersaccount dat wordt voorafgegaan door AAD_ wordt alleen gemaakt tijdens de installatie, indien geïnstalleerd op Windows Server 2008 en wanneer geïnstalleerd op een domeincontroller. |
| [Gebruikersaccount](#user-account) | Snelle en aangepaste, 2017 maart en eerdere versies | Een lokaal account voorafgegaan door AAD_ is gemaakt tijdens de installatie. Wanneer u aangepaste installatie, kan een ander account worden opgegeven. |

Als u verbinding maken met een build van 2017 maart of eerder, vervolgens u moet niet het wachtwoord opnieuw op de serviceaccount instellen sinds Windows worden de versleutelingssleutels uit veiligheidsoverwegingen vernietigd. U kunt het account niet wijzigen naar een andere account zonder Azure AD Connect opnieuw te installeren. Als u een naar een build van 2017 April upgrade of hoger, het wordt ondersteund als u wilt wijzigen van het wachtwoord voor het serviceaccount, maar u het account wijzigen niet gebruikt.

> [!Important]
> U kunt alleen het serviceaccount instellen op de eerste installatie. Het serviceaccount niet wijzigen nadat de installatie is voltooid wordt niet ondersteund.

Dit is een tabel van de standaard, aanbevolen en ondersteunde opties voor het account van de synchronisatieservice.

Legenda:

- **Vet** duidt de standaardoptie en wordt in de meeste gevallen is de aanbevolen optie.
- *Cursief* geeft de aanbevolen optie bij niet de standaardoptie is.
- 2008 - standaard optie indien geïnstalleerd op Windows Server 2008
- Niet-vette - ondersteunde optie
- Lokaal account - lokale gebruikersaccount op de server
- Domeinaccount - domeingebruikersaccount
- sMSA - [zelfstandig beheerd serviceaccount](https://technet.microsoft.com/library/dd548356.aspx)
- gMSA - [beheerd serviceaccount gebruiken](https://technet.microsoft.com/library/hh831782.aspx)

| | LocalDB</br>Express | LocalDB/LocalSQL</br>Aangepast telefoonnummer | Externe SQL</br>Aangepast telefoonnummer |
| --- | --- | --- | --- |
| **zelfstandige/werkgroep-machine** | Niet ondersteund | **LEVERANCIERSPECIFIEK KENMERK**</br>Lokale account (2008)</br>Lokaal account |  Niet ondersteund |
| **domein-machine** | **LEVERANCIERSPECIFIEK KENMERK**</br>Lokale account (2008) | **LEVERANCIERSPECIFIEK KENMERK**</br>Lokale account (2008)</br>Lokaal account</br>Domeinaccount</br>sMSA, gMSA | **gMSA**</br>Domeinaccount |
| **Domeincontroller** | **Domeinaccount** | *gMSA*</br>**Domeinaccount**</br>sMSA| *gMSA*</br>**Domeinaccount**|

#### <a name="virtual-service-account"></a>Virtuele-serviceaccount
Een virtuele-serviceaccount is een speciaal type account dat u hebt geen wachtwoord en wordt beheerd door Windows.

![LEVERANCIERSPECIFIEK KENMERK](./media/active-directory-aadconnect-accounts-permissions/aadsyncvsa.png)

De leverancierspecifiek Kenmerk is bedoeld voor gebruik met scenario's waarbij de synchronisatie-engine en SQL zijn op dezelfde server. Als u externe SQL, wordt aangeraden om te gebruiken een [groep beheerd serviceaccount gebruiken](#managed-service-account) in plaats daarvan.

Deze functie moet Windows Server 2008 R2 of hoger. Als u Azure AD Connect op Windows Server 2008 installeren, wordt de installatie terugvalt op het met een [gebruikersaccount](#user-account) in plaats daarvan.

#### <a name="group-managed-service-account"></a>Groep beheerd serviceaccount
Als u een externe SQL server gebruiken, wordt aangeraden met behulp van een **groep beheerd serviceaccount**. Zie voor meer informatie over het voorbereiden van uw Active Directory voor groep beheerd serviceaccount [Group Managed Service Accounts Overview](https://technet.microsoft.com/library/hh831782.aspx).

Deze optie gebruiken op de [vereiste onderdelen installeren](active-directory-aadconnect-get-started-custom.md#install-required-components) pagina **een bestaand serviceaccount gebruiken**, en selecteer **beheerd serviceaccount**.  
![LEVERANCIERSPECIFIEK KENMERK](./media/active-directory-aadconnect-accounts-permissions/serviceaccount.png)  
Dit wordt ook ondersteund voor het gebruik van een [zelfstandig beheerd serviceaccount](https://technet.microsoft.com/library/dd548356.aspx). Echter deze kunnen alleen worden gebruikt op de lokale computer en geen voordeel met ze te gebruiken via de standaard virtuele-serviceaccount.

Deze functie moet WindowsServer 2012 of later. Als u wilt gebruiken een ouder besturingssysteem en gebruiken van externe SQL, dan moet u een [gebruikersaccount](#user-account).

#### <a name="user-account"></a>Gebruikersaccount
Een lokale service-account wordt gemaakt door de installatiewizard (tenzij u het account moet worden gebruikt in de aangepaste instellingen opgeven). Het voorvoegsel van het account **AAD_** en gebruikt voor de werkelijke synchronisatieservice uit te voeren als. Als u Azure AD Connect op een domeincontroller installeert, wordt het account gemaakt in het domein. De **AAD_** -serviceaccount moet zich bevinden in het domein als:
   - gebruik van een externe server met SQL server
   - gebruik van een proxy die verificatie vereist

![Serviceaccount voor synchronisatie](./media/active-directory-aadconnect-accounts-permissions/syncserviceaccount.png)

Het account is gemaakt met een lange complex wachtwoord dat niet verloopt.

Dit account wordt gebruikt voor het opslaan van de wachtwoorden voor de andere accounts in een veilige manier. Deze andere accounts wachtwoorden zijn versleuteld in de database opgeslagen. De persoonlijke sleutels voor de versleutelingssleutels die zijn beveiligd met de cryptografische services geheime sleutel versleuteling met behulp van Windows Data Protection API (DPAPI).

Als u een volledige SQL Server gebruikt, is het serviceaccount de DBO van de database is gemaakt voor de synchronisatie-engine. De service werkt niet zoals bedoeld met andere machtigingen. Een SQL-aanmelding wordt ook gemaakt.

Het account wordt ook machtigingen voor bestanden, registersleutels en andere objecten die betrekking hebben op de synchronisatie-Engine.

### <a name="azure-ad-service-account"></a>Azure AD-serviceaccount
Een account in Azure AD is voor de synchronisatieservice gemaakt. Dit account kan worden geïdentificeerd door de weergavenaam.

![AD-account](./media/active-directory-aadconnect-accounts-permissions/aadsyncserviceaccount.png)

De naam van de server die het account wordt gebruikt op kan worden geïdentificeerd in het tweede gedeelte van de gebruikersnaam. Naam van de server is in de afbeelding FABRIKAMCON. Als u staging-servers hebt, heeft elke server een eigen account.

Het serviceaccount is gemaakt met een lange complex wachtwoord dat niet verloopt. Een speciale functie zijn toegekend **Directory-synchronisatie Accounts** die alleen machtigingen heeft voor directory-synchronisatietaken uitvoeren. Deze speciale ingebouwde rol kan niet worden toegekend tot buiten de Azure AD Connect-wizard. De Azure-portal ziet u dit account met de rol **gebruiker**.

Er is een limiet van 20 sync-service-accounts in Azure AD. Als u de lijst met bestaande Azure AD-service-accounts in uw Azure AD, moet u de volgende Azure AD PowerShell-cmdlet uitvoeren:`Get-AzureADDirectoryRole | where {$_.DisplayName -eq "Directory Synchronization Accounts"} | Get-AzureADDirectoryRoleMember`

Verwijderen van ongebruikte Azure AD-serviceaccounts, voer de volgende Azure AD PowerShell-cmdlet:`Remove-AzureADUser -ObjectId <ObjectId-of-the-account-you-wish-to-remove>`

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](../active-directory-aadconnect.md).

---
title: 'Azure AD Connect: Versiegeschiedenis van release | Microsoft Docs'
description: Dit artikel vindt u alle versies van Azure AD Connect en Azure AD Sync
services: active-directory
documentationcenter: 
author: billmath
manager: mtillman
editor: 
ms.assetid: ef2797d7-d440-4a9a-a648-db32ad137494
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 12/14/2017
ms.author: billmath
ms.openlocfilehash: ff43edc9799670fd90beaef1dbe4db48b2e762e5
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="azure-ad-connect-version-release-history"></a>Azure AD Connect: Versiegeschiedenis van release
Azure AD Connect het team van Azure Active Directory (Azure AD) regelmatig bijgewerkt met nieuwe functies en functionaliteit. Niet alle toevoegingen zijn van toepassing op alle doelgroepen.
' In dit artikel is bedoeld om u te helpen u de versies die zijn uitgebracht en om te begrijpen of u wilt bijwerken naar de nieuwste versie of niet.

Dit is een lijst met verwante onderwerpen:



Onderwerp |  Details
--------- | --------- |
Stappen voor het upgraden van Azure AD Connect | Methoden om [upgrade van een eerdere versie naar de meest recente](active-directory-aadconnect-upgrade-previous-version.md) Azure AD Connect-release.
Vereiste machtigingen | Zie voor de vereiste machtigingen voor een update van toepassing, [accounts en machtigingen](./active-directory-aadconnect-accounts-permissions.md#upgrade).

Download | [Azure AD Connect downloaden](http://go.microsoft.com/fwlink/?LinkId=615771).

## <a name="116540"></a>1.1.654.0
Status: 12 December 2017

>[!NOTE]
>Dit is een beveiliging gerelateerde hotfix voor Azure AD Connect

### <a name="azure-ad-connect"></a>Azure AD Connect
Een verbetering is toegevoegd aan Azure AD Connect versie 1.1.654.0 (en na) om ervoor te zorgen dat de aanbevolen beschreven onder sectie wijzigingen [toegang tot het AD DS-account voor het vergrendelen](#lock) worden automatisch toegepast wanneer Azure AD Verbinding maken met het AD DS-account maakt. 

- Bij het instellen van Azure AD Connect, kunt de beheerder van de installatie een bestaande AD DS-account opgeven of Azure AD Connect automatisch maken van het account te laten. De wijzigingen in de machtigingen worden automatisch toegepast op het AD DS-account dat is gemaakt met Azure AD Connect tijdens de installatie. Ze worden niet toegepast op bestaande AD DS-account, opgegeven door de beheerder installeren.
- Voor klanten die een upgrade hebt uitgevoerd van een oudere versie van Azure AD Connect naar 1.1.654.0 (of na), de machtiging zal wijzigingen niet worden met terugwerkende kracht toegepast op bestaande AD DS-accounts gemaakt vóór de upgrade. Ze worden alleen worden toegepast op nieuwe AD DS-accounts die na de upgrade is gemaakt. Dit gebeurt wanneer u nieuwe AD-forests worden gesynchroniseerd naar Azure AD toevoegt.

>[!NOTE]
>Deze release verwijdert alleen de kwetsbaarheid voor nieuwe installaties van Azure AD Connect waarop de serviceaccount is gemaakt door het installatieproces. Voor bestaande installaties, of in gevallen waarin u het account zelf opgeven, moet u ervoor zorgen dat deze kwetsbaarheid niet bestaat.

#### <a name="lock"></a>Toegang tot het AD DS-account vergrendelen
Vergrendelen van toegang tot het AD DS-account door het implementeren van de volgende machtigingswijzigingen in de on-premises AD:  

*   Overname van het opgegeven object uitschakelen
*   Verwijder alle vermeldingen voor toegangsbeheer in het specifieke object, met uitzondering van ACE's specifieke naar zichzelf. We willen de standaardmachtigingen behouden wanneer deze naar zichzelf wordt.
*   Deze machtigingen toewijzen:

Type     | Naam                          | Access               | Van toepassing op
---------|-------------------------------|----------------------|--------------|
Toestaan    | SYSTEEM                        | Volledig beheer         | Dit object  |
Toestaan    | Ondernemingsadministrators             | Volledig beheer         | Dit object  |
Toestaan    | Domeinadministrators                 | Volledig beheer         | Dit object  |
Toestaan    | Beheerders                | Volledig beheer         | Dit object  |
Toestaan    | Ondernemings-domeincontrollers | Inhoud weergeven        | Dit object  |
Toestaan    | Ondernemings-domeincontrollers | Alle eigenschappen lezen  | Dit object  |
Toestaan    | Ondernemings-domeincontrollers | Machtigingen lezen     | Dit object  |
Toestaan    | Geverifieerde gebruikers           | Inhoud weergeven        | Dit object  |
Toestaan    | Geverifieerde gebruikers           | Alle eigenschappen lezen  | Dit object  |
Toestaan    | Geverifieerde gebruikers           | Machtigingen lezen     | Dit object  |

De strikter de instellingen voor de AD DS-account kunnen uitvoeren [dit PowerShell-script](https://gallery.technet.microsoft.com/Prepare-Active-Directory-ef20d978). Het PowerShell-script wordt de AD DS-account genoemd machtigingen toewijzen.

#### <a name="powershell-script-to-tighten-a-pre-existing-service-account"></a>PowerShell-script voor de hand van een bestaande service-account

Deze instellingen toepassen op een bestaande AD DS-account met het PowerShell-script (ether geleverd door uw organisatie of gemaakt met een eerdere installatie van Azure AD Connect download het script van de opgegeven koppeling hierboven.

##### <a name="usage"></a>Gebruik:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN <$ObjectDN> -Credential <$Credential>
```

waar 

**$ObjectDN** = Active Directory-account waarvan de machtigingen wilt hoger worden ingesteld.

**$Credential** = beheerdersreferenties die de benodigde machtigingen heeft voor de machtigingen voor $ObjectDN account beperken. Dit is meestal de beheerder van de Enterprise- of -domein. Gebruik de volledig gekwalificeerde domeinnaam van de administrator-account om te voorkomen dat account lookup-fouten. Voorbeeld: contoso.com\admin.

>[!NOTE] 
>$credential. Gebruikersnaam moet de indeling FQDN\gebruikersnaam. Voorbeeld: contoso.com\admin 

##### <a name="example"></a>Voorbeeld:

```powershell
Set-ADSyncRestrictedPermissions -ObjectDN "CN=TestAccount1,CN=Users,DC=bvtadwbackdc,DC=com" -Credential $credential 
```
### <a name="was-this-vulnerability-used-to-gain-unauthorized-access"></a>Deze kwetsbaarheid gebruikt om onbevoegde toegang te krijgen?

Om te zien als deze kwetsbaarheid is gebruikt om inbreuk van uw Azure AD te herstellen Connect-configuratie moet u controleren of het wachtwoord van de laatste datum van het serviceaccount.  Als de tijdstempel in onverwachte, verder onderzoek via het gebeurtenislogboek voor dat het wachtwoord opnieuw instellen van gebeurtenis, moet worden ondernomen.

Zie voor meer informatie [Microsoft Security Advisory 4056318](https://technet.microsoft.com/library/security/4056318)

## <a name="116490"></a>1.1.649.0
Status: Oktober 27 2017

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie Azure AD Connect automatische Upgrade.

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issue"></a>Opgelost probleem
* Een compatibiliteitsprobleem versie tussen Azure AD Connect en Azure AD Connect Health-Agent (voor synchronisatie) opgelost. Dit probleem is van invloed op klanten die Azure AD Connect in-place upgrade naar versie 1.1.647.0 uitvoert, maar heeft momenteel Health Agent versie 3.0.127.0. Na de upgrade kan de Health-Agent niet meer statusgegevens over Azure AD Connect-synchronisatieservice verzenden naar Azure AD Health-Service. Met deze oplossing wordt Health Agent versie 3.0.129.0 geïnstalleerd tijdens een upgrade ter plekke van Azure AD Connect. Health-Agent versie 3.0.129.0 heeft geen compatibiliteitsprobleem met Azure AD Connect versie 1.1.649.0.


## <a name="116470"></a>1.1.647.0
Status: Oktober 19 2017

> [!IMPORTANT]
> Er is een bekend compatibiliteitsprobleem tussen Azure AD Connect 1.1.647.0 en Azure AD Connect Health-Agent (voor synchronisatie) versie 3.0.127.0. Dit probleem wordt voorkomen dat de Health-Agent health gegevens over de Azure AD Connect-synchronisatieservice (inclusief synchronisatiefouten en uitvoeringsgeschiedenis gegevens) te verzenden naar Azure AD Health-Service. Voordat u uw Azure AD Connect-implementatie handmatig upgraden naar versie 1.1.647.0, Controleer of dat de huidige versie van Azure AD Connect Health-Agent is geïnstalleerd op uw Azure AD Connect-server. U kunt dit doen door te gaan naar *Configuratiescherm → Software* en zoek naar de toepassing *Microsoft Azure AD Connect Health-Agent voor synchronisatie*. Als de versie ervan 3.0.127.0, verdient het aanbeveling wachten op de volgende Azure AD Connect-versie beschikbaar is voor de upgrade. Als de versie van de Health-Agent niet 3.0.127.0, is het goed om door te gaan met de handmatige, in-place upgrade. Houd er rekening mee dat dit probleem heeft geen invloed op swing-upgrade of klanten die een nieuwe installatie van Azure AD Connect uitvoert.
>
>

### <a name="azure-ad-connect"></a>Azure AD Connect
#### <a name="fixed-issues"></a>Opgeloste problemen
* Vaste een probleem met de *wijzigen gebruikersaanmelding* taak in Azure AD Connect-wizard:

  * Het probleem treedt op wanneer er een bestaande implementatie van Azure AD Connect met Wachtwoordsynchronisatie **ingeschakeld**, en u probeert in te stellen van de gebruiker aanmelden methode als *Pass through-verificatie*. Voordat de wijziging wordt toegepast, wordt de wizard onjuist geeft de '*Wachtwoordsynchronisatie uitschakelen*' prompt. Wachtwoordsynchronisatie blijft ingeschakeld nadat de wijziging wordt toegepast. Met deze oplossing wordt de wizard niet meer de prompt weergegeven.

  * Door het ontwerp van de wizard komt niet uit te schakelen Wachtwoordsynchronisatie wanneer u, wordt gebruikt door de gebruiker aanmeldingsmethode bijwerkt de *wijzigen gebruikersaanmelding* taak. Hiermee wordt voorkomen dat klanten die houden Wachtwoordsynchronisatie, willen hoewel ze bij het inschakelen van Pass through-verificatie of federation als hun primaire gebruiker aanmelden methode wordt onderbroken.
  
  * Als u uitschakelen Wachtwoordsynchronisatie wilt na het bijwerken van de gebruiker aanmelden methode, moet u uitvoeren de *synchronisatieconfiguratie aanpassen* taak in de wizard. Wanneer u navigeren naar de *optionele functies* pagina, schakelt u de *Wachtwoordsynchronisatie* optie.
  
  * Houd er rekening mee dat hetzelfde probleem vindt ook plaats als u wilt in-of uitschakelen naadloze eenmalige aanmelding. In het bijzonder u een bestaande Azure AD Connect-implementatie hebt met Wachtwoordsynchronisatie is ingeschakeld en de gebruiker aanmelden methode is al geconfigureerd als *Pass through-verificatie*. Met behulp van de *wijzigen gebruikersaanmelding* taak, die u probeert te selectievakje/schakelt de *inschakelen naadloze eenmalige aanmelding* optie terwijl de gebruiker aanmelden methode geconfigureerd als 'Pass through-verificatie' blijft. Voordat de wijziging wordt toegepast, wordt de wizard onjuist geeft de '*Wachtwoordsynchronisatie uitschakelen*' prompt. Wachtwoordsynchronisatie blijft ingeschakeld nadat de wijziging wordt toegepast. Met deze oplossing wordt de wizard niet meer de prompt weergegeven.

* Vaste een probleem met de *wijzigen gebruikersaanmelding* taak in Azure AD Connect-wizard:

   * Het probleem treedt op wanneer er een bestaande implementatie van Azure AD Connect met Wachtwoordsynchronisatie **uitgeschakeld**, en u probeert in te stellen van de gebruiker aanmelden methode als *Pass through-verificatie*. Wanneer de wijziging wordt toegepast, wordt de wizard kunt Pass through-verificatie- en Wachtwoordsynchronisatie. Met deze oplossing in de wizard Wachtwoordsynchronisatie niet meer ingeschakeld.

  * Wachtwoordsynchronisatie is eerder een vereiste voor het inschakelen van Pass through-verificatie. Als u instelt dat de gebruiker aanmelden methode als *Pass through-verificatie*, de wizard Pass through-verificatie- en Wachtwoordsynchronisatie wilt inschakelen. Wachtwoordsynchronisatie is onlangs verwijderd als een vereiste. Als onderdeel van Azure AD Connect versie 1.1.557.0 een wijziging is aangebracht naar Azure AD Connect niet wachtwoordsynchronisatie inschakelen bij het instellen van de gebruiker aanmelden methode als *Pass through-verificatie*. De wijziging is echter alleen toegepast op Azure AD Connect-installatie. Met deze oplossing ook dezelfde wijziging wordt toegepast op de *wijzigen gebruikersaanmelding* taak.
  
  * Houd er rekening mee dat hetzelfde probleem vindt ook plaats als u wilt in-of uitschakelen naadloze eenmalige aanmelding. In het bijzonder u een bestaande Azure AD Connect-implementatie hebt met Wachtwoordsynchronisatie uitgeschakeld en de gebruiker aanmelden methode is al geconfigureerd als *Pass through-verificatie*. Met behulp van de *wijzigen gebruikersaanmelding* taak, die u probeert te selectievakje/schakelt de *inschakelen naadloze eenmalige aanmelding* optie terwijl de gebruiker aanmelden methode geconfigureerd als 'Pass through-verificatie' blijft. Als de wijziging wordt toegepast, kan de wizard Wachtwoordsynchronisatie. Met deze oplossing in de wizard Wachtwoordsynchronisatie niet meer ingeschakeld. 

* Een probleem waardoor Azure AD Connect-upgrade mislukken met fout opgelost '*kan geen upgrade uitvoeren van de synchronisatieservice*'. Verder kan de synchronisatieservice niet meer kunt starten met de gebeurtenis met de fout '*de service kan niet worden gestart omdat de versie van de database nieuwer dan de versie van de binaire bestanden geïnstalleerd is is*'. Dit probleem treedt op wanneer de beheerder uitvoeren van de upgrade geen sysadmin-bevoegdheden op de SQL server die wordt gebruikt door Azure AD Connect. Met deze oplossing vereist Azure AD Connect alleen de beheerder db_owner bevoegdheid met de database van ADSync hebben tijdens de upgrade.

* Een Azure AD Connect Upgrade probleem dat klanten die ingeschakeld zijn van invloed op een vaste [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-sso). Na de upgrade van Azure AD Connect naadloze eenmalige aanmelding onjuist wordt weergegeven als uitgeschakeld in Azure AD Connect-wizard, zelfs als de functie ingeschakelde en volledig functioneel blijft. Met deze oplossing de functie wordt nu weergegeven correct ingeschakeld in de wizard.

* Een probleem waardoor Azure AD Connect-wizard altijd weergeven vaste de '*Bronanker configureren*' vragen op de *gereed om te configureren* pagina, zelfs als er geen wijzigingen die betrekking hebben op Bronanker doorgevoerd.

* Bij het uitvoeren van handmatige in-place upgrade van Azure AD Connect is de klant de referenties van de globale beheerder van de bijbehorende Azure AD-tenant vereist. Voorheen upgrade kan doorgaan, ondanks dat de opgegeven referenties voor de globale beheerder behoort tot een andere Azure AD-tenant. Tijdens de upgrade wordt voltooid weergegeven, worden bepaalde configuraties niet correct met de upgrade behouden. Met deze wijziging wordt de wizard upgrade voortgezet als de opgegeven referenties komen niet overeen met de Azure AD-tenant niet toegestaan.

* Redundante logica die onnodig opnieuw Azure AD Connect Health-service aan het begin van een handmatige upgrade gestart verwijderd.


#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Toegevoegde logica voor het vereenvoudigen van de stappen die nodig zijn voor het instellen van Azure AD Connect met Microsoft Duitsland Cloud. U bent voorheen vereist voor het bijwerken van specifieke registersleutels op de Azure AD Connect-server voor deze correct werkt met Microsoft Duitsland Cloud, zoals beschreven in dit artikel. Azure AD Connect kan nu automatisch detecteren als uw tenant wordt in Microsoft Duitsland Cloud op basis van de globale beheerdersreferenties die zijn opgegeven tijdens de installatie.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
>[!NOTE]
> Opmerking: De Synchronization Service heeft een WMI-interface waarmee u uw eigen aangepaste scheduler ontwikkelen. Deze interface is gedeprecieerd en wordt verwijderd uit toekomstige versies van Azure AD Connect na 30 juni 2018 is verzonden. Klanten die willen aanpassen Synchronisatieplanning moeten gebruiken de [ingebouwde scheduler (https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-feature-scheduler).

#### <a name="fixed-issues"></a>Opgeloste problemen
* Als u Azure AD Connect-wizard maakt de AD-Connector-account vereist om wijzigingen van de lokale Active Directory te synchroniseren, is het niet correct toekennen het account de vereiste machtiging om te lezen PublicFolder objecten. Dit probleem is van invloed op zowel de Express-installatie en de aangepaste installatie. Deze wijziging kunt u het probleem.

* Een probleem dat de pagina Azure AD Connect-Wizard voor probleemoplossing weergeven niet correct heeft voor beheerders uitvoert vanuit Windows Server 2016 vast.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Bij het oplossen van problemen met de Azure AD Connect-wizard pagina voor probleemoplossing met Wachtwoordsynchronisatie pagina voor probleemoplossing nu opgevraagd domeinspecifieke.

* Voorheen als u Hash wachtwoordsynchronisatie inschakelen probeerde, Azure AD Connect controleert niet of het AD-Connector-account vereist machtigingen voor het synchroniseren van de wachtwoord-hashes van on-premises AD dat. Azure AD Connect-wizard wordt nu, Controleer en waarschuwen als de AD-Connector-account niet beschikt over voldoende machtigingen.

### <a name="ad-fs-management"></a>AD FS-beheer
#### <a name="fixed-issue"></a>Opgelost probleem
* Een probleem is gerelateerd aan het gebruik van opgelost [msDS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) functie. Dit probleem geldt voor klanten die u hebt geconfigureerd *Federatie met AD FS* als de gebruiker aanmelden methode. Wanneer u de uitvoert *Bronanker configureren* verandert in met behulp van een taak in de wizard, Azure AD Connect * ms-DS-ConsistencyGuid als bronkenmerk voor onveranderbare id genoemd. Als onderdeel van deze wijziging probeert Azure AD Connect bij te werken van de claimregels voor onveranderbare id genoemd in AD FS. Deze stap is echter mislukt omdat Azure AD Connect heeft niet de beheerdersreferenties vereist AD FS kunt configureren. Met deze oplossing Azure AD Connect nu vraagt u de administrator-referenties invoeren voor AD FS, wanneer u uitvoert het *Bronanker configureren* taak.



## <a name="116140"></a>1.1.614.0
Status: September 05 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="known-issues"></a>Bekende problemen
* Er is een bekend probleem dat wordt veroorzaakt door Azure AD Connect-upgrade mislukken met fout "*kan geen upgrade uitvoeren van de synchronisatieservice*'. Verder kan de synchronisatieservice niet meer kunt starten met de gebeurtenis met de fout '*de service kan niet worden gestart omdat de versie van de database nieuwer dan de versie van de binaire bestanden geïnstalleerd is is*'. Dit probleem treedt op wanneer de beheerder uitvoeren van de upgrade geen sysadmin-bevoegdheden op de SQL server die wordt gebruikt door Azure AD Connect. Dbo-machtigingen zijn niet voldoende.

* Er is een bekend probleem met het Azure AD Connect Upgrade die van invloed op klanten die hebt ingeschakeld [naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md). Na de upgrade van Azure AD Connect is ziet de functie er als uitgeschakeld in de wizard, zelfs als de functie ingeschakelde blijft. Een oplossing voor dit probleem wordt in de toekomst worden opgegeven vrijgeven. Klanten die zich zorgen maakt over dit probleem weergeven kunnen handmatig corrigeren door in te schakelen naadloze eenmalige aanmelding in de wizard.

#### <a name="fixed-issues"></a>Opgeloste problemen
* Vaste een probleem dat Azure AD Connect niet bijwerken van de claimregels in AD FS lokale tijdens het inschakelen van de [msDS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) functie. Dit probleem treedt op als u de functie voor een bestaande implementatie van Azure AD Connect met AD FS geconfigureerd als de methode voor aanmelden in te schakelen. Dit probleem treedt op omdat de wizard wordt niet gevraagd om referenties ADFS voordat het bijwerken van de claimregels in AD FS.
* Vaste een probleem waardoor Azure AD Connect installatie mislukt als de on-premises AD-forest is NTLM is uitgeschakeld. Het probleem wordt veroorzaakt door van Azure AD Connect-wizard volledig gekwalificeerde referenties niet bieden bij het maken van de beveiligingscontext die voor Kerberos-verificatie vereist. Dit zorgt ervoor dat de Azure AD Connect-wizard terugvallen op met behulp van NTLM en Kerberos-verificatie.

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync
#### <a name="fixed-issues"></a>Opgeloste problemen
* Een probleem opgelost waarbij nieuwe synchronisatieregel voor kan niet worden gemaakt als het kenmerk label niet is ingevuld.
* Vaste een probleem waardoor Azure AD Connect verbinding maken met on-premises AD voor Wachtwoordsynchronisatie met NTLM, ondanks dat Kerberos beschikbaar is. Dit probleem treedt op als de on-premises AD-topologie heeft een of meer domeincontrollers die is hersteld vanaf een back-up.
* Een volledige synchronisatie stappen onnodig optreden na de upgrade, waardoor probleem opgelost. In het algemeen is stappen van de volledige synchronisatie uitgevoerd vereist na de upgrade als er wijzigingen in out-of-box-synchronisatieregels zijn. Het probleem is vanwege een fout in de wijziging detectielogica waardoor foutief een wijziging wordt gedetecteerd wanneer zich voordoen tijdens synchronisatie Regelexpressie met tekens voor nieuwe regels. Tekens voor nieuwe regels worden ingevoegd in synchronisatie regelvoorwaarde-expressie voor het verbeteren van de leesbaarheid.
* Een probleem dat kan leiden tot de Azure AD Connect-server niet goed werkt nadat de automatische Upgrade vast. Dit probleem betrekking heeft op Azure AD Connect-servers met versie 1.1.443.0 (of lager). Raadpleeg voor meer informatie over het probleem artikel [Azure AD Connect niet juist werkt na een automatische upgrade](https://support.microsoft.com/help/4038479/azure-ad-connect-is-not-working-correctly-after-an-automatic-upgrade).
* Een waardoor automatische upgrade uitvoeren naar een nieuwe poging gedaan om de 5 minuten wanneer er fouten zijn opgetreden probleem opgelost. Met de oplossing automatische Upgrade pogingen met exponentiële back uit wanneer er fouten optreden.
* Een probleem opgelost waar wachtwoord synchronisatiegebeurtenis 611 ten onrechte wordt weergegeven in de Windows-toepassing gebeurtenislogboeken als **informatief** in plaats van **fout**. Gebeurtenis 611 wordt gegenereerd wanneer er een probleem optreedt bij de synchronisatie van wachtwoorden. 
* Een probleem opgelost in de Azure AD Connect-wizard waarmee onderdeel van de groep terugschrijven zijn ingeschakeld zonder te selecteren van een organisatie-eenheid voor write-back van groep vereist.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Een taak oplossen toegevoegd aan Azure AD Connect-wizard onder aanvullende taken. Klanten kunnen gebruikmaken van deze taak voor het oplossen van problemen met Wachtwoordsynchronisatie algemene diagnostische gegevens worden verzameld. In de toekomst zal de problemen met taak worden uitgebreid met andere directory-synchronisatie-gerelateerde problemen.
* Azure AD Connect nu ondersteunt de modus van een nieuwe installatie aangeroepen **bestaande Database gebruiken**. Deze installatiemodus kan klanten Azure AD Connect waarmee een bestaande database van ADSync installeert. Raadpleeg voor meer informatie over deze functie, artikel [gebruik een bestaande database](active-directory-aadconnect-existing-database.md).
* Voor betere beveiliging standaard Azure AD Connect nu gebruik van TLS1.2 verbinding maken met Azure AD voor directory-synchronisatie. De standaardwaarde is eerder TLS1.0.
* Wanneer u Azure AD Connect Agent voor Wachtwoordsynchronisatie start, wordt geprobeerd verbinding maken met Azure AD bekende eindpunt voor Wachtwoordsynchronisatie. Nadat de verbinding is geslaagd, wordt die omgeleid naar een specifieke eindpunt. De Agent voor Wachtwoordsynchronisatie eerder, plaatst het eindpunt regiospecifieke totdat deze opnieuw is opgestart. Nu de agent wordt gewist de cache en opnieuw wordt geprobeerd met het bekende eindpunt als verbindingsprobleem met het eindpunt regiospecifieke worden aangetroffen. Deze wijziging zorgt ervoor dat Wachtwoordsynchronisatie failover naar een andere regiospecifieke-eindpunt kunt wanneer het in de cache regiospecifieke-eindpunt is niet meer beschikbaar.
* Als u wilt synchroniseren van wijzigingen uit een on-premises is AD-forest, een AD DS-account vereist. U kunt (i) maken het AD DS account zelf en de referenties opgeeft naar Azure AD Connect of (ii) bieden een ondernemingsadministrator referenties en kunt Azure AD Connect het AD DS-account maken voor u. Voorheen is (i) de standaardoptie in de Azure AD Connect-wizard. Nu (ii) is de standaardoptie.

### <a name="azure-ad-connect-health"></a>Azure AD Connect Health (Engelstalig)

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Ondersteuning toegevoegd voor Microsoft Azure Government Cloud en Microsoft Cloud Duitsland.

### <a name="ad-fs-management"></a>AD FS-beheer
#### <a name="fixed-issues"></a>Opgeloste problemen
* De cmdlet initialiseren ADSyncNGCKeysWriteBack in de AD-prep powershell-module is onjuist ACL'ing de device registration-container en zou daarom alleen bestaande machtigingen overnemen.  Dit is bijgewerkt, zodat de synchronisatie-serviceaccount de juiste machtigingen heeft.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* De taak AAD Connect controleren of AD FS-aanmelding is bijgewerkt zodat er aanmeldingen op basis van Microsoft Online en niet alleen token ophalen met AD FS wordt gecontroleerd.
* Wanneer u een nieuwe AD FS-farm met behulp van AAD Connect instelt, wordt de pagina vragen om referenties van de AD FS is verplaatst zodat deze nu plaatsvindt voordat de gebruiker wordt gevraagd op te geven van de AD FS- en WAP-servers.  Hierdoor kan AAD Connect om te controleren of het opgegeven account de juiste machtigingen heeft.
* Tijdens de upgrade AAD Connect mislukt we niet langer een upgrade als de AD FS AAD-vertrouwensrelatie niet bijwerken.  Als dat gebeurt, wordt de gebruiker een waarschuwing wordt weergegeven en moet doorgaan naar de vertrouwensrelatie via de aanvullende taak de AAD Connect opnieuw instellen.

### <a name="seamless-single-sign-on"></a>Naadloze eenmalige aanmelding
#### <a name="fixed-issues"></a>Opgeloste problemen
* Een Azure AD Connect-wizard een fout geretourneerd als u wilt inschakelen, waardoor probleem opgelost [naadloze eenmalige aanmelding](active-directory-aadconnect-sso.md). Het foutbericht is *'-configuratie van Microsoft Azure AD Connect verificatie-Agent is mislukt'.* Dit probleem is van invloed op bestaande klanten die handmatig upgrade hebben uitgevoerd van de preview-versie van de verificatie-Agents voor [Pass through-verificatie](active-directory-aadconnect-sso.md) op basis van de stappen in dit [artikel](active-directory-aadconnect-pass-through-authentication-upgrade-preview-authentication-agents.md).


## <a name="115610"></a>1.1.561.0
Status: Juli 23 2017

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Opgelost probleem

* Een probleem waardoor de synchronisatieregel voor out of box 'Out naar AD - gebruiker onveranderbare id genoemd' fixed worden verwijderd:

  * Het probleem optreedt wanneer de upgrade van Azure AD Connect wordt uitgevoerd, of wanneer de taakoptie *synchronisatieconfiguratie bijwerken* in de Azure AD Connect-wizard Configuratie van Azure AD Connect-synchronisatie bijwerken wordt gebruikt.
  
  * Deze synchronisatieregel is van toepassing op klanten die zijn ingeschakeld de [msDS-ConsistencyGuid als Bronanker functie](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor). Deze functie is geïntroduceerd in versie 1.1.524.0 en na. Wanneer de synchronisatieregel wordt verwijderd, Azure AD Connect niet meer kunt vullen lokale AD DS-ms-ConsistencyGuid kenmerk met de waarde van het kenmerk ObjectGuid. Dit voorkomt dat nieuwe gebruikers niet worden ingericht in Azure AD.
  
  * De oplossing zorgt ervoor dat de regel voor het synchroniseren niet meer worden verwijderd tijdens de upgrade, of configuratiewijziging, zolang de functie is ingeschakeld. Voor bestaande klanten die zijn beïnvloed door dit probleem, zorgt de oplossing er ook voor dat de synchronisatieregel terug na de upgrade naar deze versie van Azure AD Connect is toegevoegd.

* Een probleem dat ervoor zorgt out-of-box-synchronisatieregels dat hebben voorrang-waarde die lager is dan 100 opgelost:

  * In het algemeen zijn voorrang waarden 0 - 99 gereserveerd voor aangepaste synchronisatieregels. Tijdens de upgrade, worden de waarden van de prioriteit voor out-of-box-synchronisatieregels bijgewerkt zodat de wijzigingen in synchronisatie-regel. Vanwege dit probleem, worden out-of-box-synchronisatieregels toegewezen prioriteit die lager is dan 100.
  
  * De oplossing wordt voorkomen dat het probleem optreedt tijdens de upgrade. Echter, worden niet hersteld de waarden van de prioriteit voor bestaande klanten die zijn beïnvloed door het probleem. Een afzonderlijke oplossing wordt in de toekomst worden opgegeven om te helpen bij het herstel.

* Een probleem opgelost waarbij de [domein en OE filteren scherm](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) in de Azure AD Connect wizard wordt weergegeven *synchroniseren van alle domeinen en OE* optie geselecteerd is, zelfs als filteren op basis van organisatie-eenheid is ingeschakeld.

*   Een probleem opgelost, waardoor de [mappartities configureren scherm](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) in Synchronization Service Manager naar een fout geretourneerd als de *vernieuwen* knop wordt geklikt. Het foutbericht is *' is een fout opgetreden tijdens het vernieuwen van domeinen: kan object van het type 'System.Collections.ArrayList' naar het type ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* De fout treedt op wanneer een nieuw AD-domein is toegevoegd aan een bestaand AD-forest en u probeert bij te werken van Azure AD Connect met de knop vernieuwen.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

* [De functie voor automatisch bijwerken](active-directory-aadconnect-feature-automatic-upgrade.md) uitgebreid ter ondersteuning van klanten met de volgende configuraties:
  * U kunt de functie van de Write-back van apparaat hebt ingeschakeld.
  * U kunt het onderdeel groep terugschrijven hebt ingeschakeld.
  * De installatie is niet een snelle instellingen of een upgrade van DirSync.
  * U hebt meer dan 100.000 objecten in de metaverse.
  * U verbinding maakt met meer dan één forest. Snelle installatie is alleen verbinding maakt met één forest.
  * De AD-Connector-account is niet het standaardaccount voor MSOL_ meer.
  * De server is ingesteld op in de faseringsmodus.
  * U kunt de functie terugschrijven hebt ingeschakeld.
  
  >[!NOTE]
  >De uitbreiding van het bereik van de functie Automatische Upgrade heeft betrekking op klanten met Azure AD Connect build 1.1.105.0 en na. Als u niet dat uw Azure AD Connect-server automatisch worden bijgewerkt wilt, moet u uitvoeren volgende cmdlet op uw Azure AD Connect-server: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Raadpleeg voor meer informatie over het inschakelen/uitschakelen van automatische Upgrade artikel [Azure AD Connect: Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115580"></a>1.1.558.0
Status: Wordt niet vrijgegeven. Wijzigingen in deze versie worden opgenomen in versie 1.1.561.0.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Opgelost probleem

* Een probleem waardoor de synchronisatieregel voor out of box 'Out naar AD - gebruiker onveranderbare id genoemd' fixed moeten worden verwijderd wanneer de configuratie van filteren op basis van organisatie-eenheid is bijgewerkt. Deze synchronisatieregel is vereist voor de [msDS-ConsistencyGuid als Bronanker functie](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).

* Een probleem opgelost waarbij de [domein en OE filteren scherm](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) in de Azure AD Connect wizard wordt weergegeven *synchroniseren van alle domeinen en OE* optie geselecteerd is, zelfs als filteren op basis van organisatie-eenheid is ingeschakeld.

*   Een probleem opgelost, waardoor de [mappartities configureren scherm](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) in Synchronization Service Manager naar een fout geretourneerd als de *vernieuwen* knop wordt geklikt. Het foutbericht is *' is een fout opgetreden tijdens het vernieuwen van domeinen: kan object van het type 'System.Collections.ArrayList' naar het type ' Microsoft.DirectoryServices.MetadirectoryServices.UI.PropertySheetBase.MaPropertyPages.PartitionObject."* De fout treedt op wanneer een nieuw AD-domein is toegevoegd aan een bestaand AD-forest en u probeert bij te werken van Azure AD Connect met de knop vernieuwen.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

* [De functie voor automatisch bijwerken](active-directory-aadconnect-feature-automatic-upgrade.md) uitgebreid ter ondersteuning van klanten met de volgende configuraties:
  * U kunt de functie van de Write-back van apparaat hebt ingeschakeld.
  * U kunt het onderdeel groep terugschrijven hebt ingeschakeld.
  * De installatie is niet een snelle instellingen of een upgrade van DirSync.
  * U hebt meer dan 100.000 objecten in de metaverse.
  * U verbinding maakt met meer dan één forest. Snelle installatie is alleen verbinding maakt met één forest.
  * De AD-Connector-account is niet het standaardaccount voor MSOL_ meer.
  * De server is ingesteld op in de faseringsmodus.
  * U kunt de functie terugschrijven hebt ingeschakeld.
  
  >[!NOTE]
  >De uitbreiding van het bereik van de functie Automatische Upgrade heeft betrekking op klanten met Azure AD Connect build 1.1.105.0 en na. Als u niet dat uw Azure AD Connect-server automatisch worden bijgewerkt wilt, moet u uitvoeren volgende cmdlet op uw Azure AD Connect-server: `Set-ADSyncAutoUpgrade -AutoUpgradeState disabled`. Raadpleeg voor meer informatie over het inschakelen/uitschakelen van automatische Upgrade artikel [Azure AD Connect: Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md).

## <a name="115570"></a>1.1.557.0
Status: Juli 2017

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie Azure AD Connect automatische Upgrade.

### <a name="azure-ad-connect"></a>Azure AD Connect

#### <a name="fixed-issue"></a>Opgelost probleem
* Een probleem opgelost met de cmdlet Initialize-ADSyncDomainJoinedComputerSync waardoor het geverifieerde domein op het bestaande service connection point-object worden gewijzigd, zelfs als deze nog steeds een geldig domein geconfigureerd. Dit probleem treedt op wanneer uw Azure AD-tenant meer dan één geverifieerde domeinen die kunnen worden gebruikt heeft voor het configureren van het serviceverbindingspunt wordt gehost.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Wachtwoord terugschrijven is nu beschikbaar voor het voorbeeld met Microsoft Azure Government cloud en Microsoft Cloud Duitsland. Raadpleeg voor meer informatie over Azure AD Connect-ondersteuning voor de andere service-exemplaren artikel [Azure AD Connect: speciale overwegingen voor exemplaren](active-directory-aadconnect-instances.md).

* De cmdlet Initialize-ADSyncDomainJoinedComputerSync heeft nu een nieuwe optionele parameter met de naam AzureADDomain. Deze parameter kunt u opgeven die worden gebruikt voor het configureren van het service connection point-domein gecontroleerd.

### <a name="pass-through-authentication"></a>Pass-through-verificatie

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* De naam van de agent vereist is voor Pass-through-verificatie is gewijzigd van *Microsoft Azure AD Connector voor toepassingsproxy* naar *Microsoft Azure AD Connect-verificatie Agent*.

* Inschakelen van Pass-through-verificatie niet langer synchronisatie van wachtwoordhash standaard ingeschakeld.


## <a name="115530"></a>1.1.553.0
Status: Juni 2017

> [!IMPORTANT]
> Er zijn schema en sync regel wijzigingen die zijn geïntroduceerd in deze versie. Azure AD Connect-synchronisatieservice activeren volledige Import en een volledige synchronisatie stappen na de upgrade. Details van de wijzigingen worden hieronder beschreven. Om uit te stellen tijdelijk volledige Import en een volledige synchronisatie stappen na de upgrade, raadpleegt u artikel [het uitstellen van de volledige synchronisatie na de upgrade](active-directory-aadconnect-upgrade-previous-version.md#how-to-defer-full-synchronization-after-upgrade).
>
>

### <a name="azure-ad-connect-sync"></a>Azure AD Connect Sync

#### <a name="known-issue"></a>Bekende problemen
* Er is een probleem dat betrekking heeft op klanten die [filteren op basis van organisatie-eenheid](active-directory-aadconnectsync-configure-filtering.md#organizational-unitbased-filtering) met Azure AD Connect-synchronisatie. Wanneer u navigeren naar de [domein en OE filteren pagina](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) in de Azure AD Connect-wizard het volgende gedrag wordt verwacht:
  * Als filteren op basis van organisatie-eenheid is ingeschakeld, de **geselecteerde domeinen en OE's synchroniseren** optie is geselecteerd.
  * Anders wordt de **synchroniseren van alle domeinen en OE** optie is geselecteerd.

Het probleem dat zich voordoet, is dat de **synchroniseren van alle domeinen en organisatie-eenheden optie** is altijd ingeschakeld wanneer u de Wizard uitvoert.  Dit gebeurt zelfs als filteren op basis van organisatie-eenheid is geconfigureerd. Voordat u opslaat configuratiewijzigingen AAD Connect, zorg ervoor dat de **synchroniseren geselecteerde domeinen en organisatie-eenheden-optie is geselecteerd** en Bevestig dat alle OE's die nodig zijn om te synchroniseren die opnieuw zijn ingeschakeld. Anders wordt filteren op basis van organisatie-eenheid uitgeschakeld.

#### <a name="fixed-issues"></a>Opgeloste problemen

* Een probleem opgelost met terugschrijven van wachtwoord waarmee beheerders de Azure AD in te stellen van het wachtwoord van een on-premises AD-gebruikersaccount beschermde. Dit probleem treedt op wanneer het Azure AD Connect is de machtiging wachtwoord opnieuw instellen via het bevoorrechte account. Wordt besproken in deze versie van Azure AD Connect niet toestaat een Azure AD-beheerder om in te stellen van het wachtwoord van een willekeurige on-premises AD-gebruikersaccount beschermde tenzij de beheerder de eigenaar van dat account is. Raadpleeg voor meer informatie [Security Advisory 4033453](https://technet.microsoft.com/library/security/4033453).

* Vaste een probleem is gerelateerd aan de [msDS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) functie waar Azure AD Connect niet terugschrijven naar biedt lokale AD msDS-ConsistencyGuid kenmerk. Dit probleem treedt op wanneer er zich meerdere on-premises AD-forests die zijn toegevoegd aan Azure AD Connect en de *gebruikersidentiteiten bestaan tussen meerdere mappen optie* is geselecteerd. Wanneer u deze configuratie gebruikt, items de resulterende synchronisatieregels geen toevoegen aan het kenmerk sourceAnchorBinary in de Metaverse. Het kenmerk sourceAnchorBinary wordt gebruikt als het bronkenmerk voor het kenmerk msDS-ConsistencyGuid. Als gevolg hiervan wordt terugschrijven met het kenmerk ms DSConsistencyGuid niet uitgevoerd. U kunt het probleem oplossen door zijn volgende synchronisatie regels bijgewerkt om ervoor te zorgen dat het kenmerk sourceAnchorBinary in de Metaverse altijd is gevuld:
  * In uit Active Directory - InetOrgPerson AccountEnabled.xml
  * In uit Active Directory - InetOrgPerson Common.xml
  * In uit Active Directory - gebruiker AccountEnabled.xml
  * In uit Active Directory - gebruiker Common.xml
  * In uit Active Directory - gebruiker aanmelden SOAInAAD.xml

* Eerder, zelfs als de [msDS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) functie niet is ingeschakeld, de synchronisatieregel 'Out naar AD-gebruiker onveranderbare id genoemd' wordt nog steeds toegevoegd aan Azure AD Connect. Het effect is onschadelijk en veroorzaakt geen Write-back van kenmerk msDS-ConsistencyGuid om te worden uitgevoerd. Om verwarring te voorkomen, is logica toegevoegd om ervoor te zorgen dat de synchronisatieregel alleen worden toegevoegd als de functie is ingeschakeld.

* Een probleem waardoor de synchronisatie van wachtwoordhash mislukken met foutgebeurtenis 611 opgelost. Dit probleem treedt op nadat een of meer domain controllers zijn verwijderd uit on-premises AD. Aan het einde van elke synchronisatiecyclus wachtwoord, de cookie voor adreslijstsynchronisatie van uitgegeven door on-premises AD aanroep-id's bevat van de verwijderde domeincontrollers met USN (Update Sequence Number)-waarde van 0. Wachtwoordsynchronisatiebeheer kan niet in stand houden synchronisatie cookie met USN-waarde van 0 en mislukt met foutgebeurtenis 611. Tijdens de volgende synchronisatiecyclus hergebruikt Wachtwoordsynchronisatiebeheer de laatste permanente synchronisatie cookie die geen USN-waarde van 0. Dit zorgt ervoor dat de dezelfde wachtwoordwijzigingen opnieuw worden gesynchroniseerd. Met deze oplossing persistente Wachtwoordsynchronisatiebeheer de cookie voor adreslijstsynchronisatie correct.

* Eerder, zelfs als de automatische Upgrade is uitgeschakeld met de cmdlet Set-ADSyncAutoUpgrade, voor de automatische Upgrade wordt verdergegaan met het controleren op werk periodiek en is afhankelijk van het gedownloade installatieprogramma deactivering inwilligen. Met deze oplossing controleert de automatische Upgrade-proces langer voor upgrade regelmatig. De oplossing wordt automatisch toegepast wanneer de upgrade installatieprogramma voor deze versie van Azure AD Connect eenmaal wordt uitgevoerd.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen

* Voorheen de [msDS-ConsistencyGuid als Bronanker](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnect-design-concepts#using-msds-consistencyguid-as-sourceanchor) functie is beschikbaar voor nieuwe implementaties alleen. Het is nu beschikbaar is voor bestaande implementaties. Met name:
  * Voor toegang tot de functie moet de Azure AD Connect-wizard starten en kies de *Bronanker Update* optie.
  * Deze optie is alleen zichtbaar voor bestaande implementaties die van objectGuid als kenmerk sourceAnchor gebruikmaken.
  * Wanneer u de optie configureert, controleert de wizard of de status van het kenmerk msDS-ConsistencyGuid in uw lokale Active Directory. Als het kenmerk is niet geconfigureerd op elk gebruikersobject in Active directory, gebruikt de wizard de msDS-ConsistencyGuid als het kenmerk sourceAnchor. Als het kenmerk is geconfigureerd op een of meer objecten in Active directory, concludeert de wizard het kenmerk wordt gebruikt door andere toepassingen en is niet geschikt is als het kenmerk sourceAnchor en staat niet toe dat de wijziging Bronanker om door te gaan. Als u er zeker van bent dat het kenmerk wordt niet door bestaande toepassingen gebruikt, moet u contact op met ondersteuning voor meer informatie over het onderdrukken van de fout.

* Specifiek voor **userCertificate** -kenmerk op apparaatobjecten, Azure AD Connect nu gecontroleerd op waarden van de certificaten die zijn vereist voor [domein apparaten verbinding laten maken met Azure AD voor Windows 10-ervaring](https://docs.microsoft.com/azure/active-directory/active-directory-azureadjoin-devices-group-policy) en filters voor de overige vóór de synchronisatie met Azure AD. Om dit gedrag, de synchronisatieregel voor out of box 'Uit naar AAD--apparaat deelnemen aan SOAInAD' is bijgewerkt.

* Azure AD Connect nu ondersteunt Write-back van Exchange Online **cloudPublicDelegates** kenmerk on-premises AD dat **publicDelegates** kenmerk. Hierdoor is het scenario waarbij een Exchange Online-Postvak SendOnBehalfTo rechten aan gebruikers met lokale Exchange-postvak toekennen kunt. Ter ondersteuning van deze functie wordt een nieuwe synchronisatieregel out of box 'Out naar AD – Write-back van gebruiker Exchange hybride PublicDelegates' is toegevoegd. Deze synchronisatieregel wordt alleen toegevoegd aan Azure AD Connect als Exchange hybride-functie is ingeschakeld.

*   Azure AD Connect ondersteunt voor de nu synchroniseren van de **altRecipient** kenmerk vanuit Azure AD. Ter ondersteuning van deze wijziging zijn volgende out-of-box-sync-regels zodanig dat de vereiste kenmerkstroom bijgewerkt:
  * In uit Active Directory-gebruiker Exchange
  * Buiten het AAD-gebruiker ExchangeOnline
  
* De **cloudSOAExchMailbox** kenmerk in de Metaverse geeft aan of een bepaalde gebruiker Exchange Online-postvak of niet. De definitie is bijgewerkt met extra Exchange Online RecipientDisplayTypes als die apparatuur en vergaderzaal postvakken. Om deze wijziging, is de definitie van het kenmerk cloudSOAExchMailbox onder out of box synchronisatieregel 'In van AAD – gebruiker Exchange hybride', bijgewerkt van:

```
CBool(IIF(IsNullOrEmpty([cloudMSExchRecipientDisplayType]),NULL,BitAnd([cloudMSExchRecipientDisplayType],&amp;HFF) = 0))
```

... in het volgende:

```
CBool(
  IIF(IsPresent([cloudMSExchRecipientDisplayType]),(
    IIF([cloudMSExchRecipientDisplayType]=0,True,(
      IIF([cloudMSExchRecipientDisplayType]=2,True,(
        IIF([cloudMSExchRecipientDisplayType]=7,True,(
          IIF([cloudMSExchRecipientDisplayType]=8,True,(
            IIF([cloudMSExchRecipientDisplayType]=10,True,(
              IIF([cloudMSExchRecipientDisplayType]=16,True,(
                IIF([cloudMSExchRecipientDisplayType]=17,True,(
                  IIF([cloudMSExchRecipientDisplayType]=18,True,(
                    IIF([cloudMSExchRecipientDisplayType]=1073741824,True,(
                       IF([cloudMSExchRecipientDisplayType]=1073741840,True,False)))))))))))))))))))),False))

```

* De volgende reeks functies voor het maken van synchronisatie Regelexpressies voor het afhandelen van certificaat waarden in het kenmerk userCertificate X509Certificate2-compatibele toegevoegd:

    ||||
    | --- | --- | --- |
    |CertSubject|CertIssuer|CertKeyAlgorithm|
    |CertSubjectNameDN|CertIssuerOid|CertNameInfo|
    |CertSubjectNameOid|CertIssuerDN|IsCert|
    |CertFriendlyName|certThumbprint|CertExtensionOids|
    |CertFormat|CertNotAfter|CertPublicKeyOid|
    |CertSerialNumber|CertNotBefore|CertPublicKeyParametersOid|
    |CertVersion|CertSignatureAlgorithmOid|Selecteer|
    |CertKeyAlgorithmParams|CertHashString|waar|
    |||Met|

* Wijzigingen in het volgende schema zijn geïntroduceerd om te kunnen klanten om van aangepaste synchronisatieregels sAMAccountName, domainNetBios en domainFQDN voor een groepsobjecten worden weergegeven, evenals distinguishedName voor gebruikersobjecten stromen te maken:

  * Volgende kenmerken zijn toegevoegd aan MV-schema:
    * Groep: AccountName
    * Groep: domainNetBios
    * Groep: domainFQDN
    * Persoon: distinguishedName

  * Volgende kenmerken zijn toegevoegd aan Azure AD-Connector schema:
    * Groep: OnPremisesSamAccountName
    * Groep: NetBiosName
    * Groep: DNS-domeinnaam
    * Gebruiker: OnPremisesDistinguishedName

* Het script van de cmdlet ADSyncDomainJoinedComputerSync heeft nu een nieuwe optionele parameter met de naam AzureEnvironment. De parameter wordt gebruikt om op te geven welke regio die wordt gehost door de bijbehorende Azure Active Directory-tenant. Geldige waarden zijn:
  * AzureCloud (standaard)
  * AzureChinaCloud
  * AzureGermanyCloud
  * USGovernment
 
* Bijgewerkte Sync Rule Editor gebruiken deelnemen aan (in plaats van ingericht) als de standaardwaarde van het koppelingstype tijdens het maken van de regel voor synchronisatie.

### <a name="ad-fs-management"></a>AD FS-beheer

#### <a name="issues-fixed"></a>Problemen worden opgelost

* Volgende URL's zijn nieuwe WS-Federation-eindpunten die zijn geïntroduceerd door Azure AD tolerantie tegen storing van de verificatie verbeteren en worden toegevoegd met on-premises AD FS-configuratie voor het vertrouwen van beantwoorden partij:
  * https://Ests.Login.microsoftonline.com/Login.srf
  * https://stamp2.Login.microsoftonline.com/Login.srf
  * https://CCS.Login.microsoftonline.com/Login.srf
  * https://CCS-sdf.Login.microsoftonline.com/Login.srf
  
* Een probleem dat AD FS onjuist claimwaarde genereren voor IssuerID heeft opgelost. Het probleem doet zich voor als er meerdere geverifieerde domeinen in de Azure AD-tenant en het domeinachtervoegsel van het kenmerk userPrincipalName is gebruikt voor het genereren van de claim IssuerID ten minste is 3 niveaus diep (bijvoorbeeld johndoe@us.contoso.com). Het probleem is opgelost doordat de reguliere expressie die wordt gebruikt door de claimregels.

#### <a name="new-features-and-improvements"></a>Nieuwe functies en verbeteringen
* Voorheen werd door kan de functie AD FS-Certificaatbeheer van Azure AD Connect alleen worden gebruikt met AD FS-farms beheerd via Azure AD Connect. Nu kunt u de functie met AD FS-farms die niet worden beheerd met Azure AD Connect.

## <a name="115240"></a>1.1.524.0
Uitgebracht: Mei 2017

> [!IMPORTANT]
> Er zijn schema en sync regel wijzigingen die zijn geïntroduceerd in deze versie. Azure AD Connect-synchronisatieservice activeren volledige Import en een volledige synchronisatie stappen na de upgrade. Details van de wijzigingen worden hieronder beschreven.
>
>

**Opgeloste problemen:**

Azure AD Connect-synchronisatie

* Een probleem dat ervoor zorgt dat de automatische upgrade uitvoeren om te worden uitgevoerd op de Azure AD Connect-server, zelfs als de functie met de cmdlet Set-ADSyncAutoUpgrade is uitgeschakeld door de klant wordt opgelost. Met deze oplossing de automatische Upgrade-proces op de server nog steeds gecontroleerd op upgrade periodiek, maar het gedownloade installatieprogramma zich houdt aan de configuratie van de automatische Upgrade.
* Tijdens een upgrade ter plekke van DirSync maakt Azure AD Connect een Azure AD-serviceaccount moet worden gebruikt door de Azure AD-connector voor het synchroniseren met Azure AD. Nadat het account is gemaakt, verifieert de Azure AD Connect met Azure AD met het account. Soms verificatie is mislukt vanwege tijdelijke problemen waardoor op zijn beurt DirSync in-place upgrade mislukt met fout *' uitvoerende configureren AAD Sync-taak is een fout is opgetreden: AADSTS50034: als u wilt aanmelden bij deze toepassing, moet het account toegevoegd aan de map xxx.onmicrosoft.com.'* Ter verbetering van de tolerantie van upgrade van DirSync, opnieuw Azure AD Connect nu de verificatie stap.
* Er is een probleem met de build 443 waardoor DirSync in-place upgrade mislukt, maar uitvoeringsprofielen vereist is voor adreslijstsynchronisatie niet worden gemaakt. Herstel van logica is opgenomen in deze versie van Azure AD Connect. Wanneer de klant wordt bijgewerkt naar deze versie, wordt Azure AD Connect detecteert uitvoeringsprofielen ontbreekt en wordt deze gemaakt.
* Een probleem dat ervoor zorgt Wachtwoordsynchronisatie-proces dat starten met de gebeurtenis-ID 6900 en de fout vast *'een item met dezelfde sleutel is al toegevoegd'*. Dit probleem treedt op als u OE filteren configuratie zodanig dat de configuratiepartitie AD bijwerkt. U kunt dit probleem oplossen door Wachtwoordsynchronisatie proces nu wachtwoordwijzigingen uit domeinpartities alleen AD gesynchroniseerd. Niet-domeinpartities zoals configuratiepartitie worden overgeslagen.
* Tijdens de snelle installatie Azure AD Connect maakt een on-premises AD DS-account moet worden gebruikt door de AD-connector om te communiceren met on-premises AD dat. Voorheen het account is gemaakt met de PASSWD_NOTREQD-vlag ingesteld op het Gebruikersaccountbeheer kenmerk en een willekeurig wachtwoord is ingesteld op het account. Nu Azure AD Connect expliciet verwijdert u de vlag PASSWD_NOTREQD nadat het wachtwoord is ingesteld op het account.
* Een probleem dat ervoor zorgt dat de upgrade van DirSync mislukken met fout opgelost *' een impasse opgetreden in sql server die bij het verkrijgen van een toepassingsvergrendeling'* wanneer het kenmerk mailNickname is gevonden in de on-premises AD-schema, maar niet beperkt tot naar de objectklasse AD-gebruiker.
* Een probleem dat ervoor zorgt apparaat terugschrijven onderdeel automatisch uitgeschakeld dat wanneer een beheerder wordt bijgewerkt met behulp van Azure AD Connect-wizard voor configuratie Azure AD Connect-synchronisatie opgelost. Dit wordt veroorzaakt door de wizard uitvoeren van de controle van de vereiste voor de bestaande configuratie van de Write-back van apparaat in on-premises AD en de controle is mislukt. De oplossing wordt de controle overslaan als write-back van apparaat al eerder is ingeschakeld.
* Voor het configureren van OE-filters, kunt u ofwel de Azure AD Connect-wizard of Synchronization Service Manager gebruiken. Voorheen als u de Azure AD Connect-wizard voor het configureren van OE-filters, zijn nieuwe organisatie-eenheden die zijn gemaakt daarna opgenomen voor directory-synchronisatie. Als u niet dat nieuwe OE's wilt moeten worden opgenomen, moet u het OE-filters met Synchronization Service Manager configureren. U kunt nu hetzelfde gedrag met behulp van Azure AD Connect-wizard bereiken.
* Een probleem dat ervoor zorgt dat de opgeslagen procedures worden gemaakt onder het schema van de beheerder installeren, in plaats van in het dbo-schema met Azure AD Connect moeten worden opgelost.
* Een probleem dat ervoor zorgt het kenmerk TrackingId is geretourneerd door Azure AD dat moeten worden weggelaten in de AAD Connect Server gebeurtenislogboeken opgelost. Dit probleem treedt op als Azure AD Connect een bericht voor de omleiding van Azure AD ontvangt en Azure AD Connect is geen verbinding maken met het eindpunt. De TrackingId wordt gebruikt door ondersteuningsmedewerkers correleren met side-service worden tijdens het oplossen van problemen.
* Wanneer Azure AD Connect LargeObject fout van Azure AD ontvangt, Azure AD Connect wordt een gebeurtenis met gebeurtenis-id 6941 en het bericht gegenereerd *'het ingerichte object is te groot. Verklein het aantal kenmerkwaarden voor dit object."* Op hetzelfde moment genereert Azure AD Connect ook een misleidend gebeurtenis met gebeurtenis-id 6900 en het bericht *' Microsoft.Online.Coexistence.ProvisionRetryException: kan niet communiceren met de Windows Azure Active Directory-service. "* Om verwarring, genereert Azure AD Connect niet langer de laatste gebeurtenis wanneer LargeObject fout wordt ontvangen.
* Een probleem dat ervoor zorgt de Synchronization Service Manager niet meer reageren dat tijdens het bijwerken van de configuratie voor algemene LDAP-connector opgelost.

**Nieuwe functies/verbeteringen:**

Azure AD Connect-synchronisatie
* Wijzigingen in synchronisatie-regel – de volgende synchronisatie regel wijzigingen zijn uitgevoerd:
  * Synchronisatie van bijgewerkte standaardregel ingesteld op niet exporteren kenmerken **userCertificate** en **userSMIMECertificate** als meer dan 15 waarden van de kenmerken hebben.
  * AD-kenmerken **werknemer-id** en **msExchBypassModerationLink** zijn nu opgenomen in de regelset van de standaard synchronisatie.
  * AD-kenmerk **photo** is verwijderd uit de standaard synchronisatie regelset.
  * Toegevoegd **preferredDataLocation** naar de Metaverse-schema en het schema van de AAD-Connector. Klanten die willen werken beide kenmerken in Azure AD kunnen implementeren, aangepaste synchronisatie regels om dit te doen. Meer informatie over het kenmerk, Raadpleeg het artikel gedeelte [Azure AD Connect-synchronisatie: hoe een wijziging aanbrengt in de standaardconfiguratie - synchronisatie inschakelen van PreferredDataLocation](active-directory-aadconnectsync-change-the-configuration.md#enable-synchronization-of-preferreddatalocation).
  * Toegevoegd **userType** naar de Metaverse-schema en het schema van de AAD-Connector. Klanten die willen werken beide kenmerken in Azure AD kunnen implementeren, aangepaste synchronisatie regels om dit te doen.

* Azure AD Connect automatisch kunt nu het gebruik van kenmerk ConsistencyGuid als het kenmerk Bronanker voor on-premises AD-objecten. Bovendien kunnen de Azure AD Connect vult het ConsistencyGuid-kenmerk met de waarde van het kenmerk objectGuid als deze leeg is. Deze functie is van toepassing op nieuwe implementatie alleen. Meer informatie over deze functie, Raadpleeg het artikel gedeelte [Azure AD Connect: concepten - msDS-ConsistencyGuid met als sourceAnchor ontwerpen](active-directory-aadconnect-design-concepts.md#using-msds-consistencyguid-as-sourceanchor).
* Nieuwe probleemoplossing cmdlet is Invoke-ADSyncDiagnostics toegevoegd om u te helpen bij het analyseren van synchronisatie van wachtwoordhash problemen met betrekking tot. Raadpleeg voor informatie over het gebruik van de cmdlet, artikel [Wachtwoordsynchronisatie met Azure AD Connect-synchronisatie oplossen](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-troubleshoot-password-synchronization).
* Azure AD Connect nu ondersteunt synchroniseren Mail-Enabled openbare map objecten uit een on-premises AD naar Azure AD. U kunt de functie met behulp van Azure AD Connect-wizard onder optionele functies inschakelen. Raadpleeg voor meer informatie over deze functie, artikel [Office 365 Directory op basis van rand blokkeren ondersteuning voor lokale Mail ingeschakeld openbare mappen](https://blogs.technet.microsoft.com/exchange/2017/05/19/office-365-directory-based-edge-blocking-support-for-on-premises-mail-enabled-public-folders).
* Azure AD Connect is vereist voor een AD DS-account om te synchroniseren van lokale AD. Voorheen als u Azure AD Connect met de snelle modus hebt geïnstalleerd, kunt u opgeven dat de referenties van een Enterprise-beheerdersaccount en Azure AD Connect maakt de AD DS-account vereist. Voor een aangepaste installatie en het forests toevoegen aan een bestaande implementatie, moest u echter om de AD DS-account in plaats daarvan. Nu hebt u ook de optie te bieden de referenties van een Enterprise-beheerdersaccount tijdens een aangepaste installatie kunt Azure AD Connect maken van de AD DS-account vereist.
* Azure AD Connect ondersteunt nu SQL AOA. Voordat u Azure AD Connect installeert, moet u SQL AOA inschakelen. Tijdens de installatie van detecteert Azure AD Connect of de opgegeven SQL-exemplaar is ingeschakeld voor SQL AOA. Als SQL AOA is ingeschakeld, wordt Azure AD Connect meer cijfers uit als SQL AOA is geconfigureerd voor gebruik van replicatie van synchrone of asynchrone replicatie. Bij het instellen van de beschikbaarheidsgroep-Listener, is het raadzaam dat u de eigenschap RegisterAllProvidersIP ingesteld op 0. Dit is omdat Azure AD Connect momenteel SQL Native Client verbinding maakt met SQL en SQL Native Client biedt geen ondersteuning voor het gebruik van de MultiSubNetFailover-eigenschap.
* Als u LocalDB als de database voor uw Azure AD Connect-server gebruikt en heeft de groottelimiet van 10 GB-bereikt, wordt de synchronisatieservice niet langer wordt gestart. Eerder, moet u ShrinkDatabase bewerking uitvoeren op de LocalDB om voldoende ruimte DB voor de synchronisatieservice starten te claimen. Waarna, kunt u de Synchronization Service Manager uitvoeringsgeschiedenis om de ingenomen ruimte DB te verwijderen. Nu kunt u de cmdlet Start-ADSyncPurgeRunHistory geschiedenisgegevens opschonen uitvoeren van LocalDB om DB ruimte te claimen. Verder kan deze cmdlet biedt ondersteuning voor de offline modus (door het opgeven van de parameter - offline) die kan worden gebruikt wanneer de synchronisatieservice niet wordt uitgevoerd. Opmerking: De offlinemodus kan alleen worden gebruikt als de synchronisatieservice niet wordt uitgevoerd en de database die wordt gebruikt LocalDB is.
* Om te verminderen de hoeveelheid opslagruimte vereist, Azure AD comprimeren Connect nu sync foutdetails bij het opslaan in LocalDB/SQL-databases. Bij het bijwerken van een oudere versie van Azure AD Connect naar deze versie, voert Azure AD Connect een eenmalige compressie op bestaande foutdetails voor synchronisatie.
* Voorheen na het bijwerken van de organisatie-eenheid filteren configuratie moet u handmatig uitvoeren volledige import om ervoor te zorgen bestaande objecten zijn correct opgenomen/uitgesloten van directory-synchronisatie. Nu Azure AD Connect volledige import automatisch geactiveerd tijdens de synchronisatie van de volgende cyclus. Bovendien kunnen de volledige import geldt alleen voor de AD-connectors van invloed op een door de update. Opmerking: deze verbetering is van toepassing op de organisatie-eenheid voor het filteren van updates die zijn gemaakt met behulp van alleen de Azure AD Connect-wizard. Het is niet van toepassing op de organisatie-eenheid voor het filteren van update gemaakt met behulp van de Synchronization Service Manager.
* Voorheen filteren op basis van een groep gebruikers, groepen ondersteunt en neem contact op met alleen objecten. Ondersteunt nu het, filteren op basis van de groep ook computerobjecten.
* U kunt eerder Connectorruimte gegevens verwijderen zonder de Azure AD Connect sync scheduler uit te schakelen. Synchronization Service Manager blokkeert de verwijdering van het Connectorgebied overgebracht gegevens nu, als wordt gedetecteerd dat de planner is ingeschakeld. Bovendien wordt wordt een waarschuwing geretourneerd klanten te informeren over het verlies van gegevens als de gegevens van de adresruimte Connector is verwijderd.
* Eerder, moet u uitschakelen schrijffouten PowerShell voor Azure AD Connect-wizard correct uit te voeren. Dit probleem is gedeeltelijk opgelost. U kunt PowerShell schrijffouten inschakelen als u Azure AD Connect-wizard voor het beheren van configuratie-synchronisatie. Als u Azure AD Connect-wizard voor het beheren van AD FS-configuratie, moet u PowerShell schrijffouten uitschakelen.



## <a name="114860"></a>1.1.486.0
Uitgebracht: April 2017

**Opgeloste problemen:**
* Het probleem opgelost waarbij Azure AD Connect zal niet goed geïnstalleerd op een gelokaliseerde versie van Windows Server.

## <a name="114840"></a>1.1.484.0
Uitgebracht: April 2017

**Bekende problemen:**

* Deze versie van Azure AD Connect zal niet goed geïnstalleerd als de volgende voorwaarden alle waar zijn:
   1. U kunt beide DirSync in-place upgrade of nieuwe installatie van Azure AD Connect wilt uitvoeren.
   2. U gebruikt een gelokaliseerde versie van Windows Server waarbij de naam van de ingebouwde groep Administrators op de server 'Administrators' niet.
   3. U gebruikt de standaard SQL Server 2012 Express LocalDB geïnstalleerd met Azure AD Connect in plaats van uw eigen volledig SQL bieden.

**Opgeloste problemen:**

Azure AD Connect-synchronisatie
* Een probleem opgelost waarbij de volledige synchronisatie stap door de sync scheduler wordt overgeslagen als een of meer connectors uitvoeringsprofiel voor die stap synchronisatie ontbreken. Bijvoorbeeld, u handmatig hebt toegevoegd een verbindingslijn Synchronization Service Manager met zonder te maken van een Delta-Import profiel voor het uitvoert. Deze oplossing zorgt ervoor dat de sync scheduler blijft Delta-Import voor andere connectors worden uitgevoerd.
* Een probleem opgelost waarbij de Synchronization Service onmiddellijk stopt met de verwerking een uitvoeringsprofiel wanneer het is een probleem optreedt bij een van de stappen uitvoeren. Deze oplossing zorgt ervoor dat de synchronisatieservice die stap uitvoeren wordt overgeslagen en doorgaat met het verwerken van de rest. U hebt bijvoorbeeld een Delta-Import profiel voor uw AD-connector uitvoert met meerdere uitgevoerde stappen (één voor elk lokale AD-domein). De synchronisatieservice wordt Delta-Import uitgevoerd met de AD-domeinen, zelfs als een van deze problemen met de netwerkverbinding heeft.
* Een probleem dat ervoor zorgt de update die Azure AD-Connector dat moet worden overgeslagen tijdens de automatische Upgrade opgelost.
* Een probleem dat ervoor zorgt Azure AD Connect onjuist bepalen dat of de server een domeincontroller tijdens de installatie is opgelost, welke toegepast zorgt ervoor dat DirSync-upgrade mislukken.
* Een probleem dat ervoor zorgt DirSync in-place upgrade dat naar elk uitvoeringsprofiel niet maken voor de Azure AD-Connector opgelost.
* Een probleem opgelost waarbij de Synchronization Service Manager-gebruikersinterface niet meer reageert als bij het configureren van algemene LDAP-Connector.

AD FS-beheer
* Een probleem opgelost waarbij de Azure AD Connect-wizard mislukt als de primaire AD FS-knooppunt is verplaatst naar een andere server.

Bureaublad SSO
* Een probleem opgelost in de Azure AD Connect-wizard waar het aanmeldingsscherm kunt u niet bureaublad SSO-functie inschakelen als u synchronisatie van wachtwoorden hebt gekozen als uw aanmeldoptie tijdens de installatie van nieuwe.

**Nieuwe functies/verbeteringen:**

Azure AD Connect-synchronisatie
* Azure AD Connect-synchronisatie ondersteunt nu het gebruik van virtuele-serviceaccount, beheerd serviceaccount en beheerd serviceaccount als serviceaccount. Dit geldt voor de nieuwe installatie van Azure AD Connect. Wanneer u Azure AD Connect installeert:
    * Standaard Azure AD Connect-wizard maakt een virtuele-Account en gebruikt als serviceaccount ervan.
    * Als u op een domeincontroller installeert, valt Azure AD Connect terug naar vorige gedrag waarbij een domeingebruikersaccount maakt en in plaats daarvan gebruikt als serviceaccount ervan.
    * U kunt het standaardgedrag vervangen door een van de volgende:
      * Een groep beheerd serviceaccount
      * Een beheerd serviceaccount
      * Een domeingebruikersaccount zijn.
      * Een lokale gebruikersaccount
* Als u een naar een nieuwe build van Azure AD Connect die upgrade voorheen connectors bijwerken of wijzigingen in synchronisatie-regel, Azure AD Connect de cyclus van een volledige synchronisatie wordt geactiveerd. Azure AD Connect activeert nu selectief volledige Import stap alleen voor connectors met update, en volledige synchronisatie alleen voor connectors met wijzigingen in synchronisatie-regel.
* De verwijdering exporteren drempelwaarde geldt voorheen alleen voor uitvoer die worden geactiveerd via de sync scheduler. De functie is nu uitgebreid met uitvoer handmatig geactiveerd door de klant op basis van de Synchronization Service Manager.
* Er is een serviceconfiguratie waarmee wordt aangegeven of Password Synchronization-functie is ingeschakeld voor uw tenant op uw Azure AD-tenant. Eerder, is het eenvoudig voor de configuratie van de service niet correct worden geconfigureerd met Azure AD Connect wanneer u een actief en een server met tijdelijke bestanden. Nu Azure AD Connect probeert de serviceconfiguratie om consistent te houden met uw actieve alleen Azure AD Connect-server.
* Azure AD Connect wizard nu detecteert en een waarschuwing wordt gegeven als lokale AD heeft geen AD Recycle Bin is ingeschakeld.
* Voorheen geëxporteerd naar Azure AD-time-out en mislukt als de gecombineerde grootte van de objecten in de batch een bepaalde drempelwaarde overschrijdt. Nu probeert de synchronisatieservice te verzenden van de objecten in afzonderlijke, kleinere batches als het probleem is opgetreden.
* De synchronisatie Service Key Management-toepassing is verwijderd uit het Windows-Menu Start. Beheer van de versleutelingssleutel blijven worden ondersteund via de opdrachtregelinterface miiskmu.exe gebruiken. Raadpleeg voor informatie over het beheren van de versleutelingssleutel, artikel [opgegeven van de versleutelingssleutel van de Azure AD Connect-synchronisatie](https://docs.microsoft.com/azure/active-directory/connect/active-directory-aadconnectsync-change-serviceacct-pass#abandoning-the-azure-ad-connect-sync-encryption-key).
* Voorheen als u het wachtwoord voor de Azure AD Connect sync-serviceaccount wijzigen, zich de Synchronization Service niet kunnen starten correct totdat u hebt de versleutelingssleutel afgebroken en het wachtwoord voor de Azure AD Connect sync-serviceaccount opnieuw geïnitialiseerd. Dit is nu niet langer vereist.

Bureaublad SSO

* Azure AD Connect-wizard is niet langer vereist poort 9090 bij het configureren van Pass through-verificatie en SSO-bureaublad op het netwerk worden geopend. Alleen poort 443 is vereist. 

## <a name="114430"></a>1.1.443.0
Uitgebracht: Maart 2017

**Opgeloste problemen:**

Azure AD Connect-synchronisatie
* Een probleem waardoor Azure AD Connect-wizard mislukken als de weergavenaam van de Azure AD-Connector, de initiële onmicrosoft.com-domein dat is toegewezen aan de Azure AD-tenant niet bevat is opgelost.
* Een probleem waardoor Azure AD Connect-wizard mislukt tijdens het maken van verbinding met SQL-database wanneer het wachtwoord van het synchronisatieserviceaccount speciale tekens bevatten zoals apostrof, dubbele punt en ruimte bevat opgelost.
* Een probleem waardoor de fout 'de dimage heeft een anker die anders is dan de installatiekopie' opgelost om te worden uitgevoerd op een Azure AD Connect-server in de faseringsmodus, nadat u hebt tijdelijk een on-premises uitgesloten AD object wordt gesynchroniseerd en opnieuw opgenomen voor het synchroniseren.
* Een probleem waardoor de fout 'het object gevonden met de DN-naam is een phantomstuklijst' opgelost om te worden uitgevoerd op een Azure AD Connect-server in de faseringsmodus, nadat u tijdelijk een on-premises uitgesloten hebt AD-object wordt gesynchroniseerd en opnieuw opgenomen voor het synchroniseren van.

AD FS-beheer
* Vaste een probleem waarbij Azure AD Connect-wizard niet bijwerken van de AD FS-configuratie en de juiste claims voor de relying party trust niet instellen nadat alternatieve aanmeldings-ID is geconfigureerd.
* Een probleem opgelost waarbij Azure AD Connect-wizard is niet goed verwerkt AD FS-servers waarvan serviceaccounts zijn geconfigureerd met de indeling userPrincipalName in plaats van sAMAccountName indeling.

Pass-through-verificatie
* Een probleem waardoor Azure AD Connect-wizard mislukken als doorgeven via verificatie is geselecteerd, maar de registratie van de connector niet kan worden opgelost.
* Vaste een probleem waardoor Azure AD Connect-wizard voor het overslaan van de validatie op de aanmeldingspagina methode geselecteerd controleert als bureaublad SSO-functie is ingeschakeld.

Wachtwoord opnieuw instellen
* Een probleem dat ertoe leiden de server Azure AAD Connect niet proberen opnieuw verbinding te maken dat kan als de verbinding is afgebroken door een firewall of proxyserver opgelost.

**Nieuwe functies/verbeteringen:**

Azure AD Connect-synchronisatie
* De cmdlet Get-ADSyncScheduler retourneert nu een nieuwe Boole-eigenschap met de naam SyncCycleInProgress. Als de geretourneerde waarde true is, betekent dit dat er een cyclus geplande synchronisatie uitgevoerd bestaat.
* Doelmap voor het opslaan van Azure AD Connect-installatie en installatielogboeken heeft voor het verbeteren van de toegankelijkheid van de logboekbestanden van %localappdata%\AADConnect verplaatst naar %programdata%\AADConnect.

AD FS-beheer
* Ondersteuning toegevoegd voor het bijwerken van SSL-certificaat voor AD FS-Farm.
* Ondersteuning toegevoegd voor het beheer van AD FS-2016.
* Nu kunt u bestaande gMSA (groep beheerd serviceaccount gebruiken) opgeven tijdens de installatie van AD FS.
* U kunt nu SHA-256 configureren als de handtekening hash-algoritme voor Azure AD-vertrouwensrelatie voor relying party.

Wachtwoord opnieuw instellen
* Verbeteringen aan het product te laten functioneren in omgevingen met strengere firewallregels toestaan geïntroduceerd.
* Verbinding met verbeterde betrouwbaarheid Azure Service Bus.

## <a name="113800"></a>1.1.380.0
Uitgebracht: December 2016

**Opgelost probleem:**

* Het probleem opgelost waarbij de claimregel issuerid voor Active Directory Federation Services (AD FS) ontbreekt in deze versie.

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie Azure AD Connect automatische Upgrade.

## <a name="113710"></a>1.1.371.0
Uitgebracht: December 2016

**Bekende problemen:**

* De claimregel issuerid voor AD FS ontbreekt in deze versie. De claimregel issuerid is vereist als u bij het federeren van meerdere domeinen met Azure Active Directory (Azure AD). Als u Azure AD Connect worden gebruikt voor het beheren van uw on-premises AD FS-implementatie, een upgrade naar deze versie wordt verwijderd van de bestaande issuerid claimregel van uw AD FS-configuratie. U kunt dit probleem omzeilen door de claimregel issuerid toe te voegen na de installatie/upgrade. Voor meer informatie over het toevoegen van de issuerid claimregelsjablonen, Raadpleeg dit artikel op [ondersteuning voor meerdere domeinen voor federatie met Azure AD](active-directory-aadconnect-multiple-domains.md).

**Opgelost probleem:**

* Als poort 9090 is niet geopend voor de uitgaande verbinding, wordt de Azure AD Connect-installatie of upgrade mislukt.

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie Azure AD Connect automatische Upgrade.

## <a name="113700"></a>1.1.370.0
Uitgebracht: December 2016

**Bekende problemen:**

* De claimregel issuerid voor AD FS ontbreekt in deze versie. De claimregel issuerid is vereist als u meerdere domeinen met Azure AD federeert. Als u Azure AD Connect worden gebruikt voor het beheren van uw on-premises AD FS-implementatie, een upgrade naar deze versie wordt verwijderd van de bestaande issuerid claimregel van uw AD FS-configuratie. U kunt dit probleem omzeilen door de claimregel issuerid toe te voegen na de installatie/upgrade. Voor meer informatie over het toevoegen van issuerid claimregelsjablonen, Raadpleeg dit artikel op [ondersteuning voor meerdere domeinen voor federatie met Azure AD](active-directory-aadconnect-multiple-domains.md).
* Poort 9090 moet openen uitgaande om installatie te voltooien.

**Nieuwe functies:**

* Pass through-verificatie (Preview).

>[!NOTE]
>Deze versie is niet beschikbaar voor klanten via de functie Azure AD Connect automatische Upgrade.

## <a name="113430"></a>1.1.343.0
Uitgebracht: November 2016

**Bekende problemen:**

* De claimregel issuerid voor AD FS ontbreekt in deze versie. De claimregel issuerid is vereist als u meerdere domeinen met Azure AD federeert. Als u Azure AD Connect worden gebruikt voor het beheren van uw on-premises AD FS-implementatie, een upgrade naar deze versie wordt verwijderd van de bestaande issuerid claimregel van uw AD FS-configuratie. U kunt dit probleem omzeilen door de claimregel issuerid toe te voegen na de installatie/upgrade. Voor meer informatie over het toevoegen van issuerid claimregelsjablonen, Raadpleeg dit artikel op [ondersteuning voor meerdere domeinen voor federatie met Azure AD](active-directory-aadconnect-multiple-domains.md).

**Opgeloste problemen:**

* Azure AD Connect installeert mislukt soms, omdat dit kan een lokale service-account waarvan het wachtwoord voldoet aan het niveau van complexiteit opgegeven door de organisatie-wachtwoordbeleid niet maken.
* Een probleem opgelost waarbij join regels niet opnieuw geëvalueerd wanneer een object in de connectorruimte gelijktijdig buiten het bereik wordt voor een regel toevoegen en worden in het bereik voor een andere. Dit kan gebeuren als er twee of meer join regels waarvan join-voorwaarden sluiten elkaar wederzijds uit.
* Een probleem opgelost waarbij inkomende synchronisatieregels (van Azure AD), die geen join-regels bevatten, worden niet verwerkt als ze hebben lagere prioriteit waarden dan de join-regels met.

**Verbeteringen:**

* Ondersteuning toegevoegd voor de installatie van Azure AD Connect op Windows Server 2016 Standard of hoger.
* Ondersteuning toegevoegd voor het gebruik van SQL Server 2016 als de externe database voor Azure AD Connect.

## <a name="112810"></a>1.1.281.0
Uitgebracht: Augustus 2016

**Opgeloste problemen:**

* Wijzigingen in synchronisatie-interval komen niet plaatsvinden pas nadat de volgende synchronisatiecyclus voltooid is.
* Azure AD Connect-wizard wordt niet geaccepteerd voor een Azure AD-account waarvan de gebruikersnaam wordt gestart met een onderstrepingsteken (\_).
* Azure AD Connect-wizard mislukt voor de verificatie van de Azure AD-account als het accountwachtwoord te veel tekens bevat. Foutbericht 'kan niet om referenties te valideren. Een onverwachte fout opgetreden." wordt geretourneerd.
* Verwijderen van server voor fasering schakelt Wachtwoordsynchronisatie in Azure AD-tenant en zorgt ervoor dat Wachtwoordsynchronisatie mislukt met actieve server.
* Wachtwoordsynchronisatie is mislukt in ongewoon gevallen wanneer er geen wachtwoord-hash die is opgeslagen op de gebruiker.
* Als Azure AD Connect-server van de faseringsmodus is ingeschakeld, is niet tijdelijk wachtwoord terugschrijven uitgeschakeld.
* Azure AD Connect-wizard worden de werkelijke Wachtwoordsynchronisatie en wachtwoord terugschrijven configuratie geen weergegeven, wanneer de server in de faseringsmodus. Altijd wordt deze als uitgeschakeld.
* Configuratiewijzigingen voor Wachtwoordsynchronisatie en wachtwoord terugschrijven zijn niet permanent opgeslagen door Azure AD Connect-wizard als server in de faseringsmodus.

**Verbeteringen:**

* De cmdlet Start-ADSyncSyncCycle om aan te geven of het een nieuwe synchronisatiecyclus is gestart of niet is bijgewerkt.
* De cmdlet Stop ADSyncSyncCycle afgebroken synchronisatiecyclus en bewerking, die momenteel uitgevoerd worden toegevoegd.
* Bijgewerkt in de cmdlet Stop ADSyncScheduler afgebroken synchronisatiecyclus en bewerking, die momenteel uitgevoerd worden.
* Bij het configureren van [Directory uitbreidingen](active-directory-aadconnectsync-feature-directory-extensions.md) in Azure AD Connect-wizard kan nu de Azure AD-kenmerk van het type 'Teletex tekenreeks' worden geselecteerd.

## <a name="111890"></a>1.1.189.0
Uitgebracht: Juni 2016

**Opgeloste problemen en verbeteringen:**

* Azure AD Connect kan nu worden geïnstalleerd op een server FIPS-compatibel.
  * Zie voor Wachtwoordsynchronisatie [Wachtwoordsynchronisatie en FIPS](active-directory-aadconnectsync-implement-password-synchronization.md#password-synchronization-and-fips).
* Een probleem opgelost waarbij een NetBIOS-naam kan niet worden omgezet naar de FQDN-naam in de Active Directory-Connector.

## <a name="111800"></a>1.1.180.0
Uitgebracht: Mei 2016

**Nieuwe functies:**

* Waarschuwt en kunt u domeinen verifiëren als u deze voordat u Azure AD Connect niet hebt gedaan.
* Ondersteuning toegevoegd voor [Microsoft Cloud Duitsland](active-directory-aadconnect-instances.md#microsoft-cloud-germany).
* Ondersteuning toegevoegd voor de meest recente [Microsoft Azure Government cloud](active-directory-aadconnect-instances.md#microsoft-azure-government-cloud) infrastructuur met nieuwe URL-vereisten.

**Opgeloste problemen en verbeteringen:**

* Toegevoegd voor het filteren van de synchronisatie regel Editor gemakkelijker om regels voor synchronisatie te vinden.
* Verbeterde prestaties bij het verwijderen van een connectorruimte.
* Een probleem opgelost wanneer hetzelfde object is verwijderd en toegevoegd in dezelfde uitvoeren (aangeroepen verwijderen/toevoegen).
* Een uitgeschakelde synchronisatie regel niet meer wordt opnieuw ingeschakeld opgenomen objecten en kenmerken op upgrade of Active directory-schema vernieuwen.

## <a name="111300"></a>1.1.130.0
Uitgebracht: April 2016

**Nieuwe functies:**

* Ondersteuning toegevoegd voor kenmerken met meerdere waarden [Directory uitbreidingen](active-directory-aadconnectsync-feature-directory-extensions.md).
* Ondersteuning toegevoegd voor meer configuratie variaties voor [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) worden overwogen in aanmerking voor upgrade.
* Sommige cmdlets voor toegevoegd [aangepaste scheduler](active-directory-aadconnectsync-feature-scheduler.md#custom-scheduler).

## <a name="111190"></a>1.1.119.0
Uitgebracht: Maart 2016

**Opgeloste problemen:**

* Aangebracht wordt ervoor Express-installatie kan niet worden gebruikt op Windows Server 2008 (pre-R2), omdat het wachtwoord synchroniseren niet ondersteund op dit besturingssysteem.
* Upgrade van DirSync met een aangepast filterconfiguratie werkt niet zoals verwacht.
* Bij een upgrade naar een nieuwere versie en er zijn geen wijzigingen in de configuratie, een volledige import/synchronisatie moet niet worden gepland.

## <a name="111100"></a>1.1.110.0
Uitgebracht: Februari 2016

**Opgeloste problemen:**

* Upgrade van eerdere versies werkt niet als de installatie niet in de standaardmap C:\Program Files is.
* Als u installeert en schakel **Start het synchronisatieproces** aan het einde van de installatiewizard uitvoeren van de installatiewizard van een tweede keer niet mogelijk de planner.
* De planner werkt niet zoals verwacht op servers waarop de US-en datum/tijd-indeling niet wordt gebruikt. Blokkeert tevens `Get-ADSyncScheduler` juiste retourneren.
* Als u een eerdere versie van Azure AD Connect met AD FS hebt geïnstalleerd als de optie voor aanmelden en upgrade, niet kunt u de installatiewizard opnieuw uitvoeren.

## <a name="111050"></a>1.1.105.0
Uitgebracht: Februari 2016

**Nieuwe functies:**

* [Automatische upgrade](active-directory-aadconnect-feature-automatic-upgrade.md) functie voor snelle instellingen klanten.
* Ondersteuning voor de globale beheerder met behulp van Azure multi-factor Authentication en Privileged Identity Management in de installatiewizard.
  * U wilt toestaan dat de proxy waarmee u verkeer naar https://secure.aadcdn.microsoftonline-p.com ook als u multi-factor Authentication gebruikt.
  * U moet https://secure.aadcdn.microsoftonline-p.com toevoegen aan de lijst met vertrouwde sites voor multi-factor Authentication goed werkt.
* Het wijzigen van de gebruiker aanmelden methode na de eerste installatie toegestaan.
* Toestaan dat [domein en OE filteren](active-directory-aadconnect-get-started-custom.md#domain-and-ou-filtering) in de installatiewizard. Hierdoor kunnen ook verbinding maken met forests waarin niet alle domeinen beschikbaar zijn.
* [Scheduler](active-directory-aadconnectsync-feature-scheduler.md) is ingebouwd in de synchronisatie-engine.

**Functies van preview gepromoveerd tot GA:**

* [Apparaat terugschrijven](active-directory-aadconnect-feature-device-writeback.md).
* [Directory-uitbreidingen](active-directory-aadconnectsync-feature-directory-extensions.md).

**Nieuwe preview-functies:**

* De nieuwe standaard cyclus interval is 30 minuten gesynchroniseerd. Drie uur voor alle eerdere versies werd gebruikt. Voegt ondersteuning wijzigen de [scheduler](active-directory-aadconnectsync-feature-scheduler.md) gedrag.

**Opgeloste problemen:**

* Controleer of DNS-domeinen pagina herkend niet altijd de domeinen.
* Als u wordt gevraagd om domeinreferenties van beheerder bij het configureren van AD FS.
* De on-premises AD-accounts worden niet herkend door de installatiewizard als zich in een domein met een andere DNS-structuur dan het hoofddomein.

## <a name="1091310"></a>1.0.9131.0
Uitgebracht: December 2015

**Opgeloste problemen:**

* Wachtwoordsynchronisatie werkt mogelijk niet wanneer u wachtwoorden in Active Directory Domain Services (AD DS), maar werkt wijzigen wanneer u een wachtwoord instelt.
* Wanneer er een proxyserver, mislukt de authenticatie naar Azure AD mogelijk tijdens de installatie of als een upgrade op de configuratiepagina is geannuleerd.
* Bijwerken van een vorige versie van Azure AD Connect met een volledige SQL Server-instantie mislukt als u niet een SQL Server-systeembeheerder (SA).
* Bijwerken van een vorige versie van Azure AD Connect met een externe SQL Server, ziet u de fout 'Kan geen toegang tot de ADSync SQL database'.

## <a name="1091250"></a>1.0.9125.0
Uitgebracht: November 2015

**Nieuwe functies:**

* Kunt opnieuw configureren van AD FS en Azure AD-vertrouwensrelatie.
* Kan het Active Directory-schema vernieuwen en sync-regels genereren.
* Een synchronisatieregel kan worden uitgeschakeld.
* Kan 'AuthoritativeNull' te definiëren als een nieuwe literal in een synchronisatieregel.

**Nieuwe preview-functies:**

* [Azure AD Connect Health voor synchroniseren](../connect-health/active-directory-aadconnect-health-sync.md).
* Ondersteuning voor [Azure AD Domain Services](../active-directory-passwords-update-your-own-password.md) Wachtwoordsynchronisatie.

**Nieuwe ondersteunde scenario:**

* Biedt ondersteuning voor meerdere lokale Exchange-organisaties. Zie voor meer informatie [hybride implementaties met meerdere Active Directory-forests](https://technet.microsoft.com/library/jj873754.aspx).

**Opgeloste problemen:**

* Problemen met synchronisatie van wachtwoord:
  * Een object verplaatst van buiten het bereik aan in het bereik heeft geen bijbehorende wachtwoord is gesynchroniseerd. Dit omvat de organisatie-eenheid en kenmerkfilters inschakelt.
  * Een volledige Wachtwoordsynchronisatie is niet vereist als u een nieuwe organisatie-eenheid wilt opnemen synchroon selecteren.
  * Wanneer een uitgeschakelde gebruiker is ingeschakeld wordt het wachtwoord niet synchroniseren.
  * De wachtrij van wachtwoord opnieuw is oneindig en de vorige limiet van 5000 objecten worden gesteld is verwijderd.
* Kan geen verbinding maken met Active Directory met Windows Server 2016 forest-functionaliteitsniveau.
* Kan de groep die wordt gebruikt voor het filteren van de groep na de eerste installatie wijzigen.
* Maakt een nieuw gebruikersprofiel niet langer op de Azure AD Connect-server voor elke gebruiker een wachtwoordwijziging met wachtwoord terugschrijven ingeschakeld doen.
* Niet met lange Integer waarden synchroon regels scopes.
* Het selectievakje 'apparaat terugschrijven' blijft uitgeschakeld als er domeincontrollers niet bereikbaar.

## <a name="1086670"></a>1.0.8667.0
Uitgebracht: Augustus 2015

**Nieuwe functies:**

* De Azure AD Connect-installatiewizard is nu gelokaliseerd voor alle Windows Server-talen.
* Ondersteuning toegevoegd voor het account ontgrendelen wanneer u Azure AD-wachtwoordbeheer.

**Opgeloste problemen:**

* Azure AD Connect-installatiewizard loopt vast als een andere gebruiker heeft nog steeds installatie in plaats van de persoon die de installatie het eerst wordt gestart.
* Als een eerdere installatie van Azure AD Connect niet foutloos Azure AD Connect-synchronisatie verwijderen, is het niet mogelijk om opnieuw te installeren.
* Kan Azure AD Connect met snelle installatie als de gebruiker zich niet in het hoofddomein van het forest of als een niet-Engelse versie van Active Directory wordt gebruikt niet installeren.
* Als de FQDN-naam van het Active Directory-gebruikersaccount kan niet worden omgezet, wordt een foutbericht misleidend 'Mislukt om door te voeren van het schema' weergegeven.
* Als het account dat wordt gebruikt op de Active Directory-Connector is gewijzigd buiten de wizard, mislukt de wizard op latere wordt uitgevoerd.
* Azure AD Connect is soms niet kan worden geïnstalleerd op een domeincontroller.
* Kan niet in- en 'faseringsmodus' uitschakelen als uitbreidingskenmerken zijn toegevoegd.
* Wachtwoord terugschrijven is mislukt in bepaalde configuraties vanwege een ongeldig wachtwoord op de Active Directory-Connector.
* DirSync kan niet worden bijgewerkt als een DN-naam (Distinguished Name) wordt gebruikt in het kenmerk filteren.
* Buitensporig CPU-gebruik bij gebruik van wachtwoord opnieuw instellen.

**Verwijderde preview-functies:**

* De preview-functie [Write-back van gebruiker](active-directory-aadconnect-feature-preview.md#user-writeback) tijdelijk op basis van feedback van onze klanten preview is verwijderd. Dit wordt later opnieuw worden toegevoegd nadat de opgegeven feedback is verholpen.

## <a name="1086410"></a>1.0.8641.0
Uitgebracht: Juni 2015

**Initiële versie van Azure AD Connect.**

Naam van de gewijzigde van Azure AD Sync naar Azure AD Connect.

**Nieuwe functies:**

* [Snelle instellingen](active-directory-aadconnect-get-started-express.md) installatie
* Kan [AD FS configureren](active-directory-aadconnect-get-started-custom.md#configuring-federation-with-ad-fs)
* Kan [upgraden van DirSync](active-directory-aadconnect-dirsync-upgrade-get-started.md)
* [Onopzettelijke verwijderingen voorkomen](active-directory-aadconnectsync-feature-prevent-accidental-deletes.md)
* Geïntroduceerd [faseringsmodus](active-directory-aadconnectsync-operations.md#staging-mode)

**Nieuwe preview-functies:**

* [Write-back van gebruiker](active-directory-aadconnect-feature-preview.md#user-writeback)
* [Write-back van groep](active-directory-aadconnect-feature-preview.md#group-writeback)
* [Write-back van apparaat](active-directory-aadconnect-feature-device-writeback.md)
* [Uitbreidingen van de directory](active-directory-aadconnect-feature-preview.md)

## <a name="104940501"></a>1.0.494.0501
Uitgebracht: Mei 2015

**Nieuwe vereiste:**

* Azure AD Sync is nu vereist versie van .NET Framework 4.5.1 moet worden geïnstalleerd.

**Opgeloste problemen:**

* Wachtwoord terugschrijven van Azure AD is mislukt met een Azure Service Bus-verbindingsfout.

## <a name="104910413"></a>1.0.491.0413
Uitgebracht: April 2015

**Opgeloste problemen en verbeteringen:**

* De Active Directory-Connector verwerkt geen verwijderingen correct als de Prullenbak is ingeschakeld en er meerdere domeinen in het forest zijn.
* De prestaties van importbewerkingen is voor de Azure Active Directory-Connector verbeterd.
* Wanneer een groep het lidmaatschap limiet heeft overschreden (standaard de limiet is ingesteld op 50.000 objecten), de groep is verwijderd in Azure Active Directory. De groep niet verwijderd met de nieuwe functies, een fout gegenereerd en nieuwe lidmaatschapswijzigingen worden niet geëxporteerd.
* Een nieuw object kan niet worden ingericht als een gefaseerde verwijderen met de dezelfde DN-naam al aanwezig in het connectorgebied overgebracht is.
* Sommige objecten zijn gemarkeerd voor synchronisatie tijdens een Deltasynchronisatie, zelfs als er geen wijziging voorgefaseerd op het object is.
* De voorkeurslijst DC waardoor de Wachtwoordsynchronisatie van een worden ook verwijderd.
* CSExportAnalyzer, kent problemen met de statussen van sommige objecten.

**Nieuwe functies:**

* Een join kan nu verbinding maken met het objecttype 'ANY' in de MV.

## <a name="104850222"></a>1.0.485.0222
Uitgebracht: Februari 2015

**Verbeteringen:**

* Verbeterde import van de prestaties.

**Opgeloste problemen:**

* Wachtwoordsynchronisatie zich houdt aan het cloudFiltered-kenmerk dat wordt gebruikt door het kenmerk filteren. Gefilterde objecten zijn niet langer in het bereik voor Wachtwoordsynchronisatie.
* In zeldzame gevallen waarbij de topologie veel domeincontrollers 'D werkt Wachtwoordsynchronisatie niet.
* 'Gestopt-server' bij het importeren vanuit de Azure AD-Connector na het beheer van apparaten is ingeschakeld in Azure AD/Intune.
* Lid worden van afwijkende beveiligings-Principals (FSP's) van meerdere domeinen in hetzelfde forest, veroorzaakt een dubbelzinnige join-fout.

## <a name="104751202"></a>1.0.475.1202
Uitgebracht: December 2014

**Nieuwe functies:**

* Wachtwoordsynchronisatie met filteren op basis van een kenmerk wordt nu ondersteund. Zie voor meer informatie [Wachtwoordsynchronisatie met filtering](active-directory-aadconnectsync-configure-filtering.md).
* Het kenmerk msDS-ExternalDirectoryObjectID teruggeschreven naar Active Directory. Deze functie wordt ondersteuning toegevoegd voor Office 365-toepassingen. OAuth2 wordt gebruikt voor toegang tot de postvakken in een hybride implementatie voor Exchange Online en On-Premises.

**Vaste problemen met de upgrade:**

* Er is een nieuwere versie van de aanmeldhulp beschikbaar op de server.
* Een aangepaste installatiepad is gebruikt voor het installeren van Azure AD Sync.
* Een ongeldige aangepaste join criterium blokkeert de upgrade.

**Andere correcties voor:**

* De sjablonen voor de Office Professional Plus opgelost.
* Vaste installatieproblemen veroorzaakt door de gebruikersnamen die met een streepje beginnen.
* Vast dat de instelling sourceAnchor verloren gaan wanneer de installatiewizard wordt een tweede keer uitgevoerd.
* Vaste ETW-tracering voor Wachtwoordsynchronisatie.

## <a name="104701023"></a>1.0.470.1023
Uitgebracht: Oktober 2014

**Nieuwe functies:**

* Wachtwoordsynchronisatie van meerdere lokale Active Directory naar Azure AD.
* Gebruikersinterface voor installatie op alle Windows Server-talen gelokaliseerd.

**Een upgrade uitvoert van AADSync 1.0 NH**

Als u al Azure AD Sync geïnstalleerd hebt, is er een extra stap die u uitvoeren moet als u een van de out-of-box-synchronisatieregels zijn gewijzigd. Nadat u hebt bijgewerkt naar de 1.0.470.1023 vrijgeven, de regels die u hebt gewijzigd worden gedupliceerd synchronisatie. Ga als volgt te werk voor elke synchronisatieregel gewijzigde:

1.  Zoek de synchronisatieregel die u hebt gewijzigd, en noteer de wijzigingen.
* De synchronisatieregel verwijderen.
* Zoek de nieuwe synchronisatieregel die door Azure AD Sync is gemaakt en moet u de wijzigingen opnieuw.

**Machtigingen voor het Active Directory-account**

De Active Directory-account, moet aanvullende machtigingen om het lezen van de wachtwoord-hashes van Active Directory te kunnen worden toegekend. De machtigingen te verlenen zijn met de naam 'Directorywijzigingen repliceren' en "Replicating Directory alle gewijzigd." Beide machtigingen zijn vereist om te kunnen worden gelezen van de wachtwoord-hashes.

## <a name="104190911"></a>1.0.419.0911
Uitgebracht: September 2014

**Initiële versie van Azure AD Sync.**

## <a name="next-steps"></a>Volgende stappen
Lees meer over het [integreren van uw on-premises identiteiten met Azure Active Directory](active-directory-aadconnect.md).

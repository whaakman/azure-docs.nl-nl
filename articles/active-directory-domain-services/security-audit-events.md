---
title: Beveiligingscontrole voor Azure AD Domain Services inschakelen | Microsoft Docs
description: Beveiligingscontrole voor Azure AD Domain Services inschakelen
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 662362c3-1a5e-4e94-ae09-8e4254443697
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: iainfou
ms.openlocfilehash: 3105296b3c670d3d44789c93878fa1fc6076973b
ms.sourcegitcommit: d2785f020e134c3680ca1c8500aa2c0211aa1e24
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/04/2019
ms.locfileid: "67566493"
---
# <a name="enable-security-audits-for-azure-ad-domain-services-preview"></a>Beveiligingscontroles inschakelen voor Azure AD Domain Services (Preview)
Azure AD-domein beveiligingscontrole in de Service kan klanten het gebruik van de Azure AD Domain Services-portal op gebeurtenissen voor beveiligingscontrole stream beoogde resources. Resources die deze gebeurtenissen kunnen ontvangen omvatten Azure Storage, Azure Log Analytics-werkruimten of Azure Event Hub. Kort na de gebeurtenissen voor beveiligingscontrole is ingeschakeld, verzendt Azure AD Domain Services de gecontroleerde gebeurtenissen voor de geselecteerde categorie aan de betreffende resource. Gebeurtenissen voor beveiligingscontrole kunnen klanten archief gecontroleerde gebeurtenissen naar Azure-opslag. Klanten kunnen bovendien gebeurtenissen streamen naar security information en event management (SIEM)-software (of equivalent) met behulp van eventhubs, of hun eigen analyse en inzichten met behulp van Azure Log Analytics vanuit Azure portal doen. 

> [!IMPORTANT]
> Beveiligingscontrole van Azure AD Domain Services is alleen beschikbaar op Azure Resource Manager-exemplaren voor Azure AD Domain Services.
>
>

## <a name="auditing-event-categories"></a>Controlecategorieën
Beveiligingscontrole van Azure AD Domain Services worden uitgelijnd met traditionele controle die wordt geleverd voor Active Directory Domain Services-domeincontrollers. Hergebruik van bestaande audit-patronen, zorgt ervoor dat dezelfde logica kan worden gebruikt bij het analyseren van de gebeurtenissen. Beveiligingscontrole van Azure AD Domain Services bevat de volgende gebeurteniscategorieën.

| De naam van de audit-categorie | Description |
|:---|:---|
| Aanmelding met account|Voert een controle uit verificatiepogingen accountgegevens op een domeincontroller of op een lokale Security Accounts Manager (SAM).</p>Aanmelden en afmelden, beleidsinstellingen en -gebeurtenissen bijhouden probeert te krijgen tot een bepaalde computer. Instellingen en gebeurtenissen in deze categorie zijn gericht op de database die wordt gebruikt. Deze categorie omvat de volgende subcategorieën:<ul><li>[Validatie van referenties controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-credential-validation)</li><li>[Kerberos-Authenticatieservice controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-authentication-service)</li><li>[Kerberos-Serviceticketbewerkingen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kerberos-service-ticket-operations)</li><li>[Andere gebeurtenissen voor aanmelden/afmelden controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li></ul>|
| Accountbeheer|Controles wijzigingen in gebruikers- en computeraccounts en groepen. Deze categorie omvat de volgende subcategorieën:<ul><li>[Beheer van toepassingsgroepen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-group-management)</li><li>[Beheer van computeraccounts controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-computer-account-management)</li><li>[Beheer van distributiegroepen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-distribution-group-management)</li><li>[Andere accountbeheer](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-account-management-events)</li><li>[Beheer van beveiligingsgroepen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-group-management)</li><li>[Gebruikersaccountbeheer controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-account-management)</li></ul>|
| Detail Tracking|Controle-activiteiten van afzonderlijke toepassingen en gebruikers op die computer, en om te begrijpen hoe een computer wordt gebruikt. Deze categorie omvat de volgende subcategorieën:<ul><li>[DPAPI-activiteiten controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-dpapi-activity)</li><li>[PNP-activiteit controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-pnp-activity)</li><li>[Maken van processen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-creation)</li><li>[Beëindigen van processen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-process-termination)</li><li>[RPC-gebeurtenissen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-rpc-events)</li></ul>|
| Directory Services Access|Controle probeert te openen en te wijzigen van objecten in Active Directory Domain Services (AD DS). Deze gebeurtenissen worden vastgelegd alleen op domeincontrollers audit. Deze categorie omvat de volgende subcategorieën:<ul><li>[Gedetailleerde directoryservice-replicatie controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-directory-service-replication)</li><li>[Active Directory-Servicetoegang controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-access)</li><li>[Wijzigingen in de Directory Service controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-changes)</li><li>[Active Directory-replicatie controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-directory-service-replication)</li></ul>|
| Logon-Logoff|Voert een controle uit pogingen voor aanmelding bij een computer, interactief of via een netwerk. Deze gebeurtenissen zijn nuttig voor het bijhouden van gebruikersactiviteit en identificeren van mogelijke aanvallen op netwerkbronnen. Deze categorie omvat de volgende subcategorieën:<ul><li>[Accountvergrendeling controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-account-lockout)</li><li>[Gebruiker/apparaat Claims controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-user-device-claims)</li><li>[IPsec uitgebreide modus controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-extended-mode)</li><li>[Lidmaatschap van controle](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-group-membership)</li><li>[IPsec-hoofdmodus controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-main-mode)</li><li>[IPsec snelle modus controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-quick-mode)</li><li>[Afmelden controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logoff)</li><li>[Aanmelden controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-logon)</li><li>[Network Policy Server controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-network-policy-server)</li><li>[Andere gebeurtenissen voor aanmelden/afmelden controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-logonlogoff-events)</li><li>[Speciale aanmelding controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-special-logon)</li></ul>|
|Toegang tot objecten| Voert een controle uit probeert te krijgen tot specifieke objecten of typen objecten op een computer of het netwerk. Deze categorie omvat de volgende subcategorieën:<ul><li>[Gegenereerde toepassing controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-application-generated)</li><li>[Gegenereerde toepassing controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-certification-services)</li><li>[Gedetailleerde bestandsshare controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-detailed-file-share)</li><li>[Bestandsshare controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-share)</li><li>[Bestandssysteem controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-file-system)</li><li>[Filterplatform-verbinding controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection)</li><li>[Verloren gegane pakketten filteren controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-packet-drop)</li><li>[Ingangbewerking controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-handle-manipulation)</li><li>[Kernelobject controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-kernel-object)</li><li>[Andere gebeurtenissen van objecttoegang controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-object-access-events)</li><li>[Register controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-registry)</li><li>[Verwijderbare opslag controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-removable-storage)</li><li>[SAM controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sam)</li><li>[Audit centrale toegang Beleidsfasering](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-central-access-policy-staging)</li></ul>|
|Beleid wijzigen|Controles wijzigingen in belangrijke beveiligingsbeleid op een lokaal systeem of netwerk. Beleidsregels worden gemaakt door beheerders te helpen van beveiligde netwerkbronnen. Controle van wijzigingen of pogingen om te wijzigen van deze beleidsregels, is een belangrijk aspect van beveiligingsbeheer voor een netwerk. Deze categorie omvat de volgende subcategorieën:<ul><li>[Wijziging van controlebeleid controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-audit-policy-change)</li><li>[Wijziging in Authenticatiebeleid controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authentication-policy-change)</li><li>[Wijziging in autorisatiebeleid controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-authorization-policy-change)</li><li>[Wijziging in Filterplatformbeleid controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-policy-change)</li><li>[Controlebeleid MPSSVC op regelniveau wijzigen](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-mpssvc-rule-level-policy-change)</li><li>[Andere beleidswijzigingen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-policy-change-events)</li></ul>|
|Gebruik van bevoegdheden| Voert een controle uit het gebruik van bepaalde machtigingen op een of meer systemen. Deze categorie omvat de volgende subcategorieën:<ul><li>[Gebruik van niet-gevoelige bevoegdheid controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-non-sensitive-privilege-use)</li><li>[Gebruik van gevoelige bevoegdheid controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-sensitive-privilege-use)</li><li>[Het gebruik van andere bevoegdheden controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-privilege-use-events)</li></ul>|
|Systeem| Controles op systeemniveau wijzigingen naar een computer niet opgenomen in de andere categorieën en die hebben mogelijk invloed op de beveiliging. Deze categorie omvat de volgende subcategorieën:<ul><li>[IPsec-stuurprogramma controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-ipsec-driver)</li><li>[Andere systeemgebeurtenissen controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-other-system-events)</li><li>[Wijziging in beveiligingsstatus controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-state-change)</li><li>[Uitbreiding controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-security-system-extension)</li><li>[Systeemintegriteit controleren](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-system-integrity)</li></ul>|

## <a name="event-ids-per-category"></a>Gebeurtenis-id's per categorie
 Beveiligingscontrole van Azure AD Domain Services de volgende gebeurtenis-id's wordt vastgelegd wanneer de specifieke actie een controleerbare gebeurtenis wordt geactiveerd.

| De naam van de gebeurtenis-categorie | Gebeurtenis-id 's |
|:---|:---|
|Account aanmeldingsbeveiliging|4767, 4774, 4775, 4776, 4777|
|Account Management-beveiliging|4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4730, 4731, 4732, 4733, 4734, 4735, 4737, 4738, 4740, 4741, 4742, 4743, 4754, 4755, 4756, 4757, 4758, 4764, 4765, 4766, 4780, 4781, 4782, 4793, 4798, 4799, 5376, 5377|
|Details tracering beveiliging|Geen|
|Toegang tot Active Directory-beveiliging|5136, 5137, 5138, 5139, 5141|
|Aanmelden-afmelden beveiliging|4624, 4625, 4634, 4647, 4648, 4672, 4675, 4964|
|Beveiliging op object|Geen|
|Beleid wijzigen beveiliging|4670, 4703, 4704, 4705, 4706, 4707, 4713, 4715, 4716, 4717, 4718, 4719, 4739, 4864, 4865, 4866, 4867, 4904, 4906, 4911, 4912|
|Beveiliging van bevoegdheden gebruiken|4985|
|Systeembeveiliging|4612, 4621|

## <a name="enable-security-audit-events"></a>Gebeurtenissen voor beveiligingscontrole inschakelen
De volgende richtlijnen helpt u te abonneren op gebeurtenissen voor beveiligingscontrole Azure AD Domain Services.

> [!IMPORTANT]
> Beveiligingscontroles van Azure AD Domain Services zijn niet met terugwerkende kracht. Het is niet mogelijk om op te halen van gebeurtenissen in het verleden of voor de replay van gebeurtenissen uit het verleden. De service kan alleen verzenden van gebeurtenissen die plaatsvinden nadat deze is ingeschakeld.
>

### <a name="choose-the-target-resource"></a>Kies de doelresource
U kunt een willekeurige combinatie van Azure Storage, Azure Event Hubs of Azure Log Analytics-werkruimten gebruiken als een doelbron voor uw beveiligingscontroles. Houd rekening met de volgende tabel voor de beste resource voor uw situatie.

> [!IMPORTANT]
> U moet de doelresource maken voordat u beveiligingscontroles van Azure AD Domain Services inschakelt.
>

| Doelresource | Scenario |
|:---|:---|
|Azure Storage|Overweeg het gebruik van dit doel als uw primaire noodzaak is voor het opslaan van gebeurtenissen voor beveiligingscontrole voor archiveringsdoeleinden. Andere doelen kunnen worden gebruikt voor archiveringsdoeleinden; Deze doelen bieden echter mogelijkheden die niet de primaire noodzaak voor het archiveren. Voor het maken van een Azure Storage-account, gaat u als volgt [een opslagaccount maken.](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-portal#create-a-storage-account-1)|
|Azure Event Hubs|Overweeg het gebruik van dit doel als uw primaire noodzaak is voor het delen van gebeurtenissen voor beveiligingscontrole met aanvullende software zoals software voor analyse van gegevens of security information en event management (SIEM)-software. Voor het maken van een event hub, gaat u als volgt [Quick Start: Maak een event hub met behulp van Azure portal.](https://docs.microsoft.com/azure/event-hubs/event-hubs-create)|
|Azure Log Analytics Workspace|Overweeg het gebruik van dit doel als uw primaire nodig is om te analyseren en rechtstreeks bekijken van beveiligde controles van de Azure-portal.  Als u wilt een Log Analytics-werkruimte maken, gaat u als volgt [een Log Analytics-werkruimte maken in Azure portal.](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace)|

## <a name="using-the-azure-portal-to-enable-security-audit-events"></a>Met behulp van de Azure-portal om in te schakelen van gebeurtenissen voor beveiligingscontrole 
1. Meld u aan bij Azure Portal op https://portal.azure.com.  Klik op alle services in de Azure-portal. Typ in de lijst met resources **domein**. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Klik op **Azure AD Domain Services**.
2. Klik op het Azure AD Domain Services-exemplaar in de lijst.
3. Klik op **diagnostische instellingen (preview)** uit de lijst met acties aan de linkerkant.</p>
![diagnostische Instellingsactie](./media/security-audit-events/diagnostic-settings-action.png)
4. Typ de naam van de diagnostische configuratie (**aadds controle** als voorbeeld).</p>
![pagina Diagnostische instellingen](./media/security-audit-events/diagnostic-settings-page.png)
5. Selecteer het juiste selectievakje naast de bepaalde resources die u met gebeurtenissen voor beveiligingscontrole gebruikt.
    > [!NOTE]
    > U kunt geen doelresources maken vanaf deze pagina.
    >
    
    **Azure-opslag:**</p>
    Selecteer **archiveren naar een opslagaccount**. Klik op **Configureren**. Selecteer de **abonnement** en de **opslagaccount** u wilt gebruiken voor het archiveren van gebeurtenissen voor beveiligingscontrole. Klik op **OK**.</p>
    
    ![opslagaccount voor diagnostische instellingen](./media/security-audit-events/diag-settings-storage.png)
    
    **Azure eventhubs:**</p>
    Selecteer **Stream naar een event hub**. Klik op **Configureren**. In de **Selecteer event hub-pagina**, selecteer de **abonnement** gebruikt voor het maken van de event hub. Selecteer vervolgens de **event hub-naamruimte**, **event hub-naam**, en **event hub-beleidsnaam**. Klik op **OK**.</p>
    ![diagnostische event hub-instellingen](./media/security-audit-events/diag-settings-eventhub.png)
    
    **Azure Log Analytics-werkruimten:**</p>
    Selecteer **verzenden naar Log Analytics**. Selecteer de **abonnement** en **Log Analytics-werkruimte** gebruikt voor het opslaan van gebeurtenissen voor beveiligingscontrole.</p>
    ![de instellingen van diagnostische werkruimte](./media/security-audit-events/diag-settings-log-analytics.png)

6. Selecteer de gewenste opgenomen voor de specifieke doelresource logboekcategorieën. Als u met behulp van storage-accounts, kunt u beleid voor het bewaren configureren.

    > [!NOTE]
    > U kunt verschillende logboekcategorieën voor elke betreffende resource in een configuratie voor één selecteren. Hiermee kunt u kiezen welke logboeken categorieën die u wilt behouden voor Log Analytics en welke categorieën logboeken wilt archiveren.
    >

7. Klik op **opslaan** uw wijzigingen doorvoeren. De doelresources ontvangt Azure AD Domain Services-beveiligingscontrolegebeurtenissen kort nadat u uw configuratie op te slaan.

## <a name="using-azure-powershell-to-enable-security-audit-events"></a>Gebeurtenissen voor beveiligingscontrole inschakelen met behulp van Azure PowerShell
 
### <a name="prerequisites"></a>Vereisten

Volg de instructies in het artikel [installeren van de Azure PowerShell-module en maak verbinding met uw Azure-abonnement](https://docs.microsoft.com/powershell/azure/install-az-ps?toc=%2fazure%2factive-directory-domain-services%2ftoc.json).

### <a name="enable-security-audits"></a>Beveiligingscontroles inschakelen

1. Verifiëren naar de Azure Resource Manager voor de juiste tenant en -abonnement met de **Connect AzAccount** Azure PowerShell-cmdlet.
2. De doelresource van de beveiliging van gebeurtenissen voor beveiligingscontrole maken.</p>
    **Azure-opslag:**</p>
    Ga als volgt [een opslagaccount maken](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-powershell) om uw opslagaccount te maken.</p>
    **Azure eventhubs:**</p>
    Ga als volgt [Quick Start: Een event hub maken met Azure PowerShell](https://docs.microsoft.com/azure/event-hubs/event-hubs-quickstart-powershell) te maken van uw event hub. U moet mogelijk ook gebruik van de [New-AzEventHubAuthorizationRule](https://docs.microsoft.com/powershell/module/az.eventhub/new-azeventhubauthorizationrule?view=azps-2.3.2) Azure PowerShell-cmdlet voor het maken van een autorisatieregel om toe te staan van machtigingen voor Active Directory AD Domain Services naar de event hub **naamruimte**. De autorisatieregel moet bevatten de **beheren**, **luisteren**, en **verzenden** rechten.
    > [!IMPORTANT]
    > Zorg ervoor dat u de autorisatieregel instellen op de event hub-naamruimte en niet de event hub.
       
    </p>
    
    **Azure Log Analytics-werkruimten:**</p>
    Ga als volgt [een Log Analytics-werkruimte maken met Azure PowerShell](https://docs.microsoft.com/azure/azure-monitor/learn/quick-create-workspace-posh) om uw werkruimte te maken.
3. Haal de resource-ID voor uw Azure AD Domain Services-exemplaar. Typ de volgende opdracht in een geopend, geverifieerde Windows PowerShell-console. Gebruik de **$aadds. ResourceId** variabele als een parameter voor de Azure AD Domain Services-resource-ID voor toekomstige-cmdlets.
    ```powershell
    $aadds = Get-AzResource -name aaddsDomainName
    ``` 
4. Gebruik de **Set AzDiagnosticSetting** cmdlet voor het configureren van de Azure diagnostische instellingen voor het gebruik van de doelresource van gebeurtenissen voor beveiligingscontrole Azure AD Domain Services. In de voorbeelden hieronder de variabele $aadds. ResourceId vertegenwoordigt de resource-ID van uw Azure AD Domain Services-exemplaar (Zie stap 3).</p>
    **Azure-opslag:**
    ```powershell
    Set-AzDiagnosticSetting `
    -ResourceId $aadds.ResourceId` 
    -StorageAccountId storageAccountId `
    -Enabled $true
    ```
    Vervang *storageAccountId* met uw storage-account ID.</p>
    
    **Azure eventhubs:**
    ```powershell
    Set-AzDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -EventHubName eventHubName `
    -EventHubAuthorizationRuleId eventHubRuleId `
    -Enabled $true
    ```
    Vervang *eventHubName* met de naam van uw event hub. Vervang *eventHubRuleId* met uw toestemming regel-ID die u eerder hebt gemaakt.</p>
    
    **Azure Log Analytics-werkruimten:**
    ```powershell
    Set-AzureRmDiagnosticSetting -ResourceId $aadds.ResourceId ` 
    -WorkspaceID workspaceId `
    -Enabled $true
    ```
    Vervang *workspaceId* met de ID van de Log Analytics-werkruimte die u eerder hebt gemaakt. 

## <a name="view-security-audit-events-using-azure-monitor"></a>Gebeurtenissen voor beveiligingscontrole met behulp van Azure Monitor weergeven
De log Analytics-werkruimten kunnen u weergeven en analyseren van de gebeurtenissen voor beveiligingscontrole met behulp van Azure Monitor en de taal van de Kusto-query. De querytaal is ontworpen voor gebruik met alleen-lezen die analysemogelijkheden power met een syntaxis die gemakkelijk te lezen biedt.
Hier volgen enkele resources waarmee u aan de slag met Kusto-query-talen.
* [Documentatie voor Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/)
* [Aan de slag met Log Analytics in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-portal)
* [Aan de slag met Logboeken-query's in Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries)
* [Dashboards van Log Analytics-gegevens maken en delen](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-logs-dashboards)

## <a name="sample-queries"></a>Voorbeeldquery's

### <a name="sample-query-1"></a>Voorbeeldquery 1
Alle account lockout gebeurtenissen voor de afgelopen zeven dagen.
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= ago(7d)
| where OperationName has "4740"
```

### <a name="sample-query-2"></a>Voorbeeldquery 2
Alle account lockout gebeurtenissen (4740) tussen 26 juni 2019 om 9 uur en 1 juli 2019 middernacht, gesorteerd op het oplopend op de datum en tijd.
```Kusto
AADDomainServicesAccountManagement
| where TimeGenerated >= datetime(2019-06-26 09:00) and TimeGenerated <= datetime(2019-07-01) 
| where OperationName has "4740"
| sort by TimeGenerated asc
```

### <a name="sample-query-3"></a>Voorbeeldquery 3
Account log op gebeurtenissen zeven dagen geleden (vanaf nu) voor het account met de naam van gebruiker.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-4"></a>Voorbeeldquery 4
Aanmeldingsgebeurtenissen voor accounts zeven dagen geleden vanaf nu voor het account met de naam gebruiker die probeerde aan te melden met behulp van een onjuist wachtwoord (0xC0000006a).
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc000006a" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-5"></a>Voorbeeldquery 5
Aanmeldingsgebeurtenissen voor accounts zeven dagen geleden vanaf nu voor het account met de naam gebruiker die probeerde aan te melden, terwijl het account is vergrendeld (0xC0000234).
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "user" == tolower(extract("Logon Account:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
```

### <a name="sample-query-6"></a>Voorbeeldquery 6
Het aantal aanmeldingsgebeurtenissen voor accounts zeven dagen geleden vanaf nu voor alle pogingen die zijn opgetreden voor alle gebruikers vergrendeld aanmelden.
```Kusto
AADDomainServicesAccountLogon
| where TimeGenerated >= ago(7d)
| where "0xc0000234" == tolower(extract("Error Code:\t(.+[0-9A-Za-z])",1,tostring(ResultDescription)))
| summarize count()
```

## <a name="related-content"></a>Gerelateerde inhoud
* [Overzicht](https://docs.microsoft.com/azure/kusto/query/) van de Kusto-querytaal.
* [Kusto-zelfstudie](https://docs.microsoft.com/azure/kusto/query/tutorial) naar raakt u vertrouwd met de grondbeginselen van de query.
* [Voorbeeld van query's](https://docs.microsoft.com/azure/kusto/query/samples) waarmee u informatie over nieuwe manieren om uw gegevens te bekijken.
* Kusto [aanbevolen procedures](https://docs.microsoft.com/azure/kusto/query/best-practices) – optimaliseren van uw query's voor succes.














 

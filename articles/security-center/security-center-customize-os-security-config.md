---
title: Aanpassen OS beveiligingsconfiguraties in Azure Security Center (Preview) | Microsoft Docs
description: In dit artikel laat zien hoe security center-evaluaties aanpassen
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/16/2018
ms.author: terrylan
ms.openlocfilehash: d42dd4ba150a28109d6bb3f7c2281d07b21a366e
ms.sourcegitcommit: 79683e67911c3ab14bcae668f7551e57f3095425
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/25/2018
---
# <a name="customize-os-security-configurations-in-azure-security-center-preview"></a>OS-beveiligingsconfiguraties in Azure Security Center (Preview) aanpassen

In dit scenario laat zien hoe OS beoordelingen van configuratie van de veiligheid in Azure Security Center aanpassen.

## <a name="what-are-os-security-configurations"></a>Wat zijn de OS-beveiligingsconfiguraties?

Azure Security Center monitors beveiligingsconfiguraties door middel van een set van meer dan 150 aanbevolen regels voor het beperken van het besturingssysteem, met inbegrip van regels betrekking heeft op firewalls, controle, wachtwoordbeleid en meer. Als een machine een kwetsbare configuratie wordt gevonden, wordt een beveiligingsaanbeveling gegenereerd in Security Center.

Aanpassen van de regels kunnen organisaties bepalen welke opties zijn geschikter zijn voor hun omgeving. U kunt aangepaste assessment beleid instellen en deze vervolgens toe te passen op alle betreffende computers in het abonnement.

> [!NOTE]
> - Aanpassing van de configuratie van de OS-beveiliging is momenteel beschikbaar voor Windows Server 2008, 2008 R2, 2012 en 2012 R2-versies alleen besturingssystemen.
> - De configuratie van toepassing op alle virtuele machines en computers die zijn verbonden met alle werkruimten onder het geselecteerde abonnement.
> - Aanpassing van het besturingssysteem beveiliging-configuratie is alleen beschikbaar op de standaardcategorie Security Center.
>
>

U kunt de OS-beveiligingsregels configuratie aanpassen door inschakelen en uitschakelen van een specifieke regel, de gewenste instelling voor een bestaande regel te wijzigen of een nieuwe regel die gebaseerd op de ondersteunde regeltypen (register, controlebeleid en beveiligingsbeleid) toe te voegen. Op dit moment moet de gewenste instelling een exacte waarde.

Nieuwe regels moet de indeling en de structuur als andere bestaande regels van hetzelfde type.

> [!NOTE]
> Voor het besturingssysteem beveiligingsconfiguraties aanpassen, moet u zijn toegewezen de rol van *eigenaar abonnement*, *abonnement Inzender*, of *beveiligingsbeheerder*.
>
>

## <a name="customize-the-default-os-security-configuration"></a>De beveiligingsconfiguratie van de standaard OS aanpassen

Voor het aanpassen van de standaard OS beveiligingsconfiguratie in Security Center het volgende doen:

1.  Open het dashboard van **Security Center**.

2.  Selecteer in het linkerdeelvenster **beveiligingsbeleid**.  
    De **Security Center - beveiligingsbeleid** venster wordt geopend.

    ![Lijst met beveiligingsbeleid](media/security-center-customize-os-security-config/open-security-policy.png)

3.  Selecteer het abonnement dat u wilt uitvoeren voor aanpassing.

4. Onder **beleid onderdelen**, selecteer **beveiligingsconfiguraties bewerken**.  
    De **beveiligingsconfiguraties bewerken** venster wordt geopend. 
    
    ![Het venster 'Bewerken beveiligingsconfiguraties'](media/security-center-customize-os-security-config/blade.png)

5. Volg de stappen voor het downloaden, bewerken en het gewijzigde bestand uploadt in het rechterdeelvenster.

   > [!NOTE]
   > Het configuratiebestand dat u hebt gedownload is standaard *json* indeling. Voor instructies over het wijzigen van dit bestand, gaat u naar [het configuratiebestand aanpassen](#customize-the-configuration-file).
   >

   Nadat u het bestand hebt opgeslagen, wordt de configuratie wordt toegepast op alle virtuele machines en computers die zijn verbonden met alle werkruimten onder het abonnement. Het proces wordt doorgaans duurt een paar minuten maar kan, afhankelijk van de grootte van de infrastructuur langer duren. 

6. Selecteer om de wijziging doorvoert, **opslaan**. Anders wordt wordt het beleid niet opgeslagen.

    ![De knop Opslaan](media/security-center-customize-os-security-config/save-successfully.png)

Op elk gewenst moment kunt u de huidige configuratie van het beleid opnieuw instellen op de standaardstatus. Om dit te doen in de **bewerken OS-beveiligingsregels configuratie** Selecteer **opnieuw**. Deze optie als u selecteert bevestigen **Ja** in het pop-upvenster bevestiging.

![Het bevestigingsvenster opnieuw instellen](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Het configuratiebestand aanpassen

In het aanpassingsbestand heeft elke ondersteunde versie van het besturingssysteem een reeks regels of ruleset. Elke ruleset heeft een eigen naam en de unieke ID, zoals aangegeven in het volgende voorbeeld:

![De naam van de ruleset en ID](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> In dit voorbeeld van een bestand is bewerkt in Visual Studio, maar u kunt ook Kladblok gebruiken als u de JSON-Viewer invoegtoepassing geïnstalleerd hebt.
>
>

Wanneer u het aanpassingsbestand bewerkt, kunt u een regel of alle mappen wijzigen. Elke ruleset bevat een *regels* sectie die onderverdeeld in drie categorieën: register, controlebeleid en beveiligingsbeleid, zoals hier wordt weergegeven:

![Drie ruleset-categorieën](media/security-center-customize-os-security-config/rules-section.png)

Elke categorie heeft een eigen set kenmerken. U kunt de volgende kenmerken wijzigen:

- **expectedValue**: het gegevenstype veld van dit kenmerk moet overeenkomen met de ondersteunde waarden per *regeltype*, bijvoorbeeld:

  - **baselineRegistryRules**: de waarde moet overeenkomen met de [regValueType](https://msdn.microsoft.com/library/windows/desktop/ms724884) die is gedefinieerd in die regel.

  - **baselineAuditPolicyRules**: gebruik een van de volgende tekenreekswaarden:

    - *Geslaagd en mislukt*

    - *Geslaagd*

  - **baselineSecurityPolicyRules**: gebruik een van de volgende tekenreekswaarden:

    - *Niemand*

    - Lijst met toegestane gebruikersgroepen, bijvoorbeeld: *beheerders*, *back-upoperators*

-   **status**: de tekenreeks kan de opties bevatten *uitgeschakelde* of *ingeschakeld*. De tekenreeks is hoofdlettergevoelig voor deze private preview-versie.

Dit zijn alleen de velden die kunnen worden geconfigureerd. Als u de bestandsindeling of grootte overtreedt, niet mogelijk de wijziging op te slaan. Het volgende foutbericht treedt op wanneer het bestand kan niet worden verwerkt:

![Foutbericht beveiliging-configuratie](media/security-center-customize-os-security-config/invalid-json.png)

Zie voor een lijst met andere potentiële fouten, [foutcodes](#error-codes).

De volgende drie secties bevatten voorbeelden van de voorgaande regels. De *expectedValue* en *status* kenmerken kunnen worden gewijzigd.

**baselineRegistryRules**
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

    }
```

**baselineAuditPolicyRules**
```
    {
    "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
    "ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
    "originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
    "cceId": "CCE-11001-5",
    "ruleName": "Audit Policy: Account Management: Other Account Management Events",
    "ruleType": "AuditPolicy",
    "expectedValue": "Success and Failure",
    "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

**baselineSecurityPolicyRules**
```
    {
    "sectionName": "Privilege Rights",
    "settingName": "SeIncreaseWorkingSetPrivilege",
    "ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
    "originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
    "cceId": "CCE-10548-6",
    "ruleName": "Increase a process working set",
    "ruleType": "SecurityPolicy",
    "expectedValue": "Administrators, Local Service",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

Sommige regels worden voor de verschillende OS-typen gedupliceerd. Dubbele regels hebben dezelfde *originalId* kenmerk.

## <a name="create-custom-rules"></a>Aangepaste regels maken

U kunt ook nieuwe regels maken. Voordat u een nieuwe regel maken, moet u rekening met de volgende beperkingen:

-   Schemaversie *baselineId* en *baselineName* kan niet worden gewijzigd.

-   RuleSet kan niet worden verwijderd.

-   RuleSet kan niet worden toegevoegd.

-   Het maximum aantal regels dat is toegestaan (inclusief standaardregels), bedraagt 1000.

Nieuwe aangepaste regels zijn gemarkeerd met een nieuwe aangepaste bron (! = 'Microsoft'). De *ruleId* veld kan niet null of leeg. Als deze leeg is, genereert Microsoft een. Als dit niet leeg is, moet een geldige GUID die is uniek voor alle regels (standaard en aangepaste) hebben. Controleer de volgende beperkingen voor de core velden:

-   **originalId**: kan niet null of leeg zijn. Als *originalId* is niet leeg is, moet een geldige GUID.

-   **cceId**: kan niet null of leeg zijn. Als *cceId* is niet leeg is, wordt deze moet uniek zijn.

-   **ruleType**: (Selecteer een) register, AuditPolicy of SecurityPolicy.

-   **Ernst**: (Selecteer een) onbekend, kritiek, waarschuwing of ter informatie.

-   **analyzeOperation**: moet *gelijk is aan*.

-   **auditPolicyId**: moet een geldige GUID.

-   **regValueType**: (Selecteer een) Int, Long, tekenreeks- of MultipleString.

> [!NOTE]
> Hive moet *LocalMachine*.
>
>

Voorbeeld van een nieuwe aangepaste regel:

**Register**:
```
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule”, "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
    }
```
**Beveiligingsbeleid voor**:
```
   {
   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Controlebeleid**:
```
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Mislukte uploaden

Als het opgegeven configuratiebestand ongeldig vanwege fouten in waarden of opmaak is, wordt een fout weergegeven. U kunt een rapport van de CSV gedetailleerde fouten te herstellen en corrigeer de fouten voordat u een gecorrigeerde configuratiebestand opnieuw indienen downloaden.

![Foutbericht "opslagactie is mislukt"](media/security-center-customize-os-security-config/invalid-configuration.png)

Voorbeeld van een foutbestand met:

![Voorbeeld van een fout](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Foutcodes

Alle potentiële fouten worden vermeld in de volgende tabel:

| **Error**                                | **Beschrijving**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | De eigenschap *schemaVersion* blijkt ongeldig of leeg. De waarde moet worden ingesteld op *{0}*.                                                         |
| BaselineInvalidStringError               | De eigenschap *{0}* mag niet  *\\ n* .                                                                                                         |
| BaselineNullRuleError                    | De lijst met regels configuration baseline bevat een regel met waarde *null*.                                                                         |
| BaselineRuleCceIdNotUniqueError          | De ID van de CCE *{0}* is niet uniek.                                                                                                                  |
| BaselineRuleEmptyProperty                | De eigenschap *{0}* ontbreekt of is ongeldig.                                                                                                       |
| BaselineRuleIdNotInDefault               | De regel bevat een broneigenschap *Microsoft* maar het is niet gevonden in de Microsoft-standaard ruleset.                                                   |
| BaselineRuleIdNotUniqueError             | De regel-Id is niet uniek.                                                                                                                       |
| BaselineRuleInvalidGuid                  | De eigenschap *{0}* ongeldig is gevonden. De waarde is geen geldige GUID.                                                                             |
| BaselineRuleInvalidHive                  | De component moet LocalMachine.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Naam van de regel is niet uniek.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | De regel is niet gevonden in de nieuwe configuratie. Regel kan niet worden verwijderd.                                                                     |
| BaselineRuleNotFoundError                | De regel is niet gevonden in de ruleset basislijn.                                                                                        |
| BaselineRuleNotInPlace                   | De regel overeenkomt met een standaardregel met type {0} en wordt vermeld in de lijst {1}.                                                                       |
| BaselineRulePropertyTooLong              | De eigenschap *{0}* is te lang. Maximaal toegestane lengte: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | De verwachte waarde *{0}* komt niet overeen met het registerwaardetype die is gedefinieerd.                                                              |
| BaselineRulesetAdded                     | Het ruleset met de ID *{0}* is niet gevonden in de standaardconfiguratie. Het ruleset kan niet worden toegevoegd.                                               |
| BaselineRulesetIdMustBeUnique            | De basislijn ruleset *{0}* moeten uniek zijn.                                                                                           |
| BaselineRulesetNotFound                  | Het ruleset met id *{0}* en de naam *{1}* is niet gevonden in de opgegeven configuratie. Het ruleset kan niet worden verwijderd.                                |
| BaselineRuleSourceNotMatch               | De regel met ID *{0}* is al gedefinieerd.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Het standaardtype voor de regel is *{0}*.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Het type van de regel *{0}*, maar de *ruleType* eigenschap is *{1}*.                                                                          |
| BaselineRuleUnpermittedChangesError      | Alleen *expectedValue* en *status* eigenschappen kunnen worden gewijzigd.                                                                       |
| BaselineTooManyRules                     | Het maximum aantal toegestane aangepaste regels is {0} regels. De opgegeven configuratie bevat regels {1}, {2} standaardregels en {3} aangepaste regels. |
| ErrorNoConfigurationStatus               | Er is geen configuratie-status is gevonden. De status van de gewenste status: *standaard* of *aangepaste*.                                    |
| ErrorNonEmptyRulesetOnDefault            | De status van de configuratie is ingesteld op standaard. De *BaselineRulesets* lijst moet null of leeg.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | De status van het gegeven configuratie *aangepaste* maar de *baselineRulesets* eigenschap is null of leeg.                                             |
| ErrorParsingBaselineConfig               | De opgegeven configuratie is ongeldig. Een of meer van de gedefinieerde waarden hebben een null-waarde of een ongeldig type.                                  |
| ErrorParsingIsDefaultProperty            | De opgegeven *configurationStatus* waarde *{0}* is ongeldig. De waarde mag alleen *standaard* of *aangepaste*.                                         |
| InCompatibleViewVersion                  | De versie van de weergave *{0}* is *niet* ondersteund op dit type werkruimte.                                                                                   |
| InvalidBaselineConfigurationGeneralError | De configuratie van de basislijn is gevonden met een of meer validatiefouten van het type.                                                          |
| ViewConversionError                      | De weergave is een oudere versie dan de werkruimte ondersteunt. Weergeven van de conversie is mislukt: {0}.                                                                 |

Als u niet voldoende machtigingen hebt, krijgt u mogelijk een algemene fout is opgetreden, zoals hier wordt weergegeven:

![Foutbericht "opslagactie is mislukt"](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel besproken voor het aanpassen van OS beoordelingen van configuratie van de veiligheid in Security Center. Zie voor meer informatie over de van configuratieregels en herstel:

- [Security Center algemene configuratie-ID's en de basislijnregels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Security Center gebruikt common configuration enumeration (CCE) om de unieke id's toewijzen aan configuratieregels. Zie voor meer informatie [CCE](https://nvd.nist.gov/config/cce/index).
- Raadpleeg beveiligingsproblemen oplossen wanneer de configuratie van uw besturingssysteem komt niet overeen met de aanbevolen configuratie van de beveiligingsregels [herstellen beveiligingsconfiguraties](security-center-remediate-os-vulnerabilities.md).

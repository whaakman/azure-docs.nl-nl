---
title: OS-beveiligingsconfiguraties in Azure Security Center [Preview] aanpassen | Microsoft Docs
description: In dit artikel leert u hoe security center-evaluaties aanpassen
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
ms.date: 01/08/2018
ms.author: terrylan
ms.openlocfilehash: 2fa63515d290e6700fbe4a90ae509f4635b19f29
ms.sourcegitcommit: 719dd33d18cc25c719572cd67e4e6bce29b1d6e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2018
---
# <a name="customizing-os-security-configurations-in-azure-security-center-preview"></a>OS-beveiligingsconfiguraties in Azure Security Center [Preview] aanpassen

Informatie over het aanpassen van de beveiligingsconfiguratie OS beoordelingen in Azure Security Center gebruikt deze stapsgewijze kennismaking.

## <a name="what-are-os-security-configurations"></a>Wat zijn de OS-beveiligingsconfiguraties?

Azure Security Center met behulp van een set van meer dan 150 aanbevolen regels voor de beperking van het besturingssysteem monitors beveiligingsconfiguraties, waaronder regels in verband met firewalls, controle, wachtwoordbeleid en meer. Als een machine een kwetsbare configuratie wordt gevonden, wordt een beveiligingsaanbeveling gegenereerd.

Aanpassing van de regels kunt organisaties om te bepalen welke opties geschikter zijn voor hun omgeving zijn. Deze functie kan gebruikers een aangepaste assessment beleid ingesteld en dit toepassen op alle betreffende computers in het abonnement.

> [!NOTE]
> - OS-beveiligingsconfiguratie aanpassing is momenteel beschikbaar voor Windows Server 2008 en 2008 R2, 2012 alleen 2012R2-besturingssystemen.
- De configuratie van toepassing op alle virtuele machines en computers die zijn verbonden met alle werkruimten onder het geselecteerde abonnement.
- Beveiligingsconfiguratie OS aanpassing is alleen beschikbaar in Standard-laag van Security Center.
>
>

Het aanpassen van de beveiligingsconfiguratie OS regels?

U kunt de regels van configuratie van het besturingssysteem beveiliging aanpassen door inschakelen en uitschakelen van een specifieke regel, de gewenste instelling voor een bestaande regel te wijzigen en een nieuwe regel op basis van de ondersteunde regeltypen (register, controlebeleid en beveiligingsbeleid) toe te voegen. Op dit moment moet de gewenste instelling een exacte waarde.

Nieuwe regels hebben niet in de indeling en de structuur als andere bestaande regels van hetzelfde type.

> [!NOTE]
> Voor het besturingssysteem beveiligingsconfiguraties aanpassen, moet u de rol van eigenaar abonnement, abonnement bijdrager of beveiligingsbeheerder worden toegewezen.
>
>

## <a name="customize-security-configuration"></a>Beveiligingsconfiguratie aanpassen

Om aan te passen de standaard OS beveiligingsconfiguratie in Security Center:

1.  Open het dashboard **Security Center**.

2.  Selecteer onder het hoofdmenu Security Center **beveiligingsbeleid**.  **Security Center - beveiligingsbeleid** wordt geopend.

3.  Selecteer het abonnement dat u wilt uitvoeren voor aanpassing.

    ![](media/security-center-customize-os-security-config/open-security-policy.png)

4. Onder **beleid onderdelen**, selecteer **beveiligingsconfiguraties bewerken**.

6.  **Bewerken beveiligingsconfiguraties** wordt geopend. Volg de stappen in het scherm om te downloaden, bewerken en upload het gewijzigde bestand gemarkeerd.

    ![](media/security-center-customize-os-security-config/blade.png)

  > [!NOTE]
  > Het configuratiebestand dat u hebt gedownload is standaard *json* indeling. Voor instructies over hoe u dit bestand wijzigt, gaat u naar [het configuratiebestand aanpassen](#customize-the-configuration-file).
  >

7. Na het opslaan van het bestand is, de configuratie wordt toegepast op alle virtuele machines en computers die zijn verbonden met alle workspace(s) onder het geselecteerde abonnement. Dit proces kan enige tijd, meestal een paar minuten duren, maar kan duurt het langer omdat deze afhankelijk van de grootte van de infrastructuur is. Selecteer **opslaan** om de wijziging doorvoert, anders het beleid wordt niet opgeslagen.

    ![](media/security-center-customize-os-security-config/save-successfully.png)

Op elk gewenst moment kunt u de configuratie van het huidige beleid terugzetten op de standaardstatus voor het beleid door het selecteren van de **opnieuw** optie in **OS beveiligingsconfiguratie bewerken regels**. Als u deze optie kiest, ontvangt u de volgende pop-waarschuwing. Selecteer **Ja** om te bevestigen.

![](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Het configuratiebestand aanpassen

In het aanpassingsbestand heeft elke ondersteunde versie van het besturingssysteem een reeks regels (ruleset). Elke verzameling regels heeft een eigen naam en de unieke ID, zoals aangegeven in het volgende voorbeeld:

![](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Dit bestand is bewerkt met Visual Studio, maar u kunt ook Kladblok gebruiken, zolang u de JSON-Viewer-invoegtoepassing geïnstalleerd hebt.
>
>

Wanneer u dit bestand bewerkt, kunt u een regel of alle mappen wijzigen. Elke ruleset bevat een *regels* sectie die de regels, onderverdeeld in 3 soorten regels bevat: register, controlebeleid en beveiligingsbeleid, zoals u hierna ziet:

![](media/security-center-customize-os-security-config/rules-section.png)

Elke categorie heeft een eigen set kenmerken. Voor bestaande regels kunt u wijzigingen aanbrengen in de volgende waarden:

- expectedValue: het gegevenstype van dit kenmerk veld moet overeenkomen met de ondersteunde waarden per elk Type regel, bijvoorbeeld:

  - baselineRegistryRules: de waarde moet overeenkomen met de [regValueType] (https://msdn.microsoft.com/library/windows/desktop/ms724884 (v=vs.85) gedefinieerd in die regel.

  - baselineAuditPolicyRules: de waarde moet een tekenreekswaarde, een van de volgende:

    - Geslaagd en mislukt

    - Geslaagd

  - baselineSecurityPolicyRules: de waarde moet een tekenreekswaarde, een van de volgende:

    - "Een"

    - Lijst met toegestane gebruikersgroepen, bijvoorbeeld: "Beheerders, back-upoperators"

-   status: tekenreeks met de opties 'Uitgeschakeld' of 'Enabled'. De tekenreeks is hoofdlettergevoelig voor deze private preview-versie.

Dit zijn alleen de velden die kunnen worden geconfigureerd. Als u de bestandsindeling of grootte overtreedt, niet mogelijk de wijziging op te slaan. Het volgende foutbericht treedt op wanneer het bestand kan niet worden verwerkt:

![](media/security-center-customize-os-security-config/invalid-json.png)

Zie [foutcodes](#error-codes) voor lijst van mogelijke fouten.

Hieronder hebt u voorbeelden van deze regels en de kenmerken die kunnen worden gewijzigd (in vet weergegeven):

**Sectie regels:** baselineRegistryRules
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
    "**expectedValue**": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "**state**": "Disabled"

}
```

**Sectie regels:** baselineAuditPolicyRules
```
{
"auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
"ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
"originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
"cceId": "CCE-11001-5",
"ruleName": "Audit Policy: Account Management: Other Account Management Events",
"ruleType": "AuditPolicy",
"**expectedValue**": "Success and Failure",
"severity": "Critical",
"analyzeOperation": "Equals",
"source": "Microsoft",
"**state**": "Enabled"
},
```

**Regels secties:** baselineSecurityPolicyRules
```
{
"sectionName": "Privilege Rights",
"settingName": "SeIncreaseWorkingSetPrivilege",
"ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
"originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
"cceId": "CCE-10548-6",
"ruleName": "Increase a process working set",
"ruleType": "SecurityPolicy",
"**expectedValue**": "Administrators, Local Service",
"severity": "Warning",
"analyzeOperation": "Equals",
"source": "Microsoft", "**state**": "Enabled"
},
```

Er zijn bepaalde regels die voor de verschillende OS-typen worden gedupliceerd. Dubbele regels hebben de dezelfde originalId.

## <a name="adding-a-new-custom-rule"></a>Toevoegen van een nieuwe aangepaste regel

U kunt ook een nieuwe regel maken. Voordat u een nieuwe regel maakt, houd rekening met de volgende beperkingen:

-   Schemaversie *baselineId* en *baselineName* kan niet worden gewijzigd.

-   RuleSet kan niet worden verwijderd.

-   RuleSet kan niet worden toegevoegd.

-   Het maximum aantal regels dat is toegestaan (inclusief standaardregels) is 1000 regels.

Nieuwe aangepaste regels zijn gemarkeerd met een nieuwe aangepaste bron (! = 'Microsoft'). De *ruleId* veld mag niet null of leeg. Als deze leeg is, genereert Microsoft een. Als dit niet leeg is, moet het een geldige unieke GUID voor alle regels (standaard en aangepaste) hebben. Controleer de beperkingen hieronder met betrekking tot de belangrijkste velden:

-   *originalId* -mag niet null of leeg zijn. Als *originalId* is niet leeg is, moet een geldige GUID.

-   *cceId* -mag niet null of leeg zijn. Als *cceId* is niet leeg is, wordt deze moet uniek zijn.

-   *ruleType* : een van: register, AuditPolicy of SecurityPolicy.

-   *Ernst* : een van: onbekend, kritiek, waarschuwing of ter informatie.

-   *analyzeOperation* -moet gelijk aan.

-   *auditPolicyId* -moet geldige GUID.

-   *regValueType* -moet een van: Int, Long, tekenreeks- of MultipleString.

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
**Controlebeleid:**
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

Als het opgegeven configuratiebestand ongeldig vanwege fouten in waarden of opmaak is, wordt een fout weergegeven. U kunt een rapport van de CSV-gedetailleerde fouten te herstellen en corrigeer de fouten voordat u het opnieuw probeert een gecorrigeerde configuratiebestand downloaden.

![](media/security-center-customize-os-security-config/invalid-configuration.png)

Voorbeeld van het bestand fouten:

![](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Foutcodes

De onderstaande lijst bevat alle mogelijke fouten:

| **Fout**                                | **Beschrijving**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfiguratiohSchemaVersionError  | Eigenschap 'schemaVersion' is ongeldig of leeg gevonden. De waarde moet worden ingesteld op '{0}'.                                                         |
| BaselineInvalidStringError               | De eigenschap '{0}' mag niet '\\n ".                                                                                                         |
| BaselineNullRuleError                    | Basislijn configuration regels lijst bevat een regel met de waarde 'null'.                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE-ID '{0}' is niet uniek.                                                                                                                  |
| BaselineRuleEmptyProperty                | Eigenschap: {0} is ontbreekt of is ongeldig.                                                                                                       |
| BaselineRuleIdNotInDefault               | De regel heeft een broneigenschap 'Microsoft', maar is niet gevonden in Microsoft standaard ruleset.                                                   |
| BaselineRuleIdNotUniqueError             | Regel-Id is niet uniek.                                                                                                                       |
| BaselineRuleInvalidGuid                  | Eigenschap {0} is ongeldig gevonden. De waarde is geen geldige Guid.                                                                             |
| BaselineRuleInvalidHive                  | Hive moet LocalMachine.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Naam van de regel is niet uniek.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | De regel is niet gevonden in de nieuwe configuratie. Regel kan niet worden verwijderd.                                                                     |
| BaselineRuleNotFoundError                | De regel is niet gevonden in set van basislijn-regels.                                                                                        |
| BaselineRuleNotInPlace                   | De regel overeenkomt met een standaardregel met type {0} en wordt vermeld in de lijst {1}.                                                                       |
| BaselineRulePropertyTooLong              | Eigenschap: {0} is te lang. Maximaal toegestane lengte: {1}.                                                                                        |
| BaselineRuleRegTypeInvalidError          | De verwachte waarde '{0}' komt niet overeen met het registerwaardetype die is gedefinieerd.                                                              |
| BaselineRulesetAdded                     | De set regels met id '{0}' is niet gevonden in de standaardconfiguratie. Regels set kan niet worden toegevoegd.                                               |
| BaselineRulesetIdMustBeUnique            | De basislijn regels set '{0}' moet uniek zijn.                                                                                           |
| BaselineRulesetNotFound                  | Regels die zijn ingesteld met id {0} en naam '{1}' is niet gevonden in de opgegeven configuratie. Regels set kan niet worden verwijderd.                                |
| BaselineRuleSourceNotMatch               | Regel met id '{0}' is al gedefinieerd.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Het standaardtype voor de regel is {0}.                                                                                                              |
| BaselineRuleTypeDoesntMatchError         | Het type van de regel: {0}, maar ruleType eigenschap is: {1}.                                                                          |
| BaselineRuleUnpermittedChangesError      | Alleen 'expectedValue' en 'status' eigenschappen kunnen worden gewijzigd.                                                                       |
| BaselineTooManyRules                     | Maximaal toegestane aantal aangepaste regels is {0} regels. De opgegeven configuratie bevat {1} regels. (de standaardregels {2} + {3} aangepaste regels. |
| ErrorNoConfigurationStatus               | Er is geen configuratie-status is gevonden. Gelieve de status van de gewenste configuratie - 'Default' of 'Custom'.                                    |
| ErrorNonEmptyRulesetOnDefault            | Status van de configuratie is ingesteld op standaard. BaselineRulesets lijst moet null of leeg zijn.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | De configuratiestatus van de opgegeven is 'Custom', maar de eigenschap 'baselineRulesets' is null of leeg.                                             |
| ErrorParsingBaselineConfig               | De opgegeven configuratie is ongeldig. Een of meer van de waarden die zijn gedefinieerd hebben een null-waarde of een ongeldig type.                                  |
| ErrorParsingIsDefaultProperty            | De waarde van de opgegeven configurationStatus {0} is ongeldig. De waarde mag alleen 'Default' of 'Custom'.                                         |
| InCompatibleViewVersion                  | Versie weergeven: {0} wordt niet ondersteund op dit type werkruimte.                                                                                   |
| InvalidBaselineConfigurationGeneralError | De configuratie van de basislijn is gevonden met een of meer validatiefouten van het type.                                                          |
| ViewConversionError                      | De weergave is een oudere versie die werkruimte ondersteund. Weergeven van de conversie is mislukt: {0}                                                                 |

Als u niet voldoende machtigingen hebt, krijgt u mogelijk een algemene fout opgetreden:

![](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Volgende stappen
In dit artikel hebt u geleerd u het aanpassen van de beveiligingsconfiguratie OS beoordelingen in Security Center. Zie voor meer informatie over de van configuratieregels en herstel:

- [Security Center algemene configuratie-ID's en de Basislijnregels](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Security Center gebruikt CCE (algemene configuratie opsomming) om de unieke id's voor configuratieregels toewijzen. Ga naar de [CCE](https://nvd.nist.gov/config/cce/index) website voor meer informatie.
- [Herstellen van beveiligingsconfiguraties](security-center-remediate-os-vulnerabilities.md) ziet u hoe beveiligingsproblemen oplossen wanneer de configuratie van uw besturingssysteem komt niet overeen met de aanbevolen configuratie van de beveiligingsregels.

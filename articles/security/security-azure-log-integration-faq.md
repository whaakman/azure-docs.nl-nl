---
title: Veelgestelde vragen over Azure-logboekanalyse-integratie | Microsoft Docs
description: In dit artikel antwoorden op vragen over Azure Log-integratie.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 08/07/2017
ms.author: TomSh
ms.custom: azlog
ms.openlocfilehash: bfdc7154160bb6bb7dc9c46eb2352ce74310c4de
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-log-integration-faq"></a>Veelgestelde vragen over Azure-logboekanalyse-integratie
Dit artikel worden veelgestelde vragen (FAQ) over de integratie van Azure Log. 

Integratie van Azure Log is een service voor het besturingssysteem van Windows die u gebruiken kunt voor het integreren van onbewerkte logboeken van uw Azure-resources in uw on-premises security information en event management (SIEM) systemen. Deze integratie biedt een uniforme dashboard voor alle activa, lokaal of in de cloud. U kunt vervolgens samenvoegen, correleren, analyseren en waarschuwen voor beveiligingsgebeurtenissen die zijn gekoppeld aan uw toepassingen.

## <a name="is-the-azure-log-integration-software-free"></a>Is de software-integratie van Azure Log gratis?
Ja. Er zijn geen kosten voor de integratie van Azure Log-software.

## <a name="where-is-azure-log-integration-available"></a>Waar Azure Log integratie beschikbaar is?

Het is momenteel beschikbaar zijn in Azure commerciële en Azure Government en is niet beschikbaar in China of Duitsland.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Hoe kan ik de waarin Azure Log-integratie is binnenhalen van Logboeken van de virtuele machine van Azure storage-accounts zien?
Voer de opdracht **azlog bronlijst**.

## <a name="how-can-i-tell-which-subscription-the-azure-log-integration-logs-are-from"></a>Hoe weet ik welke abonnement afkomstig van de logboeken van de integratie van Azure-logboek zijn

In het geval van controlelogboeken die worden geplaatst in de **AzureResourcemanagerJson** mappen, het abonnement-ID de naam van het logboek wordt. Dit geldt ook voor logboeken in de **AzureSecurityCenterJson** map. Bijvoorbeeld:

20170407T070805_2768037.0000000023. **1111e5ee-1111-111b-a11e-1e111e1111dc**.json

Auditlogboeken van Azure Active Directory opnemen als onderdeel van de naam van de tenant-ID.

De abonnements-ID als onderdeel van de naam van de opnemen logboeken met diagnostische gegevens die worden vanuit een event hub gelezen niet. Ze bevatten in plaats daarvan de beschrijvende naam die is opgegeven als onderdeel van het maken van de event hub-bron. 

## <a name="how-can-i-update-the-proxy-configuration"></a>Hoe kan ik de proxyconfiguratie bijwerken?
Als de proxy-instellingen kan geen toegang tot Azure-opslag rechtstreeks, opent u de **AZLOG. EXE. CONFIG** bestanden per **c:\Program Files\Microsoft Azure Log integratie**. Bijwerken van het bestand om op te nemen de **defaultProxy** sectie met de proxy-adres van uw organisatie. Nadat de update is voltooid, stoppen en starten van de service met de opdrachten **net stop azlog** en **net start azlog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress=http://127.0.0.1:8888
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Hoe kan ik informatie over het abonnement in de Windows-gebeurtenissen weergeven?
De abonnements-ID toevoegen aan de beschrijvende naam tijdens het toevoegen van de bron:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
De gebeurtenis XML heeft de volgende metagegevens, met inbegrip van de abonnements-ID:

![XML-gebeurtenis][1]

## <a name="error-messages"></a>Foutberichten
### <a name="when-i-run-the-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Wanneer ik de opdracht uitvoert **azlog createazureid**, waarom wordt het volgende foutbericht?
Fout:

  *72f988bf-86f1-41af-91ab-2d7cd011db37-reden is mislukt voor het maken van de toepassing van de AAD - Tenant = 'Verboden' - bericht = 'Onvoldoende bevoegdheden om de bewerking te voltooien.'*

De **azlog createazureid** opdracht probeert te maken van een service-principal in de Azure AD-tenants voor de abonnementen die toegang tot de Azure-aanmelding heeft. Als uw Azure-aanmelding alleen een gastgebruiker in die Azure AD-tenant is, mislukt de opdracht met "Onvoldoende bevoegdheden om de bewerking te voltooien." Vraag de tenantbeheerder aan uw account toevoegen als een gebruiker in de tenant.

### <a name="when-i-run-the-command-azlog-authorize-why-do-i-get-the-following-error"></a>Wanneer ik de opdracht uitvoert **azlog autoriseren**, waarom wordt het volgende foutbericht?
Fout:

  *Maken van de roltoewijzing - AuthorizationFailed waarschuwing: de client janedo@microsoft.com' met object-id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' geen autorisatie voor het uitvoeren van de actie 'Microsoft.Authorization/roleAssignments/write' bereik ' / abonnementen, 70-d 95299-d689-4c 97 b971 0d8ff0000000'.*

De **azlog autoriseren** opdracht wijst de rol van lezer toe aan de Azure AD-service-principal (gemaakt met **azlog createazureid**) aan de opgegeven abonnementen. Als de Azure-aanmelding niet een CO-beheerder of een eigenaar van het abonnement is, mislukt dit met een foutbericht 'Autorisatie is mislukt'. Azure op rollen gebaseerde toegangsbeheer (RBAC) van CO-beheerder of de eigenaar is nodig om deze actie te voltooien.

## <a name="where-can-i-find-the-definition-of-the-properties-in-the-audit-log"></a>Waar vind ik de definitie van de eigenschappen in het controlelogboek
Zie:

* [Bewerkingen met Azure Resource Manager controleren](../azure-resource-manager/resource-group-audit.md)
* [Lijst van de management-gebeurtenissen in een abonnement in de REST-API van de Azure-Monitor](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Waar vind ik meer informatie over Azure Security Center alerts
Zie [beheren en erop reageren beveiligingswaarschuwingen in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hoe kan ik wijzigen wat met VM diagnostische gegevens worden verzameld?
Zie voor meer informatie over het ophalen, wijzigen en stel de configuratie van de Azure Diagnostics [Gebruik PowerShell voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Het volgende voorbeeld wordt de configuratie van Azure Diagnostics:

    -AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

Het volgende voorbeeld wijzigt u de configuratie van Azure Diagnostics. In deze configuratie worden alleen gebeurtenis-ID 4624 en gebeurtenis-ID 4625 verzameld van het beveiligingslogboek. Microsoft Antimalware voor Azure gebeurtenissen worden verzameld van het logboek voor systeemgebeurtenissen. Zie voor meer informatie over het gebruik van XPath-expressies [verbruikt gebeurtenissen](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

Het volgende voorbeeld wordt de configuratie van Azure Diagnostics:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzureRmVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Nadat u wijzigingen aanbrengt, controleert u het opslagaccount om ervoor te zorgen dat de juiste gebeurtenissen worden verzameld.

Als u problemen tijdens de installatie en configuratie hebt, opent u een [ondersteuningsaanvraag](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Selecteer **logboek integratie** als de service waarvoor u ondersteuning aanvraagt.

## <a name="can-i-use-azure-log-integration-to-integrate-network-watcher-logs-into-my-siem"></a>Kan ik Azure Log integratie netwerk-Watcher logboeken integreren in mijn SIEM gebruiken?

Azure-netwerk-Watcher wordt grote hoeveelheden van de logboekgegevens gegenereerd. Deze logboeken zijn niet bedoeld om te worden verzonden naar een SIEM. De enige ondersteunde bestemming voor netwerk-Watcher Logboeken is een opslagaccount. Integratie van Azure Log biedt geen ondersteuning voor het lezen van deze logboeken en zodat ze beschikbaar zijn in een SIEM.

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png

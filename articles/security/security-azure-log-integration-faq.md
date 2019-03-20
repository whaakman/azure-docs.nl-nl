---
title: Azure-Logboekintegratie Veelgestelde vragen over | Microsoft Docs
description: In dit artikel vindt u antwoorden op vragen over Azure-Logboekintegratie.
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
ms.date: 01/14/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 4f6a724fe6c1e8668084f1c1cefbaa01cffba181
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005847"
---
# <a name="azure-log-integration-faq"></a>Azure-Logboekintegratie Veelgestelde vragen

In dit artikel vindt u antwoorden op veelgestelde vragen (FAQ) over Azure-Logboekintegratie.

>[!IMPORTANT]
> De functie van de integratie met Azure Log worden afgeschaft door 06/01/2019. AzLog downloads zijn uitgeschakeld op 27 juni 2018. Voor informatie over wat te doen verplaatsen forward revisie van het bericht [gebruikt Azure monitor om te integreren met SIEM-hulpprogramma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure-Logboekintegratie is een service voor het besturingssysteem van Windows die u gebruiken kunt om te integreren met uw on-premises security information en event management (SIEM) systemen onbewerkte logboeken van uw Azure-resources. Deze integratie biedt via het uniforme dashboard voor al uw activa, on-premises of in de cloud. U kunt vervolgens aggregeren, correleren, analyseren en ontvang een waarschuwing voor beveiligingsgebeurtenissen die zijn gekoppeld aan uw toepassingen.

De aanbevolen methode voor het integreren van Logboeken in Azure wordt met behulp van de leverancier van uw SIEM-connector voor Azure Monitor en volgende [instructies](../azure-monitor/platform/stream-monitoring-data-event-hubs.md). Echter, als de leverancier van uw SIEM niet een connector voor Azure Monitor biedt, u mogelijk gebruik van Azure-Logboekintegratie als tijdelijke oplossing (als uw SIEM wordt ondersteund door Azure-Logboekintegratie) totdat deze een connector beschikbaar is.

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

## <a name="is-the-azure-log-integration-software-free"></a>Is de Azure-Logboekintegratie software gratis?

Ja. Er zijn geen kosten voor de Azure-Logboekintegratie-software.

## <a name="where-is-azure-log-integration-available"></a>Waar is Azure-Logboekintegratie beschikbaar?

Het is momenteel beschikbaar in commerciële Azure en Azure Government en is niet beschikbaar in China of Duitsland.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Hoe kan ik de storage-accounts van waaruit Azure VM-Logboeken is binnenhalen van Azure-Logboekintegratie zien?

Voer de opdracht **AzLog bronlijst**.

## <a name="how-can-i-tell-which-subscription-the-azure-log-integration-logs-are-from"></a>Hoe weet ik welk abonnement u voor de Azure-Logboekintegratie logboeken afkomstig van zijn?

In het geval van auditlogboeken waarin worden geplaatst in de **AzureResourcemanagerJson** mappen, het abonnement-ID in de naam van het logboek is. Dit geldt ook voor logboeken in de **AzureSecurityCenterJson** map. Bijvoorbeeld:

20170407T070805_2768037.0000000023.**1111e5ee-1111-111b-a11e-1e111e1111dc**.json

Auditlogboeken van Azure Active Directory opnemen als onderdeel van de naam van de tenant-ID.

Logboeken met diagnostische gegevens die worden gelezen vanaf een event hub omvatten niet de abonnements-ID als onderdeel van de naam. In plaats daarvan bevatten ze de beschrijvende naam die is opgegeven als onderdeel van het maken van de event hub-bron. 

## <a name="how-can-i-update-the-proxy-configuration"></a>Hoe kan ik de proxyconfiguratie bijwerken?

Als uw proxy-instellingen toestaan voor toegang tot Azure opslag niet rechtstreeks, opent u de **AZLOG. EXE. CONFIG** in het bestand **c:\Program Files\Microsoft Azure-Logboekintegratie**. Bijwerken van het bestand om op te nemen de **defaultProxy** sectie met de proxyadres van uw organisatie. Nadat de update is voltooid, stoppen en starten van de service met behulp van de opdrachten **net stop AzLog** en **net start AzLog**.

    <?xml version="1.0" encoding="utf-8"?>
    <configuration>
      <system.net>
        <connectionManagement>
          <add address="*" maxconnection="400" />
        </connectionManagement>
        <defaultProxy>
          <proxy usesystemdefault="true"
          proxyaddress="http://127.0.0.1:8888"
          bypassonlocal="true" />
        </defaultProxy>
      </system.net>
      <system.diagnostics>
        <performanceCounters filemappingsize="20971520" />
      </system.diagnostics>   

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Hoe kan ik de informatie over het abonnement in Windows-gebeurtenissen bekijken?

De abonnements-ID aan de beschrijvende naam toevoegen tijdens het toevoegen van de bron:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
De XML van de gebeurtenis heeft de volgende metagegevens, met inbegrip van de abonnements-ID:

![Gebeurtenis XML][1]

## <a name="error-messages"></a>Foutberichten
### <a name="when-i-run-the-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Wanneer ik de opdracht uitvoert ```AzLog createazureid```, waarom krijg ik de volgende fout?

Fout:

  *Kan geen AAD-toepassing - maken 72f988bf-86f1-41af-91ab-2d7cd011db37-reden Tenant = 'Verboden' - bericht = 'Onvoldoende bevoegdheden om de bewerking te voltooien.'*

De **azlog createazureid** opdracht wordt geprobeerd te maken van een service-principal in alle Azure AD-tenants voor de abonnementen die toegang tot de Azure-aanmelding heeft. Als uw Azure-aanmelding alleen een gastgebruiker in deze Azure AD-tenant is, mislukt de opdracht met "Onvoldoende bevoegdheden om de bewerking te voltooien." Vraag de tenantbeheerder uw account toevoegen als een gebruiker in de tenant.

### <a name="when-i-run-the-command-azlog-authorize-why-do-i-get-the-following-error"></a>Wanneer ik de opdracht uitvoert **azlog autoriseren**, waarom krijg ik de volgende fout?

Fout:

  *Waarschuwing roltoewijzing - AuthorizationFailed maken: De client-janedo\@microsoft.com' met object-id 'fe9e03e4-4dad-4328-910f-fd24a9660bd2' geen autorisatie om uit te voeren actie 'Microsoft.Authorization/roleAssignments/write' over scope ' /subscriptions/ 70d 95299-d689-4c 97-b971-0d8ff0000000'.*

De **azlog autoriseren** opdracht wordt de rol van lezer toegewezen aan de service-principal voor Azure AD (die zijn gemaakt met **azlog createazureid**) naar de opgegeven abonnementen. Als de Azure-aanmelding niet een CO-beheerder of een eigenaar van het abonnement is, mislukt het met een foutbericht "Autorisatie is mislukt" weergegeven. Azure Role-Based Access Control (RBAC) van CO-beheerder of eigenaar is nodig om deze actie te voltooien.

## <a name="where-can-i-find-the-definition-of-the-properties-in-the-audit-log"></a>Waar vind ik de definitie van de eigenschappen in het auditlogboek

Zie:

* [Bewerkingen controleren met Azure Resource Manager](../azure-resource-manager/resource-group-audit.md)
* [Lijst van de gebeurtenissen in een abonnement in de Azure Monitor REST-API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Waar vind ik meer informatie over Azure Security Center-waarschuwingen

Zie [beheren en erop reageren op beveiligingswaarschuwingen in Azure Security Center](../security-center/security-center-managing-and-responding-alerts.md).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hoe kan ik aanpassen wat wordt er verzameld met diagnostische gegevens van virtuele machine?

Zie voor meer informatie over het ophalen, wijzigen en stel de configuratie van de Azure Diagnostics, [PowerShell gebruiken voor het inschakelen van Azure Diagnostics in een virtuele machine met Windows](../virtual-machines/windows/ps-extensions-diagnostics.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

Het volgende voorbeeld wordt de configuratie van de Azure Diagnostics:

    Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

Het volgende voorbeeld wijzigt u de Azure Diagnostics-configuratie. In deze configuratie worden alleen gebeurtenis-is 4624 en gebeurtenis-ID 4625 verzameld van het beveiligingslogboek. Microsoft Antimalware voor Azure-evenementen worden verzameld van het logboek voor systeemgebeurtenissen. Zie voor meer informatie over het gebruik van XPath-expressies [verbruikt gebeurtenissen](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85)).

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

Het volgende voorbeeld wordt de configuratie van de Azure Diagnostics:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Nadat u wijzigingen aanbrengt, controleert u de storage-account om ervoor te zorgen dat de juiste gebeurtenissen worden verzameld.

Als u problemen ondervindt tijdens de installatie en configuratie hebt, opent u een [ondersteuningsaanvraag](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). Selecteer **Logboekintegratie** als de service waarvoor u ondersteuning aanvraagt.

## <a name="can-i-use-azure-log-integration-to-integrate-network-watcher-logs-into-my-siem"></a>Kan ik Azure-Logboekintegratie Network Watcher-logboeken integreren in mijn SIEM gebruiken?

Azure Network Watcher worden grote hoeveelheden van de logboekgegevens gegenereerd. Deze logboeken zijn niet bedoeld om te worden verzonden naar een SIEM. De enige ondersteunde bestemming voor Network Watcher-Logboeken is een opslagaccount. Azure-Logboekintegratie biedt geen ondersteuning voor het lezen van deze logboeken en zodat ze beschikbaar zijn in een SIEM.

<!--Image references-->
[1]: ./media/security-azure-log-integration-faq/event-xml.png

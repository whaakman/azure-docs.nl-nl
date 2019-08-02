---
title: Azure Log Integration Veelgestelde vragen | Microsoft Docs
description: In dit artikel vindt u antwoorden op vragen over Azure Log Integration.
services: security
documentationcenter: na
author: TomShinder
manager: MBaldwin
editor: TerryLanfear
ms.assetid: d06d1ac5-5c3b-49de-800e-4d54b3064c64
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload8: na
ms.date: 05/28/2019
ms.author: barclayn
ms.custom: azlog
ms.openlocfilehash: 7ed63364a9dc4b96470a23af3a4bff832d42621b
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727609"
---
# <a name="azure-log-integration-faq"></a>Veelgestelde vragen over Azure Log Integration

In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Log Integration.

>[!IMPORTANT]
> De Azure-functie voor logboek integratie wordt afgeschaft door 06/15/2019. AzLog-down loads zijn op 27 juni 2018 uitgeschakeld. Raadpleeg voor meer informatie over wat u verder kunt doen met [Azure monitor om te integreren met Siem-hulpprogram ma's](https://azure.microsoft.com/blog/use-azure-monitor-to-integrate-with-siem-tools/) 

Azure Log Integration is een Windows-besturings systeem-service die u kunt gebruiken om onbewerkte logboeken te integreren van uw Azure-resources in uw on-premises SIEM-systemen (Security Information and Event Management). Deze integratie biedt een uniform dash board voor al uw assets, on-premises of in de Cloud. U kunt vervolgens de beveiligings gebeurtenissen die aan uw toepassingen zijn gekoppeld, samen voegen, correleren, analyseren en waarschuwen.

De aanbevolen methode voor het integreren van Azure-Logboeken is door gebruik te maken van de Azure Monitor-connector van uw SIEM-leverancier en deze [instructies](/azure/azure-monitor/platform/stream-monitoring-data-event-hubs)te volgen. Als uw SIEM-leverancier echter geen connector biedt om Azure Monitor, kunt u Azure Log Integration gebruiken als tijdelijke oplossing (als uw SIEM wordt ondersteund door Azure Log Integration) totdat een dergelijke connector beschikbaar is.

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="is-the-azure-log-integration-software-free"></a>Is de Azure Log Integration software gratis?

Ja. Er worden geen kosten in rekening gebracht voor de Azure Log Integration software.

## <a name="where-is-azure-log-integration-available"></a>Waar is Azure Log Integration beschikbaar?

Het is momenteel beschikbaar in azure Commercial en Azure Government en is niet beschikbaar in China of Duitsland.

## <a name="how-can-i-see-the-storage-accounts-from-which-azure-log-integration-is-pulling-azure-vm-logs"></a>Hoe kan ik de opslag accounts zien waarvan Azure Log Integration Azure-VM-logboeken gaat ophalen?

Voer de opdracht **AzLog source list**uit.

## <a name="how-can-i-tell-which-subscription-the-azure-log-integration-logs-are-from"></a>Hoe kan ik zien welk abonnement de Azure Log Integration logboeken heeft?

In het geval van audit logboeken die in de **AzureResourcemanagerJson** -directory's worden geplaatst, bevindt de abonnements-id zich in de naam van het logboek bestand. Dit geldt ook voor logboeken in de map **AzureSecurityCenterJson** . Bijvoorbeeld:

20170407T070805_2768037.0000000023.**1111e5ee-1111-111b-a11e-1e111e1111dc**.json

Azure Active Directory audit logboeken bevatten de Tenant-ID als onderdeel van de naam.

Diagnostische logboeken die worden gelezen vanuit een Event Hub, bevatten de abonnements-ID niet als onderdeel van de naam. In plaats daarvan bevatten ze de beschrijvende naam die is opgegeven als onderdeel van het maken van de Event Hub bron. 

## <a name="how-can-i-update-the-proxy-configuration"></a>Hoe kan ik de proxy configuratie bijwerken?

Als uw proxy instelling toegang tot Azure Storage niet rechtstreeks toestaat, opent u het **AZLOG. EXE. CONFIG** -bestand in **c:\Program Files\Microsoft Azure log Integration**. Werk het bestand bij om de **defaultProxy** -sectie op te laten bevatten met het proxy adres van uw organisatie. Nadat de update is uitgevoerd, stopt en start u de service met de opdrachten **net stop AzLog** en **net start AzLog**.

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

## <a name="how-can-i-see-the-subscription-information-in-windows-events"></a>Hoe kan ik de abonnements gegevens weer geven in Windows-gebeurtenissen?

Voeg de abonnements-ID toe aan de beschrijvende naam tijdens het toevoegen van de bron:

    Azlog source add <sourcefriendlyname>.<subscription id> <StorageName> <StorageKey>  
De gebeurtenis-XML bevat de volgende meta gegevens, met inbegrip van de abonnements-ID:

![Gebeurtenis-XML](media/azure-log-integration-faq/event-xml.png)

## <a name="error-messages"></a>Foutberichten
### <a name="when-i-run-the-command-azlog-createazureid-why-do-i-get-the-following-error"></a>Waarom wordt de volgende fout ```AzLog createazureid```weer geven wanneer ik de opdracht uitvoer?

Fout:

  *Kan geen AAD-toepassing maken-Tenant 72f988bf-86f1-41af-91ab-2d7cd011db37-reason = ' verboden '-Message = ' onvoldoende bevoegdheden om de bewerking te volt ooien. '*

De **azlog createazureid** opdracht probeert een service-principal te maken in alle Azure AD-tenants voor de abonnementen waartoe de Azure-aanmelding toegang heeft. Als uw Azure-aanmelding alleen een gast gebruiker in die Azure AD-Tenant is, mislukt de opdracht met "onvoldoende bevoegdheden om de bewerking te volt ooien." Vraag de Tenant beheerder om uw account toe te voegen als gebruiker in de Tenant.

### <a name="when-i-run-the-command-azlog-authorize-why-do-i-get-the-following-error"></a>Waarom wordt de volgende fout weer geven wanneer ik de opdracht **azlog**autoriseren?

Fout:

  *Waarschuwing bij het maken van roltoewijzing-AuthorizationFailed: De client janedo\@Microsoft.com ' met object-id ' fe9e03e4-4dad-4328-910f-fd24a9660bd2 ' heeft geen machtiging om de actie ' micro soft. Authorization/roleAssignments/write ' over scope '/Subscriptions/uit te voeren 70d95299-d689-4c97-b971-0d8ff0000000'.*

Met de opdracht **azlog** autorisement wordt de rol van lezer toegewezen aan de Azure AD-Service-Principal (gemaakt met **azlog createazureid**) voor de geleverde abonnementen. Als de Azure-aanmelding geen mede beheerder of eigenaar van het abonnement is, mislukt het fout bericht ' autorisatie is mislukt '. Voor het volt ooien van deze actie is een op Azure Role Access Control (RBAC) van co-beheerder of eigenaar vereist.

## <a name="where-can-i-find-the-definition-of-the-properties-in-the-audit-log"></a>Waar kan ik de definitie van de eigenschappen in het audit logboek vinden?

Zie:

* [Controle bewerkingen met Azure Resource Manager](/azure/azure-resource-manager/resource-group-audit)
* [De beheer gebeurtenissen in een abonnement in de Azure Monitor weer geven REST API](https://msdn.microsoft.com/library/azure/dn931934.aspx)

## <a name="where-can-i-find-details-on-azure-security-center-alerts"></a>Waar vind ik informatie over Azure Security Center-waarschuwingen?

Zie [beveiligings waarschuwingen beheren en erop reageren in azure Security Center](/azure/security-center/security-center-managing-and-responding-alerts).

## <a name="how-can-i-modify-what-is-collected-with-vm-diagnostics"></a>Hoe kan ik wijzigen wat er met de diagnostische gegevens van virtuele machines wordt verzameld?

Zie voor meer informatie over het verkrijgen, wijzigen en instellen van de Azure Diagnostics configuratie [Power shell gebruiken om Azure Diagnostics in te scha kelen op een virtuele machine waarop Windows wordt uitgevoerd](/azure/virtual-machines/windows/ps-extensions-diagnostics?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json). 

In het volgende voor beeld wordt de Azure Diagnostics configuratie opgehaald:

    Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient
    $publicsettings = (Get-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient).PublicSettings
    $encodedconfig = (ConvertFrom-Json -InputObject $publicsettings).xmlCfg
    $xmlconfig = [System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($encodedconfig))
    Write-Host $xmlconfig

    $xmlconfig | Out-File -Encoding utf8 -FilePath "d:\WADConfig.xml"

In het volgende voor beeld wordt de configuratie van de Azure Diagnostics gewijzigd. In deze configuratie worden alleen gebeurtenis-ID 4624 en gebeurtenis-ID 4625 verzameld uit het beveiligings logboek. Micro soft antimalware voor Azure-gebeurtenissen wordt verzameld uit het systeem gebeurtenis logboek. Zie voor meer informatie over het gebruik van XPath-expressies [gebeurtenissen](https://msdn.microsoft.com/library/windows/desktop/dd996910(v=vs.85))gebruiken.

    <WindowsEventLog scheduledTransferPeriod="PT1M">
        <DataSource name="Security!*[System[(EventID=4624 or EventID=4625)]]" />
        <DataSource name="System!*[System[Provider[@Name='Microsoft Antimalware']]]"/>
    </WindowsEventLog>

In het volgende voor beeld wordt de configuratie van de Azure Diagnostics ingesteld:

    $diagnosticsconfig_path = "d:\WADConfig.xml"
    Set-AzVMDiagnosticsExtension -ResourceGroupName AzLog-Integration -VMName AzlogClient -DiagnosticsConfigurationPath $diagnosticsconfig_path -StorageAccountName log3121 -StorageAccountKey <storage key>

Nadat u wijzigingen hebt aangebracht, controleert u het opslag account om te controleren of de juiste gebeurtenissen zijn verzameld.

Als u problemen ondervindt tijdens de installatie en configuratie, moet u een ondersteunings [aanvraag](/azure/azure-supportability/how-to-create-azure-support-request)openen. Selecteer **integratie** van Logboeken als de service waarvoor u ondersteuning aanvraagt.

## <a name="can-i-use-azure-log-integration-to-integrate-network-watcher-logs-into-my-siem"></a>Kan ik Azure Log Integration gebruiken om Network Watcher-logboeken te integreren in mijn SIEM?

In azure Network Watcher worden grote hoeveel heden logboek registratie gegevens gegenereerd. Deze logboeken zijn niet bedoeld om te worden verzonden naar een SIEM. De enige ondersteunde bestemming voor Network Watcher Logboeken is een opslag account. Azure Log Integration biedt geen ondersteuning voor het lezen van deze logboeken en het beschikbaar maken ervan voor een SIEM.



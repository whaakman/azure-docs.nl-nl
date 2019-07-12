---
title: Virtuele Windows-bureaublad voor probleemoplossing overzicht, feedback en ondersteuning - Azure
description: Een overzicht voor het oplossen van problemen tijdens het instellen van een omgeving met virtuele Windows-bureaublad tenants.
services: virtual-desktop
author: ChJenk
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/08/2019
ms.author: v-chjenk
ms.openlocfilehash: b349d7f384c801e95d745f7a18535f9f5d13cf86
ms.sourcegitcommit: f10ae7078e477531af5b61a7fe64ab0e389830e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/05/2019
ms.locfileid: "67605216"
---
# <a name="troubleshooting-overview-feedback-and-support"></a>Probleemoplossingsoverzicht, feedback en ondersteuning

Dit artikel bevat een overzicht van de problemen die u kan optreden bij het instellen van een omgeving met virtuele Windows-bureaublad tenants en biedt methoden voor het oplossen van problemen.

## <a name="provide-feedback"></a>Feedback geven

We zijn niet op dit moment kwesties duurt zolang virtuele Windows-bureaublad in preview. Ga naar de [Windows virtuele bureaublad Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) bespreken van de virtuele Windows-bureaublad-service met het productteam en actieve communityleden.

## <a name="escalation-tracks"></a>Escalatie sporen te wissen

Gebruik de volgende tabel om te identificeren en oplossen van problemen die optreden kunnen bij het instellen van een omgeving met tenants met behulp van extern bureaublad-client. Zodra uw tenant de ingesteld, kunt u onze nieuwe [Diagnostics-service](https://docs.microsoft.com/azure/virtual-desktop/diagnostics-role-service) voor het identificeren van problemen voor algemene scenario's.

>[!NOTE]
>We zijn niet op dit moment kwesties duurt zolang virtuele Windows-bureaublad in preview. Wanneer we naar ondersteuning voor virtuele Windows-bureaublad verwijzen, gaat u naar ons forum Tech-Community voor nu. Ga naar de [Windows virtuele bureaublad Tech Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) wilt discussiëren over problemen met het productteam en actieve communityleden. Als u moet een Ondersteuningsprobleem op te lossen, bevatten de activiteits-ID en de geschatte tijdsbestek voor wanneer het probleem is opgetreden.

| **Probleem**                                                            | **Voorgestelde oplossing**  |
|----------------------------------------------------------------------|-------------------------------------------------|
| Het maken van een Tenant                                                    | Als er een Azure-storing, neem dan contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/options/); Neem anders contact op met **ondersteuning voor extern bureaublad Services/Windows virtuele bureaublad**.|
| Toegang tot de sjablonen op Marketplace in Azure portal       | Als er een Azure-storing, neem dan contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/options/). <br> <br> Azure virtuele Windows-bureaublad Marketplace-sjablonen zijn gratis beschikbaar.|
| Toegang tot Azure Resource Manager-sjablonen van GitHub                                  | Zie de sectie 'Creating Windows virtuele bureaublad-sessiehost VM's ' van [Tenant en de host een pool maken](troubleshoot-set-up-issues.md). Als het probleem zich nog steeds niet-omgezette, neem dan contact op met de [GitHub-ondersteuningsteam](https://github.com/contact). <br> <br> Als de fout optreedt nadat u hebt toegang tot de sjabloon in GitHub, neem dan contact op met [ondersteuning voor Azure](https://azure.microsoft.com/support/options/).|
| Sessie host groep Azure Virtual Network (VNET) en Express Route-instellingen               | Neem contact op met **Azure-ondersteuning (netwerk)** . |
| Sessie-host-pool maken van de virtuele Machine (VM) als Azure Resource Manager-sjablonen die zijn voorzien van virtuele Windows-bureaublad niet worden gebruikt | Neem contact op met **Azure-ondersteuning (rekenen)** . <br> <br> Voor problemen met de Azure Resource Manager-sjablonen die zijn meegeleverd met virtuele Windows-bureaublad, Zie gedeelte voor het maken van virtuele Windows-bureaublad-tenant van [Tenant en de host een pool maken](troubleshoot-set-up-issues.md). |
| Virtuele Windows-bureaublad-sessie hostomgeving beheren vanuit Azure portal    | Neem contact op met **ondersteuning van Azure**. <br> <br> Zie voor van beheerproblemen bij het gebruik van Remote Desktop Services/Windows virtuele bureaublad PowerShell [Windows virtuele bureaublad PowerShell](troubleshoot-powershell.md) of neem contact op met de **ondersteuningsteam Remote Desktop Services/Windows virtueel bureaublad** . |
| Beheren van virtuele Windows-bureaublad configuratie gekoppeld aan host pools en toepassingsgroepen (app-groepen)      | Zie [Windows virtuele bureaublad PowerShell](troubleshoot-powershell.md), of neem contact op met de **Remote Desktop Services/Windows virtueel bureaublad ondersteuningsteam**. <br> <br> Als er problemen zijn gekoppeld aan de grafische gebruikersinterface (GUI) van voorbeeldgegevens, kunt u contact opnemen met de Yammer-community.|
| Storing van de extern bureaublad-clients op het startscherm                                                 | Zie [verbindingen met extern bureaublad-client](troubleshoot-client-connection.md) en als het probleem niet is opgelost, neem dan contact op met **Remote Desktop Services/Windows virtueel bureaublad ondersteuningsteam**.  <br> <br> Als het een netwerkprobleem is, moeten uw gebruikers contact op met de netwerkbeheerder. |
| Verbonden maar er geen invoer                                                                 | Problemen oplossen met de ' gebruiker verbinding maakt, maar er niets wordt weergegeven (Er is geen invoer) "sectie van [verbindingen met extern bureaublad-client](troubleshoot-client-connection.md). <br> <br> Als uw gebruikers zijn toegewezen aan een app-groep, escaleren naar de **Remote Desktop Services/Windows virtueel bureaublad ondersteuningsteam**. |
| Problemen met detectie vanwege de netwerk-feed                                            | Uw gebruikers moeten contact op met de netwerkbeheerder. |
| Clients die verbinding maken                                                                    | Zie [verbindingen met extern bureaublad-client](troubleshoot-client-connection.md) en als dat niet het probleem is opgelost, Zie [Session host Virtuele-machineconfiguratie](troubleshoot-vm-configuration.md). |
| Reactietijd van externe toepassingen of bureaublad                                      | Als er problemen zijn gekoppeld aan een bepaalde toepassing of het product, neem dan contact op met het team dat verantwoordelijk is voor dat product. |
| Licentieverlening berichten of fouten                                                          | Als er problemen zijn gekoppeld aan een bepaalde toepassing of het product, neem dan contact op met het team dat verantwoordelijk is voor dat product. |

## <a name="next-steps"></a>Volgende stappen

- Zie voor het oplossen van problemen tijdens het maken van een tenant en host-pool in een omgeving met virtuele Windows-bureaublad, [Tenant en de host een pool maken](troubleshoot-set-up-issues.md).
- Zie voor het oplossen van problemen tijdens het configureren van een virtuele machine (VM) in virtuele Windows-bureaublad, [Session host Virtuele-machineconfiguratie](troubleshoot-vm-configuration.md).
- Zie voor het oplossen van problemen met virtuele Windows-bureaublad-clientverbindingen, [verbindingen met extern bureaublad-client](troubleshoot-client-connection.md).
- Zie voor het oplossen van problemen bij het gebruik van PowerShell met virtuele Windows-bureaublad, [Windows virtuele bureaublad PowerShell](troubleshoot-powershell.md).
- Zie voor meer informatie over de Preview-service, [Windows Desktop Preview-omgeving](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Als u wilt u een zelfstudie voor problemen oplossen, Zie [zelfstudie: Problemen met sjabloonimplementaties van Resource Manager-oplossen](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot).
- Zie voor meer informatie over het controleren van acties, [bewerkingen controleren met Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Zie voor meer informatie over acties voor het bepalen van de fouten tijdens de implementatie, [implementatiebewerkingen bekijken](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
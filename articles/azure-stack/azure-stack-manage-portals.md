---
title: Met behulp van de beheerportal in Azure Stack | Microsoft Docs
description: Als Azure Stack-operators, informatie over het gebruik van de beheerportal.
services: azure-stack
documentationcenter: ''
author: jeffgilb
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.custom: mvc
ms.date: 01/07/2019
ms.author: jeffgilb
ms.reviewer: ''
ms.lastreviewed: 01/07/2019
ms.openlocfilehash: dacd628098d263234f56b20879a5ac2fdf87a809
ms.sourcegitcommit: 898b2936e3d6d3a8366cfcccc0fccfdb0fc781b4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/30/2019
ms.locfileid: "55243161"
---
# <a name="quickstart-use-the-azure-stack-administration-portal"></a>Snelstartgids: Gebruik de Azure Stack-beheerportal

*Van toepassing op: Geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Er zijn twee portals in Azure Stack; de beheerportal en de gebruikersportal (soms aangeduid als de *tenant* portal.) Als een Azure Stack-Operator, kunt u de beheerportal gebruiken voor het dagelijkse beheer en bewerkingen van Azure Stack.

## <a name="access-the-administrator-portal"></a>Toegang tot de beheerdersportal

Toegang tot de administrator-portal, blader naar de portal-URL en meld u aan met behulp van de referenties van een Azure Stack-operators. Voor een geïntegreerd systeem, de portal die URL varieert op basis van de regionaam en externe volledig gekwalificeerde domeinnaam (FQDN) van uw Azure Stack-implementatie. De beheerportal-URL is altijd hetzelfde zijn voor Azure Stack Development Kit (ASDK) implementaties. 

| Omgeving | Portal-URL van de beheerder |   
| -- | -- | 
| ASDK| https://adminportal.local.azurestack.external  |
| Geïntegreerde systemen | https://adminportal.&lt; *regio*&gt;.&lt; *FQDN-naam*&gt; | 
| | |

> [!TIP]
> Voor een ASDK-omgeving, moet u eerst voor zorgen dat u kunt [verbinding maken met de host van development kit](azure-stack-connect-azure-stack.md) via verbinding met extern bureaublad of via een virtueel particulier netwerk (VPN).

 ![De beheerportal](media/azure-stack-manage-portals/admin-portal.png)

De standaardtijdzone voor alle Azure Stack-implementaties is ingesteld op Coordinated Universal Time (UTC). 

In de beheerdersportal voor de, kunt u handelingen zoals:

* [Azure Stack registreren bij Azure](azure-stack-registration.md)
* [De marketplace vullen](azure-stack-download-azure-marketplace-item.md)
* [Maken, plannen, aanbiedingen en abonnementen voor gebruikers](azure-stack-plan-offer-quota-overview.md)
* [Status en waarschuwingen bewaken](azure-stack-monitor-health.md)
* [Azure Stack-updates beheren](azure-stack-updates.md)

De **beknopte zelfstudie** tegel vindt u koppelingen naar online documentatie voor de meest algemene taken.

Hoewel een operator resources zoals virtuele machines, virtuele netwerken en storage-accounts in de beheerportal maken kan, moet u [Meld u aan bij de gebruikersportal aanmeldt](user/azure-stack-use-portal.md) kunt maken en testen van resources.

>[!NOTE]
>De **maken van een virtuele machine** koppeling in de tegel van Quick Start-zelfstudie heeft u een virtuele machine maken in de beheerportal, maar dit is alleen bedoeld om te valideren dat Azure Stack met succes is geïmplementeerd.

## <a name="understand-subscription-behavior"></a>Begrijp het gedrag van abonnement

Er zijn drie abonnementen die standaard in de beheerportal; worden gemaakt verbruik, standaard-provider en softwarelicentiecontrole. Als een Operator wordt voornamelijk gebruikt u de *Providerabonnement standaard*. U kunt geen andere abonnementen toevoegen en deze gebruiken in de beheerportal. 

Andere abonnementen worden gemaakt door gebruikers in de gebruikersportal op basis van het plannen en aanbiedingen die u voor hen maken. De gebruikersportal biedt echter geen toegang tot een van de administratieve of operationele mogelijkheden van de beheerportal.

De portals beheer en worden ondersteund door afzonderlijke exemplaren van Azure Resource Manager. Vanwege deze scheiding Azure Resource Manager overschreden abonnementen niet door de portals. Bijvoorbeeld, als u, als een Azure Stack-operators zich bij de gebruikersportal aanmeldt aanmelden, u geen toegang tot de *Providerabonnement standaard*. Hoewel u geen toegang tot alle beheerfuncties, kunt u abonnementen voor uzelf maken uit beschikbare openbare aanbiedingen. Als u bent aangemeld bij de gebruikersportal aanmeldt als een tenant-gebruiker worden beschouwd.

  >[!NOTE]
  >In een omgeving ASDK als een gebruiker tot dezelfde tenantmap als de Azure Stack-operators behoort, worden ze niet geblokkeerd bij de beheerportal. Ze kunnen echter toegang krijgen tot een van de beheerfuncties of toevoegen van de abonnementen voor toegang tot aanbiedingen die beschikbaar zijn voor deze in de gebruikersportal.

## <a name="administration-portal-tips"></a>Beheer van portal tips

### <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een reeks standaardtegels. U kunt selecteren **dashboard bewerken** wijzigen van het standaarddashboard, of selecteer **nieuw dashboard** om toe te voegen een aangepast dashboard. U kunt eenvoudig tegels toevoegen aan een dashboard. Bijvoorbeeld, kunt u **+ een resource maken**, met de rechtermuisknop op **aanbiedingen + plannen**, en selecteer vervolgens **vastmaken aan dashboard**.

Soms ziet u mogelijk een leeg dashboard in de portal. Als u wilt herstellen in het dashboard, klikt u op **Dashboard bewerken**, klik met de rechtermuisknop op en selecteer **opnieuw ingesteld op standaardstatus**.

### <a name="quick-access-to-online-documentation"></a>Snelle toegang tot de online documentatie

Toegang tot de documentatie van Azure Stack-operators, gebruikt u de Help en ondersteuning voor het pictogram (vraagteken) in de rechterbovenhoek van de beheerdersportal. Plaats de cursor naar het pictogram en selecteer vervolgens **Help en ondersteuning**.

### <a name="quick-access-to-help-and-support"></a>Snelle toegang tot help en ondersteuning

Als u Selecteer het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en selecteer vervolgens **nieuwe ondersteuningsaanvraag**, een van de volgende resultaten gebeurt:

- Als u een geïntegreerd systeem, opent deze actie u een site waar u rechtstreeks een ondersteuningsticket met van Microsoft ondersteuning voor Services (CSS openen kunt). Raadpleeg [waar u ondersteuning krijgen](azure-stack-manage-basics.md#where-to-get-support) om te begrijpen wanneer u via Microsoft ondersteuning of via het ondersteuningsteam van het oorspronkelijke leveranciers (OEM) hardware leverancier moet gaan.
- Als u de ASDK, opent u deze actie de [forums-site voor Azure Stack](https://social.msdn.microsoft.com/Forums/home?forum=AzureStack) rechtstreeks. Deze forums worden regelmatig gecontroleerd. Omdat de ASDK een evaluatieomgeving is, is er geen officiële ondersteuning aangeboden via Microsoft CSS.

### <a name="quick-access-to-the-azure-roadmap"></a>Snelle toegang tot de Azure-roadmap

Als u selecteert **Help en ondersteuning** (het vraagteken) in de rechterbovenhoek van de beheer-portal en selecteert u vervolgens **Azure-roadmap**, een nieuw browsertabblad geopend en gaat u naar de Azure-roadmap. Door te typen **Azure Stack** in de **producten** zoekvak typt, ziet u alle schema-updates voor Azure Stack.

## <a name="next-steps"></a>Volgende stappen

[Azure Stack registreren bij Azure](azure-stack-registration.md) in en vul de [Azure Stack marketplace](azure-stack-marketplace.md) met items die u wilt uw gebruikers bieden. 

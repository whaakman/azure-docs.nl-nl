---
title: Met behulp van de beheerdersportal in Azure Stack | Microsoft Docs
description: Als Azure Stack-operators, informatie over het gebruik van de beheerdersportal.
services: azure-stack
documentationcenter: ''
author: mattbriggs
manager: femila
editor: ''
ms.assetid: 02c7ff03-874e-4951-b591-28166b7a7a79
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/12/2018
ms.author: mabrigg
ms.openlocfilehash: 975c5c2c5f0da7a6db946c6d1032b485947134d2
ms.sourcegitcommit: c29d7ef9065f960c3079660b139dd6a8348576ce
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/12/2018
ms.locfileid: "44717695"
---
# <a name="using-the-administrator-portal-in-azure-stack"></a>Met behulp van de beheerdersportal in Azure Stack

*Is van toepassing op: geïntegreerde Azure Stack-systemen en Azure Stack Development Kit*

Er zijn twee portals in Azure Stack; de beheerdersportal en de gebruikersportal (soms aangeduid als de *tenant* portal.) Als Azure Stack-operators, kunt u de beheerdersportal gebruiken voor het dagelijkse beheer en bewerkingen van Azure Stack.

## <a name="access-the-administrator-portal"></a>Toegang tot de beheerdersportal

Voor een ontwikkelomgeving kit, moet u eerst voor zorgen dat u kunt [verbinding maken met de host van development kit](azure-stack-connect-azure-stack.md) via verbinding met extern bureaublad of via een virtueel particulier netwerk (VPN).

Toegang tot de administrator-portal, blader naar de portal-URL en meld u aan met behulp van de referenties van een Azure Stack-operators. Voor een geïntegreerd systeem, de portal die URL varieert op basis van de regionaam en externe volledig gekwalificeerde domeinnaam (FQDN) van uw Azure Stack-implementatie.

| Omgeving | Portal-URL van de beheerder |   
| -- | -- | 
| Development kit| https://adminportal.local.azurestack.external  |
| Geïntegreerde systemen | https://adminportal.&lt; *regio*&gt;.&lt; *FQDN-naam*&gt; | 
| | |

 ![De beheerdersportal](media/azure-stack-manage-portals/admin-portal.png)

In de beheerdersportal voor de, kunt u bijvoorbeeld dingen doen:

* Beheren van de infrastructuur (met inbegrip van systeemstatus, updates, capaciteit, enz.)
* De Marketplace vullen
* Abonnementen voor gebruikers maken
* Plannen en aanbiedingen maken

De **beknopte zelfstudie** tegel vindt u koppelingen naar online documentatie voor de meest algemene taken.

Hoewel een operator kunnen resources maken, zoals virtuele machines, virtuele netwerken en opslagaccounts in de beheerdersportal, moet u [Meld u aan bij de gebruikersportal aanmeldt](user/azure-stack-use-portal.md) kunt maken en testen van resources.

>[!NOTE]
>De **maken van een virtuele machine** koppeling in de tegel van Quick Start-zelfstudie heeft u een virtuele machine maken in de beheerdersportal, maar dit is alleen bedoeld voor het valideren van Azure Stack nadat deze voor het eerst wordt geïmplementeerd.

## <a name="understand-subscription-behavior"></a>Begrijp het gedrag van abonnement

Er is slechts één abonnement beschikbaar voor gebruik van de beheerdersportal. Dit abonnement is de *Providerabonnement standaard*. U kunt geen andere abonnementen toevoegen en deze gebruiken in de beheerdersportal.

Als Azure Stack-operators, kunt u abonnementen toevoegen voor uw gebruikers (inclusief uzelf) uit de beheerdersportal. Gebruikers (inclusief uzelf) kunnen openen en gebruiken van deze abonnementen uit de **gebruiker** portal. De gebruikersportal biedt echter geen toegang tot een van de administratieve of operationele mogelijkheden van de beheerdersportal.

De beheerder en gebruiker portals worden ondersteund door afzonderlijke exemplaren van Azure Resource Manager. Vanwege deze scheiding Resource Manager overschreden abonnementen niet door de portals. Bijvoorbeeld, als u, als Azure Stack-operators, zich bij de gebruikersportal aanmeldt aanmelden, u geen toegang tot de *Providerabonnement standaard*. Hoewel u geen toegang tot alle beheerfuncties, kunt u abonnementen voor uzelf maken uit beschikbare openbare aanbiedingen. Als u bent aangemeld bij de gebruikersportal aanmeldt als een tenant-gebruiker worden beschouwd.

  >[!NOTE]
  >In de ontwikkelomgeving kit, als een gebruiker tot dezelfde tenantmap als de Azure Stack-operators behoort, worden ze niet geblokkeerd bij de beheerdersportal. Echter, hebben geen toegang tot een van de beheerfuncties. Ook vanuit de beheerdersportal ze kunnen niet toevoegen-abonnementen of toegang biedt die beschikbaar zijn voor deze in de gebruikersportal.

## <a name="administrator-portal-tips"></a>Beheerder portal tips

### <a name="customize-the-dashboard"></a>Het dashboard aanpassen

Het dashboard bevat een reeks standaardtegels. U kunt selecteren **dashboard bewerken** wijzigen van het standaarddashboard, of selecteer **nieuw dashboard** om toe te voegen een aangepast dashboard. U kunt eenvoudig tegels toevoegen aan een dashboard. Bijvoorbeeld, kunt u **+ een resource maken**, met de rechtermuisknop op **aanbiedingen + plannen**, en selecteer vervolgens **vastmaken aan dashboard**.

### <a name="quick-access-to-online-documentation"></a>Snelle toegang tot de online documentatie

Voor toegang tot de documentatie van Azure Stack-operator, gebruikt u de Help en ondersteuning voor pictogram (vraagteken) in de rechterbovenhoek van de beheerdersportal. Plaats de cursor naar het pictogram en selecteer vervolgens **Help en ondersteuning**.

### <a name="quick-access-to-help-and-support"></a>Snelle toegang tot help en ondersteuning

Als u Selecteer het pictogram Help en ondersteuning (vraagteken) in de rechterbovenhoek van de beheerdersportal en selecteer vervolgens **nieuwe ondersteuningsaanvraag**, een van de volgende resultaten optreden:

- Als u een geïntegreerd systeem, opent deze actie u een site waar u rechtstreeks een ondersteuningsticket met van Microsoft ondersteuning voor Services (CSS openen kunt). Raadpleeg [waar u ondersteuning krijgen](azure-stack-manage-basics.md#where-to-get-support) om te begrijpen wanneer u via Microsoft ondersteuning of via het ondersteuningsteam van het oorspronkelijke leveranciers (OEM) hardware leverancier moet gaan.
- Als u de development kit, deze actie opent u de Azure Stack-forums-site rechtstreeks. Deze forums worden regelmatig gecontroleerd. Omdat de development kit een evaluatieomgeving is, is er geen officiële ondersteuning aangeboden via Microsoft CSS.

## <a name="next-steps"></a>Volgende stappen

- [Regiobeheer in Azure Stack](azure-stack-region-management.md)

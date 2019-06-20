---
title: Problemen oplossen met Azure Data Catalog
description: Dit artikel worden algemene problemen voor het oplossen van problemen met voor Azure Data Catalog-resources.
author: JasonWHowell
ms.author: jasonh
ms.service: data-catalog
ms.topic: troubleshooting
ms.date: 06/13/2019
ms.openlocfilehash: ed74e90e5e8ed55b75968f51cb50e6a1b4cdd75d
ms.sourcegitcommit: b7a44709a0f82974578126f25abee27399f0887f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67203509"
---
# <a name="troubleshooting-azure-data-catalog"></a>Oplossen van problemen met Azure Data Catalog

Dit artikel worden algemene problemen voor het oplossen van problemen met voor Azure Data Catalog-resources. 

## <a name="functionality-limitations"></a>Beperkingen van de functionaliteit

Wanneer u Azure Data Catalog, zijn de volgende functionaliteit is beperkt:

- Accounts met het type **gastrol** worden niet ondersteund. U kunt geen Gast-accounts toevoegen als gebruikers van Azure Data Catalog en gastgebruikers ook kunnen de portal op www.azuredatacatalog.com niet gebruiken.

- Het maken van Azure Data Catalog-resources met behulp van Azure Resource Manager-sjablonen of Azure PowerShell-opdrachten wordt niet ondersteund.

- De Azure Data Catalog-resource kan niet worden verplaatst tussen Tenants van Azure.

## <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-beleid configureren

Het kan voorkomen dat u zich wel kunt aanmelden bij de Azure Data Catalog-portal, maar niet bij het hulpprogramma voor gegevensbronregistratie. Er wordt dan een foutmelding weergegeven. Deze fout kan optreden wanneer u zich op het bedrijfsnetwerk bevindt of wanneer u verbinding vanaf buiten het bedrijfsnetwerk bevindt maakt.

Het registratiehulpprogramma maakt gebruik van *formulierverificatie* om de gebruikersaanmeldingen te valideren bij Azure Active Directory. Als u zich wilt aanmelden, moet een Azure Active Directory-beheerder formulierverificatie inschakelen in het *algemene verificatiebeleid*.

Met het algemene verificatiebeleid kunt u afzonderlijke verificatie voor intranet- en extranetverbindingen inschakelen, zoals wordt weergegeven in de volgende afbeelding. Aanmelden fouten kunnen optreden als formulierverificatie niet is ingeschakeld voor het netwerk van waaruit u verbinding maakt.

 ![Het algemene verificatiebeleid van Azure Active Directory](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Zie [Verificatiebeleid configureren](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* [Maken van een Azure Data Catalog](data-catalog-get-started.md)

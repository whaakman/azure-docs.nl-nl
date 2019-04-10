---
title: De Azure Active Directory-beleid configureren voor Azure Data Catalog
description: U kunt een situatie kan optreden wanneer u zich kunt aanmelden bij de Azure Data Catalog-portal, maar wanneer u probeert te melden bij het registratiehulpprogramma voor gegevensbronnen, er wordt een foutbericht weergegeven.
author: markingmyname
ms.author: maghan
ms.service: data-catalog
ms.topic: conceptual
ms.date: 04/06/2019
ms.openlocfilehash: 558f8845f5469bf157188e20f1ec65a07ff8355f
ms.sourcegitcommit: 43b85f28abcacf30c59ae64725eecaa3b7eb561a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/09/2019
ms.locfileid: "59363029"
---
# <a name="azure-active-directory-policy-configuration"></a>Azure Active Directory-beleid configureren

Het kan voorkomen dat u zich wel kunt aanmelden bij de Azure Data Catalog-portal, maar niet bij het hulpprogramma voor gegevensbronregistratie. Er wordt dan een foutmelding weergegeven. Deze fout kan optreden wanneer u zich op het bedrijfsnetwerk bevindt of wanneer u verbinding vanaf buiten het bedrijfsnetwerk bevindt maakt.

## <a name="registration-tool"></a>Hulpprogramma voor registratie

Het registratiehulpprogramma maakt gebruik van *formulierverificatie* om de gebruikersaanmeldingen te valideren bij Azure Active Directory. Als u zich wilt aanmelden, moet een Azure Active Directory-beheerder formulierverificatie inschakelen in het *algemene verificatiebeleid*.

Met het algemene verificatiebeleid kunt u afzonderlijke verificatie voor intranet- en extranetverbindingen inschakelen, zoals wordt weergegeven in de volgende afbeelding. Aanmelden fouten kunnen optreden als formulierverificatie niet is ingeschakeld voor het netwerk van waaruit u verbinding maakt.

 ![Het algemene verificatiebeleid van Azure Active Directory](./media/troubleshoot-policy-configuration/global-auth-policy.png)

Zie [Verificatiebeleid configureren](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn486781(v=ws.11)) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen

* [Een Azure Data Catalog maken](data-catalog-get-started.md)
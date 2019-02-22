---
title: Gebruikers toevoegen voor Azure Stack ADFS | Microsoft Docs
description: Meer informatie over het toevoegen van gebruikers voor AD FS-implementaties van Azure Stack
services: azure-stack
documentationcenter: ''
author: PatAltimore
manager: femila
editor: ''
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/21/2019
ms.author: patricka
ms.reviewer: unknown
ms.lastreviewed: 02/21/2019
ms.openlocfilehash: 1b47739200c79317273ea0c788f21a7ee4a3b818
ms.sourcegitcommit: a4efc1d7fc4793bbff43b30ebb4275cd5c8fec77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/21/2019
ms.locfileid: "56648503"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Azure Stack-gebruikers toevoegen in AD FS
U kunt de **Active Directory: gebruikers en Computers** module Extra gebruikers toevoegen aan een Azure Stack-omgeving gebruik te maken van AD FS als de id-provider.

## <a name="add-windows-server-active-directory-users"></a>Windows Server Active Directory-gebruikers toevoegen
> [!TIP]
> In dit voorbeeld gebruikt de standaard azurestack.local ASDK active directory. 

1. Meld u aan bij een computer met een account om toegang tot de Windows-beheerprogramma's en open een nieuwe Microsoft Management Console (MMC).
2. Selecteer **bestand > toevoegen of verwijderen-module**.
3. Selecteer **Active Directory: gebruikers en Computers** > **AzureStack.local** > **gebruikers**.
4. Selecteer **actie** > **nieuwe** > **gebruiker**.
5. Nieuw object – gebruiker, bieden gebruikersgegevens. Selecteer **Volgende**.
6. Geef en bevestig een wachtwoord.
7. Selecteer **volgende** voor het voltooien van de waarden. Selecteer **voltooien** om de gebruiker te maken.


## <a name="next-steps"></a>Volgende stappen
[Service-principals maken](azure-stack-create-service-principals.md)
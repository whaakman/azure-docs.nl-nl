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
ms.date: 10/15/2018
ms.author: patricka
ms.reviewer: unknown
ms.openlocfilehash: ee571ec14a93653b524401d1d304021178ecd794
ms.sourcegitcommit: 818d3e89821d101406c3fe68e0e6efa8907072e7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2019
ms.locfileid: "54120809"
---
# <a name="add-azure-stack-users-in-ad-fs"></a>Azure Stack-gebruikers toevoegen in AD FS
U kunt de **Active Directory: gebruikers en Computers** module Extra gebruikers toevoegen aan een Azure Stack-omgeving gebruik te maken van AD FS als de id-provider.

## <a name="add-windows-server-active-directory-users"></a>Windows Server Active Directory-gebruikers toevoegen
> [!TIP]
> In dit voorbeeld gebruikt de standaard azurestack.local ASDK active directory. 

1.  Meld u aan bij een computer met een account om toegang tot de Windows-beheerprogramma's en open een nieuwe Microsoft Management Console (MMC).
2.  Klik op **bestand > toevoegen of verwijderen-module**.
3.  Selecteer **Active Directory: gebruikers en Computers** > **AzureStack.local** > **gebruikers**.
4.  Klik op **actie** > **nieuwe** > **gebruiker**.
5.  Typ in het nieuw Object – gebruiker venster, en een wachtwoord te bevestigen
6.  Klik op **volgende** om te voltooien van de waarden en klik op Voltooien om de gebruiker te maken.


## <a name="next-steps"></a>Volgende stappen
[Service-principals maken](azure-stack-create-service-principals.md)
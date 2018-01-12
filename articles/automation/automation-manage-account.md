---
title: Azure Automation-account beheren | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de configuratie van uw Automation-account kunt beheren, met onder andere aandacht voor certificaatvernieuwing, verwijdering en onjuiste configuratie.
services: automation
documentationcenter: 
author: georgewallace
manager: carmonm
editor: 
ms.assetid: 
ms.service: automation
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/13/2017
ms.author: magoedte
ms.openlocfilehash: 56216f16ba3730d1488e45c0e7a81e87dbad6410
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="manage-azure-automation-account"></a>Azure Automation-account beheren
Op een bepaald moment voordat uw Automation-account verloopt, moet u het certificaat vernieuwen. Als u denkt dat het Uitvoeren als-account is aangetast, kunt u het verwijderen en opnieuw maken. In deze sectie wordt besproken hoe u deze bewerkingen uitvoert.

## <a name="self-signed-certificate-renewal"></a>Zelfondertekend certificaat vernieuwen
Het zelfondertekende certificaat dat u voor het Uitvoeren als-account hebt gemaakt, verloopt één jaar na de aanmaakdatum. U kunt het certificaat op elk gewenst moment vernieuwen voordat het verloopt. Als u het certificaat vernieuwt, blijft het huidige geldige certificaat behouden om ervoor te zorgen dat eventuele runbooks die nog in de wachtrij staan of nog actief zijn, en die worden geverifieerd met het Uitvoeren als-account, niet negatief worden beïnvloed. Het certificaat blijft geldig tot de vervaldatum.

> [!NOTE]
> Als u het Uitvoeren als-account voor Automation hebt geconfigureerd om een certificaat te gebruiken dat is uitgegeven door de certificeringsinstantie van uw bedrijf, en u deze optie gebruikt, wordt het bedrijfscertificaat vervangen door een zelfondertekend certificaat.

Ga als volgt te werk om het certificaat te vernieuwen:

1. Open in Azure Portal het Automation-account.

2. Selecteer in het **Automation-account** 
3. in het deelvenster **Eigenschappen van account** onder **Accountinstellingen** de optie **Uitvoeren als-accounts**.

    ![Eigenschappendeelvenster voor Automation-account](media/automation-manage-account/automation-account-properties-pane.png)
3. Selecteer op de pagina **Uitvoeren als-accounts** het Uitvoeren als- of klassieke Uitvoeren als-account waarvoor u het certificaat wilt vernieuwen.

4. Klik in het deelvenster **Eigenschappen** voor het geselecteerde account op **Certificaat vernieuwen**.

    ![Certificaat vernieuwen voor Uitvoeren als-account](media/automation-manage-account/automation-account-renew-runas-certificate.png)

5. Terwijl het certificaat wordt gemaakt, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

## <a name="delete-a-run-as-or-classic-run-as-account"></a>Een Uitvoeren als- of klassiek Uitvoeren als-account verwijderen
In dit gedeelte wordt beschreven hoe u uw Uitvoeren als- of klassieke Uitvoeren als-account kunt verwijderen en opnieuw kunt maken. Als u deze actie uitvoert, blijft het Automation-account behouden. Nadat u het Uitvoeren als- of klassieke Uitvoeren als-account hebt verwijderd, kunt u het opnieuw maken in Azure Portal.

1. Open in Azure Portal het Automation-account.

2. Selecteer op de pagina **Automation-account** de optie **Uitvoeren als-accounts**.

3. Selecteer op de pagina **Uitvoeren als-accounts** het Uitvoeren als- of klassieke Uitvoeren als-account dat u wilt verwijderen. Klik vervolgens in het deelvenster **Eigenschappen** voor het geselecteerde account op **Verwijderen**.

 ![Uitvoeren als-account verwijderen](media/automation-manage-account/automation-account-delete-runas.png)

4. Terwijl het account wordt verwijderd, kunt u in het menu onder **Meldingen** de voortgang hiervan volgen.

5. Nadat het account is verwijderd, kunt u het opnieuw maken door op de eigenschappenpagina **Uitvoeren als-accounts** de optie **Azure Uitvoeren als-account** te selecteren.

 ![Het Automation uitvoeren als-account opnieuw maken](media/automation-manage-account/automation-account-create-runas.png)

## <a name="misconfiguration"></a>Onjuiste configuratie
Bepaalde configuratie-items die nodig zijn voor het juist functioneren van het Uitvoeren als- of klassieke Uitvoeren als-account, zijn mogelijk verwijderd of niet juist gemaakt tijdens de initiële configuratie. Dit zijn onder meer de volgende items:

* Certificaatasset
* Verbindingsasset
* Uitvoeren als-account is verwijderd uit de rol Inzender
* Service-principal of toepassing in Azure AD

In het voorgaande en andere gevallen van onjuiste configuratie detecteert het Automation-account de wijzigingen en geeft het de status *Onvolledig* weer op de eigenschappenpagina **Uitvoeren als-accounts** voor het account.

![Onvolledige Uitvoeren als-configuratiestatus](media/automation-manage-account/automation-account-runas-incomplete-config.png)

Als u het Uitvoeren als-account selecteert, wordt het volgende foutbericht weergegeven in het deelvenster **Eigenschappen** van het account:

![Waarschuwingsbericht Onvolledige Uitvoeren als-configuratie](media/automation-manage-account/automation-account-runas-incomplete-config-msg.png).

U kunt deze problemen met het Uitvoeren als-account snel oplossen door het account te verwijderen en opnieuw te maken.

## <a name="next-steps"></a>Volgende stappen
* Zie [Application Objects and Service Principal Objects](../active-directory/active-directory-application-objects.md) (Toepassingsobjecten en service-principalobjecten) voor meer informatie over service-principals.
* Zie [Op rollen gebaseerd toegangsbeheer in Azure Automation](automation-role-based-access-control.md) voor meer informatie over het op rollen gebaseerd toegangsbeheer in Azure Automation.
* Voor meer informatie over certificaten en Azure-services raadpleegt u [Certificates overview for Azure Cloud Services](../cloud-services/cloud-services-certs-create.md) (Overzicht van certificaten voor Azure Cloud Services).
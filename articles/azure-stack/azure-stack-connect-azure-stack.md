<properties
    pageTitle="Verbinding maken met Microsoft Azure Stack POC | Microsoft Azure"
    description="Meer informatie over het maken van verbinding met de Azure Stack POC-portal als servicebeheerder of tenant."
    services="azure-stack"
    documentationCenter=""
    authors="ErikjeMS"
    manager="byronr"
    editor=""/>

<tags
    ms.service="azure-stack"
    ms.workload="na"
    ms.tgt_pltfrm="na"
    ms.devlang="na"
    ms.topic="get-started-article"
    ms.date="08/01/2016"
    ms.author="erikje"/>


# Aanmelden bij de virtuele machine van Azure Stack POC

U kunt u aanmelden bij de virtuele machine van Azure Stack POC als

- [**servicebeheerder**](#log-in-as-a-service-administrator) om resourceproviders, tenantaanbiedingen, plannen, services, quota en prijzen te beheren.

of

- [**tenant**](#log-in-as-a-tenant) om services waarop u bent geabonneerd, in te richten, te controleren en te beheren, zoals web-apps, opslag en virtuele machines.

## Aanmelden als servicebeheerder

1.  Meld u aan bij de fysieke computer van Azure Stack POC.

2.  Dubbelklik op het bureaubladpictogram **ClientVM.AzureStack.local.rdp** om een verbinding met extern bureaublad naar de virtuele machine van de client te openen.
 
    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)
    
    Hiervoor wordt automatisch het AzureStack\\AzureStackUser-account gebruikt dat is gemaakt met het implementatiescript. Gebruik het beheerderswachtwoord dat u in stap 5 van het scriptproces hebt opgegeven, bij de prompt **Voer het wachtwoord voor de ingebouwde beheerder in**.

3.  Dubbelklik op het bureaublad ClientVM.AzureStack.local op het pictogram voor de **Microsoft Azure Stack POC-portal** (https://portal.azurestack.local/) om de [portal](azure-stack-key-features.md#portal) te openen.

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Meld u aan met het servicebeheerdersaccount.

## Aanmelden als tenant

Een servicebeheerder kan zich aanmelden als tenant om de plannen, aanbiedingen en abonnementen te testen die zijn of haar tenants mogelijk gebruiken.
Als u nog geen account hebt, kunt u [een tenantaccount maken](azure-stack-add-new-user-aad.md) voordat u zich aanmeldt.

1.  Meld u aan bij de fysieke computer van Azure Stack.

2.  Dubbelklik op het bureaubladpictogram **ClientVM.AzureStack.local.rdp** om een verbinding met extern bureaublad naar de virtuele machine van de client te openen. 

    ![](media/azure-stack-connect-azure-stack/clientvmazurestacklocalicon.png)

    Hiervoor wordt automatisch het AzureStack\\AzureStackUser-account gebruikt dat is gemaakt met het implementatiescript. Gebruik het beheerderswachtwoord dat u in stap 5 van het scriptproces hebt opgegeven, bij de prompt **Voer het wachtwoord voor de ingebouwde beheerder in**.

3.  Dubbelklik op het bureaublad ClientVM.AzureStack.local op het pictogram voor de **Microsoft Azure Stack POC-portal** (https://portal.azurestack.local/) om de [portal](azure-stack-key-features.md#portal) te openen.

    ![](media/azure-stack-connect-azure-stack/microsoftazurestackpocprtalicon.png)

4.  Meld u aan met een tenantaccount.

RDP kan beperken hoeveel gebruikers toegang hebben tot de fysieke Microsoft Azure POC-host. Zie [Enable multiple concurrent user connections](azure-stack-enable-multiple-concurrent-users.md) (Meerdere gelijktijdige gebruikersverbindingen inschakelen) om meerdere gebruikers in te schakelen.

## Volgende stappen

[Eerste taken](azure-stack-first-scenarios.md)



<!--HONumber=Sep16_HO3-->



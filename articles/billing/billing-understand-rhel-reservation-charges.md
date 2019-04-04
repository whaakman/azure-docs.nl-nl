---
title: 'De reserveringskorting plan Red Hat-en-gebruik: Azure | Microsoft Docs'
description: Meer informatie over hoe Red Hat plan kortingen zijn toegepast op Red Hat-software op virtuele machines.
services: billing
documentationcenter: ''
author: yashesvi
manager: yashar
editor: ''
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/22/2019
ms.author: cwatson
ms.openlocfilehash: fe0d0f0baa2b3d1c08e871541dce1511e00f7f87
ms.sourcegitcommit: f093430589bfc47721b2dc21a0662f8513c77db1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/04/2019
ms.locfileid: "58917053"
---
# <a name="understand-how-the-red-hat-linux-enterprise-software-reservation-plan-discount-is-applied-for-azure"></a>Begrijpen hoe de Red Hat Linux Enterprise-hulpprogramma voor het abonnement van de software-reserveringskorting wordt toegepast voor Azure

Nadat u een Red Hat Linux-abonnement koopt, wordt de korting wordt automatisch toegepast op geïmplementeerde Red Hat virtual machines (VM's) die overeenkomen met de reservering. Een Red Hat Linux-plan dekt de kosten van het uitvoeren van de Red Hat-software op een Azure-VM.

Als u wilt het juiste Red Hat Linux-abonnement kopen, moet u begrijpen welke Red Hat VM's uitvoeren en het aantal vcpu's op deze virtuele machines. Gebruik de volgende secties voor het identificeren van uw CSV-bestand van het gebruik van wat van plan bent om te kopen.

## <a name="discount-applies-to-different-vm-sizes"></a>Korting is van toepassing op andere VM-grootten

Zoals gereserveerde VM-instanties bieden Red Hat plan aankopen instantieflexibiliteit grootte. Dit betekent dat uw korting geldt zelfs wanneer u een virtuele machine met een aantal verschillende vCPU's implementeren. De korting is van toepassing op andere VM-grootten in de software-plan.

De korting is afhankelijk van de hoogte-breedteverhouding die worden vermeld in de volgende tabellen. De verhouding tussen vergelijkt de relatieve grootte van elke meter in die groep. De verhouding is afhankelijk van de VM-vcpu's. Gebruik de verhouding-waarde voor het berekenen van het aantal VM-exemplaren krijgt u de Red Hat Linux-plan-korting.

Bijvoorbeeld, als u een abonnement voor Red Hat Linux Enterprise Server voor een virtuele machine met 3 of 4 vcpu's kopen, is de verhouding voor deze reservering 2. De korting dekt de kosten van de software Red Hat voor:

- 2 virtuele machines geïmplementeerd met 1 of 2 vcpu's,
- 1 virtuele machine geïmplementeerd met 3 of 4 Vcpu,
- of 0.77 of over 77% van een virtuele machine met 5 of meer vcpu's.

De verhouding voor 5 of meer vcpu's is 2.6. Dus een reservering voor Red Hat met een virtuele machine met 5 of meer vcpu's bevat informatie over een alleen-gedeelte van de softwarekosten voor, die ongeveer 77% is.

## <a name="understand-red-hat-vm-usage-before-you-buy"></a>Red Hat VM-gebruik begrijpen voordat u kopen

De volgende tabellen worden de software-abonnement die kunt u een reservering voor kopen, hun bijbehorende gebruik meters en de verhouding voor elk weergegeven.

### <a name="red-hat-enterprise-linux"></a>Red Hat Enterprise Linux

Namen van de Azure portal marketplace:

- Red Hat Enterprise Linux 6.7
- Red Hat Enterprise Linux 6.8
- Red Hat Enterprise Linux 6.9
- Red Hat Enterprise Linux 6.10
- Red Hat Enterprise Linux 7.2
- Red Hat Enterprise Linux 7.3
- Red Hat Enterprise Linux 7.4
- Red Hat Enterprise Linux 7.5
- Red Hat Enterprise Linux 7.6
- Red Hat Enterprise Linux 7 (latest lvm)

|Red Hat VM | Meter-id| Verhouding| Voorbeeld van de VM-grootte|
| -------| ------------------------| --- |--- |
|1-4 vCPU, VM-licentie|077a07bb-20f8-4bc6-b596-ab7211a1e247|1|D4s_v3|
|1-4 vCPU, VM-licentie|2f96d035-3bac-46d6-b2bc-c6daa0938536|1|D4s_v3|
|1-4 vCPU, VM-licentie|4831a7b4-bdd4-48a2-8e95-18d053971ede|1|D4s_v3|
|5 + vCPU VM-licentie|291b2cbc-6c34-4e2b-a4e4-1ff8c106f672|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|3b6661c4-03dd-45e7-88c9-512fcb7906d5|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|037eddc0-fedd-4d73-b5d8-92fba9edb831|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|432cdeee-4034-4ddf-9ba4-9250a19b0d5f|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|794dcb90-0793-43e6-9909-70d29974e56d|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|86b5b0b4-3c19-4720-82e9-874f8c58b48e|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|86c35ec3-0a48-426a-9625-22d80e6ea55b|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|8b698c7a-47f1-4cba-8ae1-9853d5ad562d|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|a4daffb4-96f4-4fc5-b1e6-fd3a2cf3595e|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|a838cfb1-0bd3-4965-84f0-663f49afc2e2|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|99aed7b9-a0a9-4783-b90c-be7c2f3c7e30|2.166666667|D8s_v3|
|5 + vCPU VM-licentie|d09f877e-03b4-48b2-b11a-782b965cff19|2.166666667|D8s_v3|
|44 vCPU VM-licentie|6f44ae85-a70e-44be-83ec-153a0bc23979|2.166666667||
|60 vCPU VM-licentie|b9edcc5b-a429-4778-bc5a-82e7fa07fe55|2.166666667||

### <a name="red-hat-enterprise-linux-for-sap-with-ha"></a>Red Hat Enterprise Linux voor SAP met HA

Naam van de Azure portal marketplace:

|Red Hat VM | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- | --- | ------------------------| --- | --- |
|1-4 vCPU, VM-licentie |4d902611-eed7-4060-a33e-3c7fdbac6406|1|D4s_v3|
|5 + vCPU VM-licentie|6dfb482b-23ea-487f-810c-e66360f025de|2.333333333|D8s_v3|

### <a name="red-hat-enterprise-linux-with-ha"></a>Red Hat Enterprise Linux met HA

Namen van de Azure portal marketplace:

|Red Hat VM | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- |------------------------| --- | --- |
|1-4 vCPU, VM-licentie|e9711132-d9d9-450c-8203-25cfc4bce8de|1|D4s_v3|
|5 + vCPU VM-licentie|93954aa4-b55f-4b7b-844d-a119d6bf3c4e|2|D8s_v3|

### <a name="rhel-for-sap-business-applications"></a>RHEL for SAP Business Applications

Namen van de Azure portal marketplace:

- Red Hat Enterprise Linux 6,8 voor SAP Business Apps
- Red Hat Enterprise Linux 7.3 voor SAP Business Apps
- Red Hat Enterprise Linux 7.4 voor SAP
- Red Hat Enterprise Linux 7.5 for SAP

|Red Hat VM | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- |------------------------| --- |--- |
|1 vCPU VM-licentie|25889e91-c740-42ac-bc52-6b8f73b98575|1|D2s_v3|
|2 vCPU VM-licentie|2a0c92c8-23a7-4dc9-a39c-c4a73a85b5da|1|D2s_v3|
|4 vCPU VM-licentie|875898d3-3639-423c-82c1-38846281b7e8|1|D4s_v3|
|6 vCPU VM-licentie|69a140fa-e08e-415c-85f2-48158e4c73a0|2.166666667||
|8 vCPU VM-licentie|777a5a74-22d6-48c9-9705-ac38fe05a278|2.166666667|D8s_v3|
|12 vCPU VM-licentie|d6b8917a-5127-497a-9f48-1e959df98812|2.166666667||
|16 vCPU VM-licentie|03667e82-e009-425a-83f7-8ebddbca5af4|2.166666667|D16s_v3|
|20 vCPU VM-licentie|bbd65e5b-35f1-42be-b86d-6625fbc1f1a4|2.166666667||
|24 vCPU VM-licentie|c2c07d3e-a7d0-400b-8832-b532bfd0be25|2.166666667|ND24s|
|32 vCPU VM-licentie|633d1494-5ec1-46f0-a742-eaf58eeaec7e|2.166666667|D32s_v3|
|40 vCPU VM-licentie|737142c3-8e4f-4fc1-aa41-05b1661edff8|2.166666667||
|44 vCPU VM-licentie|722bda73-a8c8-4d04-b96b-541f0bb6c0c4|2.166666667||
|60 vCPU VM-licentie|a22bb342-ba9a-4529-a178-39a92ce770b6|2.166666667||
|64 vCPU VM-licentie|d37c8e17-e5f2-4060-881b-080dd4a8c4ce|2.166666667|64s_v3|
|72 vCPU VM-licentie|14341b96-e92c-4dca-ba66-322c88a79aa6|2.166666667|F72s_v2|
|96 vCPU VM-licentie|8b2e5cb8-0362-4cbf-a30a-115e8d6dbc49|2.166666667||
|128 vCPU VM-licentie|9b198a68-974a-47a7-9013-49169ac0f2e9|2.166666667| M128ms|

### <a name="rhel-for-sap-hana"></a>RHEL for SAP HANA

Namen van de Azure portal marketplace:

- Red Hat Enterprise Linux 6.7 for SAP HANA
- Red Hat Enterprise Linux 7.2 voor SAP HANA
- Red Hat Enterprise Linux 7.3 voor SAP HANA

|Red Hat VM | Meter-id | Verhouding|Voorbeeld van de VM-grootte|
| ------- |------------------------| --- |--- |
|1 vCPU VM-licentie|be0a59d1-eed7-47ec-becd-453267753793|1|D2s_v3|
|2 vCPU VM-licentie|3b97c9f5-f5d5-4fd3-a421-b78fca32a656|1|D2s_v3|
|4 vCPU VM-licentie|b39feb58-57bf-40f2-8193-f4fe9ac3dda3|1|D4s_v3|
|6 vCPU VM-licentie|a5963812-0f5a-4053-8ace-2b5babd15ed8|2.166666667||
|8 vCPU VM-licentie|5460ab4d-ce9a-46af-8ad5-ca5e53d715b5|2.166666667|D8s_v3|
|12 vCPU VM-licentie|0e3bc72d-a888-4bcf-8437-119f763a3215|2.166666667||
|16 vCPU VM-licentie|b40e95d8-3176-42f0-967c-497785c031b2|2.166666667|D16s_v3|
|20 vCPU VM-licentie|81f34277-499d-40a3-a634-99adc08e2d45|2.166666667||
|24 vCPU VM-licentie|e03f1906-d35d-4084-b2cd-63281869c8ee|2.166666667|ND24s|
|32 vCPU VM-licentie|0a58c082-ceb8-4327-9b64-887c30dddb23|2.166666667|D32s_v3|
|40 vCPU VM-licentie|a14225c0-04e6-4669-974f-e2ddd61a9c5b|2.166666667||
|44 vCPU VM-licentie|378b8125-d8a5-4e09-99bc-c1462534ffb0|2.166666667||
|60 vCPU VM-licentie|5d7db11a-54e9-404e-aaa8-509fac7c0638|2.166666667||
|64 vCPU VM-licentie|3c8157b2-a57d-45ce-ba02-bd86e9209795|2.166666667|64s_v3|
|72 vCPU VM-licentie|5e87a3ee-7afb-4040-b8d9-b109ddb38f31|2.166666667|F72s_v2|
|96 vCPU VM-licentie|b13895fc-0d06-4de9-b860-627c471cd247|2.166666667||
|128 vCPU VM-licentie|6e67ac0b-19d3-4289-96df-05d0093d4b3b|2.166666667| M128ms|

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over reserveringen, de volgende artikelen:

- [Wat zijn de reserveringen voor Azure](billing-save-compute-costs-reservations.md)
- [Betaal vooruit voor Red Hat software plannen met Azure-reserveringen](../virtual-machines/linux/prepay-rhel-software-charges.md)
- [Vooruitbetalen voor virtuele Machines met Azure gereserveerde VM-instanties](../virtual-machines/windows/prepay-reserved-vm-instances.md)
- [Reserveringen voor Azure beheren](billing-manage-reserved-vm-instance.md)
- [Gebruik van de reservering voor uw abonnement op gebruiksbasis begrijpen](billing-understand-reserved-instance-usage.md)
- [Inzicht in gebruik van de reservering voor uw Enterprise-inschrijving](billing-understand-reserved-instance-usage-ea.md)

## <a name="need-help-contact-us"></a>Hulp nodig? Contact opnemen

Als u vragen hebt of hulp nodig hebt, [Maak een ondersteuningsaanvraag](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

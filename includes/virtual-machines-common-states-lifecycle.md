---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: shandilvarun
ms.service: virtual-machines
ms.topic: include
ms.date: 08/09/2018
ms.author: vashan, cynthn, rajsqr
ms.custom: include file
ms.openlocfilehash: b48de6a6eeed997fe162cabe4d57e6770e016971
ms.sourcegitcommit: 30d23a9d270e10bb87b6bfc13e789b9de300dc6b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/08/2019
ms.locfileid: "54122799"
---
Virtuele Azure-Machines (VM's) gaat via verschillende statussen die kunnen worden onderverdeeld in *inrichting* en *power* Staten. Het doel van dit artikel is om te beschrijven deze statussen en specifiek markeren wanneer klanten zijn bijvoorbeeld gebruik gefactureerd. 

## <a name="power-states"></a>Energiestatussen

De energiestatus vertegenwoordigt de laatst bekende status van de virtuele machine.

![Diagram van VM power status](./media/virtual-machines-common-states-lifecycle/vm-power-states.png)

<br>
De volgende tabel bevat een beschrijving van de status van elk exemplaar en geeft aan of deze in rekening voor gebruik van instanties of niet gebracht wordt.

<table>
<tr>
<th>
Status
</th>
<th>
Description
</th>
<th>
Exemplaar gebruik facturering
</th>
</tr>
<tr>
<td>
<p><b>Starten</b></p>
</td>
<td>
<p>Virtuele machine wordt gestart.</p>
<code>"statuses": [<br>
   {<br>
      "code": "PowerState/starting",<br>
       "level": "Info",<br>
        "displayStatus": "VM starting"<br>
    }<br>
    ]</code><br>
</td>
<td>
<p><b>Niet in rekening gebracht</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Wordt uitgevoerd</b></p>
</td>
<td>
<p>Normale werkende status voor een virtuele machine</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/running",<br>
 "level": "Info",<br>
 "displayStatus": "VM running"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Kosten in rekening gebracht</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Stoppen</b></p>
</td>
<td>
<p>Dit is een tijdelijke situatie. Wanneer dit is voltooid, wordt deze weergegeven als **gestopt**.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopping",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopping"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Kosten in rekening gebracht</b></p>
</td>
</tr>
<tr>
<td>
<p><b>Gestopt</b></p>
</td>
<td>
<p>De virtuele machine is afgesloten omlaag van binnen het gastbesturingssysteem te installeren of met behulp van de APIs PowerOff.</p>
<p>Hardware nog is toegewezen aan de virtuele machine en deze blijft op de host. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/stopped",<br>
 "level": "Info",<br>
 "displayStatus": "VM stopped"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Kosten in rekening gebracht&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>De toewijzing ongedaan maken</b></p>
</td>
<td>
<p>Overgangsstatus. Wanneer dit is voltooid, de virtuele machine wordt weergegeven als **Deallocated**.</p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocating",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocating"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Niet in rekening gebracht&#42;</b></p>
</td>
</tr>
<tr>
<td>
<p><b>De toewijzing ongedaan gemaakt</b></p>
</td>
<td>
<p>De virtuele machine is gestopt en verwijderd uit de host. </p>
<code>"statuses": [<br>
 {<br>
 "code": "PowerState/deallocated",<br>
 "level": "Info",<br>
 "displayStatus": "VM deallocated"<br>
 }<br>
 ]</code><br>
</td>
<td>
<p><b>Niet in rekening gebracht</b></p>
</td>
</tr>
</tbody>
</table>


&#42;Sommige Azure-resources, zoals schijven en netwerken, worden kosten in rekening gebracht. Licenties voor software op het exemplaar kunnen geen kosten in rekening gebracht.

## <a name="provisioning-states"></a>Inrichtingsstatussen

Een Inrichtingsstatus is de status van een gebruiker geïnitieerde, controlelaag bewerking op de virtuele machine. Deze statussen zijn gescheiden van de energiestatus van een virtuele machine.

- **Maak** – VM wordt gemaakt.

- **Update** : updates van het model voor een bestaande virtuele machine. Een niet-model gewijzigd in VM zoals Start/Restart vallen ook onder bijwerken.

- **Verwijder** – VM verwijderen.

- **Toewijzing** : wanneer een virtuele machine wordt gestopt en verwijderd uit de host is. Toewijzing ongedaan maken van een virtuele machine wordt beschouwd als een update, zodat het inrichtingsproces Staten met betrekking tot het bijwerken wordt weergegeven.



Hier volgen de Staten overgangs bewerking nadat het platform heeft een actie van de gebruiker geïnitieerde geaccepteerd:

<br>

<table>
<tbody>
<tr>
<td width="162">
<p><b>Statussen</b></p>
</td>
<td width="366">
<p>Description</p>
</td>
</tr>
<tr>
<td width="162">
<p><b>Het maken van</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating",<br>
 "level": "Info",<br>
 "displayStatus": "Creating"<br>
 }</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Bijwerken</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/updating",<br>
 "level": "Info",<br>
 "displayStatus": "Updating"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>Verwijderen</b></p>
</td>
<td width="366">
<code>"statuses": [<br>
 {<br>
 "code": "ProvisioningState/deleting",<br>
 "level": "Info",<br>
 "displayStatus": "Deleting"<br>
 }<br>
 ]</code><br>
</td>
</tr>
<tr>
<td width="162">
<p><b>OS-inrichting Staten</b></p>
</td>
<td width="366">
<p>Als een virtuele machine is gemaakt met een installatiekopie van het besturingssysteem en niet met een gespecialiseerde installatiekopie, kunnen volgende subtoestanden worden waargenomen:</p>
<p>1. <b>OSProvisioningInprogress</b> &ndash; de virtuele machine wordt uitgevoerd, en de installatie van de Gast-OS wordt uitgevoerd. <p /> 
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningInprogress",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning In progress"<br>
 }<br>
]</code><br>
<p>2. <b>OSProvisioningComplete</b> &ndash; tijdelijke status. De virtuele machine snel verandert in **succes** , tenzij er geen extensies moeten worden geïnstalleerd. Installeren van extensies kan even duren. <br />
<code> "statuses": [<br>
 {<br>
 "code": "ProvisioningState/creating/OSProvisioningComplete",<br>
 "level": "Info",<br>
 "displayStatus": "OS Provisioning Complete"<br>
 }<br>
]</code><br>
<p><b>Opmerking</b>: Inrichting van het besturingssysteem kunt overgaan op **mislukt** als er een OS-fouten of het besturingssysteem niet wordt geïnstalleerd in de tijd. Klanten worden gefactureerd voor de geïmplementeerde virtuele machine op de infrastructuur.</p>
</td>
</tr>
</table>


Nadat de bewerking voltooid is, wordt de virtuele machine verandert in een van de volgende statussen:

- **Geslaagd** – de gebruiker geïnitieerde acties zijn voltooid.

    ```
 "statuses": [ 
 {
     "code": "ProvisioningState/succeeded",
     "level": "Info",
     "displayStatus": "Provisioning succeeded",
     "time": "time"
 }
 ]
    ```

 

- **Kan geen** – Hiermee geeft u een mislukte bewerking. Raadpleeg de foutcodes voor meer informatie en mogelijke oplossingen.

    ```
 "statuses": [
    {
      "code": "ProvisioningState/failed/InternalOperationError",
      "level": "Error",
      "displayStatus": "Provisioning failed",
      "message": "Operation abandoned due to internal error. Please try again later.",
      "time": "time"
    }
    ]
    ```



## <a name="vm-instance-view"></a>Instantieweergave van virtuele machine

Instantieweergave van de API bevat VM-uitvoeringsstatus van informatie. Zie voor meer informatie de [Virtual Machines - Instantieweergave](https://docs.microsoft.com/rest/api/compute/virtualmachines/instanceview) API-documentatie.

Azure Resources explorer biedt een eenvoudige gebruikersinterface voor het weergeven van de virtuele machine actief is: [Resource Explorer](https://resources.azure.com/).

Inrichting statussen zijn zichtbaar voor VM-eigenschappen en instantieweergave. Energiestatussen zijn beschikbaar in de instantieweergave van virtuele machine. 


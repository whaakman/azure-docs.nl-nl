
## <a name="azure-backup"></a>Azure Backup

Voor back-ups van virtuele Azure-machines productieworkloads wordt uitgevoerd, gebruikt u Azure Backup. Azure Backup biedt ondersteuning voor toepassingsconsistente back-ups voor zowel Windows als een virtuele Linux-machines. Azure Backup maakt herstelpunten die zijn opgeslagen in een geografisch redundante recovery kluizen. Wanneer u vanaf een herstelpunt herstelt, kunt u de hele virtuele machine of alleen specifieke bestanden kunt herstellen. 

Zie voor een eenvoudige, praktische Inleiding tot Azure Backup voor Azure Virtual machines, de zelfstudie 'Back-up van virtuele machines in Azure' voor [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) of [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).

Zie voor meer informatie over hoe Azure Backup werkt [plannen van uw back-infrastructuur van de virtuele machine in Azure](../articles/backup/backup-azure-vms-introduction.md)


## <a name="azure-site-recovery"></a>Azure Site Recovery

Azure Site Recovery beveiligt uw virtuele machines van een noodgeval-scenario wanneer een hele regio optreedt in een storing veroorzaakt door grote natuurramp of wijdverbreid service wordt onderbroken. U kunt Azure Site Recovery voor uw virtuele machines configureren zodat u uw toepassing met één klik in kwestie van minuten kan herstellen. U kunt repliceren naar een Azure-regio van uw keuze, is niet beperkt tot gekoppelde regio's. 

Herstel na noodgevallen zoomt kunt u uitvoeren met testfailovers op aanvraag, zonder de productie-workloads of lopende replicatie te beïnvloeden. Herstelplannen indelen failover en failback van de gehele toepassing uitgevoerd op meerdere virtuele machines maken. De herstelfunctie van plan is geïntegreerd met Azure automation-runbooks.

U kunt aan de slag door [uw virtuele machines repliceren](https://aka.ms/a2a-getting-started). 

## <a name="managed-snapshots"></a>Beheerde momentopnamen 

Ontwikkel- en testomgevingen geeft momentopnamen een snel en eenvoudig optie voor back-ups van virtuele machines die gebruikmaken van schijven worden beheerd. Een beheerde momentopname is een alleen-lezen kopie van een beheerde schijf. Momentopnamen onafhankelijk van de bronschijf bestaan en kunnen worden gebruikt om nieuwe beheerde schijven voor het opnieuw opbouwen van een virtuele machine te maken. Ze worden gefactureerd op basis van het gebruikte gedeelte van de schijf. Als u een momentopname van een beheerde schijf met ingerichte capaciteit van 64 GB en grootte van 10 GB gegevens die u gebruikt maakt, wordt bijvoorbeeld momentopname gefactureerd alleen voor de gebruikte gegevensgrootte van 10 GB.  

Zie voor meer informatie over het maken van momentopnamen:

* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)



## <a name="next-steps"></a>Volgende stappen
U kunt Azure Backup aan de hand van de 'Back-up Windows virtuele machines zelfstudie' uitproberen voor [Linux](../articles/virtual-machines/linux/tutorial-backup-vms.md) of [Windows](../articles/virtual-machines/windows/tutorial-backup-vms.md).

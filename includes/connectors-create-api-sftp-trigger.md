We gaan een trigger toevoegen.

1. Voer *sftp* in het zoekvak op de ontwerpfunctie van logic apps selecteert u vervolgens de **SFTP - wanneer een bestand wordt toegevoegd of gewijzigd** trigger   
   ![SFTP triggerafbeelding 1](./media/connectors-create-api-sftp/trigger-1.png)  
2. De **wanneer een bestand wordt toegevoegd of gewijzigd** besturingselement wordt geopend  
   ![SFTP triggerafbeelding 2](./media/connectors-create-api-sftp/trigger-2.png)  
3. Selecteer de **...**  zich aan de rechterkant van het besturingselement. Hiermee opent u het kiezerbesturingselement map  
   ![SFTP triggerafbeelding 3](./media/connectors-create-api-sftp/action-1.png)  
4. Selecteer de **SFTP** de basismap selecteren als de map om te controleren op nieuwe en gewijzigde bestanden. U ziet de hoofdmap wordt nu weergegeven in de **map** besturingselement.  
   ![SFTP triggerafbeelding 4](./media/connectors-create-api-sftp/action-2.png)   

Op dit moment is uw logische app geconfigureerd met een trigger die een uitvoering van de andere triggers en acties in de werkstroom wordt gestart wanneer een bestand wordt gewijzigd of in de specifieke SFTP-map gemaakt. 

> [!NOTE]
> Voor een logische app wilt gebruiken, moet er ten minste één trigger en één actie bevatten. Volg de stappen in de volgende sectie voor een actie toevoegen.  
> 
> 


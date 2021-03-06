---
title: "Erste Schritte bei der Überwachung von Azure SQL-Datenbank | Microsoft-Dokumentation"
description: "Erste Schritte bei der Überwachung von Azure SQL-Datenbank"
services: sql-database
documentationcenter: 
author: giladm
manager: jhubbard
editor: giladm
ms.assetid: 89c2a155-c2fb-4b67-bc19-9b4e03c6d3bc
ms.service: sql-database
ms.custom: security
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/07/2017
ms.author: giladm
ms.translationtype: Human Translation
ms.sourcegitcommit: 6adaf7026d455210db4d7ce6e7111d13c2b75374
ms.openlocfilehash: f4324a59b5fa4c2e1ab5b1d7fc7e5fe986ea80f8
ms.contentlocale: de-de
ms.lasthandoff: 06/22/2017


---
# <a name="get-started-with-sql-database-auditing"></a>Erste Schritte bei der Überwachung von SQL-Datenbank
Die SQL-Datenbank-Überprüfung in Azure verfolgt Datenbankereignisse und schreibt diese in ein Überwachungsprotokoll in Ihrem Azure-Speicherkonto. Die Überwachung ermöglicht außerdem Folgendes:

* Sie kann Ihnen dabei helfen, die gesetzlichen Bestimmungen einzuhalten, die Datenbankaktivität zu verstehen und Einblicke in Abweichungen und Anomalien zu erhalten, die auf geschäftsspezifische Bedenken oder mutmaßliche Sicherheitsverstöße hinweisen können.

* Sie ermöglicht und unterstützt die Einhaltung von Standards, garantiert diese aber nicht. Weitere Informationen zu Azure-Programmen, die die Einhaltung von Standards unterstützen, finden Sie im [Microsoft Azure-Vertrauenscenter](https://azure.microsoft.com/support/trust-center/compliance/).


## <a id="subheading-1"></a>Übersicht über die Überwachung von Azure SQL-Datenbank
Sie können die SQL-Datenbank-Überwachung für folgende Zwecke verwenden:


* **Beibehalten** eines Überwachungspfads von ausgewählten Ereignissen. Sie können Kategorien für zu protokollierende Datenbankaktionen und -ereignisse konfigurieren.
* **Melden** von Datenbankaktivitäten. Sie können vorkonfigurierte Berichte und ein Dashboard verwenden, um schnell mit der Berichterstattung über Aktivitäten und Ereignisse zu beginnen.
* **Analysieren** von Berichten. Sie können nach verdächtigen Ereignissen, ungewöhnliche Aktivitäten und Trends suchen.

Sie können die Überwachung für verschiedene Arten von Ereigniskategorien konfigurieren, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2) erläutert.

Überwachungsprotokolle werden in Ihrem Azure-Abonnement in Azure Blob Storage geschrieben.


## <a id="subheading-8"></a>Definieren von Überwachungsrichtlinien auf Serverebene und auf Datenbankebene

Eine Überwachungsrichtlinie kann für eine spezifische Datenbank oder als Standardserverrichtlinie definiert werden:

* Eine Serverrichtlinie wird auf alle vorhandenen und neu erstellten Datenbanken auf dem Server angewendet.

* Wenn *die Serverblobüberwachung aktiviert ist*, *gilt diese immer für die Datenbank* (d.h. die Datenbank wird überwacht), unabhängig von den Datenbanküberwachungseinstellungen.

* Der Aktivierung der Blobüberwachung in der Datenbank sowie auf dem Server hebt die Einstellungen der Serverblobüberwachung *nicht* auf und ändert sie nicht. Beide Überwachungen existieren nebeneinander. Das heißt, die Datenbank wird zweimal parallel überwacht (einmal anhand der Serverrichtlinie und einmal anhand der Datenbankrichtlinie).

   > [!NOTE]
   > Aktivieren Sie die Serverblobüberwachung und die Datenbankblobüberwachung nicht zusammen, es sei denn:
    > * Sie möchten ein anderes *Speicherkonto* oder eine andere *Beibehaltungsdauer* für eine bestimmte Datenbank verwenden.
    > * Sie möchten andere Ereignistypen oder Kategorien für eine bestimmte Datenbank überwachen als die, die für den Rest der Datenbanken auf diesem Server überwacht werden. Beispielsweise sollen Tabelleneinfügungen nur für eine bestimmte Datenbank überwacht werden.
   > 
   > Andernfalls wird empfohlen, die Blobüberwachung nur auf Serverebene zu aktivieren und die Überwachung auf Datenbankebene für alle Datenbanken deaktiviert zu lassen.


## <a id="subheading-2"></a>Einrichten der Überwachung für Ihre Datenbank
Der folgende Abschnitt beschreibt die Konfiguration der Überwachung über das Azure-Portal.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com).
2. Navigieren Sie zum Blatt mit den **Einstellungen** der SQL-Datenbank/des SQL Server, die bzw. den Sie überwachen möchten. Wählen Sie auf dem Blatt **Einstellungen** die Option **Überwachung und Bedrohungserkennung**.

    <a id="auditing-screenshot"></a>
    ![Navigationsbereich][1]
3. Wenn Sie lieber eine Serverüberwachungsrichtlinie einrichten möchten (die auf alle vorhandenen und neu erstellten Datenbanken auf dem Server angewendet wird), können Sie im Blatt für die Datenbanküberwachung den Link **Servereinstellungen anzeigen** auswählen. Anschließend können Sie die Serverüberwachungseinstellungen anzeigen oder ändern.

    ![Navigationsbereich][2]
4. Wenn Sie die Blobüberwachung auf Datenbankebene aktivieren (zusätzlich zur oder anstelle der Überwachung auf Serverebene), wählen Sie unter **Überwachung**die Option **EIN** und unter **Überwachungstyp**die Option **Blob**.

    Wenn die Serverblobüberwachung aktiviert ist, existiert die konfigurierte Datenbanküberwachung parallel zur Serverblobüberwachung.  

    ![Navigationsbereich][3]
5. Wählen Sie **Speicherdetails**, um das Blatt **Speicherung von Überwachungsprotokollen** zu öffnen. Wählen Sie das Azure-Speicherkonto aus, in dem die Protokolle gespeichert werden sollen, sowie die Aufbewahrungsdauer, nach der die alten Protokolle gelöscht werden. Klicken Sie dann auf **OK**. 
   >[!TIP] 
   >Verwenden Sie für alle überwachten Datenbanken dasselbe Speicherkonto, um die Berichtvorlagen für die Überwachung optimal einzusetzen. 

    <a id="storage-screenshot"></a>
    ![Navigationsbereich][4]
6. Wenn Sie die überwachten Ereignisse anpassen möchten, können Sie PowerShell oder die REST-API verwenden. Weitere Informationen finden Sie im Abschnitt [Automatisierung (PowerShell/REST-API)](#subheading-7).
7. Nachdem Sie Ihre Überwachungseinstellungen konfiguriert haben, können Sie das neue Feature der Bedrohungserkennung aktivieren und die E-Mail-Konten konfigurieren, an die Sicherheitswarnungen gesendet werden sollen. Mit der Bedrohungserkennung können Sie proaktive Warnungen bei anomalen Datenbankaktivitäten erhalten, die auf mögliche Sicherheitsbedrohungen hinweisen können. Weitere Informationen finden Sie unter [Erste Schritte mit der Bedrohungserkennung](sql-database-threat-detection-get-started.md).
8. Klicken Sie auf **Speichern**.





## <a id="subheading-3"></a>Analysieren von Überwachungsprotokollen und -berichten
Überwachungsprotokolle werden in dem Azure-Speicherkonto aggregiert, das Sie während der Einrichtung ausgewählt haben. Sie können Überwachungsprotokolle mithilfe eines Tools wie [Azure-Speicher-Explorer](http://storageexplorer.com/) untersuchen.

Protokolle der Blobüberwachung werden als Sammlung von Blobdateien in einem Container namens **sqldbauditlogs** gespeichert.

Weitere Informationen zur Hierarchie der Speicherordner für Blobüberwachungsprotokolle, zur Benennungskonvention für Blobs sowie zum Protokollformat finden Sie in der [Formatreferenz für Blobüberwachungsprotokolle](https://go.microsoft.com/fwlink/?linkid=829599) (DOCX-Datei zum Herunterladen).

Es gibt verschiedene Methoden zum Anzeigen von Blobüberwachungsprotokollen:

* Verwenden Sie das [Azure-Portal](https://portal.azure.com).  Öffnen Sie die entsprechende Datenbank. Klicken Sie oben auf dem Blatt **Überwachung und Bedrohungserkennung** der Datenbank auf **Überwachungsprotokolle anzeigen**.

    ![Navigationsbereich][7]

    Das Blatt **Überwachungsdatensätze** wird geöffnet, auf dem Sie die Protokolle anzeigen können.

    - Sie können Protokolle bestimmter Daten anzeigen, indem Sie im oberen Bereich des Blatts **Überwachungsdatensätze** auf **Filter** klicken.
    - Sie können zwischen Überwachungsdatensätzen wechseln, die anhand der Server- oder der Datenbankrichtlinienüberwachung erstellt wurden.

       ![Navigationsbereich][8]

* Verwenden Sie die Systemfunktion **sys.fn_get_audit_file** (T-SQL), um die Daten der Überwachungsprotokolle im Tabellenformat zurückzugeben. Weitere Informationen zur Verwendung dieser Funktion finden Sie in der [Dokumentation zu „sys.fn_get_audit_file“](https://docs.microsoft.com/sql/relational-databases/system-functions/sys-fn-get-audit-file-transact-sql).


* Verwenden von **Überwachungsdateien zusammenführen** in SQL Server Management Studio (ab SSMS 17):  
    1. Wählen Sie im SSMS-Menü **Datei** > **Öffnen** > **Überwachungsdateien zusammenführen**.

        ![Navigationsbereich][9]
    2. Das Dialogfeld **Überwachungsdateien hinzufügen** wird geöffnet. Wählen Sie eine der Optionen zum **Hinzufügen**, um festzulegen, ob die Überwachungsdateien von einem lokalen Datenträger zusammengeführt oder aus Azure Storage importieren werden sollen (im zweiten Fall müssen Sie Ihre Azure Storage-Details und Ihren Kontoschlüssel angeben).

    3. Nachdem Sie alle zusammenzuführenden Dateien hinzugefügt haben, klicken Sie auf **OK**, um die Zusammenführung abzuschließen.

    4. Die zusammengeführte Datei wird in SSMS geöffnet. Hier können Sie die Datei anzeigen und analysieren und in eine XEL- oder CSV-Datei oder in eine Tabelle exportieren.

* Verwenden Sie die [Synchronisierungsanwendung](https://github.com/Microsoft/Azure-SQL-DB-auditing-OMS-integration), die wir zuvor erstellt haben. Diese wird in Azure ausgeführt und nutzt öffentliche OMS (Operations Management Suite) Log Analytics-APIs, um SQL-Überwachungsprotokolle in OMS zu verschieben. Die Synchronisierungsanwendung verschiebt SQL-Überwachungsprotokolle für die Nutzung über das OMS Log Analytics-Dashboard in OMS Log Analytics. 

* Verwenden Sie Power BI. Sie können Überwachungsprotokolldateien in Power BI anzeigen und analysieren. Erfahren Sie mehr über [Power BI, und erhalten Sie Zugriff eine herunterladbare Vorlage](https://blogs.msdn.microsoft.com/azuresqldbsupport/2017/05/26/sql-azure-blob-auditing-basic-power-bi-dashboard/).

* Laden Sie Protokolldateien aus Ihrem Azure Storage Blob-Container über das Portal oder mithilfe eines Tools wie [Azure-Speicher-Explorer](http://storageexplorer.com/) herunter.
    * Nachdem Sie die Protokolldatei lokal heruntergeladen haben, können Sie auf die Datei doppelklicken, um die Protokolle in SSMS zu öffnen, anzuzeigen und zu analysieren.
    * Sie können mit Azure-Speicher-Explorer auch mehrere Dateien gleichzeitig herunterladen. Klicken Sie mit der rechten Maustaste auf einen bestimmten Unterordner (z.B. auf einen Unterordner, der alle Protokolldateien eines bestimmten Datums enthält), und wählen Sie zum Speichern in einem lokalen Ordner **Speichern unter** aus.

* Weitere Methoden:
   * Nach dem Herunterladen mehrerer Dateien (oder eines Unterordners, der die Protokolldateien für einen ganzen Tag enthält, wie im vorherigen Listenpunkt beschrieben), können Sie sie lokal zusammenführen, wie in den zuvor aufgeführten Anweisungen für das Zusammenführen von Überwachungsdateien in SSMS beschrieben.

   * Programmgesteuertes Anzeigen von Blobüberwachungsprotokollen:

     * Verwenden Sie die C#-Bibliothek [Leser für erweiterte Ereignisse ](https://blogs.msdn.microsoft.com/extended_events/2011/07/20/introducing-the-extended-events-reader/).
     * Führen Sie [Abfragen von Dateien mit erweiterten Ereignissen](https://sqlscope.wordpress.com/2014/11/15/reading-extended-event-files-using-client-side-tools-only/) mithilfe von PowerShell durch.




## <a id="subheading-5"></a>Produktionsverfahren
<!--The description in this section refers to preceding screen captures.-->

### <a id="subheading-6">Überwachung von georeplizierten Datenbanken</a>
Wenn Sie georeplizierte Datenbanken verwenden, können Sie die Überwachung je nach Überwachungstyp für die primäre Datenbank, die sekundäre Datenbank oder für beide einrichten.

Befolgen Sie diese Anweisungen (denken Sie daran, dass die Blobüberwachung nur in den Überwachungseinstellungen der primären Datenbank aktiviert und deaktiviert werden kann):

* **Primäre Datenbank**. Aktivieren Sie die Blobüberwachung entweder auf dem Server oder in der Datenbank selbst, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2) beschrieben.
* **Sekundäre Datenbank**. Aktivieren Sie die Blobüberwachung in der primären Datenbank, wie im Abschnitt [Einrichten der Überwachung für Ihre Datenbank](#subheading-2) beschrieben. 
   * Die Blobüberwachung muss in der *primären Datenbank selbst* aktiviert werden, nicht auf dem Server.
   * Nachdem die Blobüberwachung in der primären Datenbank aktiviert wurde, wird sie auch in der sekundären Datenbank aktiv.

     >[!IMPORTANT]
     >Standardmäßig sind die Speichereinstellungen für die sekundäre Datenbank identisch mit den Einstellungen der primären Datenbank, wodurch regionsübergreifender Datenverkehr generiert wird. Sie können dies vermeiden, indem Sie die Blobüberwachung auf dem sekundären Server aktivieren und den lokalen Speicher in den Speichereinstellungen des sekundären Servers konfigurieren. Damit überschreiben Sie den Speicherort für die sekundäre Datenbank, sodass die Überwachungsprotokolle jeder Datenbank lokal gespeichert werden.  
<br>

### <a id="subheading-6">Erneute Speicherschlüsselgenerierung</a>
In einer Produktionsumgebung werden Sie Ihre Speicherschlüssel wahrscheinlich regelmäßig aktualisieren. Wenn Sie die Schlüssel aktualisieren, müssen Sie die Überwachungsrichtlinie neu speichern. Dieser Prozess verläuft wie folgt:

1. Öffnen Sie das Blatt **Speicherdetails**. Wählen Sie im Feld **Speicherzugriffsschlüssel** die Option **Sekundär** aus, und klicken Sie dann auf **OK**. Klicken Sie anschließend oben auf dem Blatt für die Überwachungskonfiguration auf **Speichern**.

    ![Navigationsbereich][5]
2. Wechseln Sie zum Blatt für die Speicherkonfiguration, und generieren Sie erneut den primären Zugriffsschlüssel.

    ![Navigationsbereich][6]
3. Wechseln Sie zurück zum Blatt für die Überwachungskonfiguration, ändern Sie den Speicherzugriffsschlüssel von „Sekundär“ in „Primär“, und klicken Sie dann auf **OK**. Klicken Sie anschließend oben auf dem Blatt für die Überwachungskonfiguration auf **Speichern**.
4. Wechseln Sie zurück zum Blatt für die Speicherkonfiguration, und generieren Sie erneut den sekundären Zugriffsschlüssel (als Vorbereitung auf den nächsten Schlüsselaktualisierungszyklus).

## <a id="subheading-7"></a>Automatisierung (PowerShell/REST-API)
Sie können die Überwachung in Azure SQL-Datenbank auch mit den folgenden Automatisierungstools konfigurieren:

* **PowerShell-Cmdlets:**

   * [Get-AzureRMSqlDatabaseAuditingPolicy][101]
   * [Get-AzureRMSqlServerAuditingPolicy][102]
   * [Remove-AzureRMSqlDatabaseAuditing][103]
   * [Remove-AzureRMSqlServerAuditing][104]
   * [Set-AzureRMSqlDatabaseAuditingPolicy][105]
   * [Set-AzureRMSqlServerAuditingPolicy][106]
   * [Use-AzureRMSqlServerAuditingPolicy][107]

   Ein Skriptbeispiel finden Sie unter [Konfigurieren von Überwachung von SQL-Datenbank und Bedrohungserkennung mit PowerShell](scripts/sql-database-auditing-and-threat-detection-powershell.md).

* **REST-API – Blobüberwachung**:

   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Datenbankblobs](https://msdn.microsoft.com/library/azure/mt695939.aspx)
   * [Erstellen oder Aktualisieren einer Richtlinie für die Überwachung von Serverblobs](https://msdn.microsoft.com/library/azure/mt771861.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Datenbankblobs](https://msdn.microsoft.com/library/azure/mt695938.aspx)
   * [Abrufen einer Richtlinie für die Überwachung von Serverblobs](https://msdn.microsoft.com/library/azure/mt771860.aspx)
   * [Abrufen des Ergebnisses eines Überwachungsvorgangs für Serverblobs](https://msdn.microsoft.com/library/azure/mt771862.aspx)


<!--Anchors-->
[Azure SQL Database Auditing overview]: #subheading-1
[Set up auditing for your database]: #subheading-2
[Analyze audit logs and reports]: #subheading-3
[Practices for usage in production]: #subheading-5
[Storage Key Regeneration]: #subheading-6
[Automation (PowerShell / REST API)]: #subheading-7
[Blob/Table differences in Server auditing policy inheritance]: (#subheading-8)  

<!--Image references-->
[1]: ./media/sql-database-auditing-get-started/1_auditing_get_started_settings.png
[2]: ./media/sql-database-auditing-get-started/2_auditing_get_started_server_inherit.png
[3]: ./media/sql-database-auditing-get-started/3_auditing_get_started_turn_on.png
[4]: ./media/sql-database-auditing-get-started/4_auditing_get_started_storage_details.png
[5]: ./media/sql-database-auditing-get-started/5_auditing_get_started_storage_key_regeneration.png
[6]: ./media/sql-database-auditing-get-started/6_auditing_get_started_regenerate_key.png
[7]: ./media/sql-database-auditing-get-started/7_auditing_get_started_blob_view_audit_logs.png
[8]: ./media/sql-database-auditing-get-started/8_auditing_get_started_blob_audit_records.png
[9]: ./media/sql-database-auditing-get-started/9_auditing_get_started_ssms_1.png
[10]: ./media/sql-database-auditing-get-started/10_auditing_get_started_ssms_2.png

[101]: /powershell/module/azurerm.sql/get-azurermsqldatabaseauditingpolicy
[102]: /powershell/module/azurerm.sql/Get-AzureRMSqlServerAuditingPolicy
[103]: /powershell/module/azurerm.sql/Remove-AzureRMSqlDatabaseAuditing
[104]: /powershell/module/azurerm.sql/Remove-AzureRMSqlServerAuditing
[105]: /powershell/module/azurerm.sql/Set-AzureRMSqlDatabaseAuditingPolicy
[106]: /powershell/module/azurerm.sql/Set-AzureRMSqlServerAuditingPolicy
[107]: /powershell/module/azurerm.sql/Use-AzureRMSqlServerAuditingPolicy


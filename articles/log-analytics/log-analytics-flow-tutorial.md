---
title: Automatisieren von Azure Log Analytics-Prozessen mit Microsoft Flow
description: In diesem Artikel erfahren Sie, wie Sie Microsoft Flow zum schnellen Automatisieren von wiederholbaren Prozessen mit dem Azure Log Analytics-Connector verwenden.
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: bwren
ms.translationtype: HT
ms.sourcegitcommit: 14915593f7bfce70d7bf692a15d11f02d107706b
ms.openlocfilehash: 4955f90de06cd720d78c5bb60c0adcd7dc633245
ms.contentlocale: de-de
ms.lasthandoff: 08/10/2017

---

# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a>Automatisieren von Log Analytics-Prozessen mit dem Connector für Microsoft Flow
Mit [Microsoft Flow](https://ms.flow.microsoft.com) können Sie automatisierte Workflows erstellen, indem Sie Hunderte von Aktionen für eine Vielzahl von Diensten verwenden. Die Ausgabe einer Aktion kann als Eingabe einer anderen Aktion genutzt werden, damit Sie die Integration zwischen unterschiedlichen Diensten erstellen können.  Mit dem Azure Log Analytics-Connector für Microsoft Flow können Sie Workflows mit Daten erstellen, die von Protokollsuchen in Log Analytics abgerufen werden.

Beispielsweise können Sie Microsoft Flow zum Verwenden von Log Analytics-Daten in einer E-Mail-Benachrichtigung aus Office 365, Erstellen eines Fehlers in Visual Studio Team Services oder Posten einer Slack-Nachricht nutzen.  Sie können einen Workflow auslösen, indem Sie einen einfachen Zeitplan oder eine Aktion in einem verbundenen Dienst verwenden, z.B. bei Erhalt einer E-Mail oder eines Tweets.  


> [!NOTE]
> Für den Azure Log Analytics-Connector für Microsoft Flow muss Ihr Arbeitsbereich auf die neue Log Analytics-Abfragesprache aktualisiert werden. Weitere Informationen zur neuen Sprache und zum Upgradeverfahren für den Arbeitsbereich finden Sie unter [Durchführen eines Upgrades für den Azure Log Analytics-Arbeitsbereich auf die neue Protokollsuche](log-analytics-log-search-upgrade.md).  

Im Tutorial in diesem Artikel wird gezeigt, wie Sie einen Flow erstellen, bei dem die Ergebnisse einer Log Analytics-Protokollsuche automatisch per E-Mail gesendet werden. Dies ist nur ein Beispiel für die Nutzung von Log Analytics in Microsoft Flow. 


## <a name="step-1-create-a-flow"></a>Schritt 1: Erstellen eines Flow
1. Melden Sie sich an [Microsoft Flow](http://flow.microsoft.com) an, und wählen Sie **Meine Flows**.
2. Klicken Sie auf **+ Ohne Vorlage erstellen**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>Schritt 2: Erstellen eines Triggers für Ihren Flow
1. Klicken Sie auf **Search hundreds of connectors and triggers** (Mehrere hundert Connectors und Trigger durchsuchen).
2. Geben Sie im Suchfeld den Suchbegriff **Zeitplan** ein.
3. Wählen Sie **Zeitplan** und anschließend **Zeitplan – Wiederholung** aus.
4. Wählen Sie im Feld **Häufigkeit** die Option **Tag** aus, und geben Sie im Feld **Intervall** den Wert **1** ein.<br><br>![Trigger-Dialogfeld für Microsoft Flow](media/log-analytics-flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>Schritt 3: Hinzufügen einer Log Analytics-Aktion
1. Klicken Sie auf **+ Neuer Schritt** und anschließend auf **Aktion hinzufügen**.
2. Suchen Sie nach **Log Analytics**.
3. Klicken Sie auf **Azure Log Analytics – Run query and visualize results** (Azure Log Analytics – Abfrage ausführen und Ergebnisse visualisieren).<br><br>![Log Analytics-Fenster zum Ausführen von Abfragen](media/log-analytics-flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>Schritt 4: Konfigurieren der Log Analytics-Aktion

1. Geben Sie die Details für Ihren Arbeitsbereich an, z.B. Abonnement-ID, Ressourcengruppe und Arbeitsbereichsname.
2. Fügen Sie dem Fenster **Abfrage** die unten angegebene Log Analytics-Abfrage hinzu.  Dies ist nur eine Beispielabfrage, die Sie auch durch andere Abfragen ersetzen können, bei denen Daten zurückgegeben werden.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computerindow. 
```

2. Wählen Sie unter **Diagrammtyp** die Option **HTML-Tabelle**.<br><br>![Log Analytics-Aktion](media/log-analytics-flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>Schritt 5: Konfigurieren des Flows zum Senden von E-Mails

1. Klicken Sie auf **Neuer Schritt** und anschließend auf **+ Aktion hinzufügen**.
2. Suchen Sie nach **Office 365 Outlook**.
3. Klicken Sie auf **Office 365 Outlook – E-Mail senden**.<br><br>![Fenster zum Auswählen von Office 365 Outlook](media/log-analytics-flow-tutorial/flow04.png)

4. Geben Sie die E-Mail-Adresse eines Empfängers im Fenster **An** und einen Betreff für die E-Mail unter **Betreff** an.
5. Klicken Sie im Feld **Textkörper** auf eine beliebige Stelle.  Das Fenster **Dynamischer Inhalt** mit Werten aus den vorherigen Aktionen wird geöffnet.  
6. Wählen Sie die Option **Textkörper**.  Dies sind die Ergebnisse der Abfrage in der Log Analytics-Aktion.
6. Klicken Sie auf **Erweiterte Optionen anzeigen**.
7. Wählen Sie im Feld **Ist HTML** die Option **Ja** aus.<br><br>![Fenster zum Konfigurieren von Office 365-E-Mails](media/log-analytics-flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>Schritt 6: Speichern und Testen Ihres Flows
1. Geben Sie im Feld **Flowname** einen Namen für den Flow ein, und klicken Sie anschließend auf **Flow erstellen**.<br><br>![Flow speichern](media/log-analytics-flow-tutorial/flow06.png)
2. Der Flow wird jetzt erstellt und nach einem Tag ausgeführt. Dies ist der von Ihnen angegebene Zeitplan. 
3. Sie können den Flow auch sofort testen, indem Sie auf **Jetzt ausführen** und dann auf **Flow ausführen** klicken.<br><br>![Flow ausführen](media/log-analytics-flow-tutorial/flow07.png)
3. Prüfen Sie nach Abschluss des Flowvorgangs die E-Mail des von Ihnen angegebenen Empfängers.  Es sollte eine E-Mail mit einem Text eingegangen sein, der etwa wie folgt lautet:<br><br>![Beispiel-E-Mail](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a>Nächste Schritte

- Informieren Sie sich über [Protokollsuchen in Log Analytics](log-analytics-log-search-new.md).
- Erfahren Sie mehr über [Microsoft Flow](https://ms.flow.microsoft.com).





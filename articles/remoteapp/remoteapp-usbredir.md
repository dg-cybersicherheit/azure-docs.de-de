---
title: "Wie werden USB-Geräte in Azure RemoteApp umgeleitet? | Microsoft Docs"
description: "Erfahren Sie, wie USB-Geräte in Azure RemoteApp umgeleitet werden."
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 191d98af-2f5a-4307-9042-aae0e4049f9f
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 1be1e6f63c4a4786d23af57d454e7a2e3bb17ad0
ms.lasthandoff: 03/31/2017


---
# <a name="how-do-you-redirect-usb-devices-in-azure-remoteapp"></a>Wie werden USB-Geräte in Azure RemoteApp umgeleitet?
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Die Geräteumleitung ermöglicht Benutzern das Verwenden der an ihren Computer oder ihr Tablet angeschlossenen USB-Geräte mit den Apps in Azure RemoteApp. Wenn Sie z. B. Skype über Azure RemoteApp freigegeben haben, müssen die Benutzer ihre Gerätekameras verwenden können.

Bevor Sie fortfahren, lesen Sie zunächst die Informationen zur USB-Umleitung unter [Verwenden von Umleitungen in Azure RemoteApp](remoteapp-redirection.md). Der empfohlene Befehl „nusbdevicestoredirect:s:*“ funktioniert jedoch nicht bei USB-Webcams und möglicherweise auch nicht bei einigen USB-Druckern oder -Multifunktionsgeräten. Es ist vorgesehen, dass der Azure RemoteApp-Administrator aus Sicherheitsgründen die Umleitung entweder über die Geräteklassen-GUID oder Geräte-Instanz-ID aktivieren muss, ehe die Benutzer diese Geräte nutzen können.

Obwohl in diesem Artikel von der Umleitung von Webcams die Rede ist, können Sie einen ähnlichen Ansatz befolgen, um USB-Drucker und andere -Multifunktionsgeräte umzuleiten, die nicht mit dem Befehl **nusbdevicestoredirect:s:***umgeleitet werden.

## <a name="redirection-options-for-usb-devices"></a>Umleitungsoptionen für USB-Geräte
Azure RemoteApp verwendet sehr ähnliche Mechanismen zum Umleiten von USB-Geräten wie diejenigen, die für Remotedesktopdienste verfügbar sind. Die zugrunde liegende Technologie ermöglicht das Auswählen der ordnungsgemäßen Umleitungsmethode für ein bestimmtes Gerät, um mit dem Befehl **usbdevicestoredirect:s:** optimale Ergebnisse sowohl bei der allgemeinen als auch der RemoteFX-USB-Geräteumleitung zu erzielen. Dieser Befehl hat vier Elemente:

| Verarbeitungsreihenfolge | Parameter | Beschreibung |
| --- | --- | --- |
| 1 |* |Wählt alle Geräte aus, die von der allgemeinen Umleitung nicht ausgewählt werden. Hinweis: Standardmäßig funktioniert „*“ nicht für USB-Webcams. |
| {Geräteklassen-GUID} |Wählt alle Geräte aus, die der angegebenen Gerätesetupklasse entsprechen. | |
| USB\Instanz-ID |Wählt ein USB-Gerät aus, das für die jeweilige Instanz-ID angegeben ist. | |
| 2 |-USB\Instanz-ID |Entfernt die Umleitungseinstellungen für das angegebene Gerät. |

## <a name="redirecting-a-usb-device-by-using-the-device-class-guid"></a>Umleiten von USB-Geräten mithilfe der Geräteklassen-GUID
Es gibt zwei Möglichkeiten, die Geräteklassen-GUID zu finden, die für die Umleitung verwendet werden kann. 

Die erste Möglichkeit sind die vom [System definierten Gerätesetupklassen, die für Anbieter verfügbar sind](https://msdn.microsoft.com/library/windows/hardware/ff553426.aspx). Wählen Sie die Klasse, die am ehesten dem Gerät entspricht, das am lokalen Computer angeschlossen ist. Für Digitalkameras kann dies eine Bildverarbeitungs- oder Videoaufnahme-Geräteklasse sein. Sie müssen ein wenig mit den Geräteklassen experimentieren, um die ordnungsgemäße Klassen-GUID zu finden, die für das lokal angeschlossene USB-Gerät (in unserem Fall die Webcam) funktioniert.

Eine bessere Möglichkeit bzw. die zweite Option ist, die folgenden Schritte zu befolgen, um die spezifische Geräteklassen-GUID zu finden:

1. Öffnen Sie den Geräte-Manager, und suchen Sie das Gerät, das umgeleitet werden soll. Klicken Sie mit der rechten Maustaste darauf, und öffnen Sie dann die Eigenschaften.
   ![Öffnen des Geräte-Managers](./media/remoteapp-usbredir/ra-devicemanager.png)
2. Wählen Sie auf der Registerkarte **Details** die Eigenschaft **Klassen-GUID**. Der angezeigte Wert ist die Klassen-GUID für diesen Gerätetyp.
   ![Kameraeigenschaften](./media/remoteapp-usbredir/ra-classguid.png)
3. Verwenden Sie den Wert der Klassen-GUID, um entsprechende Geräte umzuleiten.

Beispiel:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s:<Class Guid value>"

Sie können mehrere Geräteumleitungen in einem Cmdlet zusammenfassen. Beispiel: Zum Umleiten von lokalem Speicher und einer USB-Webcam sieht das Cmdlet folgendermaßen aus:

        Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "drivestoredirect:s:*`nusbdevicestoredirect:s:<Class Guid value>"

Wenn Sie die Geräteumleitung anhand der Klassen-GUID festlegen, werden alle Geräte umgeleitet, die der Klassen-GUID in der angegebenen Sammlung entsprechen. Wenn es beispielsweise mehrere Computer im lokalen Netzwerk gibt, die die gleiche USB-Webcam haben, können Sie mithilfe eines einzelnen Cmdlets alle Webcams umleiten.

## <a name="redirecting-a-usb-device-by-using-the-device-instance-id"></a>Umleiten eines USB-Geräts mithilfe der Geräteinstanz-ID
Wenn Sie eine präzisere Kontrolle wünschen und die Umleitung gerätebezogen steuern möchten, können Sie den Umleitungsparameter **USB\InstanceID** verwenden.

Der schwierigste Teil dieser Methode ist das Ermitteln der USB-Geräteinstanz-ID. Sie benötigen Zugriff auf den Computer und das jeweilige USB-Gerät. Führen Sie dann die folgenden Schritte durch:

1. Aktivieren Sie die Umleitung in der Remotedesktopsitzung wie unter [Wie kann ich meine Geräte und Ressourcen in einer Remotedesktopsitzung verwenden?](http://windows.microsoft.com/en-us/windows7/How-can-I-use-my-devices-and-resources-in-a-Remote-Desktop-session)
2. Öffnen Sie eine Remotedesktopverbindung, und klicken Sie auf **Optionen anzeigen**.
3. Klicken Sie auf **Speichern unter** , um die aktuellen Verbindungseinstellungen in einer RDP-Datei zu speichern.  
    ![Speichern der Einstellungen als RDP-Datei](./media/remoteapp-usbredir/ra-saveasrdp.png)
4. Wählen Sie einen Dateinamen und Speicherort aus, z.B. „MeineVerbindung.rdp“ und „Dieser PC\Dokumente“, und speichern Sie die Datei.
5. Öffnen Sie die Datei „MeineVerbindung.rdp“ in einem Text-Editor, und suchen Sie die Instanz-ID des Geräts, das Sie umleiten möchten.

Verwenden Sie anschließend die Instanz-ID im folgenden Cmdlet:

    Set-AzureRemoteAppCollection -CollectionName <collection name> -CustomRdpProperty "nusbdevicestoredirect:s: USB\<Device InstanceID value>"



### <a name="help-us-help-you"></a>Helfen Sie uns, Ihnen zu helfen
Wussten Sie schon, dass Sie diesen Artikel im Bereich unten nicht nur bewerten und kommentieren, sondern ihn auch selbst ändern können? Fehlt etwas? Ist etwas nicht ganz richtig? Habe ich etwas geschrieben, das eher verwirrend ist? Scrollen Sie nach oben, und klicken Sie auf **Edit on GitHub**, um die gewünschten Änderungen vorzunehmen. Ihr Vorschlag wird uns vorgelegt, und wenn wir ihn bestätigt haben, werden Ihre Änderungen und Verbesserungen hier angezeigt.



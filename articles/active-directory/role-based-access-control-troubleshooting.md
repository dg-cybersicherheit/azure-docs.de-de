---
title: "Azure RBAC – Problembehandlung | Microsoft-Dokumentation"
description: "Hilfe bei Problemen oder Fragen zu Ressourcen für die rollenbasierte Zugriffsteuerung."
services: azure-portal
documentationcenter: na
author: kgremban
manager: femila
ms.assetid: df42cca2-02d6-4f3c-9d56-260e1eb7dc44
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2017
ms.author: kgremban
ms.reviewer: rqureshi
ms.translationtype: HT
ms.sourcegitcommit: c3ea7cfba9fbf1064e2bd58344a7a00dc81eb148
ms.openlocfilehash: 9ad194308d30ca652b32ec3b76750b0e838472f4
ms.contentlocale: de-de
ms.lasthandoff: 07/19/2017

---
# <a name="role-based-access-control-troubleshooting"></a>Behandlung von Problemen bei der rollenbasierten Zugriffssteuerung

In diesem Dokument werden häufig gestellte Fragen über bestimmte Zugriffsrechte, die mit Rollen erteilt werden, beantwortet. Sie erfahren also, was Sie erwarten können, wenn Sie die Rollen im Azure-Portal verwenden und wie Sie Zugriffsprobleme lösen können. Diese drei Rollen decken alle Ressourcentypen ab:

* Besitzer  
* Mitwirkender  
* Leser  

Sowohl Besitzer als auch Mitwirkende haben Vollzugriff auf alle Verwaltungsfunktionen, Mitwirkende können jedoch anderen Benutzern oder Gruppen keinen Zugriff gewähren. Die Leserolle werden wir aufgrund ihrer umfassenden Eigenschaften ausführlicher beschreiben. Ausführliche Informationen zum Gewähren von Zugriff finden Sie im Artikel zu den ersten Schritten mit der [rollenbasierten Zugriffssteuerung](role-based-access-control-configure.md) .

## <a name="app-service-workloads"></a>App Service-Workloads
### <a name="write-access-capabilities"></a>Schreibzugriff
Wenn Sie einem Benutzer schreibgeschützten Zugriff für eine einzelne Web-App gewähren, sind einige Features deaktiviert, von denen Sie das unter Umständen nicht erwartet haben. Die folgenden Verwaltungsfunktionen erfordern **Schreibzugriff** auf eine Web-App (entweder als Mitwirkender oder Besitzer) und stehen nicht zur Verfügung, wenn der Benutzer nur über Lesezugriff für die Web-App verfügt.

* Befehle (z.B. starten, anhalten usw.)
* Änderung von Einstellungen wie allgemeine Konfigurationen, Skalierungseinstellungen, Sicherungseinstellungen und Überwachungseinstellungen
* Zugriff auf Anmeldedaten für die Veröffentlichung oder andere geheime Schlüssel wie App- und Verbindungseinstellungen
* Streamingprotokolle
* Konfiguration von Diagnoseprotokollen
* Konsole (Eingabeaufforderung)
* Aktive und kürzlich vorgenommene Bereitstellungen (für fortlaufende Bereitstellungen lokaler Gits)
* Geschätzte Ausgaben
* Webtests
* Virtuelles Netzwerk (für Leser nur sichtbar, wenn ein virtuelles Netzwerk zuvor von einem Benutzer mit Schreibzugriff konfiguriert wurde)

Wenn Sie auf keine dieser Kacheln zugreifen können, fragen Sie den Administrator nach Zugriff als Mitwirkender auf die Web-App.

### <a name="dealing-with-related-resources"></a>Umgang mit zugehörigen Ressourcen
Web-Apps können aufgrund verschiedener miteinander verknüpfter Ressourcen kompliziert sein. Im Folgenden ist eine typische Ressourcengruppe mit einigen Websites dargestellt:

![Web-App-Ressourcengruppe](./media/role-based-access-control-troubleshooting/website-resource-model.png)

Wenn Sie daher lediglich auf die Web-App Zugriff gewähren, sind viele Funktionen des Blatts „Website“ im Azure-Portal deaktiviert.

Die folgenden Elemente erfordern **Schreibzugriff** auf den **App Service-Plan** für Ihre Website:  

* Anzeigen des Tarifs für die Web-App (Free oder Standard)  
* Skalierungskonfiguration (Anzahl der Instanzen, Größe des virtuellen Computers, Einstellungen für automatische Skalierung)  
* Kontingente (Speicher, Bandbreite, CPU)  

Die folgenden Elemente erfordern **Schreibzugriff** auf die gesamte **Ressourcengruppe**, die Ihre Website umfasst:  

* SSL-Zertifikate und -Bindungen (SSL-Zertifikate können von Websites derselben Ressourcengruppe und desselben geografischen Standorts gemeinsam genutzt werden)  
* Warnungsregeln  
* Einstellungen für automatische Skalierung  
* Application Insights-Komponenten  
* Webtests  

## <a name="virtual-machine-workloads"></a>Workloads virtueller Computer
Ähnlich wie bei Web-Apps erfordern einige Funktionen auf dem Blatt "Virtueller Computer" Schreibzugriff auf den virtuellen Computer oder auf andere Ressourcen in der Ressourcengruppe.

Virtuelle Computer stehen in Verbindung mit Domänennamen, virtuellen Netzwerken, Speicherkonten und Warnungsregeln.

Die folgenden Elemente erfordern **Schreibzugriff** auf den **virtuellen Computer**:

* Endpunkte  
* IP-Adressen  
* Datenträger  
* Erweiterungen  

Die folgenden Elemente erfordern **Schreibzugriff** auf den **virtuellen Computer** und die **Ressourcengruppe** (zusammen mit dem Domänennamen), in der sich der virtuelle Computer befindet:  

* Verfügbarkeitsgruppe  
* Satz mit Lastenausgleich  
* Warnungsregeln  

Wenn Sie auf keine dieser Kacheln zugreifen können, fragen Sie den Administrator nach Zugriff als Mitwirkender auf diese Ressourcengruppe.

## <a name="see-more"></a>Mehr Informationen
* [Rollenbasierte Zugriffssteuerung](role-based-access-control-configure.md): Erste Schritte mit RBAC im Azure-Portal.
* [Integrierte Rollen:](role-based-access-built-in-roles.md)Hier erhalten Sie ausführliche Informationen zu den Standardrollen in RBAC.
* [Benutzerdefinierte Rollen in Azure RBAC](role-based-access-control-custom-roles.md): Erfahren Sie, wie Sie benutzerdefinierte Rollen entsprechend Ihrer Zugriffsanforderungen erstellen.
* [Erstellen eines Verlaufsbericht über Zugriffsänderungen](role-based-access-control-access-change-history-report.md): Nachverfolgen der Änderung von Rollenzuweisungen in RBAC.



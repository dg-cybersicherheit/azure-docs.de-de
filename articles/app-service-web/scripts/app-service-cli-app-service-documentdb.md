---
title: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung zwischen einer Web-App und Cosmos DB | Microsoft-Dokumentation"
description: "Azure CLI-Skriptbeispiel – Herstellen einer Verbindung zwischen einer Web-App und Cosmos DB"
services: appservice
documentationcenter: appservice
author: syntaxc4
manager: erikre
editor: 
tags: azure-service-management
ms.assetid: bbbdbc42-efb5-4b4f-8ba6-c03c9d16a7ea
ms.service: app-service
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: na
ms.workload: web
ms.date: 06/19/2017
ms.author: cfowler
ms.custom: mvc
ms.translationtype: Human Translation
ms.sourcegitcommit: 4f68f90c3aea337d7b61b43e637bcfda3c98f3ea
ms.openlocfilehash: ff5e7a794033cc51120831e09b055a86affb28a4
ms.contentlocale: de-de
ms.lasthandoff: 06/20/2017

---

# <a name="connect-a-web-app-to-cosmos-db"></a>Herstellen einer Verbindung zwischen einer Web-App und Cosmos DB

In diesem Szenario erfahren Sie, wie Sie ein Azure Cosmos DB-Konto und eine Azure-Web-App erstellen. Anschließend verknüpfen Sie die Cosmos DB-Instanz mithilfe von App-Einstellungen mit der Web-App.


[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Wenn Sie die CLI lokal installieren und verwenden möchten, müssen Sie für dieses Thema die Azure CLI-Version 2.0 oder höher ausführen. Führen Sie `az --version` aus, um die Version zu finden. Wenn Sie eine Installation oder ein Upgrade ausführen müssen, finden Sie unter [Installieren von Azure CLI 2.0]( /cli/azure/install-azure-cli) Informationen dazu. 

## <a name="sample-script"></a>Beispielskript

[!code-azurecli-interactive[main](../../../cli_scripts/app-service/connect-to-documentdb/connect-to-documentdb.sh "Azure Cosmos DB")]

[!INCLUDE [cli-script-clean-up](../../../includes/cli-script-clean-up.md)]

## <a name="script-explanation"></a>Erläuterung des Skripts

In diesem Skript werden die folgenden Befehle verwendet, um eine Ressourcengruppe, eine Web-App, die Cosmos DB-Instanz und alle zugehörigen Ressourcen zu erstellen. Jeder Befehl in der Tabelle ist mit der zugehörigen Dokumentation verknüpft.

| Befehl | Hinweise |
|---|---|
| [az group create](https://docs.microsoft.com/cli/azure/group#create) | Erstellt eine Ressourcengruppe, in der alle Ressourcen gespeichert sind. |
| [az appservice plan create](https://docs.microsoft.com/cli/azure/appservice/plan#create) | Erstellt einen App Service-Plan. Dies ist wie eine Serverfarm für die Azure-Web-App. |
| [az webapp create](https://docs.microsoft.com/cli/azure/webapp#create) | Erstellt eine Azure-Web-App. |
| [az cosmosdb create](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#create) | Erstellt ein Cosmos DB-Konto. Hier werden die Daten gespeichert. |
| [az cosmosdb list-keys](https://docs.microsoft.com/en-us/cli/azure/cosmosdb#list-keys) | Listet die Zugriffsschlüssel für das angegebene Cosmos DB-Konto auf. |
| [az webapp config appsettings set](https://docs.microsoft.com/cli/azure/webapp/config/appsettings#set) | Erstellt oder aktualisiert eine App-Einstellung für eine Azure-Web-App. App-Einstellungen werden als Umgebungsvariablen für Ihre App verfügbar gemacht. |

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zur Azure CLI finden Sie in der [Azure CLI-Dokumentation](https://docs.microsoft.com/cli/azure/overview).

Zusätzliche App Service-CLI-Skriptbeispiele finden Sie in der [Dokumentation zu Azure App Service](../app-service-cli-samples.md).


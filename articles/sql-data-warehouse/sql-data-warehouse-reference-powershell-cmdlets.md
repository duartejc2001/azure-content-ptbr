<properties
   pageTitle="Como usar os cmdlets do PowerShell e as APIs REST com o SQL Data Warehouse"
   description="Suspender e reiniciar o SQL Data Warehouse usando cmdlets do PowerShell"
   services="sql-data-warehouse"
   documentationCenter="NA"
   authors="barbkess"
   manager="barbkess"
   editor=""/>

<tags
   ms.service="sql-data-warehouse"
   ms.devlang="NA"
   ms.topic="article"
   ms.tgt_pltfrm="NA"
   ms.workload="data-services"
   ms.date="03/28/2016"
   ms.author="barbkess;mausher;sonyama"/>

# Como usar os cmdlets do PowerShell e as APIs REST com o SQL Data Warehouse

O SQL Data Warehouse pode ser gerenciado usando APIs REST ou cmdlets do PowerShell do Azure.

Os comandos definidos para o **Banco de Dados SQL do Azure** também são usados para o **SQL Data Warehouse**. Para obter uma lista atual, consulte [Cmdlets do SQL do Azure](https://msdn.microsoft.com/library/mt574084.aspx). Os cmdlets **Suspend-AzureRmSqlDatabase** e **Resume-AzureRmSqlDatabase** (abaixo) são adições projetadas para o SQL Data Warehouse.

Da mesma forma, as APIs REST para o **Banco de Dados do SQL Azure** também podem ser usadas para as instâncias do **SQL Data Warehouse**. Para obter a lista atual, consulte [Operações de bancos de dados SQL do Azure](https://msdn.microsoft.com/library/azure/dn505719.aspx).

## Obter e executar os cmdlets do PowerShell do Azure

1. Para baixar o módulo PowerShell do Azure, execute o [Microsoft Web Platform Installer](http://aka.ms/webpi-azps). Para obter mais informações sobre esse instalador, veja [Como instalar e configurar o Azure PowerShell][].
2. Para executar o módulo, na janela de início, digite **Windows PowerShell**.
3. Execute este cmdlet para fazer logon no Gerenciador de Recursos do Azure.

```PowerShell
Login-AzureRmAccount
```

3. Selecione sua assinatura para o banco de dados que você deseja suspender ou retomar. Isso seleciona a assinatura denominada "MySubscription".

```Powershell
Select-AzureRmSubscription -SubscriptionName "MySubscription"
```

## Suspend-AzureRmSqlDatabase

Para obter a referência do comando, veja [Suspend-AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt619337.aspx).

### Exemplo 1: Pausar um banco de dados por nome em um servidor

Este exemplo pausa um banco de dados denominado "Database02" hospedado em um servidor denominado "Server01." O servidor está em um grupo de recursos do Azure denominado "ResourceGroup1".

```Powershell
Suspend-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
```

### Exemplo 2: Pausar um objeto de banco de dados

Este exemplo recupera um banco de dados denominado “Database02” de um servidor chamado “Server01” contido em um grupo de recursos denominado “ResourceGroup1”. Ele canaliza o objeto recuperado para **Suspend-AzureRmSqlDatabase**. Como resultado, o banco de dados é pausado. O comando final mostra os resultados.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Suspend-AzureRmSqlDatabase
$resultDatabase
```

## Resume-AzureSqlDatabase

Para referência sobre o comando, veja [Resume-AzureRmSqlDatabase](https://msdn.microsoft.com/library/mt619347.aspx)

### Exemplo 1: Retomar um banco de dados por nome em um servidor

Este exemplo retoma a operação de um banco de dados denominado "Database02" hospedado em um servidor denominado "Server01." O servidor está contido em um grupo de recursos denominado "ResourceGroup1".

```Powershell
Resume-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" -DatabaseName "Database02"
```

### Exemplo 2: Retomando um objeto de banco de dados

Este exemplo recupera um banco de dados denominado “Database02” de um servidor chamado “Server01” que está contido em um grupo de recursos denominado “ResourceGroup1”. O objeto é canalizado para **Resume-AzureRmSqlDatabase**.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$resultDatabase = $database | Resume-AzureRmSqlDatabase
```

## Get-AzureRmSqlDatabaseRestorePoints

Esse cmdlet lista os pontos de restauração de backup para um banco de dados do SQL Data Warehouse do Azure. Os pontos de restauração são usados para restaurar o banco de dados. As propriedades do objeto retornado são como se segue.

Propriedade|Descrição
---|---
RestorePointType|DISCRETOS / CONTÍNUOS. Os pontos de restauração discretos descrevem os possíveis pontos no tempo para os quais um banco de dados do SQL Data Warehouse do Azure podem ser restaurados. Os pontos de restauração contínuos descrevem os possíveis pontos no tempo mais antigos para os quais um banco de dados SQL do Azure pode ser restaurado. O banco de dados pode ser restaurado para qualquer ponto no tempo após o ponto mais antigo.
EarliestRestoreDate|Tempo de restauração mais antigo (preenchido quando restorePointType = CONTÍNUO)
RestorePointCreationDate |Tempo do instantâneo de backup (preenchido quando restorePointType = DISCRETO)

### Exemplo 1: Recuperando pontos de restauração de um banco de dados por nome em um servidor
Este exemplo recupera os pontos de restauração para um banco de dados denominado “Database02” de um servidor chamado “Server01” contido em um grupo de recursos denominado “ResourceGroup1”.

```Powershell
$restorePoints = Get-AzureRmSqlDatabaseRestorePoints –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$restorePoints
```


### Exemplo 2: Retomando um objeto de banco de dados

Este exemplo recupera um banco de dados denominado “Database02” de um servidor chamado “Server01”, contido em um grupo de recursos denominado “ResourceGroup1”. O objeto de banco de dados é canalizado para **Get-AzureRmSqlDatabase** e o resultado são os pontos de restauração do banco de dados. O comando final imprime os resultados.

```Powershell
$database = Get-AzureRmSqlDatabase –ResourceGroupName "ResourceGroup1" –ServerName "Server01" –DatabaseName "Database02"
$restorePoints = $database | Get-AzureRmSqlDatabaseRestorePoints
$retorePoints
```


> [AZURE.NOTE] Observe que, se o servidor for foo.database.windows.net, use "foo" como o -ServerName nos cmdlets do PowerShell.


## Próximas etapas
Para obter mais informações de referência, consulte [Visão geral de referência do SQL Data Warehouse][].

<!--Image references-->

<!--Article references-->
[Visão geral de referência do SQL Data Warehouse]: sql-data-warehouse-overview-reference.md
[Como instalar e configurar o Azure PowerShell]: ../articles/powershell-install-configure.md

<!--MSDN references-->


<!--Other Web references-->
[gog]: http://google.com/
[yah]: http://search.yahoo.com/
[msn]: http://search.msn.com/

<!-----------HONumber=AcomDC_0330_2016-->
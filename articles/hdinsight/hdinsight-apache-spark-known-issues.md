<properties 
	pageTitle="Problemas conhecidos do Apache Spark no HDInsight | Microsoft Azure" 
	description="Problemas conhecidos do Apache Spark no HDInsight." 
	services="hdinsight" 
	documentationCenter="" 
	authors="mumian" 
	manager="paulettm" 
	editor="cgronlun"
	tags="azure-portal"/>

<tags 
	ms.service="hdinsight" 
	ms.workload="big-data" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="02/17/2016" 
	ms.author="nitinme"/>

# Problemas conhecidos do Apache Spark no Azure HDInsight (Linux)

Este documento mantém o registro de todos os problemas conhecidos para a visualização pública do Spark no HDInsight.

##Sessão interativa de vazamentos do Livy
 
**Sintoma:**

Quando o Livy é reiniciado com uma sessão interativa (do Ambari ou devido a reinicialização da máquina virtual de nó principal 0) ainda ativa, uma sessão de trabalho interativo irá vazar. Por isso, novos trabalhos podem travar no estado Aceito, não podendo ser iniciados.

**Atenuação:**

Use o procedimento a seguir para contornar o problema:

1. SSH no nó principal. 
2. Execute o seguinte comando para encontrar as IDs de aplicativo dos trabalhos interativos iniciados por Livy. 

        yarn application –list

    Os nomes de trabalho padrão serão Livy se os trabalhos tiverem sido iniciados com uma sessão interativa Livy sem nomes explícitos especificados; para a sessão Livy iniciada pelo notebook do Jupyter, o nome do trabalho iniciará com remotesparkmagics\_*.

3. Execute o seguinte comando para eliminar esses trabalhos.

        yarn application –kill <Application ID>

A execução de novos trabalhos terá início.

##Servidor de histórico Spark não iniciado 

**Sintoma:**
 
O servidor de histórico Spark não é iniciado automaticamente depois de um cluster ser criado.

**Atenuação:**

Inicie manualmente o servidor de histórico do Ambari.

##Erro ao carregar um Notebook de tamanhos maiores

**Sintoma:**

Você pode encontrar um erro **`Error loading notebook`** ao carregar notebooks com um tamanho maior.

**Atenuação:**

Se você visualizar esse erro, não significa que seus dados estão corrompidos ou foram perdidos. Os blocos de anotações ainda estão no disco, em `/var/lib/jupyter`, e você pode usar o SSH no cluster para acessá-los. É possível copiar os blocos de anotações do cluster para o computador local (usando SCP ou WinSCP) como um backup para impedir a perda de qualquer dado importante no bloco de anotações. Você pode aplicar SSH no túnel no nó de cabeçalho na porta 8001 para acessar o Jupyter sem passar pelo gateway. A partir daí, você pode limpar a saída do bloco de anotações e salvá-lo novamente para minimizar o tamanho dele.

Para evitar que esse erro aconteça no futuro, você deve seguir algumas práticas recomendadas:

* É importante manter um tamanho pequeno de bloco de anotações. Qualquer saída de trabalho no Spark que é enviada de volta ao Jupyter é mantida no bloco de anotações. De modo geral, uma prática recomendada com o Jupyter é evitar a execução de `.collect()` em RDDs ou em quadros de dados grandes; em vez disso, se você quiser inspecionar o conteúdo de um RDD, considere a execução de `.take()` ou `.sample()` para que sua saída não fique muito grande.
* Além disso, quando você salvar um bloco de anotações, limpe todas as células de saída para reduzir o tamanho.



##A inicialização inicial do notebook demora mais do que o esperado 

**Sintoma:**

A primeira instrução no notebook Jupyter usando a mágica do Spark pode demorar mais de um minuto.

**Explicação:**
 
Isso acontece quando a primeira célula de código é executada. Em segundo plano, isso inicia a configuração de sessão e os contextos de Spark, SQL e Hive são definidos. Depois desses contextos são definidos, a primeira instrução é executada e isso dá a impressão de que a instrução levou muito tempo para ser concluída.

##Tempo limite do notebook do Jupyter atingido na criação da sessão

**Sintoma:**

Quando o cluster Spark está sem recursos, os kernels Spark e Pyspark no notebook do Jupyter atingirão o tempo limite ao tentar criar a sessão.

**Atenuações:**

1. Libere alguns recursos do cluster Spark:

    - Pare outros notebooks do Spark no menu Fechar e Parar ou clicando em Desligar no gerenciador de notebook.
    - Interrompendo outros aplicativos Spark do YARN.

2. Reinicie o notebook que você está tentando iniciar. Recursos suficientes devem estar disponíveis para você criar uma sessão agora.

## Problema de permissão no diretório de log do Spark 

**Sintoma:**
 
Quando hdiuser envia um trabalho com spark-submit, há um erro java.io.FileNotFoundException: /var/log/spark/sparkdriver\_hdiuser.log (permissão negada) e o log do driver não é gravado.

**Atenuação:**
 
1. Adicione hdiuser ao grupo Hadoop. 
2. Forneça 777 permissões em /var/log/spark após a criação do cluster. 
3. Atualize o local do log spark usando o Ambari para ser um diretório com 777 permissões.  
4. Execute spark-submit como sudo. 

##Consulte também

- [Visão geral: Apache Spark no Azure HDInsight (Linux)](hdinsight-apache-spark-overview.md)
- [Introdução: provisionar o Apache Spark no Azure HDInsight (Linux) e executar consultas interativas usando o Spark SQL](hdinsight-apache-spark-jupyter-spark-sql.md)

<!---HONumber=AcomDC_0224_2016-->
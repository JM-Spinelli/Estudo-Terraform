
<b>Terraform Show</b>- Comando que mostra todas as configurações de um recurso<br> 
<b>Terraform import</b>- O Terraform import, para usar, é necessário criar um arquivo com extensão .tf e nele declarar duas variávei ``id = "Id ou Name do resource"`` ``to = "resource.nome resource"``. Para efetivar o import, rodar o comando ``terraform plan -generate-config-out="nome-arquivo-saida".tf``<br> 


exemplo comando import 
```
import {
  
id = "cluster-1"
to = aws_ecs_cluster.cluster-1
}

```


pelo que vi, depois que import é feito, não precisa colar no main os recursos. E após recursos importados, deletar o arquivo de import para não dar conflito, pois o terraform entende que você está tentando executar o import de um recurso já importado (mesmo executando um tarraform refresh)


caso você execute um terraform show e ele mostre existir recursos, mas que não existem mais, basta executar o comando ``terraform refresh``, que assim o terraform vai entender as alterações. 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 

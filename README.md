
<b>Terraform Show</b>- Comando que mostra todas as configurações de um recurso<br> 
<b>Terraform import</b>- 

Tem duas formas de usar o terraform apply, sendo a a primeira: 
``terrafom import 'resource.nome' 'id do resource'``

exemplo: 
```
terraform apply aws_instance.EC2 i-039dfe5521b6c2303
```
Após isso, executar o comando ``terraform apply``. isso fará com que seja gerado um novo tf.state (Sim, nós usamos o import quando perdemos nossos arquivos de stado - tf.state e tf.state.backup)


O Terraform import, para usar, é necessário criar um arquivo com extensão .tf e nele declarar duas variávei ``id = "Id ou Name do resource"`` ``to = "resource.nome resource"``. Para efetivar o import, rodar o comando ``terraform plan -generate-config-out="nome-arquivo-saida".tf``<br> 


exemplo comando import 
```
import {
  
id = "cluster-1"
to = aws_ecs_cluster.cluster-1
}

```


pelo que vi, depois que import é feito, não precisa colar no main os recursos. E após recursos importados, deletar o arquivo de import para não dar conflito, pois o terraform entende que você está tentando executar o import de um recurso já importado (mesmo executando um tarraform refresh)


caso você execute um terraform show e ele mostre existir recursos, mas que não existem mais, basta executar o comando ``terraform refresh``, que assim o terraform vai entender as alterações. 


se importar recurso errado? removemos o recurso errado importado e importamos o recurso correto. 

Comando para remover recurso errado: 
``terraform state rm 'recurso.nome'``

exemplo:

```
terraform state rm aws_security_goup.SG-Custom 
```

e em seguida importar novo recurso: 

```
terraform import aws_security_group.SG-custom sg-0268d6a0a7c8ebe9e
``` 



ao criar uma vpc customizada ``aws_security_group```, é necessário definir os parâmetros ``ingress`` e ``egress``. Se definir um, mas não definir o outro, ele não será adicionado automaticamente (por default)



<b>Terraform Show</b>- O Terraform Show mostra os recursos e todas as informações dele. é um comando muito útil quando não se lembra exatamente de todos os atributos do recurso. Para que se tenha um visão mais clean e objetiva dos recursos existentes, usar o comando ``terraform state list``.<br> 



Arquivos Outputs: 
Neles eu defino exatamente aquelas informações que quero ver de determinado recurso. 


exemplo de conteúdo de um arquivo output.tf

```
output "instance_id"{
description = "id da minha instância" #Descrição
value = aws_intance.minha-ec2.id #aqui eu coloco o recurso, seu nome e o atributo desejado, no caso, id na instância
}
```

seu deseja, ainda pra essa istância, trazer mais informações dela, eu cri outro boloco de output 

```
output "instance_type" { #Entre aspas está o nome e ele não pode ser repetido
  description = "tipo da instância"
  value = aws_instance.minha-ec2.instance_type
}
```

Para uso de variáveis, usamos os comandos: 

criar o arquivo ``variable.tf``  e o conteúdo do arquivo é: 

```
variabe "tipo_instancia" {
  description = "Tipo da instância"
  type = "string"
  default = "t3.micro" #Valor que você quer passar

```

ao chamar esse valor no main, usamos: 

 ```
resource "aws_instance" "EC2" {
    instance_type = var.tipo_instancia
    ami = "ami-0bdd88bd06d16ba03"
```

************************************************************************************************************************************

O S3 é um forma segura de remota de guardar arquivos 


para isso, criaremos e configuraremos um arquivo state. 

arquivo: state_config.tf 

e dentro dele, montamos da seguinte forma: 

```
terraform {
backend "s3" { #Estou determinando que meu backend seja remoto
  bucket = "nome meu bucket"
  key = "pasta onde meu arquivo irá ficar"
  region = "regiao que minha conta está"
  profile = "meu usuário"
}

}
```
  Essas configurações foram feitas,mas caso ao der erro de beckend na hora de executar o comando ``terraform iinit``, é necessário migrar o stado de local para remoto ou vise e versa. 
  para isso, usamos o comando: ``terraform init -migrate-state`` 

  Mas veja, isso tudo aqui faz com que eu possa trabalhar com o meu tfstate remotamente. Ou seja, qualquer alteração que eu fizer, vai replicar no arquivo tfstate produtivo e utilizado por todos. O correto, é baixar uma cópia desse tfstate para sua máquina e trabalhar localmente com ele. Feito todos os testes e estando tudo ok, ai sobe a alteração no ambiente produtivo. 

  para voltar o tf.state para a configuração original, alteramos o arquivo `state_config.tf`` para:

  ```
terraform {
  backend "local" {
  }

}
```
e após isso, executar o comando: ``terraform init -migrate-state``assim ele muda o local e também baixa o arquivo 


<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 
<b></b>- <br> 

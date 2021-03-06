# Lab 13 - Amazon Lambda

Em este lab sobre **Lambda** aprenderemos alguns conceitos do modulo de Function as a Service (FaaS) / *serverless* da plataforma da AWS:
 - Criação de funções Lambda
 - Teste de funções 
 - Criação de triggers (via API Gateway)
 
## Pre-reqs

- A seguinte tabela, com nome `Atmosfera` criada no DynamoDB:
    * sala: *primary key*, string
    * temperatura: number
    * humidade: number
    ![](img/lambda0.png)

 ## Configuração do serviço

1. Acessar o serviço **Lambda**:
    ![](img/lambda1.png)

2. Criar uma nova função:
    ![](img/lambda2.png)
   
3. Criar uma primera função getTemperatura com Python como *runtime*:
    ![](img/lambda3.png)

4. Configurar o seguinte código para a função:
    ```python
    import json
    import boto3
    def lambda_handler(event, context):
       # TODO implement
       dynamodb = boto3.resource('dynamodb')
       tableTemperatures = dynamodb.Table('Atmosfera')
       response = tableTemperatures.scan()
       return {
          'statusCode': 200,
          'body': response['Items'][0]['temperatura']
       }
    ```
    O código lee o valor temperatura da tabela `Atmosfera` do DynamoDB.
    
    
 5. Fazer *deploy* do código:
     ![](img/lambda4.png)

 6. Vamos testar o código:
     ![](img/lambda5.png)

 7. Criamos um evento de testes. A entrada do evento (o arquivo `json`) é indeferente em este caso específico, pois a API não está lendo entrada:
     ![](img/lambda6.png)

 8. Criamos um evento de testes. A entrada do evento (o arquivo `json`) é indeferente em este caso específico, pois a API não está lendo entrada:
     ![](img/lambda6.png)

 9. Executar o evento de testes recém criado `testeGetTemperatura`:
     ![](img/lambda7.png)

 10. O teste deve falhar, pois a função não tem permissão para acessar o DynamoDB:
     ![](img/lambda8.png)

 11. No IAM, procurar o *role* da função:
     ![](img/lambda9.png)

 12. Adicionar uma nova *policy*: 
     ![](img/lambda10.png)

 13. A policy `AmazonDynamoDBReadOnlyAccess` vai dar acesso de leitura ao DynamoDB:
     ![](img/lambda11.png)

 14. Estado final da *role*:
     ![](img/lambda12.png)

 15. Ejecutar de novo o teste, agora deberia funcionar:
     ![](img/lambda13.png)

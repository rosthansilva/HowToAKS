# Politicas de Rede para AKS

Através de politicas de rede podemos parametrizar, limitar e liberar o trafego de rede entre nodes, pods e services. hoje temos duas opções :

1. Azure Network Polices
2. Calico Network Polices

> As politicas de rede para o AKS utlizam o modulo do kernel linux netfilter ( Iptables legado ) e por essa razão não é suportado para containers windows.

##### Azure Polices

- Suporta Azure CNI;
- Qualquer tipo de Politica Suportada;
- Log para trafego em Iptables;
- Suporte direto do time Azure.

##### Calico

- Suporta Azure CNI e Kubnet;
- Todos os tipos de Politica são Suportados;
- Modelo Extendido de politica;
- Suporte somente da comunidade;
- Log no host suportado

> a maior diferença entre os 2 modelos de segurança é o suporte a kubenet tonrnado Calico mais utilizado na garantia de compliance.

## Criando cluster com Calico Network Police

```
az aks create \
--ressource-group AksCalico \
--name AksCalicoCluster \
--node-count 3 \
--generate-ssh-keys \
--network-plugin kubenet \
--network-police calico \
--service-principal "ayuhjsia..." \
--client-secret "asaqjsia..."

```

# Cenário de Teste

Vamos criar o seguinte cenário no nosso cluster :
- Vamos criar 3 pods :
  - Webapp
  - Api
  - DB
- vamos abrir as portas necessárias de cada pod
  - Vamos criar um service para DB com ClusterIP (:15984)
  - Vamos criar um service com um clusterIP (:8080) para Api
  - Vamos Criar um loadbalance para o Pod web (:80)
- Vamos separar nosso ambiente em 2 Namespaces ( Kube-system | App )
- Vamos permitir consultas de DNS do namespace App

> é parte das melhores praticas criar uma politica que bloqueie todo o trafego de entrada e saída do nosso cluster e em seguida liberar o necessário se baseando em tags ( app=Web, app=DB, app=Api )

Hans On

```
#Conecte-se ao cloudshell já com seu cluster criado e no contexto correto e crie um Namespace
kubectl crate namespace App

#use o contetxo desse name space
kubectl config set-context --current --namespace App

#Agora, todos os seus comandos irão afetar somente esse namespace.
#Agora vamos criar o arquivo yaml do nosso ambiente
 -- config.yaml
apiVersion: apps/v1
kind: deployment
metadata: db
  name: db
  lebels:
    app: db
spec:
  replicas: 1
  selector:
    matchLabels:
      app: db
  template:
    metadata:
      app: db
    spec:
      containers:
      - name: couchdb
        image: couchdb:2.3.0
        ports:
        - containerPort: 5984
---
apiVersion: apps/v1
kind: Service
metadata: db
  name: db
spec:
  selector:
    app: db
  ports:
    - name : db
    port: 15984
    targetPort: 5984
  type: ClusterIp

```

# CAPITULO NÃO TERMINADO POR PURA PREGUIÇA DE ESCREVER OS YAML -- ACEITO PULL REQUEST DE QUE, TIVER ESSE TEMPO LIVRE.

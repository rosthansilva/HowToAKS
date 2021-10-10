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

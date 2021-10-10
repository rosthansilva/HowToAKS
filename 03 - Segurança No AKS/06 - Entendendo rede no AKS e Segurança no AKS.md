# Segurança de rede no AKS

Basicamente temos 2 formas de configurar a rede de nosso cluster kubernetes no azure. A primeira é Kubenet ( Default) e que estamos utilizando quando setamos todo o cluester na forma padrão. A Segunda é Azure CNI

## Kubenet

Kubenet é um plugin de configuração de rede do Kubernetes no AKS. Nele, nodes adiquirem um ip da rede do AKS e os Pods recebem um ip separado dessa rede em uam rede propria para os Pods. Todo trafego de origem é nateado para os Nodes no qual os pods estão rodando.

![Kubenet](https://github.com/rosthansilva/HowToAKS/blob/master/img/AKS-Kubenet.png)

```
 Beneficios do Kubenet
 - Mais ips para os Pods evitando limitações de rede;
 - Trafego nateado torna gestão e configuração mais simples;
 - O Numero máximo de pods que podemos rodar em um cluster em kubnetmode é 110 ( Uma rede /24 para os nodes Conseguimos rodar 27.610 pods. Diferente do AzureCNI, onde com o mesmo barramento conseguiriamos rodar 240 pods somente.  ).

```

> Em kubnet mode, não podemos ter comunicação entre os pods, para que os pods se comuniquem deverá utilizar um UDR ( User defined route ) ou Ip Forwarding entre pods através dos nodes.

## IMPORTANTE NOTA:

> Evite criar seu cluster kubernetes em uma vnet já existente dentro do azure. De a ele um espaço proprio para criação de recursos para que assim evitemos que ele venha a ser um limitador da sua infra tomando todo o espaço de ips ou o inverso, que sua infra não permita que o kubernetes cresça


### Explicitando configurações

Já ciente de que o AKS usa Kubnet como configuração default, podemos escolher criar um cluster com uma configuração customizada caso o façamos manualmente setando cada ponto necessário para nosso projeto.

```
az aks create --ressource-group AksDeNoia \
--name DrugsOnMe \
--node-count 7 \
--network-plugin kubenet \
--service-cdir 172.0.0.0/16 \
--dns-service-ip 172.0.0.40 \
--pod-cdir 172.220.0.0/24 \
--vnet-subnet-id $(az network venet subnet show --ressource-group AksDeNoia --vnetname AksVnet --name AksSubnet --query id -o tsv ) \
--service-principal "ayuhjsia..." \
--client-secret "asaqjsia..."
```

> Service CDIR é onde mora a infra relativa do kubernetes, é nela onde vamos criar nossos loadbalancers e fazer a interação com outros clusteres e ter conexão com o mundo externo. Nela tbm conectamos vpns e serviços de rede.

## Azure AzureCNI

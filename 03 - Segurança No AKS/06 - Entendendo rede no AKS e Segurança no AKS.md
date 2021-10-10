# Segurança de rede no AKS

Basicamente temos 2 formas de configurar a rede de nosso cluster kubernetes no azure. A primeira é Kubenet ( Default) e que estamos utilizando quando setamos todo o cluester na forma padrão. A Segunda é Azure CNI

## Kubenet

Kubenet é um plugin de configuração de rede do Kubernetes no AKS. Nele, nodes adiquirem um ip da rede do AKS e os Pods recebem um ip separado dessa rede em uam rede propria para os Pods. Todo trafego de origem é nateado para os Nodes no qual os pods estão rodando.

![Kubenet](https://github.com/rosthansilva/HowToAKS/blob/master/img/AKS-Kubenet.png)

```
 Beneficios do Kubenet
 - Mais ips para os Pods evitando limitações de rede
 - Trafego nateado torna gestão e configuração mais simples
```

> Em kubnet mode, não podemos ter comunicação entre os pods, para que os pods se comuniquem deverá utilizar um UDR ( User defined route ) ou Ip Forwarding entre pods através dos nodes.

 

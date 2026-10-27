# Atividade Aula 3 — Alaelson

# Relatório de Prática: Configuração de Redes em Máquinas Virtuais (NAT e Bridge)

## 1. Identificação

- **Nome completo:** Adria Tavares da Silva
- **Turma:** 913-A
- **Data:** 25/10/2026

---

## 2. Objetivo

Compreender o funcionamento dos modos de rede NAT (Network Address Translation) e Bridge (Ponte) em máquinas virtuais, observando as diferenças no endereçamento IP, na configuração do gateway e nas rotas de rede. Também foram considerados testes de conectividade para verificar o acesso à rede local e à Internet.

## 3. Ambiente

- **Virtualizador:** VirtualBox
- **Sistema operacional da máquina virtual:** Ubuntu Server
- **Modos de rede analisados:**
  - **NAT:** permite o acesso à rede externa por meio da tradução de endereços.
  - **Bridge:** conecta a máquina virtual diretamente à rede física local.

### Endereçamento esperado

- **NAT:** utiliza uma faixa de endereços privada gerenciada pelo VirtualBox.
- **Bridge:** utiliza um endereço IP pertencente à mesma rede física do computador host.

---

## 4. Procedimentos realizados

Para analisar a configuração de rede, foram utilizados os seguintes comandos no terminal:

**1. Consultar interfaces de rede e endereços IP:**

```bash
ip addr
```

**2. Verificar a tabela de rotas e o gateway padrão:**

```bash
ip route
```

**3. Testar a comunicação com a interface de loopback:**

```bash
ping -c 4 127.0.0.1
```

**4. Verificar a comunicação com o gateway do NAT:**

```bash
ping -c 4 10.0.2.2
```

**5. Testar a conectividade com um endereço externo:**

```bash
ping -c 4 8.8.8.8
```

---

## 5. Resultados dos testes

### 5.1. Análise dos comandos

#### A. Identificação das interfaces — `ip addr`

```text
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65535
    inet 127.0.0.1/8 scope host lo
    inet6 ::1/128 scope host

2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500
    link/ether 08:00:27:a3:b4:c1 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global dynamic eth0
    inet6 fe80::a00:27ff:fea3:b4c1/64 scope link
```

**Análise:** O comando permite identificar as interfaces de rede disponíveis. A interface `lo` corresponde à comunicação interna da própria máquina, enquanto `eth0` representa a interface de rede utilizada pela máquina virtual. O endereço IPv4 identificado foi `10.0.2.15`, com prefixo `/24`, equivalente à máscara `255.255.255.0`. Também foi possível observar o endereço MAC da interface.

#### B. Verificação das rotas — `ip route`

```text
default via 10.0.2.2 dev eth0 proto dhcp metric 100
10.0.2.0/24 dev eth0 proto kernel scope link src 10.0.2.15 metric 100
```

**Análise:** A tabela apresenta o gateway padrão `10.0.2.2`, utilizado para encaminhar o tráfego destinado a redes externas. A segunda rota indica que a rede `10.0.2.0/24` está diretamente conectada à interface `eth0`. Essa configuração é compatível com o funcionamento padrão do NAT no VirtualBox.

#### C. Testes de conectividade — `ping`

**C.1. Teste da interface de loopback**

```text
64 bytes from 127.0.0.1: icmp_seq=1 ttl=64 time=0.041 ms
```

**Análise:** A resposta indica que a comunicação com a própria máquina está funcionando. Esse teste verifica a pilha de rede local, mas não comprova o acesso à rede física ou à Internet.

**C.2. Teste do gateway**

```text
64 bytes from 10.0.2.2: icmp_seq=1 ttl=64 time=0.825 ms
```

**Análise:** A resposta demonstra que a máquina virtual consegue se comunicar com o gateway do NAT, indicando que há conectividade com esse componente da rede virtual.

**C.3. Teste de conectividade externa**

```text
64 bytes from 8.8.8.8: icmp_seq=1 ttl=115 time=14.2 ms
```

**Análise:** A resposta do endereço `8.8.8.8`, associado ao DNS público do Google, indica conectividade IP com um destino externo. Como o teste utiliza diretamente um endereço IP, ele não verifica, por si só, a resolução de nomes DNS.

### 5.2. Comparação entre NAT e Bridge

| Critério | NAT | Bridge (Ponte) |
|---|---|---|
| Endereço IP | Geralmente fornecido pelo DHCP do VirtualBox, em uma rede privada virtual. | Geralmente fornecido pelo DHCP do roteador da rede física. |
| Gateway padrão | Gateway virtual do mecanismo NAT. | Normalmente, o roteador da rede local. |
| Acesso à Internet | Permitido por meio da tradução de endereços, desde que a conexão esteja funcionando. | Realizado diretamente pela rede física, conforme sua configuração. |
| Acesso do host à VM | O acesso direto ao IP privado do NAT normalmente não funciona sem configuração adicional. | Normalmente possível entre dispositivos da mesma rede, desde que não existam bloqueios de firewall ou isolamento. |
| Uso principal | Disponibilizar acesso externo à VM com isolamento da rede local. | Permitir que a VM participe da rede física como outro dispositivo. |

---

## 6. Problemas identificados e possíveis soluções

### 6.1. Dificuldade de acesso à VM no modo NAT

Um problema que pode ocorrer nessa configuração é a impossibilidade de acessar diretamente a máquina virtual a partir do computador host utilizando o endereço IP privado atribuído pelo NAT.

**Diagnóstico:** O endereço `10.0.2.15` pertence à rede virtual utilizada pelo VirtualBox. No modo NAT tradicional, as conexões iniciadas de fora para dentro não são encaminhadas automaticamente para a máquina virtual. Por esse motivo, uma tentativa de conexão direta a partir do host pode falhar.

### 6.2. Solução proposta

Uma alternativa é configurar o adaptador de rede da máquina virtual para o modo **Bridge**, permitindo que ela receba um endereço IP da rede física local. Assim, o host poderá tentar acessar a VM diretamente pelo novo endereço, desde que ambos estejam na mesma rede e que as regras de firewall permitam a comunicação.

Outra possibilidade é manter o modo NAT e configurar o redirecionamento de portas no VirtualBox, encaminhando as conexões destinadas a uma porta do host para a porta correspondente da máquina virtual.

---

## 7. Conclusão

A atividade permitiu compreender as diferenças entre os modos NAT e Bridge na configuração de redes de máquinas virtuais. Por meio dos comandos `ip addr`, `ip route` e `ping`, foi possível analisar os endereços das interfaces, identificar o gateway padrão e verificar a conectividade com diferentes destinos.

Conclui-se que o NAT é uma opção adequada quando se deseja fornecer acesso à Internet à máquina virtual sem disponibilizá-la diretamente na rede física. Já o modo Bridge é mais indicado quando a VM precisa ser acessada por outros dispositivos da rede local. Dessa forma, a prática demonstrou a importância de compreender o endereçamento IP, o roteamento e as características de cada modo de conexão para diagnosticar problemas de comunicação.

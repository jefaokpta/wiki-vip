# Rotas Jupiter

## Visão geral

Este documento consolida o mapeamento das rotas ativas no ambiente Jupiter, incluindo os techs utilizados para saídas de voz e os centros de custo associados.

## Resumo das rotas

As rotas podem variar conforme a operadora, mas há uma preferência para as saídas STFC pela Vivo. Em geral, a configuração tende a seguir o padrão abaixo:

- Fixo 11: Vivo STFC - New Voice 25309 - Pentagono 1558
- Móvel 11: Vivo STFC - Telglobe 6060 - New Voice 25133
- Fixo LDN: Vivo STFC - New Voice 25309 - Pentagono 1558
- Móvel LDN: Pentagono 1558 - Telglobe 6060 - New Voice 25133

## Tech 103#

Tech utilizado para saídas ITX para todos os tipos de saída, incluindo fixo local, fixo LDN, móvel local e móvel LDN.

### Centros de custo

- Fixo 11 = 2.23
- Móvel 11 = 5.02
- Fixo LDN = 3.02
- Móvel LDN = 6.02

### Configuração padrão das operadoras

- Fixo 11 = Vivo STFC - New Voice 25309 - Pentagono 1558
- Móvel 11 = Vivo STFC - Telglobe 6060 - New Voice 25133
- Fixo LDN = Vivo STFC - New Voice 25309 - Pentagono 1558
- Móvel LDN = Pentagono 1558 - Telglobe 6060 - New Voice 25133

> Ao lado do nome da operadora, aparece o tech prefix utilizado por essa rota.

## Tech 104#

Tech utilizado para saídas de Bina inteligente que apresentam destino em números móveis.

### Centros de custo

- Móvel 11 = 5.03
- Móvel LDN = 6.03

### Configuração padrão

- Móvel 11 = Telglobe 4040 - Flux 5555
- Móvel LDN = Telglobe 4040 - Flux 5555

> Ao lado do nome da operadora, aparece o tech prefix utilizado por essa rota.

## Tech 105#

Tech utilizado para saídas de Bina inteligente que apresentam destino em números fixos.

### Centros de custo

- Móvel 11 = 5.03
- Móvel LDN = 6.03

### Configuração padrão

- Móvel 11 = Pentagono 2766 - Telglobe 3030
- Móvel LDN = Telglobe 2766 - Telglobe 3030

> Ao lado do nome da operadora, aparece o tech prefix utilizado por essa rota.

## Tech 106#

Tech utilizado para saídas pela operadora Fale Sempre. A saída precisa que o número A seja da própria operadora, de forma semelhante ao funcionamento da saída STFC.

### Centros de custo

- Fixo 11 = 2.26
- Móvel 11 = 5.06
- Fixo LDN = 3.06
- Móvel LDN = 6.06

### Configuração padrão

- Fixo 11 = Fale Sempre 17094#
- Móvel 11 = Fale Sempre 17094#
- Fixo LDN = Fale Sempre 17094#
- Móvel LDN = Fale Sempre 17094#

> Ao lado do nome da operadora, aparece o tech prefix utilizado por essa rota.

## Saídas DDI

Saídas sem tech prefix próprio, utilizando 00 para identificar a saída DDI.

- Centro de custo = 4.0
- Rotas = IDT 0026 - Flux 909000

> Ao lado do nome da operadora, aparece o tech prefix utilizado por essa rota.

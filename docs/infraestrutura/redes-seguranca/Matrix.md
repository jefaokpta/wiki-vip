# Matrix

## Acesso
http://IP_MATRIX_FINAL.182:8088/

## Portas

| Porta | Descrição | Status |
|-------|-----------|--------|
| Porta 1 | Switch | Inativo |
| Porta 2 | Golden cable Vivo link 1 - 187.92.249.134/30 (GW 187.92.249.133) | Ativo |
| Porta 3 | Golden cable Vivo link 2 - 177.61.174.98/30 (GW 177.61.174.97) | Ativo |
| Porta 4 | - | Vazia |
| Porta 5 | Switch (configuração Lan Matrix 200.201.212.182/24) | Ativo |

## Endereços IP

### Vivo
- **Address**: 187.92.249.134/30
- **Network**: 187.92.249.132
- **Interface**: ether2

### Vivo2
- **Address**: 177.61.174.98/30
- **Network**: 177.61.174.96
- **Interface**: ether3

### Lan (IPs Summit)
- **Address**: 172.16.1.254/16
- **Network**: 172.16.0.0
- **Interface**: ether5

### Heimdall (Range IPs Matrix)
- **Address**: 200.201.212.182/24
- **Network**: 200.201.212.0
- **Interface**: ether5

## Configuração BGP

### Instances
- **AS**: 65321
- **Router ID**: 200.201.212.181
- **Client to client reflection**: Habilitado
- Restante: desativado/em branco

### Peers

#### VIVO
- **Remote address**: 187.92.249.133
- **Remote AS**: 10429
- **Nexthop choice**: Default
- **Hold time**: 180
- **Default originate**: never
- **Address families**: IP
- **Update source**: Ether2
- **Cisco VPLS NLRI Length Format**: auto bits
- Restante: desativado/em branco

#### VIVO2
- **Remote address**: 177.61.174.97
- **Remote AS**: 10429
- **Nexthop choice**: Default
- **Hold time**: 180
- **Default originate**: never
- **Address families**: IP
- **Update source**: Ether2
- **Cisco VPLS NLRI Length Format**: auto bits
- Restante: desativado/em branco

## Hosts

### HOST01
**URL**: https://IP_MATRIX:1443/

**VMs**:
- Summit EMS
- BRD (Trafego)

### HOST02
**URL**: https://IP_MATRIX:2443/

**VMs**:
- Summit SSW

### HOST03
**URL**: https://IP_MATRIX:3443/

**VMs**:
- Summit RSW

## Referências
- [Acessos e Documentação](https://drive.google.com/file/d/14Q9hbFCr3-Rkr37x0elSW28KVtkBe9aQ/view)

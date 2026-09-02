---
title: Summit - Fluxo de Ligações
---

# Fluxo de Ligações — Summit

Arquivo com o fluxo de roteamento e políticas utilizadas no Summit (Jupiter / Vivo).

```
Fluxo de saida de ligação:
Jupiter > Policies > Roteamento > Inbound SSW (ID 1)

Bloqueio temporario no numero 11940087541 (devido a problemas de identificação da operadora)
Se Fixo 11 = policy 10
Se Fixo LDN = policy 10
Se VC1 = policy 10
Se VC2 = Rota 11110001154 (Vivo_TR) (Ainda não foi utilizada, necessario ajuste e testes)
Se VC3 = Rota 11110001154 (Vivo_TR) (Ainda não foi utilizada, necessario ajuste e testes)

//Block temporario
(matches $CALLED.NUM '11940087541') {
  deny;
}

// STFC
(matches $CALLED.NUM '[2-6][0-9]{7}') {
  setvar $PTYPE 'LCL';
  left $CN $CALLING.NUM 2;
  prepend $CALLED.NUM $CN;
  policy 10;
  //route 11110001151;
}

(matches $CALLED.NUM '[7-9][0-9]{7,8}') {
  setvar $PTYPE 'LCLVC1';
  left $CN $CALLING.NUM 2;
  prepend $CALLED.NUM $CN;
  //prepend $CALLED.NUM '0';
  policy 10;
  //route 11110001155;
}

(matches $CALLED.NUM '11[7-9][0-9]{7,8}') {
  setvar $PTYPE 'LCLVC1';
  //leftstrip $CALLED.NUM 1;
  //left $CSP $CALLED.NUM 2;
  //leftstrip $CALLED.NUM 2;
 //prepend $CALLED.NUM '0';
  policy 10;
  //route 11110001151;
}

(matches $CALLED.NUM '0[1-9]{2}[2-6][0-9]{7}') {
  setvar $PTYPE 'LDN';
  //leftstrip $CALLED.NUM '0';
  //leftstrip $CALLED.NUM 1;
  //left $CSP $CALLED.NUM 2;
  //leftstrip $CALLED.NUM 2;
  policy 10;
  //route 11110001154;
}

(matches $CALLED.NUM '0[1-9]{2}[7-9][0-9]{7,8}') {
  setvar $PTYPE 'VC2VC3';
  //leftstrip $CALLED.NUM 1;
  //left $CSP $CALLED.NUM 2;
  //leftstrip $CALLED.NUM 2;
  //policy 10;
  route 11110001154;
}

(matches $CALLED.NUM '0[1-9]{2}[1-9]{2}[2-9][0-9]{7,8}') {
  setvar $PTYPE 'LDNVC2VC3';
  leftstrip $CALLED.NUM 1;
  left $CSP $CALLED.NUM 2;
  leftstrip $CALLED.NUM 2;
  //policy 10;
  route 11110001154;
}

(matches $CALLED.NUM '0[1-9]{2}[1-9]{2}[2-9][0-9]{7,8}') {
  setvar $PTYPE 'LDNVC2VC3';
  leftstrip $CALLED.NUM 1;
  left $CSP $CALLED.NUM 2;
  leftstrip $CALLED.NUM 2;
  //policy 10;
  route 11110001154;
}

(matches $CALLED.NUM '00[1-9]{2}[1-9][0-9]{4,20}') {
  setvar $PTYPE 'LDI';
  leftstrip $CALLED.NUM 2;
  left $CSP $CALLED.NUM 2;
  leftstrip $CALLED.NUM 2;
  policy 30;
}

(matches $CALLED.NUM '0300[0-9]{6,7}') {
  setvar $PTYPE '0300';
  leftstrip $CALLED.NUM 1;
  policy 10;
}

(matches $CALLED.NUM '0303[0-9]{6,7}') {
  setvar $PTYPE '0303';
  leftstrip $CALLED.NUM 1;
  policy 10;
}

(matches $CALLED.NUM '0500[0-9]{6,7}') {
  setvar $PTYPE '0500';
  leftstrip $CALLED.NUM 1;
  policy 10;
}

(matches $CALLED.NUM '0800[0-9]{6,7}') {
  setvar $PTYPE '0800';
  //leftstrip $CALLED.NUM 1;
  //policy 10;
  route 11110001152;
}

(matches $CALLED.NUM '0900[0-9]{6,7}') {
  setvar $PTYPE '0900';
  leftstrip $CALLED.NUM 1;
  policy 10;
}

(matches $CALLED.NUM '1[0-9]{2,4}') {
  setvar $PTYPE 'SERVICE';
  //policy 20;
  route 11110001151;
}

(matches $CALLED.NUM '9090[2-9][0-9]{7,8}') {
  setreverse;
  setvar $PTYPE 'LCLVC1';
  setvar $COLLECT '1';
  leftstrip $CALLED.NUM 4;
  left $CN $CALLING.NUM 2;
  prepend $CALLED.NUM $CN;
  policy 10;
}

(matches $CALLED.NUM '90[1-9]{2}[1-9]{2}[2-9][0-9]{7,8}') {
  setvar $PTYPE 'LDNVC2VC3';
  setvar $COLLECT '1';
  leftstrip $CALLED.NUM 2;
  left $CSP $CALLED.NUM 2;
  leftstrip $CALLED.NUM 2;
  policy 10;
}


Policies > Roteamento > Core Out (ID 10)

Se RN de A diferente de "55648" (VIP) = Rejeitar ligação fazendo com que as ligações saiam pelas operadoras SCM do Jupiter
Se RN de A igual "55648" (VIP) e numero de B igual fixo 11 = policy 51
Se RN de A igual "55648" (VIP) e numero de B igual fixo LDN = policy 55
Se RN de A igual "55648" (VIP) e numero de B igual VC1 = policy 52
Se RN de A igual "55648" (VIP) e numero de B igual VC2 = policy 56
Se RN de A igual "55648" (VIP) e numero de B igual VC3 = policy 57

(rislookup $CALLING.NUM 'A') {
  (rislookup $CALLED.NUM 'B') {
    //setvar $LNP $RISLOOKUP.NP.B;

    setcdr 'CALLING' $CALLING.NUM;
    setcdr 'CALLED' $CALLED.NUM;

    (not equals $RISLOOKUP.RN.A '55648') {
      deny;
    }

    setvar $CALLING.NOA 'SUBSCRIBER';

    (equals $RISLOOKUP.ST.B 'F') {
      (equals $RISLOOKUP.LG.A $RISLOOKUP.LG.B) {
        (equals $PTYPE 'LCL') {
          setvar $CTYPE 'LCL';
          policy 51;
        }
        setvar $ERROR 'MISDIAL';
        policy 99;
        setvar $PTYPE 'LCLVC1';
        setvar $CTYPE 'LCL';
        setcdr 0 'LCL';
        policy 51;
      }
      (equals $PTYPE 'LDNVC2VC3') {
        setvar $CTYPE 'LDN';
        setcdr 0 'LDN';
        policy 55;
      }
      (equals $PTYPE 'LDN') {
        setvar $CTYPE 'LDN';
        setcdr 0 'LDN';
        policy 55;
      }
      setvar $ERROR 'MISDIAL';
      policy 99;
    }
    (equals $RISLOOKUP.ST.B 'M') {
      left $CNA2 $CALLING.NUM 2;
      left $CNA1 $CALLING.NUM 1;
      left $CNB2 $CALLED.NUM 2;
      left $CNB1 $CALLED.NUM 1;
      
      (equals $CNA2 $CNB2) {
        (equals $PTYPE 'LCLVC1') {
          setvar $CTYPE 'VC1';
          setcdr 0 'VC1';
          policy 52;
        }
        setvar $ERROR 'MISDIAL';
        policy 99;
        setvar $PTYPE 'LCLVC1';
        setvar $CTYPE 'VC1';
        setcdr 0 'VC1';
        policy 52;
      }
      (equals $CNA1 $CNB1) {
        (equals $PTYPE 'LDNVC2VC3') {
          setvar $CTYPE 'VC2';
          setcdr 0 'VC2';
          policy 56;
        }
        setvar $ERROR 'MISDIAL';
        policy 99;
        setvar $PTYPE 'LDNVC2VC3';
        setvar $CTYPE 'VC2';
        setcdr 0 'VC2';
        policy 56;
      }
      (equals $PTYPE 'LDNVC2VC3') {
        setvar $CTYPE 'VC3';
        setcdr 0 'VC3';
        policy 57;
      }
      setvar $ERROR 'MISDIAL';
      policy 99;
    }
    (equals $RISLOOKUP.ST.B 'E') {
      left $CNA2 $CALLING.NUM 2;
      left $CNA1 $CALLING.NUM 1;
      left $CNB2 $CALLED.NUM 2;
      left $CNB1 $CALLED.NUM 1;
      
      (equals $CNA2 $CNB2) {
        (equals $PTYPE 'LCLVC1') {
          setvar $CTYPE 'VC1';
          setcdr 0 'VC1';
          policy 52;
        }
        setvar $ERROR 'MISDIAL';
        policy 99;
        setvar $PTYPE 'LCLVC1';
        setvar $CTYPE 'VC1';
        setcdr 0 'VC1';
        policy 52;
      }
      (equals $CNA1 $CNB1) {
        (equals $PTYPE 'LDNVC2VC3') {
          setvar $CTYPE 'VC2';
          setcdr 0 'VC2';
          policy 56;
        }
        setvar $ERROR 'MISDIAL';
        policy 99;
        setvar $PTYPE 'LDNVC2VC3';
        setvar $CTYPE 'VC2';
        setcdr 0 'VC2';
        policy 56;
      }
      (equals $PTYPE 'LDNVC2VC3') {
        setvar $CTYPE 'VC3';
        setcdr 0 'VC3';
        policy 57;
      }
      setvar $ERROR 'MISDIAL';
      policy 99;
    }
    (equals $RISLOOKUP.ST.B '3') {
      (equals $PTYPE '0300') {
        setcdr 0 '0300';
        policy 59;
      }
      (equals $PTYPE '0303') {
        setcdr 0 '0303';
        policy 59;
      }
      setvar $ERROR 'MISDIAL';
      policy 99;
    }
    (equals $RISLOOKUP.ST.B '5') {
      (equals $PTYPE '0500') {
        setcdr 0 '0500';
        policy 59;
      }
      setvar $ERROR 'MISDIAL';
      policy 99;
    }
    (equals $RISLOOKUP.ST.B '8') {
      (equals $PTYPE '0800') {
        setcdr 0 '0800';
        policy 59;
      }
      setvar $ERROR 'MISDIAL';
      policy 99;
    }
    (equals $RISLOOKUP.ST.B '9') {
      (equals $PTYPE '0900') {
        setcdr 0 '0900';
        policy 59;
      }
      setvar $ERROR 'MISDIAL';
      policy 99;
    }
    deny;
  }
  setvar $ERROR 'SERVICEUNAVAILABLE';
  policy 99;
}

Policies > Roteamento > Forward LC (ID 51)

Se RN de B for igual a "55648" (VIP) = policy 11 (Entrada)
Se RN de B for igual a "55115" (Vivo) = Rota 1151 (Vivo LC)
Se RN de B for igual a "55121" (Claro) = Rota 1151 (Vivo LC)
Se RN de B for qualquer outro = Rota 1151 (Vivo LC)
Ja estão separados caso haja a necessidade de ajustes futuros, caso seja necessário separar as operadoras.

(equals $RISLOOKUP.RN.B '55648') {
  policy 11;
}

//VIVO
(equals $RISLOOKUP.RN.B '55115')  {
  left $CNA $CALLING.NUM 2;
  leftstrip $CALLED.NUM 2;
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1151';

  route $ROUTE;
}

//CLARO
(equals $RISLOOKUP.RN.B '55121')  {
  //left $CNA $CALLING.NUM 2;
  leftstrip $CALLED.NUM 2;
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1151';

  route $ROUTE;
}

(startswith $RISLOOKUP.RN.B '55') {
  //left $CNA $CALLING.NUM 2;
  leftstrip $CALLED.NUM 2;
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1151';

  route $ROUTE;
}

//(startswith $RISLOOKUP.RN.B '55') {
  //left $CNA $CALLING.NUM 2;
 //leftstrip $CALLED.NUM 2;
  //prepend $CALLED.NUM '55';
  //left $ROUTE $CALLING.NUM 2;

  //deny;
  //route 50002;
//}


Policies > Roteamento > Forward LD (ID 55)

Se RN de B for igual a "55648" (VIP) = policy 11 (Entrada)
Se RN de B for qualquer outro = Rota 1154 (Vivo TR)

(equals $RISLOOKUP.RN.B '55648') {
  policy 11;
}

//VIVO
//(equals $RISLOOKUP.RN.B '55320') {
//  prepend $CALLED.NUM '0';
//  left $ROUTE $CALLING.NUM 2;
//  append $ROUTE $RISLOOKUP.LG.A;
//  append $ROUTE '1153';
//  route $ROUTE;
//}

// Encaminhamento local/transito para Telefonica
(startswith $RISLOOKUP.RN.B '55') {
  //prepend $CALLED.NUM '0';
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1154';
  route $ROUTE; 
 }


Policies > Roteamento > Forward VC1 (ID 52)

Se RN de B for igual a "55648" (VIP) = policy 11 (Entrada)
Se RN de B for igual a "55320" (Vivo) = Rota 1155 (Vivo VC1)
Se RN de B for igual a "55321" (Claro) = Rota 1151 (Vivo LC)
Se RN de B for igual a "55341" (TIM) = Rota 1151 (Vivo LC)
Se RN de B for qualquer outro = Rota 1151 (Vivo LC)
Ja estão separados caso haja a necessidade de ajustes futuros, caso seja necessário separar as operadoras.

(equals $RISLOOKUP.RN.B '55648') {
  policy 11;
}

//VIVO
(equals $RISLOOKUP.RN.B '55320') {
  //left $CNA $CALLING.NUM 2;
  //leftstrip $CALLED.NUM 2;
  left $ROUTE $CALLING.NUM 2;
  prepend $CALLED.NUM $CN;
  prepend $CALLED.NUM '0';
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1155';

  route $ROUTE;
}

//Claro
(equals $RISLOOKUP.RN.B '55321') {
  //left $CNA $CALLING.NUM 2;
  leftstrip $CALLED.NUM 2;
  left $ROUTE $CALLING.NUM 2;
  prepend $CALLED.NUM $CN;
  //prepend $CALLED.NUM '0';
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1151';

  route $ROUTE;
}

//TIM
(equals $RISLOOKUP.RN.B '55341') {
  //left $CNA $CALLING.NUM 2;
  leftstrip $CALLED.NUM 2;
  left $ROUTE $CALLING.NUM 2;
  prepend $CALLED.NUM $CN;
  //prepend $CALLED.NUM '0';
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1151';

  route $ROUTE;
}

(startswith $RISLOOKUP.RN.B '55') {
  //left $CNA $CALLING.NUM 2;
  leftstrip $CALLED.NUM 2;
  left $ROUTE $CALLING.NUM 2;
  prepend $CALLED.NUM $CN;
  //prepend $CALLED.NUM '0';
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1151';

  route $ROUTE;
}

//(startsWith block omitted)


Policies > Roteamento >Forward VC2 (ID 56)

Se RN de B for igual a "55648" (VIP) = policy 11 (Entrada)
Se RN de B for igual a "55320" (Vivo) = Rota 1153 (Vivo LD sem CSP)
Se RN de B for qualquer outro = Rota 1153 (Vivo LD sem CSP)

(equals $RISLOOKUP.RN.B '55648') {
  policy 11;
}

//VIVO
(equals $RISLOOKUP.RN.B '55320') {
  prepend $CALLED.NUM '0';
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1153';
  route $ROUTE;
}

// Encaminhamento local/transito para Telefonica
(startswith $RISLOOKUP.RN.B '55') {
  prepend $CALLED.NUM '0';
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1153';
  route $ROUTE; 
 }


Policies > Roteamento >Forward VC3 (ID 57)

Se RN de B for igual a "55648" (VIP) = policy 11 (Entrada)
Se RN de B for igual a "55320" (Vivo) = Rota 1153 (Vivo LD sem CSP)
Se RN de B for qualquer outro = Rota 1153 (Vivo LD sem CSP)

(equals $RISLOOKUP.RN.B '55648') {
  policy 11;
}

//VIVO
(equals $RISLOOKUP.RN.B '55320') {
  prepend $CALLED.NUM '0';
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1153';
  route $ROUTE;
}

// Encaminhamento local/transito para Telefonica
(startswith $RISLOOKUP.RN.B '55') {
  prepend $CALLED.NUM '0';
  left $ROUTE $CALLING.NUM 2;
  append $ROUTE $RISLOOKUP.LG.A;
  append $ROUTE '1153';
  route $ROUTE; 
 }


Fluxo de entrada de ligação:
Vivo > Policies > Roteamento > Inbound ITX (ID 2)

Qualquer entrada > Policy 11


// (remaining inbound flow omitted for brevity)

Rotas:
1151: Vivo SPO LC: Saidas para fixo local, celulares outras operadoras
1152: Vivo LD15: Saida para LDN fixo ou celular porem com mais custos
1153: Vivo LD sem CSP: Saida para LDN celulares
1154: Vivo TR: Saida para LDN fixo 
1155: Vivo VC1: Saida para celulares Vivo VC1 

IPs:
1151: 10.11.160.207
1152: 10.11.160.208
1153: 10.11.160.209
1154: 10.11.160.210
1155: 10.11.160.211
```

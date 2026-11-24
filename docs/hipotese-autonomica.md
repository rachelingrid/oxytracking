# Instabilidade ventilatória e comportamento autonômico noturno pós-COVID-19

Documento de fundamentação do eixo investigativo que motivou o **Oxytracking**.

Mantido separado do `README` de propósito: a hipótese aqui desenvolvida **não é** o que a
ferramenta afirma medir. A ferramenta calcula índices oximétricos descritivos. Este documento
registra a pergunta de pesquisa que os tornou interessantes.

---

## 1. Problema

Fadiga persistente, sonolência, névoa cognitiva, disautonomia e intolerância ao esforço são
simultaneamente manifestações de Long COVID e de distúrbio respiratório do sono não tratado.
A sobreposição produz erro nos dois sentidos: apneia incidente rotulada como Long COVID e
não investigada; e sintomas de Long COVID atribuídos a uma apneia leve que não os explica.

---

## 2. Hipótese

**Oligoemia de tronco encefálico pós-COVID-19, com injúria preferencial do locus coeruleus,
produz fenótipo respiratório do sono misto — componente obstrutivo e componente central —
a partir de um único evento vascular.**

O diagrama causal correspondente está em [`modelo_causal.svg`](modelo_causal.svg).

---

## 3. Por que hipoperfusão, e não neuroinvasão

A hipótese de infecção neuronal produtiva enfraqueceu. Achados de autópsia mostraram ativação
microglial e astrogliose em tronco **desproporcionais à carga viral detectável**, que era baixa
ou ausente. Estudos de lesão microvascular encefálica demonstraram extravasamento de
fibrinogênio, microtrombos e infiltrado perivascular **sem vírus identificável no parênquima**.

O padrão é de endotelite com neuroinflamação secundária. Isso tem vantagem explicativa:
dá conta de o dano ser **regional** em vez de difuso.

---

## 4. Por que o locus coeruleus

Perfil de vulnerabilidade seletiva: núcleo pequeno, densamente vascularizado, com axônios
amielínicos longos e extensamente ramificados, e custo metabólico alto por neurônio.
É o perfil que sofre primeiro em queda de perfusão — o mesmo argumento que sustenta sua
vulnerabilidade precoce em neurodegeneração.

**Ressalva:** este é raciocínio por analogia. Plausível, não demonstrado em contexto pós-COVID.

---

## 5. Os três ramos

### 5.1 Ramo obstrutivo — LC → núcleo hipoglosso → genioglosso

Os motoneurônios hipoglossos (XII) recebem drive noradrenérgico tônico excitatório do locus
coeruleus e dos grupos A5/A7. Esse drive é o principal determinante do tônus do genioglosso,
o dilatador faríngeo primário.

O LC é neurônio **REM-off**: cessa disparo no sono REM. A retirada do drive noradrenérgico
sobre o XII é a explicação clássica da atonia de via aérea superior em REM, e de por que
os eventos obstrutivos se concentram nesse estágio.

**Previsão:** injúria de LC produz perda de tônus dilatador faríngeo **já em NREM**,
antecipando e intensificando a colapsabilidade em relação ao padrão usual.

### 5.2 Ramo autonômico — NTS / núcleo ambíguo

Aferência do reflexo de via aérea superior e eferência parassimpática cardíaca.
A mesma região vulnerável. Previsão: desequilíbrio simpático-vagal com bradicardia noturna.

### 5.3 Ramo quimiorreceptor — RTN / parafacial

Incluído no modelo **como contraponto, não como suporte**.

A resposta ventilatória ao CO₂ é majoritariamente do núcleo retrotrapezoide (Phox2b),
não do corpo carotídeo. E lesão quimiorreceptora **reduz** o ganho do controlador —
ganho reduzido *estabiliza* a ventilação. Invocar dano quimiorreceptor para explicar
respiração periódica anda na direção contrária: oscilação exige ganho alto, ou atraso longo.

Este ramo está no diagrama para que a objeção fique antecipada, não escondida.

---

## 6. O fechamento do laço — por que o atraso importa mais que o ganho

*Loop gain* = ganho do controlador × ganho da planta × atraso circulatório.

Quando o ramo autonômico produz bradicardia e queda de débito cardíaco, o tempo de trânsito
pulmão-quimiorreceptor se alonga. O comprimento do ciclo em respiração periódica
**correlaciona-se com o tempo de circulação** — achado clássico da respiração de Cheyne-Stokes
na insuficiência cardíaca.

**Previsão testável:** em pós-COVID com bradicardia noturna, o período dominante do espectro
de SpO₂ deve ser **longo** (> 60 s) e correlacionar-se inversamente com a frequência de pulso
média. Período longo com coerência SpO₂–FP fraca favorece atraso; período curto com coerência
alta e sustentada favorece evento obstrutivo com resposta autonômica estereotipada.

É precisamente isso que o Oxytracking mede — e a razão de o período dominante e a coerência
serem reportados lado a lado.

---

## 7. Convergência com dano microvascular

Apneia e Long COVID compartilham vias finais: disfunção endotelial com redução da
biodisponibilidade de óxido nítrico, estresse oxidativo por ciclos de hipóxia-reoxigenação
(isquemia-reperfusão em miniatura, repetida centenas de vezes por noite), inflamação de
baixo grau persistente, hiperatividade simpática.

**Questão em aberto:** o distúrbio respiratório do sono **potencializa** o dano microvascular
pós-COVID de forma sinérgica, ou apenas soma? Não há, até onde se sabe, desenho experimental
que separe as duas possibilidades de forma convincente.

Se a termografia periorbital servir como marcador de perfusão, a pergunta ganha desfecho
mensurável: a assinatura térmica de hipoperfusão em pós-COVID é amplificada na presença de
distúrbio respiratório do sono coexistente?

---

## 8. Limitações do modelo

1. A seta de SARS-CoV-2 para oligoemia é o elo mais frágil. Sustentada por autópsia e imagem,
   sem demonstração longitudinal em paciente vivo com o desfecho respiratório medido.
2. A vulnerabilidade seletiva do LC é analogia, não demonstração.
3. Oximetria não separa obstrutivo de central. A confirmação do fenótipo misto exige
   polissonografia tipo I com esforço torácico-abdominal, EEG e posição corporal.
4. Bradicardia noturna tem causa medicamentosa muito mais frequente que causa neurológica.
   Este é o confundidor dominante e precede qualquer outra interpretação.

---

## 9. O que seria preciso para testar

- Coorte pós-COVID com sintoma de fadiga persistente, e controles pareados.
- Polissonografia tipo I, não poligrafia: o esforço torácico-abdominal é o que separa os ramos.
- Avaliação autonômica formal (tilt test, teste de resposta pressórica, VFC por ECG —
  não por oxímetro).
- Registro de medicação, obrigatoriamente.
- Desfecho primário: comprimento do ciclo de respiração periódica e sua correlação com
  frequência cardíaca média e com marcador de perfusão.

---

*Documento de hipótese. Nenhuma das relações aqui descritas está demonstrada no contexto
pós-COVID. Registrado para fins de prioridade e discussão acadêmica.*

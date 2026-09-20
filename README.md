# Oxytracking

**© 2026 Rachel Ingrid Pereira da Rocha Jannuzzi — Todos os direitos reservados.**
ORCID [0000-0002-0408-6302](https://orcid.org/0000-0002-0408-6302)

Rastreio oximétrico noturno. Análise de tendência e espectral de SpO₂ e frequência de pulso
a partir de registros de oxímetro domiciliar, inteiramente no navegador.

**Esta ferramenta não emite diagnóstico.** Ela organiza dados e aplica limiares declarados.
Não estabelece, confirma ou exclui apneia do sono, disautonomia, arritmia ou qualquer
outra condição clínica.

---

## Uso

Abra `index.html`. Nada mais é necessário: sem instalação, sem build, sem servidor, sem internet.

Para publicar no GitHub Pages:

1. Envie os arquivos para a raiz do repositório.
2. **Settings → Pages** → *Deploy from a branch* → branch `main`, pasta `/ (root)`.
3. Disponível em `https://<usuario>.github.io/oxytracking/`.

Há três noites sintéticas em [`exemplos/`](exemplos/) para testar a ferramenta sem dados reais.
São dados **fabricados**, gerados por simulação, não provenientes de qualquer pessoa.

---

## Arquitetura

Arquivo único, JavaScript puro, sem nenhuma dependência externa.
FFT radix-2, densidade espectral de potência pelo método de Welch, coerência espectral
e todos os gráficos são implementados dentro do próprio `index.html`.
Não há requisição de rede: nenhum CDN, nenhuma fonte remota, nenhuma biblioteca.

```
oxytracking/
├── index.html                    aplicação completa
├── README.md
├── LICENSE
├── CITATION.cff
├── docs/
│   ├── hipotese-autonomica.md    fundamentação do eixo investigativo
│   ├── modelo_causal.svg         diagrama de hipótese (vetorial)
│   └── modelo_causal.png
└── exemplos/                     três noites sintéticas para demonstração
```

---

## Formato de entrada

CSV do oxímetro, amostragem de 1 Hz:

```
Date,Time,SpO2(%),PR(bpm)
9/20/2026,1:07:28 AM,98,67
```

O leitor tolera variações: cabeçalho ausente, data em `AAAA-MM-DD`, hora em 24 h,
nomes alternativos de coluna. Amostras fora da faixa fisiológica
(SpO₂ < 50 ou > 100; FP < 20 ou > 250) são descartadas como artefato.

Arquivos iniciados a menos de 14 h do fim do anterior são agrupados como a **mesma noite**.
Registros interrompidos são unidos e a lacuna é contabilizada e exibida.

---

## Índices calculados

**Saturação.** ODI 3% e ODI 4% por método pico-vale — queda a partir do máximo dos 10 s
anteriores, vale em até 120 s, com resaturação de pelo menos metade da queda.
T88, T90, T93, nadir, duração e profundidade medianas dos eventos, e a fração de eventos
com nadir ≥ 94% (que separa oscilação rasa de dessaturação clinicamente relevante).

**Frequência de pulso.** Média, mínima, máxima, amplitude p5–p95, percentual do tempo
abaixo de 70 bpm, dispersão das médias de 5 min, e degrau entre blocos da mesma noite.

**Espectro.** PSD de Welch (janela de Hann, 2048 s, 75% de sobreposição) de SpO₂ e de FP.
Período dominante entre 4 e 60 mHz. Potência relativa em VLF (3–10 mHz),
respiração periódica (10–40 mHz) e LF (40–150 mHz). Coerência espectral SpO₂ × FP
com limiar de significância de Carter, corrigido para graus de liberdade efetivos —
segmentos sobrepostos não são independentes, e ignorar isso rebaixa falsamente o limiar.

---

## O que a ferramenta não calcula, e por quê

**IAH.** Oximetria não mede fluxo aéreo, esforço torácico-abdominal nem EEG.
Não distingue evento obstrutivo de central e não detecta hipopneia com despertar sem
dessaturação. O denominador dos índices é hora de *registro*, não hora de *sono*:
se houve vigília, as taxas reais por hora de sono são maiores.

**Variabilidade da frequência cardíaca.** O oxímetro entrega frequência de pulso já
suavizada pelo firmware, não intervalos batimento-a-batimento. Em registros típicos
cerca de 80% das amostras consecutivas são idênticas e a autocorrelação em lag de 1 s
ultrapassa 0,99. RMSSD, pNN50, SDNN e potência de alta frequência calculados sobre esse
sinal seriam artefato do filtro do aparelho, não fisiologia. Apenas índices de escala
lenta são reportados. A ferramenta mede a fração de amostras repetidas e avisa quando
o sinal está suavizado demais.

---

## Critérios de disparo

Calibrados para sensibilidade: o custo de um disparo é uma consulta, o custo da perda é maior.

| Critério | Disparo |
|---|---|
| FP noturna mediana | < 55 ou > 85 bpm |
| Descenso noturno | < 10% da FC de repouso diurna |
| Amplitude p5–p95 | < 8 bpm |
| Degrau intranoite | > 10 bpm entre blocos |
| ODI 4% mediano | ≥ 15 /h |
| T90 mediano | > 300 s |

**Estes limiares são heurísticos.** Foram derivados de fisiologia e de faixas de referência
usuais, **não de validação contra padrão-ouro**. Nenhum deles tem sensibilidade e
especificidade publicadas para este uso. A ferramenta é defensável como organizador de
dados com limiares explícitos — não como instrumento validado.

O descenso noturno só é avaliável com FC de repouso diurna informada. Sem esse valor,
a FP noturna isolada é ininterpretável: 66 bpm pode ser descenso normal a partir de 82,
ou ausência de descenso a partir de 70.

**Medicação é campo obrigatório.** Betabloqueador, digoxina, amiodarona, ivabradina e
alguns antidepressivos produzem exatamente o padrão que dispara os critérios de FP.
Sem essa checagem, a taxa de falso-positivo inviabiliza o rastreio.

---

## Suficiência amostral

- **≥ 4 h válidas por noite** — abaixo disso a banda VLF não resolve e o descenso não é estimável.
- **3 a 5 noites** — uma noite isolada tem variabilidade intraindividual grande demais
  (sono, hidratação, álcool, temperatura ambiente, estágio de sono, posição corporal).
- **FC de repouso diurna pareada**, ainda que de 2 h.

A ferramenta sinaliza cada item não atendido, e o relatório registra a insuficiência
explicitamente em vez de omiti-la.

---

## Confundidores

- **Medicação bradicardizante** — ver acima.
- **Baixa perfusão periférica** — produz oscilação lenta e rasa de SpO₂ indistinguível
  de instabilidade ventilatória real. É o confundidor mais traiçoeiro, porque coexiste
  justamente com a bradicardia que dispara o rastreio.
- **Perda de contato do sensor** — gera lacunas que reduzem a validade dos índices.
- **Posição corporal e estágio de sono** — não registrados pelo oxímetro, e explicam
  boa parte da variação da densidade de eventos ao longo da noite.

---

## Privacidade

Todo o processamento ocorre no navegador. Nenhum dado trafega em rede.
Os relatórios ficam em `localStorage`, apenas neste navegador e neste dispositivo,
e são apagados se o usuário limpar os dados do site. Para guardar fora do navegador,
exporte em JSON ou imprima em PDF.

---

## Contexto de pesquisa

O eixo investigativo que motivou a ferramenta — instabilidade do controle ventilatório
e comportamento autonômico noturno após COVID-19 — está documentado em
[`docs/hipotese-autonomica.md`](docs/hipotese-autonomica.md), separadamente,
para que a hipótese em investigação não se confunda com o que a ferramenta afirma medir.

---

## Situação regulatória

Software destinado a triagem clínica pode ser enquadrado como dispositivo médico pela
ANVISA (RDC 657/2022 e correlatas). Este repositório é disponibilizado como ferramenta
de análise de dados e material de pesquisa, **sem indicação de uso diagnóstico**.
Uso assistencial exigiria validação prospectiva contra polissonografia e o enquadramento
regulatório correspondente.

---

## Autoria e direitos

**© 2026 Rachel Ingrid Pereira da Rocha Jannuzzi. Todos os direitos reservados.**

Obra protegida pela Lei nº 9.610/1998 (Direitos Autorais) e pela Lei nº 9.609/1998
(Programa de Computador).

**Não é concedida licença de uso, cópia ou reprodução.** São vedados, sem autorização
prévia, expressa e por escrito da autora: reprodução total ou parcial do código, da
documentação, dos textos ou dos diagramas; distribuição ou hospedagem por terceiros;
modificação ou criação de obra derivada; incorporação a outro software ou serviço;
uso comercial, assistencial, acadêmico ou de pesquisa; e extração dos métodos de
cálculo para emprego em outra obra.

A visualização desta página, quando publicada na internet, **não constitui licença de uso**.

Citação acadêmica com atribuição de autoria é permitida nos termos do art. 46 da
Lei nº 9.610/1998 — dados em [`CITATION.cff`](CITATION.cff).

Pedidos de autorização devem ser dirigidos à autora.

Termos completos em [`LICENSE`](LICENSE).

---

Documento de rastreio. Não constitui laudo, diagnóstico ou prescrição.
A interpretação clínica cabe ao profissional assistente.

# Calculadora de CMV e Precificação por Quilo

Ferramenta para restaurantes self-service e por quilo: calcula o CMV por quilo
servido e sugere o preço de venda que sustenta a margem desejada.

Desenvolvida para a **Ajinomoto Food Service**.

---

## Testar agora

**→ [Abrir a calculadora](https://SEU-USUARIO.github.io/calculadora-cmv-ajinomoto/)**

> Substitua `SEU-USUARIO` pelo usuário ou organização do GitHub depois de
> publicar. As instruções de publicação estão mais abaixo.

Esta é a mesma calculadora que vai para o site, sem alteração nenhuma. A única
diferença é que aqui ela carrega a fonte Quicksand do Google Fonts, porque não
existe o tema do site para fornecê-la.

### Sugestão de cenário para testar

| Campo | Valor |
|---|---|
| Estoque inicial | 1.000,00 |
| Compras no período | 5.000,00 |
| Estoque final | 800,00 |
| Quilos vendidos | 400 |
| Despesas fixas mensais | 15.000,00 |
| Volume de vendas por mês | 1.000 |

Mantendo os padrões (CMV 32%, variáveis 12%, meta de lucro 15%), o resultado
esperado é:

- CMV por quilo: **R$ 13,00**
- Preço de venda sugerido: **R$ 40,63 / kg**
- Lucro por quilo: **R$ 7,75**
- Margem de lucro real: **19,08%** (meta atingida)
- Lucro mensal estimado: **R$ 7.750,00**
- Ponto de equilíbrio: **660 kg**

---

## O que tem neste repositório

| Caminho | O que é |
|---|---|
| `index.html` | Calculadora completa, versão de demonstração. É o que o GitHub Pages publica. |
| `wordpress/calculadora-embed.html` | **É este que vai para o site.** Fragmento para colar num bloco HTML personalizado do WordPress. |
| `docs/INSTALACAO.md` | Passo a passo da instalação, ajuste do cabeçalho fixo e o que fazer se houver CSP. |
| `docs/PAGINA-SEO.md` | Briefing da página que envolve a calculadora: título, textos, FAQ e schema. |

---

## Como publicar para o cliente testar

1. Crie um repositório novo no GitHub (pode ser privado; o GitHub Pages funciona
   em repositório privado em contas Pro, Team e Enterprise).
2. Na pasta deste projeto, rode:

```bash
git init
git add .
git commit -m "Calculadora de CMV e precificação por quilo"
git branch -M main
git remote add origin https://github.com/SEU-USUARIO/calculadora-cmv-ajinomoto.git
git push -u origin main
```

3. No repositório, vá em **Settings → Pages**.
4. Em **Source**, escolha **Deploy from a branch**; em **Branch**, escolha
   `main` e a pasta `/ (root)`. Salve.
5. Em um ou dois minutos o endereço fica no ar. Atualize o link no topo deste
   README e mande para o cliente.

---

## Como instalar no site

O arquivo que vai para o WordPress é o `wordpress/calculadora-embed.html`, não o
`index.html`. Ele não traz as tags de documento nem o link do Google Fonts,
porque o tema do site já carrega a Quicksand.

O passo a passo completo está em [`docs/INSTALACAO.md`](docs/INSTALACAO.md).
Dois pontos merecem atenção na instalação:

- **Cabeçalho fixo:** ajustar a variável `--ajfs-scroll-offset` para a altura
  real do cabeçalho vermelho, senão o topo do resultado fica escondido atrás
  dele ao rolar.
- **Um bloco por página:** os IDs dos campos são únicos e colidem se houver
  duas calculadoras na mesma página.

---

## Como as contas são feitas

| Resultado | Fórmula |
|---|---|
| CMV por quilo | (estoque inicial + compras − estoque final) ÷ quilos vendidos |
| Preço de venda | custo por quilo ÷ margem de CMV (método do markup divisor) |
| Rateio das despesas fixas | despesas fixas mensais ÷ volume mensal em quilos |
| Despesas variáveis em R$ | preço × percentual de despesas variáveis |
| Lucro por quilo | preço − custo − rateio das fixas − despesas variáveis |
| Margem de lucro real | lucro por quilo ÷ preço |
| Lucro mensal estimado | lucro por quilo × volume mensal |
| Ponto de equilíbrio | despesas fixas ÷ (preço − custo − despesas variáveis) |

O desperdício do buffet não tem campo próprio porque já está embutido: a conta
divide pelos quilos **vendidos**, não pelos produzidos, então a sobra descartada
encarece o custo por quilo automaticamente.

---

## O que já foi verificado

- **Cálculo:** conferido contra um DRE mensal montado à parte, em 5 cenários
  (incluindo prejuízo e operação sem despesas variáveis), batendo ao centavo.
  No volume do ponto de equilíbrio, o lucro dá zero.
- **Isolamento:** instalada dentro de uma página com CSS hostil de propósito.
  Os 178 elementos mantiveram tipografia e cores da marca, e os estilos da
  calculadora não afetaram nada fora dela.
- **Segurança:** 6 tipos de código malicioso injetados nos 11 campos, por dois
  caminhos diferentes. Nenhum executou.
- **Responsivo:** 375px, 703px, 876px e 1440px, sem quebra ou corte.
- **Teclado e leitor de tela:** Enter calcula a partir de qualquer campo; erros
  marcados com `aria-invalid`; textos de apoio ligados por `aria-describedby`.

---

## Privacidade

Todo o cálculo acontece no navegador de quem usa. A calculadora não faz nenhuma
requisição de rede, não grava cookie nem armazenamento local, e não envia dado
algum para servidor nenhum. O botão "Copiar resumo" apenas coloca um texto na
área de transferência da própria pessoa.

---

## Limite conhecido do modelo

O lucro mensal e o ponto de equilíbrio assumem que o volume de vendas não muda
quando o preço muda. É a simplificação padrão desse tipo de ferramenta, mas ao
simular um aumento grande de preço a projeção fica otimista, porque na prática a
procura tende a cair. Há um aviso sobre isso no rodapé da calculadora.

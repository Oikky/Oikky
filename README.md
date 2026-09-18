

# Ikky David

**Dev full-stack. Construo sistema que entra em produção e movimenta dinheiro de verdade.**

Montes Claros/MG · Técnico em Edificações (IFNMG) · Arquitetura e Urbanismo (UNIFIP-MOC)

Não trabalho dentro de time grande: eu levanto o requisito com quem vai usar, escrevo, subo e fico de plantão quando quebra. Por isso meu critério de "pronto" não é *passou no teste* — é **o cliente pagou, a cozinha produziu e o pedido saiu na hora certa**.

---

## 🎂 D'Luh Festas — pedido, pagamento e atendimento por IA

Sistema de pedidos de um buffet de festas, **no ar e operando de verdade**. Sou sócio do negócio e dono do código, o que significa que quando o sistema erra o prejuízo é meu.

O backend é **um único Cloudflare Worker de 8.496 linhas** costurando Coda (banco), InfinitePay (cobrança), Telegram (aprovação da cozinha), Google Calendar e quatro APIs de CEP em cascata. Sem framework, sem bundler, sem etapa de build — HTML/CSS/JS na mão e `git push`. **181 commits**, 13.088 linhas de front versionadas mais as 8.496 do Worker, que fica fora do repositório porque carrega credencial.

O que ele resolve:

- **Ciclo de vida de pedido em 6 status**, do orçamento à entrega, com regra de quem pode editar o quê em cada um
- **Cobrança em duas pernas** — entrada no fechamento, restante na entrega — com webhook de pagamento que avança o status sozinho
- **Painel de cozinha em modo TV**: destaque "Fazer agora" rotativo, alerta sonoro, recibo em impressora térmica
- **Cancelamento pelo cliente com taxa escalonada** — 0% dentro da carência de 2 dias, subindo linear até o teto de 80% na véspera, porque a essa altura o ingrediente já foi comprado
- **Bot de triagem** no site e painel "Meus Pedidos" com polling e sessão que sobrevive a F5

**O bug de que mais me orgulho** é de comparação de telefone. O WhatsApp entrega o número ora com o nono dígito, ora sem — comparar exato errava **em silêncio**. Comparar só os 8 últimos dígitos consertava isso e abria um buraco pior: `38 98888-7777` virava igual a `11 98888-7777`, e um cliente conseguia ver e cancelar o pedido de outro. A correção exige o DDD quando os dois lados têm, cai nos 8 dígitos só quando o dado legado está sem ele, e **loga toda vez que usa o fallback**, pra dar pra medir o legado em vez de adivinhar.

### A IA de atendimento no WhatsApp — em produção desde 2026-08-03

Serviço Node **sem nenhuma dependência externa**, rodando em máquina local e exposto por Tailscale Funnel, com **LLM local via Ollama e tool calling**.

A decisão que define o projeto: **as regras que valem dinheiro não moram no prompt.** Preço, produto e data passam por guardrails escritos em código — se o modelo inventar um valor, o guardrail derruba antes de virar pedido. Nenhum pedido é gravado sem confirmação explícita do cliente, e todo pedido criado pela IA fica marcado como tal.

`Node.js` · `Ollama` · `tool calling` · `Cloudflare Workers KV` · `Evolution API` · `Tailscale`

---

## 🥽 WebXR — realidade virtual que roda em headset móvel

### [Levanta a Obra](https://sitedluh.github.io/fenics/) — jogo de 60 segundos

Feito para o stand do FENICS/IFNMG: 24 peças, 7 etapas, levantar uma casa contra o relógio. A-Frame, jogador estacionário, no ar em GitHub Pages.

Quest 2 é hardware móvel, então performance aqui é requisito, não polimento:

- **Qualidade adaptativa** — a cena mede o FPS e se rebaixa sozinha (textura 512, sem normal map, sem sombra) quando o headset não aguenta
- **Texturas PBR com escala de UV em metros** — um tijolo tem tamanho de tijolo em qualquer peça, de qualquer dimensão
- **Zero CDN.** A cena abre sem internet. Num stand de evento isso não é preferência, é seguro contra o Wi-Fi do evento
- Geometria dos GLB fundida por material, `foveationLevel: 1`, `highRefreshRate`
- `node --test` cobrindo a lógica de jogo — **10 testes passando**

### Galeria VR do Técnico em Edificações

Museu virtual que expõe as pranchas de um projeto real de saneamento (CODEVASF/ARH, Jequitaí-MG) — nave de 14 m, abside semicircular, prancha clicável que abre em alta resolução.

Locomoção por analógico girando **em torno da cabeça, não da origem do rig** — sem isso o jogador é chicoteado toda vez que está deslocado do centro. É o tipo de detalhe que não aparece em screenshot e decide se a pessoa tira o headset enjoada.

---

## Como eu trabalho

**Número sem fonte não entra.** Se é estimativa, vai marcado como estimativa. Se eu medi, mostro quanto e como medi.

**Orquestro agentes de IA pra desenvolver.** O repositório da D'Luh tem 9 subagentes versionados, cada um dono de um domínio — worker/backend, carrinho, checkout, bot, autenticação. Contexto isolado por domínio funciona melhor que uma sessão gigante quando o arquivo tem 190 KB. É ferramenta, não substituto: a arquitetura e a decisão de negócio continuam minhas.

**Documento o que custou caro.** Todo projeto meu tem uma seção de becos sem saída já testados. Errar uma vez é aprendizado; errar duas é não ter anotado.

---

## Stack

**Front-end** JavaScript (vanilla), HTML, CSS, A-Frame/WebXR, Firebase Auth
**Back-end** Cloudflare Workers + KV, Node.js, Python
**IA aplicada** Ollama, LLM local, tool calling, guardrails em código
**Integrações** Coda API, Telegram Bot, InfinitePay, Google Calendar, Evolution API
**Infra** Git, GitHub Pages, Tailscale

---

📍 Montes Claros/MG · 💼 [LinkedIn](www.linkedin.com/in/ikky-david-augusto-teixeira-de-sousa-85253029b) · 📫 ikkysousa5@gmail.com

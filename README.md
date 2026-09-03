# IA Panorâmica (CCM-3269) — Proposta C: nativa da jornada

Terceira proposta de UX para o mesmo card. As outras duas continuam salvas:

| | Onde | Porta |
|---|---|---|
| A — módulo com abas | `../Gerenciador de whatsapp flow/` | 8899 |
| B — copiloto em painel | `../IA Panoramica/` | 8900 |
| **C — nativa da jornada** | **este projeto** | **8901** |

## Como rodar

```
python3 -m http.server 8901
```

## Telas

| Caminho | O que é |
|---|---|
| `index.html` | Tela de acesso — escolhe entre as duas versões |
| `jornada.html?modo=simplificada` | Só as análises padrão do sistema |
| `jornada.html?modo=completa` | Análises padrão + análises criadas pelo usuário |

As duas versões vivem no **mesmo arquivo** (`jornada.html`). O modo vem da query
string e controla apenas uma coisa: se a seção "Análises personalizadas" existe
na configuração da jornada e se os blocos dela aparecem no relatório. Todo o
resto — configuração, rail de análises, relatório, detalhamentos — é o mesmo
código, então as versões não divergem.

Uma pílula no canto inferior esquerdo mostra qual versão está aberta e leva de
volta à tela de acesso. É andaime de validação, não faz parte do produto.

## Deploy na Vercel

A pasta é estática, sem build. Basta apontar a Vercel para ela.

O `vercel.json` liga `cleanUrls` e cria dois atalhos, para o link ficar curto na
hora de compartilhar:

- `/simplificada`
- `/completa`

Os caminhos com query string continuam funcionando nos dois ambientes, e são os
que a tela de acesso usa — assim o protótipo roda igual local e publicado.

## O conceito

A IA não é um módulo nem um painel — é uma **capacidade da jornada**, ligada ou
desligada no momento em que a jornada é criada.

**1. Configuração, na aba "Dados da jornada"**

Um cartão novo, no mesmo padrão dos existentes (Configurações, Tipo de jornada,
Canal, Fluxos de jornada), com um toggle "Usar a IA Panorâmica nesta jornada".
Ligado, revela:

- **Análise recorrente** — toggle + periodicidade
- **Coleta de informações em variáveis** — os prompts fixos de extração, que
  são configuração da jornada e não de um módulo separado

**2. Consumo, em uma aba própria da jornada**

Ligar o toggle adiciona a aba **IA Panorâmica** ao lado de Dados da jornada,
Monitoramento e Prévia de testes. Desligar remove a aba.

A aba tem o período, o botão de gerar, o histórico de análises **desta jornada**
e o relatório aberto abaixo.

**3. Atalho na listagem**

Jornadas com a IA habilitada mostram o selo "IA Panorâmica" e um ícone ✦ que
abre direto na aba de análise, com um ponto verde quando há análise nova.

## O que essa estrutura resolve sozinha

**Não existe seletor de jornada em lugar nenhum.** A jornada é o contexto, não
um filtro — aparece travada, com cadeado, na barra de escopo. O usuário escolhe
apenas o período.

Isso elimina por construção a regra mais frágil das outras propostas: o prompt
não *precisa* ser restringido ao recorte, ele já nasce restrito. Pergunte sobre
outra jornada e a resposta aponta para a aba IA Panorâmica *dela*.

**As sugestões de melhoria apontam para o fluxo que está na aba ao lado** —
clicar no nó indicado leva para "Dados da jornada". Insight e edição no mesmo
lugar.

## Arquivos

- `index.html` — protótipo completo (estilos + markup + JS)
- `app.css` — tokens do DS
- `ESPEC_CCM-3269_ia_panoramica.md` — spec de origem

## Pontos `[a definir]` prototipados com valor provisório

Os mesmos das outras: limite mínimo de volume (120 conversas), periodicidades,
desfechos dos argumentos (acordo fechado / retorno agendado / recusa) e formato
de exportação.

Decisão nova desta proposta, que precisa ser validada: **o que acontece com as
análises já geradas quando o usuário desliga o toggle.** Aqui a aba some e as
análises ficam indisponíveis, sem serem apagadas — mas isso é uma escolha do
protótipo, não do card.

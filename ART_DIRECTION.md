# Ayé v7 — Direção Artística

> "O tabuleiro é um terreiro à noite. Cada captura é um golpe de tambor; cada batalha, uma fogueira que sobe."

## Conceito

A v7 transforma o protótipo funcional (v6) em uma experiência audiovisual que **estimula o combate**. Toda a direção de arte foi pensada para que ações agressivas — capturas, batalhas, habilidades — sejam os momentos de maior impacto sensorial da partida, recompensando o jogador que ataca.

## Pilares visuais

1. **Noite de terreiro** — paleta de âmbar, terracota e ouro-brasa sobre negro profundo. O fundo é um shader WebGL de fumaça espiritual em movimento lento, com brasas subindo em partículas.
2. **Combate esquenta a tela** — o shader de fundo possui uniforms de `boost` e `tint`: quando uma batalha acontece, a fumaça inteira flui mais forte e vira vermelho-sangue por ~1,5s, com screen shake, flash de vinheta e tambores (WebAudio procedural).
3. **Sementes são matéria viva** — toda movimentação de sementes (semeadura, captura, roubo, dádiva, Altar) é animada com partículas em arco voando entre as Kalas reais do DOM, com trilha e brilho.
4. **Cartas como artefatos** — cada uma das 14 cartas ganhou ilustração SVG procedural própria, moldura por tipo (Encantamento = verde-mata, Combate = sangue-brasa, Astúcia = púrpura-fumaça), gema de custo hexagonal e foil sheen no hover.
5. **Orixás como sigilos** — cada Linha tem um emblema SVG geométrico (opaxorô, machados cruzados, oxê, espiral de vento, abebé, ibiri, encruzilhada) usado na seleção, no HUD e no splash da Habilidade Única.

## Sistemas implementados

| Sistema | Tech | Uso |
|---|---|---|
| Shader de fundo | WebGL (fbm noise) | Fumaça espiritual; esquenta no combate |
| Partículas | Canvas 2D | Sementes em voo, bursts de captura, anéis, fogos de vitória, brasas ambiente |
| Screen shake + flash | JS + CSS | Batalhas, capturas, trancas |
| SFX | WebAudio procedural (zero assets) | Tambores de batalha, captura, semente, turno, vitória |
| Arte de cartas | SVG inline procedural | 14 ilustrações + molduras TCG |
| Emblemas de Orixá | SVG inline | Seleção, HUD, splash de habilidade |
| Animações de UI | CSS keyframes | Dados que rolam, revelação do vencedor, splash "SUA VEZ", deal de cartas, alvos pulsantes |

## Coreografia do combate (modal de batalha)

1. Tela treme (16px, 650ms) + flash vermelho + shader em fúria + tambor duplo.
2. Modal entra com borda de chama rotativa (conic-gradient blur).
3. Dados tombam na mesa (`diceTumble`, stagger entre atacante/defensor).
4. Totais aparecem com `popIn` (1s), "VS" pulsa em vermelho.
5. Vencedor é revelado com `winnerIn` (1,45s) — letras douradas que se comprimem.
6. Efeito da carta sobe por último (1,8s).

## Sincronização multiplayer

Os efeitos não são apenas locais: cada ação grava uma fila `fx` (com id único) no estado do Firebase. Ambos os clientes processam a fila uma única vez (`seenFx`), então **os dois jogadores veem as mesmas explosões, voos de semente e splashes**, cada um a partir da própria perspectiva do tabuleiro.

## Compatibilidade

- Lógica de jogo 100% idêntica à v6 (mesmas regras, mesmo schema Firebase + campo `fx` opcional).
- Sem WebGL → fallback para gradiente estático.
- Som desligável no header (🔇).
- Mobile: grid de mão 2 colunas, splashes redimensionados, DPR limitado para performance.

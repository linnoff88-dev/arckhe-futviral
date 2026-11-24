# futviral

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![futviral â€” radar de futebol](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/

**ðŸ‡§ðŸ‡· [PortuguÃªs](README.md) Â· ðŸ‡ºðŸ‡¸ [English](README.en.md) Â· ðŸ‡ªðŸ‡¸ [EspaÃ±ol](README.es.md)**

## O que Ã©

O futviral faz, sozinho, vÃ­deos curtos (9:16) com o resumo do que os 10 maiores canais de futebol do YouTube disseram: BrasileirÃ£o, futebol europeu, mercado da bola, polÃªmicas e negÃ³cios do futebol. Ele assiste aos vÃ­deos por transcriÃ§Ã£o, separa os nÃºmeros e os fatos com a fonte de cada um e monta um vÃ­deo com nÃºmeros grandes, carimbos (RUMOR, NEGOCIANDO, FECHADO, POLÃŠMICA) e o Nei comentando (avatar). Rumor Ã© tratado como rumor, nenhum clube tem lado e aposta nÃ£o entra. Roda num computador com GPU, com a assinatura do Claude Code e do HeyGen. Ã‰ irmÃ£o do [advviral](https://github.com/linnoff88-dev/advviral), do [saudeviral](https://github.com/linnoff88-dev/saudeviral) e do [finviral](https://github.com/linnoff88-dev/finviral) (mesmo motor, outro tema).

## ðŸ“– Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

## Os 10 canais

Escolhidos em 26/10/2026 por mediÃ§Ã£o prÃ³pria (yt-dlp, mediana de views dos Ãºltimos 15 vÃ­deos, canal ativo e falado). Detalhes em [`pesquisa/canais.md`](pesquisa/canais.md); lista viva em `canais.json`.

EurÃ£o Â· Jovem Pan Esportes Â· JozÃ£o Â· Fabrizio Romano Â· Chico Garcia Â· Tode Â· Fred Caldeira Â· TiaGOL Â· Canal do Nicola Â· Mauro Cezar.

## O processo, do inÃ­cio ao fim

| # | Passo | Comando | Quem faz |
|---|---|---|---|
| 1 | Lista os vÃ­deos dos canais do perÃ­odo (sÃ³ metadados) | `coleta/listar.py` (chamado pelo `auto/rodar.sh`) | âš™ï¸ script |
| 2 | Escolhe 1 vÃ­deo por canal (o mais visto), sem repetir vÃ­deo jÃ¡ usado | `auto/rodar.sh` + `auto/usados.py` | âš™ï¸ script |
| 3 | Pega a legenda do YouTube (segundos); se nÃ£o houver, baixa o Ã¡udio e transcreve local (ARCKHÉvox, Whisper large-v3; inglÃªs para o Fabrizio) | `coleta/transcrever.sh` + `coleta/legenda_yt.py` | âš™ï¸ script |
| 4 | Escreve `base.md` (fato + trecho literal), `roteiro.json` e `legenda.txt` | `claude -p` com `auto/roteiro.md` | ðŸ¤– `claude -p` |
| 5 | Confere trechos, nÃºmeros por extenso, 5+ assuntos, aviso de rumor, sem aposta; 1 correÃ§Ã£o | `auto/checar.py` | âš™ï¸ script |
| 6 | Avatar do Nei no estÃºdio HeyGen (assinatura); se falhar, foto + voz do Nei | `motor/avatar_heygen.sh` / `motor/narrar.py` | âš™ï¸ script |
| 7 | Renderiza o vÃ­deo (palco.html quadro a quadro) e compÃµe o avatar | `motor/gerar_cena.py` + `motor/compor_avatar.sh` | âš™ï¸ script |
| 8 | GeraÃ§Ã£o Ã s 9h: o vÃ­deo vai ao bot como prÃ©via. A partir das 14h o Radar Viral publica no YouTube **Radar Viral (lives24)** e manda o link ao bot (`touch <ediÃ§Ã£o>/SEGURAR` segura) | `auto/publicar.sh` + `auto/publicar-pendente.sh` (yt-pubx) | âš™ï¸ timer 09:00 / 12:00 |
| 9 | SÃ¡b/dom: longo 16:9 "A semana explicada" | `auto/roteiro-longo.md` â†’ `auto/longo.sh` | ðŸ¤– + âš™ï¸ |
| 10 | Aprovar o formato, incluir/tirar canais, ligar a agenda | `canais.json`, `systemd/` | ðŸ§  sessÃ£o com o Nei |

Desde 09/11/2026 os passos pesados (Whisper, retempo, voz local, render e encode) passam pela vaga da mÃ¡quina (`motor/vaga.sh` â†’ `~/projetos/wifi/scripts/fila-vaga`; saÃ­da 75 = mÃ¡quina ocupada, avisa e nÃ£o usa a pauta) e cada rodada grava inÃ­cio e fim no placar (`~/projetos/wifi/scripts/rodada hoje`).

Rodada Ã  mÃ£o:

```bash
auto/rodar.sh diario 2026-10-26                 # rodada completa (avatar + lives24 + bot)
SEM_AVATAR=1 auto/rodar.sh diario 2026-10-26    # versÃ£o com foto, sÃ³ para o bot
REUSAR_ROTEIRO=1 auto/rodar.sh diario 2026-10-26  # refaz avatar/vÃ­deo/publicaÃ§Ã£o com o roteiro jÃ¡ aprovado
```

Agenda (`systemd/futviral-*.timer`, ligada em 26/10/2026): geraÃ§Ã£o Ã s 09:00 (diÃ¡rio segâ€“sex, semanal no sÃ¡bado, domingo no domingo) e publicaÃ§Ã£o pelo Radar Viral a partir das 14:00.

## Regras de cada vÃ­deo

- Todo fato tem a fonte (canal, data, tÃ­tulo) na tela e um trecho literal da transcriÃ§Ã£o no `base.md`.
- Rumor sÃ³ vale quando o clube confirma: transferÃªncia Ã© "rumor" ou "negociando" atÃ© o clube confirmar.
- Sem torcer: tudo Ã© atribuÃ­do a quem disse.
- Sem aposta: nada de bet, odd ou palpite. O portÃ£o reprova.
- Sem lance, gol, imagem de jogo ou escudo de clube (direito de transmissÃ£o).
- Fala no tom do Nei, tratando por "vocÃª", com nÃºmeros por extenso. Fim: pergunta para comentar e CTA do ARCKHÉ AI.
- HeyGen sÃ³ pelo estÃºdio da assinatura. A oferta de troca de plano Ã© sempre recusada.

## LicenÃ§a

MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>
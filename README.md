# futviral

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="48" />
  <p><strong>Arckhé AI</strong></p>
</div>


[![futviral ê¢â¬â radar de futebol](guia/assets/banner.jpg)](https://linnoff88-dev.github.io/

**ê°Å¸â¡Â§ê°Å¸â¡Â· [PortuguêÂªs](README.md) êÂ· ê°Å¸â¡Âºê°Å¸â¡Â¸ [English](README.en.md) êÂ· ê°Å¸â¡Âªê°Å¸â¡Â¸ [EspaêÂ±ol](README.es.md)**

## O que êÂ©

O futviral faz, sozinho, vêÂ­deos curtos (9:16) com o resumo do que os 10 maiores canais de futebol do YouTube disseram: BrasileirêÂ£o, futebol europeu, mercado da bola, polêÂªmicas e negêÂ³cios do futebol. Ele assiste aos vêÂ­deos por transcriêÂ§êÂ£o, separa os nêÂºmeros e os fatos com a fonte de cada um e monta um vêÂ­deo com nêÂºmeros grandes, carimbos (RUMOR, NEGOCIANDO, FECHADO, POLêÅ MICA) e o Nei comentando (avatar). Rumor êÂ© tratado como rumor, nenhum clube tem lado e aposta nêÂ£o entra. Roda num computador com GPU, com a assinatura do Claude Code e do HeyGen. êâ° irmêÂ£o do [advviral](https://github.com/linnoff88-dev/advviral), do [saudeviral](https://github.com/linnoff88-dev/saudeviral) e do [finviral](https://github.com/linnoff88-dev/finviral) (mesmo motor, outro tema).

## ê°Å¸ââ Guia de uso

Guia completo (landing + passo a passo): **https://linnoff88-dev.github.io/

## Os 10 canais

Escolhidos em 26/10/2026 por mediêÂ§êÂ£o prêÂ³pria (yt-dlp, mediana de views dos êÂºltimos 15 vêÂ­deos, canal ativo e falado). Detalhes em [`pesquisa/canais.md`](pesquisa/canais.md); lista viva em `canais.json`.

EurêÂ£o êÂ· Jovem Pan Esportes êÂ· JozêÂ£o êÂ· Fabrizio Romano êÂ· Chico Garcia êÂ· Tode êÂ· Fred Caldeira êÂ· TiaGOL êÂ· Canal do Nicola êÂ· Mauro Cezar.

## O processo, do inêÂ­cio ao fim

| # | Passo | Comando | Quem faz |
|---|---|---|---|
| 1 | Lista os vêÂ­deos dos canais do perêÂ­odo (sêÂ³ metadados) | `coleta/listar.py` (chamado pelo `auto/rodar.sh`) | ê¢Å¡â¢ê¯Â¸Â script |
| 2 | Escolhe 1 vêÂ­deo por canal (o mais visto), sem repetir vêÂ­deo jêÂ¡ usado | `auto/rodar.sh` + `auto/usados.py` | ê¢Å¡â¢ê¯Â¸Â script |
| 3 | Pega a legenda do YouTube (segundos); se nêÂ£o houver, baixa o êÂ¡udio e transcreve local (ARCKHêvox, Whisper large-v3; inglêÂªs para o Fabrizio) | `coleta/transcrever.sh` + `coleta/legenda_yt.py` | ê¢Å¡â¢ê¯Â¸Â script |
| 4 | Escreve `base.md` (fato + trecho literal), `roteiro.json` e `legenda.txt` | `claude -p` com `auto/roteiro.md` | ê°Å¸Â¤â `claude -p` |
| 5 | Confere trechos, nêÂºmeros por extenso, 5+ assuntos, aviso de rumor, sem aposta; 1 correêÂ§êÂ£o | `auto/checar.py` | ê¢Å¡â¢ê¯Â¸Â script |
| 6 | Avatar do Nei no estêÂºdio HeyGen (assinatura); se falhar, foto + voz do Nei | `motor/avatar_heygen.sh` / `motor/narrar.py` | ê¢Å¡â¢ê¯Â¸Â script |
| 7 | Renderiza o vêÂ­deo (palco.html quadro a quadro) e compêÂµe o avatar | `motor/gerar_cena.py` + `motor/compor_avatar.sh` | ê¢Å¡â¢ê¯Â¸Â script |
| 8 | GeraêÂ§êÂ£o êÂ s 9h: o vêÂ­deo vai ao bot como prêÂ©via. A partir das 14h o Radar Viral publica no YouTube **Radar Viral (lives24)** e manda o link ao bot (`touch <ediêÂ§êÂ£o>/SEGURAR` segura) | `auto/publicar.sh` + `auto/publicar-pendente.sh` (yt-pubx) | ê¢Å¡â¢ê¯Â¸Â timer 09:00 / 12:00 |
| 9 | SêÂ¡b/dom: longo 16:9 "A semana explicada" | `auto/roteiro-longo.md` ê¢â â `auto/longo.sh` | ê°Å¸Â¤â + ê¢Å¡â¢ê¯Â¸Â |
| 10 | Aprovar o formato, incluir/tirar canais, ligar a agenda | `canais.json`, `systemd/` | ê°Å¸Â§Â  sessêÂ£o com o Nei |

Desde 09/11/2026 os passos pesados (Whisper, retempo, voz local, render e encode) passam pela vaga da mêÂ¡quina (`motor/vaga.sh` ê¢â â `~/projetos/wifi/scripts/fila-vaga`; saêÂ­da 75 = mêÂ¡quina ocupada, avisa e nêÂ£o usa a pauta) e cada rodada grava inêÂ­cio e fim no placar (`~/projetos/wifi/scripts/rodada hoje`).

Rodada êÂ  mêÂ£o:

```bash
auto/rodar.sh diario 2026-10-26                 # rodada completa (avatar + lives24 + bot)
SEM_AVATAR=1 auto/rodar.sh diario 2026-10-26    # versêÂ£o com foto, sêÂ³ para o bot
REUSAR_ROTEIRO=1 auto/rodar.sh diario 2026-10-26  # refaz avatar/vêÂ­deo/publicaêÂ§êÂ£o com o roteiro jêÂ¡ aprovado
```

Agenda (`systemd/futviral-*.timer`, ligada em 26/10/2026): geraêÂ§êÂ£o êÂ s 09:00 (diêÂ¡rio segê¢â¬âsex, semanal no sêÂ¡bado, domingo no domingo) e publicaêÂ§êÂ£o pelo Radar Viral a partir das 14:00.

## Regras de cada vêÂ­deo

- Todo fato tem a fonte (canal, data, têÂ­tulo) na tela e um trecho literal da transcriêÂ§êÂ£o no `base.md`.
- Rumor sêÂ³ vale quando o clube confirma: transferêÂªncia êÂ© "rumor" ou "negociando" atêÂ© o clube confirmar.
- Sem torcer: tudo êÂ© atribuêÂ­do a quem disse.
- Sem aposta: nada de bet, odd ou palpite. O portêÂ£o reprova.
- Sem lance, gol, imagem de jogo ou escudo de clube (direito de transmissêÂ£o).
- Fala no tom do Nei, tratando por "vocêÂª", com nêÂºmeros por extenso. Fim: pergunta para comentar e CTA do ARCKHê AI.
- HeyGen sêÂ³ pelo estêÂºdio da assinatura. A oferta de troca de plano êÂ© sempre recusada.

## LicenêÂ§a

MIT.

---

<div align="center">
  <img src="capa/logo.svg" alt="Arckhé AI" height="32" />
  <p><strong>Arckhé AI</strong></p>
</div>
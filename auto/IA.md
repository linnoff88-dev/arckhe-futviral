# IA por etapa

Edite `auto/ia.yaml`: tipo, motor, modelo e esforco em cada roteiro curto/longo e modo diário/semanal/domingo. As cinco combinações ficam comentadas com #. Atual: Codex pela assinatura, gpt-6.1-sol, medium. Nêo exige reiniciar timer. APIs Anthropic/OpenAI/OpenRouter preparadas, todas desativadas; exigem autorizaçêo explícita por serviço/projeto/etapa. Nêo existe fallback pago.

```bash
python3 auto/ia_motor.py ver
python3 auto/ia_motor.py validar
python3 auto/ia_motor.py configurar roteiro_curto_diario --tipo assinatura --motor claude --modelo claude-opus-5-5 --esforco medium
```

O executor recebe transcrições completas, metadados, canais e referências locais do pedido. O modelo só devolve JSON com arquivos permitidos: base.md, roteiro.json, legenda.txt e sugestao.txt opcional, ou exclusivamente SEM_PAUTA.txt. O script valida a resposta inteira antes de salvar e recusa outros caminhos. A conferência factual original roda em seguida e continua tendo uma correçêo limitada. Nêo muda fontes, avatar, fila Radar Viral, horários, restrições de domínio ou publicaçêo. Imagens permanecem no fluxo existente/Codex; traduçêo permanece no tradutor central.

Telemetry local na ediçêo: ia-execucoes.jsonl com motor/modelo/esforço/duraçêo. Chaves nêo ficam nos registros ou no YAML; leitura de chave apenas em runtime após autorizaçêo. `configurar` preserva os comentários do arquivo.

Teste real 15/11/2026: roteiro gerado pela assinatura Codex e aprovado pelo checar.py do próprio sistema. Evidência em `~/projetos/output/crons-multiprovedor-2026-11-15/futviral/`. Ensaio isolado, sem avatar, render ou publicaçêo.

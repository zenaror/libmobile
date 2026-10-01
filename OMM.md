# Memória do libmobile no OMM

O libmobile implementa o protocolo Mobile Adapter GB para integração com emuladores e hardware. A memória selecionada fica em `memory/` e acompanha este repositório no Git.

## Antes de mudar o protocolo

```sh
omm search "socket Mobile Adapter" --scope global --scope libmobile
omm context "mudança de protocolo" --scope global --scope libmobile
```

Quando a tarefa envolver REON, libmobile ou Mobile Adapter GB, consulte também a skill compartilhada `reon-libmobile-expert`, se estiver disponível. Ela orienta a investigação; os resultados específicos desta biblioteca ficam no escopo `libmobile`.

## Cuidados conhecidos

- O README aponta `mobile.h` como documentação principal da API. Confirme as regras diretamente no cabeçalho e no código antes de alterar comportamento.
- O cabeçalho diz que abrir um socket que não foi fechado tem comportamento indefinido. A biblioteca não deve fazê-lo.
- Em 2026-08-31, um commit reverteu o fechamento preventivo do socket P2P em `command_wait_call_begin()`. A mensagem do commit não explica o motivo; não deduza intenção sem encontrar a conversa ou a evidência correspondente.
- A memória OMM e a busca ajudam a localizar contexto. O código e as fontes citadas continuam necessários para validar cada afirmação.
- O índice local `.omm/` pode ser reconstruído com `omm rebuild`; os arquivos em `memory/` são portáveis e versionados.

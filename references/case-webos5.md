# Caso real: LG webOS 5

Caso anonimizado de **20/10/2026**. Serve como evidência e diagnóstico, não como receita de remoção para outras TVs.

## Ambiente e resultado comprovado

- Modelo: LG 50UN7310PSC; versão informada webOS 5.6.2-21; firmware informado 04.64.00. O instalador confirmou SDK 5.6.2.
- SlopBro no commit `21023d56ca424607bb03d2939609cc658c655fc7`.
- Homebrew Channel 0.7.3, UID 0 e acesso por chave confirmados depois de ciclo físico de energia. Telnet desabilitado e porta fechada na conferência final.
- Seis apps de usuário removidos por autorização; oito apps escolhidos e os componentes essenciais preservados.
- Espaço livre no mesmo volume: 832560 → 1769060 KiB. Ganho aproximado de **915 MiB**, com variação possível por caches.
- Sem atualização LG, downgrade, RootMy.TV ou gravação manual de partições.

O usuário confirmou uso normal de HDMI → Home → apps e percebeu maior fluidez. Não houve benchmark nem confirmação final de reprodução, login/DRM e voz em todos os serviços.

## O que falhou e o que resolveu

| Observação | Evidência/correção | Aprendizado reutilizável |
| --- | --- | --- |
| Pareamento recusado ou cancelado | Novo pareamento com usuário disponível foi aceito. | Falha de pareamento não é falha comprovada da exploração. Não reenviar sem usuário pronto. |
| Developer Mode ainda aparecia | Inventário divergia da pré-checagem humana; usuário desinstalou e ausência foi conferida. | Confirmar também na TV/rede, quando possível. Não atualizar esse app para depois removê-lo. |
| Tela dizia “Service launched successfully” | Autoroot recuperado registrava conclusão e UID 0; Homebrew apareceu após ciclo completo. | Status do lançador não equivale ao estado final. Buscar logs antes de repetir a exploração. |
| Suspeita de que `run()` não era chamada | Instrumentação temporária comprovou entrada, saída e sucesso anterior. A hipótese foi descartada. | Não transformar uma hipótese inicial em causa confirmada. |
| Overlay antigo não saía com novo lançamento | Abrir a Home antes de relançar resolveu nesse ambiente. | Uma nova chamada não prova recarga da página. |
| Parâmetro errado de abertura | Não houve download; o diagnóstico voltou ao helper e parâmetros da versão correta do SlopBro. | Não adivinhar parâmetros de apps internos. |
| `luna-send` em SSH saía sem resposta | Manter stdin aberto por alguns segundos resolveu. | Resposta vazia pode ser efeito do transporte, não ausência de API. |
| `homeAutoLaunch` voltava a on | A descrição da TV revelou `checkHomeAutoLaunchValue`; gravar `user` junto com `off` persistiu no teste seguinte. | Leitura antes do reboot não comprova persistência. Consultar preferências relacionadas. |
| `boot_id` não mudava | Uptime, `btime`, marcador temporal e queda/retorno da conexão comprovaram o novo boot. | Não usar somente `boot_id` como critério nessa TV. |

A instrumentação foi uma etapa extraordinária autorizada; não deve ser reproduzida automaticamente. O payload original foi restaurado e os servidores temporários encerrados. O root já havia concluído em execução anterior: não há evidência para afirmar que a instrumentação ou uma flag isolada “fez o root funcionar”.

## Ajustes confirmados apenas neste ambiente

Consulte existência, valores aceitos, tipos e dependências na TV atual antes de reaproveitar qualquer linha. Faça backup dos valores reais, não dos valores desta tabela.

| Categoria/serviço | Chave | Resultado no caso |
| --- | --- | --- |
| general | homeAutoLaunch | `off` |
| general | checkHomeAutoLaunchValue | `user` |
| general | homeEffect | `off` |
| general | sportsAlarm | `off` |
| general | personalRecommend | `{"changedByUser":true,"value":"off"}` |
| option | livePlus | `off` |
| option | livePromotion | `off` |
| option | webOSPromotionVideo | `off` |
| option | demoMode | `off` |
| option | smartSoundDemo | `off` |
| config | com.webos.service.favoriteservice.enableWedge | `false` |

`general.homePromotion` e `option.promotionStreamer` já estavam off; `storeMode` já era home. Foram preservados. Protetor de tela e temporizador de suspensão não foram alterados.

Preferir HDMI ao ligar foi uma escolha deste usuário. Outra pessoa pode preferir abrir a Home; não imponha `homeAutoLaunch: off` como limpeza universal.

## Armazenamento e apps

As seis remoções de apps liberaram cerca de 39 MiB. A maior parte do ganho veio de dois vídeos de demonstração na área gravável `preload/storedemo`, somando cerca de 874 MiB. Cabeçalhos de mídia, finalidade de loja, ausência de uso, tamanho e hashes foram conferidos. As cópias no computador foram validadas antes da remoção dos dois arquivos exatos.

Um arquivo squashfs montado em `preload/storeusp` foi identificado e preservado. Extensão `.lge` não significou imagem de firmware nesses dois vídeos, mas isso não permite apagar outros `.lge` por extensão.

Apps foram removidos pelo serviço nativo, acompanhando conclusão e inventário. Home, configurações, loja, navegador, Live TV e HDMI permaneceram presentes. YouTube foi aberto e confirmado em primeiro plano, o que não comprova reprodução de conteúdo.

## Alcance da evidência

O caso comprova root persistente e os estados técnicos descritos em uma combinação de modelo/firmware. Não comprova suporte a todas as LG, ganho de desempenho mensurado, acesso arbitrário ao microfone ou compatibilidade de um launcher alternativo. IP, MAC, nomes pessoais, chaves e logs brutos foram deliberadamente excluídos do pacote.

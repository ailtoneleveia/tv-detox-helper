# Android TV e Google TV

Leia somente após confirmar o sistema. Para webOS, use [lg-webos.md](lg-webos.md). Este caminho usa permissões de depuração, não root.

## Conexão e inventário

Confira as ferramentas do computador. Se faltar ADB, obtenha Android Platform Tools de fonte oficial ou do gerenciador de pacotes confiável do computador. Não habilite root, bootloader ou recuperação modificada.

Guie o usuário até as opções de desenvolvedor e a depuração disponível no modelo. Há aparelhos com autorização por aviso na tela e outros com pareamento sem fio por código. Quando houver `adb pair`, a porta de pareamento pode ser diferente da de conexão. Não presuma `:5555` nem prometa pareamento sem fio em todas as versões.

Depois de conectar, confira `adb devices -l` e identidade do alvo. Use explicitamente o serial/endpoint verificado em **todos** os comandos, inclusive se houver apenas um dispositivo conectado. Nunca deixe um script operar o aparelho que o ADB escolher por padrão.

Exemplos de consultas, com `TV_SERIAL` previamente definido a partir do alvo confirmado:

```sh
adb -s "$TV_SERIAL" shell getprop ro.product.model
adb -s "$TV_SERIAL" shell getprop ro.build.version.release
adb -s "$TV_SERIAL" shell getprop ro.build.version.sdk
adb -s "$TV_SERIAL" shell pm list packages -f
adb -s "$TV_SERIAL" shell pm list packages -d
adb -s "$TV_SERIAL" shell df -k /data
```

Confirme o usuário/perfil Android em uso antes de alterar pacotes; não fixe usuário 0 em TVs com outro perfil ativo. Registre o launcher atual, os pacotes já desativados e o estado anterior das configurações que pretende mudar.

Compare inventário real, apps escolhidos e as regras atuais em [docs/debloat.md](https://github.com/Ansub/tv-detox/blob/b937fa5ce43bff13279f014a0ad37b459f49319d/docs/debloat.md). Uma lista de candidatos não autoriza desativá-los. Serviços Google, inicialização, entradas, controle, teclado, voz e DRM precisam de avaliação de dependências. Presuma necessário quando houver dúvida.

## TV Detox: inspecione o instalador inteiro

No snapshot pesquisado, [`install.sh`](https://github.com/Ansub/tv-detox/blob/b937fa5ce43bff13279f014a0ad37b459f49319d/install.sh) instala o APK, configura a Home e pode oferecer desativar launchers. Também pode desinstalar uma versão existente se a assinatura for incompatível. Não trate esse script como uma simples instalação sem efeitos adicionais.

A regra desta skill é preservar o launcher original. No Google TV, nunca habilite `GOOGLE_TV=1` para contornar a proteção. Não use respostas automáticas de aceite para perguntas do instalador. Se a versão atual não permitir instalar dentro do plano aprovado, faça apenas as etapas equivalentes revisadas ou preserve a Home nativa.

Um caminho limitado para manter o launcher original é obter o APK da release do próprio projeto e instalar sem a parte de desativação do script. Registre release, origem e hash; um hash calculado localmente identifica o download, mas não substitui uma assinatura ou checksum publicado pelo mantenedor. Confira pacote e componente antes de abrir/configurar.

Instalação e mudança de Home são mutações e precisam constar do plano. Use `adb -s` para o alvo e confira o resultado real de cada etapa. Se houver incompatibilidade de assinatura, pare e explique a perda potencial de dados antes de desinstalar a cópia existente. Não desative Play Protect globalmente ou outros controles para forçar uma instalação.

Configure os atalhos dos apps a partir dos IDs encontrados na própria TV. Só altere o padrão da Home depois de abri-la e testar navegação. Alguns aparelhos insistem na Home do fabricante; registre essa limitação em vez de remover componentes para forçar a troca.

## Limpeza aprovada e reversão

Prefira desativação por usuário, quando suportada, a desinstalação. Salve o estado anterior de cada pacote. Se antes estava no estado padrão, a reversão deve recuperar esse estado; não habilite indiscriminadamente todos os pacotes, inclusive os que já estavam desativados antes da operação.

Desativar um app de sistema não costuma liberar o espaço ocupado por sua cópia na imagem do sistema. Meça o ganho efetivo; não compense a ausência de ganho apagando arquivos do sistema ou dados de outros apps.

Animações são uma preferência opcional: salve os valores anteriores, aprove a mudança e não anuncie ganho de CPU por apenas reduzir a duração da animação. Preserve screensaver e proteções do painel. Não altere suspensão automática sem pedido do usuário.

Leia `check.sh` e `undo.sh` da versão utilizada antes de usá-los. `check.sh --fix` pode alterar a TV: não o trate como diagnóstico de leitura. Um verificador pode esperar comportamentos que esta skill deliberadamente preservou, como o launcher original: relate a diferença. Use o registro da operação como base da reversão, sem ações abrangentes além do conjunto modificado.

## Verificação final

Aguarde o tempo de persistência previsto pela documentação após a última alteração. Faça um único reinício de conferência quando o plano estiver pronto; repita apenas se um problema corrigido justificar outro teste.

Confira Home, pacotes preservados, pacotes alterados, espaço e o retorno da conexão. Peça teste humano de reprodução, HDMI, áudio, controle e voz. Primeiro registre os resultados, depois desative USB/network/wireless debugging que foi habilitado para esta operação. Informe como reativar para uma reversão futura e desconecte a sessão ADB.

Se uma falha de inicialização ou perda de acesso ocorrer, pare novas mudanças. Use somente o caminho de recuperação já confirmado no plano. Não prometa que `undo.sh` consegue recuperar um aparelho inacessível nem proponha reset de fábrica automaticamente.

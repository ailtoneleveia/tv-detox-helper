# LG webOS

Não é Android: ADB e APK não se aplicam. Pacotes de apps são IPK. TV Detox fornece objetivos de limpeza; seus scripts não devem rodar nesta plataforma.

## Escolha o menor acesso necessário

Para apenas retirar apps ou mudar publicidade, comece pelos menus nativos. Se o usuário quiser instalar apps sem root, avalie o fluxo Developer Mode documentado e o [Device Manager](https://github.com/webosbrew/dev-manager-desktop), respeitando seus limites. Isso é um caminho separado: não instale o app LG Developer Mode numa TV com root.

Se root/Homebrew já existem, confira seu funcionamento em vez de executar outra exploração. A presença do ícone Homebrew isoladamente não prova privilégio root. Reutilize o acesso autorizado, sem exigir reinstalação ou atualizar o Homebrew por rotina.

## SlopBro: compatibilidade e pré-checagens

Consulte [README atual do SlopBro](https://github.com/throwaway96/slopbro), firmware real e documentação de compatibilidade. O projeto registra uso em várias gerações de webOS; isso não é garantia de suporte a todo firmware. Se a documentação direcionar este aparelho para outro método, explique o limite antes de qualquer tentativa; não troque automaticamente de exploit.

Com autorização específica para SlopBro, confirme:

- TV e computador alcançáveis na mesma rede e usuário disponível no controle.
- Atualizações automáticas de firmware desligadas durante o procedimento.
- App LG Developer Mode ausente. Confira o inventário, quando possível; a confirmação humana pode divergir do estado real. Se estiver instalado, oriente a desinstalação sem atualizá-lo ou abri-lo. Não reinstale após root.
- Versão de webOS e IP local corretos; não confunda número do firmware LG com versão do sistema.

Clone o repositório original numa pasta de trabalho privada e leia `python3 slopbro.py --help`. Em Windows, use o comando Python 3 disponível, sem presumir shell POSIX. Registre commit e guarde saída de depuração com o código de saída real; um pipe para um gravador de log pode ocultar a falha do processo anterior.

Exemplo de forma de chamada, somente com variáveis já conferidas:

```sh
python3 slopbro.py --webos-version "$WEBOS_MAJOR" --debug "$TV_IP"
```

Não copie a versão 5 do caso de referência para outra TV. Avise o usuário quando aceitar o pareamento SSAP. Se o pedido não foi visto, confira o resultado; não diga que a TV aceitou porque os processos locais continuam abertos.

## Diagnóstico sem tentativas cegas

| Sintoma | Próximo passo |
| --- | --- |
| Pareamento rejeitado/cancelado | Confirme usuário pronto, identidade da TV e novo aceite antes de reenviar. Não houve prova de root. |
| TV não busca a página/arquivos | Confira servidor local, rota, IP da interface e alcance no firewall. `--local-ip` pode corrigir seleção da interface; não desative o firewall inteiro. |
| Página/overlay antigo continua visível | Oriente sair da tela; no caso documentado, abrir a Home antes de relançar resolveu a reutilização da tela. Não assuma que outra chamada substituiu o conteúdo. |
| “Service launched successfully” | É status de lançamento. Procure conclusão do instalador e estado real; não reinicie nem repita a exploração só por essa frase. |
| Rooting complete, mas ícone ausente | Confira o inventário e a orientação de reinício do instalador, depois de garantir ausência do Developer Mode. |
| Opção de execução parece incompatível | Consulte a opção na versão atual. `--no-fake-service-path` só pertence a um diagnóstico fundamentado, não a uma sequência automática de alternativas. |

Logs como `/tmp/autoroot.log` e `/tmp/slopbro.log` foram úteis no caso webOS 5. Leia somente o necessário e limite o conteúdo; não faça cópia genérica de diretórios de contas. Uma falha de leitura não prova que o instalador não executou.

Se a exploração falhar ou a compatibilidade continuar incerta, pare as tentativas, preserve logs e explique o estado. Diagnóstico de leitura pode continuar. RootMy.TV, outros exploits, alterações no payload ou atualização de firmware não são fallback automático. Instrumentação temporária foi necessária num caso, mas não faz parte do caminho padrão desta skill.

## Comprove a conclusão e a persistência

Procure a conclusão do autoroot, confirme Homebrew instalado e funcional e, se houver canal administrativo, execute `id` e confira UID 0. Só então programe o teste de persistência indicado pelo instalador. O usuário precisa estar disponível para religar a TV.

No caso de referência houve desligamento, retirada da tomada por 30 segundos e religamento. Não faça isso durante escrita/instalação nem transforme o ciclo físico num requisito de toda simples limpeza. Depois confira novamente Homebrew e UID. Abrir Homebrew uma vez pode ser necessário para inicializar seu serviço.

Quick Start pode produzir retomada em vez de boot completo. Use uptime, tempo de boot e queda/retorno da conexão para distinguir. No caso registrado, `boot_id` repetiu após reboot; não exija sua mudança como prova única. Compare `btime` com marcador apenas se o relógio estiver coerente; combine evidências e confirmação humana.

## Acesso administrativo depois do root

O [startup do Homebrew](https://github.com/webosbrew/webos-homebrew-channel/blob/1a2b969e992617c9ba4b8a891542cb93a9e07b0a/services/startup.sh) pesquisado reconhece marcadores em `/var/luna/preferences/`: `webosbrew_sshd_enabled` e `webosbrew_telnet_disabled`. Confirme a implementação instalada antes de usá-los.

Provisione uma chave SSH dedicada suportada pelo daemon, preservando as chaves existentes em `/home/root/.ssh/authorized_keys`. A chave privada fica no computador, com permissões restritas; confirme a identidade do host. Teste nova conexão por chave antes de fechar o acesso anterior.

Ter uma chave não prova que login por senha esteja desabilitado. Verifique a política efetiva após boot; não deixe senha padrão como solução permanente nem presuma que uma opção passada manualmente ao daemon persistirá. Não edite scripts de boot para impor uma configuração não documentada.

Telnet do Homebrew é sem autenticação: use apenas se indispensável no bootstrap e encerre depois de comprovar SSH. Desabilitar no próximo boot não mata o processo atual; confira a porta e, se necessário, encerre apenas o PID identificado do serviço da operação. Não use encerramento amplo de processos. Se houver modo failsafe, investigue a causa em vez de remover a recuperação para obter um teste “verde”.

## Limpeza adaptada ao webOS

Monte uma lista de equivalências com os itens do TV Detox: promoções/recomendações, apps dispensáveis, Home, espaço, efeitos e verificação. Registre itens sem equivalente comprovado. Leia [case-webos5.md](case-webos5.md) se os sintomas ou a versão forem semelhantes.

Consulte as chaves e valores aceitos **nesta TV** antes de escrever. Faça backup do valor e tipo completos. Confirme por leitura, teste e nova leitura após reboot. Não sobrescreva um objeto inteiro ao tentar mudar só um campo sem conferir o contrato da API.

Em SSH no webOS 5 do caso, `luna-send` terminava sem resposta quando stdin fechava imediatamente. Manter stdin aberto resolveu. Exemplo **somente leitura** para esse sintoma:

```sh
sleep 4 | luna-send -n 1 -w 3000 \
  luna://com.webos.settingsservice/getSystemSettings \
  '{"category":"general","keys":["homeAutoLaunch"]}'
```

Execute esse comando na TV pelo canal root já validado. Não interprete resposta vazia como chave inexistente; confira processo, stdin e sintaxe primeiro. APIs de webOS OSE não garantem paridade com uma TV comercial.

Para apps, confirme ID, nome, origem, `removable` e ausência na lista preservada. Use o mecanismo nativo suportado, não apague a pasta do app. No caso, `com.webos.appInstallService/remove` com acompanhamento até o estado final funcionou; `dev/remove` não atendia aos apps de loja. Confirme ausência no inventário após a conclusão, sem tratar apenas o aceite inicial como sucesso.

Não suponha que “removível” significa dispensável: app usado e dependência continuam protegidos. Reinstalação pode depender da loja e exigir login; não prometa restauração dos dados sem backup compatível.

Para arquivos grandes de demonstração, siga os controles de [operacao-reversao.md](operacao-reversao.md). Não toque em conteúdo montado, squashfs, partições ou arquivos de função incerta.

## Encerramento

Encerre servidores locais e restaure qualquer alteração temporária autorizada de diagnóstico. Remova apenas os arquivos temporários cuja origem e hash sejam comprovados; não faça “faxina” genérica em downloads. Preserve backups e logs privados.

Confira após reinício o acesso administrativo, configurações, apps, espaço e portas temporárias. O teste humano de reprodução/voz continua necessário. Não instale um launcher de compatibilidade incerta nem um app de IA como parte implícita da limpeza.

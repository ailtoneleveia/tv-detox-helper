# Registro, reversão e testes

Use estes controles nas duas rotas. A pasta da operação pertence ao usuário e é separada da skill compartilhável. Escolha um diretório privado local com data e identificação não sensível; confirme que não é um repositório público nem uma pasta sincronizada publicamente.

## Antes de mudar

Registre por alvo: estado anterior, ação aprovada, efeito esperado, como desfazer e teste que confirmará o resultado. Inclua os estados já desativados para não habilitá-los indevidamente depois. Identificadores de apps vêm do inventário real, não de uma tabela genérica.

Guarde configurações estruturadas com os tipos originais. Uma string `"off"`, um booleano `false` e um objeto são contratos diferentes. Prefira backup por chave/serviço a copiar dados de conta ou dumps gerais desnecessários.

Para cada comando, diferencie: solicitação aceita, operação concluída, estado relido e comportamento testado. Grave o resultado e a evidência; não coloque uma remoção que falhou na lista de ações concluídas.

## Remoção de mídia de demonstração

Esse procedimento é opcional e requer autorização para os arquivos exatos. Antes de remover:

1. Confirme que é mídia dispensável de loja, não um recurso do sistema, app, imagem montada ou atualização. Extensão e nome sozinhos não bastam.
2. Resolva o caminho real. Confirme arquivo regular, ausência de symlink, ponto de montagem e que não está em uso. Verifique também a função: “não está aberto” não prova que é dispensável.
3. Confirme demos/modo loja desligados. Preserve diretórios, índices e arquivos de versão não incluídos no plano.
4. Copie para o backup privado do computador. Compare tamanho e SHA-256 da origem e da cópia, sem usar o hash da própria cópia como se fosse o da origem.
5. Registre caminho, tamanho, hash e restauração. Só então remova os arquivos individualmente, sem globs ou remoção recursiva.

Se não houver um meio confiável de validar a cópia, preserve o arquivo na TV. Não inclua os vídeos no pacote da skill.

## Relatório e reversão

Crie `RELATORIO.md` e `REVERTER.md` na pasta da operação. Nomes são sugestões; adapte à convenção do usuário. O relatório precisa conter:

| Informação | Evidência mínima |
| --- | --- |
| Alvo | Modelo, sistema, firmware e IP conferidos; diferencie informação humana de consulta. |
| Ferramentas | Origem, commit/release e método utilizado. |
| Root, se aplicável | UID, Homebrew e teste após reinício; não apenas o status de lançamento. |
| Alterações | ID/chave/caminho, antes/depois, sucesso ou falha e referência ao log. |
| Recursos | Armazenamento do mesmo volume e mesma unidade antes/depois. RAM, se medida, separadamente. |
| Funcionamento | Testes remotos, confirmação humana e pendências. |
| Acesso | Depuração desativada ou acesso administrativo mantido, autenticação e portas verificadas. |
| Recuperação | Onde estão backups e quais ações recuperam o conjunto alterado. |

`REVERTER.md` deve orientar a restauração dos valores originais, estados dos pacotes, padrão da Home e cópias de mídia. Anote se será preciso reativar depuração ou SSH. Não invente uma restauração de login/dados que não foram salvos. Não use atualização, unroot ou reset de fábrica como “desfazer limpeza”.

Nunca distribua o diretório operacional junto da skill: ele pode conter logs com tokens de pareamento, chaves SSH, IP/MAC, serial e outros dados privados. Um pedido para compartilhar a skill não autoriza compartilhar esses registros.

## Quando interromper

Pare novas mutações diante de alvo incerto, perda de acesso, erro de instalação, dependência desconhecida ou falha de backup. Preserve evidências e use somente a recuperação previamente verificada. Retome quando houver uma condição nova comprovada ou orientação autorizada, sem repetir o mesmo comando por insistência.

Pode encerrar o trabalho técnico com testes humanos pendentes, desde que relate explicitamente essa limitação. Não descreva como confirmado um streaming, microfone ou login que o usuário ainda não testou.

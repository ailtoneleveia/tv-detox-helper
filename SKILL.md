---
name: tv-detox-helper
description: Conecta, diagnostica e limpa TVs Android/Google TV e LG webOS usando TV Detox ou SlopBro/Homebrew quando compatíveis. Use para reduzir bloat, promoções e ocupar menos armazenamento, com preservação dos apps e reversão. Outros sistemas recebem somente orientação nativa compatível.
---

# TV Detox Helper

Ajude o usuário a limpar a TV real à sua frente. Identifique o sistema antes de escolher ferramentas. Conduza as ações no controle remoto com instruções simples e execute no computador local o que estiver autorizado. Instalar esta skill não autoriza root nem mudanças na TV.

## Escolha a rota

| Sistema confirmado | Referência a ler | Caminho |
| --- | --- | --- |
| Android TV / Google TV | [android-google-tv.md](references/android-google-tv.md) | ADB e TV Detox; sem root. |
| LG webOS | [lg-webos.md](references/lg-webos.md) | Limpeza nativa; SlopBro e Homebrew opcionais, se compatíveis e autorizados. |
| Outro, ou sistema ainda desconhecido | Identificação e documentação do fabricante | Não aplicar comandos dessas duas rotas. |

Leia também [operacao-reversao.md](references/operacao-reversao.md) antes de alterar qualquer TV. Consulte [sources.md](references/sources.md) para localizar os projetos originais e os commits pesquisados. Não carregue a referência da outra plataforma sem necessidade.

## Identifique e estabeleça o escopo

1. Reutilize os dados já fornecidos. Obtenha marca, modelo completo, sistema, firmware, sistema do computador e apps a manter. Confirme que o usuário pode administrar a TV e está disponível para aceitar pareamentos. Não deduza o sistema apenas pela marca.
2. Explique como encontrar o IP e as versões nos menus se necessário. Confirme a identidade da TV antes de usar seu endereço. Computador e TV precisam se alcançar pela rede; VPN, Wi-Fi de convidados ou isolamento de clientes podem impedir isso. Não desative o firewall inteiro para resolver a conexão.
3. Verifique se o ambiente tem terminal local, acesso à rede da TV e ferramentas necessárias. Uma conversa na nuvem sem esse acesso não consegue operar a TV: nesse caso, forneça comandos revisados para execução humana e peça os resultados.
4. Leia a documentação e os scripts atuais dos projetos escolhidos. Registre commit/versão, compatibilidade conhecida e incertezas. Uma exploração funcionar numa versão não prova suporte a todos os firmwares do mesmo modelo.
5. Faça inventário sem mutações: apps, componentes críticos, configuração relevante e armazenamento. Resolva nomes ambíguos com o usuário; não transforme um erro de digitação em autorização para remover outro app.

## Autorize um plano concreto

Apresente uma lista curta com ações, alvos, efeito esperado, dados que podem ser perdidos e reversão. Explique as advertências relevantes do projeto: desativar componentes errados pode causar falhas de inicialização; root adiciona risco e acesso administrativo.

Peça aprovação do conjunto de alterações se ela ainda não existir para esses alvos. Root exige autorização específica. Depois execute o conjunto aprovado sem repetir perguntas para cada comando. Mudança de alvo, remoção com perda de dados ou troca de método de root exige nova decisão. Se o usuário pediu somente diagnóstico, fique em leitura.

## Execute em etapas verificáveis

- Registre o estado anterior antes de cada grupo. Use somente IDs e chaves conferidos na TV; não use listas de remoção por marca, palavras-chave ou exemplos desta skill.
- Prefira configurações e desativação reversível. Desinstalação não equivale a desativação: pode apagar dados e depender de catálogo ainda disponível para reinstalar.
- Preserve os apps escolhidos e o launcher original. Preserve configurações, teclado, voz, controle remoto, rede, Bluetooth, HDMI, tuner, reprodução/DRM, lojas e componentes de inicialização. Dependência desconhecida permanece intacta.
- Confira o resultado por nova consulta e teste funcional, não apenas pelo código de saída. Não repita tentativas idênticas após falha. Em perda de rede, pare de enviar mutações e restabeleça o acesso para verificar o estado.
- Não atualize firmware, force downgrade, faça reset de fábrica, desbloqueie bootloader ou grave imagens/partições. A persistência administrada pelo instalador webOS compatível pertence à rota autorizada; não a substitua por edições manuais em scripts de boot.
- Preserve protetor de tela e proteções do painel, especialmente OLED. Não imponha suspensão em cinco minutos ou outra preferência que o usuário não pediu.
- Não exponha ADB, SSH ou Telnet à internet. Ao terminar, encerre servidores temporários e retire depuração temporária. Acesso root persistente precisa de autenticação adequada e documentação.

## Verifique e conclua

Depois de salvar as alterações e aguardar a persistência exigida pela plataforma, faça o reinício previsto no plano. Não reinicie durante instalação ou gravação de dados. Confirme que a TV voltou, que as preferências persistiram e que os apps preservados continuam presentes.

Peça ao usuário um teste curto: Home, vídeo e som no YouTube, conteúdo em outro streaming, HDMI e voz se utilizada. Consultar o app em primeiro plano não comprova reprodução, login ou DRM. Registre o que foi verificado remotamente e o que depende de confirmação humana.

Entregue relatório e reversão locais conforme [operacao-reversao.md](references/operacao-reversao.md), com armazenamento antes/depois na mesma unidade. RAM livre e armazenamento livre são medidas diferentes. Percepção de maior fluidez não é benchmark. Diga o que ficou pendente sem apresentar a tarefa como totalmente validada.

## Limite de generalização

O [caso webOS 5](references/case-webos5.md) registra uma operação real bem-sucedida e suas correções. Leia-o quando estiver nessa rota ou diagnosticando os mesmos sintomas. Ele não autoriza reutilizar endereços, IDs de apps, arquivos ou valores em outra TV. O caminho Android foi pesquisado no projeto original; esta skill não foi testada fisicamente em TVs Android.

Samsung Tizen, Roku, VIDAA, Fire OS e sistemas não cobertos exigem avaliação própria. Não trate Fire OS como automaticamente compatível só por ter base Android. Ofereça limpeza pelos menus quando disponível e informe o limite.

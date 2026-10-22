# TV Detox Helper

Skill em português para Claude Code e Codex com execução local. Organiza a conexão e a limpeza de Android TV/Google TV com TV Detox e de LG webOS com configurações nativas ou SlopBro/Homebrew compatível.

É uma skill independente. Os projetos citados são das respectivas comunidades, não dos fabricantes. Não acompanha exploits, APKs, IPKs, chaves ou scripts que alterem a TV automaticamente.

## Instalar

Você precisa do Claude Code ou Codex com terminal local, Git instalado e acesso à mesma rede da TV. A instalação da skill não executa alterações na TV.

### Pelo terminal — macOS ou Linux

Para **Codex**:

```sh
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/ailtoneleveia/tv-detox-helper.git "$HOME/.agents/skills/tv-detox-helper"
```

Para **Claude Code**:

```sh
mkdir -p "$HOME/.claude/skills"
git clone https://github.com/ailtoneleveia/tv-detox-helper.git "$HOME/.claude/skills/tv-detox-helper"
```

Escolha o comando do seu assistente. Se a pasta já existir, preserve sua instalação: não a apague para repetir o comando. Para usar nos dois assistentes, instale nos dois destinos.

### Sem terminal — Windows, macOS ou Linux

1. [Baixe o ZIP deste repositório](https://github.com/ailtoneleveia/tv-detox-helper/archive/refs/heads/main.zip), ou use **Code → Download ZIP** no GitHub.
2. Extraia e renomeie a pasta `tv-detox-helper-main` para `tv-detox-helper`.
3. Copie a pasta **inteira**, incluindo `SKILL.md`, `references` e `agents`, para um dos destinos:

| Assistente | Para seu usuário | Apenas num projeto |
| --- | --- | --- |
| Codex | `~/.agents/skills/tv-detox-helper/` | `.agents/skills/tv-detox-helper/` |
| Claude Code | `~/.claude/skills/tv-detox-helper/` | `.claude/skills/tv-detox-helper/` |

`~` é sua pasta de usuário. No Windows, os destinos pessoais equivalentes começam com `%USERPROFILE%\.agents\skills\` ou `%USERPROFILE%\.claude\skills\`. O arquivo precisa terminar em `tv-detox-helper/SKILL.md`, sem uma segunda pasta de mesmo nome no meio.

Abra uma nova sessão caso a skill não apareça. Políticas do ambiente podem limitar descoberta de skills ou acesso ao terminal/rede. Para operar a TV, o assistente precisa rodar num computador que alcance a rede dela; enviar o ZIP a uma conversa sem acesso local não cria essa conexão.

Locais e formatos conferidos na [documentação do Codex](https://learn.chatgpt.com/docs/build-skills) e do [Claude Code](https://code.claude.com/docs/en/skills).

## Usar

No Codex:

```text
Use $tv-detox-helper para limpar minha TV. O modelo é [modelo, se souber].
Quero manter [meus apps]. Estou com o controle e o computador na mesma rede.
Comece identificando o sistema e apresente o plano antes de alterar a TV.
```

No Claude Code:

```text
/tv-detox-helper
```

Depois informe o modelo e os apps a manter. Se não souber a versão ou o IP, a skill orienta a encontrar nos menus.

## O que está documentado

- Android/Google TV: conexão ADB, inventário, proteção do launcher original, instalação compatível do TV Detox e reversão.
- LG webOS: pré-checagens do SlopBro, confirmação real do root, Homebrew, limpeza nativa e teste de persistência.
- Caso real anonimizado de webOS 5: root persistente e cerca de 915 MiB liberados, incluindo as falhas de diagnóstico corrigidas.
- Logs privados, backups, fontes com commits e distinção entre verificação remota e teste humano.

As instruções completas estão em [SKILL.md](SKILL.md). A documentação específica está em [Android/Google TV](references/android-google-tv.md) e [LG webOS](references/lg-webos.md). Consulte também o [caso webOS 5](references/case-webos5.md), o [registro e reversão](references/operacao-reversao.md) e as [fontes pesquisadas](references/sources.md).

Versão do pacote: **1.0.0**. Pesquisa: **21/10/2026**. Reconsulte a compatibilidade a cada operação.

## Limites

Não garante suporte a qualquer TV. Em sistemas sem caminho documentado, orienta somente ajustes nativos. Root só acontece com compatibilidade e autorização específica. Desativar ou remover componentes errados pode afetar a inicialização; backup não garante recuperação de uma TV sem acesso.

A operação webOS registrada foi comprovada nos logs e no ciclo de energia. Não houve confirmação humana final de todos os streamings e da voz. O pacote foi validado estruturalmente; não foi executado novamente numa TV nem testado fisicamente em Android.

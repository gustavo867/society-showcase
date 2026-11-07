<img src="assets/icon.svg" width="64" height="64" alt="">

# Society Scheduler

**Gestão de reservas para society com React Native, Expo e TypeScript.**

[English](README.en.md) · [Minhas contribuições](#minhas-contribuições) · [Arquitetura](#arquitetura) · [LinkedIn](https://www.linkedin.com/in/gustavo-santana-83ba611a6/)

Uma agenda para consultar horários, administrar reservas semanais e acompanhar confirmações. O aplicativo combina persistência local, lembretes no aparelho e backup de dados — com atenção a conflitos de horários, compatibilidade entre versões e consistência entre o estado da aplicação e o sistema operacional.

## Veja o aplicativo

<a href="assets/demo.gif"><img src="assets/demo.gif" width="280" alt="Tour das telas de agenda, criação de reserva e backup"></a>

*Tour de três capturas reais do preview web, com dados fictícios. O GIF apresenta telas; não é uma gravação de interação nem uma release nativa.*

<details>
<summary>Ver as três telas em detalhe</summary>

| Agenda | Nova reserva | Backup |
| --- | --- | --- |
| ![Agenda com equipes fictícias](assets/screenshots/agenda.png) | ![Formulário de reserva com dados fictícios](assets/screenshots/create.png) | ![Backup com registros de demonstração](assets/screenshots/backup.png) |

Capturas em 390 × 844 CSS pixels. [Origem e escopo das imagens](assets/README.md).

</details>

## Funcionalidades

- **Agenda e disponibilidade:** navegação por dia e semana, calendário mensal, busca e filtros.
- **Reservas recorrentes:** horários semanais com exclusões por data e verificação de sobreposição por campo.
- **Confirmação e lembretes:** acompanhamento do status da reserva e coordenação de notificações locais.
- **Backup e restauração:** exportação/importação JSON, compatibilidade com formatos legados e confirmação antes de substituir os dados.

## Minhas contribuições

Contribuições de manutenção atribuídas a **Gustavo** no histórico do projeto:

- **Atualização de SDK e dependências**, incluindo configurações Expo/React Native.
- **Evolução do backup e da restauração**, com importação/exportação e tratamento de formatos legados.
- **Correções nas regras de agenda**, incluindo conflitos recorrentes e limites de horários.
- **Reconciliação de notificações**, com serialização de operações e tratamento de requests ausentes ou duplicados.
- **Testes automatizados** para regras de agenda, backups e coordenação de notificações.

O desenvolvimento contou com **assistência do Codex**. O [registro de contribuições](docs/evidence.md#e09--attribution) detalha as evidências e o escopo de atribuição, sem atribuir autoria exclusiva do aplicativo.

## Arquitetura

As telas usam regras de domínio e ações Zustand; o AsyncStorage persiste o estado local. Um módulo de notificações coordena os requests do sistema operacional, e as APIs de arquivos permitem transferir backups manualmente entre instalações.

![Arquitetura: telas React Native, regras de domínio, Zustand, AsyncStorage, notificações e arquivos](assets/architecture.png)

A aplicação mantém os dados no dispositivo. Não foi identificado backend de negócio ou sincronização remota. [Componentes e fluxos de dados](docs/architecture.md).

## Três decisões de engenharia

| Desafio | Solução implementada | Por que importa |
| --- | --- | --- |
| Conflitos em reservas semanais | Comparar recorrência, campo e intervalos sem horizonte anual fixo | Evita limitar a detecção de conflitos a uma janela arbitrária de semanas |
| Estado local diferente dos lembretes do sistema | Reconciliar requests pendentes e serializar os efeitos nativos | Permite reutilizar lembretes válidos, cancelar duplicados e recriar ausentes |
| Dados antigos em outra instalação | Normalizar backups versionados e remover identificadores de notificações do aparelho | Preserva compatibilidade e permite reconstruir lembretes no destino |

Os lembretes agendam a próxima ocorrência elegível e são renovados na atividade da aplicação. A restauração aguarda a hidratação do estado e exige confirmação de substituição. [Decisões e trade-offs](docs/engineering-decisions.md).

## Tecnologias aplicadas

| Área | Tecnologias |
| --- | --- |
| Aplicação mobile | React Native, Expo, React, TypeScript |
| Estado e persistência | Zustand, AsyncStorage |
| Navegação e interface | React Navigation, styled-components, Safe Area Context |
| Datas e validação | date-fns, Yup e regras de domínio em TypeScript |
| Integrações nativas | Expo Notifications, FileSystem, DocumentPicker, Sharing e Updates |
| Testes e ferramentas | Node test runner, tsx, ESLint, Bun e configuração EAS |

[Versões examinadas e evidências de uso](docs/evidence.md#e00--technology-versions).

## Testes e qualidade

Na revisão de **7 de novembro de 2026**, **36 testes passaram** e a checagem **TypeScript terminou sem erros**. A suíte cobre regras de agenda, backups e coordenação de notificações com operações simuladas.

Há pendências de ESLint e um erro observado no preview web. Os resultados completos e as validações nativas ainda necessárias estão em [Testes, qualidade e entrega](docs/testing-and-delivery.md); as verificações realizadas não estabelecem prontidão para produção.

Para aprofundar: [cinco desafios técnicos](docs/technical-challenges.md) · [registro de evidências](docs/evidence.md).

## Sobre este repositório

Este é o showcase público de um aplicativo com código-fonte privado. **Society Scheduler** é o nome de apresentação usado neste portfólio. O repositório reúne documentação e imagens sanitizadas; não contém o código do aplicativo nem um pacote instalável. [Escopo do material público](docs/disclosure.md).

**Gustavo** · [Perfil profissional no LinkedIn](https://www.linkedin.com/in/gustavo-santana-83ba611a6/)

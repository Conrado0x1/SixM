<p align="center">
  <img src=".github/assets/sixm-logo.png" width="140" alt="SixM logo">
</p>

<h1 align="center">SixM</h1>

<p align="center">
  <strong>Independent GTA VI Multiplayer Framework</strong><br>
  A new era of multiplayer.
</p>

<p align="center">
  C++20 · Qt 6 · CMake · Windows x64 · C# / ASP.NET Core · PostgreSQL
</p>

<p align="center">
  <a href="#estado-do-projeto">Estado</a> ·
  <a href="#compilar-e-executar">Instalação</a> ·
  <a href="#arquitetura">Arquitetura</a> ·
  <a href="#roadmap">Roadmap</a>
</p>

---

SixM é um projeto independente que pretende construir uma plataforma multiplayer modular para Grand Theft Auto VI: launcher próprio, serviços de identidade, servidores dedicados e ferramentas para developers.

O desenvolvimento é progressivo. A entrega atual contém um launcher funcional e a primeira fundação do backend de autenticação. **Ainda não é possível jogar GTA VI através do SixM.** A viabilidade da integração com o jogo será investigada separadamente.

## Estado do projeto

| Componente | Estado atual |
| --- | --- |
| Launcher v0.1.0 | Compilado e testado em Windows x64 |
| Interface | Dashboard escuro, navegação superior e identidade SixM |
| Arranque | Ecrã inicial com logo e progresso de tarefas locais |
| Configuração | Diretório do jogo guardado localmente |
| SixM Master | API experimental de contas e sessões com PostgreSQL |
| Autenticação no launcher | Ainda não ligada ao backend |
| Multiplayer e integração com o jogo | Não implementados |
| Atualizador e assinatura digital | Planeados |

Os servidores e as contagens mostrados no dashboard são **dados de demonstração**, identificados na interface. A coluna de novidades apresenta conteúdo local do projeto. O ecrã inicial não simula downloads.

<p align="center">
  <img src=".github/assets/dashboard.png" width="960" alt="SixM launcher dashboard">
</p>

<p align="center">
  <img src=".github/assets/startup.png" width="250" alt="SixM startup screen">
</p>

## Compilar e executar

### Launcher

Requisitos: Windows x64, Visual Studio 2022 com MSVC v143 e Windows SDK, Qt 6.8.3 MSVC 2022 64-bit e CMake.

```powershell
$env:SIXM_QT_ROOT = 'C:\Qt\6.8.3\msvc2022_64'
$env:PATH = "$env:SIXM_QT_ROOT\bin;$env:PATH"
cmake --preset windows-msvc2022
cmake --build --preset release
ctest --preset release
```

O pacote portátil inclui as bibliotecas necessárias: extrair a pasta completa e abrir `SixM.exe`. Não mover apenas o executável.

Instruções completas e configuração desta máquina: [Compilar o launcher](Documentation/Building.md).

### Backend e autenticação

O SixM Master utiliza .NET 10 e PostgreSQL. Disponibiliza registo, login, perfil autenticado, renovação de tokens e encerramento de sessões. As migrações da base de dados são versionadas no código.

A configuração utiliza variáveis do ambiente, incluindo `ConnectionStrings__SixM`; não guardar credenciais no repositório. O serviço desta etapa é **exclusivo para desenvolvimento**, sem confirmação de email nem recuperação de password.

[Configurar e executar o Master](Master/README.md) · [Arquitetura da autenticação](Documentation/Backend.md)

## Arquitetura

```text
SixM/
├── Launcher/          # Aplicação desktop C++ / Qt
├── Client/            # Reservado para o cliente do framework
├── Server/            # Reservado para o servidor dedicado
├── Core/              # Reservado para componentes partilhados
├── Networking/        # Reservado para o protocolo multiplayer
├── SDK/               # Reservado para recursos e scripting
├── Master/            # API C# de identidade e sessões
├── Database/          # Preparação da base de dados e referências ao schema
├── Tests/             # Testes do launcher e integração do backend
└── Documentation/     # Arquitetura, instalação e validação
```

Os módulos reservados não contêm implementações fictícias. Launcher, backend e futuros servidores podem ser desenvolvidos e testados independentemente do jogo.

## Segurança

A fundação de autenticação utiliza ASP.NET Core Identity para passwords, tokens aleatórios com apenas os respetivos hashes guardados em PostgreSQL, sessões revogáveis, rotação de refresh tokens, lockout e limites de pedidos. HTTP é permitido apenas em loopback; acessos remotos exigem HTTPS.

TLS operacional, confirmação de email, recuperação de conta, MFA, atualizações verificadas e assinatura digital dos executáveis ainda precisam das respetivas etapas de implementação e validação. O launcher de desenvolvimento não está assinado. C++ não impede a análise ou modificação do software.

## Roadmap

Estes milestones são objetivos de desenvolvimento, sujeitos à viabilidade técnica.

| Versão | Objetivo |
| --- | --- |
| v0.1.0 | Launcher Foundation |
| v0.2.0 | Interface e navegação |
| v0.3.0 | Server Browser |
| v0.4.0 | Networking |
| v0.5.0 | Server Core |
| v0.6.0 | Player Management |
| v0.7.0 | Resource System |
| v0.8.0 | Lua Runtime |
| v0.9.0 | Investigação da integração com GTA VI |
| v1.0.0 | Primeiro multiplayer, condicionado à viabilidade |

A fundação do Master está a ser desenvolvida separadamente; não significa que os milestones de multiplayer estejam concluídos.

## Desenvolvimento e testes

- [Arquitetura do launcher](Documentation/Architecture.md)
- [Ambiente e validação](Documentation/Validation.md)
- [Backend e testes de autenticação](Documentation/Backend.md)
- [Dependências e licenças de terceiros](THIRD_PARTY_NOTICES.md)

As alterações devem manter a separação dos módulos, incluir instruções reproduzíveis e testes adequados. Não submeter credenciais, tokens, ficheiros proprietários do jogo ou código destinado a contornar proteções.

## Identidade e licenciamento

SixM é um projeto independente, sem afiliação ou aprovação da Rockstar Games ou Take-Two Interactive. Não inclui ficheiros do jogo, e não assume a existência de uma API pública de modding ou de uma versão PC disponível.

O logo e o banner foram fornecidos pelo titular do projeto. A licença do código SixM ainda deve ser escolhida pelo titular antes de uma publicação com licença aberta. As bibliotecas e os ícones mantêm as suas próprias licenças, listadas em [Third-party notices](THIRD_PARTY_NOTICES.md).

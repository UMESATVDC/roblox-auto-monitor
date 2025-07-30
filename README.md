# Roblox Auto Monitor

Um aplicativo Android desenvolvido em Flutter para monitorar automaticamente o status do Roblox e reiniciá-lo quando necessário.

## 🚀 Funcionalidades

### Principais
- **Monitoramento Automático**: Verifica o status do Roblox a cada 30 segundos
- **Detecção Inteligente**: Identifica quando o Roblox:
  - Foi fechado/crashou
  - Travou ou congelou (sem renderizar frames)
  - Foi minimizado (não está em primeiro plano)
- **Reinício Automático**: Fecha e reabre o Roblox automaticamente no jogo específico
- **Notificações Discord**: Envia alertas via webhook quando problemas são detectados

### Interface do Usuário
- **Controles Simples**: Botões grandes para iniciar/parar monitoramento
- **Status Visual**: Indicadores claros do estado atual do monitoramento
- **Configuração de Webhook**: Campo para inserir e salvar URL do Discord
- **Auto-início**: Toggle para iniciar monitoramento automaticamente ao ligar o celular
- **Indicador de Permissões**: Mostra se as permissões necessárias foram concedidas

### Funcionalidades Técnicas
- **Serviço em Primeiro Plano**: Mantém o monitoramento ativo mesmo com tela bloqueada
- **Notificação Persistente**: Mostra status discreto na barra de notificações
- **Código Nativo Android**: Integração com APIs nativas para melhor detecção
- **Armazenamento Local**: Salva configurações automaticamente
- **Compatibilidade**: Android 7+ sem necessidade de root

## 📋 Requisitos

### Dispositivo
- Android 7.0 (API 24) ou superior
- Roblox instalado (pacote: com.roblox.client)
- Conexão com internet para webhooks do Discord

### Permissões Necessárias
- **Internet**: Para enviar mensagens webhook
- **Foreground Service**: Para monitoramento em segundo plano
- **Boot Completed**: Para auto-início após reinicialização
- **Usage Stats**: Para monitorar outros aplicativos (requer configuração manual)
- **System Alert Window**: Para notificações
- **Kill Background Processes**: Para fechar/reiniciar Roblox

## 🛠️ Instalação e Compilação

### Pré-requisitos
1. **Flutter SDK** (versão 3.24.5 ou superior)
2. **Android Studio** com Android SDK
3. **Java 17** (recomendado para compatibilidade)

### Passos para Compilação

1. **Clone ou extraia o código fonte**
```bash
cd roblox_auto_monitor
```

2. **Instale as dependências**
```bash
flutter pub get
```

3. **Configure o Android SDK**
```bash
flutter doctor --android-licenses
```

4. **Compile o APK**
```bash
flutter build apk --release
```

5. **Localize o APK gerado**
```
build/app/outputs/flutter-apk/app-release.apk
```

### Resolução de Problemas Comuns

#### Erro de Namespace
Se encontrar erro relacionado a namespace, atualize as dependências:
```bash
flutter clean
flutter pub get
flutter pub upgrade
```

#### Problemas com Java
Configure o Java 17:
```bash
flutter config --jdk-dir=/path/to/java17
```

#### Licenças do Android
Aceite todas as licenças:
```bash
flutter doctor --android-licenses
```

## 📱 Como Usar

### Primeira Configuração
1. **Instale o APK** no seu dispositivo Android
2. **Abra o aplicativo** "Roblox Auto Monitor"
3. **Configure a Webhook** do Discord (opcional mas recomendado)
4. **Conceda Permissões** quando solicitado
5. **Ative "Estatísticas de Uso"** nas configurações do Android

### Configuração de Permissões
1. Vá em **Configurações > Apps > Acesso Especial**
2. Encontre **"Estatísticas de Uso"** ou **"Usage Access"**
3. Ative a permissão para **"Roblox Auto Monitor"**

### Operação Normal
1. **Configure a Webhook** do Discord (cole a URL no campo)
2. **Ative o toggle** "Iniciar automaticamente" se desejar
3. **Clique em "Iniciar Monitoramento"**
4. **Minimize o app** - o monitoramento continuará em segundo plano
5. **Abra o Roblox** normalmente

### Webhook do Discord
Para receber notificações no Discord:
1. Crie um **servidor Discord** ou use um existente
2. Vá em **Configurações do Canal > Integrações > Webhooks**
3. **Crie uma nova webhook**
4. **Copie a URL** da webhook
5. **Cole no campo** do aplicativo

## 🔧 Arquitetura Técnica

### Componentes Principais

#### Interface de Usuário (Flutter)
- **main.dart**: Tela principal com controles e configurações
- **Material Design**: Interface moderna e intuitiva
- **Shared Preferences**: Armazenamento local de configurações

#### Serviço de Monitoramento
- **monitoring_service.dart**: Lógica principal de monitoramento
- **Foreground Service**: Execução contínua em segundo plano
- **Timer de 30 segundos**: Verificações regulares do status

#### Código Nativo Android
- **MainActivity.kt**: Integração com APIs nativas do Android
- **Method Channel**: Comunicação Flutter ↔ Android nativo
- **Usage Stats Manager**: Detecção precisa de apps em execução

### Fluxo de Operação

1. **Inicialização**: Usuário inicia o monitoramento
2. **Serviço Ativo**: Foreground service é iniciado
3. **Verificação Periódica**: A cada 30 segundos:
   - Verifica se Roblox está rodando
   - Verifica se está em primeiro plano
   - Detecta possível travamento
4. **Detecção de Problema**: Se identificado:
   - Força fechamento do Roblox
   - Aguarda 3 segundos
   - Reabre no jogo específico (ID: 17687504411)
   - Envia notificação Discord
5. **Notificação**: Atualiza status na barra de notificações

### Tecnologias Utilizadas

- **Flutter 3.24.5**: Framework principal
- **Dart**: Linguagem de programação
- **Kotlin**: Código nativo Android
- **Android SDK 34**: APIs do sistema
- **HTTP**: Comunicação com webhooks
- **Shared Preferences**: Armazenamento local
- **URL Launcher**: Abertura de deep links
- **Permission Handler**: Gerenciamento de permissões
- **Foreground Task**: Serviços em segundo plano

## 🎮 Configuração do Jogo

O aplicativo está configurado para abrir o jogo **"All Star Tower Defense X"** automaticamente:
- **ID do Jogo**: 17687504411
- **Deep Link**: `roblox://placeID=17687504411`

Para alterar o jogo, modifique a constante `robloxGameUrl` em `monitoring_service.dart`.

## 🔒 Privacidade e Segurança

### Dados Coletados
- **Status de Apps**: Apenas para detectar se Roblox está rodando
- **Configurações**: Webhook URL e preferências (armazenadas localmente)

### Dados NÃO Coletados
- Informações pessoais
- Dados de jogos
- Histórico de navegação
- Localização

### Permissões Justificadas
- **Usage Stats**: Necessária para monitorar status do Roblox
- **Foreground Service**: Para funcionamento em segundo plano
- **Internet**: Apenas para enviar webhooks Discord
- **Boot Completed**: Para auto-início opcional

## 🐛 Solução de Problemas

### Monitoramento Não Funciona
1. Verifique se a permissão "Estatísticas de Uso" está ativada
2. Confirme que o Roblox está instalado
3. Reinicie o aplicativo
4. Verifique se há otimizações de bateria ativas

### Webhook Não Envia
1. Verifique se a URL da webhook está correta
2. Teste a webhook manualmente no Discord
3. Confirme conexão com internet
4. Verifique logs do aplicativo

### Roblox Não Reabre
1. Confirme que o Roblox está instalado
2. Verifique se deep links estão habilitados
3. Teste abrir o link manualmente
4. Reinicie o dispositivo

### App Para de Funcionar
1. Desative otimizações de bateria para o app
2. Adicione à lista de apps protegidos
3. Verifique se as permissões não foram revogadas
4. Reinstale se necessário

## 📄 Licença

Este projeto é fornecido como está, para uso pessoal. O código fonte está disponível para modificação e melhoria.

## 🤝 Contribuição

Sugestões de melhorias são bem-vindas:
- Otimizações de performance
- Melhorias na detecção
- Interface mais intuitiva
- Suporte a mais jogos
- Correções de bugs

## 📞 Suporte

Para problemas técnicos:
1. Verifique a seção "Solução de Problemas"
2. Confirme que todas as permissões estão concedidas
3. Teste em dispositivo compatível (Android 7+)
4. Verifique logs do sistema se necessário

---

**Desenvolvido com Flutter** 🚀
**Compatível com Android 7+** 📱
**Sem necessidade de root** ✅


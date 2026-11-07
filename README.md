# GlicoVida AI — pacote Android de testes com notificações nativas

Este projeto empacota a interface web V5 já utilizada nos testes com Capacitor e adiciona notificações locais Android via `@capacitor/local-notifications`.

## O que esta versão implementa

- Solicitação da permissão Android de notificações.
- Canal local de notificações do GlicoVida.
- Sincronização de lembretes ativos do Supabase para os próximos 14 dias.
- Reprogramação após login, cadastro/pausa/exclusão de lembrete, adiamento e confirmação.
- Conteúdo genérico da notificação para não expor o nome do medicamento na tela bloqueada.
- Botão de teste que agenda uma notificação em aproximadamente 15 segundos.
- APK de depuração gerado em nuvem por GitHub Actions.

## Importante

- Este pacote é uma base de integração e teste; ainda não é um lançamento de produção nem um AAB assinado para a Play Store.
- A V5 web contém outros módulos demonstrativos. Não trate este pacote como produto médico pronto.
- Os lembretes não calculam doses nem substituem prescrição, orientação profissional ou alarmes clínicos.
- O app programa notificações locais futuras. O Android, permissões, modo Doze, fabricante e configurações de bateria podem afetar a entrega. Teste com tela bloqueada e após reinicialização do aparelho.
- O histórico atual de eventos da V5 mantém o estado da ocorrência por `upsert`; não é ainda um registro imutável de cada toque/alteração.
- A interface exige Project URL e Publishable key do Supabase no primeiro uso. Nunca use `service_role` nem `sb_secret_` no aplicativo.

## Requisitos de compilação

Capacitor 8 exige Node.js 22 ou superior e Android Studio/SDK atuais para desenvolvimento local. Como este projeto usa GitHub Actions, a compilação do APK de teste ocorre em um runner Linux na nuvem; não é necessário instalar Android Studio no Windows 7.

## Gerar APK de teste pelo GitHub

1. Crie um repositório novo no GitHub, por exemplo `glicovida-ai-android` (não substitua o repositório/site V4 ou V5 atual).
2. Envie todos os arquivos e pastas deste pacote para a raiz do repositório, mantendo `.github/workflows` e `src`.
3. Abra a aba **Actions** do repositório.
4. Se solicitado, habilite GitHub Actions.
5. Escolha **Build Android de teste (APK)** e clique em **Run workflow**.
6. Aguarde a compilação terminar com sucesso.
7. Abra a execução concluída e baixe o artefato **GlicoVida-AI-Android-APK-TESTE**.
8. Extraia o artefato para obter `app-debug.apk` e transfira-o para o Motorola.
9. Instale o APK de teste. Se o Android pedir, autorize temporariamente a instalação pelo app usado para abrir o arquivo.

## Primeiro teste no Motorola Edge 60 Pro / Android 16+

1. Abra o GlicoVida AI instalado.
2. Configure a URL do projeto Supabase e a Publishable key do projeto `glicovida-ai`.
3. Entre com a conta fictícia de paciente usada nos testes.
4. Abra **Medicamentos** e toque em **Ativar alertas Android neste aparelho**.
5. Permita notificações. Se o Android abrir **Alarmes e lembretes**, revise e conceda o acesso se desejar horários exatos. Se não conceder, o plugin pode usar horários aproximados; a tela informará quando houver fallback.
6. Toque em **Testar notificação em 15 segundos** e aguarde.
7. Cadastre um lembrete fictício para alguns minutos à frente e teste com o app fechado e a tela bloqueada.

## Política de alarmes exatos

O manifesto declara `SCHEDULE_EXACT_ALARM`, que exige concessão especial do usuário em versões Android compatíveis. Não declaramos `USE_EXACT_ALARM`, pois essa permissão é restrita pela política do Google Play a categorias específicas como alarmes, timers e calendários. A permissão de notificação comum `POST_NOTIFICATIONS` também é solicitada no Android 13+.

## Publicação na Google Play

O workflow desta versão gera apenas APK debug para teste. Para Play Store ainda será necessário configurar assinatura de release/keystore, gerar AAB, definir versão/target SDK, revisar política de dados de saúde, privacidade, consentimentos, conteúdo, testes em aparelhos e declarações de permissões no Play Console.

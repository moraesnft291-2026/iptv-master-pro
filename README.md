# IPTV Master Pro — versão de desenvolvimento

Projeto para disponibilizar apenas transmissões, canais e vídeos que o operador possui ou está autorizado a distribuir.

## O que existe nesta versão
- Página pública de contratação do plano de R$ 19,90 por 30 dias.
- API Node.js/Express e banco SQLite local.
- Criação de checkout avulso Mercado Pago e webhook de confirmação.
- API administrativa resumida protegida por `ADMIN_TOKEN`.
- Player web responsivo em `public/player.html`, com importação de arquivo M3U ou carregamento de URL M3U/M3U8 e reprodução HLS quando o dispositivo/navegador suporta.
- Configuração inicial de serviço no Render (`render.yaml`).

## Limitações que precisam ser resolvidas antes de vender acessos
- O checkout desta versão é cobrança avulsa de 30 dias, não assinatura com renovação automática.
- O player ainda não está ligado a uma sessão autenticada de cliente. Não publique links privados de playlist nele.
- A API `/api/me/:id` é demonstrativa, não é autenticação segura.
- O banco SQLite usa disco local. Hospedagem gratuita pode reiniciar, dormir ou apagar dados; portanto, esta configuração é somente para teste e não garante persistência para 100 clientes. Antes de produção, migrar para PostgreSQL gerenciado e implementar autenticação/sessões reais.
- O player depende de CORS no servidor da playlist. Alguns links M3U/M3U8 podem não funcionar em navegadores de Smart TV; compatibilidade depende do modelo, codec e formato.
- Não há canais ou mídia incluídos.

## Testar localmente
Requer Node.js 20+.
1. `npm install`
2. Copie `.env.example` para `.env`.
3. Para testar apenas o site e o player, `MP_ACCESS_TOKEN` pode ficar vazio.
4. `npm start`
5. Abra `http://localhost:3000` e `http://localhost:3000/player.html`.

## Publicar para teste no Render
1. Crie um repositório privado no GitHub e envie os arquivos do projeto.
2. No Render, escolha criar um novo Web Service a partir do repositório.
3. Use `npm install` como build command e `npm start` como start command (ou aplique o `render.yaml`).
4. Configure `BASE_URL` com a URL HTTPS que o Render fornecer.
5. Configure `MP_ACCESS_TOKEN`, `MP_WEBHOOK_SECRET` e `ADMIN_TOKEN` somente nos Environment Variables do Render. Nunca coloque tokens no HTML, GitHub ou mensagens.
6. Configure o webhook do Mercado Pago em `https://SUA-URL/api/webhooks/mercadopago`.

**Atenção:** serviço gratuito é para validação inicial. A combinação atual com SQLite não é adequada para operação comercial contínua nem para prometer 100 contas persistentes. Não receba pagamentos de clientes até implementar banco persistente, autenticação individual, painel de gestão completo e renovação automática devidamente testada.

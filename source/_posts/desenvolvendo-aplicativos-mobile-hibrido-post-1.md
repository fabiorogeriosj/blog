---
title: Desenvolvendo Aplicativos Mobile Híbrido post 1
date: 2017-08-10 09:32:05
header-img: /img/apps.jpg
tags:
- apache cordova
- ebook
- essencial
- iniciante
---

Esta página é destinada para dúvidas/problemas e respostas que ocorrem com os meus alunos que participaram dos treinamentos sobre desenvolvimento de aplicativos móveis híbrido e multi-plataforma utilizando Apache Cordova, Ionic e MobileUI.

Se você tiver alguma dúvida que não está listado a baixo pode deixar nos comentários que irei te ajudar e com isso vamos compartilhar a solução com a comunidade em geral.

## [01] Utilizo MacOS e ao tentar instalar o cordova e mobileui da erro

```
npm ERR! code EACCES
npm ERR! errno -13
npm ERR! syscall access

npm ERR! Error: EACCES: permission denied, access '/usr/local/lib/node_modules/cordova/node_modules/cordova-common/node_modules/bplist-parser/node_modules/big-integer'
npm ERR!     at Error (native)
npm ERR!  { Error: EACCES: permission denied, access '/usr/local/lib/node_modules/cordova/node_modules/cordova-common/node_modules/bplist-parser/node_modules/big-integer'
npm ERR!     at Error (native)
npm ERR!   errno: -13,
npm ERR!   code: 'EACCES',
npm ERR!   syscall: 'access',
npm ERR!   path: '/usr/local/lib/node_modules/cordova/node_modules/cordova-common/node_modules/bplist-parser/node_modules/big-integer' }
npm ERR! 
npm ERR! Please try running this command again as root/Administrator.

npm ERR! Please include the following file with any support request:
npm ERR!     /Users/jonathansantos/npm-debug.log
```

### Solução

Em algumas máquinas com o sistema operacional MacOS é necessário estar com root no terminal, para isso digite no seu terminal o comando: `sudo su -` em seguida digite sua senha de administrador, geralmente é a mesma senha do seu usuário principal. Isso irá resolver essa permissão.


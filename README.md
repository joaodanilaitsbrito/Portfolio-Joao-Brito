# João Pedro — Racing Portfolio

Portfólio em Vue 3 + Vite, com identidade visual inspirada no automobilismo. Projeto preparado para desenvolvimento local no VS Code e deploy no Netlify.

## Requisitos

- Node.js LTS (inclui npm)
- VS Code
- Extensão Vue - Official (Volar), recomendada

## 1. Criar o projeto manualmente (alternativa)

Se quiser recriar do zero em vez de usar esta pasta:

```bash
npm create vite@latest joao-pedro-racing-portfolio -- --template vue
cd joao-pedro-racing-portfolio
npm install
npm install gsap lucide-vue-next
npm run dev
```

## 2. Abrir e executar este projeto

1. Baixe e extraia `joao-pedro-racing-portfolio.zip`.
2. Abra a pasta extraída no VS Code: **File > Open Folder**.
3. Abra o terminal integrado: **Terminal > New Terminal**.
4. Instale as dependências:

```bash
npm install
```

5. Inicie o servidor:

```bash
npm run dev
```

6. Abra no navegador o endereço exibido pelo Vite (normalmente `http://localhost:5173`).

Para testar a versão de produção localmente:

```bash
npm run build
npm run preview
```

## 3. Estrutura de pastas

```text
joao-pedro-racing-portfolio/
├── index.html
├── package.json
├── vite.config.js
├── netlify.toml
├── .gitignore
└── src/
    ├── main.js
    ├── App.vue
    ├── style.css
    └── components/
        ├── RaceHeader.vue
        ├── HeroSection.vue
        ├── TelemetryBar.vue
        ├── ProfileSection.vue
        ├── ExperienceSection.vue
        ├── EducationSection.vue
        ├── ProjectsSection.vue
        ├── ContactSection.vue
        └── SiteFooter.vue
```

## 4. Personalização importante

- **Textos e trajetória:** edite os componentes dentro de `src/components/`.
- **Cores, tipografia e responsividade:** edite `src/style.css`.
- **Título e descrição para buscadores:** edite `index.html`.
- **Projetos reais:** substitua os três exemplos da seção Garage por projetos concluídos, com links para demonstração e repositórios.
- **Foto pessoal de kart:** a fotografia enviada foi incluída em `public/kart-joao-pedro.webp` e aparece na abertura cinematográfica. Ela é usada sem alteração; ajuste `object-position` em `src/style.css` se quiser mudar o enquadramento.
- **WhatsApp:** o link já utiliza o número informado. Confirme se o número está correto antes de publicar.

## 5. Publicar no Netlify

### Opção A — GitHub (recomendada)

1. Crie um repositório no GitHub.
2. Envie os arquivos do projeto para esse repositório.
3. No Netlify, escolha **Add new project / Import an existing project**.
4. Conecte o GitHub e selecione o repositório.
5. Configure:
   - Build command: `npm run build`
   - Publish directory: `dist`
6. Clique em Deploy.

A configuração equivalente já está no `netlify.toml`.

### Opção B — Netlify CLI

Instale a CLI e autentique:

```bash
npm install -g netlify-cli
netlify login
npm run build
netlify deploy
```

Para publicar em produção depois de conferir a prévia:

```bash
netlify deploy --prod
```

## 6. Antes de publicar

- Confira telefone, e-mail e links.
- Adicione links de projetos reais ao GitHub.
- Confira se as descrições profissionais representam exatamente suas atividades.
- Teste em celular e desktop.
- Teste navegação por teclado e contraste.
- Se adicionar fotos, use imagens próprias ou com licença de uso adequada.

## Tecnologias

- Vue 3 — componentes e interface.
- Vite — servidor de desenvolvimento e build.
- GSAP — animação de entrada.
- Lucide Vue Next — ícones.
- CSS — layout, ilustração vetorial do kart e animações responsivas.


## Atualização cinematic

A versão atual usa a fotografia pessoal enviada como imagem principal, com camadas de contraste, textura, sobreposição escura e animações GSAP de entrada e rolagem. A cor de destaque é laranja. As demais seções e os dados profissionais foram preservados.


## Versão 3.0 — interações cinematográficas

Foi adicionado o componente `CinematicEffects.vue`, que desenha partículas leves em canvas, acompanha o cursor no desktop e aplica iluminação/tilt 3D nos cartões. Os efeitos respeitam `prefers-reduced-motion` e são reduzidos em telas menores. Os textos de interface foram traduzidos para português brasileiro.


## Versão 4.0 — estilo HQ / filme "Powered by Honda"

- `ComicFX.vue`: linhas de velocidade que crescem com a velocidade do scroll, títulos que inclinam (skew) como câmera em movimento, onomatopeias (VRUUM!, BOOM!...) ao entrar em cada seção, explosão "POW!" ao clicar, transição de página em cortes diagonais ao navegar pelo menu e câmera com parallax do mouse no hero.
- `Marquee.vue`: faixas diagonais que correm conforme a rolagem.
- `comic.css`: halftone, cards com sombra dura estilo painel, botões que "derrapam" no hover, glitch no título.
- Foto convertida de PNG (2,7 MB) para WebP; imports sem uso removidos; vite movido para devDependencies.
- Tudo respeita `prefers-reduced-motion`.

## Versão 6.0 — interações estilo "site de piloto"

- `Preloader.vue`: intro com contagem 0–100% e cortina que sobe (1x por sessão; pulado com movimento reduzido).
- `Reel.vue`: trajetória (experiência + formação) em galeria horizontal controlada pela rolagem, com a foto do kart como primeiro painel. No celular vira carrossel com swipe.
- `TrackSplit.vue`: dois painéis grandes (NA PISTA / NO CÓDIGO) que expandem no hover e travam no clique, mais assinatura que se desenha.
- `ComicFX.vue`: cursor personalizado com rótulos, botões magnéticos e "segure no hero para acelerar" (linhas de velocidade + zoom).
- `components/vuebits/DecryptedText.vue` vem do [Vue Bits](https://vue-bits.dev) (MIT + Commons Clause).
- Conteúdo de F1 removido: o portfólio é só sobre você.

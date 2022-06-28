# Svelte UI Library 

## Tailwind CSS 
CSS 프레임워크이다. 부트스트랩과 비슷하게 m-1, flex와 같이 미리 세팅된 유틸리티 클래스를 활용하는 방식으로 HTML 코드 내에서 스타일링을 할 수 있다.


[Tailwind CSS의 단접](https://wonny.space/writing/dev/hello-tailwind-css)     


[tailwindcss 설치](https://tailwindcss.com/docs/installation)      


### Autoprefixer
CSS 규칙에 vendor 접두사를 추가하고 CSS를 파싱하기 위한 PostCSS.


[PostCSS plugin to parse CSS and add vendor prefixes to CSS rules using values from Can I Use. It is recommended by Google and used in Twitter and Alibaba.](https://www.npmjs.com/package/autoprefixer)

```
npm i autoprefixer
```



### postcss-load-config 

사이트는 [여기](https://github.com/postcss/postcss-load-config)    

```
npm i postcss-load-config
```


### rollup-plugin-postcss 
Rollup과 PostCSS를 통합하기 위한 라이브러리. 
```
npm i rollup-plugin-postcss
```

### svelte-preprocess 
Svelte preprocessor는 PostCSS, SCSS, Less, Stylus, CoffeeScript, TypeScript, Pug 등을 지원한다. 

[svelte-preprocess](https://github.com/sveltejs/svelte-preprocess/blob/main/docs/getting-started.md)     


```
npm install -D svelte-preprocess
```



## daisyUI 
```
npm i daisyui
npm i postcss 
npm install -D tailwindcss
npx tailwindcss init
```

샘플 프로젝트를 제공하는데 [여기](https://stackblitz.com/edit/daisyui-svelte-rollup?file=rollup.config.js)를 참고한다. 


daisUI에서 샘플로 제공하는 프로젝트의 package.json

```json
{
  "name": "svelte-app",
  "version": "1.0.0",
  "private": true,
  "scripts": {
    "dev": "rollup -c -w",
    "build": "rollup -c",
    "start": "sirv public --no-clear"
  },
  "devDependencies": {
    "@rollup/plugin-commonjs": "^17.0.0",
    "@rollup/plugin-node-resolve": "^11.0.0",
    "autoprefixer": "^10.4.4",
    "daisyui": "^2.13.2",
    "postcss": "^8.4.12",
    "postcss-load-config": "^3.1.3",
    "rollup": "^2.3.4",
    "rollup-plugin-livereload": "^2.0.0",
    "rollup-plugin-postcss": "^4.0.1",
    "rollup-plugin-svelte": "^7.0.0",
    "rollup-plugin-terser": "^7.0.0",
    "svelte": "^3.0.0",
    "svelte-preprocess": "^4.7.3",
    "tailwindcss": "^3.0.23"
  },
  "dependencies": {
    "sirv-cli": "^1.0.0"
  }
}
```

샘플로 제공하는 postcss.config.js

```jsx
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

샘플로 제공하는 tailwind.config.js
```jsx
module.exports = {
  content: ['./public/index.html', './src/**/*.svelte'],
  plugins: [require('daisyui')],
};
```



샘플로 제공하는 rollup.config.js 
```jsx
import svelte from "rollup-plugin-svelte";
import commonjs from "@rollup/plugin-commonjs";
import resolve from "@rollup/plugin-node-resolve";
import livereload from "rollup-plugin-livereload";
import { terser } from "rollup-plugin-terser";
import postcss from "rollup-plugin-postcss";
import sveltePreprocess from "svelte-preprocess";

const production = !process.env.ROLLUP_WATCH;

function serve() {
  let server;

  function toExit() {
    if (server) server.kill(0);
  }

  return {
    writeBundle() {
      if (server) return;
      server = require("child_process").spawn(
        "npm",
        ["run", "start", "--", "--dev"],
        {
          stdio: ["ignore", "inherit", "inherit"],
          shell: true,
        }
      );

      process.on("SIGTERM", toExit);
      process.on("exit", toExit);
    },
  };
}

export default {
  input: "src/main.js",
  output: {
    sourcemap: true,
    format: "iife",
    name: "app",
    file: "public/build/bundle.js",
  },
  plugins: [
    svelte({
      compilerOptions: {
        // enable run-time checks when not in production
        dev: !production,
      },
      preprocess: sveltePreprocess({
        sourceMap: !production,
      }),
    }),
    // we'll extract any component CSS out into
    // a separate file - better for performance
    postcss({
      plugins: [],
    }),

    // If you have external dependencies installed from
    // npm, you'll most likely need these plugins. In
    // some cases you'll need additional configuration -
    // consult the documentation for details:
    // https://github.com/rollup/plugins/tree/master/packages/commonjs
    resolve({
      browser: true,
      dedupe: ["svelte"],
    }),
    commonjs(),

    // In dev mode, call `npm run start` once
    // the bundle has been generated
    !production && serve(),

    // Watch the `public` directory and refresh the
    // browser on changes when not in production
    !production && livereload("public"),

    // If we're building for production (npm run build
    // instead of npm run dev), minify
    production && terser(),
  ],
  watch: {
    clearScreen: false,
  },
};

```



### css 파일 생성

src/ 아래에 style.css 파일 생성하고 다음과 같이 입력한다. 
```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

### css import
main.js에서 style.css를 import 한다. 
```jsx
import App from './App.svelte';
import "./style.css";
```

### 버튼 사용 
App.svelte에서 button을 사용한다. 
```
  <button class="btn">Hello daisyUI</button>
```



digit로 생성한 프로젝트에 daisyUI를 설정하기 위해서 rollup.config.js를 수정했다. 아래 파일을 참고로 해서 각 프로젝트에 맞게 수정한다. 

```jsx
import svelte from 'rollup-plugin-svelte';
import commonjs from '@rollup/plugin-commonjs';
import resolve from '@rollup/plugin-node-resolve';
import livereload from 'rollup-plugin-livereload';
import { terser } from 'rollup-plugin-terser';
import css from 'rollup-plugin-css-only';
import alias from '@rollup/plugin-alias';
import path from 'path';

// daisUI 사용하기 위해 추가한 것
import postcss from "rollup-plugin-postcss";
import sveltePreprocess from "svelte-preprocess";


const production = !process.env.ROLLUP_WATCH;

function serve() {
    let server;

    function toExit() {
        if (server) server.kill(0);
    }

    return {
        writeBundle() {
            if (server) return;
            server = require('child_process').spawn('npm', ['run', 'start', '--', '--dev'], {
                stdio: ['ignore', 'inherit', 'inherit'],
                shell: true,
            });

            process.on('SIGTERM', toExit);
            process.on('exit', toExit);
        },
    };
}

export default {
    input: 'src/main.js',
    output: {
        sourcemap: true,
        format: 'iife',
        name: 'app',
        file: 'public/build/bundle.js',
    },
    plugins: [
        svelte({
            compilerOptions: {
                // enable run-time checks when not in production
                dev: !production,
            },
            // daisUI 사용하기 위해 추가한 것
            preprocess: sveltePreprocess({
                sourceMap: !production,
            }),
        }),
		// daisUI 사용하기 위해 추가한 것
        // we'll extract any component CSS out into
        // a separate file - better for performance
        postcss({
            plugins: [],
        }),
        // we'll extract any component CSS out into
        // a separate file - better for performance
		// daisUI 사용하기 위해 주석처리한 것 
        //css({ output: 'bundle.css' }),
        // 절대경로 alias 추가하기
        // 아래와 같이 설정시 @/components는 /src/components 경로로 실행
        alias({
            entries: [
                {
                    find: '@',
                    replacement: path.resolve(__dirname, 'src/'),
                },
                {
                    find: '~',
                    replacement: path.resolve(__dirname, 'static/'),
                },
            ],
        }),

        // If you have external dependencies installed from
        // npm, you'll most likely need these plugins. In
        // some cases you'll need additional configuration -
        // consult the documentation for details:
        // https://github.com/rollup/plugins/tree/master/packages/commonjs
        resolve({
            browser: true,
            dedupe: ['svelte'],
        }),
        commonjs(),

        // In dev mode, call `npm run start` once
        // the bundle has been generated
        !production && serve(),

        // Watch the `public` directory and refresh the
        // browser on changes when not in production
        !production && livereload('public'),

        // If we're building for production (npm run build
        // instead of npm run dev), minify
        production && terser(),
    ],
    watch: {
        clearScreen: false,
    },
};

```



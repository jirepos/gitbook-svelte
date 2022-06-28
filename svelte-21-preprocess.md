# svelte-preprocess 
[sveltejs/svelte-preprocess](https://github.com/sveltejs/svelte-preprocess/blob/main/docs/getting-started.md)을 참고한다.


## 설치
```
npm install -D svelte-preprocess
```

svelte-preprocess언어별 종속성이 없으므로 사용할 나머지 도구를 설치하는 것은 우리에게 달려 있다.


* TypeScript: npm install -D typescript
* PostCSS: npm install -D postcss postcss-load-config
* 등등 



rollup.config.js에 다음을 추가한다. 
```jsx
 svelte({
      preprocess: sveltePreprocess({ /* options */ })
 })
```


스크립트를 TypeScript로, 스타일을 Sass로 작성한다고 가정해 보겠다. 또한 스타일에 자동 접두사가 붙기를 원하므로 PostCSS도 필요합니다. 다음 종속성을 설치해 보겠다. 

**주의**: svelte-preprocess는 svelte-loader, rollup-plugin-svelte , 그리고 유사한 도구에 의해 전달된 의해 전달된 콘텐츠만 처리한다. TypeScript 구성 요소가 TypeScript 파일을 가져오면 번들러가 해당 파일을 처리합니다


```
npm i -D typescript sass postcss autoprefixer  @rollup/plugin-typescript
```

rollup.config.js 파일을 다음과 같이 구성한다. 

```jsx
import svelte from 'rollup-plugin-svelte'
import sveltePreprocess from 'svelte-preprocess';
+ import typescript from '@rollup/plugin-typescript';

const production = !process.env.ROLLUP_WATCH

export default {
  input: 'src/main.js',
  output: {
    sourcemap: true,
    format: 'iife',
    name: 'app',
    file: 'public/bundle.js',
  },
  plugins: [
+    // teach rollup how to handle typescript imports
+    typescript({ sourceMap: !production }),
    svelte({
+      preprocess: sveltePreprocess({
+         sourceMap: !production,
+         postcss: {
+           plugins: [require('autoprefixer')()]
+         }
+      }),
      // enable run-time checks when not in production
      dev: !production,
      // we'll extract any component CSS out into
      // a separate file — better for performance
      css: css => {
        css.write('public/bundle.css')
      },
    }),
  ],
}
```


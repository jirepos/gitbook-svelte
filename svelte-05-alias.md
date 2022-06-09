# Svelte에서 절대 경로 사용

파일의 위치 이동시 상대 경로변경이 어렵기 때문에 절대 경로를 사용하기 위해 rollup pluing alias를 사용한다. alais를 사용하는 방법을 살펴본다. 



## @rollup/plugin-alias 설치
```
npm install @rollup/plugin-alias
```

## rollup.config.js 수정
```jsx
import alias from "@rollup/plugin-alias";
import path from "path";

export default {

	plugins: [
		svelte({
			compilerOptions: {
				// enable run-time checks when not in production
				dev: !production
			}
		}),
		// we'll extract any component CSS out into
		// a separate file - better for performance
		css({ output: 'bundle.css' }),
		// 절대경로 alias 추가하기
    // 아래와 같이 설정시 @/components는 /src/components 경로로 실행
    alias({
      entries: [
        {
          find: "@",
          replacement: path.resolve(__dirname, "src/")
        }
      ]
    }),
};

```    
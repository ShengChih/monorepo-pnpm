# init
```bash
mkdir monorepo-example
cd monorepo-example
pnpm init

touch pnpm-workspace.yaml
echo "shamefully-hoist=true" >> .npmrc
```

# multiple-project
```text
monorepo-example
├── package.json
|__ app
|   ├── (A) next-sample
|   ├── (B) nuxt-sample
├── package
│   ├── (1) animation
│   │   ├── package.json
|   |   └── pnpm-lock.yaml
│   ├── (2) data
│   │   ├── package.json
|   |   └── pnpm-lock.yaml
|   ├── (3) react
│   │   ├── package.json
|   |   └── pnpm-lock.yaml
│   └── (4) lib
|       ├── index.js
|       ├── utils
|       |   └── validator.js
│       ├── package.json
|       └── pnpm-lock.yaml
├── pnpm-lock.yaml
└── pnpm-workspace.yaml

Monorepo 目的在於想要一個 repo 管理多個 project，需求來自於各個 project 有自己的 package.json 而依賴在不同時期寫，版本皆不相同，但維護成本就會相對變高。而各個專案寫的共用 lib 只能互相複製來複製去。

優點是可以讓專案的依賴版本切齊，共用 lib 可互相使用
缺點在於專案很多，build 時間變長，依賴版本更新就需要 CI/CD 完整功能測試，不同團隊容易對其他專案更動。

感覺 pnpm workspace + git submodule 應該可以簡單管理。
用 gitlab 對專案設定 submodule 權限，且也擁有自己的 pnpm-workspace.yaml

甲團隊若使用 vue/nuxt 生態搭配 package 選用 (1) + (4):
> pnpm-workspace.yaml 包含 (B)/(1)/(4) 路徑 (git submodule init + git submodule update -r + pnpm i)

乙團隊若使用 react/next 生態 package 選用 (A) (3) (4) 同上應該就可以了
```

# command
```
pnpm -F {target} add {third-party or other package}

e.g. 對 app/nuxt-sample 專案加入 package/lib 依賴
pnpm -F app/nuxt-sample add package/lib

pnpm -F app/next-sample add three
```
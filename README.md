# wayne's hugo blog

## terminal 先進到 quickstart

## 啟動 hugo serve  

> 本地啟動網站  

```shell
hugo serve -D
```

---

## 新增文章(預設中文)  

> 文章將建立在 `/content/post/`  
> `index.md` 會自動歸類為 `中文` 文章  
> `index.en.md` 會自動歸類為 `英文` 文章  

```shell
hugo new post/{postName}.md
```

---

## 修改文章  

> 文章位置： `/content/post/`  
> 使用 `markdown` 格式撰寫  

---

## Build 網站  

> 建立網站並建置到 `/public/`  
> 將 `public` commit、push 即可更新 Blog  

```shell
hugo
```

---

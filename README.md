# 食譜分享連結的網頁

這個資料夾是放在 GitHub Pages 上的靜態網站，給食譜分享連結用（規格：`.scratch/recipe-share-link/spec.md`，決策：`docs/adr/0011-share-link-security-model.md`）。

| 檔案 | 用途 |
|---|---|
| `r/index.html` | 沒裝 App、或聊天 App 內建瀏覽器沒有直接開 App 時看到的頁面。**不顯示食譜內容**，只有「安裝 App」與「用 App 開啟」兩顆按鈕；後者把 `#` 後面的資料原封不動交給 `mealprep://r#…` |
| `.well-known/apple-app-site-association` | Universal Link 用（點連結直接開 App、不經過網頁）。**目前沒有付費的 Apple 開發者帳號，這個檔案暫時用不到**，放著不影響；之後有了再把 `TEAM_ID` 換成自己的 Team ID |
| `.nojekyll` | 讓 GitHub Pages 不要忽略 `.well-known` 這個以點開頭的資料夾 |

## 部署

`apple-app-site-association` 必須在**網域根目錄**的 `/.well-known/` 下，所以要放在 `<帳號>.github.io` 這個「使用者網站」repo，不能放在一般專案的 Pages（那會在 `/<repo名稱>/` 底下）。

1. 在 GitHub 建立名為 `<帳號>.github.io` 的 public repo。
2. 把這個資料夾裡的所有檔案（含 `.nojekyll`、`.well-known/`）複製到那個 repo 的根目錄並 push。
3. repo 的 Settings → Pages，來源選 main branch 的根目錄。
4. 幾分鐘後打開 `https://<帳號>.github.io/r/#1abc` 應該看到這一頁、按鈕是灰的（因為 `1abc` 不是有效的食譜資料）。
5. App 裡的網址設定在 `SharedRecipeLink.webHost`（目前是 `awecefil.github.io`）。

## 自動開啟 App（Universal Link）

上面做完，分享出去的就是一般的 https 連結，在 LINE、IG 裡點得動，會打開這一頁，按「用 App 開啟」進 App。

要做到「點了直接開 App、不經過這一頁」，還需要：

- **付費的 Apple Developer Program**（免費的個人帳號不能使用 Associated Domains）。
- 把 `apple-app-site-association` 裡的 `TEAM_ID` 換成自己的 Team ID。
- App 加上 Associated Domains 權限：`applinks:<帳號>.github.io`。

## 之後換網域

連結裡寫死了網址，換網域後舊連結會失效（除非舊網址的網頁也繼續留著）。

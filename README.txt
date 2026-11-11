AssetFinder V6.1.3｜使用說明

啟動
- 開發環境：安裝 requirements.txt 後執行 start.bat。
- 產生 EXE：執行 build_exe.bat（會自行安裝 PyInstaller）。
- 發給同事：AssetFinder.exe + remote_admin_config.json（同一資料夾）。
- 資料庫會自動建立於 %LOCALAPPDATA%\AssetFinderV6\assets_v6.sqlite3，不需要另外放置空白資料庫。

一般使用者
- 未授權或離線時直接進入使用者模式。
- 「授權資訊」→「複製授權欄位 A、B」；貼到 Google Sheets「授權」工作表並勾選 C 欄。
- 介面提供財產、增加單與附件查詢；空格為多條件 AND。排序使用下拉選單，勿點欄位標題排序。

管理者入口
- 遠端電腦：在 remote_admin_config.json 的 apps_script_url 填入已部署的 Apps Script /exec 網址；核准後才會顯示管理者入口。登入仍需雲端密碼。離線或未授權均為使用者模式。
- 機主電腦：使用另行保管的 owner_setup.py 完成本機綁定，離線仍有雙入口；管理者登入使用本機密碼。
- 遠端管理者每 60 秒檢查一次授權；無法確認時停止管理者模式並關閉程式。
- 兩台電腦的 SQLite 不會同步；學校 Excel 可以重匯，但在本機新增的附件、關聯、備註及歷程不在學校 Excel 內，請定期備份。

Google Sheets / Apps Script 首次設定
1. 建立私人 Google 試算表：「授權」工作表 A=裝置摘要、B=Windows SID、C=啟用（核取方塊）、D=備註。
2. 「設定」工作表 B2 暫存欲設定的雲端密碼（至少 8 字元），B3 存放驗證值。
3. 由試算表「擴充功能 → Apps Script」開啟專案，貼入 GoogleAppsScript_授權服務.gs。
4. 執行 setCloudPasswordFromSheet，確認 B2 清空、B3 有驗證值。
5. 部署網頁應用程式：「以我身分執行」「任何人可存取」，將 /exec 網址填入 remote_admin_config.json。
6. 將同事的「授權資訊」複製到工作表 A、B 欄，勾選 C 欄；重新啟動 AssetFinder。
7. Apps Script 修改後須部署新版本；Google Sheets 勾選狀態變更不需重新部署。

檔案用途
- app.py：Qt 主程式；store.py、database.py：SQLite 與關聯規則；excel_reader.py：Excel 清冊匯入。
- query_filters.py：查詢篩選；ui_common.py：共用介面。
- remote_admin.py、actor_identity.py、admin_security.py：遠端授權／Windows 識別／本機密碼。
- backup_manager.py：備份還原；GoogleAppsScript_授權服務.gs：Google 授權服務端程式。
- remote_admin_config.json：遠端服務網址；build_exe.bat、start.bat：打包、啟動。

限制
- 管理者入口控管不能代替 Windows 檔案權限或 SQLite 加密；有本機資料庫存取權的人仍可直接改資料。
- Google 授權無法連線時，一般電腦不顯示管理入口。
- 變更紀錄刪除不會還原資料；還原完整備份會覆蓋當前資料庫。
- Windows GUI、DPAPI、EXE 使用需在正式使用的電腦上測試。

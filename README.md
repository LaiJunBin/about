# 關於我

### 目前就讀於 臺北商業大學 資訊管理系

興趣是寫程式

---

## 部分 Repository 簡介

* [Python](https://github.com/xyz607xx/python) => 包含解題、應用程式、爬蟲、基本繪圖
* [VB_Subject](https://github.com/xyz607xx/VB_Subject) => 包含資料結構、設計模式(DesignPattern)、解題(Tree and Graph)、應用程式及Tree類別庫(BST、VebTree)
* [WebPage_Subject](https://github.com/xyz607xx/WebPage_Subject) => HTML5、CSS3、JS(jQuery)、PHP 基礎網頁的教學或範例(如LocalStorage、PDO、Ajax、萬年曆等等...)
* [laiPHP](https://github.com/laijunbin/laiPHP) => 自己寫的MVC框架。
* [TypeScript](https://github.com/xyz607xx/TypeScript) => ts的練習，目前包含了一些Trees資料結構的模組，
* [WebExercise](https://github.com/xyz607xx/WebExercise) => 全國技能競賽(網頁設計) 的練習檔案，包含了一些JS事件處理、遊戲、基礎小畫家、拖放API、RestfulAPI以及繪製圖表...
* [dataStructImage](https://github.com/xyz607xx/dataStructImage) => 包含了一些以R(語言)繪製的資料結構圖形

---

## 開發系統或應用程式
> 僅列出主要
* [japanStudyResultsReport](https://github.com/xyz607xx/japanStudyResultsReport) => 新北市106金手獎赴日技職研習成果網站
* [library_lend](https://github.com/xyz607xx/library_lend) => 穀保家商圖書館借用系統
* [todolist](https://github.com/xyz607xx/todolist) => 待辦事項清單(個人使用)

---

## 經歷
* 第48屆全國技能競賽(網頁設計) 金牌
* 106年全國技藝競賽(程式設計) 金手獎
* 2018-02-04 APCS大學程式設計先修檢測 觀念題 5級分 實作題 4級分
* 全國高級中等學校專業群科106年專題及創意製作競賽 電腦應用類 第三名

---

## 專業部分
### 主要使用的程式語言
VB2010、Python3、HTML5、CSS3、JavaScript ES6、TypeScript、PHP7
### 使用的函式庫(Library)
jQuery、jQuery UI、Bootstrap、AngularJS
### 使用的框架(Framework)
Laravel5、Ionic3 + Angular2、Node.js Express
### 套件管理
npm、bower、pip、composer、ionic+cordova
### 使用的資料庫
MySQL、MongoDB、FireBase

---

## VB DEMO

### 你也許不認識的VB(書上至今未出現的內容)
一行取得全排列：
```vb
Function permutationsByEnum(ByVal data As List(Of String), Optional ByVal current As String = "") As IEnumerable(Of String)
    Return If(data.Count = 0, {current}.ToList, data.SelectMany(Function(x As String) permutationsByEnum(data.Except({x}).ToList, current & x)))
End Function
```

實現匿名遞迴：

```vb
Dim Factorial As Integer = (Function(f) (Function(h) Function(x) f.Invoke(h.Invoke(h)).Invoke(x))(Function(h) Function(x) f.Invoke(h.Invoke(h)).Invoke(x)))(Function(self) Function(n) If(n = 0, 1, n * self.Invoke(n - 1)))(10)  '3628800
```
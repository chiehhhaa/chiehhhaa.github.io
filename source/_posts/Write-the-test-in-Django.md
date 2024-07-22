---
title: 📄Write the test in Django (Django 內建測試框架)
date: 2024-07-22 18:13:31
code_block_shrink:  false
categories: [Python, Django]
tags: [Django, Test]
---
嗨，這邊要來記錄我嘗試寫測試的過程🙌🏻 <br/>
使用之前協同實作的 Django 專案 － Diswork。

讓我們繼續看下去👀 ...
<!-- more -->

## 關於 Django test 模組框架
Django 內建的 test 是基於 Python 的 unittest 模組的擴展。

雙手附上 Django testing 的官方文件內容 -> [點我](https://docs.djangoproject.com/en/5.0/topics/testing/overview/#running-tests)

![tests](https://github.com/chiehhhaa/picx-images-hosting/raw/master/tests.7ljw11bw2k.webp)

Django 很貼心在你建立一個新的應用程式時，會在裡面生成一個名為 `tests.py` 的檔案，而當然的在這個檔案中就是我們用來寫測試的地方囉！

## 測試の函數方法

### setUp
在每個測試方法運行之前，setUp 方法都會被調用，確保每次測試都在相同的初始條件下運行。

**setUp** 有哪些作用呢？
1. 初始化測試數據：創建測試所需的數據，如模型實例、外鍵關係等。
2. 設置測試條件：設置測試所需的環境變量、配置等。
3. 重置狀態：確保每個測試開始前，數據和狀態重置為預期的初始狀態。

以 Diswork 中的 articles 應用程式為例：
#### Step 1
```python=
from django.test import TestCase
from .models import Article, LikeArticle
from comments.models import Comment
from django.utils import timezone

class ArticleManagerTestCase(TestCase):
    def setUp(self):
        self.article1 = Article.objects.create(title="Article 1", content="Content 1", deleted_at=None)
        self.article2 = Article.objects.create(title="Article 2", content="Content 2", deleted_at=timezone.now())
        self.article3 = Article.objects.create(title="Article 3", content="Content 3", deleted_at=None)

        LikeArticle.objects.create(article=self.article1, user_id=1)
        LikeArticle.objects.create(article=self.article1, user_id=2)
        LikeArticle.objects.create(article=self.article3, user_id=1)

        Comment.objects.create(article=self.article1, user_id=1, content="Content 1")
        Comment.objects.create(article=self.article3, user_id=2, content="Content 2")
        Comment.objects.create(article=self.article3, user_id=3, content="Content 3", deleted_at=timezone.now())
```

設定初始化測試的環境，在上面這段程式碼中初始化了哪些地方？

1. 建立了三筆文章資料，其中 `article 2` 被設定為軟刪除（`deleted_at`）。
2. 為 `article 1` 和 `article 3` 分別建立了喜愛紀錄（`LikeArticle`），`article 1` 被用戶 1 和用戶 2 喜愛，`article 3` 被用戶 1 喜愛。
3. 在 `article 1` 和 `article 3` 底下分別建立了留言（`Comment`），其中一條 `article 3` 的留言被設定為軟刪除（`deleted_at`）。

#### Step 2
定義需要被測試的方法內容
```python=
def test_get_queryset(self):
        queryset = Article.objects.all()
        
        self.assertEqual(queryset.count(), 2) 
        # 預期包含2筆文章，因為有1筆已經被軟刪除。
        
        self.assertNotIn(self.article2, queryset) 
        # 確認 self.article2 不在 queryset 中，因為它已經被軟刪除。
```
```python=
def test_with_count(self):
        queryset = Article.objects.with_count()
        
        article1 = queryset.get(id=self.article1.id)
        article3 = queryset.get(id=self.article3.id)
        # 從查詢集( queryset )中取得特定的文章

        self.assertEqual(article1.like_count, 2) # 預期文章1會有2個喜歡
        self.assertEqual(article3.like_count, 1) # 預期文章3會有1個喜歡

        self.assertEqual(article1.comment_count, 1) # 預期文章1會有1個留言 
        self.assertEqual(article3.comment_count, 1) # 預期文章3會有1個喜歡 (有一個留言已經被軟刪除)
```

### Step 3

都定義好測試的方法後，就可以開始執行測試囉！

可以在執行 python test 命令時，指定一個或多個測試標籤：

`1. python manage.py test articles.tests`
> 執行所有在 articles 模組中的所有測試

`2. python manage.py test articles`
> 執行 articles 中找到的所有測試

`3. python manage.py test articles.tests.ArticleManagerTestCase`
> 單執行一個類別測試

`4. python manage.py test articles.tests.ArticleManagerTestCase.test_with_count`
> 單執行一個方法測試


好啦以上就是紀錄我在 Django 中的測試內容✨<br/>
每一個專案需要的測試邏輯都不同，就要依照你想要的邏輯去寫囉！

老話一句，感謝大大們看到這邊🥹 <br/>
如果有內容錯誤需要更正的也麻煩大神們提點🫡




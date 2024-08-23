---
title: 👩🏻‍💻Journey of the Streanlit Learning Record-2
date: 2024-08-23 09:31:52
code_block_shrink:  false
categories: [Python, Streamlit]
tags: [Streanlit]
---

## Magic Commands
Use on Pyhon 3.

You don't need to write `st.write`. You can just leave the variable value on its own line. Streamlit will automatically print it on the web page!

<!-- more -->

## Display Text, Data and Pictures
**1. Text**
```python=
st.markdown("*Streamlit* is **really** ***cool***.")
st.header("_Streamlit_ is :blue[cool] :sunglasses:")
st.subheader("_Streamlit_ is :blue[cool] :sunglasses:")
```
It will be like:

![截圖-2024-08-23-09](https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-23-09.36.06.wihlhtxog.webp)

You can use a lot of methods to print the string. And you can write the markdown directly!!

```python=
st.caption(":gray[用於標注]")
```
![截圖-2024-08-23-09](https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-23-09.36.21.1023j7n0e7.webp)

```python=
code = '''def hello():
    print("Hello, Streamlit!")'''

st.code(code, language="python")
```
![截圖-2024-08-23-09](https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-23-09.36.28.5mnqjwlk1q.webp)

**2. Data**
```python=
import streamlit as st
import pandas as pd
import random

df = pd.DataFrame(np.random.randn(10, 10), columns=("col %d" % i for i in range(10)))
st.dataframe(df)  # Same as st.write(df)

df = pd.DataFrame(np.random.randn(10, 10), columns=("col %d" % i for i in range(10)))
st.dataframe(df.style.highlight_max(axis=0))
# To `highlight` the maximum value in each column of a DataFrame.
# `axis` means that the operation is performed along the `columns`. 

```

It will be like:
![截圖-2024-08-23-09](https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-23-09.45.03.8l00nf43nc.webp)


`st.dataframe` can automatically create the data table on the web page!! You can use it with pandas or Numpy...

```python=
df = pd.DataFrame(
    {
        "name": ["Roadmap", "Extras", "Issues"],
        "url": ["https://roadmap.streamlit.app", "https://extras.streamlit.app", "https://issues.streamlit.app"],
        "stars": [random.randint(0, 1000) for _ in range(3)],
        "views_history": [[random.randint(0, 5000) for _ in range(30)] for _ in range(3)],
    } # Must use dict to write!
)
# ⬆️ Create a dataframe and give to the variable value name df.

st.dataframe(
    df,
    column_config={
        "name": "App name",
        "stars": st.column_config.NumberColumn(
            "Github Stars",
            help="Number of stars on GitHub",
            format="%d ⭐",
        ),
        "url": st.column_config.LinkColumn("App URL"),
        "views_history": st.column_config.LineChartColumn(
            "Views (past 30 days)", y_min=0, y_max=5000
        ),
    },
    hide_index=True, use_container_width=True
)
# ⬆️ Use Streamlit to display the DataFrame and configure the way it is displayed.
```
It will be like:
![截圖-2024-08-23-09](https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-23-09.36.46.2obggedakb.webp)

### st.dataframe VS st.table

- st.dataframe(df)：用於顯示可互動的表格，適合大數據集和需要自定義樣式的場景。
- st.table(df)：用於顯示靜態表格，適合小數據集和不需要互動的場景。

**可互動：**
    1. 滾動
    垂直和水平滾動：當表格數據超過顯示區域時，使用者可以滾動查看所有數據。
    2. 排序
    按列排序：使用者可以點擊表格列標題來按該列排序數據。這讓使用者可以快速找到最大或最小值，或者根據其他列的數據排序。
    3. 搜尋和過濾
    搜尋功能：表格上方會有搜尋框，使用者可以輸入關鍵字來篩選表格中的數據。
    4. 列寬調整
    調整列寬：使用者可以手動調整列的寬度，以適應不同長度的內容。
    5. 表格自適應
    自動調整大小：st.dataframe 可以根據容器大小自動調整表格的顯示，使其適應不同的顯示屏和窗口大小。
    6. 高亮顯示和樣式
    樣式支持：通過 df.style，你可以在 st.dataframe 中使用 Pandas 的樣式功能來高亮顯示特定數據（例如最大值、最小值等）。

3. Temperature -> st.matric
- First Way to display
```pytho=
st.metric(label="Temperature", value="30 °C", delta="1.2 °C")
```
- Second Way to display
```python=
col1, col2, col3 = st.columns(3)
col1.metric("溫度", "30 °C", "1.2°C")
col2.metric("風力", "9mpd", "-8%")
col3.metric("濕度", "60%", "0%")
```
It will be like:
![截圖-2024-08-23-09](https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-23-09.36.53.2obggedak9.webp)

4. st.json
```python!
data = {
    '姓名': '王小明',
    '年齡': 30,
    '地址': '台北市',
    '學歷': {
        '學士學位': '資訊科學',
        '碩士學位': '資訊管理',
    },
    '興趣': [
        '運動',
        '讀書',
        '旅遊',
    ],
}

st.json(data)
```

It will be like:
![截圖-2024-08-23-09](https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-23-09.37.01.1hs57sodyw.webp)




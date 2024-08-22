---
title: 👩🏻‍💻Journey of the `Streanlit` Learning Record-1
date: 2024-08-22 11:03:06
code_block_shrink:  false
categories: [Python]
tags: [Streanlit]
---
# About Streamlit
Streamlit is a Python framework for data scientists and AI/ML engineers to deliver dynamic data apps with only a few lines of code. Don't need to weite the front-end code! Just use Python then you can build the page!
<!-- more -->

# Install
```shell!
$ pip install streamlit
```

Before install streamlit, you can make a virtual environment.( use venv )

### Run your own file
```shell!
$ streamlit run yourapp.py
```

### streamlit hello
You can see 4 cool demo from streamlit Official website when you enter command:
```shell!
$ streamlit hello
```

![截圖 2024-08-22 10.30.47](https://hackmd.io/_uploads/Hk8WXXNiR.png)
![截圖 2024-08-22 10.31.12](https://hackmd.io/_uploads/Hy8WmmVsR.png)
![截圖 2024-08-22 10.31.03](https://hackmd.io/_uploads/SJwbXX4oR.png)
![截圖 2024-08-22 10.30.52](https://hackmd.io/_uploads/rkUbQXViC.png)


If you read the code of this 4-demo, you will notice that the `@st.cache` is in front of the function!

### What is `@st.cache` ?
> `@st.cache` will change the download data to cache, and make the app run faster!

### st.write
st.write can print any kind of data. Including dataframe、matplotlib/altair/ploty,emoji... etc...


### Hello Streamlit!

Let's make a Hello Streamlit Page!

```python!
import streamlit as st

st.title("My first Streamlit Demo!!")
st.write("Hello, Streamlit!😎")
```
First of all, import streamlit as its name to st.

Then you can use `st.title` and `st.write` to write the things you want to print on the web page.

They will look like:

![截圖 2024-08-22 10.46.57](https://hackmd.io/_uploads/SkCj8mVsR.png)

This completes my first Hello Streamlit!!✨✨

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

https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-22-10.30.47.3gobwsm7sa.webp
https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-22-10.30.52.4jo17oi1ny.webp
https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-22-10.31.03.839yxhkrg6.webp
https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-22-10.31.12.60u69fm6ex.webp

If you read the code of this 4-demo, you will notice that the `@st.cache` is in front of the function!

### What is @st.cache ?
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

https://chiehhhaa.github.io/picx-images-hosting/截圖-2024-08-22-11.07.39.4jo17oilrl.webp

This completes my first Hello Streamlit!!✨✨

## 需求

记住用户名，下次用户打开浏览器，就在文本框类里面自动显示上次登录的用户名

## 思路分析

1. 把数据存起来，用到本地存储。
2. 关闭页面，也可以显示用户名，所以用到`localStorage`。
3. 打开页面，先判断是否有这个用户名，如果有，就在表单里显示用户名，并且勾选复选框。
4. 当复选框发生改变的时候`change`事件
5. 如果勾选，就存储，否则就移除

## 代码示例

```html
<body>
    <input type="text" id="username"> <input type="checkbox" name="" id="remember"> 记住用户名
    <script>
        var username = document.querySelector('#username');
        var remember = document.querySelector('#remember');
        if (localStorage.getItem('username')) {
            username.value = localStorage.getItem('username');
            remember.checked = true;
        }
        remember.addEventListener('change', function() {
            if (this.checked) {
                localStorage.setItem('username', username.value);
            } else {
                localStorage.removeItem('username');
            }
        })
    </script>
</body>
```






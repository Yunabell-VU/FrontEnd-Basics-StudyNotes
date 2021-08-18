**注意：本章调用的ajax函数采用[Ajax封装]章节中的Ajax封装函数。**

## 搜索提示

搜索框中输入关键字后，搜索框下方会有下拉文本提示。



```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>搜索提示</title>
  </head>
  <body>
    <input id="search" type="text" />
    <ul id="result"></ul>

    <script type="module">
        //搜索提示功能只需要getJSON功能就足够
      import { getJSON } from './ajax/index.js';

      const searchInput = document.getElementById('search');
      const resultList = document.getElementById('result');

      const url = 'https://www.imooc.com/api/http/search/suggest?words=';

      const handleInputEvent = () => {
          //trim避免输入框中只有空格
        if (searchInput.value.trim() !== '') {
          getJSON(`${url}${searchInput.value}`)
            .then(response => {
              console.log(response);
              // [{word: "jsp"}]

              let html = '';
              for (const item of response.data) {
                html += `<li>${item.word}</li>`;
              }

              resultList.innerHTML = html;

              resultList.style.display = '';

              // resultList.innerHTML = '<li>jsp</li><li>js</li>';
            })
            .catch(err => {
              console.log(err);
            });
        } else {
          resultList.innerHTML = '';
          resultList.style.display = 'none';
        }
      };

      let timer = null;
      // IE9 开始支持
      searchInput.addEventListener(
          //'input'事件 输入框内有输入变动就会触发
        'input',
        () => {
          // handleInputEvent();

          if (timer) {
            clearTimeout(timer); //如果定时器在走的时候就输入了新内容，则重置定时器
          }
          // jsa
          timer = setTimeout(handleInputEvent, 500); //避免请求一直发送，隔一会再发送一次请求
        },
        false
      );
    </script>
  </body>
</html>

```

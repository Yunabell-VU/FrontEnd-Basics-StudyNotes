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



****

## 二级菜单

一级菜单的请求自动发送，发送之后，才会处理二级菜单的请求

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <title>二级菜单</title>
    <style>
      /* css reset */
      * {
        padding: 0;
        margin: 0;
      }
      li {
        list-style: none;
      }

      /* menu */
      .menu {
        width: 100px;
        background-color: rgba(0, 0, 0, 0.1);
        margin: 10px;
      }
      .menu-item {
        position: relative;
        padding: 5px;
        cursor: pointer;
      }
      .menu-content {
        display: none;
        position: absolute;
        left: 100%;
        top: 0;
        width: 200px;
        height: 100px;
        padding: 0 5px;
        background-color: rgba(0, 0, 0, 0.1);
      }
      .menu-item:hover {
        background-color: rgba(0, 0, 0, 0.4);
      }
      .menu-item:hover .menu-content {
        display: block;
      }
      .menu-loading {
        margin: 45px 0 0 92px;
      }
    </style>
  </head>
  <body>
    <ul id="menu" class="menu">
      <!-- <li class="menu-item" data-key="hot" data-done="done">
        <span>热门</span>
        <div class="menu-content">
          <p><img class="menu-loading" src="./loading.gif" alt="加载中" /></p>
        </div>
      </li> -->
    </ul>

    <script type="module">
      // https://www.imooc.com/api/mall-PC/index/menu/hot
      // https://www.imooc.com/api/mall-PC/index/menu

      import { getJSON } from './ajax/index.js';
      const menuURL = 'https://www.imooc.com/api/mall-PC/index/menu';
      const menuEl = document.getElementById('menu');

      getJSON(menuURL)
        .then(repsonse => {
          // console.log(repsonse);

          let html = '';

          for (const item of repsonse.data) {
            html += `
              <li class="menu-item" data-key="${item.key}">
                <span>${item.title}</span>
                <div class="menu-content">
                  <p><img class="menu-loading" src="./loading.gif" alt="加载中" /></p>
                </div>
              </li>
            `;
          }

          menuEl.innerHTML = html;

          // [{key: "hot", title: "热门出发地", subTitles: Array(5)}]

          // ...
        })
        .then(() => {
          const items = menuEl.querySelectorAll('.menu-item');

          for (const item of items) {
            item.addEventListener(
              'mouseenter',
              () => {
                // console.log(item.getAttribute('data-key'));

                // IE11 开始支持
                // console.log(item.dataset.key);

                if (item.dataset.done === 'done') return;

                getJSON(
                  `https://www.imooc.com/api/mall-PC/index/menu/${item.dataset.key}`
                )
                  .then(repsonse => {
                    // console.log(repsonse);

                    // [{title: "内地热门城市", cities: Array(27)}]

                    item.dataset.done = 'done';

                    let html = '';

                    for (const item of repsonse.data) {
                      html += `<p>${item.title}</p>`;
                    }

                    item.querySelector('.menu-content').innerHTML = html;
                  })
                  .catch(err => {
                    console.log(err);
                  });
              },
              false
            );
          }
        })
        .catch(err => {
          console.log(err);
        });
    </script>
  </body>
</html>

```


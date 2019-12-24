
`<demo example>` : <https://github.com/Reksmeys/pagination/blob/master/index.html>

Before start: All the function in paginator.js you should call with referance page

For ex you will call initPaginator func, then it will be like paginator.initPaginator(yourData);

or if you gonna call destroy ; paginator.destroyPaginator();

### HTML

Add below html before init paginator

```html
  <div class="col-md-12 paginator js-paginator">

  </div>
```

### Javascript

Call 'initpaginator' function to create your pages

__'previousPage'__ :  for naming previous page button (string) - Default is Previous Page

__'nextPage'__:  for naming next page button (string) - Default is Next Page

__'totalPage'__:  total pages number (int)

__'triggerFunc'__:  the function name that you want to fire after pages created

__'paginationClass'__:  customize your class for the parent element (string)


```javascript
 $(document).ready(function () {
    let totalPage = 1;
    $.ajax({
    url: `http://110.74.194.124:15011/v1/api/articles?page=1&limit=15`,
    method: 'GET',
    success: function(res){
      // first load 
      content = ''
        for (article of res.DATA){
          content += `
            <tr>
                <th scope="row">${article.ID}</th>
                <td>${article.TITLE}</td>
                <td>${article.DESCRIPTION}</td>
                <td><button class="btn btn-outline-danger waves">delete</button></td>
            </tr>
          `
        }
        $('tbody').html(content);
        //--------------------------------------
      totalPage = res.PAGINATION.TOTAL_PAGES
      paginator.initPaginator({
      'previousPage': 'Next',
      'nextPage': 'Prev',
      'totalPage': totalPage,
      'triggerFunc': renderPage,
      'paginationClass': 'paginatorCustomClass'
    });
    }
  })
    
  });
```
The function example which is triggered right after pages created

```javascript
  function renderPage() {
    
    selectdPg = $('.js-paginator').data('pageSelected');
   
    $.ajax({
      url: `http://110.74.194.124:15011/v1/api/articles?page=${selectdPg}&limit=15`,
      method: 'get',
      success: function(res){
        content = ''
        for (article of res.DATA){
          content += `
            <tr>
                <th scope="row">${article.ID}</th>
                <td>${article.TITLE}</td>
                <td>${article.DESCRIPTION}</td>
                <td><button class="btn btn-outline-danger waves">delete</button></td>
            </tr>
          `
        }
        $('tbody').html(content);
      },
      error: function(er){
        console.log(er);
      }
    })
  }
```

__Destroy__ paginator
```javascript
    paginator.destroyPaginator();
```

#### Total page changes
for changing the total number of the page dynamically
you can call the function 'paginatorPages'.
Before you call this function, you should know there are 3 parameters;

__pageTotal__: new total page number(int)

__changedClass__: new customized class if you want to change something with css or etc (string) - if you dont send this parameter your first customized class will not change.

__selectedPage__: if you want to specify your selected page with init this function (int). default page is 1.


So your function will be like : 
__paginator.paginatorPages(pageTotal, 'newCustClass', paginationClass);__

Special thanks to [@sseymakaya](https://github.com/sseymakaya) for testing.


### End

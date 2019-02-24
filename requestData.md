### ==ajax==
+ 什么是ajax
    + ajax是Asynchronous [eɪˈsɪŋkrənəs] JavaScript and XML的简写。 
+ 为什么要使用ajax
    + 这一技术可以向服务器请求额外的数据而无须卸载页面，会带来更好的用户体验。
+ 使用ajax请求后台数据的步骤：

1. 新建一个XMLHttpRequest对象
2. ==如果是get请求，先open()，再send()；如果是post请求，先open()，再setRequestHeader(),最后send()，==这些函数的参数也有些不一样，要注意。
    + open()接收三个参数：要发送的请求的类型（'get'、'post'等）、请求的URL和表示是否异步发送的布尔值。
    + 注意：调用open()并不会真正发送请求，而只是启动一个请求以备发送。
    + send()接收一个参数，即要作为请求主体发送的数据。如果不需要，必须传入null.
    + 在收到响应后，响应的数据会自动填充XHR对象的属性，如下属性：
        + responseText
        + responseXML:如果响应的内容是“text/xml”或“application/xml"，这个属性中将保存包含着响应数据的XML DOM文档。
        + status：相应的HTTP状态
        + statusText：状态说明
3. 若为同步请求，可以直接判断status，但多数情况下，我们还是要发送异步请求。==设置事件处理程序onreadystatechange(),如果readystate等于4且200<=status<=300或者status=304，就执行success()函数，否则就执行error()函数。这个success函数会传入xhr对象的responseText作为参数，responseText是一个json对象，判断其"ok"属性即可验证身份。==
    + 必须在调用open()之前指定onreadystatechange事件处理程序才能确保跨浏览器兼容性。
    + XHR对象的readyState属性表示请求/响应过程的当期活动阶段。这个属性可取的值如下：
        + 0：未初始化。尚未调用open()方法。
        + 1：启动。已经调用open()方法，但尚未调用send()方法。
        + 2：发送。已经调用send()方法，但尚未接收到响应。
        + 3：接收。已经接收到部分响应数据。
        + 4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了。
    + 在接收到响应之前还可以调用abort()方法来取消异步请求。

+ 用Promise封装ajax, 类似$.Ajax()
```
window.jQuery.ajax = ({method,path,body,headers}) => { //原本这里还要传入successFn和failFn
    //Promise接收的这个函数有两个参数,一个叫做resolve.一个叫reject
    return new Promise((resolve, reject) => {   //原本没有这行代码
        let xhr = new XMLHttpRequest();
        xhr.open(method, path);

        for (const key in headers) {
            let value = headers[key];
            xhr.setRequestHeader(key, value);
        }
        xhr.send(body);

        xhr.onreadystatechange = () => {
            if (xhr.readyState >= 200 && xhr.status <= 400) {
                resolve.call(undefined, xhr.responseText);  //原本是successFn.call(参数相同)
            } else if (xhr.status >= 400) {
                reject.call(undefined, xhr);  //原本是failFn.call(参数相同)
            }
        }
    })
}
```
+ 如何调用？
    + 在ajax()函数后接上.then(),成功就调用then()函数第一个参数里的函数,失败就调用then()函数第二个参数里的函数
```
myButton.addEventListener("click", (e) => {
    $.ajax({
        method:"post",
        path:"/xxx",
        body:"username=mtt&password=1",
        headers:{
            "content-type":'application/x-www-form-urlencoded',
            "mataotao":18
        }
    }).then(
        (responseText) => {console.log(responseText);}, //成功就调用这个函数
        (request) => {console.log(request);} //失败就调用这个函数
    )
})
```
+ 原来版本，不使用promise
```
window.jQuery.ajax = ({method,path,body,successFn,failFn,headers}) => {
    
    let xhr = new XMLHttpRequest();
    xhr.open(method,path);

    for (const key in headers) { //遍历header，设置响应头
        let value = headers[key];
        xhr.setRequestHeader(key, value);
    }
    xhr.send(body);

    xhr.onreadystatechange = () => {
        if (xhr.readyState === 4) {
            if (xhr.status >=200 && xhr.status <=400) {
                successFn.call(undefined, xhr.responseText);
            } else if (xhr.status >= 400) {
                failFn.call(undefined, xhr);
            }
        }
    }
}
```
+ 如何调用？
```
$.ajax({
        method:"post",
        url:"/xxx",
        data:"username=mtt&password=1",
        dataType:'json',
        success:()=>{}//成功后的回调函数
        error:()=>{}//失败后的回调函数
    }
    )
```
+ Promise本质上只是规定一种形式!
+ promise的第一个意义:不用记住成功和失败到底是success,还是successFN或者是fail或者是error,不用记住函数名字.只需要知道成功是第一个参数,失败是第二个参数，完全不需要写函数名
### GET和POST的区别：
+ 最直观的区别就是GET把参数包含在URL中，POST通过request body传递参数。
+ GET是最常见的请求类型，最常用于==向服务器查询某些信息==。对XHR而言，传入open()方法的查询字符串必须经过正确的编码才行，每个参数的名称和值必须使用encodeURIComponent()进行编码，而且所有名-值对儿都必须由（&）分隔。
+ POST通常用于==向服务器发送应该被保存的数据==，把数据作为请求的主体提交。POST请求的主题可以包含非常多的数据，而且格式不限。由于XHR最初的设计主要是为了处理XML。因此可以在此传入XML DOM文档，传入的文档经序列化之后将作为请求主体被提交到服务器。默认情况下，服务器对POST请求和提交Web表单的请求并不会一视同仁。因此，服务器端必须有程序来读取发送过来的原始数据，并从中解析出有用的部分。除了这个方法外，我们还可以使用XHR来模仿表单提交：首先将Content-Type头部信息设置为appliccation/x-www-form-urlecoded，也就是表单提交时的诶荣类型，其次是以适当的格式创建一个字符串。
+ 性能上的区别：与GET请求相比，POST请求消耗的资源会更多一些，以发送相同的数据计，GET请求的速度最多可达到POST请求的两倍。
+ GET在浏览器回退时是无害的，而POST会再次提交请求。
+ GET请求会被浏览器主动cache，而POST不会，除非手动设置。
+ GET请求只能进行url编码，而POST支持多种编码方式。
+ GET请求参数会被完整保留在浏览器历史记录里，而POST中的参数不会被保留。
+ GET请求在URL中传送的参数是有长度限制的，而POST么有。
+ 对参数的数据类型，GET只接受ASCII字符，而POST没有限制。
+ GET比POST更不安全，因为参数直接暴露在URL上，所以不能用来传递敏感信息。
+ GET 请求可被收藏为书签，POST 不能

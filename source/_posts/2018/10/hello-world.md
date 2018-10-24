---
title: 自定义FormRequest ValidationException 返回错误信息
tag: 
   - laravel
   - FormRequest
   - ValidationException
---
对于复杂的验证场景，你可能想要创建一个“表单请求”。
表单请求是包含验证逻辑的自定义请求类，要创建表单验证类，
可以使用 Artisan 命令 make:request

## 创建表单请求

### Create a FormRequest

``` bash
$ php artisan make:request
```
<!--more-->

### 配置request文件
``` bash
      public function authorize()
      {
          return true;
      }
      public function rules()
      {
          return [
              'title' => 'required',
              'category_id' => 'required',
              'content' => 'required',
          ];
      }
      public function messages()
      {
          return [
              'title.required' => '标题不能为空',
              'category_id.required' => '类别不能为空',
              'content.required' => '内容不能为空',
          ];
      }
```



### 获取错误
自定义返回异常在app\Exceptions\Handler.php里。

表单验证抛出的是ValidationException，修改render方法里：
``` bash
public function render($request, Exception $exception) {
    //其他异常...

    //自定义表单验证异常
    if ($exception instanceof ValidationException) {
        return $this->handleValidationExceptionToResponse($exception, $request);
    }
    //...
}
```
然后写你自己处理异常的逻辑：
``` bash
//自定义返回异常消息
protected function handleValidationExceptionToResponse(ValidationException $e, $request) {
     $errors = $e->validator->errors()->first();
    //自定义异常返回json
    return response()->json(['errcode' => 1111, 'errmsg' => $errors], 422);
}
```

### 前端处理
axios catch 到错误信息并用message打印出来:
```bash
 var _this = this.$message;
 
 this.$axios.post('/news/create', { params:params})
                    .then(function (response) {
                        _this.success(response.data.msg);
                    })
                    .catch(function (error) {
                        _this.error(error.response.data.errmsg);
                    });
```

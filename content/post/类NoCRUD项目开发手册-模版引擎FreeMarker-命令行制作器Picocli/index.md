+++
date = '2024-10-29T19:35:19+08:00'
draft = true
title = '类NoCRUD项目开发手册 模版引擎FreeMarker 命令行制作器Picocli'

+++

初次发布于[我的个人文档](https://colablack.github.io/)

### 参考资料

[FreeMarker官方文档（英文）]([Apache FreeMarker Manual](https://freemarker.apache.org/docs/index.html))

[FreeMarker 中文官方参考手册](http://freemarker.foofun.cn/toc.html)

[Picocli官方文档（英文）](https://picocli.info/#_example_application)

[picocli-中文博客](https://blog.csdn.net/it_freshman/article/details/125458116)

### 1.安装依赖

```kotlin
// https://mvnrepository.com/artifact/org.freemarker/freemarker
implementation("org.freemarker:freemarker:2.3.33")
// https://mvnrepository.com/artifact/info.picocli/picocli
implementation("info.picocli:picocli:4.7.6")
```

### 2.编写Freemarker demo

参考官方文档，编写一个简单Freemarker的demo，生成一个Java文件。

```kotlin
import freemarker.template.Configuration
import freemarker.template.TemplateExceptionHandler
import java.io.File
import java.io.OutputStreamWriter


fun main() {
    // 创建配置对象
    val config = Configuration(Configuration.VERSION_2_3_33)
    // 设置模板文件存放的目录
    config.setDirectoryForTemplateLoading(File("src/main/resources/templates"))
    // 设置默认的编码格式
    config.defaultEncoding = "UTF-8"
    // 设置异常处理器
    config.templateExceptionHandler = TemplateExceptionHandler.RETHROW_HANDLER
    /*
    利用HashMap创建数据模型
    {
    "user": "Big Joe",
    "latestProduct": {
        "url": "https://github.com/ColaBlack",
        "name": "No CRUD"
    }
     */
    val hashMap = HashMap<String, Any>()
    hashMap["user"] = "ColaBlack"
    val latest: MutableMap<String, Any> = HashMap()
    hashMap["latestProduct"] = latest
    latest["url"] = "https://github.com/ColaBlack"
    latest["name"] = "No CRUD"
    // 获取模板文件
    val template = config.getTemplate("demo.ftl")
    val out = OutputStreamWriter(File("src/main/java/edu/zafu/generated/demo.java").outputStream())
    // 输出渲染后的内容
    template.process(hashMap, out)
    // 关闭输出流
    out.close()
}
```

对应的模版文件`demo.ftl`如下：

```text
package edu.zafu.generated;

public class demo {
    public static void main(String[] args) {
        System.out.println("Hello ${user}");
        System.out.println("The latest product is ${latestProduct.name}");
        System.out.println("You can find it at ${latestProduct.url}");
    }
}

```

将模版文件放入`src/main/resources/templates`目录下，运行`main`
函数，将生成的Java文件输出到`src/main/java/edu/zafu/generated/demo.java`文件中。

### 3.编写命令行picocli demo

参考官方文档，编写一个命令行demo，实现一个简单的`ls`命令。

```kotlin
import picocli.CommandLine
import picocli.CommandLine.*
import java.io.File
import kotlin.system.exitProcess

/**
 * ls命令demo
 *
 * @author ColaBlack
 */
// 定义一个dir命令，name为ls，description为该命令的描述信息，mixinStandardHelpOptions为true，表示该命令需要自动生成help选项
@Command(name = "dir", description = ["列出当前目录的目录结构"], mixinStandardHelpOptions = true)
// 定义一个dir类，继承Runnable接口，实现run方法，用于执行ls命令，其中Callable的泛型int表示call方法的返回值类型
class Dir : Runnable {

    // 该参数在命令行中指定，索引为0，description为描述信息
    @Parameters(index = "0", description = ["要列出的目录路径"])
    var path: String? = null // 定义一个path变量，用于接收命令行参数

    // 该选项缩写为-a，全称为--all，description为描述信息
    @Option(names = ["-a", "--all"], description = ["显示所有文件，包括隐藏文件"])
    var showHidden = false // 定义一个showHidden变量，用于接收-a选项，是否显示隐藏文件

    // 用户执行命令时，会调用run方法
    override fun run() {
        if (path == null) {
            println("文件路径不能为空")
            return
        }
        val file = File(path!!)
        if (!file.exists()) {     // 判断目录是否存在
            println("文件路径不存在: $path")
            return
        }
        if (!file.isDirectory) {  // 判断是否为目录
            println("$path 不是一个目录")
            return
        }
        val files = file.listFiles()
        if (files == null) {       // 判断目录是否为空
            println("目录为空")
            return
        }
        for (item in files) {
            if (showHidden || !item.name.startsWith(".")) {
                println(item.name)
            }
        }
    }
}

fun main(args: Array<String>) {
    val exitCode = CommandLine(Dir()).execute(*args)    // 执行命令
    exitProcess(exitCode)    // 退出程序
}
```

运行`main`函数并设置参数就可以使用CLI

### 4.编写需要制作成模版的代码

编写需要制作成模版的代码，例如本项目的controller代码：

```java
package $cn.cola.controller;

import cn.cola.model.dto.user.UserUpdateRequest;
import cn.cola.model.dto.user.UserQueryRequest;
import cn.cola.model.dto.user.UserAddRequest;
import cn.cola.common.constant.UserConstant;
import cn.cola.common.exception.ThrowUtils;
import cn.cola.common.DeleteRequest;
import cn.cola.common.BaseResponse;
import cn.cola.common.ResultUtils;
import cn.cola.common.AuthCheck;
import cn.cola.common.ErrorCode;
import cn.cola.model.po.User;
import cn.cola.model.vo.UserVO;
import cn.cola.service.UserService;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import org.springframework.web.bind.annotation.*;
import org.springframework.beans.BeanUtils;
import lombok.extern.slf4j.Slf4j;

import javax.annotation.Resource;
import java.util.List;

/**
 * 用户接口
 *
 * @author ColaBlack
 */
@RestController
@RequestMapping("/user")
@Slf4j
public class UserController {

    @Resource
    private UserService userService;

    // region 增删改查

    /**
     * 插入用户（仅管理员）
     *
     * @param userAddRequest 用户添加请求体
     * @return 新增的用户ID
     */
    @PostMapping("/insert")
    @AuthCheck(mustRole = UserConstant.ADMIN_ROLE)
    public BaseResponse<Long> insertUser(@RequestBody UserAddRequest userAddRequest) {
        ThrowUtils.throwIf(userAddRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(userAddRequest.getUserAccount() == null || userAddRequest.getUserAccount().isEmpty(), ErrorCode.PARAMS_ERROR, "账号不能为空");
        User user = new User();
        BeanUtils.copyProperties(userAddRequest, user);
        int res = userService.getBaseMapper().insert(user);
        ThrowUtils.throwIf(res <= 0, ErrorCode.OPERATION_ERROR, "数据库异常，增加用户失败");
        return ResultUtils.success(user.getId());
    }

    /**
     * 删除用户(仅管理员)
     *
     * @param deleteRequest 删除请求体
     * @return 删除的记录数
     */
    @PostMapping("/delete")
    @AuthCheck(mustRole = UserConstant.ADMIN_ROLE)
    public BaseResponse<Integer> deleteUser(@RequestBody DeleteRequest deleteRequest) {
        ThrowUtils.throwIf(deleteRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(deleteRequest.getId() <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        int res = userService.getBaseMapper().deleteById(deleteRequest.getId());
        ThrowUtils.throwIf(res <= 0, ErrorCode.OPERATION_ERROR, "数据库异常，删除用户失败");
        return ResultUtils.success(res);
    }

    /**
     * 修改用户(仅管理员)
     *
     * @param userUpdateRequest 用户更新请求体
     * @return 更新的记录数
     */
    @PostMapping("/update")
    @AuthCheck(mustRole = UserConstant.ADMIN_ROLE)
    public BaseResponse<Integer> updateUser(@RequestBody UserUpdateRequest userUpdateRequest) {
        ThrowUtils.throwIf(userUpdateRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(userUpdateRequest.getId() <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        User user = new User();
        BeanUtils.copyProperties(userUpdateRequest, user);
        int res = userService.getBaseMapper().updateById(user);
        ThrowUtils.throwIf(res <= 0, ErrorCode.OPERATION_ERROR);
        return ResultUtils.success(res);
    }

    /**
     * 分页查询用户列表（仅管理员）
     *
     * @param userQueryRequest 条件查询请求体
     * @return 用户列表
     */
    @PostMapping("/select/page")
    @AuthCheck(mustRole = UserConstant.ADMIN_ROLE)
    public BaseResponse<Page<User>> selectUserByPage(@RequestBody UserQueryRequest userQueryRequest) {
        long current = userQueryRequest.getCurrent();
        long size = userQueryRequest.getPageSize();
        ThrowUtils.throwIf(current <= 0 || size <= 0 || size > 100, ErrorCode.PARAMS_ERROR, "参数错误");
        Page<User> page = new Page<>(current, size);
        QueryWrapper<User> queryWrapper = userService.getQueryWrapper(userQueryRequest);
        Page<User> res = userService.getBaseMapper().selectPage(page, queryWrapper);
        return ResultUtils.success(res);
    }

    /**
     * 根据ID查询用户信息（仅管理员）
     *
     * @param userQueryRequest 条件查询请求体
     * @return 用户信息
     */
    @PostMapping("/select/id")
    @AuthCheck(mustRole = UserConstant.ADMIN_ROLE)
    public BaseResponse<User> selectUserById(@RequestBody UserQueryRequest userQueryRequest) {
        ThrowUtils.throwIf(userQueryRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(userQueryRequest.getId() <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        User user = userService.getBaseMapper().selectById(userQueryRequest.getId());
        return ResultUtils.success(user);
    }

    /**
     * 根据ID查询用户信息（全体用户）
     */
    @GetMapping("/get/id")
    public BaseResponse<UserVO> getUserById(@RequestParam("id") long id) {
        ThrowUtils.throwIf(id <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        User user = userService.getBaseMapper().selectById(id);
        UserVO userVO = new UserVO();
        BeanUtils.copyProperties(user, userVO);
        return ResultUtils.success(userVO);
    }

    /**
     * 分页查询用户信息（全体用户）
     */
    @PostMapping("/get/page")
    public BaseResponse<Page<UserVO>> getUserByPage(@RequestBody UserQueryRequest userQueryRequest) {
        long current = userQueryRequest.getCurrent();
        long size = userQueryRequest.getPageSize();
        ThrowUtils.throwIf(current <= 0 || size <= 0 || size > 20, ErrorCode.PARAMS_ERROR, "参数错误");
        Page<User> page = new Page<>(current, size);
        QueryWrapper<User> queryWrapper = userService.getQueryWrapper(userQueryRequest);
        Page<User> res = userService.getBaseMapper().selectPage(page, queryWrapper);
        Page<UserVO> userVoPage = new Page<>();
        BeanUtils.copyProperties(res, userVoPage);
        List<UserVO> records = userVoPage.getRecords();
        records.clear();
        for (User user : res.getRecords()) {
            UserVO userVO = new UserVO();
            BeanUtils.copyProperties(user, userVO);
            records.add(userVO);
        }
        return ResultUtils.success(userVoPage);
    }
    // endregion

    // region 其他接口

    // todo: 此处补充其他接口

    // endregion
}
```

这个文档最先是在我的另一个个人文档网站的，但是在文档迁移的时候这个文件出了问题，里面的内容是我手工复原的，这段模版代码可能有问题，但是这不影响这篇文章的内容。

### 5.编写FreeMarker模版

将原代码中可以被参数化的部分用`${}`包裹起来就得到了模版，例如：

```java
package ${packageName}.controller;

import ${packageName}.model.dto.${key}.${upperKey}UpdateRequest;
import ${packageName}.model.dto.${key}.${upperKey}QueryRequest;
import ${packageName}.model.dto.${key}.${upperKey}AddRequest;
import ${packageName}.common.constant.${upperKey}Constant;
import ${packageName}.common.exception.ThrowUtils;
import ${packageName}.common.DeleteRequest;
import ${packageName}.common.BaseResponse;
import ${packageName}.common.ResultUtils;
import ${packageName}.common.AuthCheck;
import ${packageName}.common.ErrorCode;
import ${packageName}.model.po.${upperKey};
import ${packageName}.model.vo.${upperKey}VO;
import ${packageName}.service.${upperKey}Service;
import com.baomidou.mybatisplus.extension.plugins.pagination.Page;
import com.baomidou.mybatisplus.core.conditions.query.QueryWrapper;
import org.springframework.web.bind.annotation.*;
import org.springframework.beans.BeanUtils;
import lombok.extern.slf4j.Slf4j;

import javax.annotation.Resource;
import java.util.List;

/**
 * ${name}接口
 *
 * @author ${author}
 */
@RestController
@RequestMapping("/${key}")
@Slf4j
public class ${upperKey}Controller {

    @Resource
    private ${upperKey}Service ${key}Service;

    // region 增删改查

    /**
     * 插入${name}（仅管理员）
     *
     * @param ${key}AddRequest ${name}添加请求体
     * @return 新增的${name}ID
     */
    @PostMapping("/insert")
    @AuthCheck(mustRole = UserConstant.ADMIN_ROLE)
    public BaseResponse<Long> insert${upperKey}(@RequestBody ${upperKey}AddRequest ${key}AddRequest) {
        ThrowUtils.throwIf(${key}AddRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(${key}AddRequest.get${upperKey}Account() == null || ${key}AddRequest.get${upperKey}Account().isEmpty(), ErrorCode.PARAMS_ERROR, "账号不能为空");
        ${upperKey} ${key} = new ${upperKey}();
        BeanUtils.copyProperties(${key}AddRequest, ${key});
        int res = ${key}Service.getBaseMapper().insert(${key});
        ThrowUtils.throwIf(res <= 0, ErrorCode.OPERATION_ERROR, "数据库异常，增加${name}失败");
        return ResultUtils.success(${key}.getId());
    }

    /**
     * 删除${name}(仅管理员)
     *
     * @param deleteRequest 删除请求体
     * @return 删除的记录数
     */
    @PostMapping("/delete")
    @AuthCheck(mustRole = $UserConstant.ADMIN_ROLE)
    public BaseResponse<Integer> delete${upperKey}(@RequestBody DeleteRequest deleteRequest) {
        ThrowUtils.throwIf(deleteRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(deleteRequest.getId() <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        int res = ${key}Service.getBaseMapper().deleteById(deleteRequest.getId());
        ThrowUtils.throwIf(res <= 0, ErrorCode.OPERATION_ERROR, "数据库异常，删除${name}失败");
        return ResultUtils.success(res);
    }

    /**
     * 修改${name}(仅管理员)
     *
     * @param ${key}UpdateRequest ${name}更新请求体
     * @return 更新的记录数
     */
    @PostMapping("/update")
    @AuthCheck(mustRole = $UserConstant.ADMIN_ROLE)
    public BaseResponse<Integer> update${upperKey}(@RequestBody ${upperKey}UpdateRequest ${key}UpdateRequest) {
        ThrowUtils.throwIf(${key}UpdateRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(${key}UpdateRequest.getId() <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        ${upperKey} ${key} = new ${upperKey}();
        BeanUtils.copyProperties(${key}UpdateRequest, ${key});
        int res = ${key}Service.getBaseMapper().updateById(${key});
        ThrowUtils.throwIf(res <= 0, ErrorCode.OPERATION_ERROR);
        return ResultUtils.success(res);
    }

    /**
     * 分页查询${name}列表（仅管理员）
     *
     * @param ${key}QueryRequest 条件查询请求体
     * @return ${name}列表
     */
    @PostMapping("/select/page")
    @AuthCheck(mustRole = $UserConstant.ADMIN_ROLE)
    public BaseResponse<Page<${upperKey}>> select${upperKey}ByPage(@RequestBody ${upperKey}QueryRequest ${key}QueryRequest) {
        long current = ${key}QueryRequest.getCurrent();
        long size = ${key}QueryRequest.getPageSize();
        ThrowUtils.throwIf(current <= 0 || size <= 0 || size > 100, ErrorCode.PARAMS_ERROR, "参数错误");
        Page<${upperKey}> page = new Page<>(current, size);
        QueryWrapper<${upperKey}> queryWrapper = ${key}Service.getQueryWrapper(${key}QueryRequest);
        Page<${upperKey}> res = ${key}Service.getBaseMapper().selectPage(page, queryWrapper);
        return ResultUtils.success(res);
    }

    /**
     * 根据ID查询${name}信息（仅管理员）
     *
     * @param ${key}QueryRequest 条件查询请求体
     * @return ${name}信息
     */
    @PostMapping("/select/id")
    @AuthCheck(mustRole = $UserConstant.ADMIN_ROLE)
    public BaseResponse<${upperKey}> select${upperKey}ById(@RequestBody ${upperKey}QueryRequest ${key}QueryRequest) {
        ThrowUtils.throwIf(${key}QueryRequest == null, ErrorCode.PARAMS_ERROR, "参数不能为空");
        ThrowUtils.throwIf(${key}QueryRequest.getId() <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        ${upperKey} ${key} = ${key}Service.getBaseMapper().selectById(${key}QueryRequest.getId());
        return ResultUtils.success(${key});
    }

    /**
     * 根据ID查询${name}信息（全体用户）
     */
    @GetMapping("/get/id")
    public BaseResponse<${upperKey}VO> get${upperKey}ById(@RequestParam("id") long id) {
        ThrowUtils.throwIf(id <= 0, ErrorCode.PARAMS_ERROR, "参数错误");
        ${upperKey} ${key} = ${key}Service.getBaseMapper().selectById(id);
        ${upperKey}VO ${key}VO = new ${upperKey}VO();
        BeanUtils.copyProperties(${key}, ${key}VO);
        return ResultUtils.success(${key}VO);
    }

    /**
     * 分页查询${name}信息（全体用户）
     */
    @PostMapping("/get/page")
    public BaseResponse<Page<${upperKey}VO>> get${upperKey}ByPage(@RequestBody ${upperKey}QueryRequest ${key}QueryRequest) {
        long current = ${key}QueryRequest.getCurrent();
        long size = ${key}QueryRequest.getPageSize();
        ThrowUtils.throwIf(current <= 0 || size <= 0 || size > 20, ErrorCode.PARAMS_ERROR, "参数错误");
        Page<${upperKey}> page = new Page<>(current, size);
        QueryWrapper<${upperKey}> queryWrapper = ${key}Service.getQueryWrapper(${key}QueryRequest);
        Page<${upperKey}> res = ${key}Service.getBaseMapper().selectPage(page, queryWrapper);
        Page<${upperKey}VO> ${key}VoPage = new Page<>();
        BeanUtils.copyProperties(res, ${key}VoPage);
        List<${upperKey}VO> records = ${key}VoPage.getRecords();
        records.clear();
        for (${upperKey} ${key} : res.getRecords()) {
            ${upperKey}VO ${key}VO = new ${upperKey}VO();
            BeanUtils.copyProperties(${key}, ${key}VO);
            records.add(${key}VO);
        }
        return ResultUtils.success(${key}VoPage);
    }
    // endregion

    // region 其他接口

    // todo: 此处补充其他接口

    // endregion
}
```

### 6.制作CLI工具

完整代码参考：[No CRUD项目](https://github.com/ColaBlack/NoCRUD)

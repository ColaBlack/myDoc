+++

date = '2024-10-28T19:39:49+08:00' 

draft = true 

title = '常用配置类' 

+++

初次发布于[我的个人文档](https://colablack.github.io/)

本文收集了一些常用的配置类，便于后续查询使用。
## 后端常用配置

### 1.全局跨域配置

```java
package edu.zafu.teaai.config;

import org.springframework.context.annotation.Configuration;
import org.springframework.web.servlet.config.annotation.CorsRegistry;
import org.springframework.web.servlet.config.annotation.WebMvcConfigurer;

/**
 * 全局跨域配置
 *
 * @author ColaBlack
 */
@Configuration
public class CorsConfig implements WebMvcConfigurer {

    @Override
    public void addCorsMappings(CorsRegistry registry) {
        // 覆盖所有请求
        registry.addMapping("/**")
                // 允许发送 Cookie
                .allowCredentials(true)
                // 放行哪些域名（必须修改为你实际的域名，否则 * 会和 allowCredentials 冲突）
                .allowedOriginPatterns("*")
                .allowedMethods("GET", "POST", "PUT", "DELETE", "OPTIONS")
                .allowedHeaders("*")
                .exposedHeaders("*");
    }
}

```

注意：放行的域名，allowedOriginPatterns()必须修改为前端的域名，否则会冲突。

### 2.避免Long转json时的精度丢失

```java
package edu.zafu.teaai.config;

import com.fasterxml.jackson.databind.ObjectMapper;
import com.fasterxml.jackson.databind.module.SimpleModule;
import com.fasterxml.jackson.databind.ser.std.ToStringSerializer;
import org.springframework.boot.jackson.JsonComponent;
import org.springframework.context.annotation.Bean;
import org.springframework.http.converter.json.Jackson2ObjectMapperBuilder;

/**
 * 配置如何处理Json文本
 *
 * @author ColaBlack
 */
@JsonComponent
public class JsonConfig {

    /**
     * 添加 Long 转 json 精度丢失的配置
     */
    @Bean
    public ObjectMapper jacksonObjectMapper(Jackson2ObjectMapperBuilder builder) {
        ObjectMapper objectMapper = builder.createXmlMapper(false).build();
        SimpleModule module = new SimpleModule();
        module.addSerializer(Long.class, ToStringSerializer.instance);
        module.addSerializer(Long.TYPE, ToStringSerializer.instance);
        objectMapper.registerModule(module);
        return objectMapper;
    }
}
```

在Java中Long类型是64位有符号整数，‌范围从-9,223,372,036,854,775,808到9,223,372,036,854,775,807，然而在JavaScript中整数最长只有15位，所以需要在Long类型对象进行序列化时将Long类型转化为字符串避免精度丢失。

此时，如果传递一个Long类型的ID则前端收到的是一个字符串，就不会有精度问题。

### 3.开启MyBatis分页功能

```java
package edu.zafu.teaai.config;

import com.baomidou.mybatisplus.annotation.DbType;
import com.baomidou.mybatisplus.extension.plugins.MybatisPlusInterceptor;
import com.baomidou.mybatisplus.extension.plugins.inner.PaginationInnerInterceptor;
import org.mybatis.spring.annotation.MapperScan;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * MyBatis Plus 配置
 *
 * @author ColaBlack
 */
@Configuration
@MapperScan("edu.zafu.teaai.mapper")
public class MyBatisPlusConfig {

    /**
     * 拦截器配置
     */
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
        // 分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
        return interceptor;
    }
}
```

Mybatis的分页功能需要以上配置才能开启。

### 4.开启MyBatis下划线转驼峰、逻辑删除功能

```yaml
mybatis-plus:
  configuration:
    map-underscore-to-camel-case: true # 驼峰命名
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  global-config:
    db-config:
      logic-delete-field: is_delete # 全局逻辑删除的实体字段名
      logic-delete-value: 1 # 逻辑已删除值（默认为 1）
      logic-not-delete-value: 0 # 逻辑未删除值（默认为 0）
```

需要在application.yml中增加以上配置。

## 前端常用配置

### 1.前端为开发服务器开启代理解决跨域问题

```typescript
  server: {
    port: 1222,
    proxy: {
      '/api': {
        target: 'http://localhost:1221',
        changeOrigin: true
      }
    }
  },

```

在vite.config.ts的defineConfig函数传入的实参对象中增加这样的配置即可。

其意义是，将前端开发服务器开在1222端口，将所有/api开头的请求代理到后端`http://localhost:1221`。

以后只需要向前端服务器`http://localhost:1222`发请求即可。

增加完之后你的配置文件可能长这样：

```typescript
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  server: {
    port: 3000,
    proxy: {
      '/api': {
        target: 'http://localhost:1221',
        changeOrigin: true
      }
    }
  },
  plugins: [
    vue()
  ],
  resolve: {
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  }
})

```

对于这个配置，官方文档是这样说明的：

```typescript
export default defineConfig({
  server: {
    proxy: {
      // 字符串简写写法：http://localhost:5173/foo -> http://localhost:4567/foo
      '/foo': 'http://localhost:4567',
      // 带选项写法：http://localhost:5173/api/bar -> http://jsonplaceholder.typicode.com/bar
      '/api': {
        target: 'http://jsonplaceholder.typicode.com',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/api/, ''),
      },
      // 正则表达式写法：http://localhost:5173/fallback/ -> http://jsonplaceholder.typicode.com/
      '^/fallback/.*': {
        target: 'http://jsonplaceholder.typicode.com',
        changeOrigin: true,
        rewrite: (path) => path.replace(/^\/fallback/, ''),
      },
      // 使用 proxy 实例
      '/api': {
        target: 'http://jsonplaceholder.typicode.com',
        changeOrigin: true,
        configure: (proxy, options) => {
          // proxy 是 'http-proxy' 的实例
        }
      },
      // 代理 websockets 或 socket.io 写法：ws://localhost:5173/socket.io -> ws://localhost:5174/socket.io
      // 在使用 `rewriteWsOrigin` 时要特别谨慎，因为这可能会让代理服务器暴露在 CSRF 攻击之下
      '/socket.io': {
        target: 'ws://localhost:5174',
        ws: true,
        rewriteWsOrigin: true,
      },
    },
  },
})
```

官方文档链接：[开发服务器选项 | Vite 官方中文文档](https://vitejs.cn/vite5-cn/config/server-options.html#server-proxy)

### 2.配置axios请求

在src/config下创建request.ts用于配置axios。

配置内容如下：

```typescript
import axios from 'axios'

export const BASE_URL = 'http://localhost:1222'

const request = axios.create({
  baseURL: BASE_URL,
  timeout: 60000,
  withCredentials: true
})

// 请求拦截器
request.interceptors.request.use(
  function (config) {
    // Do something before request is sent
    return config
  },
  function (error) {
    // Do something with request error
    return Promise.reject(error)
  }
)

// 响应拦截器
request.interceptors.response.use(
  function (response) {
    // Any status code that lie within the range of 2xx cause this function to trigger
    // Do something with response data

    const { data } = response
    // 未登录
    if (data.code === 40100) {
      // 不是获取用户信息接口，并且不是登录页面，则跳转到登录页面并保存当前页面的路径
      if (
        !response.request.responseURL.includes('user/get/login') &&
        !window.location.pathname.includes('/user/login')
      ) {
        window.location.href = `/user/login?redirect=${window.location.href}`
      }
    }
    return response
  },
  function (error) {
    // Any status codes that falls outside the range of 2xx cause this function to trigger
    // Do something with response error
    return Promise.reject(error)
  }
)

export default request

```

BASE_URL务必换成自己后端的地址（如果前端开了代理则为前端的地址），真实上线时请修改为真实的请求地址！

withCredentials: true请务必开启，否则cookie会有问题。

### 3. Umijs/openapi

在有些时候，我们可能已经有了一份openapi格式的接口文档。（如：后端已经开发完成则可以使用swagger或knife4j自动生成接口文档）

这时可以使用umijs/openapi自动生成请求代码。

需要在src/config下新建openapi.config.js配置如下：

```javascript
import { generateService } from '@umijs/openapi'

generateService({
  requestLibPath: "import request from '@/config/request'",
  schemaPath: 'http://localhost:1221/api/v2/api-docs',
  serversPath: './src'
})

```

然后在package.json中增加如下运行脚本

```json
    "openapi": "node src/config/openapi.config.js"
```

### 4.Pinia存储用户登录态样例代码

```typescript
import { defineStore } from 'pinia'
import type { Ref } from 'vue'
import { ref } from 'vue'
import { getLoginUserUsingGet } from '@/api/userController'
import type { AxiosResponse } from 'axios'
import roleEnums from '@/access/roleEnums'

export const useUserStore = defineStore('USER_LOGIN_STATE', () => {
  const loginUser: Ref<API.LoginUserVO> = ref({ userName: '未登录' })

  async function fetchLoginUser() {
    const res: AxiosResponse<API.BaseResponseLoginUserVO_> = await getLoginUserUsingGet()
    if (res.data.code === 200 && res.data.data) {
      loginUser.value = res.data.data
    } else {
      loginUser.value = { userName: '未登录', userRole: roleEnums.PUBLIC }
    }
  }

  function setLoginUser(newUser: API.LoginUserVO) {
    loginUser.value = newUser
  }

  return {
    loginUser,
    fetchLoginUser,
    setLoginUser
  }
})

```

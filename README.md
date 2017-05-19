
# start-server: a command-line http server mock tools

`start-server` 是一个简单的http服务器,既可以是一个静态文件服务器，也可以是一个mock的api服务端，还支持类似CI框架的dcm的query来mock文件,在`http-server`基础上改写而来。


# 安装:

Installation via `npm`:

     npm install start-server -g


## Usage:
- 启动服务器：

    ```start-server [path] [options]```

    `[path]` defaults to `./mocks` if the folder exists, and `./` otherwise.

    *Now you can visit http://localhost:8899 to view your server*

- 根据现有的url创建一个静态文件(如果能请求到则使用请求的，如果不能则只创建一个文件):

    ```start-server mock url [options]```

        options:{
            proxy:"http://xxx.com:8080",//为mock设置代理
            path:"./mocks",//设置生成的base目录
        }

    examples:

    `start-server mock http://ditu.amap.com/service/regeo?longitude=121.04925573429551&latitude=31.315590522490712`

## 配置文件格式

除了静态文件路由之外，还支持用配置文件来做一些路由映射,优先取配置文件里的配置,配置文件的地址默认为`/mocks/config.js`,主要用来配置一些简单的mock路径对应的响应,examples:

```
module.exports = {
    //路由配置
    'routes': {
        //方法,all代表全部方法
        'all': {
            //路径:响应的内容
            '/api/path1/path2': {
                'data':{
                    list:[
                        '1','2'
                    ]
                }
            },
            //如果是CI框架类型的话这里需要把dcm直接按照路径写,访问的时候使用/node?d=dd&c=cc&m=mm或者/node/dd/cc/mm都可以访问
            '/node/dd/cc/mm':{
                'data':{
                    list:[
                        '3','4'
                    ]
                }
            }
        }
    }

}
```

## Available Options:

`--config` 指定配置文件地址(可选),默认`[path]/config.js` 

`-p` Port to use (defaults to 8080)

`-a` Address to use (defaults to 0.0.0.0)

`-d` Show directory listings (defaults to 'True')

`-i` Display autoIndex (defaults to 'True')

`-g` or `--gzip` When enabled (defaults to 'False') it will serve `./public/some-file.js.gz` in place of `./public/some-file.js` when a gzipped version of the file exists and the request accepts gzip encoding.

`-e` or `--ext` Default file extension if none supplied (defaults to 'html')

`-s` or `--silent` Suppress log messages from output

`--cors` Enable CORS via the `Access-Control-Allow-Origin` header

`-o` Open browser window after starting the server

`-c` Set cache time (in seconds) for cache-control max-age header, e.g. -c10 for 10 seconds (defaults to '3600'). To disable caching, use -c-1.

`-U` or `--utc` Use UTC time format in log messages.

`-P` or `--proxy` Proxies all requests which can't be resolved locally to the given url. e.g.: -P http://someurl.com

`-S` or `--ssl` Enable https.

`-C` or `--cert` Path to ssl cert file (default: cert.pem).

`-K` or `--key` Path to ssl key file (default: key.pem).

`-r` or `--robots` Provide a /robots.txt (whose content defaults to 'User-agent: *\nDisallow: /')

`-h` or `--help` Print this list and exit.

`--pathQueryKeys` 如果需要类CI的路由风格，可以在这里指定，默认是`d,c,m`,设置为false则关闭query风格的路由模式  


# Development

Checkout this repository locally, then:

```sh
$ npm i
$ node bin/http-server
```

*Now you can visit http://localhost:8080 to view your server*

You should see the turtle image in the screenshot above hosted at that URL. See
the `./public` folder for demo content.

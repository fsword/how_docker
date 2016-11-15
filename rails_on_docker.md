# Rails on docker

Rails是ruby社区的full stack web开发框架，它一出现就吸引了很多其它语言社区的程序员，用趋之若鹜形容也是不夸张的，这说明它有很多优点。

但是，Rails工程师有很多都是mac用户，在这个环境中想使用docker，需要通过boot2docker，不过实践中我发现，要顺利高效的使用docker，还需要考虑一些问题。

### Gemfile.lock怎么生成？

1. shared folder（vboxsf）性能太差

[docker-osx-dev](https://github.com/brikis98/docker-osx-dev)的作者认为，vboxsf的读取文件速度大约是本地的10~20倍，而我们在开发时常常需要mount本地的源码目录或者mysql数据目录，这使得rails框架的载入变成一个灾难。

2. `bundle install`生成的`Gemfile.lock`与mac环境紧密联系

但是，也有很多程序员在接触 Rails
时遇到了非常大的阻力，其中之一就是——搞不好环境，Docker很适合处理这类问题。


windows自不必提，不过，即使是社区非常认可的mac平台，在上面做开发也不是没有环境问题的。

> Rails社区一开始就不太待见windows平台，我在几年前用Rails进行开发时曾经多次尝试过帮助同事在windows上搭建Rails开发环境，发现异常困难，后来要么放弃，要么换用jruby。

举个例子，如果你使用mysql数据库，那么你的`Gemfile`文件里面应该有这么一句：

```
gem 'mysql2'
```
然后执行`bundle install`，生成的`Gemfile.lock`文件里面会出现这句话

```

```
显然，如果我们把这个`Gemfile.lock`通过源码更新到线上的linux服务器，再执行`bundle install`，是不会成功运行的
